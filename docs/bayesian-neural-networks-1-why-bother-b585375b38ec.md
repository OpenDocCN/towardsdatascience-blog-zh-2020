# 贝叶斯神经网络:1 为什么烦恼？

> 原文：<https://towardsdatascience.com/bayesian-neural-networks-1-why-bother-b585375b38ec?source=collection_archive---------33----------------------->

## [贝叶斯神经网络](https://towardsdatascience.com/tagged/adam-bayesian-nn)

![](img/98e5b1915267f7b11f8e93ea6d71ac5c.png)

由[法鲁尔·阿兹米](https://unsplash.com/@fahrulazmi?utm_source=medium&utm_medium=referral)在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄

**这是贝叶斯深度学习系列的第一章。下一章可用** [**此处**](/bayesian-neural-networks-2-fully-connected-in-tensorflow-and-pytorch-7bf65fb4697) **而第三章则是** [**此处**](/bayesian-neural-networks-3-bayesian-cnn-6ecd842eeff3) **。**

我经常被要求解释我对贝叶斯神经网络的古怪兴趣。所以这是一系列关于他们的开始。因此，我可以有把握地说，这是一系列关于深度学习未来的开始——一个现在就可以实现的未来。在接下来的几周里，您将学习如何使用为贝叶斯型神经网络构建的一个新库。我将包含代码并讨论 TensorFlow-Probability 和 Pytorches Pyro 的工作，包括安装、训练和预测各种不同的网络架构。在本章中，我们将讨论以下目标:

了解为什么贝叶斯神经网络如此有用和令人兴奋
了解它们与普通神经网络的不同
理解您可以获得的不确定性度量是一个主要优势
安装一个库，开始处理所有随机的事情

今天，许多深度学习的开发者并不是从计算机科学或学术背景开始的。反复试验，对一本书和一些博客的耐心大有帮助。不出所料，贝叶斯深度学习不需要任何不同。

但是一些基础理论知识的优势比平时更大。令人放心的是，这可以压缩，表达和理解相当简单。首先，是不寻常的术语引起了混淆。因此，在开始之前，我们将用一些代码来讨论它们。

我们对随机变量感兴趣。这意味着大多数变量在需要的时候都会改变值。所以非正式的随机变量是不可靠的东西(更正式的说法是我们称之为‘随机变量’，但是谁会从正式中受益呢？).使用张量流概率(TFP)或 Pytorches Pyro，您可以调用:

```
rv_normal = dist.Normal(loc=0., scale=3.)
rv_normal.sample([1])
```

这将返回一个值，每次我们运行第二行时，这个值都是不同的。即使我们不重新运行第一行也是如此，因为 rv_normal 是由正态(高斯)分布定义的随机变量。

因此，您可以将这些随机变量插入到神经网络中，并使用它们来代替我们通常用于权重的点值。你为什么要这么麻烦？因为在网络被训练之后，我们所做的每一个预测都将具有不同的随机可变权重。所以，整个模式会不一样。然而——在我放开你之前，我们用随机变量构建的神经网络的重要性在于，这些变量就像被拴在狗链上一样，只能返回特定范围内的值。这是因为我们的神经网络训练已经训练了我们的正态分布，使其钟形峰值位于最佳值附近。

从所有这些中可以得出三点:
分布拥有随机变量，每次我们做预测时，这些随机变量都会返回我们的随机权重值。
我们的神经网络训练移动整个分布，而不仅仅是一个点权重值。
这类似于在传统神经网络中使用分布初始化权重的过程(用于权重的初始值)。除了我们在整个训练和预测中继续使用分布，而不仅仅是初始化。

训练分布更正式地称为调节分布(类似于巴甫洛夫调节他的狗。(我似乎既不能逃避形式，也不能逃避狗)。所以所有这些复杂的随机变量和分布加起来就是一件事..，不同的预测。但是，如果我们已经很好地训练了贝叶斯神经网络，我们的预测将非常相似(对于连续、浮点、回归输出)和相同(对于分类输出)。).如果我们训练我们的神经网络不好，我们每次都会得到非常不同的结果。如果贝叶斯网络可以说话，它会说它的预测相似或非常不同之间的差异是它的不确定性。将不确定性视为信心(只有贝叶斯神经网络不喜欢谈论他们的信心，所以称它为不同的东西)。如果我们建立了一个良好的网络架构，最重要的是如果我们提供了良好的数据，神经网络将给出自信的预测，如果我们做的一切都不好，我们将得到不自信的预测。在贝叶斯魔法术语中，当我们给出坏消息时，神经网络会更加不确定，而当我们给出好消息时，神经网络会更加不确定。术语上的神奇差异因为贝叶斯神经网络从来都不是确定的，它们只是不太确定。而我们传统的神经网络总是确定一切！当你第一次开始使用神经网络时，你可能已经看到了类似的东西。您完成了对第一个模型的训练，并且表现一般，然后您在不做任何更改的情况下训练了另一个模型，平均来说仍然表现一般，只是您得到了完全不同的结果。贝叶斯神经网络就像那样，只是不是依靠你的挫折来保持训练有微小差异的神经网络，而是通过贝叶斯方法，你训练一次，就内置了无限数量的模型。

太棒了。掌声。那又怎样？所以…我们可以改变这一切，通过观察结果的可变性来看我们的数据(主要)有多适合这项任务。但比贝叶斯模型好得多的是，我们通常能够用比神经网络常规所需数据更少的数据来训练模型。贝叶斯模型将很好地推广。它也较少受到每个训练类的数据量不平衡的困扰——这是表现不佳的一个被忽视的原因。

说够了，我的好人，告诉我们怎么做。对哦。

首先，您需要安装两个主要概率编程库中的一个，以遵循本系列。虽然您可以仅使用核心库来构建贝叶斯神经网络，但您需要手动实现训练代码，这不会在数学和隐藏错误方面感到害羞。所以我们要么用 Pyro(建立在 Pytorch 之上)，要么用 TensorFlow-Probability(可能很明显建立在 TensorFlow 之上)。如果您有使用某个核心库的经验，最好继续使用它，并从您的熟悉中获益，而不是改变它。滚动到你喜欢的一个相关的库部分，或者如果你是一个受虐狂，继续执行它们，然后给我发电子邮件。

# 张量流概率

我们将使用 TensorFlow 2 作为基础库，它有一些类似于 PyTorch 的实现，尽管受益于我们将在后面章节中使用的许多特性。TensorFlow 通常也受益于语言和平台的可移植性。您可以从从 Java 到 C#的几乎任何语言中调用用 Python TensorFlow 编写的模型，并非常容易地将模型部署到移动设备上。

TensorFlow 和 TensorFlow-Probability 很容易安装。确保你已经安装了 Python(Python 3，如果你还没有这个或者任何 Python 发行版，我强烈推荐你从:[www.anaconda.com](http://www.anaconda.com)下载 Anaconda 发行版，它包含了你在这里开始所需要的一切。)

## 安装张量流和张量流-概率:

如果要使用 Anaconda/Miniconda 安装 TensorFlow，请使用 conda 软件包管理器并键入:

```
conda install tensorflow
```

或者对于 GPU 支持:

```
conda install tensorflow-gpu
```

在撰写本文时，如果你想获得 GPU 支持，用 conda 而不是 pip 安装有一些好处。conda 版本自动安装数学库，显著提高 CPU 训练性能。使用 conda 安装 GPU(tensor flow-GPU)也更容易，因为默认情况下，它会安装 CUDA 和 cuDNN，而这两个软件需要使用 pip 进行独立安装。GPU versoins 要求机器上有 CUDA(即 NVIDIA) GPU。然而，conda 并不像 pip 那样包含那么多不同的库，当同时使用 pip 时很容易崩溃。

要使用 pip 安装计算(非 GPU)版本:

```
pip install — upgrade tensorflow==2.*
```

或者，如果您想使用带画中画的 GPU 版本:

```
pip install — upgrade tensorflow-gpu==2.*)
```

如果你不确定，选择第一个。2 号。*部分确保安装最新的 2 个版本。该系列大部分可以通过 TensorFlow 1.13 以上的版本完成，但需要在导入 tensor flow 后使用:tf.enable_eager_execution()激活急切执行(在 TF2 默认激活)，此外还需要在该系列后面的章节中进行一些其他更改。

> ***注意:*** *在安装 TensorFlow-Probability 之前必须安装核心 TensorFlow 库，意外的是 TensorFlow-Probability 安装不会为你安装核心库作为依赖。*

通过 conda 安装 TFP，包括:

```
conda install -c conda-forge tensorflow-probability
```

对于 pip:

```
pip install — upgrade tensorflow-probability
```

然后检查一切是否正常，创建一个新的 [Jupyter 笔记本](https://jupyter.org/)，IPython 实例或 Python 脚本，并添加:

```
import tensorflow as tf
import tensorflow_probability as tfp
dist = tfp.distributionsrv_normal = dist.Normal(loc=0., scale=3.)
print(rv_normal.sample([1]))
```

你会认出这一章开头的最后两行。你在对一个随机变量进行采样，所以作为输出，你应该得到一个张量，它包含了每次执行最后一行时不同的随机值。

[Out]: tf。张量([2.2621622]，shape=(1，)dtype=float32)

# 安装 Pytorch 和 Pyro:

对于 Pyro，你首先需要安装 PyTorch。通常，这可以通过 conda 以下列方式最容易地完成:

```
 conda install pytorch-cpu torchvision-cpu -c pytorch
```

或者如果有 NIVIDA GPU 可供您使用:

```
 conda install pytorch torchvision cudatoolkit=9.0 -c pytorch
```

Pip 安装也是可能的，但不幸的是平台*和*的 CUDA 版本有很大不同，因此如果你只想使用 pip，建议你参考 pytorch.org 的安装指南。

Pyro 随后安装有:

```
conda install -c gwerbin pyro-ppl
```

或者

```
 pip install pyro-ppl 
```

> **注意:**软件包‘Pyro’(没有-ppl)是完全不同的软件，与 Pytorch-Pyro 无关。

要开始使用 Pyro，创建一个 [Jupyter 笔记本](https://jupyter.org/)，Ipython 控制台或 python 脚本并键入:

```
import torch
import pyro
import pyro.distributions as dist
rv_normal = dist.Normal(loc=0., scale=3.)
print(rv_normal.sample([1]))
```

你会认出这一章开头的最后两行。你在对一个随机变量进行采样，所以作为输出，你应该得到一个张量，它包含了每次执行最后一行时不同的随机值。

[Out]:张量([0.0514])

# 摘要

你已经学习了随机变量和分布之间的关系，并看到了它的作用。您实现了一个位于 0、标度为 3 的正态分布。位置是分布平均值的另一个词，而标度是标准差的另一个词。然而，我们将坚持使用术语位置和比例，因为这些术语有助于我们在处理其他(非正态)分布时保持一致。

下周我们将学习如何实现贝叶斯神经网络，并进一步了解这些分布和随机变量的适用范围。我打赌你等不及了！订阅博客以获得关于下一版本的通知。

[1]:你可能也听说过 PyMC3，这是另一个很好的库，你可以用它来构建概率模型。然而，PyMC3 总体上特别倾向于统计建模，对使用神经网络的人不太友好(例如，自动签名和 GPU 支持更具挑战性)。

[2]:例如，TensorFlow 2 删除了设置图形所需的“会话”上下文，转而使用函数。

[3]:如果你不能选择使用哪个库，你会发现 TensorFlow-Probability 比 Pyro 更容易使用和理解。然而，这份关于 Pyro 的文档非常好，但是从神经网络的角度来解释 TFP 却很简单。