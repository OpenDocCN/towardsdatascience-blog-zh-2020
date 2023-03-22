# 数据库可靠性工程初学者指南

> 原文：<https://towardsdatascience.com/a-beginners-guide-to-data-reliability-engineering-8bfd9b9fcaf6?source=collection_archive---------17----------------------->

![](img/1b956c8e0d80a05aa58fadb9d4711e08.png)

克林特·帕特森在 [Unsplash](https://unsplash.com/s/photos/monitor?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) 上拍摄的照片

## 数据工程

## 当站点可靠性工程遇到数据工程时

## 背景

这不是新闻。随着处理、存储和分析大量数据的高度可扩展和高度可用的系统的发展，一个新的工程分支已经诞生。它被称为数据库可靠性工程。它明显源于[谷歌新的开发方法，叫做站点可靠性工程](https://landing.google.com/sre/)。已经有很多关于 SRE 和德国有什么不同的文章发表了。理解为什么会这样很重要。

数据库可靠性工程是 SRE 的一个子领域。就像 SRE 全面处理公司所有系统的可靠性一样，DRE 处理公司所有数据基础设施的系统。想到的第一个问题是，为什么需要这种分离？这是一个合理的问题。在过去的十年中，数据工程本身已经发展成为一个巨大的领域。工具、技术和流程的多样性是巨大的。

[](/complete-data-engineers-vocabulary-87967e374fad) [## 完整的数据工程师词汇

### 数据工程师必须知道的 10 个单词以内的概念

towardsdatascience.com](/complete-data-engineers-vocabulary-87967e374fad) 

## 数据库可靠性工程

在 SRE 的框架内，数据库可靠性已经成为一项独立的专业。然后，DRE 成为负责数据基础设施(可靠性)的人，包括数据管道、数据库、部署、CI/CD、HA、访问和权限(在某些情况下)、数据仓库、存储和归档。简而言之，DRE 负责数据工程师正常工作所需的所有基础设施。同样值得注意的是—

> 数据可靠性工程对于数据库管理来说并不是一个花哨的词。

数据库管理是一项高度专业化的工作。DBA 的大部分工作已经被 DevOps 或 DRE 占用了。如前所述，数据库可靠性工程师不仅负责数据库，还负责整个数据基础设施。然而，一些公司利用 DBRE 的职位来具体管理数据库的可靠性。我会给你介绍一家法国家居装修技术公司，它帮助人们做一些 DIY 的事情，描述了数据库可靠性工程师的角色

> *数据库可靠性工程师负责保持支持所有面向用户的服务的数据库系统和许多其他生产系统全天候平稳运行。dre 是数据库工程和管理部门主管和软件开发人员的混合体，他们应用合理的工程原理、操作规程和成熟的软件开发和自动化，专攻数据库。*

## DRE 的角色和职责

DRE 的职责范围从[为数据建模提供建议，到确保数据库在时机成熟时做好伸缩准备](https://jobs.smartrecruiters.com/FireEyeInc1/743999684961418-senior-database-reliability-engineer-dbre-remote-)。DRE 使用的工具和技术非常相似，在大多数情况下，SRE 使用的工具和技术如 IaC (Terraform，Pulumi，CloudFormation)，CI/CD (Jenkins)，配置管理(Ansible，Chef，Puppet)等等。除此之外，数据库可靠性工程师还包括数据分析师、数据工程师、数据库工程师、开发人员等。有时建模数据、编写&优化查询、分析日志&访问模式、安全性&合规性、管理复制拓扑、磁盘空间、警报、监控、计费等等。

许多公司已经开始意识到并在其不断增长的工程团队中开放数据库可靠性工程师的职位。我在 [Vimeo](https://angel.co/company/vimeo/jobs/1012153-data-reliability-engineer) ， [Twitch](https://lensa.com/data-reliability-engineer-jobs/san-francisco/jd/d08fff9fe2f9ff57f30f9251b38f2270) ， [Pinterest](https://medium.com/pinterest-engineering/scalable-and-reliable-data-ingestion-at-pinterest-b921c2ee8754) ， [Okta](https://startup.jobs/database-reliability-engineer-senior-staff-principal-at-okta) ， [DataDog](https://www.datadoghq.com/careers/detail/?gh_jid=1826111) ， [Fastly](https://remotejobs.world/job/25856/senior-data-reliability-engineer-data-engineering-united-states-at-fastly) ， [Box](https://startup.jobs/database-sre-manager-at-boxhq) ， [ActiveCampaign](https://startup.jobs/database-reliability-engineer-at-activecampaign) ， [Mambu](https://startup.jobs/database-reliability-engineer-at-mambu_com-2) ， [Peloton](https://startup.jobs/senior-database-reliability-engineer-at-peloton-2) ， [InVision](https://startup.jobs/lead-database-reliability-engineer-at-invision) ，[air 分析了其中的一些职位简介我发现许多公司在 DBA 工作中包括数据建模方面，而其他公司则没有。就像任何其他工作一样，这取决于公司决定如何定义数据库可靠性工程师。](https://startup.jobs/software-engineer-database-reliability-at-airbnb-4)

基于[我的经验和科技行业的趋势](https://linktr.ee/kovid)，我按照共性递减的顺序列出了数据库可靠性工程师最常见的职责

*   数据库的高可用性和高可扩展性—包括数据仓库和数据湖
*   数据库内部性能调优、优化和最佳实践建议方面的专业知识
*   数据管道、大数据处理系统(MPP)、分布式系统、存储和归档的基础设施可靠性
*   跨数据基础架构的安全性和法规遵从性，包括上述所有领域以及更多领域

所有这些职责都需要在多个云平台的数据工程服务方面具备一定水平的专业知识。

## 数据操作与数据库可靠性工程

数据操作是另一个经常与 DRE 混淆的领域。适用于德沃普斯& SRE 公司的类比同样适用于这里。DRE 负责数据系统的工作，而数据运营人员则通过提供基础设施、授予使用基础设施的权限来帮助数据工程师。

如前所述，在许多公司，将这些工作分成不同工作的界限变得模糊不清。例如，在初创公司的早期阶段，可能只有一个人同时负责数据运营和 DBRE，很有可能同一个人同时担任 SRE 和 DevOps 的工程师。

## 结论

职位描述的演变是不可避免的。随着新技术的不断发展，工程师的工作将会不断变化，而且变化的速度也非常快。工作会不断变得多余。更新的技术将为更新但更少的工作腾出空间。

数据库可靠性工程作为一个概念只有大约五年的时间，仍然是一个非常年轻的职业。它会不断进化。角色和职责将不断变化，随着管理基础架构的自动化程度越来越高，角色和职责可能会减少。不过，就目前而言，这是一个很好的进入领域。一定要看看 Liane Campbell & Charity Majors 写的关于数据库可靠性工程的书，以获得对该领域的全面介绍。

[](https://www.goodreads.com/book/show/36523657-database-reliability-engineering) [## 数据库可靠性工程

### 数据库可靠性工程书籍。阅读来自世界上最大的读者社区的 5 篇评论。的…

www.goodreads.com](https://www.goodreads.com/book/show/36523657-database-reliability-engineering) 

## 资源

1.  [Laine Campbells 在 Surge 2015](https://www.youtube.com/watch?v=lsiI8AIzLrE) 上谈数据库可靠性工程
2.  [露丝·王君馨对 SRE 可汗学院的采访](https://www.youtube.com/watch?v=aLtn_nV5rHA)
3.  [git lab 的数据库可靠性工程](https://www.youtube.com/watch?v=aNxBIhsJ2nA)
4.  [Google Cloud Next 2019](https://youtu.be/VXqfbp_zE0c)+[devo PS 之间的对比& SRE](https://www.youtube.com/watch?v=uTEL8Ff1Zvk&list=PLIivdWyY5sqJrKl7D2u-gmis8h9K66qoj)
5.  [Sapient 的客户数据平台 SRE](https://medium.com/engineered-publicis-sapient/site-reliability-engineering-sre-for-customer-data-platforms-cdps-66ea0a273691)