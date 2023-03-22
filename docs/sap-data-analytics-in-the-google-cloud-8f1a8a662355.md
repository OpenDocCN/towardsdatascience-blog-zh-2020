# 谷歌云中的 SAP 数据分析

> 原文：<https://towardsdatascience.com/sap-data-analytics-in-the-google-cloud-8f1a8a662355?source=collection_archive---------41----------------------->

## 如何将 SAP 与谷歌云平台(BigQuery、Data Studio 或 recently looker 等强大数据分析工具的提供商)相结合，以获得强大的数据分析平台和宝贵的见解。如果您对 SAP 数据的实用数据分析方法感兴趣，[这篇文章](https://medium.com/@christianlauer90/sap-data-analytics-examples-for-internal-audit-financial-tax-accounting-and-controlling-analysis-1fbc81d01eac)可能也会让您感兴趣。

![](img/92ffc9b4c1e7ac945a560ffb375e00df.png)

云是数据分析的推动者——图片由 [Alex Machado](https://unsplash.com/@alexmachado) 在 [Unsplash](https://unsplash.com/photos/80sv993lUKI) 上提供

## 体系结构

SAP HANA 已经内置了 SAP 数据服务，因此您可以轻松地将数据从 SAP 应用程序或其底层数据库导出到 BigQuery。然而，GCP 和思爱普的结合对于那些仍然生活在 ERP 世界中的公司来说也是可能的。

以下几点显示了将数据从 SAP 加载到 Google Services 的可能性:

*   使用 talend/Google data flow/等数据集成工具，通过 RFC(远程函数调用)接口提取数据。
*   编写 ABAP 程序
*   fivetran 等第三方连接器
*   使用 SAP 复制服务器

因此，可能的解决方案如下所示:

![](img/2ba1d849ca8c2b298fe7a4b979aab41b.png)

SAP 到大型查询—图片来自 Google [1]

## 实现增量负载

获取数据是第一步。之后，您将处理如何实现一个 delta 逻辑的问题，因为您可能不想通过满载定期加载所有数据。这只会带来加载时间长、没有历史数据等缺点。尤其是控制、内部审计等部门。通常基于历史数据视图构建他们的数据分析。

虽然 SAP 数据服务已经内置在 CDC 向导中，但其他数据集成方法可能需要一些额外的工作。要实现这一步，非常了解 SAP 表和表结构是很重要的。在自己的逻辑上实现增量加载尤其可以借助两个表来实现: **CDHDR** (改变标题)和 **CDPOS** (改变位置)。这些表跟踪主数据或事务数据的变化。

## 数据转换

随着基于列的数据仓库工具(如 BigQuery)的出现，由于性能以及源系统和目标系统之间不同的表结构或数据类型，数据转换通常是必要的。

一个例子是，在 SAP 中，你有 BKPF(会计凭证标题)和 BSEG(会计凭证段)表。由于其基于列的结构，对 BigQuery 中的数据进行**反规范化**是有意义的。解决方案可以是连接 BKPF 和 BSEG，如果需要，还可以连接其他主数据和参考表——例如借助 BigQuery DTS(数据传输服务)中的内置功能。因此，您可以得到一个非规范化的数据对象体系结构，如下图所示:

![](img/d132d2e004ccae4078a6503445f567f0.png)

插图:从原始表格到非规范化数据对象-作者图片

另一个获得性能和成本效率的转换是使用嵌套数据结构。BigQuery 或 Amazon 的 Redshift 等较新的数据仓库技术确实能更好地处理这种数据结构。使用嵌套数据的用例有，例如标题/段表，如 BKPF 和 BSEG，或者如果一个表包含的列变化不大，如:

*   `name`
*   `lastname`

结合历史上经常变化的列，如[2]:

*   `address`(一个嵌套重复的字段及其后续嵌套字段):
*   `address.address`
*   `address.city`

## **实施(自助)申报**

最后一部分将实现报告和数据分析的可能性。有了 BigQuery，数据分析师已经有了一个很好的分析工具。数据科学家可以使用数据实验室或 GCP 内众多 ML 服务之一。Data Studio 可能是业务用户共享 KPI、仪表板等的合适解决方案。为了在 BigQuery 中实现角色/用户访问模型，通常使用 BUKRS 属性(公司代码)，这样用户就只能看到分配给他的公司代码的数据，这也是 SAP 处理用户权限的方式。

**结论 SAP 和 GCP 的结合将为贵公司带来巨大的数据分析能力和可能性。SAP 系统托管了大量想要分析的有趣数据，而 GCP 提供了许多数据分析/科学家工具和服务，可以实现这一点。**

## 资料来源和进一步阅读:

[1] Google，[https://cloud . Google . com/solutions/sap/docs/big query-replication-from-sap-apps？hl=de](https://cloud.google.com/solutions/sap/docs/bigquery-replication-from-sap-apps?hl=de)

[2]谷歌，[https://cloud.google.com/bigquery/docs/nested-repeated](https://cloud.google.com/bigquery/docs/nested-repeated)

[https://blogs.sap.com/2018/04/08/hana-sda-google-bigquery/](https://blogs.sap.com/2018/04/08/hana-sda-google-bigquery/)

[https://cloud . Google . com/solutions/sap/docs/big query-sap-export-using-SDS](https://cloud.google.com/solutions/sap/docs/bigquery-sap-export-using-sds)