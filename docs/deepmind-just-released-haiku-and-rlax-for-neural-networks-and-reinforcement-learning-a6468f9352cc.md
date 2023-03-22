# DeepMind 刚刚发布了用于神经网络和强化学习的 Haiku 和 RLax

> 原文：<https://towardsdatascience.com/deepmind-just-released-haiku-and-rlax-for-neural-networks-and-reinforcement-learning-a6468f9352cc?source=collection_archive---------18----------------------->

![](img/da78bbe2c198fe08b6e6ee669edc6bfd.png)

图片由 [Gerd Altmann](https://pixabay.com/users/geralt-9301/?utm_source=link-attribution&amp;utm_medium=referral&amp;utm_campaign=image&amp;utm_content=3382507) 从 [Pixabay](https://pixabay.com/) 拍摄

## 利用最新的 JAX 图书馆来促进你的人工智能项目

人工智能(AI)研究每天都在发展，其工具也是如此——尖端人工智能公司 DeepMind 刚刚发布了两个基于 **JAX** 的神经网络(NN)和强化学习(RL)库，分别是**俳句**和 **RLax** 、。由于两者都是开源的，我们可以利用这些最新的库来进行我们自己的人工智能工作。

## JAX 是什么？

**JAX** 由谷歌于 2018 年发布，是一款开源工具，通过转化 Python 和 NumPy 程序生成高性能加速器代码。在幕后， **JAX** 高度集成了**亲笔签名**，可以有效区分原生 Python 和 NumPy 函数， **XLA** ，是一个线性代数编译器，可以优化 **TensorFlow** 模型。

因此， **JAX** 通过提供学习模型优化的集成系统，为高性能机器学习研究而构建。它在人工智能研究人员中越来越受欢迎。然而，这并不是最容易处理的事情，这可能导致 DeepMind 开发了**俳句**和 **RLax** 。

## 什么是俳句和 RLax？

**俳句**是为 **JAX** 准备的简单 NN 库，这个库是由**十四行诗**的一些作者开发的——为**张量流**准备的神经网络库。这个库通过提供面向对象的编程模型，允许人工智能研究人员完全访问 **JAX** 的纯函数转换。通过提供两个核心工具— `hk.Module`和`hk.transform`，Haiku 被设计用来管理 NN 模型参数和其他模型状态。

RLax 是为 **JAX** 设计的简单 RL 库。这个库没有为 RL 提供完整的算法，而是为实现特定的数学运算以构建全功能的 RL 代理提供了有用的构建块。RL 中的代理是学习与嵌入式环境交互的学习系统。通常，智能体与环境的交互有一些离散的步骤，包括智能体的*动作*，被称为*观察*的环境更新状态，以及被称为*奖励*的可量化反馈信号。RLax 专门用于改进代理如何更好地与环境交互，从而使 RL 更加有效。

## 为什么是他们？

由于这两个库都是开源的，开发人员可能会考虑在自己的项目中使用它们。当然，在生产项目中使用它们可能还为时过早，但是有两个主要因素可以证明我们应该开始试验它们。

*   *整体质量高*。这两个库都由 DeepMind 支持，并且已经在合理的规模上进行了测试。例如，**俳句**的作者也在开发 Sonnet，一个著名的**tensor flow**NN 库。
*   *用户友好。*两个库都执行一组定义良好的易用操作。对于俳句来说，它的 API 和抽象非常接近于十四行诗，使得过渡到俳句变得不那么痛苦。如果您不熟悉 Sonnet，您仍然可以相对快速地学会使用它，因为它提供了面向对象的编程模型。

## 结论

这两个库都将通过在 **JAX** 保护伞下提供一个伟大的集成工具来促进 NN 和 ML 模型的开发。如果你喜欢尝试新事物，我肯定会推荐你考虑使用这些库，至少可能是为了实验的目的。

## 有用的链接

[GitHub JAX](https://github.com/google/jax)

[GitHub 俳句](https://github.com/deepmind/dm-haiku)

[GitHub RLax](https://github.com/deepmind/rlax)