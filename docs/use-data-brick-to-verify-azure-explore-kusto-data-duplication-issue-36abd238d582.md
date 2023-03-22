# Python 找到了带有数据重复问题的 Kusto 表

> 原文：<https://towardsdatascience.com/use-data-brick-to-verify-azure-explore-kusto-data-duplication-issue-36abd238d582?source=collection_archive---------51----------------------->

## Python 与 Kusto 一起寻找重复

![](img/8398cc2267cfd1f374494f4c32641171.png)

约书亚·迈克尔斯在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上的照片

Azure Data Explorer ( **Kusto** )是市场上最专注的关系数据库之一。整个系统运行在固态硬盘和内存中，以提供快速响应的数据分析。作为热路径数据存储，这可能是一个不错的选择。

由于各种原因，如客户端功能不良、数据管道不完善等。数据可能会多次被输入到 Kusto。这导致了数据重复的问题。如果摄取的数据是**汇总**数据，比如一组商店的总收入等，这个问题可能会更加严重。

数据重复会搞乱所有后续的数据分析，人们可能会据此做出错误的决定。因此，数据清理/重复数据删除是必要的。在此之前，我们需要首先确认，当前的 Kusto 表是否存在重复问题。

**确认**步骤是本文的重点。

主要思想包含以下步骤:

1.  连接到 Kusto 集群。
2.  查询表架构。
3.  创建每行的唯一标识
4.  对具有相同标识的行进行计数
5.  查找任何计数大于 1 的标识值，标记为重复。

## 连接到 Kusto 集群

Python 有连接 Kusto 的包:[Azure Data Explorer Python SDK](https://docs.microsoft.com/en-us/azure/data-explorer/kusto/api/python/kusto-python-client-library)。在这里，我们使用包:azure-kusto-data。

下面的代码片段将允许我们创建 KustoClient。它用于查询 Kusto 集群。在连接到 Kusto 之前，我们需要创建 AppId，并将其注册到 Kusto 集群。

## 查询表模式

**getschema** 将返回表模式。KustoClient 将返回表模式作为我们熟悉的 Pandas DataFrame。我们很容易做进一步的加工。

**每行的唯一标识**

假设，该表是一个汇总表。没有可以唯一标识该行的列子集。因此，我们将使用模式中的所有列来创建**标识**。标识将是所有列值的串联。

因此，我们将使用 tostring()操作符将所有非字符串数据转换为字符串。这就是 **schema.apply( axis = 1)** 的目的，其中 axis = 1 将逐行遍历表。

最后，Kusto 的 **strcat()** 将根据 hashOp 定义的操作连接所有的列。

如果对于另一个表，我们知道列的子集可以唯一地标识行，例如 user_id 和 order_id 的组合。在这种情况下，我们可以使用第二个 hashKusto 案例。

## 相同标识值计数并查找重复项

注意，我们上面创建的 hashKusto 值在 Kusto 查询中用作扩展。这将在 KustoTable 中创建一个额外的列， **hash** 。我们稍后使用 summarize 来获得每个标识散列的计数。

最后，重复的记录是 recordsCount > 1 的记录。

# 带走:

通过使用 Python，我们建立了一种简单直接的方法来验证和识别 Kusto 表中的重复行。这将为后续的数据分析提供坚实的基础。