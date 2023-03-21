# 熊猫加入 vs .合并

> 原文：<https://towardsdatascience.com/pandas-join-vs-merge-c365fd4fbf49?source=collection_archive---------2----------------------->

![](img/e1899f8172e9f80df8e1a740975bf4c1.png)

罗马卡夫在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄的照片

## 它们有什么作用，我们应该在什么时候使用它们？

我写了很多关于统计和算法的文章，但是为建模准备数据也是数据科学的一个重要部分。事实上，很有可能你会花更多的时间盯着你的数据，检查它，修补它的漏洞，而不是训练和调整你的模型。

因此，我们在收集、清理和对数据执行快速“健全性检查”分析方面做得越好，我们就能在建模上花费越多的时间(大多数人认为这更有趣)。为此，让我们看看如何快速组合来自不同数据框架的数据，并为分析做好准备。

# 数据

让我们假设我们是一家生产和销售回形针的公司的分析师。我们需要运行一些关于我们公司销售部门的报告，以了解他们的工作情况，并从以下字典中获得数据:

```
import numpy as np
import pandas as pd# Dataframe of number of sales made by an employee
sales = {'Tony': 103,
         'Sally': 202,
         'Randy': 380,
         'Ellen': 101,
         'Fred': 82
        }# Dataframe of all employees and the region they work in
region = {'Tony': 'West',
          'Sally': 'South',
          'Carl': 'West',
          'Archie': 'North',
          'Randy': 'East',
          'Ellen': 'South',
          'Fred': np.nan,
          'Mo': 'East',
          'HanWei': np.nan,
         }
```

我们可以从字典中创建两个独立的数据帧，如下所示:

```
# Make dataframes
sales_df = pd.DataFrame.from_dict(sales, orient='index', 
                                  columns=['sales'])
region_df = pd.DataFrame.from_dict(region, orient='index', 
                                   columns=['region'])
```

数据框架 sales_df 现在看起来像这样:

```
sales
Tony     103
Sally    202
Randy    380
Ellen    101
Fred      82
```

region_df 如下所示:

```
 region
Tony     West
Sally   South
Carl     West
Archie  North
Randy    East
Ellen   South
Fred      NaN
Mo       East
HanWei    NaN
```

# 我应该合并，加入，还是连接？

现在，让我们将所有数据合并到一个数据帧中。但是我们怎么做呢？

熊猫数据框架有很多类似 SQL 的功能。事实上，与 SQL 表相比，我更喜欢它们(全世界的数据分析师都对我虎视眈眈)。但是当我第一次开始用 Pandas 做许多类似 SQL 的东西时，我发现自己总是不确定是使用 **join** 还是 **merge** ，并且我经常互换使用它们(选择首先想到的)。那么我们应该在什么时候使用这些方法，它们之间到底有什么不同呢？好了，是时候不再困惑了！

# **加入**

[***(如果你不熟悉什么是连接表，我写了这篇关于它的文章，我强烈建议你先读一读)***](/sql-joins-a-brief-example-9d015868b56a)

先从**加入**开始吧，因为这是最简单的一个。数据帧有一个叫做索引的东西。这是您的表的关键，如果我们知道索引，那么我们可以使用**轻松地获取保存数据的行。loc** 。如果你打印你的数据帧，你可以通过查看最左边的一栏来看索引是什么，或者我们可以更直接地使用**。索引**:

```
In:  sales_df.indexOut: Index(['Tony', 'Sally', 'Randy', 'Ellen', 'Fred'], 
           dtype='object')
```

所以 sales_df 的指标就是我们销售人员的名字。顺便说一下，与 SQL 表的主键不同，数据帧的索引不必是唯一的。但是一个唯一的索引使我们的生活更容易，搜索数据帧的时间更短，所以它绝对是一个好东西。给定一个索引，我们可以像这样找到行数据:

```
In:  sales_df.loc['Tony']Out: sales    103
     Name: Tony, dtype: int64
```

好了，回到**加入**。 **join** 方法获取两个数据帧，**在它们的索引**上将它们连接起来(从技术上来说，您可以为左边的数据帧选择要连接的列)。让我们看看当我们通过 **join** 方法将两个数据帧组合在一起时会发生什么:

```
In:  joined_df = region_df.join(sales_df, how='left')
     print(joined_df)Out:        region  sales
Tony     West  103.0
Sally   South  202.0
Carl     West    NaN
Archie  North    NaN
Randy    East  380.0
Ellen   South  101.0
Fred      NaN   82.0
Mo       East    NaN
HanWei    NaN    NaN
```

结果看起来像一个 SQL 连接的输出，它或多或少是这样的。 **join** 方法使用索引或它所调用的数据帧中的指定列，也就是左边的数据帧，作为连接键。因此，我们为左侧数据帧匹配的列不一定是它的索引。**但是对于正确的数据帧，连接键必须是它的索引。**我个人觉得更容易把 **join** 方法理解为基于索引的连接，如果我不想在索引上连接，就使用 **merge** (即将出现)。

在组合数据框架中有一些 nan。那是因为不是所有的员工都有销售。没有销售的那些在 sales_df 中不存在，但是我们仍然显示它们，因为**我们执行了一个左连接(通过指定“how=left”)，它从左数据帧 region_df 返回所有的行，不管是否有匹配**。如果我们不想在我们的连接结果中显示任何 nan，我们将改为执行内部连接(通过指定“how=inner”)。

# 合并

基本上，**合并**和**加入**差不多。这两种方法都用于将两个数据帧组合在一起，但 merge 更通用，代价是需要更详细的输入。让我们看看如何使用**合并**创建与使用**连接**相同的组合数据帧:

```
In:  joined_df_merge = region_df.merge(sales_df, how='left', 
                                      left_index=True,
                                      right_index=True)
     print(joined_df_merge)Out: region  sales
Tony     West  103.0
Sally   South  202.0
Carl     West    NaN
Archie  North    NaN
Randy    East  380.0
Ellen   South  101.0
Fred      NaN   82.0
Mo       East    NaN
HanWei    NaN    NaN
```

这与我们使用 **join** 时没有什么不同。但是 **merge** 允许我们为左右两个数据帧指定要连接的列。这里通过设置“左索引”和“右索引”等于真，我们让**合并**知道我们想要在索引上连接。我们得到了与之前使用 **join** 时相同的组合数据帧。

**Merge** 在我们不想在索引上连接的时候很有用。例如，假设我们想知道每个员工为他们的区域贡献了多少百分比。我们可以使用 **groupby** 来汇总每个地区的所有销售额。在下面的代码中， **reset_index** 用于将 region 从 data frame(grouped _ df)的索引转换为普通的列——是的，我们可以将它保留为索引并在其上连接，但是我想演示如何在列上使用 **merge** 。

```
In:  grouped_df = joined_df_merge.groupby(by='region').sum()
     grouped_df.reset_index(inplace=True)
     print(grouped_df)Out: region  sales
0   East  380.0
1  North    0.0
2  South  303.0
3   West  103.0
```

现在让**使用 region 列合并** joined_df_merge 和 grouped_df。我们必须指定一个后缀，因为我们的两个数据框架(我们正在合并)都包含一个名为 sales 的列。**后缀**输入将指定的字符串附加到两个数据帧中具有相同名称的列的标签上。在我们的例子中，因为第二个 dataframe 的 sales 列实际上是整个地区的销售额，所以我们可以在它的标签后面加上“_region”来说明这一点。

```
In:employee_contrib = joined_df_merge.merge(grouped_df, how='left', 
                                         left_on='region', 
                                         right_on='region',
                                         suffixes=('','_region'))
print(employee_contrib) Out: region  sales  sales_region
0   West  103.0         103.0
1  South  202.0         303.0
2   West    NaN         103.0
3  North    NaN           0.0
4   East  380.0         380.0
5  South  101.0         303.0
6    NaN   82.0           NaN
7   East    NaN         380.0
8    NaN    NaN           NaN
```

哦不，我们的索引不见了！但是我们可以用 **set_index** 把它取回来(否则我们就不知道每行对应的是哪个员工):

```
In:employee_contrib = employee_contrib.set_index(joined_df_merge.index)
print(employee_contrib) Out: region  sales  sales_region
Tony     West  103.0         103.0
Sally   South  202.0         303.0
Carl     West    NaN         103.0
Archie  North    NaN           0.0
Randy    East  380.0         380.0
Ellen   South  101.0         303.0
Fred      NaN   82.0           NaN
Mo       East    NaN         380.0
HanWei    NaN    NaN           NaN
```

我们现在有了原始的 sales 列和一个新的 sales_region 列，它告诉我们一个地区的总销售额。让我们计算每个雇员占销售额的百分比，然后通过删除没有区域的观察值(弗雷德和韩伟)并用零填充销售列中的 NaNs 来清理我们的数据框架

```
In:# Drop NAs in region column
employee_contrib = employee_contrib.dropna(subset=['region'])# Fill NAs in sales column with 0
employee_contrib = employee_contrib.fillna({'sales': 0})employee_contrib['%_of_sales'] = employee_contrib['sales']/employee_contrib['sales_region']print(employee_contrib[['region','sales','%_of_sales']]\
      .sort_values(by=['region','%_of_sales'])) Out: region  sales  %_of_sales
Mo       East    0.0    0.000000
Randy    East  380.0    1.000000
Archie  North    0.0         NaN
Ellen   South  101.0    0.333333
Sally   South  202.0    0.666667
Carl     West    0.0    0.000000
Tony     West  103.0    1.000000
```

全部完成！请注意，北部地区没有销售额，因此有 NaN(不能被零除)。

# 结论

让我们快速回顾一下:

*   我们可以使用 **join** 和 **merge** 来合并 2 个数据帧。
*   当我们在索引上连接数据帧时, **join** 方法效果最好(尽管您可以为左边的数据帧指定另一个要连接的列)。
*   **merge** 方法更加灵活，它允许我们指定索引之外的列来连接两个数据帧。如果索引在**合并**后被重置为计数器，我们可以使用 **set_index** 将其改回。

下一次，我们将了解如何通过 Pandas 的 **concatenate** 函数添加新的数据行(以及更多内容)。干杯！

如果你总体上喜欢这篇文章和我的写作，请考虑通过我的推荐链接注册 Medium 来支持我的写作。谢谢！T32