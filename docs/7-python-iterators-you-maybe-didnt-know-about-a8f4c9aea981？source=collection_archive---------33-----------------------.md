# 你(可能)不知道的 7 个 Python 迭代器

> 原文：<https://towardsdatascience.com/7-python-iterators-you-maybe-didnt-know-about-a8f4c9aea981?source=collection_archive---------33----------------------->

## 以及为什么你应该关心

![](img/9099f4af02ac19dec43b45dcf1d51aa3.png)

安德鲁·西曼在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄的照片

如果你曾经用 Python 编程过，你可能知道迭代器。这些对象包含可计数数量的值。**列表、元组、字典和集合都是可迭代的**，这意味着你可以对它们调用函数 **iter()** 来创建遍历它们的值的迭代器。使用函数 **next()** 遍历迭代器。For 循环即时从可迭代对象实例化迭代器。

除了 Python 结构上的标准迭代器，Python 标准库中还有更高级的迭代器。它们都位于 **itertools** 模块中。这里我解释 10 个最有趣的。

# 数数

简单来说， **count()** 和 **range()** 类似，但是无穷无尽。它采用一个*开始*和一个可选的*步长*参数，并在此之后输出无穷多个值。如果您想要迭代某些值，直到满足特定条件，这是很有用的。与 map 结合使用时，它可以这样使用:

```
import itertoolsdef sqr(n):
   return n*nfor i in map(sqr, itertools.count(10)):
   if i > 1000:
      break
   print(i)
```

这段代码输出 i < 1000.

# cycle

As the name implies, **cycle(p)** 循环的所有值，循环次数不限。这个迭代器有用的一个例子是以循环顺序给列表元素分配标签:

```
import itertoolsa = ‘ABCDEFGH’
print(list(zip(a, itertools.cycle(range(2)))))
```

在这种情况下，输出将具有值[('A '，0)，(' B '，1)，(' C '，0)，…]。

# 链条

将多个可重复项链接在一起。像这样使用它:

```
import itertoolsa = ‘ABCD’
b = ‘EFGH’
print(list(itertools.chain(a,b)))
```

输出将是“ABCDEFGH”。

# 星图

该函数采用另一个函数和一个 iterable 作为参数，如下所示:

```
itertools.starmap(function, iterable)
```

*starmap* 与 *map* 非常相似，除了它也允许输入函数采用多个参数，这些参数已经分组在 iterable 的元组中，如下所示:

```
import itertoolsdef compute_sqr_sum(x, y):
   return x**2 + y**2a = [(x,x+1) for x in range(4)]
print(list(itertools.starmap(compute_sqr_sum, a)))
```

在这种情况下，输出是[1，5，13，25]。

# 产品

生成参数中可迭代项的笛卡尔乘积。它相当于多个嵌套循环。当你需要计算所有项目组合时，你可以使用它。可选的 *repeat* 参数允许你计算一个 iterable 与其自身的笛卡尔积。例如，您可能会这样写:

```
import itertoolsa = [1, 2, 3]
print(list(itertools.product(a,repeat=2)))
```

输出为[(1，1)、(1，2)、(1，3)、(2，1)、(2，2)、(2，3)、(3，1)、(3，2)、(3，3)]。

# 伊斯利斯

从 iterable 中返回特定的切片。这和常规的*切片()*很像。然而，slice()创建了使用它的字符串、列表或元组的副本。

> 相比之下， *islice(iterable，start，stop，[step])* ，除了 iterable 之外，几乎具有完全相同的语法，它返回 iterable，因此速度更快，因为元素是动态生成的。

因此，如果内存效率起作用的话， *islice* 是首选方法。 *islice* 的一个警告是它不能使用负索引(因为它总是从开始迭代 iterable，而 iterable 甚至可能不是有限长度的)。如何使用*的例子是 ice* :

```
import itertoolsgen = itertools.count()
print(list(itertools.islice(gen, 2, 5)))
```

输出将是[2，3，4]。

# 积聚

这个函数在很多函数式编程语言中也被称为 *fold* 。它允许您迭代地将一个特定的二元函数应用于一个列表的元素，并将集合放入一个新的列表中。accumulate 的默认函数是运算符 add，用于将 iterable 中的项相加，例如[1，2，3，4]->[1，2，6，10]。此时，我将向您展示一个使用多个迭代器的稍微复杂一些的示例:

```
import itertools
import operatorgen = itertools.count(1)
factorials = itertools.accumulate(itertools.count(1), func=operator.mul)
fac_and_nums = zip(gen, factorials)print(list(itertools.islice(fac_and_nums, 2, 5)))
```

你认为输出会是什么？

顺便说一下，使用迭代器时必须小心，因为每次调用 *next()* 函数时迭代器都会递增。因此，这段代码没有实现作者的意图:

```
import itertools
import operatorgen = itertools.count(1)
factorials = itertools.accumulate(gen, func=operator.mul)
fac_and_nums = zip(gen, factorials)print(list(itertools.islice(fac_and_nums, 2, 5)))
```

你能找出原因吗？

# 结论

虽然 Python 绝对不是函数式语言，但是它借用了一些非常有趣的函数式概念。将这些迭代器与 map 之类的函数结合起来，创造了一个全新的机会世界，并允许编写快速有效的对象代码处理序列。

itertools 中还有一些迭代器。所有这些都可能在某种情况下有用。如果你想了解更多，不要害怕看一下官方 Python 文档！