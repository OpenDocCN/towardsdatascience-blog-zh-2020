# c 代表分类

> 原文：<https://towardsdatascience.com/c-is-for-classification-37b798ca7cf1?source=collection_archive---------72----------------------->

## 分类和回归问题之间的区别的简要概述。

![](img/3911fcdc240fade03fb5241f1459d3c4.png)

Alexander Schimmeck 在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上的照片

# 什么是分类？

**分类**是两种类型的[监督机器学习任务](/explaining-supervised-learning-to-a-kid-c2236f423e0f)(即我们有一个[标记数据集](https://stackoverflow.com/a/19172720/4691538)的任务)之一，另一种是**回归**。

**需要记住的要点**:监督学习任务使用特征来预测目标，或者，用非技术术语来说，它们使用属性/特征来预测某些东西。例如，我们可以通过篮球运动员的身高、体重、年龄、脚步速度和/或其他多个方面来预测他们会得多少分或者他们是否会成为全明星。

# *那么这两者有什么区别呢？*

*   回归任务预测一个*连续的*值(即某人将得到多少分)
*   分类任务预测一个*非连续*值(即某人是否会成为全明星)

# 我如何知道使用哪种技术？

回答以下问题:

> “我的目标变量有顺序吗？”

例如，[我预测读者推荐年龄](https://github.com/educatorsRlearners/book-maturity)的项目是一个*回归*任务，因为我预测的是一个精确的**年龄**(例如，4 岁)。如果我试图确定一本书是否适合青少年，那么这将是一项分类任务，因为答案可能是“是”或“否”。

# 好吧，所以分类只针对*是/否*，对/错，猫/狗问题，对吧？

不，这些只是简单的例子😄

# 示例 1:将人员分组

想象一下这样一个场景，你每年都有一批新学生，你必须根据他们的性格特征把他们分门别类。

![](img/f469b9ce9453dac2bb9991a33df0aa63.png)

照片由[em recan ark](https://unsplash.com/@emrecan_arik?utm_source=medium&utm_medium=referral)在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄

在这种情况下，房屋没有任何类型的顺序/等级。当然，哈利肯定不想被安置在斯莱特林，分院帽显然考虑到了这一点，但这并不意味着斯莱特林更接近格兰芬多，就像 25 更接近 30 而不是 19 一样。

# 示例 2:应用标签

类似地，如果我们有一个包含菜肴成分的数据集，并试图预测原产地，我们将解决一个分类问题。为什么？因为国家名称没有数字顺序。我们可以说俄罗斯是地球上最大的国家，或者中国是人口最多的国家，但这些都是国家的属性(即土地面积和人口)，而不是国家名称的内在属性。

# 明白了！数字是回归，文字是分类

抱歉，没有。

回想一下我之前提到的[图书推荐项目](https://github.com/educatorsRlearners/book-maturity)，回答以下问题:如果我想预测一本书是否适合

*   幼儿(2-5 岁)
*   小学年龄的儿童(6-10 岁)
*   青少年(11 -12 岁)
*   年轻成年人(13-17 岁)
*   成人(18 岁以上)

我会用什么:回归还是分类？

![](img/5f0c024951826b6c73f6673a89d95609.png)

照片由[马库斯·温克勒](https://unsplash.com/@markuswinkler?utm_source=medium&utm_medium=referral)在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄

好吧，假设标签有一个清晰的**顺序**，你肯定会想把这个问题当作一个*回归*问题，把“小孩”编码为“1”，把“成人”编码为“5”。

# 结论

非常感谢您的阅读！我希望这个对分类和回归任务之间的关键区别的简要介绍已经澄清了你的问题和/或巩固了你已经知道的东西。

最后，如果你没有带走任何其他东西，我希望你永远不要忘记以下几点:

> 总是从问“我试图预测什么”开始。

为什么？因为一旦这个问题解决了，剩下的就变得简单多了；正如卡尔·荣格曾经说过的，“问对了问题，就已经解决了一半的问题。”

# 进一步阅读

[向孩子(或你的老板)解释监督学习](/explaining-supervised-learning-to-a-kid-c2236f423e0f)

*原载于 2020 年 5 月 24 日*[*https://educatorsrlearners . github . io*](https://educatorsrlearners.github.io/an-a-z-of-machine-learning/c/2020/05/24/c-is-for-classification.html)*。*