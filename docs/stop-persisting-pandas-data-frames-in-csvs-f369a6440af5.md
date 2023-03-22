# 停止在 CSV 中保存 pandas 数据帧

> 原文：<https://towardsdatascience.com/stop-persisting-pandas-data-frames-in-csvs-f369a6440af5?source=collection_archive---------3----------------------->

## pickle、parquet 和其他产品的优势——更快、更可靠、更高效

![](img/a6b9db5dc1a4bc0338dca065acefb410.png)

CSV 是一种很好的数据交换格式。它在世界各地都可以理解，并且可以在普通记事本中编辑。这并不意味着它适合持久化所有数据帧。CSV 的读写速度很慢，它们占用更多的磁盘空间，最重要的是 CSV 不存储关于**数据类型**的信息。

CSV 的优势:

*   普遍可以理解
*   大多数编程语言都支持解析和创建

CSV 的缺点:

*   更大的磁盘使用率
*   读写速度较慢
*   不要存储关于数据类型的信息

伊利亚·扎依采夫对各种熊猫坚持方法的速度和内存使用做了很好的分析。当我们编写自动化性能测试程序时，我会在最后谈到性能。首先，我想关注一件不同的事情——这些文件格式如何处理各种数据类型。

# 熊猫数据类型

Pandas 支持丰富的数据类型，其中一些数据类型具有多种子类型，以便更有效地处理大数据框。基本数据类型包括:

*   `object` —字符串或混合类型
*   `string` — [自熊猫 1.0.0](https://pandas.pydata.org/pandas-docs/stable/user_guide/text.html)
*   `int` —整数
*   `float` —浮点数
*   `bool` —布尔值真和假
*   `datetime` —日期和时间值
*   `timedelta` —两个日期时间之间的时间差
*   `category` —有限的值列表存储在高效内存查找中

由于 pandas 使用 numpy 数组作为其后端结构，`int` s 和`float` s 可以区分为更高效的内存类型，如`int8`、`int16`、`int32`、`int64`、`unit8`、`uint16`、`uint32`和`uint64`以及`float32`和`float64`。

CSV 不存储关于数据类型的信息，您必须用每个`read_csv()`来指定它。在没有告知 CSV 阅读器的情况下，它将推断所有整数列为效率最低的`int64`，浮点为`float64`，类别将作为字符串和日期时间加载。

```
# for each loaded dataset you have to specify the formats to make DataFrame efficientdf = pd.read_csv(new_file,
                 dtype={"colA": "int8",
                        "colB": "int16",
                        "colC": "uint8",
                        "colcat": "category"},
                 parse_dates=["colD","colDt"])
```

`**TimeDeltas**`作为字符串存储在 CSV`-5 days +18:59:39.000000`中，你必须编写一个特殊的解析器将这些字符串转换回熊猫的 timedelta 格式。

`Timezones`看起来像`2020-08-06 15:35:06-06:00`，在`read_csv()`中也需要特殊处理。

![](img/01303c2394d1b0c4fc7c1d383dd842b5.png)

使用不带参数的 read_csv()比较原始数据类型和自动推断的类型

# CSV 替代方案

幸运的是，csv 不是持久存储数据帧的唯一选择。[阅读熊猫的 IO 工具](https://pandas.pydata.org/pandas-docs/stable/user_guide/io.html)你看到一个数据帧可以写成多种格式，数据库，甚至剪贴板。

你可以用这个 [**GitHub**](https://github.com/vaclavdekanovsky/data-analysis-in-examples/blob/master/Pandas/Persistance/Stop%20Persisting%20Pandas%20to%20CSV.ipynb) 笔记本自己运行代码。最后，我将详细描述数据是如何创建的，并使用一个真实的数据帧指导您完成性能测试和健全性检查。

![](img/4466b69963c74eb24bedef16b985a52d.png)

在本文中，我们用几个参数测试了许多类型的持久化方法。由于 [Plotly](/visualization-with-plotly-express-comprehensive-guide-eb5ee4b50b57) 的交互功能，您可以探索任何方法的组合，图表将自动更新。

# Pickle 和 to_pickle()

Pickle 是用于对象序列化的 [python 原生格式](https://docs.python.org/3/library/pickle.html)。它允许 python 代码实现任何类型的增强，如[pep 574](https://www.python.org/dev/peps/pep-0574/)pickle 带外数据缓冲区中描述的最新协议 5。

这也意味着在 Python 生态系统之外进行酸洗是困难的。然而，如果你想存储一些预处理过的数据以备后用，或者你不想在没有直接使用这些数据的情况下浪费掉几个小时的分析工作，就把它们保存起来。

```
# Pandas's to_pickle method
df.to_pickle(path)
```

与`.to_csv()` `.to_pickle()`相反，方法只接受 3 个参数。

*   `path` —数据将存储在哪里
*   `compression` —允许选择多种压缩方式
*   `[protocol](https://docs.python.org/3/library/pickle.html#data-stream-format)` —更高的协议可以更高效地处理更广泛的数据

泡菜的优点:

*   比 CSV 快(CSV 写入的 5–300%，CSV 读取的 15–200%，具体取决于压缩方法)
*   生成的文件较小(约为 csv 的 50%)
*   它保留了关于数据类型的信息(100%)
*   不需要指定过多的参数

泡菜的缺点:

*   原生于 python，所以它缺乏对其他编程语言的支持
*   即使跨不同 python 版本也不可靠

![](img/5061bd24edf5041bb703d6053b3e2f3d.png)

Pickle 能够序列化 100%的 padnas 数据类型

当您读取在 python 的新版本中创建的 pickle 时，您会得到一个错误:

```
**ValueError**: unsupported pickle protocol: 5
```

# **拼花和 to_parquet()**

Apache Parquet 是 Hadoop 生态系统中使用的压缩二进制列存储格式。它允许序列化复杂的嵌套结构，支持按列压缩和按列编码，并提供快速读取，因为它不需要读取整个列，因为您只需要部分数据。

```
# Pandas's to_parquet method
df.to_parquet(path, engine, compression, index, partition_cols)
```

`.to_parquet()`方法只接受几个参数。

*   `path` —存储数据的位置
*   `engine` — pyarrow 或 fastparquet 引擎。`pyarrow`通常更快，但它与`timedelta`格式斗争。`fastparquet`可以明显变慢。
*   `compression` —允许选择多种压缩方式
*   `index` —是否存储数据帧的索引
*   `partition_cols` —指定列分区的顺序

拼花地板的优点:

*   比 CSV 快(从 10 行开始，pyarrow 大约快 5 倍)
*   生成的文件更小(约为 CSV 的 50%)
*   它保存关于数据类型的信息(pyarrow 不能处理 timedelta，而较慢的 fastparquet 可以)
*   Hadoop 生态系统中的广泛支持允许在许多分区中进行快速过滤

拼花地板的缺点:

*   不支持重复的列名
*   pyarrow 引擎不支持某些数据类型

更多细节请访问[熊猫 IO 页面](https://pandas.pydata.org/pandas-docs/stable/user_guide/io.html#parquet)。

# Excel 和 to_excel()

有时候，将数据导出到 excel 中很方便。它以最慢的读写速度为代价，增加了易于操作的优势。它还忽略了许多数据类型。`Timezones`根本无法写入 excel。

```
# exporting a dataframe to excel
df.to_excel(excel_writer, sheet_name, many_other_parameters)
```

有用的参数:

*   `excel_writer` —熊猫 excel writer 对象或文件路径
*   `sheet_name` —将输出数据的工作表的名称
*   `float_format` — excel 的原生数字格式
*   `columns` -别名数据框列的选项
*   `startrow` —向下移动起始单元格的选项
*   `engine` — `openpyxl`或`xlsxwriter`
*   `freeze_panes` —冻结行和列的选项

excel 的优势:

*   允许自定义格式和单元格冻结
*   人类可读和可编辑的格式

excel 的缺点:

*   非常慢的读/写速度(慢 20/40 倍)
*   限制为 1048576 行
*   带时区的日期时间序列化失败

更多关于[熊猫 IO 页面](https://pandas.pydata.org/pandas-docs/stable/user_guide/io.html#excel-files)的信息。

![](img/04681057e0e720e894d16d66607928dc.png)

excel 的性能测试结果。只有 54%的列保留了原始的数据类型，它占用了 CSV 的 90%的大小，但是写的时间多了 20 倍，读的时间多了 42 倍

# HDF5 和 to_hdf()

使用内部文件式结构的压缩格式，适用于巨大的异构数据。如果我们需要随机访问数据集的各个部分，这也是非常理想的。如果数据存储为`table` (PyTable)，您可以使用`store.select(key,where="A>0 or B<5")`直接查询 hdf 存储

```
# exporting a dataframe to hdf
df.to_hdf(path_or_buf, key, mode, complevel, complib, append ...)
```

有用的参数:

*   `path_or_buf` —文件路径或 HDFStore 对象
*   `key`—商场内的团体标识
*   `mode` —写入、附加或读取-附加
*   `format` — `fixed`用于快速写入和读取，而`table`允许仅选择数据的子集

HDF5 的优势:

*   对于某些数据结构，其大小和访问速度可能非常惊人

HDF5 的缺点:

*   数据帧可以非常大(甚至比 csv 大 300 倍)
*   HDFStore 对于写入不是线程安全的
*   `fixed`格式无法处理分类值

# **SQL 和 to_sql()**

将数据持久存储到数据库中通常很有用。像`sqlalchemy`这样的库致力于这项任务。

```
# Set up sqlalchemy engine
engine = create_engine(
    'mssql+pyodbc://user:pass@localhost/DB?driver=ODBC+Driver+13+for+SQL+server',
    isolation_level="REPEATABLE READ"
)# connect to the DB
connection = engine.connect()# exporting dataframe to SQL
df.to_sql(name="test", con=connection)
```

有用的参数:

*   `name` —SQL 表的名称
*   `con` —通常由`sqlalchemy.engine`连接引擎
*   `chunksize` —可以选择按 chunksize 批量加载数据

SQL 的优势:

*   比在磁盘上持久存储慢(读取 10 次/写入 5 次，但这可以优化)
*   所有程序员都能理解数据库

SQL 的缺点:

*   有些数据格式没有保留——类别、整数、浮点和时间增量
*   根据数据库的不同，性能可能会很慢
*   在某些情况下，您可能很难建立数据库连接

如果你想增加`.to_sql()`的写时间，试试 Kiran Kumar Chilla 在加速批量插入文章中描述的方法。

[](https://medium.com/analytics-vidhya/speed-up-bulk-inserts-to-sql-db-using-pandas-and-python-61707ae41990) [## 使用 Pandas 和 Python 加速 SQL db 的批量插入

### 本文详细介绍了:1 .使用 pandas 和 pyodbc 将数据帧写入数据库的不同方法 2。如何…

medium.com](https://medium.com/analytics-vidhya/speed-up-bulk-inserts-to-sql-db-using-pandas-and-python-61707ae41990) 

# 羽化和羽化()

[Feather](https://arrow.apache.org/docs/python/feather.html) 是一种轻量级格式，用于存储数据框和箭头表。这是如何存储数据的另一种选择，相对较快，文件较小。它没有包括在测量中，因为引擎在相当长的时间内锁定文件，很难重复几次性能测试。如果您计划将数据框持久化一次，feather 可能是一个理想的选择。

# 其他方法

熊猫提供了更多的坚持和阅读方法。我省略了 json 和 fix-width-file，因为它们具有类似 csv 的特征。可以尝试用`.to_gbq()`或者`stata`格式直接写到谷歌大查询。新的格式肯定会出现，以满足与各种云提供商通信的需求。

多亏了这篇文章，当我把一行程序复制到电子邮件、excel 或 google doc 时，我开始喜欢上了`.to_clipboard()`。

# 特性试验

许多方法都比 CSV 有优势，但是当 CSV 在世界范围内如此容易理解时，是否值得使用这些不寻常的方法。让我们来看看表演。

在性能测试期间，我关注 4 个关键指标:

*   `data type preservation` —读取后有多少%的列保持原始类型
*   `compression/size` —文件有多大，以 csv 的百分比表示
*   `write_time` —编写这种格式需要多长时间，占 csv 编写时间的%
*   `read_time` —读取此格式需要多长时间，占 csv 读取时间的%

为此，我准备了一个包含 50K 随机数、字符串、类别、日期时间和布尔值的数据集。数值范围来自 [numpy 数据类型概述](https://numpy.org/doc/stable/user/basics.types.html)。

```
data = []
for i in range(1000000):
    data.append(
        [random.randint(-127,127),  # int8
         random.randint(-32768,32767),  # int16
        ...
```

> 产生随机样本是几乎每个测试都要用到的技巧。

你可以在 [GitHub](https://github.com/vaclavdekanovsky/data-analysis-in-examples/blob/master/Pandas/Persistance/Stop%20Persisting%20Pandas%20to%20CSV.ipynb) 笔记本中查看生成随机字符串和日期的支持功能，这里我只提一个:

```
def get_random_string(length: int) -> str:
    """Generated random string up to the specific lenght"""

    letters = string.ascii_letters
    result_str = ''.join([random.choice(letters) for i in range(random.randint(3,length))])
    return result_str
```

生成数据帧的完整代码描述如下:

生成随机数据，并在 7 次迭代中测量读/写速度

一旦我们有了一些数据，我们希望通过不同的算法反复处理它们。您可以分别编写每个测试，但是让我们将测试压缩成一行:

```
# performance test
performance_df = performance_test(exporting_types)# results
performance_df.style.format("{:.2%}")
```

`performance_test` 函数接受一个带有测试定义的字典，如下所示:

```
d = { ..."parquet_fastparquet": {
        "type": "Parquet via fastparquet",
        "extension": ".parquet.gzip",
        "write_function": pd.DataFrame.to_parquet,
        "write_params": {"engine":"fastparquet","compression":"GZIP"},
        "read_function": pd.read_parquet,
        "read_params": {"engine":"fastparquet"}
    }
... }
```

字典包含应该运行的功能，例如`pd.DataFrame.to_parquet`和参数。我们迭代字典，一个接一个地运行函数:

```
path = "output_file"
# df is our performance test sample dataframe# persist the df
d["write_function"](df, path, **d["write_params"])# load the df 
df_loaded = d["read_function"](path, **d["read_params"]
```

我将结果存储到一个数据帧中，以便利用[。Express](/visualization-with-plotly-express-comprehensive-guide-eb5ee4b50b57) 用几行代码显示结果的能力:

```
# display the graph with the results
fig = pe.bar(performance_df.T, barmode='group', text="value")# format the labels
fig.update_traces(texttemplate='%{text:.2%}', textposition='auto')# add a title
fig.update_layout(title=f"Statistics for {dataset_size} records")
fig.show()
```

![](img/640850306a041b9a875754364cec9f95.png)

性能测试结果。数据格式保存测量成功百分比以及大小和速度与 csv 进行比较。

[](/visualization-with-plotly-express-comprehensive-guide-eb5ee4b50b57) [## 用 Plotly 可视化。快递:综合指南

### 一个数据集和 70 多个图表。交互性和动画通常只需一行代码。

towardsdatascience.com](/visualization-with-plotly-express-comprehensive-guide-eb5ee4b50b57) 

## 健全性检查

在随机样本上测试东西对于获得你的应用程序或工具有多好的第一印象是有用的，但是一旦它必须符合现实。为了避免任何意外，您应该在真实数据上尝试您的代码。我选择了我最喜欢的数据集——美国证券交易委员会季度数据转储——并对其进行性能测试。我已经获得了非常相似的结果，这使我相信我的假设不是完全错误的。

![](img/6ab88593c82c33dfdbd076441169dc02.png)

对 SEC 季度数据转储的 8 列进行健全性检查的结果证实了性能测试的结果。

[](/stock-fundamental-analysis-eda-of-secs-quarterly-data-summary-455e62ff4817) [## 股票基本面分析:SEC 季度数据汇总的 EDA

### 大熊猫 2020 SEC 申报的探索性数据分析

towardsdatascience.com](/stock-fundamental-analysis-eda-of-secs-quarterly-data-summary-455e62ff4817) 

# 结论

尽管 pickle 显然赢得了性能竞赛，但在一些用例中，您可能更愿意选择另一种格式。我们还有一个非常特殊的数据集，在真实数据上，性能可能会有所不同。

请随意使用[代码(GitHub)](https://github.com/vaclavdekanovsky/data-analysis-in-examples/blob/master/Pandas/Persistance/Stop%20Persisting%20Pandas%20to%20CSV.ipynb) ，并在您喜欢的数据集上尝试性能。对我个人来说，当我存储预处理数据集时，`.to_pickle()`很棒，因为我不必担心数据格式，我只需将`read_pickle()`和工作时间具体化到我的笔记本上。

[](/complete-guide-to-pythons-cross-validation-with-examples-a9676b5cac12) [## python 交叉验证的完整指南及示例

### sklearn 交叉验证的示例和使用案例解释了 k 折叠、洗牌、分层以及它如何影响…

towardsdatascience.com](/complete-guide-to-pythons-cross-validation-with-examples-a9676b5cac12) [](/is-something-better-than-pandas-when-the-dataset-fits-the-memory-7e8e983c4fe5) [## 当数据集符合内存时，是否有比熊猫更好的东西？

### 探索 Vaex，Dask，PySpark，摩丁和朱莉娅

towardsdatascience.com](/is-something-better-than-pandas-when-the-dataset-fits-the-memory-7e8e983c4fe5) ![](img/54771ec31d1a5d5d7cf200331a0fa4dd.png)

canva.com 制造

```
If you want to create inspiring infographics like the one above, use [canva.com](https://canva.7eqqol.net/WnbXZ) (affiliate link, when you click on it and purchase a product, you won't pay more, but I can receive a small reward; you can always write canva.com to your browser to avoid this). You can do more than infographic and some features are available for free.
```