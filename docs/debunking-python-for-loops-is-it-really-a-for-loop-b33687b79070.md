# 揭穿 Python For 循环:For 循环的幕后

> 原文：<https://towardsdatascience.com/debunking-python-for-loops-is-it-really-a-for-loop-b33687b79070?source=collection_archive---------36----------------------->

## 了解 Python 中 for 循环的实际工作方式，以及对 Python 中迭代器的理解。

![](img/af9842d9e9ac025b28420efb0947670a.png)

蒂内·伊万尼奇在 Unsplash.com 的照片

循环。任何编程语言中最基本的构造之一。一般来说，循环帮助遍历一系列对象，一次访问一个元素。与其他流行的编程语言一样，Python 也有自己的循环实现，并附带了 While 和 For 循环，所做的事情或多或少与循环相同。

当我在探索 python 的时候，我发现 For 循环中发生了一些非常惊人的事情，我认为这是值得一写的。乍一看，在一个 for 循环中会发生如此惊人的事情，这似乎很平常，但实际上非常有趣。本文将涵盖以下内容:

*   迭代器，它们是什么，如何工作。
*   Python 中 for 循环的实际实现。

# Python 中的迭代器

在我们开始揭穿“For 循环”之前，我们需要首先建立对迭代器的理解，因为迭代器在它的工作中起着重要的作用。

## 什么是迭代器？

> 在 python 中，一切都是一个对象，就像 python 中的任何其他对象一样，**迭代器**也是一个对象，但这个对象可以被**迭代到**上，并且在调用时一次返回一个元素。

迭代器在 python 中随处可见。我们将在列表理解、生成器中找到迭代器，正如你可能已经猜到的，它们也在“For 循环”中实现。

## 迭代器是如何工作的？

如前所述，迭代器是一个可以被迭代的对象。创建迭代器对象的必要条件是它必须实现两个特殊的方法

*   __iter__()方法。当调用 iterable 时，这个方法返回一个 iterator 对象。

> 一个 **iterable** 是一个对象，我们可以从中获得一个迭代器。例如列表、元组、字符串。

*   __next__()方法。这个方法一次从一个 iterable 中返回一个元素。它用于迭代一个迭代器对象。

让我们看下面一个简单的代码示例，以便更好地理解迭代器。

```
# a list which is an iterable
numbers = [1,2,3,4,5]# creating an iterator object from the iterable (numbers)
num_iterator = iter(numbers) # returns an iterator#Iterating through num_iterator using next() method
print(next(num_iterator)) # output: 1
print(next(num_iterator)) # output: 2
print(next(num_iterator)) # output: 3
print(next(num_iterator)) # output: 4
print(next(num_iterator)) # output: 5# when no more elements left, exception is raised
print(next(num_iterator)) # StopIteration Exception Raised
```

使用 iter()方法，我们从数字列表中创建了一个迭代器对象。然后，我们使用 next()方法手动遍历迭代器的所有元素。一旦我们到达数字的末尾，如果我们继续调用迭代器上的 next()方法，就会引发 StopIteration 异常。

# Python 中 For 循环的实际实现

既然我们对 python 中迭代器的工作原理有了很好的理解，现在我们可以看看 Python 中的 **For 循环**的幕后发生了什么。

## 需要 for 循环吗？

从前面的代码片段中我们可以看到，我们手动遍历列表，这对于一个包含大量数字或元素的列表来说是非常繁忙的。一定有更好更优雅的方法来做同样的事情，那就是 via For 循环。因此，我们之前的代码可以转换为以下代码:

```
for number in numbers:
    print(number) # output: 1,2,3,4,5
```

只需两行代码和相同的功能。太好了。

## For 幕后循环

现在看一下下面的代码片段

```
num_iterator = iter(numbers)while True:
    try:
        # get the next item
        number = next(num_iterator)
    except StopIteration:
        break
```

看着上面的代码，你可能想知道为什么这里有 while 循环，而我们一直在谈论 for 循环？你现在看到的实际上是一个 **for 循环背后发生的事情。**上面的代码是 python 中一个 **for 循环的实际实现。**

每当对 iterable 调用 for 循环时，在内部使用 iter()方法从 iterable 创建迭代器对象，并使用无限 while 循环在 try/catch 块中使用 next()方法迭代迭代器对象的值。一旦我们到达迭代器中的元素末尾，就会引发 **StopIteration** 异常，我们就跳出了无限循环。

# 所以“for 循环”只是一个无限的“while 循环”,带有迭代器的实现。讽刺。

# 摘要

在本文中，我们学习了什么是迭代器，它们是如何工作的，以及一个 for 循环是如何在 python 内部实现的，它只是一个无限的 while 循环。

如果你喜欢这篇文章，请看看我的其他文章。帮我达到 500 个追随者，跟随我在媒体上获得更多这样惊人的内容。谢了。

[](https://medium.com/@furqan.butt) [## Furqan 黄油培养基

### 在介质上阅读 Furqan Butt 的文字。大数据工程师，Python 开发者。让我们连接…

medium.com](https://medium.com/@furqan.butt)