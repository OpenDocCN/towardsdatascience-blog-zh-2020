# Google Colab 中 ResNet 的端到端适配—第 4 部分:培训

> 原文：<https://towardsdatascience.com/end-to-end-adaptation-of-resnet-in-google-colab-part-4-training-fad7d5bff1df?source=collection_archive---------52----------------------->

## [端到端 ResNet](https://towardsdatascience.com/tagged/end2end)

## 继续，但现在是“大卡哈纳”

![](img/243e3f72c0aedaaf3e0e30e351f23e75.png)

[来源](https://unsplash.com/photos/rD2dc_2S3i0)

现在，我们必须解决整个项目的核心问题，以及在此之前的所有问题，以便无缝地进行培训——下载数据集，格式化所有内容以便输入网络，为图像处理创建数据转换，以及创建有助于以结构化方式插入图像的数据加载器。

如果你刚刚加入，我已经做了一个 colab 笔记本，让你运行整个代码(一次点击)，最后，允许你上传一张图片进行测试。这个特殊的数据集和网络旨在区分狗和猫，但你可以上传任何你想生成输出的图片。

这样做实际上是很有启发性的，尤其是对你将来想做的任何工作。**了解基础数据、其问题和偏差对于解释结果至关重要。**每个数据集都有偏差，都是不完美的，这并不一定意味着它是无用的。

以下是前面部分的链接:

[](/end-to-end-adaptation-of-resnet-in-google-colab-part-1-5e56fce934a6) [## Google Colab 中 ResNet 的端到端改编—第 1 部分

### 只需点击几下鼠标，就能训练出一个深度神经网络

towardsdatascience.com](/end-to-end-adaptation-of-resnet-in-google-colab-part-1-5e56fce934a6) [](/end-to-end-adaptation-of-resnet-in-google-colab-part-2-hardware-dataset-setup-f23dd4e0004d) [## Google Colab 中 ResNet 的端到端适配—第 2 部分:硬件和数据集设置

### 评估您的硬件— CPU 和 GPU，并设置您的数据集

towardsdatascience.com](/end-to-end-adaptation-of-resnet-in-google-colab-part-2-hardware-dataset-setup-f23dd4e0004d) [](/end-to-end-adaptation-of-resnet-in-google-colab-part-3-image-pre-processing-fe30917d9aa2) [## Google Colab 中 ResNet 的端到端适配第 3 部分:图像预处理

### 为训练准备数据集

towardsdatascience.com](/end-to-end-adaptation-of-resnet-in-google-colab-part-3-image-pre-processing-fe30917d9aa2) 

下面我们先来解开 train_model 到底是怎么回事:

我粘贴了上面的整个函数，只是为了大致了解它有多大，但它将被分成几个部分:

# 几个定义

**Epoch** —底层数据对模型的一个完整呈现(呃，*架构，*正如我告诉自己我会说的)。

**训练阶段** —如果您还记得，我们使用了 75/25 的比率(80/20 和 70/30 也很常见)，我们随机分割数据集，将 75%分配给训练集，25%分配给验证集。随机地做是很重要的，因为如果你把所有的图片都按顺序排列，如果不随机的话，你可能会在训练阶段过度表现狗或猫(结果，架构会更好地“训练”一种动物而不是另一种动物)。

**验证阶段** —我们分配给验证集的 25%不应用于培训。这是向网络显示新数据的最佳方式，以查看它是否学习得很好。就像教一个孩子加法一样，你不会想提出同样的问题，而是一个*相似的*问题来看看孩子是否学会了加法。

**优化零毕业生**——这本身并不是一个“词汇”术语，但知道这一点很重要。当梯度在小批量中生成时，它们将继续传播通过(这是每个训练周期所不希望的；你想重新开始)。zero_grad 功能使您能够为每个迷你批次重新开始计算梯度。

# 重物搬运

大部分繁重的工作都在这里完成:

第一个 if/else 语句将模型设置为训练或评估模式(pytorch 特性)。默认情况下，模型处于训练模式。有趣的是，如果您不使用 dropout layers 或批处理规范化，这个约定并不太重要，但是拥有这个约定对于将来验证您的代码(以及您如何使用 pytorch)是非常重要的。

然后使用数据加载器迭代数据，并使用“inputs.to(device)”和“labels.to(device)”行将代码加载到 GPU 中。我们之前讨论过通过 Colab 使用高端 GPU 的能力。

with torch . set _ grad _ enabled(phase = = ' train ')-括号仅在阶段确实处于训练模式时为真，允许在训练模式期间计算梯度，loss.backward(同样，在训练期间)将计算梯度，optimizer.step 使用最近的计算更新所有梯度。

现在，如果阶段不是列车，则 set_grad_enabled 变为 False，不计算梯度，也不更新任何内容。模型只评估输入的图像(这是你在训练中想要的)。

“scheduler.step()”是一行很小的代码，但是对于获得更低的错误率是至关重要的。改变学习率对超参数调整至关重要。开始时，你希望学习率高，这样才有效，随着训练的进行，你希望降低学习率。想象一下，试图找到一个领域的最低点。你希望一开始就迈出大步，这样你就不会错过任何山谷，也不会被困在地图的一个小角落里。但是当你发现山谷时，你不会想在山谷的墙壁上反弹，因为你的脚步很大。让你的步数(这里是学习率)变小会让你更有效地找到最低点。

最后，计算精度，并且如果该特定精度的历元比最佳精度(初始设置为 0)好，则更新 best_acc 变量，并且也更新模型权重，并且重复整个过程。

# **模型下载&参数确定**

名副其实，我们使用的是 Resnet18 型号。全连接层(model_ft.fc)有两个最终输出(猫对狗)。然后，我们将模型加载到 GPU 上，并将损失函数定义为 **CrossEntropyLoss** 。

对这个损失函数的讨论超出了一个介绍性系列，但可以说，它非常常用于分类(而不是回归)模型，并且被许多其他帖子和视频所涵盖。

优化函数(**SGD——随机梯度下降**)也是我鼓励你阅读(或观看 youtube 视频)的内容。

最后是学习率调度器——如上所述，我们希望学习率随着 epoch #变大而变小，以便更好地微调架构(权重)和降低损耗。

最后，我们将所有这些放在一起，让它运行:

```
model_ft = train_model(model_ft, criterion, optimizer_ft, exp_lr_scheduler,num_epochs=10)
```

我建议你至少做 5-10 个周期来说服自己有一个合理的停止点，迭代超过 100 个周期会大大减少收益。

如果你正在阅读上面的文章，并发现它过于简单，我为你领先我几光年而鼓掌。但是，如果上面的某些部分没有意义，请留下评论，我会将您连接到适当的资源。

本系列的目标不是深入研究神经网络——我没有资格这么做。我们的目标是让您相信这种架构对您来说是可行的，您可以在高端计算上运行它，并上传您自己的映像以供最终测试——我已经对此进行了概述。

下一次——上传一张照片和相关的细节。

# 参考

[1] [深度学习用 Pytorch](https://pytorch.org/tutorials/beginner/deep_learning_60min_blitz.html) ，2020 年 10 月访问

[2] [神经网络与 Pytorch。【2020 年 10 月访问](https://pytorch.org/tutorials/beginner/blitz/neural_networks_tutorial.html)

【3】[计算机视觉的迁移学习。2020 年 10 月访问](https://pytorch.org/tutorials/beginner/transfer_learning_tutorial.html)

[4] [Kaggle API。](https://github.com/Kaggle/kaggle-api)2020 年 10 月访问