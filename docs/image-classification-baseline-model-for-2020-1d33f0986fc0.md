# 2020 年影像分类基线模型

> 原文：<https://towardsdatascience.com/image-classification-baseline-model-for-2020-1d33f0986fc0?source=collection_archive---------22----------------------->

## 如何使用 fastai 建立强基线模型进行图像分类？

# TL；DR —要使用的代码片段

有一些基本的想法，这里只是一段代码来构建您的基线模型？用这个:

```
from fastai.vision import ***# Defining the data and the model** path = untar_data(URLs.CIFAR)
tfms = get_transforms(do_flip=False)
data = ImageDataBunch.from_folder(path, valid='test', ds_tfms=tfms, size=128)
learn = cnn_learner(data, models.resnet50, metrics=accuracy)**# Estimate learning rate**
learn.lr_find()
learn.recorder.plot()**# Training** learn.fit_one_cycle(3, max_lr=1e-3)**# Finetuning** learn.unfreeze()
learn.fit_one_cycle(3, max_lr=slice(1e-6, 1e-4))**# Test Time Augmentation** preds,targs = learn.TTA()
accuracy(preds, targs).item()
```

# 介绍

自 20 世纪 50 年代以来，计算机视觉就一直存在，但直到最近十年，该领域才完全改变了自己(事实上，特殊的时刻发生在 2012 年，当时 [AlexNet](https://en.wikipedia.org/wiki/AlexNet) 赢得了 [ImageNet](https://en.wikipedia.org/wiki/ImageNet#ImageNet_Challenge) 挑战赛)。

在过去的几年中，出现了许多强大的框架。我们将使用 fastai，因为在编写本文时，它提供了最简单的 API 和最强的默认设置。这是 PyTorch 的高级包装。我们的目标是到 2020 年建立一个通用的图像分类基准。

![](img/c1794719a68b9f3ee936718ab047cfa0.png)

图片来自 [Pixabay](https://pixabay.com/?utm_source=link-attribution&utm_medium=referral&utm_campaign=image&utm_content=2198994) 的[Gerhard ge linger](https://pixabay.com/users/Gellinger-201217/?utm_source=link-attribution&utm_medium=referral&utm_campaign=image&utm_content=2198994)

# 图像分类任务

让我们使用流行的 CIFAR-10 数据集，它包含 10 个不同类别的 60，000 幅 32x32 彩色图像。它分为 50，000 个训练样本和 10，000 个测试样本。fastai 库中已经提供了 CIFAR 数据集作为样本数据集。

```
from fastai.vision import *
path = untar_data(URLs.CIFAR)
```

让我们从直接来自[文档](https://docs.fast.ai/vision.html)的一个最小的例子开始，并随着我们的进展修改它

```
tfms = get_transforms(do_flip=False)
data = ImageDataBunch.from_folder(path, valid='test', ds_tfms=tfms)
learn = cnn_learner(data, models.resnet50, metrics=accuracy)
learn.fit(3)
```

让我们来分解一下这里发生的 3 件最重要的事情:

1.  **数据扩充**:在训练用于图像分类的大型神经网络时，通常使用诸如裁剪、填充和水平翻转等技术。这有助于避免过度拟合和更好地概括，而不必实际收集任何新数据。最简单的方法是使用 fastai 中的`get_transforms`,它允许你从一组标准的变换中进行选择，其中默认的是为照片设计的。
2.  **将数据集分割成训练验证测试集**:我们应该总是将数据集分割成训练验证测试集。如果我们使用验证集来调优任何超参数(比如模型架构、学习率等)，那么如果您想要报告最终度量(比如准确性)，也需要有一个测试集。在这个例子中，我们没有使用验证集来调优任何超参数，所以我们没有使用测试集。实际上，你可以用 50000 个例子来训练，5000 个例子来验证，5000 个例子来测试。fastai 中的 [ImageDatabunch API](https://docs.google.com/document/d/1llqIWIsCKbTF-apBBXxAsh5yKz8XAX-HwSIN_xU844s/edit#heading=h.ew127o18zyqq) 提供了一种简单的方法来加载你的数据，并在数据已经以一些标准格式存储时将其拆分。如果你想对如何选择、分割和标记数据进行更多的自定义控制，你也可以使用[数据块 API](https://docs.fast.ai/data_block.html) 。
3.  **迁移学习**:迁移学习包括采用在一项任务中训练过的模型，然后将其用于另一项任务。这里我们使用的是在 ImageNet 上训练的名为 ResNet50 的架构，它包含大约 1400 万张图像。一旦有了基线，您还可以尝试其他架构，包括更深的 ResNet 模型。迁移学习背后的想法是，网络的大多数早期层将识别对任何图像的分类都有用的一般特征，如边缘。当我们为新任务进行训练时，我们将保持所有卷积层(称为模型的主体或主干)的权重在 ImageNet 上预先训练，但将定义一个随机初始化的新头部。该头部适应于新分类任务所需的类别数量。通过默认调用`cnn_learner`上的`fit`，我们保持身体冻结，只训练头部。

![](img/b9c36bffed17f8186660be114d3aa967.png)

3 个时代的训练

经过 3 个时期的训练，我们在这一点上获得了大约 70%的准确率。

# 修改输入图像尺寸

ResNet 最初是在 224x224 图像上训练的，我们的数据集有 32x32 个图像。如果您使用的输入图像与原始大小相差太大，那么它对于模型来说不是最佳的。让我们来看看将这些图像的大小调整为 128x128 后，对精确度的影响。ImageDataBunch API 允许您传入一个大小。

```
tfms = get_transforms(do_flip=False)
data = ImageDataBunch.from_folder(path, valid='test', ds_tfms=tfms, size=128)
learn = cnn_learner(data, models.resnet50, metrics=accuracy)
learn.fit(3)
```

![](img/32d99b3ea9b785a24b6953a80ceb238c.png)

调整输入大小后训练 3 个时期

准确率高达 94%。在这项任务中，我们已经达到了人类的水平。我们也可以将尺寸调整到 224x224，但这将增加训练时间，而且由于这些图像分辨率很低，当我们放大很多时，可能会留下伪像。一旦你有了一个基线，你可以根据需要在以后进行试验。

# 修改学习率

学习率通常是训练神经网络时要调整的最重要的超参数。它影响网络的权重随每个训练批次而修改的速率。非常小的学习率会导致训练缓慢。另一方面，非常大的学习率会导致损失函数在最小值附近波动，甚至发散。传统上，它是一个超参数，使用网格搜索和可选公式进行调整和设置，使用特定策略(如基于时间的衰减、阶跃衰减等)降低学习速率。

莱斯利·史密斯在 2015 年提出了一种设定学习率的新方法，称为[循环学习率](https://arxiv.org/abs/1506.01186) (CLR)。这种方法不是单调地降低学习率，而是让学习率在合理的边界值之间循环变化。这消除了寻找学习率的最佳值的需要。Fastai 有一个单周期 CLR 策略的实现，其中学习速率从一个低值开始，增加到一个非常大的值，然后降低到一个比其初始值低得多的值。

Fastai 还提供了一种简便的方法来估计单周期策略中使用的最大学习速率。为此，它开始训练模型，同时将学习率从非常低的值增加到非常大的值。最初，损失会下降，但随着学习率的不断上升，损失开始增加。当损失在最小值左右时，学习率已经过高。经验法则是将最大学习率设置为比最小值小一个数量级。

```
learn = cnn_learner(data, models.resnet50, metrics=accuracy)
learn.lr_find()
learn.recorder.plot()
```

![](img/87a97284400c6dd3f62af2923f7c8746.png)

使用 lr_find 估计学习率

在这里，您可以尝试将学习率设置在 1e-03 和 1-e02 之间。目前，我们只使用 1e-03。

```
learn = cnn_learner(data, models.resnet50, metrics=accuracy)
learn.fit_one_cycle(3, max_lr=1e-3)
```

![](img/5b067ceb726d322b20079512db0d68f9.png)

使用单周期策略训练 3 个时期

在 3 个时期内，我们在验证集上达到了大约 95%的准确率。

# 微调

我们之前只训练了新的随机初始化的头部，保持身体的重量不变。我们现在可以解冻身体，训练整个网络。在此之前，让我们使用`lr_find`并估计要使用的新学习率。

```
learn.lr_find()
learn.recorder.plot()
```

![](img/ed781786b071bacc8f2da8451db5a3e6.png)

使用 lr_find 再次估计学习率

训练曲线似乎已经从之前的曲线移动了一个数量级，并且看起来 1e-4 似乎是一个合理的训练速率。与以前不同，我们可能不想对身体的所有层使用相同的最大学习速率。我们可能希望使用**区别学习率**，其中早期层获得小得多的学习率。这个想法是，早期的层将学习更多的基本特征，如边缘(这对您的新任务也很有用)，而后期的层将学习更复杂的特征(如可能识别头部，这对您的新任务不一定有用)。Fastai 允许您发送一个切片，其中第一个值将用作第一层的学习率，第二个值将是最后一层的学习率，中间各层的值介于两者之间。

```
learn.unfreeze()
learn.fit_one_cycle(3, max_lr=slice(1e-6, 1e-4))
```

![](img/00cb758908c07158d8c13717506a1f85.png)

微调 3 个以上的时代

我们可以看到精度还在提高。

# 测试时间增加

您可以为通用基准测试做的最后一件事是测试时间增加(TTA)。TTA 是在测试/验证集上完成的，以提高预测质量-它包括首先使用数据增强创建每个图像的多个版本，然后我们通过我们训练的模型获得多个预测，最后我们取这些预测的平均值以获得最终预测。TTA 经常给我们一个更好的表现，而不需要任何额外的训练，但它对最终的预测时间有影响。

```
preds,targs = learn.TTA()
accuracy(preds, targs).item()
```

![](img/c5c1781bc3dc0da64975e40034ad1cb0.png)

TTA 后的精确度

我们达到了超过 96%的准确率。太好了。另一个可以提高我们准确度的简单方法就是针对更多的时期进行训练和微调。但是我们暂时就此打住。

# 我们的立场是什么？

查看 CIFAR-10 上实现的最先进的[基准](https://benchmarks.ai/cifar-10)。96%以上只是最近几年才达到的。那里的大多数模型训练的时间要长得多，使用的资源比 collab 笔记本更多，参数也更多——因此我们可以确信，我们对这种通用图像分类的 96%是非常好的。

# 结论

在一个新的机器学习项目开始时，你可以做的最有用的事情之一就是建立一个基线。对于图像分类，fastai 帮助您快速创建一个强基线，几乎不需要调整超参数。

你可以在这个 T4 笔记本中找到这篇文章的完整代码。请随意使用它并在您的项目中使用它。如果您有任何想法或问题，我很乐意与您讨论。