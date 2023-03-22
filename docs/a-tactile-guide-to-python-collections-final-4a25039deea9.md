# Python 系列的触觉指南

> 原文：<https://towardsdatascience.com/a-tactile-guide-to-python-collections-final-4a25039deea9?source=collection_archive---------9----------------------->

![](img/1f776cb59265db659859a26c448cb135.png)

由 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上的 [chuttersnap](https://unsplash.com/@chuttersnap?utm_source=medium&utm_medium=referral) 拍摄

Python 是一种功能强大的编程语言，具有动态语义，由被称为“python 之禅”的 19 条原则指导。这些原则列举如下:

*谓美丽胜过丑陋。
显性比隐性好。简单比复杂好。
复杂总比复杂好。
扁平比嵌套好。
稀不如密。
可读性很重要。特例不足以特殊到违反规则。
虽然实用性胜过纯粹性。错误永远不会悄无声息地过去。
除非明确消音。
面对暧昧，拒绝猜测的诱惑。应该有一种——最好只有一种——显而易见的方法来做这件事。虽然这种方式一开始可能并不明显，除非你是荷兰人。
现在总比没有好。
虽然永远也不会比现在好。如果实现很难解释，这是个坏主意。
如果实现起来容易解释，这也许是个好主意。名称空间是一个非常棒的想法——让我们多做一些吧！*”

如果我们不想违背 python 的原则，我们必须适当地充分存储我们的数据。Python 提供了一些内置的容器来帮助存储我们的数据，如列表、元组、集合、字典等。已经开发了几个模块，它们提供附加的数据结构来存储数据集合。Python [集合模块](https://docs.python.org/2/library/collections.html)就是这样一个模块，它的目的是改进内置容器的功能。坚持 python 说“… *很难解释，这是个坏主意”的禅，我会进一步解释什么是模块。*

**模块化编程**指的是将一个庞大、笨拙的编程任务分解成单独的、更小的、更易于管理的子任务或**模块**的过程。然后可以将各个模块拼凑在一起，创建一个更大的应用程序。

![](img/226400e34dcef8b2b7645775c04690ce.png)

照片由[弗兰拍摄。](https://unsplash.com/@fran_?utm_source=medium&utm_medium=referral)开[退溅](https://unsplash.com?utm_source=medium&utm_medium=referral)

模块化代码的一些优势包括:

1.  简单
2.  可维护性
3.  复用性

模块是包含 Python 定义和语句的文件，有助于实现一组函数。模块可以定义函数、类和变量。文件名是模块名加上后缀`.py`。而包是堆叠在一起的相关模块的集合。它们也被称为图书馆。

Python **模块**和 Python **包**，两种便于**模块化编程**的机制。

# 收集模块

collections 模块提供了内置容器数据类型的替代方法，如 list、tuple 和 dict。在本文中，我们考虑 python 集合模块中六(6)种常用的数据结构。它们如下:

1.  命名元组
2.  双端队列
3.  计数器
4.  有序直接
5.  默认字典
6.  链式地图

回想起来，内置容器的概要如下。

*   **List** 是一个有序的、异构的、可变的 python 容器。它是用“[]”创建的。
*   Tuple 非常类似于一个列表，但有一个主要区别——它是不可变的。我们用括号“()”创建一个元组。
*   **集合**也类似于列表，只是它是无序的。它可以存储异构数据，并且是可变的。我们通过用花括号“{}”将数据括起来来创建一个集合。
*   Dictionary 是一个无序的、异构的、可变的 python 容器。它与一个**密钥对值**相关联。它只能通过它的键进行索引。

1.  **named tuple:**‘named tuple’生成一个类似于元组的类，但是有命名条目。namedtuple 返回一个元组，其中包含元组中每个位置(索引)的名称，而不是一个数字。普通元组的一个最大问题是，你必须记住元组对象的每个字段的索引。

namedtuple 的代码段

2.**dequee:**dequee 是一个为插入和删除项目而优化的列表。我们可以把一个“队列”想象成一个列表，我们通常关心的是处理列表的末尾。

deque 的代码段

3.计数器:计数器是一个非常有用的对象。它对一些 iterable 中的元素进行计数，并返回一个类似字典的结构，其中包含每个元素的计数。

4.**order dict:**Python 字典没有自然的顺序，但是有时候让字典条目的属性通过排序来访问是很有用的。“OrderedDict”与“Dict”完全一样，但它会记住键的插入顺序。

5.字典的一个常见范例是处理丢失键的情况。defaultdict 的工作方式与 python 字典完全一样，只是当您试图访问一个不存在的键时，它不会抛出 KeyError。相反，它使用创建 defaultdict 时作为参数传递的数据类型的元素来初始化键。数据类型称为 default_factory。

6. **ChainMap:** 这是用来合并几个字典，它返回一个字典列表。

我希望这篇文章能让你体会到 python 集合的重要性和应用，以便你能更频繁地使用它。感谢阅读。

![](img/df98da08874f4f5371d6a015bdc5a8be.png)

由[凯利·西克玛](https://unsplash.com/@kellysikkema?utm_source=medium&utm_medium=referral)在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄