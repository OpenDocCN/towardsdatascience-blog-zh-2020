# P 值到底是什么？

> 原文：<https://towardsdatascience.com/defining-the-p-value-for-everyone-9103130f4fc2?source=collection_archive---------35----------------------->

## 数据科学概念

## 解释 P 值

![](img/0e4f9b150404b2082cfa53998bd6f44a.png)

彼得·德·格兰迪在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄的照片

O 作为一名数据科学家，你必须学会的第一件事是 **P 值**。如果你没有统计学背景，并且希望进入数据科学领域，你会遇到 P 值的概念。你最终还会发现的另一件事是，在你可能进行的任何技术面试中，你都必须解释 P 值。p 值的重要性永远不会被低估，因为它经常被用于大多数(如果不是全部)数据科学项目。

这里我们将解释 P 值的概念以及如何解释它的值。即使您没有统计学和数学背景，在本文结束时，您也应该能够理解 P 值的概念和目的。

# P 值是多少？

![](img/dbd3fb44ff8c0e65f5e9d1a71f4b4457.png)

斯文·米克在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄的照片

让我们从解释 P 值到底是多少开始。P 值中的*“P”*表示*概率*。它所指的值是从 0 到 1 的数值。到目前为止，这是 P 值的最基本情况。

数值的计算是由一个叫做 [***的东西决定的 z 值***](https://www.statisticshowto.com/probability-and-statistics/z-score/) 。而 z-score 是从另一个计算 [***标准差***](https://www.mathsisfun.com/data/standard-deviation-formulas.html) 的公式推导出来的。这些都一起工作，以检索 P 值。

> 在这里注册一个中级会员，可以无限制地访问和支持像我这样的内容！在你的支持下，我赚了一小部分会费。谢谢！

# P 值有什么用？

![](img/72f49031ea9feb229825cfd2ef3b5cd7.png)

照片由[艾丽卡·李](https://unsplash.com/@sept_pancake?utm_source=medium&utm_medium=referral)在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄

进行统计或假设检验时，p 值用于确定结果是否具有统计显著性。这是什么意思？换句话说，这个结果是由于其他一些因素和*而不是随机的*。这个因素可能就是您第一次开始测试时所寻找的因素。

# P 值中的数字

![](img/9f01fe223edea94d0c6d9b9030412463.png)

照片由[米卡·鲍梅斯特](https://unsplash.com/@mbaumi?utm_source=medium&utm_medium=referral)在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄

当我们进行这些统计假设检验时，我们通常会寻找一个小于我们期望阈值的 P 值。该阈值通常小于 0.05%或 5%。为什么会有这些数字？因为这些数字在统计学上已经足够低了，而且通常是大多数统计学家设定他们统计显著性水平的地方。

该数字表示由于随机性或偶然性而获得结果的可能性。如果这个值小于 0.05，那么我们可以有把握地说，这个结果不是随机的。仅凭这一点可能还不足以得出结论，但这是一个开始。

# P 值的简单示例

让我们通过提供一个示例，用更多的上下文来解释 P 值:

![](img/8ba4f1b705ae9ac252f15fadf8cdfedf.png)

照片由 [Rai Vidanes](https://unsplash.com/@raividanes?utm_source=medium&utm_medium=referral) 在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄

假设你是一个面包师，刚进了一批生蜂蜜。但问题是，你以前从未吃过或听说过蜂蜜。人们告诉你，它会让你的饼干变甜，就像糖一样，但你不相信他们，你会自己尝试。所以你开始用蜂蜜做你自己的实验或测试。

首先，你把两种不同的饼干食谱放在一边。一杯加蜂蜜，一杯不加蜂蜜。然后，你混合并烘烤它们。在烤箱中烘烤后，你把它们拿出来品尝每一个，看看甜度是否有差异。你可以比没有蜂蜜的饼干更能尝到蜂蜜饼干里的甜味。然而，因为你是一个固执而传统的面包师，从不远离糖，你认为这个测试可能是侥幸的。所以你做了另一个测试，但是用了更多的饼干。

![](img/b45915bdde96f09b9813982a2fdfc838.png)

Erol Ahmed 在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄的照片

在接下来的测试中，你要烘烤 200 块饼干:100 块有蜂蜜，100 块没有蜂蜜。这些是很多饼干，但你要确保蜂蜜实际上是一种甜味剂。所以你在测试开始时简单陈述一下你的总体信念，统计学家称之为*零假设*:

> “蜂蜜**不会让这些饼干变得更甜”**

因此，另一种替代说法，也就是统计学家所说的*替代假说*:

> “蜂蜜**会让这些饼干更甜吗”**

混合、烘焙，最后品尝并给每块饼干一个甜度分数后，你开始计算 P 值:

1.  你找到了饼干甜度分数的标准偏差。
2.  你找到了 Z 值。
3.  你通过知道 Z 值来找到 P 值。

在这一切之后，你得出一个 P 值，像 0.001 这样小得可笑的值。由于 p 值远低于 0.05 的统计显著性阈值，您别无选择，只能拒绝您在开始时声明的一般陈述或无效假设。你已经得出结论，根据统计概率，蜂蜜确实使你的饼干更甜。

从现在开始，你可以考虑时不时地在你的食谱中加入蜂蜜。

# 结论

以上是一个*简单的*例子，说明了人们可能如何进行假设检验，以及他们可能如何解释实验结束时的 P 值。通过使用从面包师测试结束时得出的 P 值，他们能够以足够的统计概率确定饼干中有蜂蜜会更甜。

希望你对 P 值以及如何解读 P 值有更清晰的理解。理解它们的重要性很重要，因为它们在数据科学中无处不在，最终会在面试中出现。如果你愿意，用这篇文章来准备和理解。希望你喜欢学习数学和统计学！

[*在 Twitter 上关注我:@_Marco_Santos_*](https://twitter.com/_Marco_Santos_)