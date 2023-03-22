# 跟上数据工程:数据工程师资源指南

> 原文：<https://towardsdatascience.com/keeping-up-with-data-engineering-a-resource-guide-for-data-engineers-3e4383ddfde2?source=collection_archive---------27----------------------->

![](img/fdb5d64e981c61fb65e5540714f7be42.png)

凯文·Ku 在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上的照片

今年早些时候，在 [LinkedIn 2020 年新兴工作报告](https://business.linkedin.com/content/dam/me/business/en-us/talent-solutions/emerging-jobs-report/Emerging_Jobs_Report_U.S._FINAL.pdf)中，数据工程师的角色在美国新兴工作排名中名列第八——年招聘增长率为 33%。最近，Robert Half Technology 的薪酬指南将数据工程列为 2021 年薪酬最高的 IT 工作。[根据 Robert Half 的说法](https://www.roberthalf.com/blog/salaries-and-skills/the-13-highest-paying-it-jobs-in-2019)，数据工程人才将继续获得高于平均水平的薪酬，因为组织将严重依赖“能够将大量原始数据转化为可操作信息以进行战略制定、决策和创新的个人”

随着对数据工程师需求的增加，数据团队必须准备好合适的工具和资源，以确保该角色保持高效的工作流程，而不是像最近的[调查](https://www.ascend.io/wp-content/uploads/2020/07/ascend-data-eng-survey-infographic.pdf)所引用的那样，将大部分时间花在现有系统的维护上。

必须承认，数据工程领域正在快速发展，不断有新的开源项目和工具发布，“最佳实践”也在不断变化，随着企业数据需求的增加，数据工程师比以往任何时候都更加捉襟见肘。不仅如此，数据工程师最近亲眼目睹了数据湖和数据仓库的混合，现代仓库通过将计算与存储分开来模糊二者的区别。这种混合已经将过去的基本“ETL”管道发展成更复杂的编排，经常是从仓库中读取数据和向仓库中写入数据。此外，随着数据科学家和数据分析师同事开始实施他们的工作，数据工程师需要与这些职能部门进行更多协作，并进一步赋予这些角色更多权力。由于所有这些压力和不断的变化，很难跟上这个空间(甚至进入它！).

从播客到博客，以下是资源综述——包括新数据工程师和经验丰富的专业人士——希望了解数据工程领域的最新动态。

# **数据工程播客**

在这个领域，很难有比数据工程播客更贴切的了。每周一集，节奏很容易跟上。然而，跟上主题是另一回事。话题从公司讨论他们的数据架构和技术挑战到深入研究开源和付费产品。

我在下面强调了一些有趣的情节:

[Presto Distributed SQL Engine:](https://www.dataengineeringpodcast.com/presto-distributed-sql-episode-149/)我们已经如此习惯于聚合数据以备查询，从另一个角度考虑这种模式很有启发性——联合查询执行以查询/组合数据所在的位置。听到这种方法的利弊有助于为数据工程师的工具箱添加另一个工具。

[Jepsen:](https://www.dataengineeringpodcast.com/jepsen-distributed-systems-testing-episode-143/) 传奇的 Jepsen 项目一直在深入研究分布式系统是如何设计、构建的，或许最重要的是，它们如何处理故障情况。虽然在堆栈中比普通的日常堆栈更深，但是更熟悉我们所依赖的和正在构建的分布式系统(当我们将多个不同的系统联系在一起时)是一件好事。

[非盈利数据专家:](https://www.dataengineeringpodcast.com/non-profit-data-engineering-episode-126/)本期节目的嘉宾是一家非盈利机构的数据基础设施总监，她分享了在预算严格的情况下处理大量数据源和合规性变化是多么具有挑战性——这是当今初创公司(或本案例中的非盈利机构)的许多数据专家都可以理解和借鉴的。

订阅这个播客是每周花些时间保持领先的好方法。

# InfoQ 的大数据

也许播客不是你的风格，或者也许你渴望一周不止一次。如果你正在寻找更多的资源(多种格式，包括播客、文章、演示文稿等等)，InfoQ 的大数据部分是一个深入挖掘的好地方。该公司一直在通过创建一个由工程师和从业者组成的编辑社区来为自己命名，而不是记者，其内容反映了这一选择。大数据部分涵盖了广泛的主题，包括人工智能，机器学习和数据工程。虽然它并不完全专注于数据工程，但它是一种了解更多相邻空间的有益方式。

# 令人惊叹的大数据

我是软件社区创建 GitHub 库的趋势的忠实粉丝，GitHub 库拥有专注于特定领域的精选资源列表(又名[牛逼](https://github.com/sindresorhus/awesome)列表)。在我的 Clojure 时代， [awesome-clojure](https://github.com/razum2um/awesome-clojure) 是查找不同数据库适配器或 linters 的绝佳资源。幸运的是，大数据有一个[“太棒了”，其中包括](https://github.com/sindresorhus/awesome#big-data)[数据工程](https://github.com/igorbarinov/awesome-data-engineering#docker)和[公共数据集](https://github.com/awesomedata/awesome-public-datasets#readme)的子部分(这些似乎总是会派上用场)。在该项目大受欢迎之前，监管似乎有点严格；别担心，即使拥有一个庞大的公共数据集列表——例如，在能源领域——也是非常有价值的。

无论是以招聘增长率、业务需求还是工具集的发展来衡量，数据工程领域都在快速发展。幸运的是，可用资源保持同步。虽然很容易迷失在日复一日的项目中，但我已经能够重新燃起热情，并通过后退一步了解更新的范式/技术来跟上这个领域。有了这个资源列表，我希望你们都能找到这样做的起点！