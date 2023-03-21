# Itertools 中的 5 个高级函数简化了 Python 中的迭代

> 原文：<https://towardsdatascience.com/5-advanced-functions-in-itertools-to-simplify-iterations-in-python-c6213312ca47?source=collection_archive---------6----------------------->

![](img/d7b8dc79c6eb7871ac87dbf5e6629b72.png)

[斯蒂夫·约翰森](https://unsplash.com/@steve_j?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)在 [Unsplash](https://unsplash.com/s/photos/five?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) 上拍照

## 使用 itertools 模块，使一些复杂的迭代变得更加容易

# 介绍

## 干燥原则

一个基本的编码原则是 **DRY** (即不要重复自己)，这也适用于 Python 编程。例如，假设我们需要处理一系列具有相同数据结构的 csv 文件。如果没有实现 DRY 原则，我们可能需要编写一些 Python 代码，如下所示。

没有干燥原理的重复操作

如你所见，由于违反了 DRY 原则，我们无法重用代码的某些部分。此外，当需要改变每个文件的操作时，我们必须更新程序中的多个地方，这是一个容易出错的过程。

下面的代码怎么样？它用上面的代码做了完全相同的事情，但是更加简洁。

根据干燥原理重复操作

代码不仅通过组合所有单个文件的公共操作而具有更好的可读性，而且具有更好的可维护性，因为如果以后我们决定对每个文件进行不同的操作，我们只需更改一个地方的代码。

如您所知，这段代码利用了 Python 中实现的迭代特性，这是一个遵循 DRY 原则的良好实践。迭代背后的基本思想是为特定序列的单个项目提取公共动作，这被称为可迭代的(关于可迭代的更多信息，请参考我以前关于这个主题的文章[。](https://medium.com/swlh/understand-pythons-iterators-and-iterables-and-create-custom-iterators-633939eed3e7)

## 内置迭代函数

如上面的最后一段代码所示，我们通常使用`for`循环来实现迭代，这使得我们可以方便地遍历可迭代对象来执行特定的操作。下面是一些在大多数情况下各种用法的例子。

内置迭代函数

如上面的代码片段所示，除了使用`range()`的基本迭代，我们还利用三个关键函数，包括`enumerate()`、`reversed()`和`zip()`，来改进我们的循环逻辑。要了解更多关于迭代中使用这些内置函数的技巧，你可以参考我以前关于这个主题的文章。

# itertools 模块

在本文中，我将重点介绍五个高级函数，它们将在更复杂的场景中简化迭代。具体来说，我们将探索`itertools`模块。作为标准 Python 库的一部分，`itertools`模块提供了各种工具，允许我们高效地处理迭代器。

如果你想继续学习本教程，请先运行`import itertools`让这个模块在你的程序中可用。重要的是，为了提供适当的使用环境，您可以在以后自己参考，这些函数都有一个实际的例子。

## `accumulate`()

这个函数`accumulate(*iterable*[, *func*, ***, *initial=None*])`通过指定`func`参数来构造一个迭代器，该迭代器返回二进制函数(即带有两个参数的函数)的累积结果。可选的`initial`参数是在输出 iterable 的开头设置一个额外的值。乍一看，这个函数可能听起来有点混乱，但是下面的例子将帮助您理解它。

假设你花 5 万美元买了一辆特斯拉汽车，月供 2000 美元，月息 0.3%。我们想跟踪每月的余额。下面的代码展示了我们如何使用`accumulate()`函数来实现这一点。需要注意的一点是，我们使用一个 lambda 函数，它正好接受两个参数，作为二元函数来处理连续的元素以产生累加的结果。

使用 accumulate()的迭代

## `The groupby`()

这个函数`groupby(*iterable*, *key=None*)`接受一个 iterable 和一个 key，这个函数指定 iterable 中的元素应该如何分组。需要注意的一点是，如果元素没有事先排序，这个`groupby()`函数不会像您预期的那样使用 key 函数对元素进行分组。换句话说，在使用`groupby()`函数之前，通常需要使用相同的函数对 iterable 进行排序。

假设我们有一些新员工参加了他们的入职培训，我们想按他们姓氏的首字母对他们进行分组，这样他们就可以更快地签到以获得他们的徽章。下面的代码展示了我们如何使用`groupby()`函数来获得想要的结果。

使用 groupby()的迭代

## `The combinations`()

这个函数`combinations(*iterable*, *r*)`接受一个 iterable 和一个整数，并返回长度为 r 的元素子序列。有一点需要注意，这些元组中元素的顺序将反映它们在初始 iterable 中的原始顺序。

假设我们有一些不同面值的硬币，我们想用一定数量的硬币找出所有可能的组合。眼熟吗？你说得对——我是从我 8 岁女儿的数学作业中得到这个想法的。请随意将代码用于您的内部辅导工作。

使用组合的迭代()

## `product`()

这个函数`product(**iterables*, *repeat=1*)`接受一个或多个可迭代对象，并返回这些可迭代对象的笛卡尔积。这个函数非常像嵌套的`for`循环，它创建了一个迭代器来“展平”嵌套的循环。可选的`repeat`参数是为指定的数字重复唯一的输入迭代器。换句话说，`product(a_list, repeat=3)`与`product(a_list, a_list, a_list)`相同。

假设你将有一个侄子，你姐姐让你给这个男孩取名。我们知道他会姓汤普森。我们有两个名单，分别是名字和中间名。让我们找出可能的组合。最后，我们可能会选择约翰·埃利奥特·汤普森这个名字，因为 JET 这个首字母对一个男孩来说很酷:)

使用产品的迭代()

## `The permutations`()

这个函数`permutations(*iterable*, *r=None*)`接受一个 iterable 和一个整数，并从 iterable 返回连续的 *r* 长度的元素排列。如果省略可选的`r`参数，该函数将返回指定 iterable 的所有可能排列。这个函数与`combinations()`相似，都产生元素的组合。区别在于顺序在`permutations()`函数中很重要，但在`combinations()`函数中不重要。请看下面的插图。

```
>>> numbers = [1, 2]
>>> print(list(itertools.combinations(numbers, 2)))
[(1, 2)]
>>> print(list(itertools.permutations(numbers, 2)))
[(1, 2), (2, 1)]
```

在一个实际的例子中，假设南美的四个国家在世界杯预选赛中处于同一组，我们想知道所有可能的比赛。每支球队都将与其他球队进行一场主场和客场比赛，因此每支球队配对的顺序至关重要。所以我们可能要用`permutations()`功能。

使用排列的迭代()

# 外卖食品

本教程向您展示了五个高级函数，我们可以用它们来简化我们在实际项目中可能遇到的一些复杂的迭代。当然，我们可以用自己的代码来实现这些迭代需求，但是可能需要复杂的逻辑来实现这些功能，并且我们的项目可能会因为涉及更多的代码而出现错误。因此，只要适用，我们应该考虑使用这些函数，因为它们已经被特别优化来处理各种迭代需求。

要加快你的项目进度，除了干原则，编程中的另一个原则就是**不要多此一举！**因此，学习 itertools 模块中的这些高级函数和更多函数，如其官方文档[中所列，并在任何适用的时候使用它们。](https://docs.python.org/3.8/library/itertools.html)

# 附加阅读

感谢阅读这篇文章。这里有一些关于这个主题的额外读物供感兴趣的读者阅读。

[](https://medium.com/better-programming/how-to-use-for-loops-better-in-python-1dfbc3d9e91f) [## 如何在 Python 中更好地使用 For 循环

### 几个可以改善循环逻辑的函数

medium.com](https://medium.com/better-programming/how-to-use-for-loops-better-in-python-1dfbc3d9e91f) [](https://medium.com/swlh/understand-pythons-iterators-and-iterables-and-create-custom-iterators-633939eed3e7) [## 理解 Python 的迭代器和可迭代对象，并创建自定义迭代器

### 迭代是 Python 中最重要的概念之一。与迭代相关的两个术语是迭代器和…

medium.com](https://medium.com/swlh/understand-pythons-iterators-and-iterables-and-create-custom-iterators-633939eed3e7)  [## itertools -为高效循环创建迭代器的函数- Python 3.8.2 文档

### 这个模块实现了许多受 APL、Haskell 和 SML 启发的构件。每个都是…

docs.python.org](https://docs.python.org/3.8/library/itertools.html)