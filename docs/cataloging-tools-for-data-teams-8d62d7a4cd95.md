# 数据团队的编目工具

> 原文：<https://towardsdatascience.com/cataloging-tools-for-data-teams-8d62d7a4cd95?source=collection_archive---------16----------------------->

![](img/8af74115bc6400cf3cff8788d5632631.png)

在 [Unsplash](https://unsplash.com/s/photos/catalog?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) 上由[伊尼基·德尔·奥尔莫](https://unsplash.com/@inakihxz?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)拍摄的照片

## 数据科学

## 介绍数据编目和数据团队可用于数据发现的主要工具

*这篇文章最后一次更新是在 2021 年 10 月 12 日。*

随着过去十年中各种数据存储和检索系统的激增，数据团队不得不处理许多数据源——所有数据源都用于特定的用例。这催生了 ETL 解决方案，它集成了高度异构的数据源、灵活、高度可伸缩的数据仓库和数据湖。由于有如此多的来源，将数据复制、转换和集成到几个目标中，数据系统变得相当复杂。也就是说，有像 Airflow 这样的工具来管理工作流编排，有工具来监控作业，等等。

> 数据发现对于数据科学家、数据分析师和业务团队来说至关重要

如果你认为将所有的数据整合到一个真实的来源，甚至多个来源就能完成任务，那么真相就离它不远了。根据一份[报告](https://www.ataccama.com/resources/4-reasons-your-data-lake-needs-a-data-catalog)，数据科学家[至少有 50%的时间](https://visit.figure-eight.com/rs/416-ZBE-142/images/CrowdFlower_DataScienceReport.pdf) **寻找**、清理和准备数据。数据科学家、数据分析师和业务团队经常发现很难找出数据的含义和来源，这两个问题在数据团队中都很常见。前者可以通过实现数据目录或数据字典来解决，后者可以通过实现数据沿袭解决方案来处理。

数据沿袭和数据编目都属于元数据管理的范畴。在本文中，我们将讨论市场上最流行、最有效的数据编目工具。我们将探索开源项目、专有软件和基于云的解决方案，这些解决方案总体上解决了数据发现、编目、沿袭和元数据管理的问题。

> 如今，数据系统高度异构，它们是多云、多源甚至多目标的。

# 开源数据目录

竞争者不多，但是活跃的竞争者做得很好，例如由一个政府组织发起并被许多人采用的 Magda T1。通过 Postgres 中的后端存储和 Elasticsearch 的搜索功能，Magda 提供了一个类似搜索引擎的界面来搜索您的数据。你可以在这里看到真实数据集的现场演示[。](https://demo.dev.magda.io)

![](img/5625bca6a2ded184b383365c6dfa6e97.png)

magda——开源数据目录

在 Magda 出现之前， [CKAN](https://ckan.org) 是主要的开源数据目录。事实上，玛格达也在引擎盖下使用 CKAN 的部分。加拿大政府和美国政府使用 CKAN 作为他们的元数据管理系统之一。

大约一年半以前，Amundsen 是这个领域的另一个有力竞争者。它已经被很多像 Workday 和 Asana 这样的大公司采用。与 Magda 不同，Amundsen 使用 neo4j 作为后端数据库来存储元数据，但使用 Elasticsearch 进行搜索。你可以在这里和[冯涛](https://medium.com/u/e7233a5d331e?source=post_page-----8d62d7a4cd95--------------------------------)的这篇博文中了解到[阿蒙森的建筑。](https://www.amundsen.io/amundsen/architecture/)

[](https://eng.lyft.com/open-sourcing-amundsen-a-data-discovery-and-metadata-platform-2282bb436234) [## 开源 Amundsen:一个数据发现和元数据平台

### 由冯涛，张金赫，塔米卡坦尼斯，丹尼尔获得

eng.lyft.com](https://eng.lyft.com/open-sourcing-amundsen-a-data-discovery-and-metadata-platform-2282bb436234) 

谈谈 Lyft 在 DataCouncil 的 Amundsen。

还有许多其他开源工具，如 LinkedIn 的 DataHub、Airbnb 的 [Dataportal](https://medium.com/airbnb-engineering/democratizing-data-at-airbnb-852d76c51770) 、网飞的 [Metacat](https://netflixtechblog.com/metacat-making-big-data-discoverable-and-meaningful-at-netflix-56fb36a53520) 、WeWork 的 [Marquez](https://marquezproject.github.io/marquez/) 。您可以在本文中找到关于这些工具的好资源。

**荣誉提名** — Spotify 尚未开源 Lexikon，但[这里有一篇有趣的文章](https://engineering.atspotify.com/2020/02/27/how-we-improved-data-discovery-for-data-scientists-at-spotify/)讲述了它如何为他们的数据科学家解决数据发现问题。

# 特定于云平台的数据目录

所有主要的云平台现在都有大量的服务可以提供。对于基于云的编排服务、数据管道和 ETL 解决方案，需要实现一个基本的数据编目组件。像 [AWS Glue Catalog](https://docs.aws.amazon.com/glue/latest/dg/components-overview.html#data-catalog-intro) 和 [Google Cloud Data Catalog](https://cloud.google.com/data-catalog) 这样的大多数解决方案都使用了下面的 [Hive Metastore](https://hive.apache.org) 。微软在 Azure 数据目录中有自己的目录实现。

[](https://cloud.google.com/data-catalog) [## 数据目录:数据发现|谷歌云

### 发送反馈 Google Cloud Next '20: OnAir |了解数据目录中的新内容，聆听贝莱德的观点，现已在以下网站发布…

cloud.google.com](https://cloud.google.com/data-catalog) 

不用说，这些工具在各自的云平台上与它们各自耦合的 web 服务配合得非常好，但是它们都具有有限的特性。它们不是为了元数据管理，而是为了确保它们有足够的数据来支持 ETL 操作、编排管道等等。可以把它们想象成您的数据库或数据仓库系统目录视图&带有一些附加信息的表。大概就是这样。

# 专有软件

考虑到商业性，许多公司已经为元数据管理构建了出色的成熟产品。 [Atlan](https://atlan.com) 、 [Ataccama](https://www.ataccama.com) 和 [Alation](https://www.alation.com/about/) 是这个市场的一些主要参与者。市场上也有许多传统的参与者 Informatica 是其中最受欢迎的，以至于它在 2019 年的 Gartner 元数据管理解决方案魔力象限中被评为该行业的领导者。

根据客户的说法，专有软件在为企业使用做好准备、强大的技术支持以及产品增强方面领先于开源软件。他们通常有一个非常吸引人的用户界面和一个没有 bug 的代码库。所有这些显然都是随着钱而来的😄

# [结论](https://linktr.ee/kovid)

最后，值得重申的是，元数据管理是数据工程领域中较少被关注的问题之一——使用适当的工具进行元数据管理和数据发现可以使数据科学家、数据分析师和公司的业务用户的生活更加轻松，工作效率更高。确保您选择了适合您需求的正确产品！