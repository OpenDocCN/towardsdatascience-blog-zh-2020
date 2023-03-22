# Python 基础知识-2:使用列表、元组和字典

> 原文：<https://towardsdatascience.com/python-basics-2-working-with-list-tuples-dictionaries-871c6c01bb51?source=collection_archive---------36----------------------->

## 机器学习 100 天的第 2 天

![](img/5abeec7be0983f8199a5c829cb4c9402.png)

照片由[埃米尔·佩龙](https://unsplash.com/@emilep?utm_source=medium&utm_medium=referral)在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄

# 内容

*   使用列表
*   列表理解
*   高级列表操作:切片列表，遍历切片列表，复制列表
*   元组
*   基本元组操作:修改元组，遍历所有元组值
*   字典
*   基本的字典操作:修改字典，从字典中删除键值
*   通过字典循环
*   嵌套词典

# 使用列表

## 制作号码列表

在 python 中，我们可以使用`range()`函数创建一系列数字。让我们来看看如何使用`range()`函数创建列表。

## 遍历列表

在数据分析中，您经常需要遍历列表中的每一项。让我们看一看。

## `range()`功能同 for 循环

我们也可以在 for 循环中使用`range()`函数——有时需要循环遍历一系列数字。让我们来看看代码。

## 带列表的基本统计

可以用`min()`、`max()`、`sum()`等列表进行简单的统计。

# 列表理解

列表理解是一种生成列表的高级方法。列表理解允许你组合 for 循环来创建一个列表。假设您必须创建一个包含 1 到 10 的立方体的列表。首先，让我们看看如何在不理解列表的情况下做到这一点。

现在让我们用列表理解法列出同样的列表。

# 预先列表操作

## 切片列表

要对列表进行切片，必须传递起始索引和最后一个元素索引。像`range()`函数一样，slice 也在一个元素前停止。如果你想要元素从开始到第 5 个元素，你必须传递 0(第一个元素索引)到 5(第六个元素索引)。让我们来看看如何用代码实现它。

## 遍历切片列表

之前，我们已经看到了如何循环遍历列表以及如何对列表进行切片。甚至我们可以循环遍历切片列表。让我们看看如何做到这一点。

## 复制列表

在进行数据分析时，可能需要对现有列表和现有列表的副本进行操作。让我们看看如何复制列表。

现在我们有两个列表，我们可以根据需要修改它们。让我们应用我们在之前的博客中学到的基本操作。

# 元组

和列表一样，元组也是 python 中的集合类型。然而，元组是不可变的，这意味着不可变列表的值不能改变。在 python 中，元组使用括号“(”。让我们来看看如何在 python 中定义元组。

# 基本元组操作

## 修改元组

我们知道，元组是不可变的列表类型，这意味着它不能被修改。但是，我们可以覆盖元组，这意味着我们可以完全改变元组值。

## 遍历所有元组值

在 python 中，我们可以像处理列表一样遍历元组。

与列表相比，元组是简单的数据结构。当程序员希望存储一些在整个程序中不能改变的值时，通常会用到它们。

# 字典

在 python 中，字典是键和值对的集合。在字典中，每个键都与值相关联。要访问任何值，我们必须使用与值相关联的键。我们可以在字典中使用任何数据对象作为键或值。让我们创建一个简单的字典。

# 基本字典操作

## 修改字典

像列表数据收集方法一样，我们也可以修改字典。让我们来看看代码。

## 从字典中删除键值

我们可以从字典中删除不需要的信息。

# 通过字典循环

像列表和元组一样，我们可以使用循环来检查字典的每个键和值。

在字典中，只能循环访问键或值。让我们来看看代码。

# 嵌套词典

在 python 中，可以在列表中存储字典，反之亦然。首先，让我们看看如何创建字典列表。

现在让我们来看看如何在字典中创建列表。

甚至你可以在字典里储存一本字典。

今天我们从 [python 速成教程](https://geni.us/ew5ZE7B)的书中学习了 Python 的基础知识(第 4 章和第 6 章)。我参加这个 100 天机器学习挑战的目标是从零开始学习机器学习，并帮助其他想开始机器学习之旅的人。很多概念我都知道，但我是从零开始，帮助机器学习社区的初学者，修正概念。

**感谢阅读。**

如果你喜欢我的工作并想支持我，我会非常感谢你在我的社交媒体频道上关注我:

*   支持我的最好方式就是跟随我上 [**中级**](/@durgeshsamariya) 。
*   订阅我的新 [**YouTube 频道**](https://www.youtube.com/c/themlphdstudent) 。
*   在我的 [**邮箱列表**](https://tinyletter.com/themlphdstudent) 报名。

如果你错过了我的前一部分系列。

*   第 0 天:[挑战介绍](https://medium.com/@durgeshsamariya/100-days-of-machine-learning-code-a9074e1c42c3)
*   第 1 天: [Python 基础知识— 1](https://medium.com/the-innovation/python-basics-variables-data-types-and-list-59cea3dfe10f)
*   第 2 天: [Python 基础知识— 2](/python-basics-2-working-with-list-tuples-dictionaries-871c6c01bb51)
*   第三天: [Python 基础— 3](https://medium.com/towards-artificial-intelligence/python-basics-3-97a8e69066e7)

我希望你会喜欢我的其他文章。

*   [八月月度阅读清单](https://medium.com/@durgeshsamariya/august-2020-monthly-machine-learning-reading-list-by-durgesh-samariya-20028aa1d5cc)
*   [集群资源](https://medium.com/towards-artificial-intelligence/a-curated-list-of-clustering-resources-fe355e0e058e)