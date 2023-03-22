# 通过多 GPU/TPU 训练扩展逻辑回归

> 原文：<https://towardsdatascience.com/scaling-logistic-regression-for-multi-gpu-tpu-training-b4898d5049ff?source=collection_archive---------41----------------------->

## 了解如何使用带有 PyTorch 闪电的 GPU 和 TPU 将逻辑回归扩展到大规模数据集。

![](img/d949cb4e426e825db84b7d221c9e7f92.png)

这种逻辑回归实现旨在利用大型计算集群([来源](https://www.pxfuel.com/en/free-photo-eortv))

逻辑回归是一种简单但强大的分类算法。在这篇博文中，我们将看到我们可以 ***将逻辑回归视为一种神经网络*** 。

将其框架化为神经网络允许我们使用 PyTorch 和 [PyTorch Lightning](https://github.com/PyTorchLightning/pytorch-lightning) 这样的库在硬件加速器上进行训练(比如 GPU/TPU)。这使得分布式实现可以扩展到大规模数据集。

在这篇博文中，我将通过将 NumPy 实现连接到 PyTorch 来说明这个链接。

我已经将这个高度可扩展的逻辑回归实现添加到了 [PyTorch Lightning Bolts](https://pytorch-lightning.readthedocs.io/en/stable/bolts.html) 库中，您可以轻松地使用它在自己的数据集上进行训练。

![](img/0047102e499d8c0dc6795c4c402d76a4.png)

来源: [PyTorch 闪电](https://pytorch-lightning.readthedocs.io/en/stable/)

# 跟着走

如果您想自己运行每个步骤中的代码，我们将浏览的所有代码示例都可以在这个[配套的 Colab](https://colab.research.google.com/drive/1KihqFRl_Z89MfPH-pNHldvKVMGawVi-U?usp=sharing) 笔记本中找到。

# 为什么我需要可伸缩的逻辑回归？

使用 8 个 GPU 拟合逻辑回归的示例。

就像其他 ML 库一样(例如 Sci-kit Learn ),这个实现将允许你在几行代码中建立一个逻辑回归模型。与其他库不同，您可以在许多机器上的多个 GPU、TPU 或 CPU 上训练大量数据集。

除了具有十几个要素的玩具数据集之外，真实数据集可能具有数万个要素和数百万个样本。这种规模，CPU 根本不行。相反，我们可以利用 GPU 和 TPU 将几天的训练变成几分钟。

例如，在本教程结束时，我们在几秒钟内在 1 个 GPU 上对包含 70，000 幅图像和 784 个特征的完整 MNIST 数据集进行训练。事实上，我们甚至尝试过 ImageNet。我们的逻辑回归实现可以在大约 30 分钟内在 2 个 GPU(v100)上循环遍历所有 130 万个图像，每个图像具有 150，528 个像素(输入特征)。

# 作为基线的逻辑回归

不管您的分类问题有多复杂，使用逻辑回归分类器设置一个强基线总是一个好的实践。即使您打算使用更复杂的方法，如神经网络。

从 PyTorch 中实现的逻辑回归基线开始的优点是，它使得用神经网络替换逻辑回归模型变得容易。

# 逻辑回归概述

如果你熟悉逻辑回归，可以跳过这一节。

我们使用逻辑回归来预测一个离散的类别标签(如猫对狗)，这也称为分类。这与回归不同，回归的目标是预测一个连续的实值量(如股票价格)。

在最简单的逻辑回归案例中，我们只有 2 个类别，这被称为*二元分类*。

## 二元分类

我们在逻辑回归中的目标是从输入值或特征矩阵`X`中预测二元目标变量`Y`(即 0 或 1)。例如，假设我们有一群宠物，我们想根据耳朵形状、体重、尾巴长度等特征找出哪只是猫或狗(`Y`)。(`X`)。设 0 代表猫，1 代表狗。我们有`n`样品或宠物，以及关于每只宠物的`m`特征:

![](img/a0d5acd7e0182d812b1e42ba0461013a.png)

我们要预测宠物是狗的概率。为此，我们首先对输入变量进行加权求和——让`w`表示权重矩阵。特征`X`和权重`w`的线性组合由向量`z`给出。

![](img/19bb76e0b0d75e4d8f6dc53ab33cf0e3.png)

接下来，我们将 sigmoid 函数应用于`z`向量的每个元素，从而得到向量`y_hat`。

![](img/10783401622a504f3f405cfba46e5005.png)

*S 形函数*，也称为*逻辑函数*，是一个 S 形函数，将`z`的值“挤压”到范围`[0,1]`内。

![](img/bf1f0095550e0dfad13c8262672094c2.png)

Sigmoid 函数(作者自己的图像)

由于`y_hat`中的每个值现在都在 0 和 1 之间，我们将此解释为给定样本属于“1”类(与“0”类相对)的概率。在这种情况下，我们将把`y_hat`解释为宠物是狗的概率。

我们的目标是找到`w`参数的最佳选择。我们希望找到一个`w`，使得当`x`属于“1”类时`P(y=1|x)`的概率大，当`x`属于“0”类时`P(y=1|x)`的概率小(在这种情况下`P(y=0|x) = 1 — P(y=1|x)`大)。

请注意，每个型号都是通过选择`w`完全指定的。我们可以使用*二元交叉熵*损失或对数损失函数来评估特定模型的表现。我们希望了解我们的模型预测与训练集中的真实值有多“远”。

![](img/0b1488ad518ff2370623ab69a0078b30.png)

二元交叉熵损失

注意，对于每个训练示例，求和中的两项中只有一项是非零的(取决于真实标签`y`是 0 还是 1)。在我们的例子中，当我们有一只狗(即`y = 1` ) *时，最小化*的损失意味着我们需要使`y_hat = P(y=1|x)`变大。如果我们有一只猫(即`y = 0`)，我们想把`1 — y_hat = P(y=0|x)`做大。

现在我们有了损失函数来衡量给定的`w`与我们的训练数据有多吻合。我们可以通过最小化`L(w)`来学习分类我们的训练数据，以找到`w`的最佳选择。

我们可以搜索最佳`w`的一种方式是通过迭代优化算法，如*梯度下降。*为了使用梯度下降算法，我们需要能够针对`w`的任何值计算损失函数相对于`w`的导数。

注意，由于 sigmoid 函数是可微的，损失函数相对于`w`是可微的。这允许我们使用梯度下降，也允许我们使用自动微分包，如 PyTorch，来训练我们的逻辑回归分类器！

## 多类分类

![](img/1ad45265900986572ae0984fe6e57ba2.png)

二元对多类分类(作者自己的图像)

我们可以将上面的内容推广到多类设置，其中标签`y`可以有`K`个不同的值，而不是只有两个。注意，我们从 0 开始索引。

![](img/45050ce4cac61c4a192219859b114523.png)

现在的目标是估计类别标签采用每个`K`不同可能值的概率，即每个`k = 0, …, K-1`的`P(y=k|x)`。因此，预测函数将输出一个 K 维向量，其元素总和为 1，以给出 K 个估计概率。

具体来说，假设类现在采用以下形式:

![](img/2d2ef5df83ace7e8a9d7bc224e936630.png)

这也被称为*多项式逻辑回归*或 *softmax 回归*。

关于维度的注释——上面我们只看了一个例子，`x`是一个`m x 1`向量，`y`是一个在`0`和`K-1`之间的整数值，让`w(k)`表示一个`m x 1`向量，它代表第 k 个类的特征权重。

输出向量的每个元素采用以下形式:

![](img/dbcac94267205d12bc852e51be7090d3.png)

这被称为 *softmax 功能*。softmax 将任意的真实值转化为概率。softmax 函数的输出始终在[0，1]范围内，分母中的求和意味着所有项的总和为 1。因此，它们形成一个概率分布。它可以被视为 sigmoid 函数的推广，在二进制情况下，softmax 函数实际上简化为 sigmoid 函数(试着自己证明这一点！)

为了方便起见，我们指定矩阵`W`来表示模型的所有参数。我们将所有的`w(k)`向量连接成列，使得矩阵`W`具有维度`m x k`。

![](img/a5f5c03f90dc82c4c93bd44b5a2c9f37.png)

与二进制情况一样，我们训练的目标是学习使交叉熵损失函数最小化的`W`值(二进制公式的扩展)。

# 现在来看神经网络

我们可以认为逻辑回归是一个完全连接的单层神经网络，后面跟着 softmax 函数。

![](img/78f7310d06c044917da0601b7a63facc.png)

逻辑回归可以看作是一个单层神经网络(作者自己的图像)。

输入层为每个特征包含一个神经元(可能为一个偏差项包含一个神经元)。在二进制的情况下，输出层包含 1 个神经元。在多类情况下，输出层包含每个类的一个神经元。

# NumPy 与 PyTorch 代码

事实上，传统的逻辑回归和神经网络公式是等价的。这在代码中最容易看到——我们可以证明原始公式的 NumPy 实现等价于在 PyTorch 中指定一个神经网络。为了说明这一点，我们将使用 Iris 数据集中的例子。

**虹膜数据集**是一个非常简单的数据集，用于演示多类分类。有 3 个类别(编码为 0，1，2)代表鸢尾花的类型(setosa，versicolor 和 virginica)。植物萼片和花瓣的长度和宽度有 4 个实值特征。

![](img/319f6d142ccd228cb2384e6041572a4e.png)

虹膜数据集(作者自己的)

下面是我们如何使用 sci-kit learn datasets 库中的工具加载和混洗 Iris 数据集:

让我们检查数据集维度:

![](img/ef67a65c0f99b1f2106dd26438860706.png)

有 150 个样本——事实上我们知道每个类有 50 个样本。现在，让我们挑选两个具体的例子:

![](img/14410406cde6c904be02c6de930ca18f.png)

上面我们看到，`x`有维度`2 x 4`，因为我们有两个例子，每个例子有 4 个特征。`y`有`2 x 1`值，因为我们有两个例子，每个例子都有一个标签，可以是 0，1，2，其中每个数字代表类的名称(versicolor，setosa，virginica)。

现在我们将创建我们想要学习的权重参数`w`。因为 3 个类中的每一个都有 4 个特征，所以`w`的尺寸必须是`4 x 3`。

注意:为了简单起见，我们只看特性，这里不包括偏见术语。现在我们有了，

![](img/ee9f3858b1c3c0439428a1e7646673f7.png)

## 在 NumPy 中学习 w:

根据逻辑回归公式，我们先计算`z = xw`。`z`的形状是 2 x 3，因为我们有两个样本和三个可能的类。这些原始分数需要被标准化为概率。我们通过对`z`的每一行应用 softmax 函数来实现这一点。

因此，最终输出`y_hat`具有与`z`相同的形状，但是现在每行总和为 1，并且该行的每个元素给出了该列索引作为预测类的概率。

![](img/2dff10dbc41c370215f8c1b479669d72.png)

在上面的示例中，`y_hat`的第 1 行中的最大数字是 0.6，这给出了作为预测类的第二个类标签(1)。对于第二行，预测的类别也是 1，但概率为 0.5。事实上，在第二行中，标签 0 也有 0.41 的高概率。

## 在 PyTorch:

现在让我们用 PyTorch 实现完全相同的东西。

首先，我们需要将 NumPy 数组转换成 PyTorch 张量。

然后定义神经网络的线性层和 softmax 激活函数。

PyTorch 自动初始化随机权重，所以我用上面使用的相同权重矩阵显式替换权重。

现在我们计算这个一层神经网络的输出，看到它和上面完全一样。

![](img/605207c78503ef9ab4aacf3421eaafc4.png)

当训练神经网络学习最优`W`时，上述步骤被称为*正向传递*(计算输出和损失)。接着是*向后传球。*在反向传递过程中，我们使用*反向传播*算法(本质上是链式法则)(link)计算每个权重参数的损失梯度。最后，我们使用梯度下降算法来更新每个权重的值。这构成了一次迭代。我们重复这些步骤，迭代地更新权重，直到模型收敛或者应用了一些停止标准。

如您所见，无论您使用 NumPy 还是 PyTorch 之类的神经网络库，输出都是相同的。然而，PyTorch 使用张量数据结构而不是 NumPy 数组，后者针对硬件加速器(如 GPU 和 TPU)上的快速性能进行了优化。因此，这允许逻辑回归的可扩展实现。

使用 PyTorch 的第二个优点是，我们可以使用自动微分来有效地自动计算这些梯度。

# 使用 PyTorch 闪电

虽然像我刚才做的那样实现一个简单的逻辑回归示例很简单，但是在许多机器上正确地在多个 GPU/TPU 上分发数据批次和训练变得非常困难。

然而，通过使用 PyTorch Lightning，我实现了一个处理所有这些细节的版本，并将其发布在 [PyTorch Lightning Bolts](https://pytorch-lightning-bolts.readthedocs.io/en/latest/classic_ml.html#logistic-regression) 库中。这种实现使得在任何数据集上定制和训练该模型变得很简单。

## 1.首先，安装螺栓:

```
pip install pytorch-lightning-bolts
```

## 2.导入模型并实例化它:

我们指定输入要素的数量、类的数量以及是否包含偏差项(默认情况下设置为 true)。对于虹膜数据集，我们将指定`in_features=4`和`num_classes=3`。您还可以指定学习率、L1 和/或 L2 正则化。

## 3.加载数据，可以是任何 NumPy 数组。

让我们继续以虹膜数据集为例:

上面你看到的是你如何使用一个叫做*数据集*和*数据加载器*的东西在 PyTorch 中加载数据。数据集只是 PyTorch 张量格式的示例和标签的集合。

![](img/abdefc3abb2dfdea133c9fae101743e8.png)

数据加载器有助于高效地迭代数据集的批次(或子集)。数据加载器指定了诸如批量大小、无序播放和数据转换等内容。

![](img/021b44b7fef25559bdddd01776e5df62.png)

如果我们在 DataLoader 中迭代一个批处理，我们会看到`x`和`y`都包含 10 个样本，因为我们指定了 10 个批处理大小。

**数据模块:**

然而，在大多数方法中，我们还需要对数据进行训练、验证和测试。有了 PyTorch Lightning，我们有了一个极其方便的类叫做 [*DataModule*](https://pytorch-lightning.readthedocs.io/en/stable/datamodules.html) 来为我们自动计算这些。

我们使用[*SklearnDataModule*](https://pytorch-lightning-bolts.readthedocs.io/en/stable/api/pl_bolts.datamodules.sklearn_datamodule.html)——输入任何 NumPy 数据集，定制您希望如何分割数据集，它将返回数据加载器供您输入到您的模型中。

一个[数据模块](https://pytorch-lightning.readthedocs.io/en/stable/datamodules.html)只不过是一个训练数据加载器、验证数据加载器和测试数据加载器的集合。除此之外，它还允许您完全指定和组合所有数据准备步骤(如拆分或数据转换)以实现重现性。

![](img/c3710c79e306098622916136a790350b.png)

我已经指定将数据分成 80%的训练、10%的验证、10%的测试，但是如果您想要使用自己的自定义拆分，也可以传入您自己的验证和测试数据集(作为 NumPy 数组)。

假设您有一个包含 20 个样本的自定义测试集，并希望使用 10%的训练集进行验证:

分割数据是很好的做法，但完全是可选的——如果不想使用验证或测试集，只需将`val_split`和`test_split`中的一个或两个设置为 0。

## 4.训练模型

现在我们有了数据，让我们在 1 个 GPU 上训练我们的逻辑回归模型。当我呼叫`trainer.fit`(第 6 行)时，训练将开始。我们可以调用`trainer.test`来查看模型在我们的测试集上的性能:

我们最终的测试集准确率是 100%——这是我们使用 Iris 这样完全可分离的玩具数据集所能享受到的。

由于我是在一台配有 1 个 GPU 的 Colab 笔记本上进行这方面的训练，所以我将`gpus=1`指定为`Trainer`的一个参数——然而，在任何类型的多个硬件加速器上进行训练都非常简单:

![](img/4de8c6e6a1dce527d40d44a91b06bae2.png)

PyTorch Lightning 在幕后处理分布式培训的所有细节，以便您可以专注于模型代码。

# 基于完整 MNIST 数据集的训练

在大型数据集上运行时，GPU 和 TPU 上的培训非常有用。对于像 Iris 这样的小数据集，硬件加速器并没有太大的作用。

例如，手写数字的原始 MNIST 数据集包含 70，000 张 28*28 像素的图像。这意味着输入特征空间的维数为 784，输出空间中有 K = 10 个不同的类。事实上， [Sci-kit Learn](https://scikit-learn.org/stable/auto_examples/linear_model/plot_sparse_logistic_regression_mnist.html) 中的实现并不使用原始数据集，而是使用仅 8*8 像素的降采样图像。

![](img/1c92cff00df7fee65e81af9b5ad17ed3.png)

MNIST 数据集([来源](https://commons.wikimedia.org/wiki/File:MnistExamples.png))

给定 785 个特征(偏差为 28*28 + 1)和 10 个类，这意味着我们的模型有 7850 个我们正在学习的权重(！).

Bolts 方便地提供了一个`[MNISTDataModule](https://pytorch-lightning-bolts.readthedocs.io/en/stable/api/pl_bolts.datamodules.mnist_datamodule.html#pl_bolts.datamodules.mnist_datamodule.MNISTDataModule)`来下载、分割和应用标准转换，比如对 MNIST 数字图像进行图像归一化。

我们再次使用 1 个 GPU 进行训练，并看到最终的测试集准确率(对 10，000 个看不见的图像)为 92%。相当令人印象深刻！

# 规模更大

鉴于 PyTorch 的效率和 PyTorch Lightning 的便利，我们甚至可以扩展这个逻辑回归模型，在包含数百万样本的大规模数据集上进行训练，如 ImageNet。

如果您有兴趣了解更多关于闪电/闪电的信息，我建议您查看以下资源:

*   [PyTorch 闪电——从 TPU 的线性逻辑回归到预培训的 GANs](https://medium.com/pytorch/pytorch-lightning-bolts-from-boosted-regression-on-tpus-to-pre-trained-gans-5cebdb1f99fe)
*   【PyTorch Lightning 如何成为第一个在 TPUs 上运行持续集成的 ML 框架
*   [从 PyTorch 到 PyTorch 闪电](/from-pytorch-to-pytorch-lightning-a-gentle-introduction-b371b7caaf09)

# 如何开始

希望这份指南能准确地告诉你如何开始。最简单的开始方式是用我们已经看过的所有代码例子运行 [Colab 笔记本](https://colab.research.google.com/drive/1KihqFRl_Z89MfPH-pNHldvKVMGawVi-U?usp=sharing)。

你可以自己试试，只要装上螺栓，随便玩玩！它包括不同的数据集，模型，损失和回调，你可以混合和匹配，子类化，并在你自己的数据上运行。

```
pip install pytorch-lightning-bolts
```

或者查看 [PyTorch 闪电](https://pytorch-lightning-bolts.readthedocs.io/en/latest/classic_ml.html)。

![](img/7fdb9e78525de29222090974126d924b.png)

来源: [PL 螺栓 Github](https://github.com/PyTorchLightning/pytorch-lightning-bolts)