# 专注于技术深度学习的文章

> 原文：<https://towardsdatascience.com/technical-deep-learning-focused-articles-7ad3b0fe3b5a?source=collection_archive---------45----------------------->

## 发现解释卷积神经网络，算法，计算机视觉技术和更多的内部工作的文章。

# 介绍

![](img/d9b36d34e23ee79b02edc6a66968fe47.png)

照片由 [Unsplash](https://unsplash.com/s/photos/learning?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) 上的 [Dmitry Ratushny](https://unsplash.com/@ratushny?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) 拍摄

深度学习是机器学习的一部分，涉及利用人工神经网络进行计算机视觉、语音识别或自然语言处理任务。

深度学习的知识量是巨大的。尽管如此，从最简单的神经网络架构到最先进的复杂架构，仍然存在一些关键的基本概念和思想。

**这篇文章将通过收集我以前写的文章来介绍一些基本的神经网络架构、技术、思想和算法。**

# 卷积神经网络

卷积神经网络(CNN)是许多深度学习应用的基本构建模块。

通过实施精心设计的 CNN 架构，可以解决日常计算机视觉任务的解决方案，例如图像分类、对象检测、姿态估计、深度估计。

在我的学术和职业生涯中，我发现深入了解用于开发广泛使用的 CNN 架构的内部组件至关重要。

[迁移学习](https://en.wikipedia.org/wiki/Transfer_learning#:~:text=Transfer%20learning%20(TL)%20is%20a,when%20trying%20to%20recognize%20trucks.)和微调使得访问最先进的 CNN 架构变得更加容易。尽管如此，退一步回到最简单的架构和最早的深度学习研究论文，并理解我们今天所观察到的深度学习的最初构建模块是有益的。

## AlexNet

下面的文章探讨了 AlexNet 的研究论文，还包括如何为图像分类任务实现一个 AlexNet CNN 架构。

*“Alex net 赢得 ImageNet 大规模视觉识别挑战赛(ILSSVRC 2012 竞赛)后，首次在公共场合使用。正是在这次大赛上，AlexNet 展示了深度卷积神经网络可以用于解决图像分类。”*

下面文章中 AlexNet 架构的实现是使用 [Keras](https://keras.io/) 和 [TensorFlow 2.0](https://www.tensorflow.org/) 进行的。深度学习实践者非常熟悉的两个库。

[](/implementing-alexnet-cnn-architecture-using-tensorflow-2-0-and-keras-2113e090ad98) [## 使用 TensorFlow 2.0+和 Keras 实现 AlexNet CNN 架构

### 了解如何实施引发深度卷积神经网络革命的神经网络架构…

towardsdatascience.com](/implementing-alexnet-cnn-architecture-using-tensorflow-2-0-and-keras-2113e090ad98) 

下面这篇文章是介绍 AlexNet 架构的研究论文的细分。在这篇文章中，你会发现常见的机器学习术语的解释，如辍学，增强，规范化等。

除了理解神经网络架构的实现细节，对于机器学习从业者来说，了解研究人员当时所做的设计和实现决策背后的推理同样至关重要。

[](/what-alexnet-brought-to-the-world-of-deep-learning-46c7974b46fc) [## AlexNet 给深度学习世界带来了什么

### 花一分钟来了解琐碎的技术和神经网络架构，革命性的有多深…

towardsdatascience.com](/what-alexnet-brought-to-the-world-of-deep-learning-46c7974b46fc) 

## LeNet

AlexNet 于 2012 年推出。

LeNet 论文展示了利用卷积层进行字符识别的可能性。它为 AlexNet、GoogLeNet 等网络以及今天几乎所有基于卷积神经网络的架构铺平了道路。

这篇研究论文值得一读，因为它介绍了一些技术和概念，如局部感受野、子采样、权重分配。

下面的文章介绍了使用 TensorFlow 和 Keras 实现 LeNet 架构。

[](/understanding-and-implementing-lenet-5-cnn-architecture-deep-learning-a2d531ebc342) [## 理解和实现 LeNet-5 CNN 架构(深度学习)

### 在本文中，我们使用定制实现的 LeNet-5 神经网络对 MNIST 数据集进行图像分类

towardsdatascience.com](/understanding-and-implementing-lenet-5-cnn-architecture-deep-learning-a2d531ebc342) 

下面的其余文章将更详细地探讨在 [LeNet 研究论文](http://vision.stanford.edu/cs598_spring07/papers/Lecun98.pdf)中介绍的技术和概念。

[](/you-should-understand-sub-sampling-layers-within-deep-learning-b51016acd551) [## (你应该)理解深度学习中的子采样层

### 平均池、最大池、子采样、下采样，这些都是你在深度学习中会遇到的短语…

towardsdatascience.com](/you-should-understand-sub-sampling-layers-within-deep-learning-b51016acd551) [](/understanding-parameter-sharing-or-weights-replication-within-convolutional-neural-networks-cc26db7b645a) [## 理解卷积神经网络中的参数共享(或权重复制)

### 在深度学习研究中，参数共享或权重复制是一个容易被忽略的话题领域…

towardsdatascience.com](/understanding-parameter-sharing-or-weights-replication-within-convolutional-neural-networks-cc26db7b645a) [](/understand-local-receptive-fields-in-convolutional-neural-networks-f26d700be16c) [## 理解卷积神经网络中的局部感受野

### 想过为什么卷积神经网络中的所有神经元都没有连接起来吗？

towardsdatascience.com](/understand-local-receptive-fields-in-convolutional-neural-networks-f26d700be16c) 

# 技术

机器学习充斥着研究人员创造的技术和算法怪癖，这些技术和算法怪癖要么从神经网络中挤出最佳性能，要么控制某些神经网络的内部组件以实现最佳结果。

诸如辍学、批量标准化、l1/l2 正则化等技术在许多机器学习课程和课程中已经很常见。这些常见的技术在许多神经网络架构中频繁使用，因此从业者理解这些技术背后的直觉是至关重要的。

下面的文章涵盖了主要的机器/深度学习技术，包括如何使用 TensorFlow 和 Keras 实现它们。

[](/understanding-and-implementing-dropout-in-tensorflow-and-keras-a8a3a02c1bfa) [## 在 TensorFlow 和 Keras 中理解和实现辍学

### 辍学是一种常见的正规化技术，是杠杆在国家的艺术解决方案，以计算机视觉…

towardsdatascience.com](/understanding-and-implementing-dropout-in-tensorflow-and-keras-a8a3a02c1bfa) [](/how-to-implement-custom-regularization-in-tensorflow-keras-4e77be082918) [## 如何在 TensorFlow(Keras)中实现自定义正则化

### 了解如何使用 TensorFlow 和 Keras 相对轻松地实现自定义神经网络正则化技术。

towardsdatascience.com](/how-to-implement-custom-regularization-in-tensorflow-keras-4e77be082918) [](/regularization-techniques-and-their-implementation-in-tensorflow-keras-c06e7551e709) [## 正则化技术及其在 TensorFlow(Keras)中的实现

### 理解用于减轻深度神经网络中过拟合问题的传统技术。

towardsdatascience.com](/regularization-techniques-and-their-implementation-in-tensorflow-keras-c06e7551e709) [](/batch-normalization-in-neural-networks-code-d7c9b88da9f5) [## 神经网络中的批量标准化(代码)

### 通过 TensorFlow (Keras)实施

towardsdatascience.com](/batch-normalization-in-neural-networks-code-d7c9b88da9f5) [](/batch-normalization-explained-algorithm-breakdown-23d2794511c) [## 解释了神经网络中的批量标准化(算法分解)

### 理解深度神经网络中使用的一种常见转换技术

towardsdatascience.com](/batch-normalization-explained-algorithm-breakdown-23d2794511c) 

# 临时演员

深度学习领域具有压倒性的技术性，从业者很容易迷失在编程语言、库、开源项目和研究论文中。但是深度学习有一个有趣的部分，至少是它能得到的最有趣的部分。

自然风格转移(NST)是一种深度学习技术，它采用一个艺术作品的内容和另一个艺术作品的风格，并将两者结合起来，以创建两种不同艺术创作的独特外观。

更令人着迷的是风格转换技术是如何工作的。尽管 NST 的实际应用可能不会立即实现，但该算法的技术知识和创造力成分使得深度学习的这个主题值得探索。

[](/neural-style-transfer-with-tensorflow-hub-dfe003df0ea7) [## 张量流中枢神经式传递

### 我不会画画，但是机器学习可以…

towardsdatascience.com](/neural-style-transfer-with-tensorflow-hub-dfe003df0ea7) 

在这篇文章的结尾，我将对神经网络中的学习过程进行技术探索。梯度下降是许多机器学习从业者的入门话题。

*“梯度下降是一种非常常见的优化算法，很可能是许多机器学习工程师和数据科学家引入的第一种优化算法。”*

[](/understanding-gradient-descent-and-its-variants-cf0df5c45478) [## 理解梯度下降及其变体

### 简要了解机器学习模型中的学习过程是如何得到优化支持的…

towardsdatascience.com](/understanding-gradient-descent-and-its-variants-cf0df5c45478) 

# 我希望这篇文章对你有用。

要联系我或找到更多类似本文的内容，请执行以下操作:

1.  订阅我的 [**邮件列表**](https://richmond-alake.ck.page/c8e63294ee) 获取每周简讯
2.  跟我上[中型 ](https://medium.com/@richmond.alake)
3.  通过 [**LinkedIn**](https://www.linkedin.com/in/richmondalake/) 联系我