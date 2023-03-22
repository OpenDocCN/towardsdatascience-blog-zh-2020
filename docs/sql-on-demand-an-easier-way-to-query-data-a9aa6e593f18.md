# 按需 SQL:一种更简单的数据查询方式

> 原文：<https://towardsdatascience.com/sql-on-demand-an-easier-way-to-query-data-a9aa6e593f18?source=collection_archive---------21----------------------->

## 查询 CSV、JSON 和 Parquet 之类的文件，而无需将其从其位置移走

![](img/30ba4ece1bc49e44d390b954c6ccab76.png)

作者图片(Azure Synapse Workspace)

# TLDR

*   关于 SQL 按需
*   创建数据源
*   获得对 Azure 存储的访问权限
*   通过 Azure Synapse Workspace 查询 Azure 存储中的文件
*   YouTube 视频对以下步骤进行了可视化描述

# 关于 SQL 按需

在我的上一篇[帖子](/top-10-features-in-azure-synapse-analytics-workspace-ec4618a7fa69)中，我提到了 Azure Synapse Analytics 的 10 大特性。Synapse SQL 数据库有 2 个选项:SQL 池& SQL On-Demand。在这篇文章中，我将深入探讨第二种选择；SQL 随需应变。SQL On-Demand 是一个有趣的特性，它将使查询数据变得更加容易。

那么什么是 SQL 随需应变？根据[微软文档](https://docs.microsoft.com/en-us/azure/synapse-analytics/quickstart-sql-on-demand)。这是一个

> Synapse SQL on-demand (preview)是一种无服务器查询服务，使您能够对 Azure 存储中的文件运行 SQL 查询。

那么，您会在存储中找到什么类型的文件呢？这些文件类似于 **CSV** 、 **Parquet** 文件和 **JSON** 。这非常有趣，因为通常您必须将这些文件导入到数据库中，更不用说确保数据格式与目标数据库环境兼容了。

# 不拉文件怎么查询文件？

这是通过创建外部数据源来完成的。数据来源包括:

*   万能钥匙
*   数据库范围的凭据

# 如何创建数据源

首先，确保您在想要创建它的数据库中。那应该不是*大师*吧。您可以像这样更改您的数据库。

![](img/defd8257d6456960cb224b89f84c03d0.png)

作者图片

1.  **创建主密钥**

> —创建将保护凭证的主密钥:
> 通过密码创建主密钥加密= <在此输入非常强的密码>

**2。创建数据库范围的凭据**

微软文档有一个公共的 URI 可以访问，以按需测试 SQL

> —在我们的演示存储帐户中为容器创建凭据
> 创建数据库范围的凭据 sqlondemand
> ，其中 IDENTITY= '共享访问签名'，
> SECRET = ' SV = 2018–03–28&ss = BF&SRT = SCO&sp = rl&ST = 2019–10–14t 12% 3a 10% 3a 25 z&se = 2061–12–31t 12% 3a 10%

**3。通过创建外部数据源**将所有这些放在一起

> 用(
> LOCATION = '[https://sqlondemandstorage . blob . core . windows . net '](https://sqlondemandstorage.blob.core.windows.net')，
> CREDENTIAL = sqlondemand
> )创建外部数据源 SqlOnDemandDemo

**如果需要重新创建外部数据源**

如果您需要重新创建它，您需要以特定的顺序删除它。首先删除外部数据源，其次是作用域凭据，最后是主密钥。

删除外部数据源 SqlOnDemandDemo

删除数据库范围的凭据 sqlondemand

放下主密钥

# **理解共享访问签名**

理解在 SQL On-Demand 中创建数据源的用例。假设你在 Azure 存储中有一个文件夹，需要授予某人访问权限。这

在上面 2 的脚本中，我们创建了数据库范围的凭证。这是通过使用所谓的共享访问签名 URI 又名 SAS URI。是凭证给了你访问 Azure 存储的权限。这是一个非常漂亮的功能，因为你可以安排一个人访问多长时间。

**那么，我为什么需要一个 SAS URI？**

*   获取 Azure 存储帐户的访问权限
*   为 SQL 按需外部数据源创建数据库范围的凭据

如果您在创建数据源之前直接进入存储，那么创建数据源的步骤可能会更有意义。这在现实世界中很有可能发生，因为在查询数据之前，您应该知道/知道您想要提取什么数据。

**了解 Azure 存储中有什么？**

要进入存储，你需要使用一个叫做 [Azure Storage Explorer](https://azure.microsoft.com/en-us/features/storage-explorer/) 的工具。这是描述其功能的最简单的方式。

*Windows 资源管理器= > OnPremise*

*Azure Storage Explorer = >云*

现在我们了解了 Azure Storage Explorer，让我们打开我们试图查询的文件。

**设置对 Azure Storage Explorer 的访问**

1.  单击图标并
2.  选择“添加帐户”

![](img/ae30a53a2c0c0ab3529e996875ae2b1a.png)

作者照片

3.选择共享访问签名 URI

![](img/4f86a1ce9b7e796d0d07ffb1e530d59a.png)

4.附加共享访问签名 URI，然后按下一步

粘贴在 URI。其他所有内容，如显示名称、Blob、文件和队列端点都将自动填充。

微软文档已经慷慨地提供了一个公共的 URI 来按需测试 SQL

**注:**

*   将 *Blob 端点* + *密钥与这个* **/？”**

斑点终点—[https://sqlondemandstorage.blob.core.windows.net](https://sqlondemandstorage.blob.core.windows.net/?sv=2018-03-28&ss=bf&srt=sco&sp=rl&st=2019-10-14T12%3A10%3A25Z&se=2061-12-31T12%3A10%3A00Z&sig=KlSU2ullCscyTS0An0nozEpo4tO5JAgGBvw%2FJX2lguw%3D)

加上这个**——**[**/？**](https://sqlondemandstorage.blob.core.windows.net/?sv=2018-03-28&ss=bf&srt=sco&sp=rl&st=2019-10-14T12%3A10%3A25Z&se=2061-12-31T12%3A10%3A00Z&sig=KlSU2ullCscyTS0An0nozEpo4tO5JAgGBvw%2FJX2lguw%3D)

secret—[SV = 2018-03-28&ss = BF&SRT = SCO&sp = rl&ST = 2019-10-14t 12% 3a 10% 3a 25 z&se = 2061-12-31t 12% 3a 10% 3a 00 z&SIG = klsu 2 ullcscyts 0 an 0 nozepo 4 to 5 jaggbvw % 2 FJ x2 lguw](https://sqlondemandstorage.blob.core.windows.net/?sv=2018-03-28&ss=bf&srt=sco&sp=rl&st=2019-10-14T12%3A10%3A25Z&se=2061-12-31T12%3A10%3A00Z&sig=KlSU2ullCscyTS0An0nozEpo4tO5JAgGBvw%2FJX2lguw%3D)

![](img/9c7e8a45a8694d4ac5e207806f68c98d.png)

5.选择连接

![](img/683e65692b9f8b8de2bc6a3de8c28d8b.png)

现在，您应该能够在 Azure 存储中看到 CSV、Parquet 和 JSON 文件

![](img/89513671ac59b5953b7a68b1bb55553b.png)

# 从 Azure Synapse 工作区查询 Azure 存储中的文件

现在我们已经了解了要查询的文件，让我们回到 Synapse 工作区进行查询。

这是一个来自微软文档的示例查询。

[](https://docs.microsoft.com/en-us/azure/synapse-analytics/quickstart-sql-on-demand) [## 快速入门:按需使用 SQL-Azure Synapse Analytics

### Synapse SQL on-demand (preview)是一种无服务器查询服务，使您能够对放置在…中的文件运行 SQL 查询

docs.microsoft.com](https://docs.microsoft.com/en-us/azure/synapse-analytics/quickstart-sql-on-demand) 

> 从 OPENROWSET
> (
> BULK ' CSV/population/*)中选择前 10 个*
> 。' csv '，
> DATA _ SOURCE = ' SqlOnDemandDemo '，
> FORMAT = 'CSV '，PARSER _ VERSION = ' 2.0 '
> )
> WITH
> (
> country _ code VARCHAR(5)
> ，country_name VARCHAR (100)
> ，year smallint
> ，population bigint
> ) AS r
> 其中
> country_name = 'Luxembourg '且年份= 2017

我们可以看到熟悉的 SQL 语句，如 SELECT、WITH 和 WHERE 子句。在 from 语句中，我们有 OPENROWSET。这是您设置文件所在的文件夹结构的地方。上面的查询指定遍历文件夹路径“csv/population”中的所有 CSV 文件，以查找国家名称为卢森堡且年份为 2017 年的位置。所有这些都无需移动数据。我们可以对 Parquet 文件和 JSON 文件做同样的事情。查看 my GitHub [链接](https://github.com/Dayobam/Getting-Started-with-Azure-Synapse-Youtube-Series/blob/master/Getting-Started-with-SQL-On-Demand/QuerySQLonDemand.sql)获取其他示例。

# 结论

SQL On-Demand 无疑是 Synapse SQL 数据库的一个有趣部分。最佳用例之一是将数据保存在一个位置，同时仍然能够运行报告、数据分析和机器学习等。