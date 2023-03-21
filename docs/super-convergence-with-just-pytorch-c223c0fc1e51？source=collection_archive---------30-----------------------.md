# 仅使用 PyTorch 的超收敛

> 原文：<https://towardsdatascience.com/super-convergence-with-just-pytorch-c223c0fc1e51?source=collection_archive---------30----------------------->

## 使用内置 PyTorch 函数和类减少训练时间同时提高效果的指南

# 为什么？

当创建 Snaked，我的蛇分类模型时，我需要找到一种方法来改善结果。超级收敛只是一种更快训练模型同时获得更好结果的方法！然而，我发现**没有关于如何使用内置 PyTorch 调度程序的指南。**

![](img/7d32063f8be811fe42a3517a2c5fd8bc.png)

封面图片来源于[此处](https://pixnio.com/objects/mechanism-metal-gears-steel-iron)

# 学习理论

在你阅读这篇文章之前，你可能想知道*什么是*超级收敛，以及*它是如何工作的*。总的要点是在开始时尽可能提高学习速度，然后以循环的速度逐渐降低学习速度。这是因为较大的学习率训练得更快，而导致损失发散。我在这里的重点是 PyTorch，所以我自己不会做任何进一步的解释。

以下是可供深入研究的资源列表:

*   [超收敛:使用大学习率非常快速地训练神经网络](https://arxiv.org/abs/1708.07120)
*   [自亚当以来，深度学习优化器是怎么回事？](https://medium.com/vitalify-asia/whats-up-with-deep-learning-optimizers-since-adam-5c1d862b9db0)
*   [超收敛:使用大学习率非常快速地训练神经网络](/https-medium-com-super-convergence-very-fast-training-of-neural-networks-using-large-learning-rates-decb689b9eb0)
*   [AdamW 和超收敛是目前训练神经网络最快的方法](https://www.fast.ai/2018/07/02/adam-weight-decay/)
*   [1 周期政策](https://sgugger.github.io/the-1cycle-policy.html)
*   [如何找到好的学习率](https://sgugger.github.io/how-do-you-find-a-good-learning-rate.html)

# 进口

```
import torch
from torchvision import datasets, models, transforms
from torch.utils.data import DataLoader

from torch import nn, optim
from torch_lr_finder import LRFinder
```

# 设置超参数

## 设置变换

```
transforms = transforms.Compose([
transforms.RandomResizedCrop(size=256, scale=(0.8, 1)),
    transforms.RandomRotation(90),
    transforms.ColorJitter(),
    transforms.RandomHorizontalFlip(),
    transforms.RandomVerticalFlip(),
    transforms.CenterCrop(size=224), 
    transforms.ToTensor(),
    transforms.Normalize((0.485, 0.456, 0.406), (0.229, 0.224, 0.225)), 
])
```

## 加载数据、模型和基本超参数

```
train_loader = DataLoader(datasets.CIFAR10(root="train_data", train=True, download=True, transform=transforms))
test_loader = DataLoader(datasets.CIFAR10(root="test_data", train=False, download=True, transform=transforms))

model = models.mobilenet_v2(pretrained=True)

criterion = nn.CrossEntropyLoss()
optimizer = optim.AdamW(model.parameters())

device = torch.device("cuda" if torch.cuda.is_available() else "cpu")
model = model.to(device)

Note that doing this requires a seperate library from [here](https://github.com/davidtvs/pytorch-lr-finder).

```python
lr_finder = LRFinder(model, optimizer, criterion, device)
lr_finder.range_test(train_loader, end_lr=10, num_iter=1000)
lr_finder.plot()
plt.savefig("LRvsLoss.png")
plt.close()HBox(children=(FloatProgress(value=0.0, max=1000.0), HTML(value='')))

Stopping early, the loss has diverged
Learning rate search finished. See the graph with {finder_name}.plot()
```

## 创建计划程序

使用单周期学习率调度程序(用于超收敛)。

请注意，调度程序使用图表中的最大学习率。要选择向下寻找最大梯度(斜率)。

必须在中输入训练的周期数和每个周期的步数。通常的做法是使用批量大小作为每个时期的步骤。

```
scheduler = optim.lr_scheduler.OneCycleLR(optimizer, 2e-3, epochs=50, steps_per_epoch=len(train_loader))
```

# 火车模型

训练模型 50 个纪元。每个时期后打印统计数据(损失和准确性)。

不同的调度程序应该在不同的代码中调用。将调度器放在错误的位置会导致错误，所以使用单周期策略时，要确保在每个批处理之后直接调用 step 方法。

```
best_acc = 0
epoch_no_change = 0

for epoch in range(0, 50):
    print(f"Epoch {epoch}/49".format())

    for phase in ["train", "validation"]:
        running_loss = 0.0
        running_corrects = 0

        if phase == "train":
            model.train()
        else: model.eval()

        for (inputs, labels) in train_loader:

            inputs, labels = inputs.to(device), labels.to(device)

            optimizer.zero_grad()

            with torch.set_grad_enabled(phase == "train"):

                outputs = model(inputs)
                _, preds = torch.max(outputs, 1)
                loss = criterion(outputs, labels)

                if phase == "train":

                    loss.backward()
                    optimizer.step()

                    scheduler.step()

            running_loss += loss.item() * inputs.size(0)
            running_corrects += torch.sum(preds == labels.data)

        epoch_loss = running_loss / len(self.data_loaders[phase].sampler)
        epoch_acc = running_corrects.double() / len(self.data_loaders[phase].sampler)
        print("\nPhase: {}, Loss: {:.4f}, Acc: {:.4f}".format(phase, epoch_loss, epoch_acc))

        if phase == "validation" and epoch_acc > best_acc:
            epoch_no_change += 1

            if epoch_no_change > 5:
                break
```

# 感谢阅读！

我希望这足够简单，可以相对快速地理解。当我第一次实现超级收敛时，我花了很长时间才弄明白如何使用调度程序(我找不到任何利用它的代码)。如果你喜欢这篇博文，考虑看看其他方法来改进你的模型。如果你想看看超级收敛在实际项目中是如何使用的，只需看看[我的蛇分类项目](https://github.com/KamWithK/Snaked)就行了。