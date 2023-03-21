# 雪花和达克

> 原文：<https://towardsdatascience.com/snowflake-and-dask-33c84ce0bc36?source=collection_archive---------23----------------------->

## 如何高效地将数据从雪花加载到 Dask

![](img/e58b702801c24d7142986bfe3c709dbe.png)

照片由[大流士·科托伊](https://unsplash.com/@dariuscotoi?utm_source=medium&utm_medium=referral)在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 拍摄

雪花是土星用户中最受欢迎的数据仓库。本文将介绍将雪花数据加载到 Dask 的有效方法，这样您就可以大规模地进行非 sql 操作(比如机器学习)。声明:我是土星云的首席技术官，我们专注于企业 Dask。

# 基础知识

首先，一些基础知识，将雪花数据加载到 Pandas 的标准方法:

```
import snowflake.connector
import pandas as pdctx = snowflake.connector.connect(
    user='YOUR_USER',
    password='YOUR_PASSWORD',
    account='YOUR_ACCOUNT'
)
query = "SELECT * FROM SNOWFLAKE_SAMPLE_DATA.TPCH_SF1.CUSTOMER"
pd.read_sql(query, ctx)
```

雪花最近为这个操作引入了一个更快的方法，`fetch_pandas_all`和`fetch_pandas_batches`利用了[箭头](https://arrow.apache.org/)

```
cur = ctx.cursor()
cur.execute(query)
df = cur.fetch_pandas_all()
```

`fetch_pandas_batches`返回一个迭代器，但是由于我们将把重点放在将它加载到一个分布式数据帧中(从多台机器中提取)，我们将设置我们的查询来分割数据，并在我们的 workers 上使用`fetch_pandas_all`。

# 雪花有什么好处？

将所有数据从雪花中分离出来，以便在 Dask 中使用，这可能非常诱人。这肯定是可行的，但是，snowflake 在对数据应用类似 sql 的操作方面要快得多。雪花存储数据，并有高度优化的例程，以获得查询的每一盎司的性能。这些示例将假装我们正在将整个数据加载到 Dask 中，在您的情况下，您可能会有一些 sql 查询，它执行您所关心的类似 SQL 的转换，并且您将把结果集加载到 Dask 中，这是 Dask 擅长的事情(可能是某些类型的功能工程和机器学习)。[土星云](https://www.saturncloud.io/s/)已经与 [Dask 和雪花](https://www.saturncloud.io/s/snowflake/)进行了原生集成，所以[如果你对这个感兴趣的话就去看看](https://https//manager.aws.saturnenterprise.io/register)。

# Dask 如何加载数据？

你可以把一个 Dask 数据帧想象成一个大熊猫数据帧，它已经被切碎并分散在一堆计算机上。当我们从 Snowflake 加载数据时(假设数据很大)，将所有数据加载到一台机器上，然后分散到您的集群中，效率并不高。我们将重点让 Dask 集群中的所有机器加载数据的一个分区(一小部分)。

# 数据分组

我们需要一种方法将数据分割成小的分区，这样我们就可以将数据加载到集群中。SQL 中的数据不一定有任何自然的顺序。您不能只是说将前 10k 行放入一个分区，而将后 10k 行放入另一个分区。这种划分必须基于一列数据。例如，您可以按日期字段对数据进行分区。或者您可以通过在雪花表格中添加一个`identity`列来创建一个行号。

一旦您决定了要在哪个列上对数据进行分区，在雪花一侧设置数据聚类就非常重要。每个工人都会要求一小部分数据。大约

```
select * from table where id < 20000 and id >= 10000
```

如果不设置数据集群，每个查询都会触发对结果数据库的全表扫描(我可能夸大了这个问题，但是如果没有数据集群，这里的性能会非常差)

# 装弹！

这里我们不打算使用 dask 库中的`read_sql_table`。我更喜欢对如何从雪花加载数据有更多的控制，我们想调用`fetch_pandas_all`，这是一个雪花特定的函数，因此不支持`read_sql_table`

```
import snowflake.connector
from dask.dataframe import from_delayed
from dask.distributed import delayed @delayed
def load(connection_info, query, start, end):
    conn = snowflake.connector.connect(**connection_info)
    cur = conn.cursor()
    cur.execute(query, start, end)
    return cur.fetch_pandas_all() ddf = from_delayed(*[load(connection_info, query, st, ed) for st, ed in partitions])
ddf.persist()
```

此代码假设分区是开始/结束分区的列表，例如:

```
partitions = [(0, 10000), (10000, 20000), ...]
```

`delayed`是一个装饰器，它把一个 Python 函数变成一个适合在 dask 集群上运行的函数。当您执行它时，它会返回一个`delayed`结果，代表函数的返回值。`from_delayed`获取这些`delayed`对象的列表，并将它们连接成一个巨大的数据帧。

# 内存优化

这是先进的概念，但我强烈建议您阅读这一部分，它可以为您节省大量时间，并避免您工作站内存不足的问题。不要因为 Snowflake 说一个数据集是 20GB，就以为加载到`pandas`里就是 20GB。内存表示中的`pandas`总是要大得多，尽管您可以通过更好地使用数据类型来做得更好。

# 诊断内存使用情况

`df.memory_usage(deep=True)`是了解每一列使用了多少内存的好方法。这有助于您了解将数据转换为适当的数据类型的好处。

# StringDType

Python 字符串大约有 40 字节的开销。这听起来不是很多，但如果你有十亿个字符串，它可以很快累加起来。新的 StringDType 可以在这方面有所帮助。

```
df['column'] = df['column'].astype(pd.StringDType())
```

# 分类数据类型

许多字符串和数值字段实际上是分类的。取一个名为“家庭收入”的栏目。你通常得到的不是一个数值，而是一组数据，比如“0-$40，000”或者“超过$100，000”。

一般来说，我通常会寻找唯一值的数量与行数之比小于 1%的列。

在熊猫中，这是相关的代码。

```
df['column'] = df['column'].astype("category")
```

但是，我假设从雪花中加载整个列来计算分类数据类型是不可行的。我建议使用以下类型的查询来确定哪些列适合分类数据类型:

```
select 
  count(distinct(col1)),
  count(distinct(col2)),
  ...
 from table
```

您可以将结果与表中的行数进行比较，以确定哪些列应该是分类的。

然后算出独特的价值

```
select distinct(col1) from table
```

# 把所有的放在一起

假设您已经完成了上面列出的一些内存优化，并且已经确定了一些应该转换为 StringDType 的字段，一些应该转换为 categoricals。假设您有一个名为`dtypes`的字典，它是列名到您希望将结果强制转换成的 dtype 的映射。

```
import snowflake.connector
from dask.dataframe import from_delayed
from dask.distributed import delayed @delayed
def load(connection_info, query, start, end, dtypes):
    conn = snowflake.connector.connect(**connection_info)
    cur = conn.cursor()
    cur.execute(query, start, end)
    return cur.fetch_pandas_all().astype(dtypes) ddf = from_delayed(*[load(connection_info, query, st, ed) for st, ed in partitions])
ddf.persist()
```

感谢阅读。如果你有兴趣将 [Dask 用于雪花](https://www.saturncloud.io/s/snowflake/)，那么[我推荐你去看看土星云](https://manager.aws.saturnenterprise.io/register?trackid=17314f965cd13a-02aa54870e0bee-24414032-317040-17314f965ce320)。