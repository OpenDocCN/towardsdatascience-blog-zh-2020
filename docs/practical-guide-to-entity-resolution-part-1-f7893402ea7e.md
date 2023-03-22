# 实体解析实用指南—第 1 部分

> 原文：<https://towardsdatascience.com/practical-guide-to-entity-resolution-part-1-f7893402ea7e?source=collection_archive---------13----------------------->

## [理解大数据](https://towardsdatascience.com/tagged/making-sense-of-big-data)

## *这是关于实体解析的小型系列文章的第 1 部分*

![](img/9737c7f9ca36734b5142611893782f7a.png)

照片由[亨特·哈里特](https://unsplash.com/@hharritt?utm_source=medium&utm_medium=referral)在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 拍摄

**什么是实体解析(ER)？**

实体解析(ER)是在缺少连接键的情况下，在代表现实中同一事物的不同数据记录之间创建系统链接的过程。例如，假设您有一个在 Amazon 上销售的产品数据集，和另一个在 Google 上销售的产品数据集。它们可能有稍微不同的名称、稍微不同的描述、可能相似的价格和完全不同的唯一标识符。人们可能会查看这两个来源的记录，并确定它们是否涉及相同的产品，但这并不适用于所有记录。我们需要查看的潜在对的数量是 x G，其中 A 是亚马逊数据集中的记录数量，G 是谷歌数据集中的记录数量。如果每个数据集中有 1000 条记录，那么就有 100 万对要检查，而这只是针对 2 个相当小的数据集。随着数据集越来越大，复杂性会迅速增加。实体解析是解决这一挑战的算法，并以系统和可扩展的方式导出规范实体。

**我为什么要关心 ER？**

从数据中释放价值通常依赖于数据集成。通过整合关于感兴趣的实体的不同数据片段而创建的整体数据图，在为决策提供信息方面比部分或不完整的视图有效得多。例如，如果汽车制造商的保修分析师能够通过运行过程中产生的传感器读数，从供应链开始，到装配到车辆中，全面了解与相关零件相关的所有数据，她就可以更有效地诊断索赔。类似地，如果风险投资者对潜在公司的所有数据有一个综合的看法，从团队的质量到其在市场上的吸引力和围绕它的传言，她可以更有效地选择最佳投资。执法、反恐、反洗钱等众多工作流也需要集成的数据资产。然而，数据(尤其是第三方数据)的不幸现实是，它通常是特定应用程序或流程的耗尽物，并且它从未被设计为与其他数据集结合使用来为决策提供信息。因此，包含关于同一现实世界实体的不同方面信息的数据集通常存储在不同的位置，具有不同的形状，并且没有一致的连接键。这就是 ER 特别有用的地方，它可以系统地集成不同的数据源，并提供感兴趣的实体的整体视图。

我怎么呃？

概括来说，ER 有 5 个核心步骤:

1.  [源规范化](https://yifei-huang.medium.com/practical-guide-to-entity-resolution-part-2-ab6e42572405) —清理数据并将不同的源协调到一个通用的要素(列)模式中，该模式将用于评估潜在的匹配
2.  [特征化和块密钥生成](https://yifei-huang.medium.com/practical-guide-to-entity-resolution-part-3-1b2c262f50a7) —为块密钥创建特征，块密钥是可能在匹配记录之间共享的目标令牌，以将搜索空间从 N 限制为在计算上更易于管理的空间
3.  [生成候选对](https://yifei-huang.medium.com/practical-guide-to-entity-resolution-part-4-299ac89b9415)——使用阻塞连接键创建潜在的候选对。这本质上是块键上的自连接，通常使用图形数据结构来实现，单个记录作为节点，潜在的匹配作为边
4.  [匹配评分和迭代](https://yifei-huang.medium.com/practical-guide-to-entity-resolution-part-5-5ecdd0005470) —通过匹配评分功能决定哪些候选对实际匹配。这可以是基于规则的，但是通常更好地实现为学习算法，其可以在非线性决策边界上适应和优化
5.  [生成实体](https://yifei-huang.medium.com/practical-guide-to-entity-resolution-part-6-e5d969e72d89) —消除图形中不匹配的边，并生成已解析的实体和到各个源记录的关联映射

在接下来的几篇文章中，我将深入研究上面的每一个步骤，通过一个具体的例子，提供深入的解释，以及如何大规模实现它们的 PySpark 代码示例。

希望这是一次有益的讨论。如果您有任何意见或问题，请随时联系我们。[Twitter](https://twitter.com/yifei_huang)|[Linkedin](https://www.linkedin.com/in/yifeihuangdatascience/)|[Medium](https://yifei-huang.medium.com/)

查看第 2 部分[源标准化](/practical-guide-to-entity-resolution-part-2-ab6e42572405)