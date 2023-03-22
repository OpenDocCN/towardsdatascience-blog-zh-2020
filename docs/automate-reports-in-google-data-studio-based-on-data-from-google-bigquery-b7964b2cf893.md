# 基于来自 Google BigQuery 的数据，在 Google Data Studio 中自动生成报告

> 原文：<https://towardsdatascience.com/automate-reports-in-google-data-studio-based-on-data-from-google-bigquery-b7964b2cf893?source=collection_archive---------21----------------------->

![](img/fdc38f0ab800207e9dff52941db0fe71.png)

来源:[沉积照片](https://ru.depositphotos.com/102486980/stock-photo-man-at-computer-screen-at.html)

## 将报告配置为使用 Google App 脚本自动更新，并在 Data Studio 中可视化数据

# 关于使用 Google Data Studio

为那些还不知道[数据工作室](https://datastudio.google.com/)的人说几句:

*   首先，Data Studio 很方便，因为它有[很多第三方服务的连接器](https://datastudio.google.com/data)。他们可以很容易地连接到几乎任何数据源。谷歌的原生连接器和其他公司开发的连接器都可用，例如 Yandex。度量，Yandex。目录、脸书、推特等。如有必要，您可以创建自己的连接器。
*   该服务易于使用，并且易于可视化数据。多个源可以连接到一个仪表板。
*   在 Data Studio 中与同事共享报告很容易，让他们能够查看或编辑报告。同事不需要获得授权，他们只需点击链接打开仪表板。
*   该工具的几乎所有功能都可以在免费版本中获得。

下面是 Data Studio 中的一个示例报告，其中包含一个在线商店的主要 KPI 和流量来源:

![](img/599c6643002608aa8d3f8ec2ec9552e4.png)

这是一个交互式控制面板，您可以在其中看到指标如何随时间、渠道、设备类别等发生变化。一份报告中可以有多页。这就是它如此方便的原因:一旦您设置了一个仪表板并与同事共享了一个链接，那么就不需要再做任何更改了(除非您添加了新的参数)。只要选择正确的日期，获取最新的信息。

现在让我们来解释一下如何才能创造出这样的美。我们基于来自 Google BigQuery 的数据构建报告，该报告将根据指定的时间表自动更新。

## 第一步。从 Google BigQuery 收集数据

BigQuery 有现成的库和连接器，可以让你从 CRM 系统上传交易、客户和项目信息到云存储。OWOX BI 将帮助您[自动收集 GBQ:](https://www.owox.com/products/bi/pipeline/) 中的所有其他数据

*   用户在网站上的行为(未采样的实时数据)。
*   广告服务的成本数据。
*   来自通话跟踪系统的通话和聊天数据。
*   来自电子邮件服务的电子邮件信息。

将不同来源的数据合并到 BigQuery 后，您需要:

*   使用 SQL-query 选择您希望在报告的单独表格中看到的指标。
*   使用 Google App 脚本设置一个规则来更新此表中的数据。
*   在 Data Studio 中，将表与 GBQ 中的数据连接起来，并将其可视化。

用 OWOX BI 组合数据和自动生成报告的示意图如下:

![](img/7715ae676f742420a8488eb435d089e4.png)

## 第二步。在 Google BigQuery 中准备一个包含报告数据的表格

我们没有详细描述这些指令，因为我们假设您熟悉 Google BigQuery 接口。如果您需要复习如何处理表创建，请查看这篇文章:[Google 中的 BigQuery 数据结构:如何开始使用云存储。](https://www.owox.com/blog/use-cases/bigquery-schema/)

为了节省 Google BigQuery 资源，我们建议您首先创建一个 SQL-query 来生成一个表，其中包含您在特定时间段内需要的一组参数和指标:

![](img/818384fc44198d39a77bd4f4fa20e11c.png)

进行查询并将结果保存为单独的 BigQuery 表:

![](img/d55567b0c09c0e72f35f4179a6745bad.png)

然后创建一个视图，只在更短的时间内计算相同的指标。例如，您创建了一个十二月的源表。然后，每天视图将请求昨天的数据并将其添加到表中。

为此，运行另一个查询并单击保存视图:

![](img/58d5b094c31cb93c7a9b01aad06a7889.png)

## 第三步。创建自动更新表格的应用程序脚本

现在您需要创建一个脚本，该脚本将自动启动视图并将更新后的数据从视图发送到源 BigQuery 表。

为此，[打开 Apps 脚本](https://script.google.com/home/start)，单击创建脚本，命名它，并编写以下代码，用您自己的表替换项目名称、数据集和 BigQuery 表。

![](img/58011790969577bcff48067d6f1e9e14.png)

**填写表格，在您的电子邮件中获得代码的全文**

[接收代码](https://www.owox.com/blog/use-cases/bigquery-reports-in-google-data-studio/#download)

然后单击时钟图标设置脚本运行的时间表。单击右下角的添加触发器按钮。选择时间触发事件源，指定所需的运行频率，然后单击保存。

![](img/8edd7c43f539a4f585fa5b1fbacef3d7.png)

准备好了！现在，GBQ 表中的数据将根据指定的时间表进行更新。

## 第四步。在 Google Data Studio 中创建报告

进入 [Data Studio 主页](https://datastudio.google.com/navigation/reporting)，点击左上角的 New，选择 Report。然后点击右下角的 Create Data Source 并选择 BigQuery:

![](img/178e68bbd80b1f30de67855f691719f4.png)

然后指定包含报告数据的项目、数据集和 GBQ 表，并单击右上角的链接:

![](img/550ea822ef251050286b0b6a6409c259.png)

在打开的窗口中，您将看到存储在连接表中的所有参数和指标，您可以使用这些参数和指标。在这里，如果需要，您可以通过单击右侧指标来重命名它们。

![](img/285668afe465b1a01ac8f4d52a85001f.png)

您还有机会创建自己的定制指标。使用“添加字段”按钮和公式(如果您单击问号，会出现帮助):

![](img/24236d6de1914de40e7e189a7b8054b3.png)

定义字段后，单击右上角的“添加到报告”按钮:

![](img/c141a01a4ea85a971c4a6d479f545da4.png)

然后选择可视化工具(图形类型)，并在操控板上突出显示要放置它的区域。

![](img/3036622790d0c42ae72ddd4c01fcd5d7.png)

控制面板在右侧打开，您可以根据需要定制报告:添加用于比较的参数、定制过滤器、日期范围、仪表板样式等。

![](img/cce8d7a7e0c88aeae44ba1a723a912d5.png)

然后剩下的就是使用 Grant Access 按钮与同事共享报告。