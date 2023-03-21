# 优化您的 Python 代码

> 原文：<https://towardsdatascience.com/optimizing-your-python-code-156d4b8f4a29?source=collection_archive---------4----------------------->

## 使用 Python 处理大量数据的基本技巧

![](img/5c63acf00277c662fb96b2ff7fafea0e.png)

照片由[克里斯里德](https://unsplash.com/@cdr6934?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)在 [Unsplash](https://unsplash.com/s/photos/data?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) 拍摄

虽然您只是偶尔需要运行一个分析器来分析您的代码并找到[瓶颈](https://annaeastori.medium.com/profiling-in-python-83415daa844c)，但是养成编写高效代码的习惯并找出您可以立即改进的地方绝对是一个好主意。

在寻找优化代码的方法时，要记住的一件重要事情是，很可能总会有一些折衷要接受。例如，它要么是一段运行更快的代码，要么是一段更简单的代码。这里的简单不仅仅指“看起来不那么酷的代码”(想想著名的 Python“一行程序”)。更简单的代码意味着更容易维护和测试。

此外，关于如何优化 Python 代码的每一个技巧都需要根据您的情况进行严格的检查。当然，有一般性的观察，但是你也可能有一个上下文，在这个上下文中那些观察是不相关的，或者甚至产生相反的结果。所以你需要了解“幕后”发生了什么，以及它将如何在你的案例中起作用。

然而，在某些情况下，你确实需要给你的代码一点提升，这里是我在学习 Python 时发现的一些有用的地方。

## 1.列出理解

在 Python 中，一个伟大的语法结构是 [list comprehensions](https://docs.python.org/3/tutorial/datastructures.html#list-comprehensions) ，它在创建列表时比传统的循环计算效率更高。因此，如果您需要一个二进制特征向量作为数据点，其中所有负数将被分配 0，其余的将被分配 1，而不是:

```
>>> input_list = [1, 2, -3]
>>> output_list = []>>> for x in input_list:
...    if x >= 0:...        output_list.append(1)...    else:...        output_list.append(0)>>> output_list[1, 1, 0]
```

您可以:

```
>>> output_list = [1 if x >= 0 else 0 for x in input_list]>>> output_list[1, 1, 0]
```

您可以尝试使用`[timeit](https://docs.python.org/3/library/timeit.html)`模块来比较哪种实现运行得更快。

你也可以像嵌套循环一样使用嵌套列表理解，但是通常不鼓励这样做，因为这样会使代码更难阅读、维护和测试。

## 2.尽可能避免 for 循环和列表理解

事实上，在前面的例子中，如果您创建一个只有一个初始化值的向量，而不是使用低效的 for 循环甚至列表理解，您可以这样做:

```
>>> my_list2 = [0] * len(my_list1)
>>> my_list2
[0, 0, 0]
```

## 3.避免不必要的功能

一个很好的例子是函数调用，这种方法可以帮助减少大量的运行时复杂性，但需要仔细考虑权衡。虽然您确实希望函数提供良好的抽象性、可扩展性和可重用性，但是您可能不希望为每一件事情都提供一个函数，因为在 Python 中函数调用是非常昂贵的(如果您感兴趣，在这篇[文章](https://ilovesymposia.com/2015/12/10/the-cost-of-a-python-function-call/)中有一些有趣的观察)。所以有时候，你可能想要牺牲，例如，写一个 getter 和/或 setter。另一方面，函数越少，代码的可测试性越差。因此，最终的决定实际上取决于您的具体应用。

## 4.尽可能使用内置的

另一个与函数相关的提示是选择[内置函数](https://docs.python.org/3/library/functions.html)，如`max()`、`sum()`、`map()`、`reduce()`等。而不是自己完成这些计算——它们通常用 C 语言编写，运行速度会更快。此外，如果您使用内置函数——您需要为自己编写的测试代码会更少。因此，举例来说，如果您想要一个包含所有`input_list`绝对值的集合，您可以这样做:

```
>>> output_set = set(map(abs, input_list))
>>> output_set
{1, 2, 3}
```

如果您正在处理文本数据，用于字符串连接，而不是`+=`:

```
>>> sentence_list = ['This ', 'is ', 'a ', 'sentence.']
>>> sentence = ''
>>> for i in sentence_list:
...     sentence += i
>>> sentence
'This is a sentence.'
```

使用`[str.join()](https://docs.python.org/3/library/stdtypes.html#str.join)`:

```
>>> sentence = ''.join(sentence_list)
>>> sentence
'This is a sentence.'
```

使用`+=`，Python 为每个中间字符串分配内存，如果使用了`str.join()`，则只分配一次。

使用`[operator.itemgetter](https://docs.python.org/3/library/operator.html#operator.itemgetter)`进行分类。例如，如果您有一个名和姓的元组列表，如下所示:

```
>>> my_tuples =[('abbie', 'smith'), ('jane', 'adams'), ('adam', 'johnson')]
```

默认排序将返回以下内容:

```
>>> sorted(my_tuples)
[('abbie', 'smith'), ('adam', 'johnson'), ('jane', 'adams')]
```

如果你想按姓氏而不是名字来排序，你可以这样做:

```
>>> sorted(my_tuples, key=operator.itemgetter(1))
[('jane', 'adams'), ('adam', 'johnson'), ('abbie', 'smith')]
```

## 5.避开圆点

如果您有一个对象，并且正在使用它的一些属性，请先将它们赋给局部变量:

```
rectangle_height = rectangle.heightrectangle_width = rectangle.width
```

所以如果你在代码的后面计算，比如说，它的表面，你会做:

```
surface = rectangle_height * rectangle_width
```

如果以后你也计算它的周长，你将重复使用相同的变量:

```
perimeter = 2 * rectangle_height + 2 * rectangle_width
```

您可以再次使用`timeit`模块来验证它为您节省了对矩形对象及其属性的每个引用的查找时间。

出于同样的原因，通常不鼓励使用全局变量——你不希望浪费时间首先查找全局变量本身，然后查找你可能引用的它的每个属性。

这也适用于函数引用。例如，如果您正在处理一个数据点序列，并且正在将每个结果项追加到一个列表中，您可以执行以下操作:

```
push_item = my_list.append
```

然后将其应用于每个结果项目:

```
push_item(item)
```

## 6.了解您的数据结构，并了解它们在您的 Python 版本中是如何工作的

除了每个典型数据结构的一般属性之外，例如，从链表中检索一个条目的复杂性，了解 Python 数据结构是如何实现的以及哪里可以节省一些 CPU 或内存也是很好的。例如，如果你在字典中查找一个键，你甚至不需要引用`dict.keys()`，这在 Python 3 中会慢一点。你可以简单地做:

```
>>> if k in dict:...    do_something(k)
```

在 Python 2 中，`dict.keys()`甚至用来创建一个额外的键列表！

## 7.明智地选择一种方法

同时，不要忘记看一看更大的画面。如果您有一个要处理的项目列表，并且您知道在集合中查找是 O(1)对列表中的 O(n ),那么您可能会尝试将列表转换为集合:

```
>>> my_set = set(my_list)
```

然而，如果你只是查找列表中的一项，如果你先把列表变成一个集合，你可能会让事情变得更糟。在幕后，Python 将遍历整个列表，并将每个项目添加到一个新创建的集合中。创建集合的开销会使您失去在集合中查找的优势。

另一方面，如果您想有效地从列表中删除重复项，将它转换为 set 可能是一个不错的选择(尽管 Python 中还有其他选项可能更适合您的情况)。

因此，正如我之前提到的，有一些一般性的观察，但是你需要仔细检查它们，以了解它们如何在你的案例中起作用。

有很多更高级的方法来管理您的代码，以便使它更有效和执行得更快——从并行性或并发性到不同的技巧，如设计更小的对象，以便它们适合堆内存的缓存层而不是主缓存层。对于某些任务，您可能能够使用实际上为优化这些任务而设计的库。然而，以上是我在学习 Python 时不得不开始注意的第一件事。你在使用什么技巧和方法？

如果您在数据科学领域使用 Python，请随意查看我关于 Python 3.9 中的[新特性](/dictionary-union-operators-and-new-string-methods-in-python-3-9-4688b261a417)和 Python 中的[并发性](https://annaeastori.medium.com/concurrency-and-parallelism-in-python-bbd7af8c6625)的文章！