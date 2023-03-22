# Tableau 给数据科学家带来的好处

> 原文：<https://towardsdatascience.com/benefits-of-tableau-for-data-scientists-fe936a2ae799?source=collection_archive---------26----------------------->

## Tableau 在有编程和没有编程的情况下如何有用？

![](img/b31773326cc9535252a87e0b43db2792.png)

照片由[凯特琳·贝克](https://unsplash.com/@kaitlynbaker?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)在[Unsplash](https://unsplash.com/s/photos/person-working-technology?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)【1】上拍摄。

# 目录

1.  介绍
2.  快速简单
3.  SQL、R、Python 和 MATLAB
4.  k 均值算法
5.  跨职能部门
6.  摘要
7.  参考

# 介绍

[Tableau](https://www.tableau.com/)【2】对数据科学家来说越来越有用，因为它包含了更精细的功能。如果你不熟悉 Tableau，它本质上是一个工具，被几种不同类型的人在各自的职业生涯中广泛使用。您可以将数据可视化并与其他人共享，这样您就可以看到它不仅对数据科学家有好处，而且对产品经理、SQL 开发人员、数据分析师、商业智能分析师等等也有好处——这样的例子不胜枚举。但是，真正的问题是，是什么让这个工具对数据科学家特别有用？下面，我将讨论数据科学家可以获得的一些一般好处，以及一些更具轮廓的数据科学工具和利用 Tableau 的好处。

# 快速简单

作为一个更广泛的好处，Tableau 可以很快和简单地使用。如果您准备好了 CSV 文件，您可以在几分钟内创建一个仪表板。这个仪表板将包括几个不同的可视化表单，无论是地图、图表还是表格(*等。*)。实际上，您可以将字段拖放到工作表中，然后选择一个可视化效果——就这么简单。现在，让我们来谈谈如何将一些编程语言融入其中。

# SQL、R、Python 和 MATLAB

您可以选择静态 CSV 文件或通过 SQL 数据库的实时连接，在 SQL 数据库中，您更新相同的查询以更新您的数据及其各自的表、仪表板和与之连接的故事。

作为一名数据科学家，你将熟悉 SQL，或结构化查询语言。为了开发数据科学模型的数据集，您需要使用 SQL ( *最有可能是*)查询数据库。Tableau 使连接到您当前的 SQL 数据库变得容易，这样您就可以在 Tableau 中执行查询，然后从那里开发报告。对于数据科学，当您需要执行以下操作时，此功能会很有用:

*   可视化探索性数据分析
*   可视化模型指标

> 集成

— R 集成

您可以导入 R 包以及它们相关的库。更强大的是，您还可以将保存的数据模型导入 Tableau。

— Python

也被称为 ***TabPy*** ，Tableau 的这种集成允许用户使用一个可以远程执行 Python 代码的框架。一些主要用途是用于数据清理和预测算法(以及计算字段的使用)。*这里有一个对 tabby[3]有用的链接:*

[](https://github.com/tableau/TabPy) [## 表格/标签

### 动态执行 Python 代码，并在 Tableau 可视化中显示结果:-Tableau/tabby

github.com](https://github.com/tableau/TabPy) 

— MATLAB

对于从 MATLAB 部署模型，您可以利用这种集成。它包括在预测洞察力中的使用，以及预处理数据。

当然，所有这些集成和语言都可以作为数据科学过程的一部分用于数据分析。此外，您从预测模型中得出的结果可以显示在 Tableau 中。

# k 均值算法

你可以使用 Tableau 本身的机器学习算法。这种聚类算法也被称为 *k-means* 。其思想是通过将相似的数据从其特征中组合在一起来发现数据中的模式。您还需要确保这些相同的组不同于其他组。这种分组的技术术语包括**组内平方和** (WGSS)和**组间平方和** (BGSS)。在 Tableau 中，您所要做的就是加载数据，然后选择列和行，同时隔离您想要构建集群的变量。您可以根据指标自动创建集群，也可以手动强制集群数量。

由于 Tableau 侧重于可视化，您的集群将被很好地标记、交互和着色，以便于查看和理解。

> 在 Tableau 中使用这个流行的算法的好处是，它执行起来相当快，并且不需要您编写任何代码。

*这里有一个链接，更详细地描述了 Tableau 中的聚类[4]:*

[](https://help.tableau.com/current/pro/desktop/en-us/clustering.htm) [## 在数据中查找聚类

### 聚类分析将视图中的标记划分为多个聚类，每个聚类中的标记更类似于…

help.tableau.com](https://help.tableau.com/current/pro/desktop/en-us/clustering.htm) 

# 跨职能部门

![](img/f083856003d735799907d60af5f66f75.png)

[活动创建者](https://unsplash.com/@campaign_creators?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)在[Unsplash](https://unsplash.com/s/photos/computer-presentation?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)【5】上的照片。

对于数据科学家来说，Tableau 最重要的好处可能是您可以将该工具用于其主要功能——可视化地共享数据。作为一名数据科学家，你可能会在公司内部不同类型的部门遇到不同类型的人。你的工作是向他人解释你复杂的机器学习算法及其各自的结果。最好的方法之一就是把它形象化。您可以使用任何图表来描述您的模型结果—您是否对业务进行了改进，哪些数据组表现更好，等等。Tableau 在数据科学中的用例数不胜数。以下是 Tableau 在数据科学方面的一些优势。

*   *很多人使用 Tableau，所以分享可视化效果会很容易*
*   *很多公司，所以了解一下总体情况很好*
*   *你可以把复杂的模型变成易读的视觉效果*

# 摘要

正如您所看到的，Tableau 对于许多不同的角色有几个主要的好处，对于数据科学家也是如此。无论您想要简单地拖放数据以创建简单的探索性数据分析，还是在 Tableau 中从头创建模型，它都可以作为整个数据科学过程的整体工具。

总而言之，下面列出并总结了一些好处:

```
Quick and SimpleSQL, R, Python, and MATLABk-means AlgorithmCross-functional
```

*感谢您的阅读！我很感激。请随时在下面发表评论，并以任何用户或数据科学家的身份写下您对 Tableau 的体验。*

# 参考

[1]照片由[凯特琳·贝克](https://unsplash.com/@kaitlynbaker?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)在[Unsplash](https://unsplash.com/s/photos/person-working-technology?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)(2017)上拍摄

[2] TABLEAU SOFTWARE，LLC，一家 SALESFORCE 公司，TABLEAU 主页，(2003–2020)

[3] TABLEAU SOFTWARE，LLC，SALESFORCE 公司，[tabby](https://github.com/tableau/TabPy)，(2003–2020 年)

[4] TABLEAU SOFTWARE，LLC，一家 SALESFORCE 公司，[在数据中查找聚类](https://help.tableau.com/current/pro/desktop/en-us/clustering.htm)，(2003–2020)

[5]由[活动创建者](https://unsplash.com/@campaign_creators?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)在 [Unsplash](https://unsplash.com/s/photos/computer-presentation?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) 上拍摄的照片，(2018)