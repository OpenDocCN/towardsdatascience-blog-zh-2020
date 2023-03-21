# 鲜为人知的熊猫在 Python 中的应用

> 原文：<https://towardsdatascience.com/less-known-pandas-groupby-applications-in-python-72fb304a1973?source=collection_archive---------34----------------------->

## 但是它们对你还是有用的

几个月前，我发表了一篇关于如何在熊猫中掌握 groupby 功能的文章。然后前几天我的一个朋友问了一个问题也可以用 groupby 函数解决。所以今天我列出一些 groupby 函数可以实现的不太为人知的应用。

![](img/b9a9025a65c02423b2cf267216486881.png)

照片由[替代代码](https://unsplash.com/@altumcode?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)在 [Unsplash](https://unsplash.com/s/photos/python-programming?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) 上拍摄

(如果你没有看过我之前关于 groupby 函数的文章，那就继续吧。它会帮助你更快地理解这篇文章。)

[](/learn-how-to-master-groupby-function-in-python-now-4620dd463224) [## 现在学习如何掌握 Python 中的 groupby 函数

### Pandas 中的 GroupBy 比 SQL 中的 groupby 更加复杂和强大。

towardsdatascience.com](/learn-how-to-master-groupby-function-in-python-now-4620dd463224) 

# **1。各组的百分比**

在 Excel 数据透视表中，您可以选择显示父合计的百分比。这表示数据透视表显示了每一项占其总父小计的百分比。在 lambda 的帮助下，这可以很容易地在 Pandas groupby 函数中完成。

```
>>> import pandas as pd
>>> df = pd.DataFrame({'A':['a','b','c']*2,
...                    'B':['d','e']*3,
...                    'C':[1,3,6,10,12,16]
...                    })
>>> df
   A  B   C
0  a  d   1
1  b  e   3
2  c  d   6
3  a  e  10
4  b  d  12
5  c  e  16# First Groupby 
>>> df['Perc_gpby_A'] = df.groupby('A')['C'].apply(lambda x: x/x.sum())# Second Groupby
>>> df['Perc_gpby_B'] = df.groupby('B')['C'].apply(lambda x: x/x.sum())
>>> df
   A  B   C  Perc_gpby_A  Perc_gpby_B
0  a  d   1     0.090909     0.052632
1  b  e   3     0.200000     0.103448
2  c  d   6     0.272727     0.315789
3  a  e  10     0.909091     0.344828
4  b  d  12     0.800000     0.631579
5  c  e  16     0.727273     0.551724
```

上例的主要要点是在 groupby 函数中选择适当的分组级别。在第一个 groupby 语句中，分组级别是列 a。总共有三个组 a、b 和 C。然后，将对列 C 应用 lambda 函数。λ函数是`x/x.sum()`。因此，C 列中的每个值将除以其各自组的总和。例如，a 组的总和是 11。因此，在行 0 和 3 中，c 的值将除以 11。因此结果是 0.0909 和 0.9091。这同样适用于 A 列中的其余两个组和第二个 groupby 语句。

除了使用 lambda 函数，使用`transform`也可以执行类似的计算。

```
>>> df['Perc_gpby_A'] = df['C']/df.groupby('A')['C'].transform('sum')
>>> df['Perc_gpby_B'] = df['C']/df.groupby('B')['C'].transform('sum')
>>> df
   A  B   C  Perc_gpby_A  Perc_gpby_B
0  a  d   1     0.090909     0.052632
1  b  e   3     0.200000     0.103448
2  c  d   6     0.272727     0.315789
3  a  e  10     0.909091     0.344828
4  b  d  12     0.800000     0.631579
5  c  e  16     0.727273     0.551724
```

# 2.转换为列表/字典

这是我朋友前几天问的。如果您想将同一组中的所有值作为一个列表或字典进行分组，本节将为您提供答案。

```
>>> df = pd.DataFrame({'A':['a','b','c']*2,
...                    'B':['d','e']*3,
...                    'C':[1,3,6,10,12,16]
...                    })
>>> df.groupby('A')['C'].apply(list)
A
a    [1, 10]
b    [3, 12]
c    [6, 16]
Name: C, dtype: object
>>> type(df.groupby('A')['C'].apply(list))
<class 'pandas.core.series.Series'>
```

在上面的例子中，在`groupby('A')`之后，`apply(list)`将同一组中的所有值组合成一个列表形式。最终结果是序列形式。在这个层次上，你可以进一步转化为一个列表或字典。

```
>>> df.groupby('A')['C'].apply(list).to_list()
[[1, 10], [3, 12], [6, 16]]
>>> df.groupby('A')['C'].apply(list).to_dict()
{'a': [1, 10], 'b': [3, 12], 'c': [6, 16]}
```

当然，您也可以在`apply`功能中选择`set`。

```
>>> df.groupby('B')['C'].apply(set).to_dict()
{'d': {1, 12, 6}, 'e': {16, 10, 3}}
```

# 3.按组列出的最常见值

```
>>> df = pd.DataFrame({'A':list('ab'*4),
...                    'B':list('c'*2+'d'+'e'*5)})
>>> df
   A  B
0  a  c
1  b  c
2  a  d
3  b  e
4  a  e
5  b  e
6  a  e
7  b  e
>>> df.groupby('A')['B'].agg(pd.Series.mode)
A
a    e
b    e
Name: B, dtype: object
```

我想我不需要在这里做太多的解释。用`agg(pd.Series.mode)`可以得到每组最频繁的值。

这三个应用程序的代码相当简单，但是它们仍然可以帮助您更好地处理分组数据。我希望下次您可以使用 groupby 函数，而不仅仅是简单的聚合。本文到此为止。感谢您的阅读，下次再见。

我的其他 Python 文章

如果你问这个问题，你是在告诉别人你是一个 Python 初学者。

[如果你是 Python 新手(尤其是自学 Python 的话)，请将此加入书签](/bookmark-this-if-you-are-new-to-python-especially-if-you-self-learn-python-54c6e7b5dad8)

[由 Python Selenium 开发的 Web Scrape Twitter(第一部分)](/web-scrape-twitter-by-python-selenium-part-1-b3e2db29051d)

[Web Scrape Twitter by Python Selenium(第二部分)](/web-scrape-twitter-by-python-selenium-part-2-c22ae3e78e03)