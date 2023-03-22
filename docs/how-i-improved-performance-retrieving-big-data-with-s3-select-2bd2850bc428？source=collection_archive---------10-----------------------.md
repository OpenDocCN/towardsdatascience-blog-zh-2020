# 我如何通过 S3 选择提高检索大数据的性能

> 原文：<https://towardsdatascience.com/how-i-improved-performance-retrieving-big-data-with-s3-select-2bd2850bc428?source=collection_archive---------10----------------------->

## [理解大数据](https://towardsdatascience.com/tagged/making-sense-of-big-data)

## 如何使用 S3 选择有效地提取数据，以及它与亚马逊雅典娜有何不同

![](img/d303b1ead9f3df7348eeb61a5396e1e2.png)

照片由 [Pexels](https://www.pexels.com/photo/abstract-blur-bright-color-243908/?utm_content=attributionCopyText&utm_medium=referral&utm_source=pexels) 的 [Erkan Utu](https://www.pexels.com/@erkan?utm_content=attributionCopyText&utm_medium=referral&utm_source=pexels) 拍摄

我最近在 S3 遇到一个功能，在处理大数据时特别有用。您可以编写一个简单的 SQL 查询来选择特定的列并筛选特定的行，以便只检索您的应用程序所需的数据。在本文中，我将演示如何使用 Python 的`boto3`库来实现这一点。

# 示例使用案例

想象以下场景:

*   你经常会得到一个**大型 CSV 文件**，它存储在一个 S3 桶中。该文件包括您的网上商店活跃的所有国家的营销数据。
*   然而，你有一个运行在 AWS Lambda 中的应用程序，它只需要来自某个特定国家的营销数据。

通常，您必须下载整个大文件并过滤掉应用程序中的数据。问题是你的 Lambda 函数可能没有足够的内存将这个大文件读入内存。有一些方法可以解决这个问题，比如读取和过滤块中的数据或者将函数移动到 Docker 容器，但是在这个场景中最简单和最具成本效益的解决方案是使用 **S3 选择**特性。这是它的工作原理。

# 履行

首先，由于我实际上没有任何营销数据，我们将使用来自纽约的出租车出行数据[1]，我们将过滤具有**未知支付类型**的可疑记录，根据[数据字典](https://www1.nyc.gov/assets/tlc/downloads/pdf/data_dictionary_trip_records_yellow.pdf)编码为`payment_type==5`。

让我们看看整个文件有多少行，相比之下，只有那些与`payment_type==5`。

这意味着我们只对 50 万条记录中的 12 条感兴趣。仅仅为了获得这 12 行数据而将整个数据集下载并读取到内存中是一种巨大的资源浪费！

## S3-选择

为了展示 S3 选择的实际效果，我们首先将我们的大型 CSV 文件[1]上传到 S3 存储桶:

现在，我们可以使用 S3 选择来仅获取付款类型等于 5 的数据，即**我们仅从 S3 检索我们感兴趣的数据** —付款类型未知的数据。最棒的是，这一切都是在一个简单的 SQL 查询中定义的:

*   在我们的查询中，我们只选择用例需要的列
*   我们只过滤`payment_type='5'` —注意，在 S3，平面文件中的所有列都被认为是文本，所以一定要用引号`'5'`将值括起来。

在上面的代码片段中，我们必须定义`InputSerialization='CSV'`来指定这是我们的 S3 对象的格式。此外，我们将`FileHeaderInfo`设置为`'Use'`，这确保我们可以在 S3 选择查询中使用列名。

通过使用`OutputSerialization`参数，我们定义我们希望我们的输出是逗号分隔的，这允许我们将结果存储在一个单独的 CSV 文件中。如果您想在 API 中使用 S3 选择，您可能更喜欢`JSON`格式。

**一些额外的警告:**

*   S3 选择只返回记录，不返回列名(*标题*)，所以在第 27 行，我确保在`query`中定义的相同列也作为第一行包含在`TARGET_FILE`中。
*   S3 选择返回一个**编码字节流** [2]，所以我们必须循环返回的流并解码输出:`.decode('utf-8')`。

## 测试结果

让我们交叉检查一下`TARGET_FILE` ——它应该只有 12 行。

结果是:

```
Nr of rows: 12
    ID  distance  tip  total
0    1       9.7    0  31.80
1    1      10.0    0  30.80
2    1       0.4    0   7.80
3    1      13.6    0  40.80
4    1       0.5    0   7.80
5    1       1.7    0  11.80
6    1       6.4    0  22.80
7    1       4.0    0  16.80
8    1       1.7    0  11.30
9    1       5.6    0  22.80
10   1       7.3    0  30.42
11   1       6.7    0  24.80
```

# S3-选择 vs 雅典娜

您可能会问:如果 S3 这么容易使用，为什么我们需要 Athena 来查询数据湖？这两者的主要区别如下:

*   雅典娜可以一次**查询** **多个对象**，而使用 S3 选择，我们只能查询单个对象( *ex。单个平面文件*
*   有了 Athena，我们可以使用符合 ANSI 的 SQL 查询封装复杂的业务逻辑，而 **S3 选择**让您只执行基本查询，在从 S3 加载数据之前**过滤掉数据**。
*   雅典娜支持更多的文件格式和更多形式的文件压缩比 S3 选择。例如， **S3 选择**只支持 **CSV、JSON 和 Parquet** ，而 Athena 另外允许 TSV、ORC 文件等等。
*   S3 选择**仅适用于 S3 API** (例如。通过使用 Python **boto3** SDK)，而 Athena 可以通过 JDBC 从管理控制台或 SQL 客户端直接查询。
*   Athena 允许许多**优化**技术来获得更好的性能和成本优化，例如分区、列存储，而 S3 选择是一个非常基本的查询，除了过滤数据什么都不是。
*   S3 选择可以直接查询，而**雅典娜**需要定义一个**模式**。

# S3-精选的优势

简而言之，这个 API 的好处是:

*   减少 IO，从而提高性能
*   由于数据传输费用减少，降低了成本。

# 结论

在本文中，我们讨论了 S3 选择，它允许过滤存储在 S3 的数据。S3 选择应该只在处理单个文件时使用，并且只需要从平面文件中选择特定的列和特定的行。

**如果这篇文章有帮助，** [**关注我**](https://medium.com/@anna.anisienia) **看我下一篇文章。**

在下面链接的文章中，我讨论了在 S3 存储桶之间传输大量数据的各种选择。

[](https://medium.com/better-programming/it-took-2-days-and-7-engineers-to-move-data-between-s3-buckets-d79c55b16d0) [## 7 名工程师花了 2 天时间在 S3 存储桶之间移动数据

### 在两个 S3 存储桶之间传输大数据的最佳选择

medium.com](https://medium.com/better-programming/it-took-2-days-and-7-engineers-to-move-data-between-s3-buckets-d79c55b16d0) 

**资源:**

[1] TLC 行程记录数据:[https://www1 . NYC . gov/site/TLC/about/TLC-Trip-Record-Data . page](https://www1.nyc.gov/site/tlc/about/tlc-trip-record-data.page)

[2]boto 3 Docs:[https://boto 3 . Amazon AWS . com/v1/documentation/API/latest/reference/services/S3 . html # S3。客户端.选择 _ 对象 _ 内容](https://boto3.amazonaws.com/v1/documentation/api/latest/reference/services/s3.html#S3.Client.select_object_content)

[3] AWS 文档:[https://Docs . AWS . Amazon . com/Amazon S3/latest/API/API _ input serialization . html](https://docs.aws.amazon.com/AmazonS3/latest/API/API_InputSerialization.html)和[https://Docs . AWS . Amazon . com/Amazon S3/latest/API/API _ selectobject content . html](https://docs.aws.amazon.com/AmazonS3/latest/API/API_SelectObjectContent.html)