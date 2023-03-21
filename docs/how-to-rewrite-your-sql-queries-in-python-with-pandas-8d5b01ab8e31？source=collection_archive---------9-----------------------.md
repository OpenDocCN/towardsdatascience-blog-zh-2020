# 如何用熊猫用 Python 重写 SQL 查询

> 原文：<https://towardsdatascience.com/how-to-rewrite-your-sql-queries-in-python-with-pandas-8d5b01ab8e31?source=collection_archive---------9----------------------->

## 在 Python 中再现相同的 SQL 查询结果

![](img/d0a2167bad247a7baab2a54f4fc2b563.png)

照片由 [Hitesh Choudhary](https://unsplash.com/@hiteshchoudhary?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) 在 [Unsplash](https://unsplash.com/s/photos/python-programming?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) 上拍摄

我们中的一些人熟悉 SQL 中的数据操作，但不熟悉 Python，我们倾向于在项目中频繁地在 SQL 和 Python 之间切换，导致我们的效率和生产力降低。事实上，我们可以使用 Pandas 在 Python 中实现类似的 SQL 结果。

# 入门指南

像往常一样，我们需要安装熊猫包，如果我们没有它。

```
conda install pandas
```

在本次会议中，我们将使用 Kaggle 著名的[泰坦尼克号数据集。](https://www.kaggle.com/c/titanic/data?select=test.csv)

在安装包和下载数据之后，我们需要将它们导入到 Python 环境中。

我们将使用 pandas 数据帧来存储数据，并使用各种 pandas 函数来操作数据帧。

# 选择、不同、计数、限制

让我们从我们经常使用的简单 SQL 查询开始。

`titanic_df[“age”].unique()`将在这里返回唯一值的数组，所以我们需要使用`len()`来获得唯一值的计数。

# 选择，WHERE，OR，AND，IN(带条件选择)

学完第一部分后，你应该知道如何用简单的方法探索数据框架。现在让我们尝试一些条件(也就是 SQL 中的`WHERE`子句)。

如果我们只想从数据框中选择特定的列，我们可以使用另一对方括号进行选择。

> 注意:如果您选择多列，您需要将数组`["name","age"]`放在方括号内。

`isin()`的工作方式与 SQL 中的`IN`完全相同。要使用`NOT IN`，我们需要使用 Python 中的否定`(~)`来达到相同的结果。

# 分组依据、排序依据、计数

`GROUP BY`和`ORDER BY`也是我们用来探索数据的流行 SQL。现在让我们用 Python 来试试这个。

如果我们只想对计数进行排序，我们可以将布尔值传递给`sort_values`函数。如果我们要对多列进行排序，那么我们必须将一个布尔值数组传递给`sort_values`函数。

`sum()`函数将为我们提供数据帧中的所有聚合数字总和列，如果我们只需要一个特定的列，我们需要使用方括号指定列名。

# 最小值、最大值、平均值、中间值

最后，让我们尝试一些在数据探索中很重要的常用统计函数。

由于 SQL 没有中位数函数，所以我将使用 BigQuery `APPROX_QUANTILES`来获得年龄的中位数。

熊猫聚合功能`.agg()`还支持`sum`等其他功能。

现在你已经学会了如何**用熊猫用 Python 重写你的 SQL 查询。**希望这篇文章对你有用。如果我犯了任何错误或错别字，请给我留言。

可以在我的 [**Github**](https://github.com/chingjunetao/medium-article/tree/master/rewrite-sql-with-python) 中查看完整的脚本。干杯！

**如果你喜欢读这篇文章，你可能也会喜欢这些:**

[](/manage-your-python-virtual-environment-with-conda-a0d2934d5195) [## 使用 Conda 管理您的 Python 虚拟环境

### 轻松在 Python 2 和 Python 3 环境之间切换

towardsdatascience.com](/manage-your-python-virtual-environment-with-conda-a0d2934d5195) [](/how-to-send-email-with-attachments-by-using-python-41a9d1a3860b) [## 如何使用 Python 发送带附件的电子邮件

### 作为一名数据分析师，我经常会收到这样的请求:“你能每周给我发一份报告吗？”或者…

towardsdatascience.com](/how-to-send-email-with-attachments-by-using-python-41a9d1a3860b) 

**你可以在 Medium 上找到我其他作品的链接，关注我** [**这里**](https://medium.com/@chingjunetao) **。感谢阅读！**