# SQL(关系数据库)还是 NoSQL？the FAANG 系统设计面试

> 原文：<https://towardsdatascience.com/sql-relational-database-or-nosql-ace-the-faang-system-design-interview-2d17439ecb3b?source=collection_archive---------14----------------------->

![](img/5cf102024831201b84fd8884f2b89a76.png)

泰勒·维克在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上的照片

(我的[博客](https://kylelix7.github.io/SQL-(Relational-Database)-or-NoSQL-Ace-the-FAANG-System-Design-Interview/)里也有这个帖子)

许多系统需要永久数据存储来存储应用程序数据。随着近年来 NoSQL 数据库的兴起，确定 SQL 和 NoSQL 数据库的不同优势，并选择适合您的使用案例的合适数据库至关重要。当涉及到那些大型科技公司的技术系统设计面试时，能够比较 SQL 和 NoSQL 的权衡可以是一个合格候选人的良好指标。

# 无模式

当然，关键的区别在于数据的结构。如您所知，在关系数据库中，数据存储在表中。表中的每一行都有相同的一组列。这意味着数据模型建模良好。相比之下，NoSQL 数据库提供了非常灵活的数据模型。数据存储为文档或项目。每个项目可以有不同的列。例如，在 AWS DynamoDB 中，可以用分区键和排序键定义一个表。其余的属性在运行时以宽松的方式插入/更新。

# 一致性

关系数据库是 [ACID](https://en.wikipedia.org/wiki/ACID) (原子性、一致性、隔离性、持久性)兼容的。那些 SQL 数据库具有很强的一致性。而梅·NoSQL 以牺牲一致性为代价提供了非常高的性能。大多数 NoSQL 只提供最终一致性，或者有些支持强一致性，但默认提供最终一致性。

# 垂直可扩展性与水平可扩展性

由于在大量节点中管理 ACID 编译的复杂性，横向扩展 SQL 数据库的成本非常高。因此，大多数 SQL 数据库都是纵向扩展的。这意味着它需要升级单个节点的硬件。这种可伸缩性比水平可伸缩性高得多。水平可伸缩性依赖于许多低成本的商用计算机来执行密集的工作负载。大多数 NoSQL 可以水平扩展以获得更好的性能，而无需/只需很少的节点管理成本。通过将请求路由到不同的碎片，NoSQL 数据库可以处理更多的请求。

# 询问

SQL 数据库有标准的查询语言。相比之下，NoSQL 数据库有自己的查询语言。因此，将 SQL 数据库从一个供应商迁移到另一个供应商可能不需要在应用程序级别做任何更改。而改变 NoSQL 数据库从一个到另一个可能需要一些代码的变化。

# 临终遗言

如果您对数据库感兴趣，肯定会有更多的主题供您深入研究。[酸](https://en.wikipedia.org/wiki/ACID)和[上限定理](https://en.wikipedia.org/wiki/CAP_theorem)是很好的学习主题。在选择数据库时，理解基础知识并能够阅读数据库供应商提供的用户指南是至关重要的技能。要了解关于系统设计的更多信息，您可以在[探索系统设计](https://www.educative.io/courses/grokking-the-system-design-interview?aff=VEzk)和[设计数据密集型应用](https://amzn.to/3w3KfLK)中找到更多信息。

![](img/591f0ff43d4d36b3169b36f272d1abd8.png)

以前的帖子:

[系统设计面试:如何设计一个系统来处理长时间运行的作业](https://blog.usejournal.com/system-design-interview-prep-how-to-handle-long-running-job-asynchronously-with-long-polling-34d8b2a890e1)

[FAANG Ace 系统设计面试](/ace-system-design-interview-in-faang-d9e479e25bf0?source=your_stories_page---------------------------)

[我如何获得 6 个月的代码并获得 FAANG offer](/how-i-leetcode-for-6-months-and-land-a-job-at-amazon-b76bdfc79abb?source=your_stories_page---------------------------)

[这些都是帮我找到方工作的资源](/these-are-all-the-resources-that-help-me-land-a-fang-job-452341dd6bed?source=your_stories_page---------------------------)

[我关于 FAANG 访谈的帖子](https://medium.com/@fin.techology/my-posts-about-faang-interview-20e529c5f13f?source=your_stories_page---------------------------)

[我关于金融和科技的帖子](https://medium.com/@fin.techology/my-posts-about-finance-and-tech-7b7e6b2e57f4?source=your_stories_page---------------------------)

[从 CRUD web 应用开发到语音助手中的 SDE——我正在进行的机器学习之旅](https://medium.com/@fin.techology/from-crud-app-dev-to-sde-in-voice-assistant-my-ongoing-journey-to-ml-4ea11ec4966e?)

[全栈开发教程:将 AWS Lambda 无服务器服务集成到 Angular SPA 中](/full-stack-development-tutorial-integrate-aws-lambda-serverless-service-into-angular-spa-abb70bcf417f)

[全栈开发教程:用运行在 AWS Lambda 上的无服务器 REST API 提供交易数据](/full-stack-development-tutorial-serverless-rest-api-running-on-aws-lambda-a9a501f54405)

[全栈开发教程:在 Angular SPA 上可视化交易数据](/full-stack-development-tutorial-visualize-trading-data-on-angular-spa-7ec2a5749a38)

[强化学习:Q 学习简介](https://medium.com/@kyle.jinhai.li/reinforcement-learning-introduction-to-q-learning-444c951e292c)