# 在 Python 中使用地图和过滤器

> 原文：<https://towardsdatascience.com/using-map-and-filter-in-python-ffdfa8b97520?source=collection_archive---------11----------------------->

## 如何使用 python 中内置的地图和过滤功能

![](img/0456b8310d0a736cb163a25525bc0084.png)

凯文·Ku 在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上的照片

在本教程中，我们将学习何时以及如何使用 Python 中内置的地图和过滤功能。

## 地图功能

假设您想使用已经有的列表创建一个列表。这意味着您希望使用现有的列表，对每个元素应用某种操作或函数，并使用这些输出来创建一个新的列表。

例如，如果我们有一个数字列表，我们想创建一个包含它们的方块的新列表。一种方法是使用 for 循环遍历数字列表，并应用一个函数返回每个数字或元素的平方。当我们遍历列表时，我们可以在新列表中添加或附加平方值。

让我们看看如何在代码中做到这一点:

> 我们有一个数字列表， **num_list** ，我们想要创建一个新列表， **num_list_squared** ，它包含了 **num_list** 的平方。我们使用 for 循环遍历 **num_list** ，并将每个数字的平方或 **num** 添加到我们的 **num_list_squared** 列表中。

另一种实现方法是使用名为 map 的内置 python 函数。

[map](https://docs.python.org/3/library/functions.html#map) 函数接受两个参数:**我们想要应用的函数和我们想要应用它的 iterable 对象或序列(比如本例中的 list)。**换句话说，map 函数将这个函数映射或应用到我们传入的序列的每个元素。

## **我们将用于地图功能的格式如下:**

> map(要应用的函数，要应用的元素序列)

这个 map 函数将返回一个 map 对象，它是一个迭代器。如果我们想从这个 map 对象创建一个列表，我们需要将 map 对象传递给内置的 list 函数，如下所示:

> **列表(映射(功能，顺序))**

让我们看看如何使用内置的 map 函数来完成上面的代码:

请记住，我们可以将 map 函数应用于任何可迭代对象或序列中的每个元素，而不仅仅是列表。

**那么让我们来分析一下这行代码中发生了什么:**

```
num_list_squared = list(map(squared, num_list))
```

> **map** 函数从 **num_list** 中获取第一个元素，即 1，并将其作为参数传递给**平方**函数(因为我们将该函数作为第一个参数传递给 map 函数)。然后, **squared** 函数返回 1 的平方，也就是 1，它被添加到我们的地图对象中。然后，map 函数从 **num_list** 中取出第二个元素，即 2，并将其作为参数传递给 **squared** 函数。 **squared** 函数返回 2 的平方，也就是 4，然后它被添加到我们的地图对象中。在它完成了对 **num_list** 元素的遍历，并且我们的其余平方数被添加到地图对象之后， **list** 函数将这个地图对象转换成一个列表，并且该列表被分配给变量 **num_list_squared** 。

## **使用λ表达式**

我们可以通过传入一个 lambda 表达式作为我们的函数来进一步缩短代码:

如果你不熟悉 lambda 表达式，你可以看看这个教程:

[](/lambda-expressions-in-python-9ad476c75438) [## Python 中的 Lambda 表达式

### 如何用 python 写匿名函数

towardsdatascience.com](/lambda-expressions-in-python-9ad476c75438) 

**注**:我们传入 map 的函数可以是 python 中的内置函数。例如，如果我们有一个字符串列表，我们想要创建一个包含这些字符串长度的新列表，我们可以像下面这样传递内置的 len 函数:

```
list(map(len, list_of_strings))
```

*了解更多关于可迭代、迭代器和迭代的知识:*

[](/iterables-and-iterators-in-python-849b1556ce27) [## Python 中的迭代器和迭代器

### Python 中的可迭代对象、迭代器和迭代

towardsdatascience.com](/iterables-and-iterators-in-python-849b1556ce27) 

## 过滤功能

同样，假设我们想用一个已经有的列表来创建一个列表。但是这一次我们希望我们的新列表只包含满足给定条件的元素。例如，我们有一个数字列表，我们想创建一个只包含偶数的新列表。我们可以使用 for 循环来完成这项任务，如下所示:

> 我们有一个数字列表， **list_of_nums** ，其中包含数字 1、2、3、4、5 和 6。我们想要创建一个新的数字列表， **list_of_even_nums** ，它只包含来自 **list_of_nums** 的偶数。所以我们创建了一个函数， **is_even** ，它接受一个输入，如果输入是偶数，则返回 True，否则返回 False。然后，我们创建了一个 for 循环，该循环遍历 **list_of_nums** ，并通过将该元素传递给 **is_even** 函数来检查列表中的每个数字是否都是偶数。如果 **is_even** 函数返回 True，则该数字被追加到 **list_of_even_nums** 中。如果 **is_even** 返回 False，则该数字不会追加到 **list_of_even_nums** 中。

另一种实现方法是使用名为 filter 的内置 python 函数。 [filter](https://docs.python.org/3/library/functions.html#filter) 函数接受两个参数:检查特定条件的函数和我们想要应用它的序列(比如本例中的列表)。filter 函数从我们的列表中取出每个元素，并将其传递给我们给它的函数。如果将该特定元素作为参数的函数返回 True，filter 函数将把该值添加到 filter 对象中(然后我们可以从该对象中创建一个列表，就像我们对上面的 map 对象所做的那样)。如果函数返回 False，那么该元素将不会被添加到我们的 filter 对象中。

换句话说，我们可以认为过滤函数是基于某种条件过滤我们的列表或序列。

## **我们用于过滤功能的格式如下:**

> 过滤器(检查条件、我们想要应用的元素序列的函数)

这个过滤函数将返回一个过滤对象，它是一个迭代器。如果我们想从这个过滤器对象创建一个列表，我们需要将过滤器对象传递给内置的 list 函数(就像我们对 map 对象所做的那样),如下所示:

> **列表(过滤(功能，顺序))**

现在让我们使用内置的过滤函数创建与上面相同的列表:

> **filter** 函数从 **list_of_nums** 中获取第一个元素，即 1，并将其作为参数传递给 **is_even** 函数(因为我们将该函数作为第一个参数传递给 filter 函数)。然后 **is_even** 函数返回 False，因为 1 不是偶数，所以 1 没有被添加到我们的过滤器对象中。然后，filter 函数从 **list_of_nums** 中获取第二个元素，即 2，并将其作为参数传递给 **is_even** 函数。 **is_even** 函数返回 True，因为 2 是偶数，因此 2 被添加到我们的过滤器对象中。在遍历完 **list_of_nums** 中的其余元素并且将其余偶数添加到我们的 filter 对象中之后， **list** 函数将这个 filter 对象转换为一个列表，并且该列表被分配给变量 **list_of_even_nums** 。

## **使用λ表达式**

我们可以通过传入一个 lambda 表达式作为我们的函数来进一步缩短代码:

如果你喜欢阅读这样的故事，并想支持我成为一名作家，考虑注册成为一名媒体成员。每月 5 美元，你可以无限制地阅读媒体上的故事。如果你用我的 [*链接*](https://lmatalka90.medium.com/membership) *注册，我会赚一小笔佣金。*

[](https://lmatalka90.medium.com/membership) [## 通过我的推荐链接加入媒体——卢艾·马塔尔卡

### 阅读卢艾·马塔尔卡的每一个故事(以及媒体上成千上万的其他作家)。您的会员费直接支持…

lmatalka90.medium.com](https://lmatalka90.medium.com/membership) 

## 结论

在本教程中，我们学习了 python 内置的地图和过滤函数是如何工作的。我们也看到了一些使用它们的例子。最后，我们看到了 lambda 表达式如何作为参数传递给这些函数。