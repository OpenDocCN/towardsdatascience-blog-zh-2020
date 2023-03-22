# 实体解析实用指南—第 6 部分

> 原文：<https://towardsdatascience.com/practical-guide-to-entity-resolution-part-6-e5d969e72d89?source=collection_archive---------39----------------------->

## 生成实体

![](img/387e5d1a27566056c1e69fcf16f96525.png)

迈克尔·泽兹奇在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上的照片

*这是关于实体解析的迷你系列的最后一部分。查看* [*第一部分*](https://yifei-huang.medium.com/practical-guide-to-entity-resolution-part-1-f7893402ea7e)*[*第二部分*](https://yifei-huang.medium.com/practical-guide-to-entity-resolution-part-2-ab6e42572405)*[*第三部分*](https://yifei-huang.medium.com/practical-guide-to-entity-resolution-part-3-1b2c262f50a7)*[*第四部分*](https://yifei-huang.medium.com/practical-guide-to-entity-resolution-part-4-299ac89b9415)*[*第五部分*](https://yifei-huang.medium.com/practical-guide-to-entity-resolution-part-5-5ecdd0005470) *如果你错过了*****

**ER 的最终输出是一个数据结构，它具有每个解析实体的唯一标识符，以及唯一实体标识符和不同源系统中解析数据记录的相应标识符之间的映射。这样做相对简单**

1.  **使用调整的基于模型的评分函数来消除不匹配的候选对。这通常意味着在计分迭代之后选择匹配概率的截止阈值。基于手动检查，我们为我们的示例用例选择了 0.5，但是该值可以根据数据集、功能、模型和用例而变化**
2.  **仅使用超过匹配概率阈值的“强”边来重新生成图**
3.  **通过连接组件算法创建组件(或实体)映射**

**就是这样！我们现在有了一个映射表，将不同的数据记录连接成统一的实体。由此，通过基于对来自每个连接记录的各个元数据值进行优先排序或组合来选择或生成规范元数据(例如，名称、描述等)来创建规范实体表通常是有益的。这里的具体方法将在很大程度上取决于所需的应用程序和工作流，所以我们不会深入研究它。**

**关于实体解析的迷你系列到此结束。无论如何，这都不是一个详尽的研究，但是希望提供核心概念和实现步骤的实用概述，以帮助您开始自己的应用程序。我欢迎你的任何意见和建议。**