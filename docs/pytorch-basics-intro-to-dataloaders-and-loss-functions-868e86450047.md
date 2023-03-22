# py torch[基础知识] —数据加载器和损失函数简介

> 原文：<https://towardsdatascience.com/pytorch-basics-intro-to-dataloaders-and-loss-functions-868e86450047?source=collection_archive---------14----------------------->

![](img/840b4e62b667c78ce2f768c8b11e4488.png)

如何训练你的神经网络[图片[0]]

## [如何训练你的神经网络](https://towardsdatascience.com/tagged/akshaj-wields-pytorch)

## 这篇博文将带您了解 PyTorch 中的数据加载器和不同类型的损失函数。

在这篇博文中，我们将看到自定义数据集和数据加载器的简短实现，以及一些常见的损失函数。

# 数据集和数据加载器

![](img/6470b727327f514fe1985cdebbe7afd5.png)

数据加载器 meme [Image [1]]

使用 3 个主要组件创建一个定制的数据集类。

*   `__init__`
*   `__len__`
*   `__getitem__`

```
class CustomDataset(Dataset):
    def __init__(self):
        pass def __getitem__(self, index):
        pass def __len__(self):
        pass
```

`__init__`:用于执行读取数据、预处理等初始化操作。
`__len__`:返回输入数据的大小。
`__getitem__`:批量返回数据(输入输出)。

然后在这个**数据集**类上使用一个**数据加载器**来批量读取数据。

```
train_loader = DataLoader(custom_dataset_object, batch_size=32, shuffle=True)
```

让我们实现一个基本的 PyTorch 数据集和数据加载器。假设你有输入和输出数据-

`X` : 1，2，3，4，5，6，7，8，9，10

`y` : 0，0，0，1，0，1，1，0，0，1

让我们定义数据集类。我们将返回一个元组(输入，输出)。

```
class CustomDataset(Dataset):

    def __init__(self, X_data, y_data):
        self.X_data = X_data
        self.y_data = y_data

    def __getitem__(self, index):
        return self.X_data[index], self.y_data[index]

    def __len__ (self):
        return len(self.X_data)
```

初始化数据集对象。输入必须是张量类型。

```
data = CustomDataset(torch.FloatTensor(X), torch.FloatTensor(y))
```

让我们使用方法`__len__()`和`__getitem__()`。`__getitem__()`将索引作为输入。

```
data.__len__()################### OUTPUT ##################### 10
```

从输出数据中打印出第 4 个元素(第 3 个索引)。

```
data.__getitem__(3)################### OUTPUT ##################### (tensor(4.), tensor(1.))
```

让我们现在初始化我们的数据加载器。在这里，我们指定批量大小和洗牌。

```
data_loader = DataLoader(dataset=data, batch_size=2, shuffle=True)data_loader_iter = iter(data_loader)
print(next(data_loader_iter))################### OUTPUT ##################### [tensor([3., 6.]), tensor([0., 1.])]
```

让我们使用带有 for 循环的数据加载器。

```
for i,j in data_loader:
    print(i,j)################### OUTPUT #####################tensor([ 1., 10.]) tensor([0., 1.])
tensor([4., 6.]) tensor([1., 1.])
tensor([7., 5.]) tensor([1., 0.])
tensor([9., 3.]) tensor([0., 0.])
tensor([2., 8.]) tensor([0., 0.])
```

# 损失函数

![](img/0bae239fd02c79c86438826270db97ce.png)

损失函数 meme [Image [2]]

以下是不同深度学习任务常用的损失函数。

回归:

*   平均绝对误差— `torch.nn.L1Loss()`
*   均方差— `torch.nn.MSELoss()`

分类:

*   二元交叉熵损失— `torch.nn.BCELoss()`
*   具有罗吉斯损失的二元交叉熵— `torch.nn.BCEWithLogitsLoss()`
*   负对数可能性— `torch.nn.NLLLoss()`
*   交叉斜视— `torch.nn.CrossEntropyLoss()`

从官方 [PyTorch 文档](https://pytorch.org/docs/stable/nn.html#loss-functions)了解更多损失函数。

# 导入库

```
import torch
import torch.nn as nn
```

# 回归

为了计算损失，让我们从定义实际和预测输出张量开始。

```
y_pred = torch.tensor([[1.2, 2.3, 3.4], [4.5, 5.6, 6.7]], requires_grad=True)print("Y Pred: \n", y_pred)
print("\nY Pred shape: ", y_pred.shape, "\n")print("=" * 50)y_train = torch.tensor([[1.2, 2.3, 3.4], [7.8, 8.9, 9.1]])
print("\nY Train: \n", y_train)
print("\nY Train shape: ", y_train.shape) ###################### OUTPUT ######################Y Pred: 
 tensor([[1.2000, 2.3000, 3.4000],
        [4.5000, 5.6000, 6.7000]], requires_grad=True)Y Pred shape:  torch.Size([2, 3]) ==================================================Y Train: 
 tensor([[1.2000, 2.3000, 3.4000],
        [7.8000, 8.9000, 9.1000]])Y Train shape:  torch.Size([2, 3])
```

## 平均绝对误差— `torch.nn.L1Loss()`

输入和输出必须与**尺寸相同**，并且具有数据类型**浮动**。

`y_pred = (batch_size, *)`和`y_train = (batch_size, *)`。

```
mae_loss = nn.L1Loss()print("Y Pred: \n", y_pred)print("Y Train: \n", y_train)output = mae_loss(y_pred, y_train)
print("MAE Loss\n", output)output.backward() ###################### OUTPUT ######################Y Pred: 
 tensor([[1.2000, 2.3000, 3.4000],
        [4.5000, 5.6000, 6.7000]], requires_grad=True)
Y Train: 
 tensor([[1.2000, 2.3000, 3.4000],
        [7.8000, 8.9000, 9.1000]])
MAE Loss
 tensor(1.5000, grad_fn=<L1LossBackward>)
```

## 均方差— `torch.nn.MSELoss()`

输入和输出必须与**尺寸相同**，并且具有数据类型**浮动**。

`y_pred = (batch_size, *)`和`y_train = (batch_size, *)`。

```
mse_loss = nn.MSELoss()print("Y Pred: \n", y_pred)print("Y Train: \n", y_train)output = mse_loss(y_pred, y_train)
print("MSE Loss\n", output)output.backward() ###################### OUTPUT ######################Y Pred: 
 tensor([[1.2000, 2.3000, 3.4000],
        [4.5000, 5.6000, 6.7000]], requires_grad=True)
Y Train: 
 tensor([[1.2000, 2.3000, 3.4000],
        [7.8000, 8.9000, 9.1000]])
MSE Loss
 tensor(4.5900, grad_fn=<MseLossBackward>)
```

# 二元分类

`y_train`有两个类——0 和 1。当网络的最终输出是介于 0 和 1 之间的单个值(最终密集层的大小为 1)时，我们使用 BCE 损失函数。

如果网络的输出是长度为 2 的张量(最终密集层的大小为 2 ),其中两个值都位于 0 和 1 之间，则二进制分类可以重新构造为使用 **NLLLoss** 或**交叉熵**损失。

让我们定义实际和预测的输出张量，以便计算损失。

```
y_pred = torch.tensor([[1.2, 2.3, 3.4], [7.8, 8.9, 9.1]], requires_grad = True)
print("Y Pred: \n", y_pred)
print("\nY Pred shape: ", y_pred.shape, "\n")print("=" * 50)y_train = torch.tensor([[1, 0, 1], [0, 0, 1]])
print("\nY Train: \n", y_train)
print("\nY Train shape: ", y_train.shape) ###################### OUTPUT ######################Y Pred: 
 tensor([[1.2000, 2.3000, 3.4000],
        [7.8000, 8.9000, 9.1000]], requires_grad=True)Y Pred shape:  torch.Size([2, 3]) ==================================================Y Train: 
 tensor([[1, 0, 1],
        [0, 0, 1]])Y Train shape:  torch.Size([2, 3])
```

## 二元交叉熵损失— `torch.nn.BCELoss()`

输入和输出必须与**尺寸相同**并具有浮动的数据类型。

`y_pred = (batch_size, *)`，浮点型(值应通过一个 Sigmoid 函数传递，其值介于 0 和 1 之间)

`y_train = (batch_size, *)`，浮动

```
bce_loss = nn.BCELoss()y_pred_sigmoid = torch.sigmoid(y_pred)print("Y Pred: \n", y_pred)print("\nY Pred Sigmoid: \n", y_pred_sigmoid)print("\nY Train: \n", y_train.float())output = bce_loss(y_pred_sigmoid, y_train.float())
print("\nBCE Loss\n", output)output.backward() ###################### OUTPUT ######################Y Pred: 
 tensor([[1.2000, 2.3000, 3.4000],
        [7.8000, 8.9000, 9.1000]], requires_grad=True)Y Pred Sigmoid: 
 tensor([[0.7685, 0.9089, 0.9677],
        [0.9996, 0.9999, 0.9999]], grad_fn=<SigmoidBackward>)Y Train: 
 tensor([[1., 0., 1.],
        [0., 0., 1.]])BCE Loss
 tensor(3.2321, grad_fn=<BinaryCrossEntropyBackward>)
```

## 具有罗吉斯损失的二元交叉熵— `torch.nn.BCEWithLogitsLoss()`

输入和输出必须是相同尺寸的**和浮动的**。该类将**s 形**和**b 形**组合成一个类。这个版本在数值上比单独使用 Sigmoid 和 BCELoss 更稳定。

`y_pred = (batch_size, *)`，浮动

`y_train = (batch_size, *)`，浮动

```
bce_logits_loss = nn.BCEWithLogitsLoss()print("Y Pred: \n", y_pred)print("\nY Train: \n", y_train.float())output = bce_logits_loss(y_pred, y_train.float())
print("\nBCE Loss\n", output)output.backward() ###################### OUTPUT ######################Y Pred: 
 tensor([[1.2000, 2.3000, 3.4000],
        [7.8000, 8.9000, 9.1000]], requires_grad=True)Y Train: 
 tensor([[1., 0., 1.],
        [0., 0., 1.]])BCE Loss
 tensor(3.2321, grad_fn=<BinaryCrossEntropyWithLogitsBackward>)
```

# 多类分类

让我们定义实际和预测的输出张量，以便计算损失。

`y_train`有 4 个等级——0、1、2 和 3。

```
y_pred = torch.tensor([[1.2, 2.3, 3.4], [4.5, 5.6, 6.7], [7.8, 8.9, 9.1]], requires_grad = True)
print("Y Pred: \n", y_pred)
print("\nY Pred shape: ", y_pred.shape, "\n")print("=" * 50)y_train = torch.tensor([0, 1, 2])
print("\nY Train: \n", y_train)
print("\nY Train shape: ", y_train.shape) ###################### OUTPUT ######################Y Pred: 
 tensor([[1.2000, 2.3000, 3.4000],
        [4.5000, 5.6000, 6.7000],
        [7.8000, 8.9000, 9.1000]], requires_grad=True)Y Pred shape:  torch.Size([3, 3]) ==================================================Y Train: 
 tensor([0, 1, 2])Y Train shape:  torch.Size([3])
```

## 负对数可能性— `torch.nn.NLLLoss()`

`y_pred = (batch_size, num_classes)`，Float(值应传递使用 log_softmax 函数获得的对数概率。

`y_train = (batch_size)`，长整型(取值范围= 0，num_classes-1)。类别必须从 0、1、2 开始，...

```
nll_loss = nn.NLLLoss()y_pred_logsoftmax = torch.log_softmax(y_pred, dim = 1)print("Y Pred: \n", y_pred)print("\nY Pred LogSoftmax: \n", y_pred_logsoftmax)print("\nY Train: \n", y_train)output = nll_loss(y_pred_logsoftmax, y_train)
print("\nNLL Loss\n", output)output.backward() ###################### OUTPUT ######################Y Pred: 
 tensor([[1.2000, 2.3000, 3.4000],
        [4.5000, 5.6000, 6.7000],
        [7.8000, 8.9000, 9.1000]], requires_grad=True)Y Pred LogSoftmax: 
 tensor([[-2.5672, -1.4672, -0.3672],
        [-2.5672, -1.4672, -0.3672],
        [-2.0378, -0.9378, -0.7378]], grad_fn=<LogSoftmaxBackward>)Y Train: 
 tensor([0, 1, 2])NLL Loss
 tensor(1.5907, grad_fn=<NllLossBackward>)
```

## 交叉斜视— `torch.nn.CrossEntropyLoss()`

这个类将 **LogSoftmax** 和 **NLLLoss** 组合成一个类。

`y_pred = (batch_size, num_classes)`、Float
`y_train = (batch_size)`、Long(取值范围= 0，num_classes-1)。类别必须从 0、1、2 开始，...

```
ce_loss = nn.CrossEntropyLoss()print("Y Pred: \n", y_pred)print("\nY Train: \n", y_train)output = ce_loss(y_pred, y_train)
print("\nNLL Loss\n", output)output.backward() ###################### OUTPUT ######################Y Pred: 
 tensor([[1.2000, 2.3000, 3.4000],
        [4.5000, 5.6000, 6.7000],
        [7.8000, 8.9000, 9.1000]], requires_grad=True)Y Train: 
 tensor([0, 1, 2])NLL Loss
 tensor(1.5907, grad_fn=<NllLossBackward>)
```

感谢您的阅读。欢迎提出建议和建设性的批评。:)你可以在 [LinkedIn](https://www.linkedin.com/in/akshajverma7/) 和 [Twitter](https://twitter.com/theairbend3r) 找到我。如果你喜欢这个，看看我的其他[博客](https://medium.com/@theairbend3r)。