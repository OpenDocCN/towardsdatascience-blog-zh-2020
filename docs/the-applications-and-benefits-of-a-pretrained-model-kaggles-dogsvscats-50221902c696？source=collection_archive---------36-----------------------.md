# 预训练模型的应用和优势 Kaggle 的 DogsVSCats

> 原文：<https://towardsdatascience.com/the-applications-and-benefits-of-a-pretrained-model-kaggles-dogsvscats-50221902c696?source=collection_archive---------36----------------------->

![](img/f38edb1eeefde644cf4faab960b558ca.png)

凯文·Ku 上[的 Unsplash](https://unsplash.com/?utm_source=medium&utm_medium=referral)

对于图像识别任务，使用预先训练的模型是很好的。首先，它们更容易使用，因为它们为您提供了“免费”的架构此外，他们通常有更好的结果，并且通常需要较少的培训。

为了看到这一理论的实际应用，我将使用 Kaggle 的 CatVSDogs 数据集来尝试讨论使用不同方法的结果。

这些步骤如下:

```
1) Imports2) Download and Unzip Files3) Organize the Files4) Set-up and Train Classic CNN Model 5) Test the CNN Model6) Set-up and Train Pre-Trained Model7) Test the Pre-Trained Model
```

**1。进口**

在任何机器学习项目中，导入都是必要的。对于这个项目，有各种各样的进口是必需的。

```
import warningswarnings.filterwarnings("ignore", category=UserWarning, module="torch.nn.functional") from google.colab import drivedrive.mount('/content/gdrive')  # connects to data stored on google driveimport osos.chdir('/content/gdrive/My Drive/')import shutilimport reimport torchimport torchvisionfrom torchvision import transformsimport torch.nn as nnimport torch.nn.functional as Fimport torch.optim as optimfrom google.colab import filesimport zipfile
```

虽然这些导入中的大部分在以后才会用到，但是最好现在就全部导入。这些进口往往分为两类:PyTorch 的进口和 google collab 所需的进口。

Google Colab 为每个人提供了一个免费的 GPU，所以使用起来非常棒，尤其是对于初学者。使用下面的代码检查您是否正在使用 GPU。如果它不打印 cuda，打开你的 GPU！

```
device = torch.device("cuda" if torch.cuda.is_available() else "cpu") print(device)
```

PyTorch，类似地，倾向于对初学者倾斜，因为它有一个更容易的界面。然而，它仍然可以完成任何机器学习任务！

**2。下载并解压文件**

这些数据将直接从 Kaggle(一个开源的机器学习网站)中获取。文件可以在[这里](https://www.kaggle.com/c/dogs-vs-cats/data)找到。按下载全部。然后，将 zip 文件拖到您的驱动器中，并将其放在任何所需的位置。

不要直接从 Google Drie 或通过你的电脑解压。这将需要大量的时间。因此，在 Google Drive 中编写一个解压文件并组织它们的方法是对你最有利的。

在解压缩之前，创建一个 DogsVS Cats 文件夹。

```
os.mkdir('/content/gdrive/My Drive/DogsVSCats')
```

在这里，我将存储解压缩后的数据。

要解压缩，运行以下命令。

```
with zipfile.ZipFile('/content/gdrive/My Drive/dogs-vs-cats.zip') as zf:zf.extractall('/content/gdrive/My Drive/DogsVSCats')
```

这将提取 dogs-vs-cats 压缩文件，并将其放在新文件夹中。之后，我们需要解压缩 test1.zip 和 train.zip 文件。您可以删除示例 submission.csv 文件；在本教程中，我们不需要它。

要解压缩 train.zip 文件，请运行以下命令:

```
with zipfile.ZipFile('/content/gdrive/My Drive/DogsVSCats/train.zip') as zf:zf.extractall('/content/gdrive/My Drive/DogsVSCats/')
```

要解压缩 test1.zip 文件，请运行以下命令:

```
with zipfile.ZipFile('/content/gdrive/My Drive/DogsVSCats/test1.zip') as zf:zf.extractall('/content/gdrive/My Drive/DogsVSCats/')
```

这可能需要一些时间，但是之后所有的文件都应该在它们相应的文件夹中。

**3。整理文件**

一旦所有文件都被解压缩，根据它们的真实分类来组织它们是至关重要的。对于这个数据集，我们将文件组织到“dog”和“cat”文件夹中。

制作所需的文件夹。

```
os.mkdir('/content/gdrive/My Drive/DogsVSCats/train/cat')os.mkdir('/content/gdrive/My Drive/DogsVSCats/train/dog')
```

设置分类。

```
train_dr= '/content/gdrive/My Drive/DogsVSCats/train/'train_dog_dir = '/content/gdrive/My Drive/DogsVSCats/train/dog'train_cat_dir = '/content/gdrive/My Drive/DogsVSCats/train/cat' files = os.listdir('/content/gdrive/My Drive/DogsVSCats/train')
```

然后，组织。

```
train = os.listdir("/content/gdrive/My Drive/DogsVSCats/train")for f in files: catSearchObj = re.search("cat", f) dogSearchObj = re.search("dog", f) if catSearchObj: shutil.move(f'{train_dr}/{f}', train_cat_dir) print("moved!-cat") elif dogSearchObj: shutil.move(f'{train_dr}/{f}', train_dog_dir) print("moved!-dog")
```

实际上*没有*来添加打印语句，但是看到这种运动很酷！

接下来，我们需要将一些训练数据移动到验证数据集中。

```
os.mkdir('/content/gdrive/My Drive/DogsVSCats/val/cat')os.mkdir('/content/gdrive/My Drive/DogsVSCats/val/dog') val_dog_dir = '/content/gdrive/My Drive/DogsVSCats/val/dog'val_cat_dir = '/content/gdrive/My Drive/DogsVSCats/val/cat'
```

从 train dog 文件夹中重新定位 1，000 个文件，并将其发送到验证文件夹。

```
files = os.listdir(train_dog_dir)for f in files: valDogSearch = re.search("5\d\d\d", f) if valDogSearch: shutil.move(f'{train_dog_dir}/{f}', val_dog_dir)!ls {val_dog_dir} | head -n 5
```

现在，对 cat trained 文件夹执行相同的操作。

```
files = os.listdir(train_cat_dir)for f in files: valCatSearch = re.search("5\d\d\d", f) if valCatSearch: shutil.move(f'{train_cat_dir}/{f}', val_cat_dir)!ls {val_cat_dir} | head -n 5
```

这一部分的最后一步是转换数据，以便更容易训练和扫描。

```
transforms = torchvision.transforms.Compose([torchvision.transforms.Resize((224,224)),torchvision.transforms.ToTensor()])train_image_folder = torchvision.datasets.ImageFolder('/content/gdrive/My Drive/DogsVSCats/train/', transform=transforms)train_loader = torch.utils.data.DataLoader(train_image_folder, batch_size=64, shuffle=True, num_workers=4)val_image_folder = torchvision.datasets.ImageFolder('/content/gdrive/My Drive/DogsVSCats/val/', transform=transforms)val_loader = torch.utils.data.DataLoader(val_image_folder, batch_size=64, shuffle=True, num_workers=4)
```

**4。设置和训练经典 CNN 模型**

现在，我们到了实际训练部分！在这个变体中，我们将使用一个经典的 CNN 模型。

该模型如下:

```
class DogDetector(nn.Module): def __init__(self): super().__init__() self.cnn_layers = nn.Sequential( nn.Conv2d(3, 6, kernel_size=3, stride=1, padding=1), nn.ReLU(inplace=True), nn.MaxPool2d(kernel_size=2, stride=2), nn.Conv2d(6, 12, kernel_size=3, stride=1, padding=1), nn.ReLU(inplace=True), nn.MaxPool2d(kernel_size=2, stride=2), 

      )

      self.linear_layers = nn.Sequential( nn.Linear(256*7*7*3, 196), nn.Linear(196, 1), nn.Sigmoid(),
      ) def forward(self, x): x = self.cnn_layers(x) x = x.view(x.size(0), -1) x = self.linear_layers(x) return xdog_detector = DogDetector()dog_detector.cuda()
```

现在，将 dog_detector 设置为类，并将其放在 GPU 上。

训练模型是一个简单的过程。因为我们使用 Kaggle 文件只是为了测试，而不是为了直接竞争，所以我们不需要测试文件。相反，我们可以基于验证集进行训练，而不需要提交给 Kaggle。

```
optimizer = optim.Adam(dog_detector.parameters(), lr = 0.0001)loss_func = nn.BCELoss().cuda()EPOCHS = 5for epoch in range(EPOCHS): print(f"epoch: {epoch}") for i, data in enumerate(train_loader): if i % 50 == 0: print(f"  batch: {i}") X, y = data y = y.type(torch.FloatTensor).view(len(y), -1).cuda() dog_detector.zero_grad() output = dog_detector(X.view(-1, 3, 224, 224).cuda()) loss_val = loss_func(output, y) loss_val.backward() optimizer.step() print(f"loss: {loss_val}")
```

损失值倾向于在大约 0.4-0.6 左右徘徊。

**5。测试 CNN 模型**

因为我们是在没有 Kaggle 的情况下进行测试，所以我们需要自己检查准确性，这给了我们更直接的结果。这可以通过以下方式实现:

```
correct = 0total = 0with torch.no_grad(): for data in val_loader: X, y = data   output = dog_detector(X.view(-1, 3, 224, 224).cuda()) correct_sum = output.round().transpose(0, 1).cpu() == y correct += correct_sum.sum().item() total += len(y)print(f"Accuracy: {round(correct/total, 3)}")
```

测试时，准确率往往徘徊在 73.6%左右。这并不坏，但远远称不上伟大。

**6。设置并训练预训练模型**

预训练模型的设置要简单得多。

首先，下载预先训练好的模型。

```
model_resnet18 =  torchvision.models.resnet18(pretrained=True) new_lin = nn.Sequential( nn.Linear(512, 1), nn.Sigmoid() )model_resnet18.fc = new_lin
```

冻结训练不需要的层。

```
for name, param in model_resnet18.named_parameters(): if("bn" not in name): param.requires_grad = False
```

然后，将模型发送到 GPU。

```
model_resnet18.cuda()
```

就是这样！现在是再次训练的时候了。

```
optimizer = optim.Adam(model_resnet18.parameters(), lr = 0.0001)loss_func = nn.BCELoss().cuda()EPOCHS = 5for epoch in range(EPOCHS): print(f"epoch: {epoch}") for i, data in enumerate(train_loader): if i % 50 == 0: print(f"  batch: {i}") X, y = data y = y.type(torch.FloatTensor).view(len(y), -1).cuda() model_resnet18.zero_grad() output = model_resnet18(X.view(-1, 3, 224, 224).cuda()) loss_val = loss_func(output, y) loss_val.backward() optimizer.step()print(f"loss: {loss_val}")
```

损耗范围在 0.2–0.4 之间，明显优于 CNN 模型。

**7。测试预训练模型**

```
correct = 0total = 0with torch.no_grad(): for data in val_loader: X, y = data output = model_resnet18(X.view(-1, 3, 224, 224).cuda()) correct_sum = output.round().transpose(0, 1).cpu() == y correct += correct_sum.sum().item() total += len(y)print(f"Accuracy: {round(correct/total, 3)}")
```

测试该模型给出了 97.3%的近似准确度，这明显优于 CNN 模型。

**总而言之，这次测试的结果是显而易见的；出于几个原因，对许多图像识别任务使用预训练模型是有益的。**第一个原因是，使用预训练模型需要较少的训练，并且在构建模型架构时需要较少的努力。相反，模型的定义是“免费的”另一个积极的方面是准确性。使用预先训练的模型比使用定制的卷积神经网络(CNN)要精确得多。因此，当执行图像识别任务时，从预先训练的模型开始是有意义的，因为这几乎总是最佳的行动过程。