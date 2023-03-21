# 在你第一份工作之前获得现实生活数据科学经验的 3 种方法

> 原文：<https://towardsdatascience.com/3-ways-to-get-real-life-data-science-experience-before-your-first-job-545db436ef12?source=collection_archive---------2----------------------->

## 如何通过实际项目发展您的数据科学技能

![](img/bd4ab74a39aefc678eee8f6596645eb9.png)

由故事创建的信息图向量—[www.freepik.com](http://www.freepik.com)

## 介绍

> 获得我的第一份数据科学工作很难。

当公司通常要求硕士学位和至少 2-3 年的经验时，进入数据科学领域尤其困难。也就是说，我发现了一些很棒的资源，想和大家分享一下。

在本文中，我将向您介绍三种可以让您自己获得实际数据科学经验的方法。通过完成这些项目，你将对 **SQL** 、**熊猫**和**机器学习建模**有一个深刻的理解。

1.  首先，我将为您提供真实的 SQL 案例研究，在这些案例中，您遇到了一个业务问题，需要查询数据库来诊断问题并制定解决方案。
2.  其次，我将为你提供几十个熊猫练习题，熊猫是 Python 中的一个库，用于数据操作和分析。这将有助于您发展数据争论和数据清理所需的技能。
3.  最后，我将为您提供各种机器学习问题，您可以开发一个机器学习模型来进行预测。通过这样做，您将学习如何处理机器学习问题，以及从头到尾开发机器学习模型所需的基本步骤。

说了这么多，让我们开始吧！

# 1.SQL 案例研究

如果你想成为一名数据科学家，你必须有很强的 SQL 技能。Mode 提供了三个模拟真实业务问题的实用 SQL 案例研究，以及一个在线 SQL 编辑器，您可以在其中编写和运行查询。

***要打开 Mode 的 SQL 编辑器，请转到*** [***此链接***](https://mode.com/sql-tutorial/intro-to-intermediate-sql/) ***并单击显示“打开另一个 Mode 窗口”的超链接。***

## 学习 SQL

如果您是 SQL 新手，我会首先从 [Mode 的 SQL 教程](https://mode.com/sql-tutorial/introduction-to-sql/)开始，在那里您可以学习基本、中级和高级 SQL 技术。如果您已经对 SQL 有了很好的理解，可以跳过这一步。

## **案例研究 1:调查用户参与度的下降**

[**链接到案例**](https://mode.com/sql-tutorial/a-drop-in-user-engagement/) **。**

本案例的目的是确定 Yammer 项目用户参与度下降的原因。在深入研究这些数据之前，您应该先阅读一下 Yammer 的概述[这里是](https://mode.com/sql-tutorial/sql-business-analytics-training/)。您应该使用 4 张表。

到案例的链接将为您提供关于问题、数据和应该回答的问题的更多细节。

如果你需要指导，请点击这里的[查看我是如何完成这个案例研究的。](/sql-case-study-investigating-a-drop-in-user-engagement-510b27d0cbcc?source=friends_link&sk=49cdc679e66cae75257b955db51f4fe5)

## **案例研究 2:了解搜索功能**

[**链接到案例**](https://mode.com/sql-tutorial/understanding-search-functionality/) 。

这个案例更侧重于产品分析。在这里，您需要深入研究数据，确定用户体验是好是坏。这个案例的有趣之处在于，由你来决定什么是“好”和“坏”以及如何评估用户体验。

## **案例研究 3:验证 A/B 测试结果**

[**链接到案例**](https://mode.com/sql-tutorial/validating-ab-test-results/) **。**

最实际的数据科学应用之一是执行 A/B 测试。在本案例研究中，您将深入研究 A/B 测试的结果，其中对照组和治疗组之间存在 50%的差异。在这种情况下，您的任务是在彻底分析后验证或否定结果。

# 2.熊猫练习题

当我第一次开始开发机器学习模型时，我发现我缺乏熊猫技能是我所能做的一个很大的限制。不幸的是，互联网上没有多少资源可以让你练习熊猫技能，不像 Python 和 SQL…

然而，几周前，我偶然发现了这个资源[](https://github.com/guipsamora/pandas_exercises)****——这是一个专门为熊猫准备的练习题资源库。通过完成这些练习题，您将知道如何:****

*   ****过滤和排序您的数据****
*   ****分组和聚合数据****
*   ****使用。apply()操作数据****
*   ****合并数据集****
*   ****还有更多。****

****如果你能完成这些练习题，你应该可以自信地说，你知道如何使用熊猫进行数据科学项目。这也将对你下一节有很大的帮助。****

# ****3.机器学习建模****

****获得数据科学经验的最佳方式之一是创建自己的机器学习模型。这意味着找到一个公共数据集，定义一个问题，用机器学习解决问题。****

****Kaggle 是世界上最大的数据科学社区之一，有数百个数据集可供选择。以下是一些你可以用来开始的想法。****

## ****预测葡萄酒质量****

******数据集**[此处](https://www.kaggle.com/uciml/red-wine-quality-cortez-et-al-2009) **。******

****![](img/d64814c68e0a707f67e9517ff17b3f92.png)****

****特里·维利迪斯在 [Unsplash](https://unsplash.com/s/photos/red-wine?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) 上拍摄的照片****

****该数据集包含各种葡萄酒、其成分和葡萄酒质量的数据。这可能是一个回归或分类问题，取决于你如何设计它。看看你能否预测给定 11 个输入(固定酸度、挥发性酸度、柠檬酸、残糖、氯化物、游离二氧化硫、总二氧化硫、密度、pH、硫酸盐和酒精)的红酒质量。****

*****如果你想要一些为这个数据集创建机器学习模型的指导，请查看我的方法* [*这里*](/predicting-wine-quality-with-several-classification-techniques-179038ea6434) *。*****

## ****二手车价格评估员****

******数据集** [**此处**](https://www.kaggle.com/austinreese/craigslist-carstrucks-data) **。******

****![](img/c0615d8ef1c1eecaac2d0db06d89b5ed.png)****

****[帕克·吉布斯](https://unsplash.com/@parkergibbsmccullough?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)在 [Unsplash](https://unsplash.com/s/photos/used-car?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) 上拍摄的照片****

****Craigslist 是世界上最大的二手车销售网站。这个数据集由 Craigslist 搜集的数据组成，每隔几个月更新一次。使用这个数据集，看看是否可以创建一个数据集来预测一个汽车列表是过高还是过低。****

# ****感谢阅读！****

****我希望这些资源和想法对您的数据科学之旅有所帮助。:)****

## ****特伦斯·申****

*******如果你喜欢这样，你应该每周查看一下*** [***我的免费数据科学资源***](https://docs.google.com/document/d/1UV6pvCi9du37cYAcKNtuj-2rkCfbt7kBJieYhSRuwHw/edit#heading=h.m63uwvt9w358) ***有新素材！*******

*****创始人*[*ShinTwin*](https://shintwin.com/)*|我们连线上*[*LinkedIn*](https://www.linkedin.com/in/terenceshin/)*|项目组合这里是*[](http://terenceshin.com/)**。******