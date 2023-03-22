# Pyspark 数据操作教程

> 原文：<https://towardsdatascience.com/pyspark-data-manipulation-tutorial-8c62652f35fa?source=collection_archive---------13----------------------->

## [入门](https://towardsdatascience.com/tagged/getting-started)

## 绝对火花初学者入门

![](img/e7cf53ecafd51108ff5c6447ce612e81.png)

图片由[免费提供-照片](https://pixabay.com/photos/?utm_source=link-attribution&amp;utm_medium=referral&amp;utm_campaign=image&amp;utm_content=839831)来自 [Pixabay](https://pixabay.com/?utm_source=link-attribution&amp;utm_medium=referral&amp;utm_campaign=image&amp;utm_content=839831)

## 这个教程适合你吗？

本教程是为有一些 Python 经验的数据人员编写的，他们绝对是 Spark 初学者。它将帮助您安装 Pyspark 并启动您的第一个脚本。您将了解弹性分布式数据集(rdd)和数据帧，它们是 Pyspark 中的主要数据结构。我们讨论一些基本的概念，因为我认为它们会让你以后不会感到困惑和调试。您将学习数据转换以及从文件或数据库中读取数据。

## 为什么火花

学习 Spark 的主要原因是，您将编写可以在大型集群中运行并处理大数据的代码。本教程只讨论 Python API py Spark，但是你应该知道 Spark APIs 支持 4 种语言:Java、Scala 和 R 以及 Python。由于 Spark core 是用 Java 和 Scala 编程的，所以那些 API 是最完整和最有原生感觉的。

Pyspark 的优势在于 Python 已经有了许多数据科学的库，您可以将它们插入到管道中。再加上 Python 摇滚的事实！！！能让 Pyspark 真正有生产力。例如，如果你喜欢熊猫，你知道你可以用一个方法调用将 Pyspark 数据帧转换成熊猫数据帧。然后你可以对数据做一些事情，并用 matplotlib 绘制出来。

随着时间的推移，你可能会发现 Pyspark 几乎和 pandas 或 sklearn 一样强大和直观，并在你的大部分工作中使用它。试试 Pyspark 吧，它可能会成为你职业生涯中的下一件大事。

# Pyspark 快速启动

有很多关于如何创建 Spark 集群、配置 Pyspark 向它们提交脚本等等的文章。所有这些都是在 Spark 上进行高性能计算所需要的。然而，在大多数公司中，他们会有数据或基础架构工程师来维护集群，而您只需编写和运行脚本。因此，您可能永远不会安装 Spark 集群，但是如果您需要，您可以在问题出现时处理它。OTOH，如果我告诉你，只需一个命令，你就可以在几分钟内开始工作，那会怎么样？

```
pip install pyspark
```

这将在您的机器上下载并安装 Pyspark，这样您就可以学习其中的诀窍，然后，当您得到一个集群时，使用它将会相对容易。

为了支持本教程，我创建了这个 GitHub 库:

[](https://github.com/ArmandoRiveroPi/pyspark_tutorial) [## ArmandoRiveroPi/py spark _ 教程

### pyspark GitHub 中的数据操作和机器学习介绍是超过 5000 万开发人员的家园，他们致力于…

github.com](https://github.com/ArmandoRiveroPi/pyspark_tutorial) 

示例中的大部分代码最好放在[tutorial _ part _ 1 _ data _ wrangling . py](https://github.com/ArmandoRiveroPi/pyspark_tutorial/blob/master/tutorial_part_1_data_wrangling.py)文件中。

在加快速度之前，先给你点提示。我发现如果我不在主 Python 脚本的开头设置一些环境变量，Pyspark 会抛出错误。

```
import sys, os
environment = ['PYSPARK_PYTHON', 'PYSPARK_DRIVER_PYTHON']
*for* var *in* environment:
    os.environ[var] = sys.executable
```

之后，您可以创建 spark 会话

```
*from* pyspark.sql *import* SparkSession
session = SparkSession.builder.getOrCreate()
```

相信我，这是你开始工作所需要的。

# 基本概念

## 火花建筑

好了，我们可以进入正题了。首先，你必须知道 Spark 是复杂的。它不仅包含大量用于执行许多数据处理任务的代码，而且在执行分布在数百台机器上的代码时，它有望高效无误。在开始的时候，你真的不需要这种复杂性，你需要的是把事情抽象出来，忘记那些具体的细节。不过，重要的是司机和工人之间的区别。驱动程序是 Python (single)流程，您在其中为工人发布订单。工人是听从司机命令的火花过程。您可能会认为这些工人“在云中”(Spark 集群)。Spark 假设大数据将分布在工人中，这些工人有足够的内存和处理能力来处理这些数据。预计驱动程序没有足够的资源来保存这么多的数据。这就是为什么你需要明确地说你什么时候想要移动数据到驱动程序。我们一会儿会谈到这一点，但要注意，在实际应用中，你应该非常小心你带给驱动程序的数据量。

**RDDs**

弹性分布式数据集是 Spark 中最重要的数据结构之一，是数据帧的基础。您可以将它们视为“分布式”阵列。在许多方面，它们的行为类似于列表，一些细节我们将在下面讨论。

那么，如何创建 RDD 呢？最直接的方法是“并行化”Python 数组。

```
rdd = session.sparkContext.parallelize([1,2,3])
```

要开始与您的 RDD 互动，请尝试以下操作:

```
rdd.take(num=2)
```

这将为驾驶员带来 RDD 的前两个值。*计数*方法将返回 RDD 的长度

```
rdd.count()
```

如果你想把所有的 RDD 数据作为一个数组发送给驱动程序，你可以使用 *collect*

```
rdd.collect()
```

但是要小心，正如我们之前说过的，在实际应用中，这可能会使驱动程序崩溃，因为 RDD 的大小可能是千兆字节。一般来说，更喜欢使用 *take* 传递您想要的行数，只有当您确定 RDD 或数据帧不太大时才调用 *collect* 。

## 数据帧

如果你了解熊猫或 R 数据帧，你会非常清楚 Spark 数据帧代表什么。它们用命名列表示表格(矩阵)数据。在内部，它们是通过行对象的 RDD 实现的，这有点类似于名为 tuple 的 Python。数据框架的最大优势在于，它们使您能够将 SQL 思维付诸实践。我们稍后将讨论数据帧操作，但是让我们开始创建一个数据帧，以便您可以使用它。

```
df = session.createDataFrame(
  [[1,2,3], [4,5,6]], [‘column1’, ‘column2’, ‘column3’]
)
```

首先是数据矩阵，然后是列名。

可以像 RDD 案一样，尝试*拿*、*数*、*收*的方法；take and collect 将为您提供一个行对象列表。但是对我来说，最友好的显示方式应该是 *show* :

```
df.show(n=3)
```

它将用前 n 行打印数据帧的表格表示。

## 不变

Python 列表的一个关键区别是 rdd(以及数据帧)是不可变的。在并发应用程序和函数式语言中，经常需要不可变数据。

让我们讨论一下 Python 中不变性的含义。假设你这样做:

```
a = list(range(10))
a.append(11)
```

在这里，解释器首先创建一个名称“a ”,它指向一个列表对象，在第二行中，它修改了同一个对象。然而，当你这样做的时候

```
st = “my string”
st += “ is pretty”
```

解释器首先创建指向字符串对象的名称“st”，然后创建一个带有“my string is pretty”的全新字符串对象(在内存中的不同位置)，并将名称“**ST”**指向该新对象。发生这种情况是因为字符串是不可变的，你不能就地修改它们。

同样，rdd 和数据帧也不能就地修改，所以当您这样做时

```
my_rdd.map(*lambda* *x*: x*100)
```

我的 _rdd 不变。你需要做什么

```
my_rdd = my_rdd.map(*lambda* *x*: x*100)
```

以获得指向转换后的 rdd 对象的名称“my_rdd”。

## **转换和动作**

与普通 Python 相比，一个更令人震惊的区别可能会让您在开始时感到困惑。有时您会注意到一个非常繁重的操作会立即发生。但是后来你做了一些小事情(比如打印 RDD 的第一个值),这似乎要花很长时间。

让我试着把这个问题简化很多来解释。在 Spark 中，**变换**和**动作**是有区别的。当你改变一个数据帧时，那是一个**转换**，然而，当你实际使用数据时(例如 df.show(1))，那是一个**动作**。转换是延迟加载的，当你调用它们时它们不会运行。当您通过操作使用它们的结果时，它们就会被执行。然后，所有需要的转换将被一起计划、优化和运行。所以当你看到看起来瞬时的操作时，即使它们很重，那也是因为它们是转换，它们将在以后运行。

# 数据帧操作

## 用户定义函数

用户定义的函数允许您使用 Python 代码对数据帧像元进行操作。你创建一个常规的 Python 函数，把它包装在一个 UDF 对象中，并把它传递给 Spark，它会让你的函数在所有的工作器中可用，并安排它的执行来转换数据。

```
*import* pyspark.sql.functions *as* funcs
*import* pyspark.sql.types *as* typesdef multiply_by_ten(number):
    return number*10.0multiply_udf = funcs.udf(multiply_by_ten, types.DoubleType())transformed_df = df.withColumn(
    'multiplied', multiply_udf('column1')
)
transformed_df.show()
```

首先你创建一个 Python 函数，它可以是一个对象中的方法，也是一个函数。然后创建一个 UDF 对象。恼人的部分是您需要定义输出类型，这是我们在 Python 中不太习惯的。要真正有效地使用 UDF，你需要学习这些类型，特别是复合映射类型(如字典)和数组类型(如列表)。这样做的好处是，您可以将这个 UDF 传递给 dataframe，告诉它将对哪一列进行操作，这样您就可以在不离开旧 Python 的舒适环境的情况下完成奇妙的事情。

然而，UDF 的一个主要限制是，尽管[它们可以接受几列作为输入](https://stackoverflow.com/questions/42540169/pyspark-pass-multiple-columns-in-udf)，但是它们不能整体改变行。如果您想要处理整行，您将需要 RDD 地图。

## RDD 制图

这很像使用 UDF，你也传递一个常规的 Python 函数。但是在这种情况下，该函数将接收一个完整的行对象，而不是列值。预计它也将返回一整行。这将赋予您对行的最终权力，但有几个注意事项。首先:Row 对象是不可变，所以您需要创建一个全新的行并返回它。第二:你需要将数据帧转换成 RDD，然后再转换回来。幸运的是，这些问题都不难克服。

让我向您展示一个函数，它将对数转换您的数据帧中的所有列。作为一个不错的尝试，它还将每个列名转换为“log(column_name)”。我发现最简单的方法是用 row.asDict()将行放入字典。这个函数有点 Python 的魔力，比如字典理解和关键字参数打包(双星形)。希望你能全部理解。

```
*import* pyspark.sql.types *as* types*def* take_log_in_all_columns(row: types.Row):
     old_row = row.asDict()
     new_row = {f'log({column_name})': math.log(value) 
                *for* column_name, value *in* old_row.items()}
     *return* types.Row(**new_row)
```

这本身不会做任何事情。你需要执行地图。

```
logarithmic_dataframe = df.rdd.map(take_log_in_all_columns).toDF()
```

您会注意到这是一个链式方法调用。首先调用 rdd，它将为您提供存储数据帧行的底层 RDD。然后在这个 RDD 上应用 map，在这里传递函数。要关闭，您可以调用 toDF()，它将行的 RDD 转换为数据帧。进一步讨论见[这个栈溢出问题](https://stackoverflow.com/questions/39699107/spark-rdd-to-dataframe-python)。

## SQL 操作

由于数据帧表示表，自然地，它们被赋予了类似 SQL 的操作。为了让你兴奋，我将只提到其中的几个，但是你可以期待找到几乎同构的功能。调用 *select* 将返回一个只有一些原始列的数据帧。

```
df.select('column1', 'column2')
```

对*的调用，其中*将返回一个数据帧，其中只有列 1 的值为 3 的行。

```
df.where('column1 = 3')
```

这个对 *join* 的调用将返回一个 dataframe，也就是，嗯…，一个 df 和 df1 通过 column1 的连接，就像 SQL 中的内部连接一样。

```
df.join(df1, [‘column1’], how=’inner’)
```

如果您需要执行右或左连接，在我们的例子中，df 就像左边的表，而 df1 就是右边的表。外部连接也是可能的。

但是除此之外，在 Spark 中，您可以更直接地执行 SQL。您可以从数据帧中创建时态视图

```
df.createOrReplaceTempView(“table1”)
```

然后在视图上执行查询

```
df2 = session.sql("SELECT column1 AS f1, column2 as f2 from table1")
```

这些查询将返回一个具有相应列名和值的新数据帧。

## 数据帧列操作

列抽象支持像算术这样的直接操作，例如，假设您想将列 1 相加，再加上列 2 和列 3 的乘积。那你可以

```
df3 = df.withColumn(
    'derived_column', df['column1'] + df['column2'] * df['column3']
)
```

## **聚合和快速统计**

对于这个例子，我们需要从一个 CSV 文件中读取。我从[https://archive.ics.uci.edu/ml/datasets/adult](https://archive.ics.uci.edu/ml/datasets/adult)下载了成人数据文件

我将文件移动到我的[存储库](https://github.com/ArmandoRiveroPi/pyspark_tutorial)中的*数据*文件夹，并将其重命名为成人.数据. csv。

```
ADULT_COLUMN_NAMES = [
     "age",
     "workclass",
     "fnlwgt",
     "education",
     "education_num",
     "marital_status",
     "occupation",
     "relationship",
     "race",
     "sex",
     "capital_gain",
     "capital_loss",
     "hours_per_week",
     "native_country",
     "income"
 ]
```

读取文件后，我使用这个列表设置列名，因为文件没有标题。

```
csv_df = session.read.csv(
     'data/adult.data.csv', header=*False*, inferSchema=*True*
 )*for* new_col, old_col *in zip*(ADULT_COLUMN_NAMES, csv_df.columns):
     csv_df = csv_df.withColumnRenamed(old_col, new_col)
```

之后，您可以尝试下面的一些快速描述性统计

```
csv_df.describe().show()
```

若要获取聚合，请将 groupBy 与 agg 方法一起使用。例如，下面将为您提供一个数据框架，其中包含按年龄组划分的平均工作时间和标准偏差。我们也按年龄对数据帧进行排序。

```
work_hours_df = csv_df.groupBy(
    'age'
).agg(
    funcs.avg('hours_per_week'),
    funcs.stddev_samp('hours_per_week')
).sort('age')
```

# 连接到数据库

您很可能将数据保存在关系数据库中，由 MySQL 或 PostgreSQL 之类的 RDBMS 处理。如果是这样的话，不要担心，Spark 有办法与多种数据存储进行交互。我敢打赌，你可以通过谷歌搜索到你可能有的大多数 IO 需求。

要连接到 RDBMS，您需要一个 JDBC 驱动程序，这是一个 Spark 可以用来与数据库对话的 jar 文件。我将向您展示一个 PostgreSQL 示例。

首先你需要下载[驱动](https://jdbc.postgresql.org/download.html)。

在我的 GitHub repo 中，我把 JDBC 驱动程序放在 bin 文件夹中，但是你可以在任何路径中找到它。此外，您应该知道本例中的代码位于 repo 中的另一个[文件](https://github.com/ArmandoRiveroPi/pyspark_tutorial/blob/master/tutorial_part_1_reading_from_database.py)中。我必须这样做，因为与其他文件不同，这个文件会抛出错误，因为数据库凭证当然是假的。您需要用您的数据库来替换它们以使其运行。

现在，您将通过一个额外的步骤启动 Pyspark:

```
session = SparkSession.builder.config(
    'spark.jars', 'bin/postgresql-42.2.16.jar'
).config(
    'spark.driver.extraClassPath', 'bin/postgresql-42.2.16.jar'
).getOrCreate()
```

这将创建一个可识别驱动程序的会话。然后，您可以像这样从数据库中读取(用您的真实配置替换假配置):

```
url = f"jdbc:postgresql://your_host_ip:5432/your_database"
properties = {'user': 'your_user', 'password': 'your_password'}
*# read from a table into a dataframe* df = session.read.jdbc(
    url=url, table='your_table_name', properties=properties
)
```

然后，您可以以任何方式创建转换后的数据帧，并将数据写回数据库(可能在不同的表中)。

```
transformed_df.write.jdbc(
    url=url, table='new_table', mode='append', properties=properties
)
```

根据文档，写入模式有:

*   append:将此数据帧的内容追加到现有数据中。
*   覆盖:覆盖现有数据。
*   忽略:如果数据已经存在，则忽略此操作。
*   error 或“errorifexists”(默认情况):如果数据已经存在，则抛出异常。

# 结论

我希望你受到鼓励，马上开始学习 Pyspark。我的意思是，这将需要几周的时间来提高效率，这取决于你每天能花多少时间，但之后你就可以用 Pyspark 来执行大量的数据处理了。万一你不能选择你在工作中使用的，至少你可以在你的空闲时间学习它。我的介绍几乎没有触及表面，主要是希望给你一个诱人的咬。比熊猫或者 sklearn 难不了多少。

熟练掌握 Spark，它可以为您打开大数据的大门。

第 2 部分将介绍基本的分类和回归。

# 进一步阅读

*PySpark 食谱*作者 Raju Kumar Mishra。Apress，2018。

官方文件

【https://spark.apache.org/docs/latest/api/python/ 