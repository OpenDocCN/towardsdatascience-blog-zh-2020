# 探索 Python 范围函数

> 原文：<https://towardsdatascience.com/exploring-python-range-function-d509ebd36ec?source=collection_archive---------4----------------------->

## 了解 Python Range()函数及其功能

![](img/248b88ac078852d9b521196f2f9f2c2e.png)

照片由[凯](https://unsplash.com/@kaysha?utm_source=medium&utm_medium=referral)在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄

# 介绍

`range()`是 Python 中的内置函数。默认情况下，它返回一个从零开始递增 1 的数字序列，并在给定的数字之前停止。

现在我们知道了范围的定义，让我们看看语法:

```
**range***(start, stop, step*)
```

它有三个参数，其中两个是可选的:

*   `**start:**`可选参数，用于定义序列的起点。默认情况下是零。
*   `**stop:**`这是一个强制参数，用来定义序列的停止点
*   `**step:**`它也是一个可选参数，用于指定每次迭代的增量；默认情况下，该值为 1。

# 一般用法

由于它返回一个数字序列，大多数开发人员使用这个范围来编写循环。当您没有列表或元组，而只有实现循环的特定值时，这很方便。

## 变奏一

这里，我们将实现一个只有一个参数— `stop`值的 for 循环。

这里`x` 是我们用来实现循环的范围，`n` 是每次迭代中的值。观察到输出在`stop`值之前结束；它绝不是类似于`list.size()`的范围迭代的一部分。

## 变奏二

这里我们将使用 start 和 stop 作为参数来实现 for 循环。

## 变奏三

现在，我们将使用所有三个参数:`start`、`stop`和`step`。看一看:

当步长值为 2 时，循环在每次迭代时增加 2，而不是增加 1。我们需要记住的一件重要事情是步长值永远不应该为零；否则，它将抛出一个`ValueError`异常。

## 列表类型上的迭代

除了循环之外，`range()`还用于使用`len`函数遍历列表类型，并通过`index`访问值。看一看:

# 反向范围

对于该范围内的任何参数，我们可以给出正数或负数。这个特性提供了实现反向循环的机会。我们可以通过传递一个更高的索引作为`start`和一个负的`step`值来实现。看一看:

# 使用范围创建列表、集合和元组

range()在很多情况下都很方便，而不仅仅是用来写循环。例如，我们使用范围函数创建列表、集合和元组，而不是使用循环来避免样板代码。看一看:

为了更有趣一点，我们可以在 step 中传递负值来创建升序列表。看一看:

# 索引范围

就像我们使用索引访问列表中的值一样，我们可以对 range 做同样的事情。语法也类似于列表索引访问。

# 浮动范围内的参数

默认情况下，`range()`函数只允许整数作为参数。如果您传递流值，那么它会抛出以下错误:

```
TypeError: 'float' object cannot be interpreted as an integer
```

但是有一个解决方法；我们可以编写一个类似下面的自定义 Python 函数。它将允许您为步长参数指定一个浮点值。

# 奖金

要了解更多关于 Python 从基础到高级的知识，请阅读以下文章

*   [“Python 初学者—基础知识”](https://medium.com/android-dev-hacks/python-for-beginners-basics-7ac6247bb4f4)
*   [《Python 初学者——函数》](/python-for-beginners-functions-2e4534f0ae9d)
*   [《Python 初学者——面向对象编程》](https://medium.com/better-programming/python-for-beginners-object-oriented-programming-3b231bb3dd49)
*   [《Python 初学者——控制语句》](/python-for-beginners-control-statements-6feadf4ac4c)

就这些了，希望你能学到一些有用的东西，感谢阅读。

你可以在 [Medium](https://medium.com/@sgkantamani) 、 [Twitter](https://twitter.com/SG5202) 、 [Quora](https://www.quora.com/profile/Siva-Ganesh-Kantamani-1) 和 [LinkedIn](https://www.linkedin.com/in/siva-kantamani-bb59309b/) 上找到我。