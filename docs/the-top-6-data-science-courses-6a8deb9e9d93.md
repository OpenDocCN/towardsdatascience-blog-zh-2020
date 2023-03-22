# 六大数据科学课程

> 原文：<https://towardsdatascience.com/the-top-6-data-science-courses-6a8deb9e9d93?source=collection_archive---------8----------------------->

## 去挑战卡格尔。

![](img/f062800fc3f1fd90098c02c23901ebb7.png)

马库斯·温克勒在[Unsplash](https://unsplash.com/?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)【1】上的照片。

# 目录

1.  介绍
2.  卡格尔
3.  课程
4.  摘要
5.  参考

# 介绍

在写了一篇关于为什么每个人都使用 Kaggle 的文章，并随后自己对 Kaggle 做了一些进一步的研究之后，我意识到有几门数据科学课程。有无数家公司提供在线课程，但我想具体描述 Kaggle 顶级课程的主要原因是，在学习数据科学方面(在线课程之外)，我使用 Kaggle 的次数超过了任何其他平台——比如查看代码、下载数据和查看其他 Jupyter 笔记本。

例如，LinkedIn 提供课程，但我宁愿参加我已经学习过的网站的课程。因为 Kaggle 上的例子和数据，我练习了无尽的机器学习算法和它们各自的代码。那么，为什么不信任 Kaggle 教授数据科学课程，并提高当前的数据科学知识呢？

下面，我将概述 Kaggle 上的顶级数据科学课程。

# 卡格尔

[ka ggle](https://www.kaggle.com/learn/overview)【2】是一个网站，在这里你可以了解数据科学，并查看由其他数据科学家开发的其他机器学习模型。你可以查看数百行代码，参加机器学习竞赛，从大量有用的数据集来源下载，最终提高自己作为数据科学家的水平。关于这篇文章，我将阐述 Kaggle 的数据科学课程部分。

> 令人惊讶的是，它有非常有益的课程，而且与其他常见的数据科学课程不同，你可以在几小时或几天内完成整个计划，而不是几周或几个月。

# 课程

这里再一次列出了 Kaggle 所有 14 门课程的所有主题。正如您所看到的，有些非常简单，而有些则非常独特，有助于学习数据科学和实践技能，让您成为一名与众不同的数据科学家。这些课程由先进的数据科学、机器学习和人工智能领导者指导/创建。一旦你在课程列表中继续往下，在你开始新的课程之前，会提到一些必备的技能。许多其他非 Kaggle 课程可以专注于特定的函数、列表、数组、查询技术，但这些课程专注于它们如何从头到尾与数据科学项目相关，以便您可以了解、学习和改进整个数据科学过程。这里列出了所有 14 种课程:

```
1\. Python
2\. Intro to Machine Learning
3\. Intermediate Machine Learning
4\. Data Visualization
5\. Pandas
6\. Feature Engineering
7\. Deep Learning
8\. Intro to SQL
9\. Advanced SQL
10\. Geospatial Analysis
11\. Microchallenges
12\. Machine Learning Explainability
13\. Natural Language Processing
14\. Intro to Game AI and Reinforcement Learning
```

这六大课程是:

*   **特色工程**

—本课程之所以重要，是因为大多数数据科学家在他们的职业生涯中不会收到一个完美的精选数据集来摄取到他们的模型中。它在现实世界的应用中是不可或缺的，这意味着你需要完善特征工程的艺术。本课程重点介绍基线模型、分类编码、特征生成和特征选择的过程。

*基线模型*:在基线模型部分，您将练习加载数据、准备目标列、转换时间戳、准备分类变量、创建训练、验证和测试分割、训练模型，以及进行预测和评估该模型。

*分类编码*:这部分特性工程的好处是它假设你熟悉一键编码和级别编码。因此，它提出了新的方法，以前我甚至不知道。其中包括计数编码、目标编码和 CatBoost 编码。

特征生成:现在我们将进入本课程真正精彩的部分，即生成特征。本主题涵盖了交互作用(组合分类变量)以及时间和数字特征(与本课程的具体示例相关)。

*特征选择*:拥有太多的特征会导致一个糟糕的模型，并且很难使用。本节涵盖单变量特征选择和 L1 正则化。虽然我以前知道并使用过这些方法，但我完全不知道 sklearn [3]中的 feature_selection 库包括 SelectKBest、f_classif 和 SelectFromModel。

*   **高级 SQL**

—虽然 SQL 课程没什么特别的，但我非常喜欢看其中的一些部分，它们可能是我见过的最有用的 SQL 可视化工具，以及 BigQuery 的例子[4]。

本课程涵盖的主要主题有:

```
**JOINS and UNIONS** — combining information from multiple tables**Analytic Functions** — OVER, PARTITION BY, ORDER BY, window frame clause, analytical aggregate functions, analytic navigation functions, and analytic numbering functions**Nested and Repeated Data** — STRUCT and RECORD for nested data, ARRAY and UNNEST() for repeated data**Writing Efficient Queries** — queries optimizer, show_amount_of_data_scanned(), and show_time_to_run(), selecting only the columns you want, reading less data, and avoiding N:N JOINs
```

*   **地理空间分析**

这个课程，到目前为止，可能是我见过的最好的可视化例子。在本课程中，您可以执行许多在其他程序中通常无法执行的自定义地图。本课程包括几个部分:

```
Your First Map - GeoPandasCoordinate Reference Systems - map projectionInteractive Maps - heatmaps, choropleth mapsManipulating Geospatial Data - spatial relationshipsPromoiximty Analysis - measuring distance and neighboring points
```

*   **机器学习的可解释性**

该课程概述了 SHAP 价值观，这是一个非常有用的图书馆，不仅可以帮助数据科学家向他们自己解释机器学习的结果，还可以向其他非技术利益相关者解释。以下是您将在这门独特的课程中学到的内容:

—模型洞察的用例

—排列重要性

父亲的土地

— SHAP 价值观

Shap 值的高级用途

*   **自然语言处理**

令人惊讶的是，我在职业生涯中使用数据科学的这一部分最多。总会有重要的、典型的数字数据，但文本数据几乎一样普遍。您可以将文本作为一个特征添加到几个机器学习模型中。本课程介绍自然语言处理、文本分类和词向量。虽然这个主题在数据科学领域似乎有些新，但数据科学的这个方面已经司空见惯多年了。例如，谷歌搜索引擎很可能已经应用自然语言处理(NLP)来创建搜索建议。

*   **游戏人工智能和强化学习简介**

在过去学习数据科学以及跟上当前数据科学趋势的过程中，我从未见过这样的课程。例如，也许这些课程中最有趣的部分是你可以学习如何制作一个视频游戏。本课程关注的其他主题包括:

—玩游戏:玩游戏代理

—单步前瞻:启发式和博弈树

— N 步前瞻:极大极小算法

—深度强化学习:神经网络

# 摘要

数据科学课程在网上随处可见，所以找到最好的课程可能会令人沮丧和害怕。Kaggle 上有我在其他平台上没见过的独特课程。这最终取决于你在寻找什么，但是如果你想要一个直接的、非常有益的、真实世界的数据科学课程应用，那么 Kaggle 就是你要走的路。

*我希望你喜欢我的文章。感谢您的阅读！*

*请随时关注我，这样你可以了解更多关于数据分析、机器学习和数据科学的信息！下面是一些有用的链接和参考资料。*

# 参考

*参考资料和有用链接:*

[1]Markus Winkler 在 [Unsplash](https://unsplash.com/?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) 上拍摄的照片，(2020)

[2] Kaggle 公司，[课程概述](https://www.kaggle.com/learn/overview)，(2020)

[3] sklearn。， [sklearn 主页](https://scikit-learn.org/)，(2020)

[4]谷歌，[大查询主页](https://cloud.google.com/bigquery)，(2020)