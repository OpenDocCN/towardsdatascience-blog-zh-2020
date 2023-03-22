# 用石灰建立信任:解释机器学习预测

> 原文：<https://towardsdatascience.com/building-trust-with-lime-explaining-machine-learning-predictions-55ba1fc0cb27?source=collection_archive---------64----------------------->

![](img/6bd5360d1a8534087510f97d2d3222a5.png)

Joshua Hoehne 在 Unsplash 上拍摄的照片

## [解释机器学习预测，用石灰建立信任](/explaining-machine-learning-predictions-and-building-trust-with-lime-473bf46de61a)

作者:[杨紫琼](https://medium.com/u/297181e56116?source=post_page-----55ba1fc0cb27--------------------------------) — 6 分钟阅读

不用说:机器学习是强大的。

在最基本的层面上，机器学习算法可以用来对事物进行分类。给定一组可爱的动物图片，分类器可以将图片分成“狗”和“不是狗”两类。给定关于顾客餐馆偏好的数据，分类器可以预测用户接下来去哪家餐馆。

![](img/8aca39e85ecd07c4ce79ad816f30567c.png)

## [梯度下降优化器对神经网络训练的影响](/effect-of-gradient-descent-optimizers-on-neural-net-training-d44678d27060)

由 [Daryl Chang](https://medium.com/u/5db9aa81aa19?source=post_page-----55ba1fc0cb27--------------------------------) 和 [Apurva Pathak](https://medium.com/u/c6fb3b9f100f?source=post_page-----55ba1fc0cb27--------------------------------) — 23 分钟读取

欢迎来到我们深度学习实验系列的另一部分，在这里我们进行实验来评估关于训练神经网络的常见假设。我们的目标是更好地理解影响模型训练和评估的不同设计选择。为了做到这一点，我们提出了关于每个设计选择的问题，然后运行实验来回答它们。

![](img/340b2f5a43e7baad5f2292f6cabc70ed.png)

Sean Lim 在 Unsplash 上拍摄的照片

## [实用 Cython —音乐检索:短时傅立叶变换](/practical-cython-music-retrieval-short-time-fourier-transform-f89a0e65754d)

通过 [Stefano Bosisio](https://medium.com/u/ff7141087b94?source=post_page-----55ba1fc0cb27--------------------------------) — 10 分钟阅读

我非常喜欢 Cython，因为它吸取了两个主要编程领域的精华:C 和 Python。这两种语言可以以一种简单明了的方式结合在一起，以便为您提供计算效率更高的 API 或脚本。此外，用 Cython 和 C 编写代码有助于您理解常见 python 包(如`sklearn`)下的内容，让数据科学家更进一步，这与简单的`import torch`和预定义算法用法相去甚远。

![](img/31fc56264c70cd5dc2206d5978beac4c.png)

作者形象

## [互换:soft max-加权平均池](/swap-softmax-weighted-average-pooling-70977a69791b)

肖恩·贾恩和布莱克·埃利亚斯——7 分钟阅读

我们提出了一种卷积神经网络的池化方法，作为最大池化或平均池化的替代方法。我们的方法 softmax-加权平均池(SWAP)应用平均池，但是通过每个窗口的 soft max 对输入重新加权。虽然向前传递的值与最大池化的值几乎相同，但 SWAP 的向后传递具有这样的属性，即窗口中的所有元素都接收渐变更新，而不仅仅是最大值。

![](img/60fccb915a524bbb1e1dd91ca178b775.png)

Fatos Bytyqi 在 Unsplash 上拍摄的照片

## [零到 Jupyter 与 Docker 在亚马逊网络服务上](/zero-to-jupyter-with-docker-on-amazon-web-services-26590d88b2f2)

由[约书亚·库克](https://medium.com/u/de07938a9d94?source=post_page-----55ba1fc0cb27--------------------------------) — 11 分钟阅读

AWS 是占主导地位的云服务提供商。我们不赞同他们的统治地位是使用他们服务的理由的想法。相反，我们提出了一个 AWS 解决方案，它最容易被大多数人采用。我们相信，这种方法将推广到其他基于云的产品，如 DigitalOcean 或 Google Cloud Platform，前提是读者可以通过安全外壳(ssh)访问这些系统，并且它们运行的是 Linux 版本。

![](img/313a687e03d0f3053753af9e42b97f88.png)

## [GitHub 动作介绍](/introduction-to-github-actions-7fcb30d0f959)

作者:德博拉·梅斯基塔(Déborah Mesquita)

如果您是 DevOps 和 CI/CD 世界的新手，GitHub 操作可能会有点混乱，所以在本文中，我们将探索一些特性，看看我们可以使用该工具做些什么。

从 CI/CD 的角度来看，GitHub 动作和工作流的主要目标是在每次有变化的时候测试我们的软件。这样我们可以发现错误，并在错误出现时立即纠正。