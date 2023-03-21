# Pandas 查询方法节省了对变量的双重处理

> 原文：<https://towardsdatascience.com/pandas-query-method-saves-double-handling-of-variables-9293d703b804?source=collection_archive---------17----------------------->

## 最后更新时间:2020 年 1 月

## 使用。query()可以使我们在选择数据时不必在一行代码中多次键入数据帧的变量名。

在 pandas DataFrame 对象中检索具有特定条件的行的标准方法需要“双重处理”；不是特别优雅。

例如，我们想要检索列 *A* 大于 1 的行，这是使用*的标准方法。loc* 属性。

```
*## Setup ##**# Import pandas* **import** pandas **as** pd*# Create dataframe from dict* # Specify the type in the suffix of each column name
my_df = pd.DataFrame({**"A_int"**: [1, 2, -3],
                      **"B_float"**: [7.5, 1.9, 8.4],
                      **"C_str"**: [**'eight'**, **'nine'**, **'Ten'**],
                      **"D_str"**: [**'Mar 2017'**, **'May 2018'**, **'Jun 2016'**]}
                    )*# Convert to datetime column* my_df[**'D_date'**] = pd.to_datetime(my_df[**'D_str'**])"""
>>> my_df
   A_int  B_float  C_str     D_str     D_date
0      1      7.5  eight  Mar 2017 2017-03-01
1      2      1.9   nine  May 2018 2018-05-01
2     -3      8.4    Ten  Jun 2016 2016-06-01
"""## Show loc example 1 ##ex_1_loc = my_df.loc[my_df[**'A_int'**] > 0, :]"""
>>> ex_1_loc
   A_int  B_float  C_str     D_str     D_date
0      1      7.5  eight  Mar 2017 2017-03-01
1      2      1.9   nine  May 2018 2018-05-01
"""
```

这里，我们获得了布尔行序列，其中对于给定的行，A 列大于 0。*。loc* 用于取出设置为 True 的行。注意，变量 *my_df* 在一行代码中使用了两次。
幸好这个变量名不是很长。我发现这种对变量的“双重处理”在语法上类似于 R 中的基本方法，特别是在嵌套和完全不必要的时候。

一些其他语言可以以更易读的格式执行这种过滤:

```
## SQL EXAMPLE ##
SELECT *
FROM my_df
WHERE A > 0## R-TIDYVERSE EXAMPLE ##
library(tidyverse)
my_df %>%
  dplyr::filter(A > 0)
```

对于 python，我们可以用 pandas 的“查询”方法更简单地做到这一点(类似于上面的 SQL / R-Tidyverse 结构)。*查询*只返回符合给定条件的行，所有列都保留在数据帧中。

```
#ex_1_loc = my_df.loc[my_df[**'A_int'**] > 0, :]
ex_1_query = my_df.query("A > 0")
# Or use @ when referencing a variable
pivot_val = 0
ex_1a_query = my_df.query("A > @pivot_val")
```

![](img/bd8b4391e86e2435934f751b83ba712e.png)

标准的熊猫方法经常会让我们看到双重图像 src:[https://pix abay . com/photos/panda-family-pandas-cute-bamboo-3811734/](https://pixabay.com/photos/panda-family-pandas-cute-bamboo-3811734/)

让我们再看六个例子，在这些例子中，query 是对*的适当替代。锁定*方法

## 2.对外部列表进行筛选

给定一个外部列表，获取数据帧中某一列与该列表中的一个元素匹配的行

```
*# Create a list of legitimate entries* legit_entries = [**'eight'**, **'nine'**, **'ten'**]*# Filter column 'C_str' by the array* ex_2_loc = my_df.loc[my_df[**'C_str'**].isin(legit_entries), :]
ex_2_query = my_df.query(**"C_str in @legit_entries"**)"""
   A_int  B_float  C_str     D_str     D_date
0      1      7.5  eight  Mar 2017 2017-03-01
1      2      1.9   nine  May 2018 2018-05-01
"""
```

## 3.列比较

比较两列—这需要在*中进行三重处理。锁定*示例。

```
*# Return rows where 'A_int' is greater than 'B_float'* ex_3_loc = my_df.loc[my_df[**'A_int'**] > my_df[**'B_float'**], :]
ex_3_query = my_df.query(**"A_int > B_float"**)"""
   A_int  B_float C_str     D_str     D_date
1      2      1.9  nine  May 2018 2018-05-01
"""
```

## 4.多条件过滤

在可能的多个列上使用多个条件进行筛选。 *&* 和 *|* 位运算符都是允许的。

```
*# Return rows where 'A_int' is greater than zero
# And where C_str is in the legit_entries array* ex_4_loc = my_df.loc[(my_df[**"A_int"**] > 0) &
                     (my_df[**'C_str'**].isin(legit_entries)), :]
ex_4_query = my_df.query(**"A_int > 0 & C_str in @legit_entries"**)"""
   A_int  B_float  C_str     D_str     D_date
0      1      7.5  eight  Mar 2017 2017-03-01
1      2      1.9   nine  May 2018 2018-05-01
"""
```

## 5.时间戳

查询识别日期，并可以将它们与字符串进行比较。查询引号内的所有字符串都必须用引号括起来。

```
*# Return rows where D_date is after Jan 2018* ex_5_loc = my_df.loc[my_df[**'D_date'**] > **'Jan 2018'**, :]
ex_5_query = my_df.query(**"D_date > 'Jan 2018'"**)"""
   A_int  B_float C_str     D_str     D_date
1      2      1.9  nine  May 2018 2018-05-01
"""
```

## 6.比较前的列转换

您可以在比较之前对给定的列执行函数。
注意 dataframe 的输出仍然包含原来格式的 *C_str* 列？

```
*# First convert C_str to lowercase, than compare entries* ex_6_loc = my_df.loc[my_df[**'C_str'**].str.lower().isin(legit_entries),
                     :]
ex_6_query = my_df.query(**"C_str.str.lower() in @legit_entries"**)"""
   A_int  B_float  C_str     D_str     D_date
0      1      7.5  eight  Mar 2017 2017-03-01
1      2      1.9   nine  May 2018 2018-05-01
2     -3      8.4    Ten  Jun 2016 2016-06-01
"""
```

7。包含空格
的列名最初禁止使用 pandas 查询，这个问题在 0.25.2 中已经解决。
用反斜杠引用该列。列中的其他特殊字符可能不起作用。

```
*# Return rows where 'E rogue column str' column contains the word 'this'* my_df[**'E rogue_column_str'**] = [**'this'**, **'and'**, **'that'**]"""
>>>my_df
   A_int  B_float  C_str     D_str     D_date E rogue_column_str
0      1      7.5  eight  Mar 2017 2017-03-01               this
1      2      1.9   nine  May 2018 2018-05-01                and
2     -3      8.4    Ten  Jun 2016 2016-06-01               that
"""ex_7_loc = my_df.loc[my_df[**"E rogue_column_str"**] == **'this'**, :]
ex_7_query = my_df.query(**"`E rogue_column_str` == 'this'"**))"""
   A_int  B_float  C_str     D_str     D_date E rogue_column_str
0      1      7.5  eight  Mar 2017 2017-03-01               this
"""
```

# 克服“项目”贬值

在熊猫的最新版本(在撰写本文时为 0.25.3)中，*。item()* 方法已被弃用。结合查询功能， *item* 是一个非常有用的工具，可以在假设只返回一行的情况下从给定的列中获取单个值。这将把列从类型 *pd 转换成。系列*的数据类型，如 int、str、float 等。如果返回多行，方法将引发 ValueError。

我们可以使用 squeeze 方法作为替代方法，只需对代码做一些小的修改。与 item 类似，当筛选后的系列只有一行长时，squeeze 会将 pandas 系列从系列类型转换为数据类型。但是，如果序列中存在多个值，则不会引发 value error**而只是返回同一个序列。我们可以通过检查返回值是否仍然是 series 类型来利用这一点。**

```
*# Previous code* **try**:
    single_value = ex_3_query[**'B_float'**].item()
    print(single_value)
**except** ValueError:
    print(**"Error, expected to return only one value, got %d"** %
          len(ex_3_query[**'B_float'**]))*# New code* single_value = ex_3_query[**'B_float'**].squeeze()
**if** isinstance(single_value, pd.Series):
    print(**"Error, expected to return only one value, got %d"** %
          len(ex_3_query[**'B_float'**]))
**else**:
    print(single_value)
```

引用:
[熊猫查询 API](https://pandas.pydata.org/pandas-docs/stable/reference/api/pandas.DataFrame.query.html)