# Power BI: M 与 DAX 以及测量值与计算值列

> 原文：<https://towardsdatascience.com/power-bi-m-vs-dax-vs-measures-4c77ae270790?source=collection_archive---------5----------------------->

## 刚开始学 Power BI 时希望知道的基础知识。

当我踏上我的权力 BI 之旅时，我几乎立即被一股外来的和令人困惑的术语打了耳光，这些术语似乎都做着相似但又有些不同的事情。

这里有一种叫做 M 的语言，但也有一种叫做 DAX 的语言？计算列和度量值有什么区别？我应该在什么时候使用超级查询？

![](img/683c8a67b90ac1a9a9b35c13dfbe3093.png)

[丹尼尔·延森](https://unsplash.com/@dallehj?utm_source=medium&utm_medium=referral)在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍照

在我的旅程中，我很早就陷入了试图理解一些关键概念的困境，这些概念使 Power BI 看起来更加困难和混乱。我几乎放弃了我的商业智能抱负，爬回到我熟悉的伙伴 Excel 身边。最好是你认识的恶魔，对吧？不对！

我是如此，如此，如此高兴，以至于我一直用头撞墙来克服最初的障碍。今天我几乎每天都在用 Power BI，而且是兴高采烈地用，*甚至是兴高采烈地*，很少用 Excel。但是来到这里很难，真的很难。这比想象的要难，因为很长一段时间以来，我对一些关键的 Power BI 概念没有清晰的概念性理解。

我会尽量让事情简单，只给你一个*提示*，一个*色调*，一个*味道*让你开始。

更有经验的 Power BI 用户可能会对我简化和减少这些概念的方式感到畏缩，但是你知道吗？在这一点上，所有的细微差别和细节真的无关紧要。你需要一个开始的地方。相信我，复杂性会来的。

所以，我无畏的战士，我的学徒，我未来的力量 BI 大师，这里有一个非常基本的概述:

1.  **电量查询和 M**
2.  **DAX**
3.  **计算列和度量**
4.  **Power Query vs. DAX**

![](img/e2b36331cd47cb72f70eb76dd6c84338.png)

照片由 [NeONBRAND](https://unsplash.com/@neonbrand?utm_source=medium&utm_medium=referral) 在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄

# 电源查询和 M

这到底是什么？这是一切的开始。Power Query 是将数据导入 Power BI 的地方。m 是 Powery Query 使用的编码语言。您可以通过点击来使用 Power Query，M 中的代码将会自动生成。也可以直接用 M 写自己的代码。

我什么时候使用它？清理和格式化你的数据。你可以做诸如删除和添加列、过滤数据、改变列格式等事情。

**我一定要用 M 吗？**:简而言之，没有，你可以通过指向和点击来完成 Power Query 中的大部分事情，完全不需要使用 M。然而，了解 M 确实有助于使您的过程更加灵活，并且在将来更容易复制。例如，当使用 M 时，您可以复制并粘贴想要重用的代码，并且可以对您的步骤进行注释。

***学习*** *提示:点击* [*打开*](https://docs.microsoft.com/en-us/power-bi/transform-model/desktop-query-overview) *高级编辑器，在电力查询中做你需要做的事情。M 代码将出现在您刚刚完成的操作中。通过这种方式，您可以开始熟悉这种语言，并在尝试从头开始编写自己的 M 代码之前做一些小的调整。*

[](/power-bi-understanding-m-5a3eb9e6c2da) [## 权力 BI:理解 M

### 足以让你变得危险。酷毙了。

towardsdatascience.com](/power-bi-understanding-m-5a3eb9e6c2da) 

# 指数

**这到底是什么？** DAX 是数据分析表达式的缩写，一旦使用 Power Query/M 将您的数据提取到 Power BI 中，就可以使用它来创建度量和计算列。

我什么时候使用它？:使用 DAX 本质上就像在 Excel 中使用公式——它允许你根据你的数据进行计算。您可以使用 DAX 创建计算列或度量值。

# **计算列和度量**

这到底是什么？计算列和度量是 DAX 中编写表达式的两种方式。计算列将为表中的每一行创建一个包含值的附加列。度量是基于一个表或多个表中的多行的聚合表达式。

**什么时候应该对计算列进行度量？我很高兴你问了，因为我真的很纠结这个问题。这是一个棘手的问题，因为您通常可以使用度量值或计算列(但这并不意味着您应该这样做)。**

**关于度量和计算列要记住的事情:**

*   当你可以使用任何一个时，使用*测量*
*   当您希望将*计算列*用作筛选器或切片器时，或者如果您希望创建类别(例如，添加一个根据任期包含“新员工”、“员工”或“高级员工”的列)，通常会使用该列。
*   如果您想根据用户选择的内容进行计算(例如，计算切片器中所选部门的总收入),您*需要*使用*度量值*

***如果计算列和度量之间的区别现在还不完全有意义，那也没关系。*** 尝试创建一个度量和一个计算列，看看它们是否都符合您的要求。如果是的话，很好，用这个方法。如果只有计算列有效，就用它吧！

# **幂查询(M)还是DAX？**

这里的事情也让我感到困惑，因为似乎我在 Power Query/M 中可以做的大部分事情，在 DAX 中也可以做。*我应该在什么时候使用哪一个？*

**简答**:在可能的情况下使用 Power Query/M。*(这里很少使用的一个大例外是，如果您试图创建一个引用不同表中的列的计算列。在这种情况下，使用 DAX 创建一个计算列)*

**长回答:** [看这个](https://www.sqlbi.com/articles/comparing-dax-calculated-columns-with-power-query-computed-columns/)。

![](img/3c80c9dbdd7b96a7e831018715dd87ff.png)

照片由[陈京达](https://unsplash.com/@jingdachen?utm_source=medium&utm_medium=referral)在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄

就这样，你做到了！您通过对*幂查询、M、DAX、measures、*和*计算列*的基本概念化完成了它。如果你现在不明白所有的区别和用途，那也没关系！你已经奠定了基础，这是朝着正确方向迈出的一大步。

# 要记住的事情:

*   超级查询是一切开始引入和清理您的数据的地方。
*   M 是 Power Query *中使用的语言(不一定要直接使用 M，但是从长远来看让你的生活更轻松)。*
*   DAX 是在 Power BI 中创建计算列和度量时使用的语言。
*   如果您 ***可以在 Power Query/M 中*** 做到，您 ***应该*** *(除了当您向引用不同表中的列的表添加列时)。*
*   如果一个计算列或一个度量可用， ***使用一个度量。***

# 后续步骤:

让这些概念在你的大脑中扎根的最简单、最快的方法就是接触和练习。以下是一些很好的入门资源:

*   [SQLBI](https://www.sqlbi.com/)
*   [微软 PBI 学习](https://docs.microsoft.com/en-us/power-bi/guided-learning/?WT.mc_id=sitertzn_learntab_guidedlearning-card-powerbi)
*   [电力 BI 社区](https://community.powerbi.com/)
*   参加来自[锻炼的每周社区挑战周三](http://www.workout-wednesday.com/power-bi-challenges/)

[](https://jeagleson.medium.com/new-in-2021-learn-power-bi-with-weekly-challenges-8742db48a52) [## 2021 年新品:通过每周挑战学习 Power BI

### 最后，Power BI 用户的每周挑战。以下是我如何完成第一次每周挑战的分步指南…

jeagleson.medium.com](https://jeagleson.medium.com/new-in-2021-learn-power-bi-with-weekly-challenges-8742db48a52) 

虽然我非常鼓励参与所有精彩的资源，但没有什么比自己尝试一些事情并看看会发生什么更能帮助你了。

**继续用你的头撞墙。会变得更好。**