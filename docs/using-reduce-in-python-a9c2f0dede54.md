# 在 Python 中使用 Reduce

> 原文：<https://towardsdatascience.com/using-reduce-in-python-a9c2f0dede54?source=collection_archive---------8----------------------->

## 如何使用 Python 中的 reduce 函数

![](img/6d58782058c6c038fef94761322c29b2.png)

凯文·Ku 在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上的照片

## 介绍

Python 是一种面向对象的编程(OOP)语言。然而，它提供了一些提供函数式编程风格的工具。其中一些工具包括 map()、filter()和 reduce()函数。在本教程中，我们将探索 reduce()函数，并揭示它提供的多功能性和实用性。

***贴图()和滤镜()函数在这里详细介绍:***

[](/using-map-and-filter-in-python-ffdfa8b97520) [## 在 Python 中使用地图和过滤器

### 如何使用 python 中内置的地图和过滤功能

towardsdatascience.com](/using-map-and-filter-in-python-ffdfa8b97520) 

## 使用 For 循环

引入 reduce()函数的最佳方式是从一个问题开始，尝试用传统的方法解决它，使用 for 循环。然后，我们可以使用 reduce 函数尝试相同的任务。

假设我们有一个数字列表，我们想返回他们的产品。换句话说，我们希望将列表中的所有数字相乘并返回一个值。我们可以使用 for 循环来实现这一点:

> 我们有我们的数字列表。我们想把这个列表中的数字相乘，得到它们的乘积。我们创建变量 **product** 并将其设置为 1。然后，我们使用 for 循环遍历 **num_list** ，并将每个数字乘以前一次迭代的结果。在循环通过 **num_list** 之后，**乘积**或累加器将等于 120，这是列表中所有数字的乘积。

# 减少功能

事实证明，我们可以使用 reduce 函数代替 for 循环来完成上述任务。reduce 函数可以接受三个参数，其中两个是必需的。两个必需的参数是:一个函数(它本身接受两个参数)和一个 iterable(比如一个列表)。第三个参数是初始化式，是可选的，因此我们将在后面讨论它。

> reduce(函数，可迭代[，初始值设定项])

## 导入减少

reduce 函数在模块 [functools](https://docs.python.org/3/library/functools.html) 中，该模块包含高阶函数。高阶函数是作用于或返回其他函数的函数。因此，为了使用 reduce 函数，我们要么需要导入整个 functools 模块，要么可以只从 functools 导入 reduce 函数:

```
import functoolsfrom functools import reduce
```

***注意:*** *如果我们导入 functools，我们需要访问 reduce 函数，如下所示:functools.reduce(arguments)。如果我们只从 functools 模块导入 reduce 函数，我们只需输入 reduce(arguments)就可以访问 reduce 函数。*

## 探索归约函数

如前所述，reduce 函数接受两个必需的参数:一个函数和一个 iterable。为了避免 reduce 函数和它作为参数的函数之间的混淆，我将 reduce 函数称为 reduce 。

reduce 接受的第一个参数，函数本身必须接受两个参数。Reduce 然后将这个函数累积地应用到 iterable 的元素上(从左到右)，并将其减少到一个值。

**我们来看一个例子:**

假设我们使用的 iterable 是一个列表，比如上面例子中的 **num_list** :

```
num_list = [1,2,3,4,5]
```

我们用作 reduce 的第一个参数的函数如下:

```
def prod(x, y):
    return x * y
```

*这个 prod 函数接受两个参数:x 和 y。然后返回它们的乘积，即 x * y。*

让我们分别传入 **prod** 函数和 **num_list** 作为我们的函数和 iterable，以减少:

```
from functools import reduceproduct = reduce(prod, num_list)
```

> 我们的 iterable 对象是 **num_list** ，也就是 list: [1，2，3，4，5]。我们的函数 **prod** 接受两个参数 x 和 y。Reduce 将从接受 **num_list** 的前两个元素 1 和 2 开始，并将它们作为 x 和 y 参数传递给我们的 **prod** 函数。 **prod** 函数返回他们的乘积，即 1 * 2，等于 2。Reduce 随后将使用这个累加值 2 作为新的或更新的 x 值，并使用 **num_list** 中的下一个元素 3 作为新的或更新的 y 值。然后，它将这两个值(2 和 3)作为 x 和 y 发送给我们的 **prod** 函数，然后该函数返回它们的乘积 2 * 3 或 6。然后，这个 6 将被用作我们新的或更新的 x 值，并且 **num_list** 中的下一个元素将被用作我们新的或更新的 y 值，即 4。然后，它将 6 和 4 作为我们的 x 和 y 值发送给 **prod** 函数，后者返回 24。因此，我们的新 x 值是 24，我们的新 y 值是来自 **num_list** 的下一个元素，即 5。这两个值作为我们的 x 和 y 值传递给 **prod** 函数，然后 **prod** 返回它们的乘积，24 * 5，等于 120。因此，reduce 接受一个可迭代对象，在本例中是 **num_list** ，并将其缩减为一个值，120。然后将该值分配给变量 product。**换句话说，x 参数用累积值更新，y 参数从 iterable 更新。**

[](/iterables-and-iterators-in-python-849b1556ce27) [## Python 中的迭代器和迭代器

### Python 中的可迭代对象、迭代器和迭代

towardsdatascience.com](/iterables-and-iterators-in-python-849b1556ce27) 

## 第三个参数:初始化器

还记得我们说过 reduce 可以接受可选的第三个参数，初始化器吗？它的默认值是无。如果我们传入一个初始化器，它将被 reduce 用作第一个 x 值(而不是 x 是 iterable 的第一个元素)。所以如果我们把上面例子中的数字 2 作为初始化式传入:

```
product = reduce(prod, num_list, 2)print(product) #240
```

那么 x 和 y 的前两个值或自变量将分别是 2 和 1。所有后续步骤都是一样的。换句话说，在计算中，初始化器放在 iterable 的元素之前。

## 使用 Lambda 表达式

在上面的示例中，我们可以使用 lambda 表达式(或匿名函数)来显著缩短代码，而不是将 prod 作为函数传入:

```
product = reduce(lambda x,y: x*y, num_list)
```

*注意 lambda 表达式如何接受两个参数:x 和 y，然后返回它们的乘积 x * y。*

以下是关于如何使用 lambda 表达式的完整教程:

[](/lambda-expressions-in-python-9ad476c75438) [## Python 中的 Lambda 表达式

### 如何用 python 写匿名函数

towardsdatascience.com](/lambda-expressions-in-python-9ad476c75438) 

## 使用 Reduce 的其他示例

我们可以使用 reduce 的场景还有很多。

例如，我们可以找到列表中数字的总和:

```
num_list = [1,2,3,4,5]sum = reduce(lambda x,y: x + y, num_list)print(sum) #15
```

或者我们可以在列表中找到最大数量:

```
num_list = [1,2,3,4,5]max_num = reduce(lambda x,y: x if x > y else y, num_list)print(max_num) #5
```

或列表中的最小数量:

```
num_list = [1,2,3,4,5]min_num = reduce(lambda x,y: x if x < y else y, num_list)print(min_num) #1
```

还有很多其他的应用！

*注意:Python 确实有内置函数，比如 max()、min()和 sum()，这些函数对于这三个例子来说更容易使用。然而，目标是展示如何使用 reduce()来完成许多不同的任务。*

*如果你喜欢阅读这样的故事，并想支持我成为一名作家，可以考虑注册成为一名灵媒会员。每月 5 美元，你可以无限制地阅读媒体上的故事。如果你用我的* [*链接*](https://lmatalka90.medium.com/membership) *注册，我会赚一小笔佣金。*

[](https://lmatalka90.medium.com/membership) [## 通过我的推荐链接加入媒体——卢艾·马塔尔卡

### 阅读卢艾·马塔尔卡的每一个故事(以及媒体上成千上万的其他作家)。您的会员费直接支持…

lmatalka90.medium.com](https://lmatalka90.medium.com/membership) 

## 结论

在本教程中，我们学习了如何在 python 中导入和使用 reduce 函数。然后我们看到 lambda 表达式如何作为一个参数传入 reduce。最后，我们看到了一些如何使用 reduce 的例子。