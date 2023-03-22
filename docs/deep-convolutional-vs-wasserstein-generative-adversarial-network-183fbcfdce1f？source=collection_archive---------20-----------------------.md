# 深度卷积 Vs Wasserstein 生成对抗网络

> 原文：<https://towardsdatascience.com/deep-convolutional-vs-wasserstein-generative-adversarial-network-183fbcfdce1f?source=collection_archive---------20----------------------->

## 了解 b/w DCGAN 和 WGAN 的差异。使用 TensorFlow 2.x 实现 WGAN

![](img/da37a34e0d6edc56c6048e00d517c978.png)

丁满·克劳斯在 [Unsplash](https://unsplash.com/s/photos/art?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) 上的照片

在本文中，我们将尝试了解两种基本 GAN 类型(即 DCGAN 和 WGAN)的区别，还将了解它们的区别以及 WGAN 在 TensorFlow 2.x 中的实现。我使用 TensorFlow 的官方教程代码 DCGAN 作为本教程的基础代码，并针对 WGAN 对其进行了修改。你可以在这里找到。

## DCGAN

1.  DCGAN 像其他 GAN 一样由两个神经网络组成，即一个生成器和一个鉴别器。
2.  生成器将随机噪声作为输入，并输出生成的假图像。
3.  鉴别器将真实和伪造图像作为输入，并输出值 b/w 0 和 1，即图像真实或伪造的置信度。
4.  DCGAN 使用二进制交叉熵作为损失函数。
5.  生成器看不到真实图像，仅通过来自鉴别器的反馈进行学习。
6.  生成器的目标是通过生成逼真的假图像来欺骗鉴别器。而鉴别器的目标是正确识别真实和伪造的图像。

## DCGAN 中的问题

DCGAN 中出现的一些问题是由于使用了二进制交叉熵损失，如下所示。

1.  **模式崩溃**:这是一个术语(在 GAN 的上下文中)，用来定义 GAN 无法生成不同的类镜像。例如:当用 MNIST 数据集训练时，GAN 可能只能生成一种类型的数，而不是所有 10 种，即它可能只生成“2”或一些其他数。你也可以举一个例子，甘只能够产生一个品种的狗，如哈士奇，但甘训练所有品种的狗。
2.  **消失梯度:**由于鉴别器的置信值是只能为 b/w 0 和 1 的单个值，并且目标是获得尽可能接近 1 的值，因此计算的梯度接近于零，并且因此，生成器不能获得太多信息并且不能学习。因此，这可能导致一个强鉴别器，这将导致一个差的发生器。

## 解决办法

上述问题的一个解决方案是使用接近推土机距离的 Wasserstein 损失(EMD 是从一个分布到另一个分布所需的努力量。在我们的情况下，我们希望使生成的图像分布等于真实的图像分布)。WGAN 利用了 Wasserstein 损耗，所以我们现在来谈谈 WGAN。

# WGAN

如上所述，WGAN 利用了 Wasserstein 损耗，因此让我们了解发生器和鉴别器的目的。

## 瓦瑟斯坦损失{ E(d(R)) — E(d(F)) }

1.  因此，我们的损失是鉴别器输出对真实图像的 b/w 期望值与鉴别器输出对所产生的伪图像的期望值之差。
2.  鉴别器的目标是最大化这个差异，而生成器的目标是最小化这个差异。
3.  ***注*** *:* ***为了使用 Wasserstein 损耗，我们的鉴别器需要是 1-L (1-Lipschitz)连续的，即梯度的范数在每个点上必须至多为 1。***

让我们来看一种加强 1-1 连续性的方法。

## 梯度惩罚

梯度惩罚用于加强 1-L 连续性，并作为鉴别器梯度的正则化添加到损失中。以下是计算梯度损失的步骤。

1.  通过**(real _ image * epsilon+fake _ image *(1—epsilon))**从真实和虚假图像计算插值图像。
2.  然后，计算鉴频器输出相对于插值图像的梯度。之后，计算梯度的范数。
3.  然后，惩罚被计算为(norm-1)的平方的**平均值，因为我们希望范数接近 1。**

## 生成器和鉴别器的目标

因此，发生器的最终目标是增加鉴频器假输出的平均值。鉴别器的目标是加权惩罚的 w 损失。

## 训练 WGAN

训练 WGAN 时，鉴别器每步训练多次，而生成器每步训练一次。

## 结论

*WGAN 导致稳定的训练，并解决了我们在 DCGAN 中面临的模式崩溃和消失梯度的问题。*

*但这一切都是有代价的，即 WGAN 在训练时比 DCGAN 慢。*

你可以在这里找到这篇文章的完整代码。敬请关注即将发布的文章，我们将在 TensorFlow 2 中实现一些有条件和可控的 gan。

所以，本文到此结束。谢谢你的阅读，希望你喜欢并且能够理解我想要解释的内容。希望你阅读我即将发表的文章。哈里奥姆…🙏

# 参考

[](https://www.coursera.org/specializations/generative-adversarial-networks-gans) [## 生成对抗网络

### 由 DeepLearning.AI 提供关于 GANs 生成对抗网络(GANs)是强大的机器学习模型…

www.coursera.org](https://www.coursera.org/specializations/generative-adversarial-networks-gans) [](https://keras.io/examples/generative/wgan_gp/) [## Keras 文档:WGAN-GP 覆盖` Model.train_step '

### 作者:A_K_Nain 创建日期:2020/05/9 最近修改时间:2020/05/9 内容简介:用 Wasserstein 实现甘…

keras.io](https://keras.io/examples/generative/wgan_gp/)