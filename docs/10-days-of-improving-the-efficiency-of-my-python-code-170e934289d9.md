# 10 天来改进我的 Python 代码

> 原文：<https://towardsdatascience.com/10-days-of-improving-the-efficiency-of-my-python-code-170e934289d9?source=collection_archive---------51----------------------->

## 对代码进行小的日常修改如何对效率产生大的影响

![](img/6fd544f0d8665c577957c621180a0634.png)

照片由 [chuttersnap](https://unsplash.com/@chuttersnap?utm_source=medium&utm_medium=referral) 在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄

在之前的一篇文章中，我谈到了通过每天早上对你前一天写的代码做一个修改来提高你的 Python 代码的效率。无论我做了什么更改，我都会确保在将来使用新的方法，这意味着我的代码的质量和效率总体上有所提高。

做出这些改变后，我会给我的团队发一条消息，让他们知道我做了什么。以下是我在头 10 天发出的消息，它们被汇编成一篇文章，这样你就可以看到这种日常练习对你的代码的重大影响。

*注意:这些是我在两周的工作中用来改进代码的真实例子。其中一些可能看起来有点随意或明显，但它们都适用于我当时的代码。不管怎样，我希望你能理解和欣赏代码改进的基本方法。*

# 第一天

**仔细定义 Pandas** 中的列类型如果您正在使用 [Pandas](https://pandas.pydata.org/) 中的大型数据集，那么在使用`dtype`参数读取数据或使用`astype`更改类型时，定义模式总是值得的。存储为 0 和 1 的布尔值通常被设置为 int32 或 int64 类型。在某些情况下，将它们转换为 bool 类型可以大大减少数据帧占用的内存。

将列的类型从 int64 更改为 bool 会导致该列占用八分之一的内存

# 第二天

**快速遍历 Pandas DataFrame** 如果您需要遍历 Pandas DataFrame，那么遍历作为 [NumPy](https://numpy.org/) 数组的值可能会更快。在下面的例子中，这导致代码运行速度提高了两个数量级！

迭代数据帧的值而不是使用 iterrows 可以使时间减少 2 个数量级

# 第三天

**海象操作符** 从 [Python 3.8](https://docs.python.org/3/whatsnew/3.8.html) 开始，海象操作符(:= see eyes and tucks)已经可以用于变量的赋值，同时也可以用作更大表达式的一部分。

```
if (n := len(a)) > 5: print(f’The length of a is {n}’)
```

代替

```
n = len(a)
if n > 5:
   print(f’The length of a is {n}’)
```

![](img/06980e2474c5d131944ce5782877b613.png)

照片由 [Jay Ruzesky](https://unsplash.com/@wolsenburg?utm_source=medium&utm_medium=referral) 在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄

# 第四天

**羽毛文件格式** 在漫长的数据处理流水线中，在关键的里程碑处保存输出是一个很好的做法。 [Apache arrow](https://arrow.apache.org/) (你需要[安装 pyarrow](https://arrow.apache.org/docs/python/install.html) )已经创建了[羽化格式](https://github.com/wesm/feather)，非常适合这些情况。文件比 csv 小，它是语言不可知的(可以在 R，Python，Julia 等中使用)。)，而且它的读/写速度一般只受本地磁盘性能的速度限制。Pandas 能够读写 feather 文件，因此当您想要快速保存中间数据帧时，它是完美的。

羽化格式几乎是 CSV 格式的三分之一，读/写速度更快

# 第五天

**字符串格式化** Python 提供了许多不同的[方法来格式化字符串](https://docs.python.org/3.4/library/string.html#format-examples)，其中一个要避免的就是将字符串加在一起。这增加了内存占用，因为 Python 必须在将每个单独的字符串相加之前为它们分配内存。两个更好的选择是使用%符号，或 f 字符串。f 字符串通常是最快的选择，可以让你的代码更容易阅读。

f 字符串速度更快，提高了代码的可读性。(是的，58 个，还在继续)

# 第六天

**Cython Jupyter 扩展** Cython 是 Python 的库和超集语言。它允许你编写编译成 C 语言的 Python 代码，所以它通常运行得更快。 [Jupyter 笔记本](https://jupyter.org/)有一个[扩展](https://cython.readthedocs.io/en/latest/src/quickstart/build.html#using-the-jupyter-notebook)用于在一个单元内运行 Cython。和 C 一样，你需要确保使用`cdef`来指定每个变量的类型。

*注意:记得先* [*安装 cython*](https://cython.readthedocs.io/en/latest/src/quickstart/install.html) *。*

Cython 将这个斐波那契函数的速度提高了 2 个数量级！

# 第七天

**布尔比较** 有几种方法可以将变量与布尔进行比较。在 if 语句中，删除比较运算符通常是最快的方法。时差可能看起来不是很大，但这是一个很容易做到的改变，可以使您的代码更具可读性。

对布尔变量使用不带比较运算符的 if 语句会更快，并提高可读性

# 第八天

**NumPy 掩码数组** NumPy[masked array](https://numpy.org/doc/stable/reference/maskedarray.generic.html)类是 NumPy 数组类的子集，在有数据缺失时使用。当一个特定的值已经被用于缺失数据，并且您想从一个操作中排除它时，例如 np.max，它特别有用。

MaskedArray 可以从操作中排除丢失的数据，而无需从对象中移除数据

# 第九天

**排序函数** Python 中有几种排序的方法。两个内置方法是`sorted`和`sort`，用于基本排序，可以互换使用。然而，因为`sort`方法就地修改列表，而不是像`sorted`那样创建一个新列表，所以它通常更快。即使在使用`key`参数时也是如此，它在排序前对每个元素应用一个函数。

然而，值得注意的是`sorted`方法的功能比`sort`广泛得多。例如，`sorted`可以接受任何 iterable 作为输入，而`sort`只能接受一个列表。

对于简单的排序任务，排序方法通常比 sorted 快

# 第 10 天

**使用 pandas 进行多重处理** 沿给定轴应用的 Pandas 操作，如 apply 和 map 方法，非常容易并行化，其中沿轴的每一行/列都被单独处理。当数据帧很大或者您想要应用/映射计算开销很大的函数时，这尤其有用。这可以使用 Python 多处理库相对简单地实现。

但是，请记住，对于较小的数据，在内核之间分配作业所花费的时间可能会超过在单个内核上顺序运行作业所花费的时间。

多处理库可用于在多个内核之间分配作业以加快处理速度，但对于较小的数据帧，处理速度通常会较慢

对一些读者来说，这些变化中的一些似乎是显而易见的或无用的，而对其他人来说，它们可能是你从未见过的或真正有用的。不管怎样，我希望看到这篇文章能让你明白，每天花一点时间改进你的 Python 代码(或任何其他语言)会对你的编码技能和效率产生持久的影响。