# 3 个神奇的熊猫提高记忆效率的窍门

> 原文：<https://towardsdatascience.com/these-3-tricks-will-make-pandas-more-memory-efficient-455f9b672e00?source=collection_archive---------46----------------------->

## 你应该马上开始使用这三个技巧，停止使用不必要的工具。

![](img/8e2478244b56c3ef9878da0e9e96ed40.png)

照片由 [Aaron Burden](https://unsplash.com/@aaronburden?utm_source=medium&utm_medium=referral) 在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

M 任何数据分析任务仍然在笔记本电脑上执行。这加快了分析的速度，因为您已经准备好了熟悉的工作环境和所有的工具。但是你的笔记本电脑很可能不是拥有 x-GB 主内存的“最新野兽”。

然后一个记忆错误让你大吃一惊！

![](img/ab7d254f6ef0f54d72db3a8e7e0efccf.png)

Gif 来自 [Giphy](https://giphy.com/gifs/wwe-reaction-wrestling-h3u7w8BR07IHDsnzQw)

你该怎么办？用 Dask？你从来不使用它，这些工具通常有一些怪癖。你应该要求一个火花集群吗？或者说火花在这一点上是不是有点夸张的选择？

冷静…呼吸。

![](img/fbbeae1bcf0639b70d38dc9b1b9a6d7a.png)

Gif 来自 [Giphy](https://giphy.com/gifs/cbc-funny-comedy-l1J9NRpOeS7i54xnW)

在考虑使用另一种工具之前，问自己以下问题:

> 我需要所有的行和列来执行分析吗？

**在您开始之前，这里有几个您可能感兴趣的链接:**

```
- [Labeling and Data Engineering for Conversational AI and Analytics](https://www.humanfirst.ai/)- [Data Science for Business Leaders](https://imp.i115008.net/c/2402645/880006/11298) [Course]- [Intro to Machine Learning with PyTorch](https://imp.i115008.net/c/2402645/788201/11298) [Course]- [Become a Growth Product Manager](https://imp.i115008.net/c/2402645/803127/11298) [Course]- [Deep Learning (Adaptive Computation and ML series)](https://amzn.to/3ncTG7D) [Ebook]- [Free skill tests for Data Scientists & Machine Learning Engineers](https://aigents.co/skills)
```

*上面的一些链接是附属链接，如果你通过它们购买，我会赚取佣金。请记住，我链接课程是因为它们的质量，而不是因为我从你的购买中获得的佣金。*

# 技巧 1:阅读时过滤行

在一种情况下，您不需要所有行，您可以分块读取数据集并过滤不必要的行以减少内存使用:

```
iter_csv = pd.read_csv('dataset.csv', iterator=True, chunksize=1000)
df = pd.concat([chunk[chunk['field'] > constant] for chunk in iter_csv])
```

> 分块读取数据集比一次全部读取要慢。我建议只对大于内存的数据集使用这种方法。

# 技巧 2:阅读时过滤列

在一种情况下，您不需要所有的列，您可以在读取数据集时使用“usecols”参数指定必需的列:

```
df = pd.read_csv('file.csv', u*secols=['col1', 'col2'])*
```

> 这种方法通常会加快读取速度并减少内存消耗。因此，我建议对每个数据集使用。

# 技巧 3:结合两种方法

![](img/cbea6908d2b63d421f898f21dfe7325e.png)

Gif 来自 [Giphy](https://giphy.com/gifs/loop-play-colors-xUA7b0Klw8Wfor7FWo)

这两种方法的伟大之处在于，您可以将它们结合起来。因此过滤要读取的行并限制列数。

**要升级你的熊猫游戏，看看我的熊猫系列:**

[](https://medium.com/@romanorac/pandas-data-analysis-series-b8cec5b38b22) [## 熊猫数据分析系列

### 从提示和技巧，如何不指南到与大数据分析相关的提示，熊猫文章的精选列表。

medium.com](https://medium.com/@romanorac/pandas-data-analysis-series-b8cec5b38b22) 

# 但是我需要所有的列和行来进行分析！

![](img/68a1f9e1f89186e56b8651a6dd48c26a.png)

Gif 来自 [Giphy](https://giphy.com/gifs/pbs-data-its-okay-to-be-smart-big-xT0BKi1TLjmKiu1HGg)

那你应该试试 [Vaex](/how-to-process-a-dataframe-with-billions-of-rows-in-seconds-c8212580f447) ！

Vaex 是一个高性能的 Python 库，用于懒惰的核外数据帧(类似于 Pandas)，以可视化和探索大型表格数据集。它每秒可以计算超过 10 亿行的基本统计数据。它支持多种可视化，允许对大数据进行交互式探索。

[](/how-to-process-a-dataframe-with-billions-of-rows-in-seconds-c8212580f447) [## 如何在几秒钟内处理数十亿行的数据帧

### 您应该知道的另一个用于数据分析的 Python 库——不，我不是在说 Spark 或 Dask。

towardsdatascience.com](/how-to-process-a-dataframe-with-billions-of-rows-in-seconds-c8212580f447) 

# 在你走之前

在[推特](https://twitter.com/romanorac)上关注我，在那里我定期[发关于数据科学和机器学习的](https://twitter.com/romanorac/status/1328952374447267843)推特。

![](img/b5d426b68cc5a21b1a35d0a157ebc4f8.png)

照片由[Courtney hedge](https://unsplash.com/@cmhedger?utm_source=medium&utm_medium=referral)在 [Unsplash](https://unsplash.com/?utm_source=medium&utm_medium=referral) 上拍摄