# Python+ 101:最有用的数据结构和算法

> 原文：<https://towardsdatascience.com/python-101-most-useful-data-structures-and-algorithms-895d6bf2f1b3?source=collection_archive---------30----------------------->

一种使用鲜为人知的 python 特性的新方法

![](img/7eacff7a9b715f2c22c73d38c01722a7.png)

照片由[内森·安德森](https://unsplash.com/@nathananderson?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)在 [Unsplash](https://unsplash.com/s/photos/scenery?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) 上拍摄

Python 无疑是本世纪使用最广泛的编程语言之一，它在易用性和直观性方面为编程提供了一个全新的视角。除了拥有丰富的最常见的数据结构之外，它还提供了许多标准库，可以极大地减轻为不同领域的各种问题从头开始开发代码的负担。在本教程中，我将关注一些最常见的问题，或者我应该说，遭遇，以及使用标准 python 模块的数据结构和算法的解决方案。因此，让我们深入“Python+”的世界，这是关于在我们所了解的 Python 中加入更多的东西。

**1。列表、元组、集合或 numpy 数组中的 n 个最大或最小元素**

在许多场景中，我们需要提取集合中最大或最小的元素。python 的 **heapq** 模块的 *nlargest* 和 *nsmallest* 方法可以用来获取一个列表中 N 个最大和最小的元素。`nlargest(n,iterable)`和`nsmallest(n,iterable)`其中`n`是需要获取的最大或最小元素的个数，`iterable`可以是列表、元组、集合或 numpy 数组，使用如下。

图一。列表、元组和数组中最小和最大的元素

如果在一个集合中需要不止一个最大或最小的元素，这些方法会更快更有效。如果需要单个最大或最小的元素，python 中的`min`和`max`函数更有效。*注意，* `*nlargest*` *和* `*nsmallest*` *函数总是返回一个列表，与集合的类型无关。*

**2。集合中最常见的元素和元素出现的次数**

有时候，我们需要在一个集合中找到 N 个最常见的元素。**集合**库的`Counter`类是一种简单的方法，通过它的`most_common`方法提取列表或 numpy 数组中最常见的元素以及它们出现的次数。

图二。数组中最常见的元素

`most_common`方法将我们想要的最常见元素的数量作为参数，并返回一个元组列表，其第一个元素表示数组中最常见的元素，第二个元素是该元素的出现次数，按照出现次数递减排序。在图 2 所示的代码片段中，`myarray`中的`1`出现了 6 次，`0`出现了 3 次。

此外，特定元素的出现次数可以通过索引获得。例如，元素`5`在`myarray`中出现 3 次，只需使用`myarray[5].`即可找到

`Counter`是一个非常有用的类(更准确地说是子类),用于统计可散列的项目。数组或列表的元素存储为字典键，它们的计数作为相应的字典值。

**3。表格分类:将数组一分为二**

有时，需要根据一些断点对数据进行分类/归类。**平分**模块的`bisect`方法是一个很好的工具，可以很容易地实现这种分类。考虑一下 100 分制考试中 10 个学生的分数。我们需要根据他们的分数是低于 25 分、在 25 分和 50 分之间、在 50 分和 75 分之间还是在 75 分以上，将他们分为 4 类。这可以按如下方式完成

图 3。数组二分法

根据`breakpoints.`将数组`scores`的每个元素放入各自在`classes`中的类中

**4。排列和组合**

在我对万亿字节数据的研究中，我经常在文件操作中遇到排列和组合的需要。itertools 库提供了一些不错的方法来计算数据的排列和组合(有和没有替换)。`permutations(iterable, r)`提供了`iterable`的`r`长度排列的个数，可以是列表，也可以是数组。类似地，`combinations()`和`combinations_with_replacement()`分别返回替换和不替换 iterable 的组合。后者允许单个元素连续重复。所有三个函数的参数都是相同的。

感谢阅读！

希望有帮助。欢迎任何评论或批评。