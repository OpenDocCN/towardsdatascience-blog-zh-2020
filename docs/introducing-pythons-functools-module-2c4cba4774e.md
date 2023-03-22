# 介绍 Python 的 Functools 模块

> 原文：<https://towardsdatascience.com/introducing-pythons-functools-module-2c4cba4774e?source=collection_archive---------15----------------------->

## 处理高阶函数和可调用对象

![](img/227ecadc9295d4c0609e73d9b6c78dbe.png)

阿里安·达尔维什在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄的照片

# 介绍

functools 模块是 Python 标准库的一部分，它提供了一些有用的特性，使得使用高阶函数(返回一个函数或接受另一个函数作为参数的函数)变得更加容易。有了这些特性，您可以重用或扩展函数或可调用对象的效用，而无需重写它们。这使得编写可重用和可维护的代码变得非常简单。

根据当前的稳定版本，即 Python 3.8 系列，functools 模块包含 11 个函数，其中一些可能不可用，或者在早期或后期版本中工作方式不同。它们包括:

1.  *减少()*
2.  *lru_cache()*
3.  *偏()*
4.  *partialmethod()*
5.  *singledispatch()*
6.  *singledispatchmethod()*
7.  *cached_property()*
8.  *total_ordering()*
9.  *更新 _ 包装()*
10.  *wrapps()*
11.  *cmp_to_key()*

我们将简要讨论每一个，然后借助例子，说明它们的用法和功能。

## 1.减少()

首先我们有一个经典。 **reduce(function，sequence)** 函数接收两个参数，一个函数和一个 iterable。它从左到右对 iterable 的所有元素累积应用 argument 函数，然后返回一个值。

简单地说，它首先将参数函数应用于 iterable 的前两个元素，第一次调用返回的值成为函数的第一个参数，iterable 的第三个元素成为第二个参数。重复这个过程，直到可迭代次数用完。

例如，reduce()可用于轻松计算列表的总和或乘积。

在第一个例子中 *reduce(lambda a，b: a + b，[1，2，3，4])* 计算((1+2)+3)+4)并返回列表的总和 10。

## 2.lru_cache()

lru_cache()是一个 decorator，它用一个 [memoizing](https://www.python-course.eu/python3_memoization.php) callable 来包装一个函数，该函数用于保存一个函数调用的结果，如果用相同的参数再次调用该函数，则返回存储的值。当使用相同的参数定期调用昂贵的或 I/O 受限的函数时，它可以节省时间。

本质上，它使用两种数据结构，一个字典将函数的参数映射到结果，一个链表跟踪函数的调用历史。

在全 LRU 缓存中，代表最近最少使用的缓存，指的是在达到最大条目大小时丢弃最近最少使用的元素的缓存。如果 **maxsize** 设置为 None，则禁用 LRU 功能；如果 **typed** 为 True，则分别缓存不同数据类型的参数，例如， *f(3)* 和 *f(3.0)* 将被明确缓存。

lru_cache()实用程序的一个例子可以在优化生成数字阶乘的代码中看到

在没有 *@lru_cache* 的情况下，阶乘函数需要大约 1.46 s 来运行，而另一方面，使用 *@lru_cache，*该函数只需要 158 ns。这相当于性能提高了近 100，000 倍——太神奇了！对吗？

通常，仅当您想要重用之前计算的值时，才应使用 LRU 缓存。因此，缓存需要在每次调用时创建不同可变对象的函数是没有意义的。此外，由于使用字典来缓存结果，函数的位置和关键字参数必须是可哈希的。

## 3.部分()

部分函数是具有一些预先分配的输入参数的派生函数。例如，如果一个函数有两个参数，比如" **a"** 和" **b"** "，那么可以从它创建一个分部函数，将" **a"** 作为预填充的参数，然后可以使用" **b"** 作为唯一的参数来调用它。Functool 的 partial()用于创建分部函数/对象，这是一个有用的特性，因为它允许:

1.  复制已经传入了一些参数的现有函数。
2.  以记录良好的方式创建现有功能的新版本。

让我们考虑一个简单的例子来说明这一点

我们首先基于 *math.perm()* 函数创建一个分部对象。在这种情况下，我们将 9 设置为第一个参数。因此，新创建的 *permutation_of_nine* 函数的行为就像我们调用 *math.perm()* 并将 9 设置为默认参数一样。在我们的例子中， *permutation_of_nine(2)* 与 *math.perm(9，2)做同样的事情。*

需要注意的是， ***__name__*** 和 ***__doc__*** 属性是由程序员指定的，因为它们不是自动创建的

partial 函数还带有一些重要的属性，这些属性在跟踪 partial 函数/对象时非常有用。其中包括:

*   ***partial . args***—返回预先分配给分部函数的位置参数。
*   ***partial . keywords***—返回预先分配给分部函数的关键字参数。
*   ***partial.func*** —返回父函数的名称及其地址。

让我们看另一个说明这些特征的例子

偏旁非常有用。例如，在函数调用的管道序列中，一个函数的返回值是传递给下一个函数的参数。

## 4.partialmethod()

*partialmethod()* 返回一个新的 *partialmethod* 描述符，其行为类似于 *partial* ，只是它被设计为用作方法定义，而不是可直接调用的。你可以把它看作是方法的 *partial()* 。

也许一个例子最适合说明这一点。

我们首先创建一个动物类，它有一个属性*物种*和一个实例方法 *_set_species()* 来设置动物的物种。接下来，我们创建两个 partialmethod 描述符 *set_dog()* 和 *set_rabbit()* ，分别用“狗”或“兔子”调用 *_set_species()* 。这允许我们创建动物类的一个新实例，调用 *set_dog()* 将动物的种类改为 dog，最后打印新的属性。

## 5.singledispatch()

在我们讨论这个函数之前，我们首先要弄清楚两个重要的概念，这很重要:

*   第一个是通用函数，它是由对不同类型实现相同操作的多个函数组成的函数。呼叫期间使用的实现由[调度算法](http://ezyfleet.com/dispatch-algorithm-overview-2/)决定。
*   第二种是单一分派，这是一种通用函数分派的形式，根据单一参数的类型选择实现。

考虑到这一点，functool 的 *singledispatch* 是一个装饰器，它将一个简单函数转换成一个通用函数，其行为取决于其第一个参数的类型。用简单的语言来说，它用于函数重载

让我们来看一个实际例子。

我们首先定义一个函数 *divide()* ，它接受两个参数 **a** 和 **b** ，并返回 **a/b** 的值。然而，分割字符串会导致一个 [**类型错误**](https://docs.python.org/3/tutorial/errors.html) ，为了处理这个问题，我们定义了 _ functions，它指定了 *divide()* 的行为，如果它是由字符串提供的话。注意，重载的实现是使用通用函数的 *register()* 属性注册的

## 6.singledispatchmethod()

它是一个装饰器，做的事情和 *@singledispatch* 完全一样，但是它是为方法而不是函数指定的。

考虑下面的例子。

Product 类的 prod 方法被重载以返回一个列表或集合的元素的乘积，但是如果提供了不同的类型，默认情况下，它会引发一个[**notimplementererror**](https://docs.python.org/3/tutorial/errors.html)。

## 7.cached _ property()

顾名思义， *cached_property()* 是一个装饰器，它将一个类方法转换成一个属性，该属性的值只计算一次，然后作为实例生命周期中的一个普通属性进行缓存。它类似于 [*@property*](https://www.freecodecamp.org/news/python-property-decorator/) ，除了它的缓存功能。这对于实例的计算开销很大的属性很有用，否则这些属性实际上是永久的。

在上面的例子中，我们有一个*数据集*类，它保存了一个观察值列表，并实现了计算方差和标准差的方法。问题是，每次调用这些方法时，都必须重新计算方差和标准差，这可能证明是非常昂贵的，尤其是对于大型数据集。 *@cached_property* 通过只计算和存储一次值来缓解这个问题，如果该方法被同一个实例再次调用，则返回该值。

## 8.total_ordering()

给定一个定义了一个或多个[富比较排序方法](https://www.python.org/dev/peps/pep-0207/)即 _ *_lt__()、__le__()、_ _ gt _ _()*或 *__eq__()* (对应 *<、< =、>、> =* 、*=*)。您可以定义一些比较方法， *@total_ordering* 会根据给定的定义自动提供其余的方法。重要的是，该类应该提供一个 *__eq__()* 方法。

例如，如果您想创建一个比较不同数字的类。您可能需要实现所有丰富的比较方法。然而，这可能是相当繁琐和多余的，要解决这个问题，你只能实现 _ *_eq__* 和 *__gt__* 方法，并使用 *@total_ordering* 来自动填充其余的。

不过有一个限制，使用 *@total_ordering* 会增加[的开销](https://en.wikipedia.org/wiki/Overhead_(computing)#:~:text=In%20computer%20science%2C%20overhead%20is,to%20perform%20a%20specific%20task.&text=Examples%20of%20computing%20overhead%20may,data%20transfer%2C%20and%20data%20structures.)导致执行速度变慢。此外，派生比较方法的[堆栈跟踪](https://en.wikipedia.org/wiki/Stack_trace)更加复杂。因此，如果您需要高效的代码，明智的做法是自己显式地实现比较方法

## 9.更新包装器()

它更新包装函数的元数据，使之看起来像被包装的函数。例如，对于分部函数，***update _ wrapper(partial，parent)*** 将更新分部函数的文档( ***__doc__*** )和名称( ***__name__*** )以匹配父函数。

## 10.换行()

这是一个方便调用 *update_wrapper()* 到修饰函数的函数。相当于运行***partial(update _ wrapper，wrapped=wrapped，assigned=assigned，updated=updated)*** 。

举个例子，

## 11.cmp_to_key()

它将旧式的比较函数转换为关键函数。比较函数是任何可调用的函数，它接受两个参数，对它们进行比较，返回负数表示小于，返回零表示等于，返回正数表示大于。键函数是一个可调用函数，它接受一个参数并返回另一个值以用作排序键，一个例子是***operator . item getter()***键函数。***sorted()******min()******max()******ITER tools . group by()***等工具中都用到关键函数。

cmp_to_key() 主要用作 Python 2 编写的支持比较函数的程序的转换工具。

让我们举一个例子，说明如何使用比较函数根据首字母对字符串列表进行排序，以说明 *cmp_to_key()* 的用法

# 结论

在本文中，我们已经学习了 functools 模块，希望您现在已经了解了如何使用它来实现高阶函数，并编写高度健壮、可读和可重用的代码。

祝你好运，编码愉快，愿 bug 在你的力量面前颤抖。

![](img/ae6fdda31bfde274700a6ef1b3849d09.png)

由 [meme-arsenal](https://meme-arsenal.com/create/meme/2637984) 制作的图像