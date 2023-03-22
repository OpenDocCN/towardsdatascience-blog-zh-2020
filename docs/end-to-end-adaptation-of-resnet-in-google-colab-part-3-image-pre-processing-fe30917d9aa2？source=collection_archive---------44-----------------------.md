# Google Colab 中 ResNet 的端到端适配第 3 部分:图像预处理

> 原文：<https://towardsdatascience.com/end-to-end-adaptation-of-resnet-in-google-colab-part-3-image-pre-processing-fe30917d9aa2?source=collection_archive---------44----------------------->

## 端到端 Resnet

## 为训练准备数据集

![](img/906af147ece9a90e40c318dfc413586b.png)

来源:Unsplash.com

快速回顾一下我们到目前为止所做的工作:

[第一部分](/end-to-end-adaptation-of-resnet-in-google-colab-part-1-5e56fce934a6) —整个项目概述。不需要 python，只需要一个 Google 和 Kaggle 账户。希望您已经说服自己，您个人能够完整地运行代码，并对其进行测试。

[第二部分](/end-to-end-adaptation-of-resnet-in-google-colab-part-2-hardware-dataset-setup-f23dd4e0004d) —库和数据库都是从 Kaggle 导入的。它被解压缩到一个文件夹中。我们还做了一个简短的概述，介绍了如何确定您正在使用哪种硬件。在处理像我们这样的图像数据库时，GPU 信息很重要。

**链接到 Colab 笔记本:** [**猫&狗— Resnet-18**](https://colab.research.google.com/drive/1W5tU1X3w1uIY-R1HpQredT1vpiLGcDIQ?usp=sharing)

在我们能够将图像“加载”到架构中进行训练之前，我们必须首先对它们进行预处理。

# 创建文件夹结构

上面的代码创建了以下结构(val 和 train 位于主数据集文件夹下，而 dogs 和 cats 文件夹位于它们各自的子文件夹中):

```
dataset_dogs_vs_cats
     train
          dogs
          cats
     val
          dogs
          cats
```

对于那些想要命名的人，我们正在这里用标记的数据执行*监督的*机器学习。我们给数据“贴标签”的方式是把它放在一个文件夹里。代码将被告知使用包含图像的文件夹的名称作为标签。

# 旁白

将来我几乎肯定会在这个问题上犯错，但你对神经网络了解得越多，“建模”和“模型”这两个词就开始变得模糊，你总是会说“我们正在用这个特定的模型来建模我们的问题。”

当提到模型的技术定义时，我鼓励你使用**架构**这个词。我承认我的代码会有类似‘train _ model’的语句，但在这种情况下，model = architecture，即 ResNet-18 架构。“我们正在用这种特殊的架构来建模我们的问题”听起来更容易接受。

# 整理我们的数据

第一步是将数据分为训练和测试。我们将使用训练数据来训练我们的架构(调整参数)。ResNet-18 有 1100 万个可训练参数。测试集将用于衡量架构的执行情况，以“测试”的形式向其显示新数据。

“src_directory”包含所有图像。“listdir”获取源目录中所有文件的列表，如果文件以“cat”开头，我们将它放在 cats 文件夹中，与 dog 相同。这是在“file.startswith('cat 或 dog ')”中捕获的。

您可以看到，默认情况下，目的地是“train”文件夹，除非一个随机数(默认为 0 到 1 之间)小于 0.25，这种情况应该发生在 25%左右，在这种情况下，文件会被传送到“val”文件夹。

# 图像转换

这里需要解释一下。让我去掉一行——“调整大小”。ResNet 架构要求图像为 224x224(为什么不在本文讨论范围之内)，因此 train 和 val(验证)图像都必须调整大小。

让我们倒着做。行“ToTensor()”设置我们的图像用于计算。它本质上是一个多维数组(高度、宽度、颜色空间)。每个[train，val]的“归一化”功能根据三个 RGB 通道归一化数据。第一组数字是平均值，第二组是标准差。使用这些特定的归一化值是因为 torchvision 架构(如 ResNet)已经在 ImageNet 上进行了训练，并基于数百万张图像进行了计算。

剩下三种变换—旋转、RandomHorizontalFlip 和 RandomResizedCrop。与“为什么”相比，这些转换的“是什么”更容易理解。本质上，您希望向您的网络呈现尽可能多的**不同的**训练图像。增加这些图片种类的一种方法是使用庞大的数据集——在这种情况下，有近 19，000 张训练图像。

我们能够通过对图像进行小的修改来人为地增加图像的多样性。

旋转和 RandomHorizontalFlip 是不言自明的。

RandomResizedCrop 从图像(本例中为 224x224)中选取一小块，随机选取的范围在 0.96 到 1.0 之间，随机选取的纵横比在 0.95 到 1.05 之间。

# 数据加载器

数据加载器允许将数据集批量交付到用于培训的架构中。它们对于确保简化的数据流至关重要。相应地，您看到设置的第一个变量是 batch_size = 16。如果您在训练时遇到困难(尤其是 GPU 内存问题)，请以 2 的倍数减少批量。

Data_dir 设置训练和验证图像的主目录。数据集。“ImageFolder”允许像我们之前所做的那样设置标签——文件夹的名称是标签本身(而不是嵌入在文件名中或存储在其他地方的标签)。您可以看到，我们在上面定义的“数据转换”应用于“图像数据集”变量中。

“torch.utils.data.DataLoader”允许将“image_datasets”合并到“dataloaders”变量中。

我们后来在最后两行中将“dataloaders”变量拆分为“train_dataloader”和“val_dataloader”。

最后，确保使用 GPU:device = torch . device(" cuda:0 ")。在接下来的步骤中，我们将确保架构加载到 GPU 上。

请在下面留下任何问题，我会尽力回答。

# 参考

[1] [深度学习与 Pytorch](https://pytorch.org/tutorials/beginner/deep_learning_60min_blitz.html) ，2020 年 10 月访问

[【2】神经网络与 Pytorch。【2020 年 10 月接入](https://pytorch.org/tutorials/beginner/blitz/neural_networks_tutorial.html)

[3] [为计算机视觉转移学习。【2020 年 10 月接入](https://pytorch.org/tutorials/beginner/transfer_learning_tutorial.html)

[4] [Kaggle API。【2020 年 10 月访问](https://github.com/Kaggle/kaggle-api)