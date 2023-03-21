# 构建增量推荐系统:第二部分

> 原文：<https://towardsdatascience.com/building-an-incremental-recommender-system-part-ii-2d0e782b2cf6?source=collection_archive---------42----------------------->

## 自信地超越最先进的技术！

![](img/1387cf1b310dad82dc24ac9032ec3936.png)

在 [Unsplash](https://unsplash.com/s/photos/netflix?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) 上由 [Thibault Penin](https://unsplash.com/@thibaultpenin?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) 拍摄的照片

克里斯·安德森在他 2004 年题为《长尾理论》的文章中说，我们正在离开信息时代，进入推荐时代。**除非我们有办法过滤我们每天吸收的过量信息，只保留对我们重要的信息，否则可能的选择会变成噪音。**

在本系列的第一部分[中，我们讨论了推荐系统应该理想地适应变化。为此，我们使用 Pytorch 上构建的 python 库](/building-an-incremental-recommender-system-8836e30afaef) [CF Step](https://pypi.org/project/cf-step/) 实现了一个推荐系统，它方便了增量推荐系统的编译。

[](/building-an-incremental-recommender-system-8836e30afaef) [## 构建一个增量推荐系统

### 理想情况下，推荐系统应该能够适应发生的变化。

towardsdatascience.com](/building-an-incremental-recommender-system-8836e30afaef) 

我们将我们的模型应用于 [Movielens 1m](https://grouplens.org/datasets/movielens/) 数据集，我们能够重现相关出版物的结果；[Joao vina GRE 等人提出的用于推荐的快速增量矩阵分解，仅提供正面反馈](https://s3.amazonaws.com/academia.edu.documents/41648623/Fast_Incremental_Matrix_Factorization_fo20160127-32666-jb2771.pdf?response-content-disposition=inline%3B%20filename%3DFast_Incremental_Matrix_Factorization_fo.pdf&X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Credential=AKIAIWOWYYGZ2Y53UL3A%2F20200227%2Fus-east-1%2Fs3%2Faws4_request&X-Amz-Date=20200227T172248Z&X-Amz-Expires=3600&X-Amz-SignedHeaders=host&X-Amz-Signature=170da994bc340a019e3b20bfb688d2b9ef85e6d040dba9aa697e0c941f481cf9)。

在这个故事中，我们的目标是走得更远，获得更好的结果。以前，我们能够得到平均召回率@10 ≈ 0.05，这是论文中展示的结果。让我们看看我们能否用一个简单的技巧战胜它！

> [Learning Rate](https://mailchi.mp/d2d2d4a109b5/learning-rate-newsletter) 是一份时事通讯，面向那些对 AI 和 MLOps 世界感到好奇的人。你会在每周五收到我关于最新人工智能新闻和文章的更新和想法。订阅[这里](https://mailchi.mp/d2d2d4a109b5/learning-rate-newsletter)！

# 充满信心地建设

为了获得更好的结果，我们需要使用整个数据集。这样做有望给我们提供更多的信息，特别是在训练过程中包含负面例子的方法。在本系列的第一部分中，我们只利用了 Movielens 数据集中的正面例子。这些是用户与电影的互动，用户给电影打了 5 颗星。

> 为了在我们的指标上获得更好的性能，我们需要使用整个数据集，并专门开发一种方法，以某种方式允许我们包括负面评级。

**然而，根据我们的隐式反馈假设，我们应该有办法将表示*偏好*的评级值转换为表示*置信度*** 的数字。换句话说，我们必须用数学来表达，如果用户`u`给电影`i`打了一颗星，我们肯定用户`u`绝对讨厌电影`i`。另一方面，我们有点确信用户喜欢一部评分为 4 星的电影。三星级这个棘手的案例又是怎么回事呢？

## 置信函数

*置信度函数*只是一个试探，它将某个用户-项目交互的评级作为输入，并产生一个新的实数值。

我们传递给函数的评级可以是我们拥有的任何隐含信息。例如，它可以是消费者购买特定产品的次数、观众观看电影的时间或任何其他对问题有意义的提示。我们的工作就是设计这样一个函数。

## 损失函数

在我们在[第一部分](/building-an-incremental-recommender-system-8836e30afaef)中讨论的例子中，我们针对我们的问题优化了一个损失函数；只有正面的反馈。

![](img/1b5b563ee72449213740c5ffa5522407.png)

目标总是 1(由于仅正的假设),并且预测评级来自对应于活动用户的向量与对应于活动项目的向量的点积。

**既然我们想包含整个数据集，目标并不总是等于** `**1**` **。可以是正反馈的** `**1**` **和负反馈的** `**0**` **。因此，我们首先定义返回目标的偏好函数。**

![](img/da29ecc98de52db5c8b19cd81acb50e3.png)

因此，就像之前我们在数据集中定义一个新的*偏好*列，如果评级大于`3`则取值`1`，否则取值`0`。

最后，损失函数也应该改变以说明二进制目标。因此，我们将使用的是一个加权变量的二元交叉熵函数。

![](img/fc22ca6037ee1b92afaff1f1cdb4219a.png)

如果偏好是`1`，那么括号内等式的第一部分仍然有效，它乘以我们的置信水平。因此，如果偏好是`0`，括号内的第二部分保留下来，并乘以置信度函数的结果。

**直觉上，我们对损失函数说，如果我们对自己通过偏好函数(** `**0**` **或** `**1**` **)分配的目标非常有信心，而你弄错了，请多注意这个错误，这很重要！另一方面，如果我们对我们作为目标值传递的** `**1**` **或** `**0**` **不那么确信，无论如何都要使用它，但是如果你弄错了也不要太担心。**想象一下我们之前看到的 3 星评级的案例。我们将`0`作为目标，但是我们非常怀疑这是真的。

为此，让我们设计一个支持我们直觉的简单置信函数。

![](img/80a4c997fca2c7744da17640a042147d.png)

我们想通过这个函数传达的是，如果我们得到的评分是`1`或`5`，那么我们分别是对目标有信心的`α`分、`0`分或`1`分。然而，当我们有一个额定值`3`时，我们对我们计算为偏好的`0`没有任何信心，因此我们用`0.1α`乘以损失。数字`α`只是我们适应数据的一个超参数。现在，让我们看看所有这些措施的实施情况，并获得结果。

# 实施和评估

该实现与第一部分中的[相似，因此我们将在此强调其变化。](/building-an-incremental-recommender-system-8836e30afaef)

在这种情况下，我们希望创建 preference 列，但是使用整个数据集，而不仅仅是正面的例子。此外，我们现在认为 4 级也是正面例子。

![](img/b39a71effdb0574d62692c7fdc2ecf9a.png)

接下来，我们需要在代码中定义我们在上一节中设计的简单置信函数。

接下来，我们需要定义我们的模型，以便利用置信度函数。幸运的是，CF Step 的 API 已经接受了这样的函数。默认情况下，它定义了一个总是返回`1`的`lamda`函数，所以没有权重。现在我们将传递我们指定的函数。此外，我们将`True`传递给`SimpleCF`网络的`binary`关键字，告诉它在输出上使用 sigmoid 激活。

传递给`SimpleCF`的`a`和`b`关键字参数只是我们用于嵌入初始化的均匀分布的开始和结束限制。

其他一切与第一部分相同。为了比较这两种方法，我们将一起绘制结果。蓝线代表我们仅使用正例得到的结果，橙线显示使用置信度函数的整个数据集的结果。显然，第二种方法更优越。我们刚刚超越了出版物中呈现的结果！

![](img/c83ad64aa751157b800c65124bbdfb11.png)

# 结论

这个故事从我们在第一部分中离开的地方开始了构建一个增量推荐系统的主题。我们提出了一个置信函数的概念，它允许我们使用整个数据集并包含负面评价。我们展示了如何使用 CF Step 库，最重要的是，我们超越了[第一部分](/building-an-incremental-recommender-system-8836e30afaef)的结果。

有许多方法可以设计一个置信度函数，算法的性能显然取决于我们的选择。此外，我们还可以使用其他方法来获得更好的结果。如果我们能在下面的故事中用 NMF 学的电影嵌入，那么迁移学习呢？这留给读者作为练习，因为 CF Step 支持加载预先计算的嵌入矩阵并冻结它们，所以它们不能被进一步训练。

[](/extract-a-films-inherent-vice-from-its-plot-f43525abf0dd) [## 从电影情节中提取性本恶

### 如何使用非负矩阵分解得到不仅仅是你的电影摘要？

towardsdatascience.com](/extract-a-films-inherent-vice-from-its-plot-f43525abf0dd) 

> **我叫 Dimitris Poulopoulos，是希腊比雷埃夫斯大学***[**BigDataStack**](https://bigdatastack.eu/)***的机器学习研究员和博士(c)。我曾为欧洲委员会、欧盟统计局、国际货币基金组织、欧洲中央银行、经合组织和宜家等主要客户设计和实施人工智能和软件解决方案。如果你有兴趣阅读更多关于机器学习、深度学习和数据科学的帖子，请在 twitter 上关注我的****[](https://medium.com/@dpoulopoulos)****[**LinkedIn**](https://www.linkedin.com/in/dpoulopoulos/)**或**[**@ james2pl**](https://twitter.com/james2pl)**。********