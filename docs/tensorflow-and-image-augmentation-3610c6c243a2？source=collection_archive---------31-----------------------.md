# 张量流和图像增强

> 原文：<https://towardsdatascience.com/tensorflow-and-image-augmentation-3610c6c243a2?source=collection_archive---------31----------------------->

## 是的，我知道。你已经听说过很多图像增强。但是你猜怎么着！无论如何我都想给你一个惊喜。让我们开始吧！

![](img/e56246ed920996eea32e9c609128ab3b.png)

来源:Pexels.com

A 增强是一种众所周知的防止深度神经网络过度拟合的技术。如果你做得好，你可以增加你的数据集，而不需要收集和注释新数据的高成本。Python 中有很多这方面的库，但我想主要谈谈 TensorFlow。

# *快速概览*

为了获得最佳性能，应该考虑使用 [*tf.data*](https://www.tensorflow.org/guide/data_performance) API。第一眼看上去很好看。使用简单，有效。过了一会儿，你开始想知道图像放大。我如何把它们放进去？

如果你习惯了像*[*imgaug*](https://imgaug.readthedocs.io/en/latest/)，[*albuminations*](https://github.com/albumentations-team/albumentations)*甚至 [*OpenCV*](https://opencv.org/) *，*你需要一个[*TF . py _ function*](https://www.tensorflow.org/api_docs/python/tf/py_function)的帮助。但是一些纯张量流解呢？有些可以编译成图表运行。**

## **tf .图像**

**[*这个*](https://www.tensorflow.org/api_docs/python/tf/image) 是你可能会看的第一个地方。至少我做到了。有一些非常有用的函数，例如:**

*   **图像调整—亮度、对比度、饱和度、JPEG 质量等。**
*   **裁剪。**
*   **翻转、旋转和换位—向左/向右翻转、向上/向下翻转、旋转 90 度。不幸的是不是完整的旋转。**

## **tfa .图像**

**[*图片*](https://www.tensorflow.org/addons/api_docs/python/tfa/image) 包来自[*tensor flow Addons*](https://www.tensorflow.org/addons)是另一个你应该定期检查的包。不仅仅是增强，还有附加层、损耗、优化器等等。但是现在回到赛道上。我们有什么？主要是旋转、剪切和一般的平移和变换。**

## **图像检测？**

**不幸的是，这两个人都对图像检测不感兴趣。是的，你可以翻转图像。但是选择的边界框呢？！我猜你不想失去他们！**

**![](img/ee4bb7382c75b728e74c610e255de003.png)**

**来源:Pexels.com**

# **我们的图书馆— tf-image**

**如果你渴望自己查库，请访问我们的 [*GitHub 资源库*](https://github.com/Ximilar-com/tf-image) *。*全部是开源的。我们只是刚刚开始，所以我们欢迎任何关于你喜欢(不喜欢)或你需要什么的反馈。如果你觉得它有用，我们将非常感谢你的代码贡献！**

## ****不同的工作流程****

**这篇博文旨在介绍这个库，而不是完整的手册或对每个功能的描述。因此，我将仅限于举两个例子。**

## **多合一**

**为了使增强尽可能简单，有 **random_augmentations** 函数。您提供图像、增强设置和可选的边界框。剩下的就是函数本身了。此外，增强是以随机顺序执行的，以使该过程更加强大。**

**很酷，但在某些方面有局限性。主要是这个设置太简单了。每个选项都有许多参数。将来我们会推出更先进的设置。但是现在，您可以使用我们库的所有其他部分轻松创建自己的增强功能。**

## **使用核心功能**

**使用核心函数也很容易。为了向您展示 random _ function(random _ function _ bbox es)，我在下面演示了一个例子。如果你不想每次都应用你的增强，这是一个值得一看的地方。**

# **结论**

**今天到此为止。如果你已经走了这么远，谢谢你。请，评论，批评，分享，发送合并请求！**

**我要感谢 [*Michal Lukac*](https://medium.com/@michallukac) 与我在这个项目上的合作，感谢 [*Ximilar*](https://www.ximilar.com/) 给了我这个机会和一个积极的方法来开源我们的一些工作。**