# 如何用 Pandas 编写所有的 SQL 查询

> 原文：<https://towardsdatascience.com/how-to-write-all-of-your-sql-queries-in-pandas-449dd8b2c94e?source=collection_archive---------24----------------------->

## 一个全面的 SQL 到熊猫字典

![](img/8cfcb319df5fbd9adfdaa49bd01d57c2.png)

伊洛娜·弗罗利希在 [Unsplash](https://unsplash.com/s/photos/pandas?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) 上的照片

# 介绍

SQL 之所以如此神奇，是因为它太容易学了——之所以这么容易学，是因为代码语法太直观了。

另一方面，Pandas 就不那么直观了，尤其是如果你像我一样先从 SQL 开始。

就个人而言，我发现真正有帮助的是思考如何在 SQL 中操作我的数据，然后在 Pandas 中复制它。所以，如果你想更精通熊猫，我强烈建议你也采用这种方法。

因此，这篇文章作为一个备忘单，字典，指南，无论你怎么称呼它，以便你在使用熊猫时可以参考。

说到这里，让我们开始吧！

# 目录

1.  选择行
2.  组合表格
3.  过滤表格
4.  排序值
5.  聚合函数

# 1.选择行

## SELECT * FROM

如果您想选择整个表格，只需调用表格的名称:

```
**# SQL**
SELECT * FROM table_df**# Pandas**
table_df
```

## 从中选择 a、b

如果要从表格中选择特定的列，请在双括号中列出所需的列:

```
**# SQL**
SELECT column_a, column_b FROM table_df**# Pandas**
table_df[['column_a', 'column_b']]
```

## 选择不同

简单地使用[。drop_duplicates()](https://stackoverflow.com/questions/30530663/how-to-select-distinct-across-multiple-data-frame-columns-in-pandas) 获取不同的值:

```
**# SQL**
SELECT DISTINCT column_a FROM table_df**# Pandas**
table_df['column_a'].drop_duplicates()
```

## 选择 a 作为 b

如果你想重命名一个列，使用[。rename()](https://pandas.pydata.org/pandas-docs/stable/reference/api/pandas.DataFrame.rename.html) :

```
**# SQL**
SELECT column_a as Apple, column_b as Banana FROM table_df**# Pandas**
table_df[['column_a', 'column_b']].rename(columns={'column_a':
'Apple', 'column_b':'Banana'})
```

## 选择案例时间

对于 SELECT CASE WHEN 的等效项，可以使用 np.select()，首先指定您的选择以及每个选择的值。

如果您想在每个选项中包含多个条件，请查看本文。

```
**# SQL**
SELECT CASE WHEN column_a > 30 THEN "Large"
            WHEN column_a <= 30 THEN "Small"
            END AS Size
FROM table_df**# Pandas**
conditions = [table_df['column_a']>30, table_df['column_b']<=30]
choices = ['Large', 'Small']
table_df['Size'] = np.select(conditions, choices)
```

# 2.组合表格

## 内/左/右连接

简单用[。merge()](https://pandas.pydata.org/pandas-docs/stable/reference/api/pandas.DataFrame.merge.html) 要连接表，可以使用“how”参数指定是左连接、右连接、内连接还是外连接。

```
**# SQL**
SELECT * FROM table_1 t1
         LEFT JOIN table_2 t1 on t1.lkey = t2.rkey **# Pandas** table_1.merge(table_2, left_on='lkey', right_on='rkey', how='left')
```

## 联合所有

只需使用 [pd.concat()](https://stackoverflow.com/questions/12850345/how-do-i-combine-two-data-frames) :

```
**# SQL** SELECT * FROM table_1UNION ALLSELECT * FROM table_2**# Pandas** final_table = pd.concat([table_1, table_2])
```

# 3.过滤表格

## 选择位置

当像在 SQL 中使用 WHERE 子句一样过滤数据帧时，只需在方括号中定义标准:

```
**# SQL** SELECT * FROM table_df WHERE column_a = 1**# Pandas** table_df[table_df['column_a'] == 1]
```

## SELECT column_a WHERE column_b

当您想要从表中选择某一列并使用不同的列对其进行过滤时，请遵循以下格式(如果您想要了解更多信息，请查看此[链接](https://stackoverflow.com/questions/36684013/extract-column-value-based-on-another-column-pandas-dataframe)):

```
**# SQL** SELECT column_a FROM table_df WHERE column_b = 1**# Pandas** table_df[table_df['column_b']==1]['column_a']
```

## 选择地点和

如果要按多个条件进行筛选，只需将每个条件用括号括起来，并用' & '分隔每个条件。更多的变化，请查看这个[链接](https://stackoverflow.com/questions/17071871/how-to-select-rows-from-a-dataframe-based-on-column-values)。

```
**# SQL** SELECT * FROM table_df WHERE column_a = 1 AND column_b = 2**# Pandas** table_df[(table_df['column_a']==1) & (table_df['column_b']==2)]
```

## 选择喜欢的地方

SQL 中 LIKE 的对等词是 [.str.contains()](https://stackoverflow.com/questions/11350770/select-by-partial-string-from-a-pandas-dataframe) 。如果您想应用不区分大小写，只需在参数中添加 case=False(参见[这里的](https://stackoverflow.com/questions/32616261/filtering-pandas-dataframe-rows-by-contains-str))。

```
**# SQL** SELECT * FROM table_df WHERE column_a LIKE '%ball%'**# Pandas** table_df[table_df['column_a'].str.contains('ball')]
```

## 在()中选择 WHERE 列

SQL 中 IN()的对等词是[。isin()](https://stackoverflow.com/questions/11350770/select-by-partial-string-from-a-pandas-dataframe) 。

```
**# SQL** SELECT * FROM table_df WHERE column_a IN('Canada', 'USA')**# Pandas** table_df[table_df['column_a'].isin(['Canada', 'USA'])]
```

# 4.排序值

## 按一列排序

SQL 中 ORDER BY 的对等词是[。sort_values()](https://stackoverflow.com/questions/41825978/sorting-columns-and-selecting-top-n-rows-in-each-group-pandas-dataframe) 。使用' ascending '参数指定是要按升序还是降序对值进行排序，默认情况下是像 SQL 一样按升序排序。

```
**# SQL** SELECT * FROM table_df ORDER BY column_a DESC**# Pandas** table_df.sort_values('column_a', ascending=False)
```

## 按多列排序

如果要按多列排序，请在括号中列出列，并在括号中的“升序”参数中指定排序方向。请确保遵循您列出的各个列的顺序。

```
**# SQL** SELECT * FROM table_df ORDER BY column_a DESC, column_b ASC**# Pandas** table_df.sort_values(['column_a', 'column_b'], ascending=[False, True])
```

# 5.聚合函数

## 非重复计数

您会注意到聚合函数的一个常见模式。

要复制 COUNT DISTINCT，只需使用。groupby()和。努尼克()。更多信息请参见此处的:

```
**# SQL** SELECT column_a, COUNT DISTINCT(ID) 
FROM table_df
GROUP BY column_a**# Pandas** table_df.groupby('column_a')['ID'].nunique()
```

## 总和

```
**# SQL** SELECT column_a, SUM(revenue) 
FROM table_df
GROUP BY column_a **# Pandas** table_df.groupby(['column_a', 'revenue']).sum()
```

## **AVG**

```
**# SQL** SELECT column_a, AVG(revenue) 
FROM table_df
GROUP BY column_a**# Pandas** table_df.groupby('column_a')['revenue'].mean()
```

# 感谢阅读！

希望这可以作为使用 Pandas 操作数据的有用指南。也不要觉得你必须记住所有这些！当我和熊猫一起工作时，这是我经常想起的事情。

最终，通过足够的练习，你会对熊猫感到更加舒适，并且会充分理解其中的基本机制，而不需要依赖像这样的小抄。

一如既往，我祝你在努力中一切顺利！:)

不确定接下来要读什么？我为你挑选了另一篇文章:

[](/a-complete-pandas-glossary-for-data-science-a3bdd37febeb) [## 完整的熊猫数据科学术语表

### 学习熊猫基础知识的最佳资源

towardsdatascience.com](/a-complete-pandas-glossary-for-data-science-a3bdd37febeb) 

## 特伦斯·申

*   *如果你喜欢这个，* [*跟我上 Medium*](https://medium.com/@terenceshin) *了解更多*
*   *关注我*[*Kaggle*](https://www.kaggle.com/terenceshin)*了解更多内容！*
*   *我们连线上*[*LinkedIn*](https://www.linkedin.com/in/terenceshin/)
*   *有兴趣合作？查看我的* [*网站*](http://want%20to%20collaborate/?) *。*