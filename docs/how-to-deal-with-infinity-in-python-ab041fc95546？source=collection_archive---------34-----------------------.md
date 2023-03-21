# 如何在 Python 中处理无穷大

> 原文：<https://towardsdatascience.com/how-to-deal-with-infinity-in-python-ab041fc95546?source=collection_archive---------34----------------------->

## 介绍在 Python 中定义和使用无限值的方法

![](img/9c68a2ed74dc83bf5b8ae63b5bf155e4.png)

由[鲁本·特奥](https://unsplash.com/@reubenteo?utm_source=medium&utm_medium=referral)在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 拍摄的照片

> 我们只是追求绝对完美的无限探索者。—g·h·哈代

很多人认为无穷大只是一个**非常大的数**。但在现实中，无限是无限的概念，没有任何界限。

从早期开始，数学家们就在用这种永无止境的概念赌博。正如约翰·格林在他的书《我们星星中的错误》中所写的，

> "在一个由其结局定义的宇宙中，无尽是一个非常奇怪的概念."

对于算法设计来说，无穷大有许多可取的特性。最受欢迎的是:

> ∀ x ∈ ℝ，-∞ < x < ∞
> 
> **每个数都小于正无穷大，大于负无穷大。**

在 Python 中，我们可以用多种方式**表示无穷大。我们将在下面讨论三个最受欢迎的。**

# 设置

本文编写的代码在 [**Python 3.6**](https://www.python.org/downloads/release/python-360/) 上进行了测试。这里唯一需要的非标准库是 [**numpy**](https://numpy.org/) 。您可以使用以下命令非常简单地安装它:

```
pip install numpy
```

# 使用 float('inf ')

我们将创建两个变量并用正负无穷大初始化它们。

输出:

```
Positive Infinity:  inf
Negative Infinity:  -inf
```

# **使用数学模块(math.inf)**

另一种表示无穷大的流行方法是使用 Python 的数学模块。看一看:

输出:

```
Positive Infinity:  inf
Negative Infinity:  -inf
```

# 使用 Numpy 模块(numpy.inf)

[Numpy](https://numpy.org/devdocs/reference/constants.html) 对于无穷大的值也有自己的定义。我们也可以创建用无穷大值填充的数组。代码如下所示:

输出:

```
Positive Infinity:  inf
Negative Infinity:  -inf
Infinity Array:  [inf inf inf inf inf inf inf inf inf inf]
```

# 在 Python 中检查一个数是否是无限的

math 模块提供了一个`isinf()`方法，允许我们轻松地检查 Python 中的无限值。它返回一个布尔值。看一看:

输出:

```
For First Infinity: True
For Second Infinity: True
For Third Infinity: True
For Large Number: False
```

# 无限算术

大多数无穷数的算术运算都会产生其他无穷数。无限零悖论成为一个非常有趣的晚餐话题。至少我希望如此。我还没试过。

输出:

```
Addition :  inf
Subtraction :  inf
Multiplication :  inf
Division :  inf

Multiplication by Zero:  nan
```

# 结束语

在最优化问题中经常用到无穷大。例如，在**最短路径算法**中，我们需要将当前距离与迄今为止的最佳或最小距离进行比较。

我们**用正无穷大**初始化未知距离。无论你在程序中输入什么距离，没有一个数能大于这个无穷大的表示。

我们讨论了在 Python 中表示无限值的多种方式。

我个人使用 `math.inf` **方法**来定义一个无穷数。这是因为这种方法通常是三种方法中最快的。另外，`math`是一个标准库，已经是我大部分代码的一部分。

```
%timeit np.inf
# 38.3 ns ± 0.208 ns per loop (mean ± std. dev. of 7 runs, 10000000 loops each)%timeit float('inf')
# 198 ns ± 2.56 ns per loop (mean ± std. dev. of 7 runs, 10000000 loops each)%timeit math.inf
# 37.5 ns ± 0.0933 ns per loop (mean ± std. dev. of 7 runs, 10000000 loops each)
```

但是像往常一样，当我们开始处理数组时，Numpy 变得更快:

```
%timeit [math.inf for _ in range(10000)]
# 585 µs ± 8.29 µs per loop (mean ± std. dev. of 7 runs, 1000 loops each)%timeit np.full(10000, np.inf)
# 10.4 µs ± 49.3 ns per loop (mean ± std. dev. of 7 runs, 100000 loops each)
```

希望你觉得这个教程有用！你可以在这里找到我的其他故事[。](https://medium.com/@chaitanyabaweja1)