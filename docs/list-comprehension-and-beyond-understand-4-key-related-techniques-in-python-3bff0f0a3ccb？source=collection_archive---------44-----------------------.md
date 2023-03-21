# 列表理解和超越——理解 Python 中的 4 个关键相关技术

> 原文：<https://towardsdatascience.com/list-comprehension-and-beyond-understand-4-key-related-techniques-in-python-3bff0f0a3ccb?source=collection_archive---------44----------------------->

## 中级 Python 知识

## 它们比你想象的要简单

![](img/25946530420dda20c75058f478e50dd3.png)

由[费尔伯特·曼贡达普](https://unsplash.com/@filbertmang?utm_source=medium&utm_medium=referral)在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄的照片

当我们学习 Python 时，列表理解是一项棘手的技术，需要花一些时间才能完全理解它。在我们学会它之后，我们喜欢使用它，因为它是展示我们在 Python 中的编码专业知识的一种简洁的方式。特别是，当我们有机会让初学者阅读我们的代码时，他们会惊讶地发现 Python 中存在如此简洁的创建列表的方法。实际上，他们可能不知道的是，理解 list comprehension 的语法对他们理解其他一些关键的 Python 技术是有用的。在本文中，让我们一起来探索它们。

## 1.列表理解

我们先来复习一下什么是列表理解。它有以下基本语法:`[expression for item in iterable]`。本质上，它遍历一个 iterable，执行创建一个项的特定操作，并返回由这些项组成的列表。考虑下面的例子。

```
>>> # create a list of words
>>> words = ["quixotic", "loquacious", "epistemology", "liminal"]
>>> # create a list of numbers counting the letters
>>> letter_counts = [len(x) for x in words]
>>> letter_counts
[8, 10, 12, 7]
```

在上面的代码中，我们创建了一个名为`letter_counts`的数字列表，每个数字都是`words`列表中每个单词的字母数。很简单，对吧？

让我们做一些更有趣的东西。在下面的代码中，我们通过使用`if`语句过滤`words`列表来创建一个大写单词列表。

```
>>> # create a list of uppercased words with letter count > 8
>>> uppercased = [x.upper() for x in words if len(x) > 8]
>>> uppercased
['LOQUACIOUS', 'EPISTEMOLOGY']
```

## 2.词典理解

除了列表理解，Python 还有一种类似的创建字典的技术，称为字典理解。它有以下基本语法:`{exp_key: exp_value for item in iterable}`。如您所见，该表达式与 list comprehension 相似，都有迭代部分(即`for item in iterable`)。

有两个区别。首先，我们使用花括号进行字典理解，而不是方括号进行列表理解。第二，字典理解有两个表达式，一个表示键，另一个表示值，与列表理解中的一个表达式相反。

让我们看看下面的例子。我们有一个元组列表，每个元组保存学生的姓名和分数。接下来，我们使用字典理解技术创建一个字典，其中名称是键，分数是值。

```
>>> # create a list of tuples having student names and scores
>>> scores = [("John", 88), ("David", 95), ("Aaron", 94)]
>>> # create a dictionary using name as key as score as value
>>> dict_scores = {x[0]: x[1] for x in scores}
>>> dict_scores
{'John': 88, 'David': 95, 'Aaron': 94}
```

为了使我们的例子更有趣(从而可以学到更多)，我们可以将条件赋值与字典理解(实际上也包括列表理解)结合起来。考虑下面仍然使用`scores`列表的例子。

```
>>> # create the dictionary using name as key as grade as value
>>> dict_grades = {x[0]: 'Pass' if x[1] >= 90 else "Fail" for x in scores}
>>> dict_grades
{'John': 'Fail', 'David': 'Pass', 'Aaron': 'Pass'}
```

## 3.集合理解

我们都知道 Python 中有三种主要的内置集合数据结构:列表、字典和集合。既然有列表和字典理解，很惊讶的知道还有集合理解。

集合理解的语法如下:`{expression for item in iterable}`。语法与 list comprehension 几乎相同，只是它使用了花括号而不是方括号。让我们通过下面的例子来看看它是如何工作的。

```
>>> # create a list of words of random letters
>>> nonsenses = ["adcss", "cehhe", "DesLs", "dddd"]
>>> # create a set of words of unique letters for each word
>>> unique_letters = {"".join(set(x)) for x in nonsenses}
>>> unique_letters
{'d', 'cdas', 'eLsD', 'ceh'}
```

在上面的代码中，我们用随机字母创建了一个名为`nonsenses`的无意义单词列表。然后我们创建一组名为`unique_letters`的单词，每个单词都由单词的唯一字母组成。

需要注意的一点是，在 Python 中，集合不能有重复值的项，因此 set comprehension 会自动为我们删除重复项。请查看该特性的代码。

```
>>> # create a list of numbers
>>> numbers = [(12, 20, 15), (11, 9, 15), (11, 13, 22)]
>>> # create a set of odd numbers
>>> unique_numbers = {x for triple in numbers for x in triple}
>>> unique_numbers
{9, 11, 12, 13, 15, 20, 22}
```

在上面的代码中，我们从列表`numbers`中创建了一个名为`unique_numbers`的集合，其中包含元组项。如您所见，列表中的重复数字(例如 11)在集合中只有一个副本。

这里的一个新东西是我们使用了一个嵌套的理解，它的语法如下:`expression for items in iterable for item in items`。这种技术在 iterable 包含其他集合的情况下很有用(例如`list`中的`list`或`list`中的`tuple`)。值得注意的是，我们可以将这种嵌套理解用于列表、字典和集合理解。

## 4.生成器表达式

我们知道我们用花括号来表示集合理解，用方括号来表示列表理解。如果我们使用圆括号，比如`(expression for item in iterable)`，会怎么样？好问题，由此引出生成器表达式的讨论，也有人称之为生成器理解。

换句话说，当我们使用括号时，我们实际上是在声明一个生成器表达式，它创建了一个生成器。**生成器是 Python 中的“懒惰”迭代器。**这意味着生成器可以用在需要迭代器的地方，但它会提供所需的项，直到它被请求(这就是为什么它被称为“懒惰”，[一个编程术语](https://en.wikipedia.org/wiki/Lazy_evaluation))。让我们看看下面的例子。

```
>>> # create the generator and get the item
>>> squares_gen = (x*x for x in range(3))
>>> next(squares_gen)
0
>>> next(squares_gen)
1
>>> next(squares_gen)
4
>>> next(squares_gen)
Traceback (most recent call last):
  File "<stdin>", line 1, in <module>
StopIteration
```

在上面的代码中，我们使用生成器表达式创建了一个名为`squares_gen`的生成器。使用内置的`next()`方法，我们能够从生成器中检索下一个项目。但是，当生成器用完物品时，会引发一个`StopIteration`异常，表示所有物品已经用完。

由于它的惰性求值特性，生成器是一种内存高效的技术，可以迭代一个巨大的条目列表，而不需要首先创建 iterable。例如，我们处理一个非常大的文件，将整个文件读入内存可能会耗尽计算机的 RAM，导致它无法响应。相反，我们可以使用生成器表达式技术，就像这个`(row for row in open(filename))`，它允许我们逐行读取文件以最小化内存使用。

为了说明生成器表达式是如何工作的，让我们考虑一个简化的例子。在下面的代码中，我们创建了一个包含 1 亿个数字的列表和生成器，每个数字都是一个正方形。显然，当我们检查它们的大小时，生成器使用的内存比列表少得多。

```
>>> # a list of 100 million numbers
>>> numbers_list = [x*x for x in range(100000000)]
>>> numbers_list.__sizeof__()
859724448
>>> # a generator of 100 million numbers
>>> numbers_gen = (x*x for x in range(100000000))
>>> numbers_gen.__sizeof__()
96
```

如果我们的目标是计算这些数字的总和，那么两种选择都会得到相同的结果。但是重要的是，在计算出总和之后，生成器不会产生任何额外的项，如下所述。如果您确实需要多次使用 iterable，您可以在每次需要时创建一个列表或创建一个生成器，后者是一种更节省内存的方式。

```
>>> # calculate the sum
>>> sum(numbers_list)
333333328333333350000000
>>> sum(numbers_gen)
333333328333333350000000
```

## 结论

在本文中，我们研究了 Python 中的四种重要技术，所有这些技术在语法上都包含相同的组件。下面是这些技术的快速回顾和它们用例的亮点。

*   **列表理解** : `[expression for item in iterable]` —创建列表的简明方法
*   **词典理解** : `{exp_key: exp_value for item in iterable}` —一种创建词典的简明方法
*   **集合理解** : `{expression for item in iterable}` —创建集合的简明方法(无重复项)
*   **生成器表达式** : `(expression for item in iterable)` —创建生成器的简洁方法(内存高效)

## 关于作者

我写关于 Python 和数据处理与分析的博客。万一你错过了我以前的一些博客，这里有一些与当前文章相关的文章链接。

[](https://medium.com/better-programming/30-simple-tricks-to-level-up-your-python-coding-5b625c15b79a) [## 提升 Python 编码水平的 30 个简单技巧

### 更好的 Python

medium.com](https://medium.com/better-programming/30-simple-tricks-to-level-up-your-python-coding-5b625c15b79a) [](https://medium.com/better-programming/9-things-to-know-to-master-list-comprehensions-in-python-8bc0411ec2ed) [## 掌握 Python 中列表理解的 9 件事

### 本教程将帮助你学习 Python 中列表理解的最常见用法

medium.com](https://medium.com/better-programming/9-things-to-know-to-master-list-comprehensions-in-python-8bc0411ec2ed) [](https://medium.com/swlh/understand-pythons-iterators-and-iterables-and-create-custom-iterators-633939eed3e7) [## 理解 Python 的迭代器和可迭代对象，并创建自定义迭代器

### 迭代是 Python 中最重要的概念之一。与迭代相关的两个术语是迭代器和…

medium.com](https://medium.com/swlh/understand-pythons-iterators-and-iterables-and-create-custom-iterators-633939eed3e7)