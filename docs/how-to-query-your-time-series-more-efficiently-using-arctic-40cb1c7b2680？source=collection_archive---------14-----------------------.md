# 如何使用 Arctic 更有效地查询时间序列数据

> 原文：<https://towardsdatascience.com/how-to-query-your-time-series-more-efficiently-using-arctic-40cb1c7b2680?source=collection_archive---------14----------------------->

## 加快 Python 时序数据处理脚本的速度

有没有想过在处理大型时间序列数据集时，如何提高数据分析过程的时间效率？ [**北极**](https://github.com/man-group/arctic) **也许就是你要找的。**

![](img/41807dd5a5f9a1cda0610261b189e3c3.png)

史蒂文·勒勒姆在 [Unsplash](https://unsplash.com/s/photos/speed-track?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) 上拍摄的照片

Arctic 是一个为 Python 设计的数据库，目的只有一个:**性能**。使用 [Mongo DB](https://www.mongodb.com/) 作为底层数据库，高效存储数据，使用 [LZ4](https://github.com/lz4/lz4) 压缩，每秒可以查询上亿行。

除了性能数字之外，它还提供了一些非常有说服力的特性:

*   可以处理[熊猫](https://pandas.pydata.org/)、 [Numpy](https://numpy.org/) 和 Python 对象(通过酸洗)；
*   可以拍摄对象的多个版本的快照；
*   为您提供“大块”数据；
*   可以利用 MongoDB 的认证；
*   拥有三种不同类型的*商店。*

# 什么？商店？

***存储引擎*** **是直接与底层 MongoDB 数据库交互的机制。他们被称为斗牛士。为了使您的查询获得最佳性能，您可以选择 S *tores:* 中的三个内置参数之一**

1.  ***版本存储*** 默认存储。基于键值和时间序列。它使创建数据快照和检索数据变得容易，而不会损失任何性能。
2.  ***tick store*** 面向列，支持动态字段。适用于高频金融数据或任何持续跳动的数据。不支持快照。
3.  ***chunk store*** 允许以预定义的块大小存储数据。不支持快照。

您也可以将自己的*存储*实现和插件放入 Arctic，以更好地适应您自己的数据。

> “好的，我明白了，非常有说服力的观点，但是我不知道是否值得使用一个新的数据库……”你，读者，还没有被说服。

# 入门指南

那么，让我们看看使用 Arctic 有多简单，看看我是否能让你，*读者，*更深入地了解使用另一个数据库的想法。这将是一个非常简单的演练，只是为了说明北极的一些核心功能。

## 安装

首先，你需要安装并运行 MongoDB。你可以在[官方 MongoDB 文档页面阅读你的操作系统的说明。](https://docs.mongodb.com/manual/installation/)

完成后，我们可以使用 [pip](https://pypi.org/project/pip/) 安装 Arctic

```
pip install git+https://github.com/manahl/arctic.git
```

我们需要安装熊猫库，因为我们将处理`[DataFrames](https://pandas.pydata.org/pandas-docs/stable/reference/api/pandas.DataFrame.html)`

```
pip install pandas
```

## **编码**

首先:让我们将北极导入到我们的空 Python 脚本中

```
from arctic import Arctic
```

现在，我们需要将 Arctic 连接到它的底层 MongoDB 实例。您可以将 Arctic 连接到托管在云上或本地网络中的任何 MongoDB 实例。因为我在笔记本电脑上运行 MongoDB，所以我将使用`localhost` 作为我的实例的地址。

```
db = Arctic('localhost')
```

太好了！现在我们需要创建一个*库*。

**Arctic 使用*库的概念分离不同的数据。*** 他们可以是市场、地区、用户等。

在本例中，我将使用一个大约 160MB 的 CSV 文件，其中包含一些财务数据。让我们创建一个*财务*库来存储它。通过不向`initialize_library`方法传递一个`lib_type`值，Arctic 将默认这个库使用*版本存储*存储引擎。对于我们的例子来说，这很好。

```
db.initialize_library('Finance')
```

我们需要访问刚刚创建的*库*，以便向其中写入一些数据。让我们用库名索引我们的`db`对象。

```
finance_library = db['Finance']
```

在写入数据之前，让我们打开时间序列数据文件。在本例中，我将使用一个名为`[finance.csv](https://github.com/tgcandido/time-series-with-arctic/blob/master/finance.csv)`的文件(本例中使用的 CSV 结构的演示文件)。

让我们使用 Pandas 库打开 CSV 文件。首先，我们需要导入库，然后使用`[read_csv](https://pandas.pydata.org/pandas-docs/stable/reference/api/pandas.read_csv.html)`方法将内容读入一个熊猫`DataFrame`。

在将`string`转换为`datetime`之后，我们还会将`unix`列设置为`DataFrame`的索引。

```
import pandas as pd
df = pd.read_csv('finance.csv')
df['unix'] = pd.to_datetime(df['unix'])
df.set_index('unix', inplace=True)
```

好吧！我们准备将数据写入北极。为此，我们需要定义一个*符号。*

一个*符号*是一个字符串，我们将使用它在一个库中读取或写入我们的数据。

让我们使用符号`Stocks`将我们加载的`df`的内容存储到库`finance_library`中。

```
finance_library.write('Stocks', df)
```

我们可以通过使用方法`read`并访问返回对象的`data`属性以获得结果`DataFrame`来验证数据是否被正确插入。

```
new_df = finance_library.read('Stocks').data
```

要执行查找特定时间间隔的查询，我们可以使用`read`方法中的`date_range`参数。

首先，我们需要从`arctic.date`导入`DateRange`:

```
from arctic.date import DateRange
```

我们可以将一个`DateRange`的实例传递给`date_range`参数。我们可以通过调用它的构造函数并将开始日期和结束日期作为参数传递来创建它。

```
custom_range = DateRange('2020-01-01', '2020-01-02')
```

现在我们可以运行查询:

```
range_df = finance_library.read('Stocks', date_range=custom_range).data
```

就是这样！在这个非常简单的例子中，我们通过实现一个处理一些财务数据的 Python 脚本看到了 Arctic 的主要特性。

官方文档真的很好，几乎包含了所有需要的信息，从基础到高级特性。没有必要阅读软件包的源代码，但是如果您有兴趣了解 Arctic 的工作方式以及它是如何实现其高性能的，我鼓励您这样做。

但恐怕我不会说服你，*读者，*除非我给你看一些业绩数字，对吗？

# 基准

这些数字都是在我的 2.3 GHz 双核 13 英寸 2017 Mac Book Pro 上运行脚本获得的。

> 请记住，这绝不是北极数据库的完整基准。这是用 Python 的时间库在 Arctic、MongoDB、SQLite 和普通 CSV 文件之间进行的简单比较。

## 性能比较

下面是运行一个简单的 Pandas `read_csv`、一个 PyMongo 查询、 [SQLAlchemy](https://www.sqlalchemy.org/) 查询(使用 SQLite 数据库)和一个 Arctic `read`查询(使用大约 160 MB 的财务数据作为源)的结果。

在引擎返回结果后，PyMongo 和 SQLAlchemy 查询结果被解析成一个`DataFrame` ，这个时间在基准测试中被考虑。两个数据库都由`unix`列索引。

*   熊猫`read_csv`:4.6 秒
*   PyMongo 查询:~28 秒
*   SQLite 查询:大约 30 秒
*   北极`read` : ~1.45 秒

这些结果来自于一个“get all”类型的查询。让我们尝试添加一个范围参数，看看结果是否成立。

```
new_df = finance_library.read('Stocks', date_range=DateRange('2020-01-01', '2020-01-02')).data
```

*   熊猫`read_csv`:4.9 秒
*   PyMongo 查询:~1.66 秒
*   SQLite 查询:~0.7 秒
*   北极`read`与`DateRange` : ~0.12 秒

在使用了`DateRange`之后，结果仍然支持 Arctic，这是我们在处理时间序列数据时使用的主要查询类型。

## 磁盘压缩

同一个 CSV 文件用于播种每个数据库。

这是每个备选方案使用的磁盘空间量:

*   普通 CSV 文件:160.8 MB
*   MongoDB 集合:347.31 MB
*   SQLite: 297.9 兆字节
*   **北极:160.59 兆字节**

# 结论

**在处理大型时间序列数据集时使用 Arctic 使我们能够实现显著的速度和压缩改进**。凭借其简单的设置和使用，它可以提高生产力和节省一些宝贵的时间。

即使不使用其更高级的功能，如快照或其他存储引擎，我们也可以为使用 Arctic 处理时间序列数据提供有力的支持。

演练和基准测试的代码可以在这里找到[。出于许可的原因，它不包含完整的 CSV 文件，但是我鼓励您使用您自己的一些数据来运行，看看您的结果是否与我的相似。](https://github.com/tgcandido/time-series-with-arctic)

我希望你喜欢阅读这篇文章。

如果你有，可以考虑在 [*推特*](https://twitter.com/ogaihtcandido) *上关注我。*

谢谢你的时间。保重，继续编码！