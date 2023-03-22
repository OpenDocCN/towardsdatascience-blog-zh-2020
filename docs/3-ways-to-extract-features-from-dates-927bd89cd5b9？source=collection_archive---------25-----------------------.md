# 使用 Python 从日期中提取要素的 3 种方法

> 原文：<https://towardsdatascience.com/3-ways-to-extract-features-from-dates-927bd89cd5b9?source=collection_archive---------25----------------------->

## 在一行代码中从时间序列数据集中获取更多要素

![](img/733ebd1234ddd38da10099edd94da24b.png)

由[马迪·巴佐科](https://unsplash.com/@maddibazzocco?utm_source=medium&utm_medium=referral)在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 拍摄的照片

# 动机

处理时间序列时，数据集的值可能会受到节假日、一周中的哪一天以及一个月中有多少天的影响。有没有一种方法可以用一行代码从日期特性中提取这些特性？

是啊！在本文中，我将向您展示如何使用两个不同的 Python 库从 date 列中提取上述三个特性。

# 导入和处理数据

我们将使用[每日女性出生数据集](https://machinelearningmastery.com/time-series-datasets-for-machine-learning/)。这个数据集描述了 1959 年加州每天女性出生的数量。

使用以下方式下载数据:

```
!wget [https://raw.githubusercontent.com/jbrownlee/Datasets/master/daily-total-female-births.csv](https://raw.githubusercontent.com/jbrownlee/Datasets/master/daily-total-female-births.csv)
```

Datapane 随机分配时间，因此您可以忽略表中的时间。现在我们将数据可视化

```
df.set_index('Date').plot()
```

# 提取工作日

一周中不同的日子可能会影响出生的数量。因此，我们将添加工作日作为我们数据的一个特征

# 提取假期

节假日也会影响生育数量。父母可能会避免在新年或圣诞节等节日带孩子。在这篇博客中，圣诞节被认为是出生婴儿最少的一天。

基于日期提取假日的最简单方法是使用名为 [holidays](https://pypi.org/project/holidays/) 的 Python 库。安装假日使用

```
pip install holidays
```

假日图书馆提供许多国家的假日。我们还可以指定要从国家的哪个州或省提取假期。

由于我们的数据描述了加州每天女性分娩的数量，因此我们将找到加州的假期。在得到一个假期字典后，我们简单地使用`holiday_dict.get(date)`来得到那个日期的假期。

```
Timestamp('1959-01-01 00:00:00')"New Year's Day"
```

我们也可以在所有可用的假期达到顶峰

```
datetime.date(1959, 1, 1): "New Year's Day",
datetime.date(1959, 2, 22): "Washington's Birthday",
datetime.date(1959, 5, 30): 'Memorial Day',
datetime.date(1959, 7, 4): 'Independence Day',
datetime.date(1959, 7, 3): 'Independence Day (Observed)',
datetime.date(1959, 9, 7): 'Labor Day',
datetime.date(1959, 10, 12): 'Columbus Day',
datetime.date(1959, 11, 11): 'Veterans Day',
datetime.date(1959, 11, 26): 'Thanksgiving',
datetime.date(1959, 12, 25): 'Christmas Day'}
```

尝试调用字典中没有的日期

```
None
```

如果您发现字典中缺少一些日期，您还可以创建[自定义假日对象。](https://pypi.org/project/holidays/)

让我们使用这个库为数据集创建一个假日要素。

厉害！现在我们有了一个具有假日特征的数据集。为了比较节假日和正常工作日的出生人数，我们将数据按节假日分组，并取同一天所有出生人数的中位数。

可视化我们的数据

元旦出生人数最少。与正常日期的出生人数相比，其他节日的出生人数似乎相当高。

然而，这个数据仅仅是一年内的，所以不足以让我们确定假期对出生人数的实际影响。如果我们有一个 3 年或 4 年的数据集，我们可能会发现在其他节假日出生的人数比正常日期少。

# 提取一个月中的天数

父母也可能避免在 2 月 29 日生孩子，因为这是每四年一次的日子。

如果我们想比较不同月份之间的出生人数，一个月中的日期数也可能会影响出生人数。因此，在分析我们的时间序列数据集时，将一个月中的日期数量考虑在内可能很重要。

提取特定月份和年份的天数的一个非常快速的方法是使用`calendar.monthrange.`这是一个内置的 Python 库，所以我们不需要安装这个库来使用它。

`monthrange()`方法用于获取指定年份和月份中每月第一天的工作日和一个月中的天数。

```
Weekday of first day of the month: 5
Number of days in month: 29
```

自从我们知道 2020 年 2 月有 29 天以来，这个库似乎工作得很好。我们将使用它为我们的数据集创建一个`days_in_a_month`列

厉害！现在我们可以比较每月平均出生人数和每月天数

9 月份平均出生人数有一个高峰。可能是因为 9 月份的天数比其他月份多？你可能已经知道答案了，但是让我们用一个月中的天数柱状图来确认一下

不完全是。九月只有 30 天。一月、三月、五月等月份的天数更多。

即使我们在这里看不到直接的相关性，但一个月中的天数和其他特征的组合可以给我们一些与出生人数的相关性。

# 结论

恭喜你！您已经学习了如何使用日期为时间序列数据集创建更多要素。我希望这些想法能帮助你想出创造性的方法来处理时间序列数据集。

本文的源代码可以在这里找到。

[](https://github.com/khuyentran1401/Data-science/blob/master/time_series/extract_features/extract_features_from_dates.ipynb) [## khuyentran 1401/数据科学

### Permalink GitHub 是 5000 多万开发人员的家园，他们一起工作来托管和审查代码、管理项目以及…

github.com](https://github.com/khuyentran1401/Data-science/blob/master/time_series/extract_features/extract_features_from_dates.ipynb) 

我喜欢写一些基本的数据科学概念，并尝试不同的算法和数据科学工具。你可以在 LinkedIn 和推特上和我联系。

如果你想查看我写的所有文章的代码，请点击这里。在 Medium 上关注我，了解我的最新数据科学文章，例如:

[](/3-python-tricks-to-read-create-and-run-multiple-files-automatically-5221ebaad2ba) [## 自动读取、创建和运行多个文件的 3 个 Python 技巧

### 用 Python 和 Bash For Loop 自动化枯燥的东西

towardsdatascience.com](/3-python-tricks-to-read-create-and-run-multiple-files-automatically-5221ebaad2ba) [](/top-6-python-libraries-for-visualization-which-one-to-use-fe43381cd658) [## 可视化的 6 大 Python 库:使用哪一个？

### 对使用哪种可视化工具感到困惑？我为你分析了每个图书馆的利弊

towardsdatascience.com](/top-6-python-libraries-for-visualization-which-one-to-use-fe43381cd658) [](/i-scraped-more-than-1k-top-machine-learning-github-profiles-and-this-is-what-i-found-1ab4fb0c0474) [## 我收集了超过 1k 的顶级机器学习 Github 配置文件，这就是我的发现

### 从 Github 上的顶级机器学习档案中获得见解

towardsdatascience.com](/i-scraped-more-than-1k-top-machine-learning-github-profiles-and-this-is-what-i-found-1ab4fb0c0474) [](/supercharge-your-python-string-with-textblob-2d9c08a8da05) [## 使用 TextBlob 增强您的 Python 字符串

### 在一行代码中获得更多关于文本的见解！

towardsdatascience.com](/supercharge-your-python-string-with-textblob-2d9c08a8da05)