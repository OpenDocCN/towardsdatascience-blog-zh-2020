# 没什么特别的——但是 16 个必要的操作让你开始接触熊猫

> 原文：<https://towardsdatascience.com/nothing-fancy-but-16-essential-operations-to-get-you-started-with-pandas-5b0c2f649068?source=collection_archive---------33----------------------->

## 使用 Pandas 的基本数据处理技能概述

![](img/1eeea1c602cbbbd992b15294b65a5308.png)

照片由[卡伦·艾姆斯利](https://unsplash.com/@kalenemsley?utm_source=medium&utm_medium=referral)在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄

Python 已经成为许多数据科学家和机器学习研究人员的首选编程语言。熊猫图书馆是他们做出这一选择的一个重要的数据处理工具。当然，pandas 库是如此通用，以至于它可以用于几乎所有的初始数据操作，以便为进行统计分析或建立机器学习模型准备好数据。

然而，对于同样的多功能性，开始舒适地使用它可能会令人不知所措。如果你正在纠结如何开始，这篇文章很适合你。本文的目的不是介绍太多而失去要点，而是概述您希望在日常数据处理任务中使用的关键操作。每个关键操作都有一些需要考虑的重要参数。

当然，第一步是安装熊猫，你可以按照这里的说明[使用`pip`或`conda`。在编码环境方面，我推荐 Visual Studio Code，JupyterLab，或者 Google Colab，所有这些都需要很少的努力就可以设置好。当安装完成后，在您首选的编码环境中，您应该能够将 pandas 导入到您的项目中。](https://pandas.pydata.org/pandas-docs/stable/getting_started/install.html)

```
import pandas as pd
```

如果您运行上面的代码没有遇到任何错误，您就可以开始了。

## 1.读取外部数据

在大多数情况下，我们从外部来源读取数据。如果我们的数据是类似电子表格的格式，下面的函数应该可以满足目的。

```
*# Read a comma-separated file* df = pd.read_csv(**"the_data.csv"**)*# Read an Excel spreadsheet* df = pd.read_excel(**"the_data.xlsx"**)
```

*   应该正确处理标题。默认情况下，读数将假定第一行数据是列名。如果它们没有标题，你必须指定它(如`header=None`)。
*   如果你正在读取一个制表符分隔的文件，你可以通过指定制表符作为分隔符来使用`read_csv`(例如`sep=“\t”`)。
*   当你读一个大文件时，读一小部分数据是一个好主意。在这种情况下，您可以设置要读取的行数(例如`nrows=1000`)。
*   如果您的数据涉及日期，您可以考虑设置参数使日期正确，例如`parse_dates`和`infer_datetime_format`。

## 2.创建系列

在清理数据的过程中，您可能需要自己创建*系列*。在大多数情况下，您只需传递一个 iterable 来创建一个*系列*对象。

```
*# Create a Series from an iterable* integers_s = pd.Series(range(10))# Create a Series from a dictionary object
squares = {x: x*x for x in range(1, 5)}
squares_s = pd.Series(squares)
```

*   您可以通过设置`name`参数为*系列*对象指定一个名称。如果该名称成为 *DataFrame* 对象的一部分，它将成为该名称。
*   如果您发现索引比默认的基于 0 的索引更有用，您也可以将索引分配给*系列*(例如，设置`index`参数)。请注意，索引的长度应该与数据的长度相匹配。
*   如果你从一个*字典*创建一个*系列*对象，这些键将成为索引。

## 3.构建数据框架

通常，您需要使用 Python 内置对象创建 *DataFrame* 对象，比如列表和字典。下面的代码片段突出了两个常见的使用场景。

```
*# Create a DataFrame from a dictionary of lists as values* data_dict = {**'a'**: [1, 2, 3], **'b'**: [4, 5, 6], **'c'**: [7, 8, 9]}
data_df0 = pd.DataFrame(data_dict)

*# Create a DataFrame from a list* data_list = [[1, 4, 7], [2, 5, 8], [3, 6, 9]]
data_df1 = pd.DataFrame(data_list, columns=tuple(**'abc'**))
```

*   第一个使用了一个*字典*对象。它的键将成为列名，而它的值将成为相应列的值。
*   第二个使用了一个*列表*对象。与前面的方法不同，构造的*数据帧*将按行使用数据，这意味着每个内部列表将成为创建的*数据帧*对象的一行。

## 4.数据帧概述

当我们有了要处理的数据框架时，我们可能想看看 30，000 英尺高度的数据集。您可以使用几种常见的方法，如下所示。

```
*# Find out how many rows and columns the DataFrame has* df.shape

*# Take a quick peak at the beginning and the end of the data* df.head()
df.tail()*# Get a random sample* df.sample(5)*# Get the information of the dataset* df.info()*# Get the descriptive stats of the numeric values* df.describe()
```

*   检查头部和尾部非常重要，尤其是在处理大量数据时，因为您希望确保所有数据都已被完整读取。
*   `info()`函数将根据数据类型和项目计数给出列的概述。
*   获取数据的随机样本来检查数据的完整性也很有趣(即`sample()`函数)。

## 5.重命名列

您注意到数据的某些列没有太多意义，或者名称太长而无法使用，您想要重命名这些列。

```
*# Rename columns using mapping* df.rename({**'old_col0'**: **'col0'**, **'old_col1'**: **'col1'**}, axis=1)

*# Rename columns by specifying columns directly* df.rename(columns={**'old_col0'**: **'col0'**, **'old_col1'**: **'col1'**})
```

*   如果您只是提供一个映射对象(例如， *dict* )，那么您需要指定`axis=1`来重命名列。
*   或者，您可以显式地将映射对象指定给`columns`参数。
*   默认情况下,`rename`函数将创建一个新的数据帧。如果你想就地重命名*数据帧*，你需要指定`inplace=True`。

## 6.排序数据

为了使你的数据更加结构化，你需要对 *DataFrame* 对象进行排序。

```
*# Sort data* df.sort_values(by=[**'col0'**, **'col1'**])
```

*   默认情况下，`sort_values`函数将对您的行进行排序(`axis=0`)。在大多数情况下，我们使用列作为排序关键字。
*   默认情况下，`sort_values`函数将创建一个排序后的*数据帧*对象。要改变它，使用`inplace=True`。
*   默认情况下，所有排序关键字的排序都基于升序。如果要使用降序，请指定`ascending=False`。如果您想要混合排序(例如，一些键是升序，一些键是降序)，您可以创建一个布尔值列表来匹配键的数量，比如`by=[‘col0’, ‘col1’, ‘col2’], ascending=[True, False, True]`。
*   原始索引将包含它们的旧数据行。在许多情况下，您需要重新索引。不用直接调用 rese `t` _index 函数，可以指定`ignore_index`为`True`，排序完成后会为你重置索引。

## 7.处理重复

在现实生活的数据集中，包含重复记录是很常见的情况，要么是人为错误，要么是数据库故障。我们希望删除这些重复项，因为它们会在以后导致意想不到的问题。

```
*# To examine whether there are duplicates using all columns* df.duplicated().any()

*# To examine whether there are duplicates using particular columns* df.duplicated([**'col0'**, **'col1'**]).any()
```

上述函数将返回一个布尔值，告诉您数据集中是否存在任何重复的记录。要找出重复记录的确切数量，您可以使用`sum()`函数，利用从`duplicated()`函数返回的布尔值*系列*对象(Python 将`True`视为值 1)，如下所示。另外需要注意的是，当参数`keep`被设置为`False`时，它会将任何重复标记为`True`。假设有三条重复记录，当`keep=False`时，两条记录都会被标记为`True`(即正在重复)。当`keep= “first”`或`keep=“last”`时，只有第一条或最后一条记录标记为`True`。

```
*# To find out the number of duplicates* df.duplicated().sum()
df.duplicated(keep=False).sum()
```

为了实际查看复制的记录，您需要使用生成的复制的*系列*对象从原始数据集中选择数据，如下所示。

```
*# Get the duplicate records* duplicated_indices = df.duplicated([**'col0'**, **'col1'**], keep=False)duplicates = df.loc[duplicated_indices, :].sort_values(by=[**'col0'**, **'col1'**], ignore_index=True)
```

*   我们通过将`keep`参数设置为`False`来获得所有重复的记录。
*   为了更好地查看重复记录，您可能希望使用相同的一组键对生成的*数据帧*进行排序。

一旦您对数据集的重复记录有了一个好的想法，您可以像下面这样删除它们。

```
*# Drop the duplicate records* df.drop_duplicates([**'col0'**, **'col1'**], keep=**"first"**, inplace=True, ignore_index=True)
```

*   默认情况下，保存的记录将是第一个副本。
*   如果您想就地更新*数据帧*，您需要指定`inplace=True`。BTW:许多其他函数都有这个选项，即使不是全部，大多数时候也不会讨论这个问题。
*   与`sort_values()`函数一样，您可能希望通过指定`ignore_index`参数来重置索引(熊猫 1.0 中的一个新特性)。

## 8.处理丢失的数据

缺失数据在现实生活的数据集中很常见，这可能是由于测量值不可用或只是人为数据输入错误导致无意义的数据被视为缺失。为了对您的数据集有多少缺失值有一个总体的概念，您已经看到 info()函数告诉我们每一列有多少非空值。我们可以通过更结构化的方式获得关于数据缺失的信息，如下所示。

```
*# Find out how many missing values for each column* df.isnull().sum()

*# Find out how many missing values for the entire dataset* df.isnull().sum().sum()
```

*   `isnull()`函数创建一个与原始*数据帧*形状相同的*数据帧*，每个值表示原始值缺失(`True`)或不缺失(`False`)。相关的注意事项是，如果您想生成一个指示非空值的数据帧，您可以使用`notnull()`函数。
*   如前所述，Python 中的`True`值在算术上等于 1。`sum()`函数将为每一列计算这些布尔值的总和(默认情况下，它按列计算总和)，这反映了缺失值的数量。

通过对数据集的缺失有所了解，我们通常想要处理它们。可能的解决方案包括删除带有任何缺失值的记录，或者用适用的值填充它们。

```
*# Drop the rows with any missing values* df.dropna(axis=0, how=**"any"**)

*# Drop the rows without 2 or more non-null values* df.dropna(thresh=2)

*# Drop the columns with all values missing* df.dropna(axis=1, how=**"all"**)
```

*   默认情况下，`dropna()`函数按列工作(即`axis=0`)。如果指定`how=“any”`，将会删除任何缺少值的行。
*   当您设置`thresh`参数时，它要求行(或当`axis=1`时的列)具有非缺失值的数量。
*   与许多其他功能一样，当您设置`axis=1`时，您正在按列执行操作。在这种情况下，上面的函数调用将删除那些所有值都丢失的列。

除了删除带有缺失值的数据行或列的操作之外，还可以用一些值来填充缺失值，如下所示。

```
*# Fill missing values with 0 or any other value is applicable* df.fillna(value=0)*# Fill the missing values with customized mapping for columns* df.fillna(value={**"col0"**: 0, **"col1"**: 999})*# Fill missing values with the next valid observation* df.fillna(method=**"bfill"**)
*# Fill missing values with the last valid observation* df.fillna(method=**"ffill"**)
```

*   要用指定的值填充缺失的值，您可以将 value 参数设置为所有值的固定值，也可以设置一个 *dict* 对象，该对象将指示基于每列的填充。
*   或者，您可以通过使用缺失孔周围的现有观测值来填充缺失值，回填或向前填充。

## 9.分组描述性统计

当您进行机器学习研究或数据分析时，通常需要对一些分组变量执行特定操作。在这种情况下，我们需要使用`groupby()`函数。下面的代码片段向您展示了一些适用的常见场景。

```
*# Get the count by group, a 2 by 2 example* df.groupby([**'col0'**, **'col1'**]).size()

*# Get the mean of all applicable columns by group* df.groupby([**'col0'**]).mean()

*# Get the mean for a particular column* df.groupby([**'col0'**])[**'col1'**].mean()

*# Request multiple descriptive stats* df.groupby([**'col0'**, **'col1'**]).agg({
    **'col2'**: [**'min'**, **'max'**, **'mean'**],
    **'col3'**: [**'nunique'**, **'mean'**]
})
```

*   默认情况下，`groupby()`函数将返回一个 *GroupBy* 对象。如果你想把它转换成一个*数据帧*，你可以调用对象上的`reset_index()`。或者，您可以在`groupby()` 函数调用中指定`as_index=False`来直接创建一个*数据帧*。
*   如果您想知道每组的频率，则`size()`很有用。
*   `agg()`功能允许您生成多个描述性统计数据。您可以简单地传递一组函数名，这将应用于所有列。或者，您可以传递一个带有函数的 *dict* 对象来应用于特定的列。

## 10.宽到长格式转换

根据数据的收集方式，原始数据集可能是“宽”格式，每一行代表一个具有多个测量值的数据记录(例如，研究中某个主题的不同时间点)。如果我们想要将“宽”格式转换为“长”格式(例如，每个时间点变成一个数据行，因此一个主题有多行)，我们可以使用`melt()`函数，如下所示。

从宽到长的转换

*   `melt()`函数本质上是“取消透视”一个数据表(我们接下来将讨论透视)。您将`id_vars`指定为在原始数据集中用作标识符的列。
*   使用包含值的列设置`value_vars`参数。默认情况下，这些列将成为融合数据集中`var_name`列的值。

## 11.长到宽格式转换

与`melt()`功能相反的操作叫做旋转，我们可以用`pivot()`功能来实现。假设创建的“宽”格式数据帧被称为`df_long`。下面的函数向您展示了如何将宽格式转换为长格式——基本上与我们在上一节中所做的过程相反。

从长到宽的转变

除了`pivot()`函数之外，一个密切相关的函数是`pivot_table()`函数，它比`pivot()`函数更通用，允许重复的索引或列(更详细的讨论见此处的)。

## 12.选择数据

当我们处理一个复杂的数据集时，我们需要根据一些标准为特定的操作选择数据集的一个子集。如果您选择一些列，下面的代码将向您展示如何操作。所选数据将包括所有行。

```
*# Select a column* df_wide[**'subject'**]

*# Select multiple columns* df_wide[[**'subject'**, **'before_meds'**]]
```

如果要选择包含所有列的特定行，请执行下列操作。

```
*# Select rows with a specific condition* df_wide[df_wide[**'subject'**] == 100]
```

如果你想选择某些行和列，我们应该考虑使用`iloc`或`loc`方法。这些方法的主要区别在于`iloc`方法使用基于 0 的索引，而`loc`方法使用标签。

数据选择

*   以上几对调用创建了相同的输出。为了清楚起见，只列出了一个输出。
*   当您将 slice 对象与`iloc`一起使用时，不包括停止索引，就像常规的 Python slice 对象一样。然而，切片对象在`loc`方法中包含停止索引。请参见第 15–17 行。
*   如第 22 行所示，当您使用布尔数组时，您需要使用实际值(使用 values 方法，这将返回底层 numpy 数组)。如果不这样做，您可能会遇到下面的错误:`NotImplementedError: iLocation based boolean indexing on an integer type is not available`。
*   在选择行方面，`loc`方法中标签的使用恰好与索引相同，因为索引与索引标签同名。换句话说，`iloc`将始终根据位置使用基于 0 的索引，而不管索引的数值。

## 13.使用现有数据的新列(映射和应用)

现有列并不总是以我们想要的格式显示数据。因此，我们经常需要使用现有数据生成新列。这种情况下有两个函数特别有用:`map()`和`apply()`。我们有太多的方法可以使用它们来创建新的列。例如，`apply()`函数可以有一个更复杂的映射函数，它可以创建多个列。我将向您展示两个最常用的案例，并遵循以下经验法则。让我们的目标保持简单——只需用任一用例创建一个列。

*   如果您的数据转换只涉及一列，只需对该列使用`map()`函数(本质上，它是一个*系列*对象)。
*   如果您的数据转换涉及多列，请使用`apply()`功能。

映射并应用

*   在这两种情况下，我都使用了 lambda 函数。但是，您可以使用常规函数。还可以为`map()`函数提供一个 *dict* 对象，它将基于键-值对将旧值映射到新值，其中键是旧值，值是新值。
*   对于`apply()`函数，当我们创建一个新列时，我们需要指定`axis=1`，因为我们是按行访问数据的。
*   对于 apply()函数，所示的例子是为了演示的目的，因为我可以使用原来的列做一个更简单的算术减法，如下所示:`df_wide[‘change’] = df_wide[‘before_meds’] —df_wide[‘after_meds’]`。

## 14.连接和合并

当我们有多个数据集时，有必要不时地将它们放在一起。有两种常见的情况。第一种情况是当您有相似形状的数据集，或者共享相同的索引或者相同的列时，您可以考虑直接连接它们。下面的代码向您展示了一些可能的连接。

```
*# When the data have the same columns, concatenate them vertically* dfs_a = [df0a, df1a, df2a]
pd.concat(dfs_a, axis=0)

*# When the data have the same index, concatenate them horizontally* dfs_b = [df0b, df1b, df2b]
pd.concat(dfs_b, axis=1)
```

*   默认情况下，串联执行“外部”连接，这意味着如果有任何不重叠的索引或列，它们都将被保留。换句话说，这就像创建两个集合的并集。
*   另一件要记住的事情是，如果您需要连接多个*数据帧*对象，建议您创建一个列表来存储这些对象，并且如果您按顺序执行连接，通过避免生成中间*数据帧*对象，只执行一次连接。
*   如果您想重置串联数据帧的索引，您可以设置`ignore_index=True`参数。

另一种情况是合并具有一个或两个重叠标识符的数据集。例如，一个*数据帧*有身份证号、姓名和性别，另一个有身份证号和交易记录。您可以使用 id 号列合并它们。下面的代码向您展示了如何合并它们。

```
*# Merge DataFrames that have the same merging keys* df_a0 = pd.DataFrame(dict(), columns=[**'id'**, **'name'**, **'gender'**])
df_b0 = pd.DataFrame(dict(), columns=[**'id'**, **'name'**, **'transaction'**])
merged0 = df_a0.merge(df_b0, how=**"inner"**, on=[**"id"**, **"name"**])

*# Merge DataFrames that have different merging keys* df_a1 = pd.DataFrame(dict(), columns=[**'id_a'**, **'name'**, **'gender'**])
df_b1 = pd.DataFrame(dict(), columns=[**'id_b'**, **'transaction'**])
merged1 = df_a1.merge(df_b1, how=**"outer"**, left_on=**"id_a"**, right_on=**"id_b"**)
```

*   当两个 DataFrame 对象共享相同的一个或多个键时，可以使用`on`参数简单地指定它们(一个或多个都可以)。
*   当它们有不同的名称时，您可以指定哪个用于左*数据帧*和哪个用于右*数据帧*。
*   默认情况下，合并将使用内部联接方法。当您希望有其他连接方法(例如，left、right、outer)时，您可以为`how`参数设置适当的值。

## 15.删除列

虽然您可以通过重命名数据帧中的所有列而不产生任何冲突，但有时您可能希望删除一些列以保持数据集的整洁。在这种情况下，你应该使用`drop()`功能。

```
*# Drop the unneeded columns* df.drop([**'col0'**, **'col1'**], axis=1)
```

*   默认情况下，`drop()`函数使用标签来引用列或索引，因此您可能希望确保标签包含在 *DataFrame* 对象中。
*   要删除索引，您可以使用`axis=0`。如果您删除列，我发现它们更常见，您可以使用`axis=1`。
*   同样，该操作创建了一个*数据帧*对象，如果您喜欢更改原始的*数据帧*，您可以指定`inplace=True`。

## 16.写入外部文件

当您希望与合作者或队友交流数据时，您需要将 DataFrame 对象写入外部文件。在大多数情况下，逗号分隔的文件应该可以满足要求。

```
*# Write to a csv file, which will keep the index* df.to_csv(**"filename.csv"**)

*# Write to a csv file without the index* df.to_csv(**"filename.csv"**, index=False)

*# Write to a csv file without the header* df.to_csv(**"filename.csv"**, header=False)
```

*   默认情况下，生成的文件将保留索引。您需要指定`index=False`来从输出中删除索引。
*   默认情况下，生成的文件将保留标题(例如，列名)。您需要指定`header=False`来移除标题。

## 结论

在本文中，我们回顾了一些基本操作，您会发现它们对您开始使用 pandas 库很有用。正如文章标题所示，这些技术并不打算以一种奇特的方式处理数据。相反，它们都是允许你以你想要的方式处理数据的基本技术。稍后，您可能会找到更好的方法来完成一些操作。