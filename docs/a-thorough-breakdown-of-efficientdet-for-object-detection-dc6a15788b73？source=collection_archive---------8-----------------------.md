# 物体探测效率的彻底崩溃

> 原文：<https://towardsdatascience.com/a-thorough-breakdown-of-efficientdet-for-object-detection-dc6a15788b73?source=collection_archive---------8----------------------->

## 在这篇文章中，我们深入探讨了用于物体检测的 EfficientDet 的结构，重点是模型的动机、设计和架构。*我们还在博客上发布了* [*物体探测效率分析*](https://blog.roboflow.ai/breaking-down-efficientdet/) *。*

最近，Google Brain 团队[发布了](https://arxiv.org/abs/1911.09070)他们的 EfficientDet 对象检测模型[目标是将架构决策具体化为一个可扩展的框架，该框架可以轻松应用于对象检测的其他用例。该论文的结论是 **EfficientDet 在基准数据集上的表现优于类似规模的模型**。在 Roboflow，我们发现基本的](https://blog.roboflow.com/the-ultimate-guide-to-object-detection/) [EfficientDet 模型可以推广到托管在我们平台上的自定义数据集](https://blog.roboflow.ai/training-efficientdet-object-detection-model-with-a-custom-dataset/)。(关于实现 EfficientDet 的深入教程，请参见这篇关于[如何训练 EfficientDet](https://blog.roboflow.ai/training-efficientdet-object-detection-model-with-a-custom-dataset/) 的博文和这篇关于如何训练 EfficientDet 的 [Colab 笔记本。)](https://colab.research.google.com/drive/1ZmbeTro4SqT7h_TfW63MLdqbrCUk_1br#scrollTo=KwDS9qqBbMQa)

在这篇博客文章中，我们探讨了在形成最终 EfficientDet 模型时所做决策背后的基本原理，EfficientDet 如何工作，以及 EfficientDet 与 YOLOv3、fast R-CNN 和 MobileNet 等流行的对象检测模型相比如何。<and now="" an="" class="ae kg" href="https://blog.roboflow.ai/a-thorough-breakdown-of-yolov4/" rel="noopener ugc nofollow" target="_blank">约洛夫 4 的说明和约洛夫 5 的>的说明</and>

![](img/e51cf8e797d41c0d916515b685b7fd88.png)

探索激发 EfficientDet 创作的理论基础。[图片来源](https://www.clipart.email/)和[引用论文](https://arxiv.org/abs/1911.09070)

```
The founder of Mosaic Augmentation, Glen Jocher has released a new YOLO training framework titled YOLOv5\. You may also want to see our post on [YOLOv5 vs YOLOv4](https://blog.roboflow.ai/yolov5-improvements-and-evaluation/) This post will explain some of the pros of the new YOLOv5 framework, and help illuminate breakthroughs that have happened since the EfficientDet publication.[YOLOv5 Breakdown](https://blog.roboflow.ai/yolov5-improvements-and-evaluation/)
```

# 深度学习效率面临的挑战

在探索该模型之前，这里有一些阻碍图像检测系统部署到现实生活用例中的关键领域。

1.  **数据收集** —借助模型架构和预训练检查点，EfficientDet 减少了推广到新领域所需的数据量。
2.  **模型设计和超参数化** —一旦收集了数据，机器学习工程师需要仔细设置模型设计并调整许多超参数。
3.  **训练时间** —在收集的数据集上训练模型所需的时间。在 EfficientDet 论文中，这是以 FLOPS(每秒浮点运算次数)来衡量的。
4.  **内存占用** —一旦模型被训练，当被调用进行推理时，需要多少内存来存储模型权重？
5.  **推理时间** —当模型被调用时，它能足够快地执行预测以用于生产设置吗？

# 图像特征和 CNN

训练对象检测模型的第一步是将图像的像素转换成可以通过神经网络输入的特征。通过使用[卷积神经网络](https://cs231n.github.io/convolutional-networks/)从图像中创建可学习的特征，计算机视觉领域取得了重大进展。卷积神经网络以不同的粒度级别混合和汇集图像特征，允许模型在学习手头的图像检测任务时选择可能的组合。然而，一段时间以来，卷积神经网络(ConvNet)创建特征的确切方式一直是研究社区感兴趣的领域。ConvNet 版本包括 ResNet、NASNet、YoloV3、Inception、DenseNet 等，每一个版本都试图通过缩放 ConvNet 模型大小和调整 ConvNet 设计来提高图像检测性能。这些 ConvNet 模型是以可扩展的方式提供的，因此如果资源允许，程序员可以部署更大的模型来提高性能。

# 效率网:动机和设计

最近，谷歌大脑团队发布了他们自己的 ConvNet 模型，名为 EfficientNet。 **EfficientNet 构成了 EfficientDet** 架构的主干，因此在继续介绍 EfficientDet 的贡献之前，我们将介绍它的设计。EfficientNet 着手研究 ConvNet 架构的扩展过程。有很多方法——事实证明💭-您可以向 ConvNet 添加更多参数。

![](img/2000ab45342172e9fcb6101f5c618c8e.png)

[引用论文](https://arxiv.org/abs/1911.09070)

你可以使每一层更宽，你可以使层数更深，你可以以更高的分辨率输入图像，或者你可以将这些改进结合起来。你可以想象，对机器学习研究人员来说，探索所有这些可能性可能会非常*乏味*。EfficientNet 开始定义一个自动程序来扩展 ConvNet 模型架构。本文试图在给定深度、宽度和分辨率的自由范围内优化下游性能，同时保持在目标存储器和目标触发器的限制范围内。他们发现，他们的扩展方法改进了以前的 ConvNets 的优化及其高效的网络架构。

# 高效网络、扩展和评估

创建一种新的模型缩放技术是向前迈出的一大步，作者通过创建一种新的 ConvNet 架构将他们的发现向前推进了一步，从而将他们的最新成果推向了更高的水平。通过神经结构搜索发现新的模型结构。在给定一定数量的 FLOPS 的情况下，神经架构搜索可优化精度，并创建一个名为 EfficientNet-B0 的基线 ConvNet。使用扩展搜索，EfficientNet-B0 将扩展到 EfficientNet-B1。从 EfficientNet-B0 到 EfficientNet-B1 的缩放功能被保存并应用于通过 EfficientNet-B7 的后续缩放，因为额外的搜索变得极其昂贵。

新的高效网络系列在 ImageNet 排行榜上进行评估，这是一项图像分类任务。注意:自从 EfficientNet 最初发布以来，已经有了一些改进，包括从[优化分辨率差异](https://arxiv.org/pdf/2003.08237v3.pdf)以及将 EfficientNet 部署为[教师和学生](https://arxiv.org/pdf/1911.04252.pdf)。

![](img/7ce2deeed61d767ed65826a3fbdba86f.png)

[引用论文](https://arxiv.org/abs/1911.09070)

相当甜蜜！EfficientNet 看起来是一个很好的基础。它可以有效地随模型大小扩展，性能优于其他 ConvNet 主干网。

到目前为止，我们已经介绍了 EfficientDet 网络的以下部分:

![](img/95f212aa683e2b38ea73a93a4099f652.png)

[引用论文](https://arxiv.org/abs/1911.09070)

# 引入效率检测

现在，我们将讨论 EfficientDet 的贡献，它试图回答以下问题:我们应该如何准确地将 ConvNets 的功能结合起来用于对象检测？一旦我们开发了这个组合过程，我们应该如何扩展我们模型的架构呢？

# 特征融合

特征融合寻求组合给定图像在不同分辨率下的表示。通常，融合使用 ConvNet 的最后几个要素图层，但确切的神经架构可能会有所不同。

![](img/bd62de99bb115249055e5fe6d1ca62c1.png)

[引用论文](https://arxiv.org/abs/1911.09070)

在上图中，FPN 是一种自上而下融合要素的基本方法。PA net 允许特征融合从较小的分辨率到较大的分辨率来回流动。NAS-FPN 是一种通过神经架构搜索发现的特征融合技术，它看起来肯定不像人们可能想到的第一个设计。EfficientDet 论文使用“直觉”(可能还有许多许多开发集)来编辑 NAS-FPN 的结构，以确定双向功能金字塔网络 BiFPN。EfficientDet 模型将这些 BiFPN 块堆叠在一起。在模型缩放过程中，块的数量是不同的。此外，作者假设某些特征和特征通道对最终预测的贡献可能不同，因此他们在通道的开头添加了一组可学习的权重。

# 效率检测模型缩放

先前关于图像检测的模型缩放的工作通常独立地缩放网络的部分。例如，ResNet 仅扩展主干网络的规模。但是还没有探索联合标度函数。这种方法非常类似于为创建 EfficientNet 而进行的联合扩展工作。

作者设置了一个缩放问题来改变主干网络、BiFPN 网络、类/箱网络和输入分辨率的大小。主干网络通过 EfficientNet-B0 到 EfficientNet-B6 的预训练检查点直接扩展。BiFPN 网络的宽度和深度随着 BiFPN 堆叠的数量而变化。

# EfficientDet 模型评估和讨论

EfficientDet 模型在 COCO(上下文中的公共对象)数据集上进行评估，该数据集包含大约 170 个图像类和跨越 100，000 个图像的注释。COCO 被认为是对象检测的通用挑战。如果模型在这个一般领域表现良好，它很可能在更具体的任务上表现得很好。在许多约束条件下，EfficientDet 优于以前的对象检测模型。下面，我们来看看该模型的性能与 FLOPS 的关系。

![](img/1659ccfe24480a0cf39589f559915e6b.png)

[引用论文](https://arxiv.org/abs/1911.09070)

在这里，我们可以看到，在类似的约束条件下，该模型相对于其他模型族表现得相当好。作者还对 Pascal VOC 上的语义切分模型进行了评估。他们发现他们在那里也达到了艺术水平。嘿，为什么不呢？

# 简单来说，EfficientDet 为什么有用？

从实现细节上退一步，想想 EfficientDet 的开源检查点对计算机视觉工程师来说意味着什么是相当不可思议的。**efficient net 的预训练检查点结晶了所有的发现**和谷歌大脑的研究人员在建立一个 ConvNet 时放置的自动化，以及**ImageNet 上的图像分类可以提供的所有监督**。功能融合进一步利用了 EfficientNet 检查点**，并且架构的所有组件都得到了有效扩展**。最后，这些模型权重在 COCO 上进行预训练，COCO 是一个通用的图像检测数据集。作为一个用户，除了提供模型的数据类型之外，没有什么决策需要考虑。

# 很好的分解——我如何使用 EfficientDet？

在 [Roboflow](https://roboflow.ai) 上，我们已经在这篇关于[如何训练 EfficientDet](/training-efficientdet-object-detection-model-with-a-custom-dataset-25fb0f190555) 的博文和这本关于如何训练 EfficientDet 的 [Colab 笔记本上提供了一个教程。通过 Roboflow，您可以输入带有注释的数据集，只需将新的数据下载链接输入到我们的示例中，就可以得到一些结果。然后，在训练之后，笔记本将训练好的重量导出，以便部署到应用程序中！](https://colab.research.google.com/drive/1ZmbeTro4SqT7h_TfW63MLdqbrCUk_1br#scrollTo=KwDS9qqBbMQa)

![](img/c51fc6f30e7100a8dfa6415d933a4bdf.png)

使用我们的 Colab 笔记本中的 efficientDet 进行推理

Roboflow 是免费使用的，您可以托管和转换多达 1000 张图像。保持联系！

如果您对 EfficientDet 有一些很棒的成果，并想与我们分享，[请给我们写信🎣！](https://roboflow.ai/contact)