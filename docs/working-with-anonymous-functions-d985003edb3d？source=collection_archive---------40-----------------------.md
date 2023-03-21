# 使用匿名函数

> 原文：<https://towardsdatascience.com/working-with-anonymous-functions-d985003edb3d?source=collection_archive---------40----------------------->

## 使用这些简单的操作快捷方式节省您的时间

![](img/c311df544e17d2d02c4ee8e41401176d.png)

塔里克·黑格在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄的照片

当我们处理数据时，我们很少会让它保持原样、不被转换或不被聚集。必须以高效的方式清理、转换、总结和描述数据，以便为实现准备数据。创建函数是一种标准且可靠的做法，在对数据应用预定义的操作时可以节省时间。然而，尽管定义自己的函数有时非常重要，但有时却完全没有必要。我们可以对一个特定的级数使用特殊类型的函数来加速这个过程，而不牺牲计算效率或精度。

# 匿名函数

我们在 Python 中实现了一些函数，但没有给它们命名(因此，匿名)。这些函数只能在数据操作过程的特定时刻使用一次。**介绍，λ函数:**

```
lambda <arguments>: <expression>
```

以上是 lambda 函数的一般语法。我们稍后将进一步深入，但首先让我们定义语法。

*   Lambda 函数可以有任意数量的参数(将要操作的内容)，但只能有一个表达式(将要计算和返回的内容)
*   lambda 函数可以应用于任何需要函数对象的地方
*   仅限于单一表达式

让我们来比较一下匿名 lambda 函数和传统定义的 function 对象在试图将一个列表的所有内容加倍时的情况:

**繁体:**

```
list1 = [1, 2, 3]def double_items(list1): list2 = []    
     for x in list1:
          list2.append(x*2) return list2
```

它返回:

```
double_items(list1)Output: [2,4,6]
```

**兰巴:**

```
list1 = [1,2,3]
list2 = list(map(lambda x: x*2, list1))
```

它返回:

```
print(list2)Output: [2,4,6]
```

我们可以清楚地看到，第二个 lambda 函数允许我们在一行代码中完成相同的计算，而不需要定义不必要的代码块。

# Lambda 场景:

lambda 匿名函数可以应用于三种主要场景:filter()、map()和 reduce()。

## 过滤器()

这个函数将一个列表作为它的参数，然后过滤掉列表中的元素，为表达式返回“true”。我们下面看到的函数将只返回*偶数*列表项(使用了“%”模数运算符)。

```
ex_list = [33, 2, 42, 1, 47, 430, 23, 98, 12]
final = list(filter(lambda x: (x%2 == 0), ex_list))
```

输出:

```
print(final)[2, 42, 430, 98, 12]
```

## **地图()**

在我处理数据的经验中，这个函数是目前使用最多的匿名函数。我们已经在最初的例子中使用了 map。当与熊猫系列对象结合使用时，贴图功能也非常有效。Map 是一个带两个参数的函数:函数名(或函数本身)和序列(一个列表、一个熊猫系列等)。).因此，map 函数与 lambda 函数一起将*映射*序列中所有元素的函数。

```
a = [1, 2, 3, 4]
b = [5, 6, 7, 8]sum_a_b = map(lambda x, y: x + y, a, b)
```

输出:

```
print(sum_a_b)[6, 8, 10, 12]
```

示例 map/lambda 组合采用函数 *x + y，*以及序列 a 和 b。它将每个序列中的各个项目分别视为函数的 x 和 y，以返回结果。

## 减少()

这个函数也将一个列表作为参数，然后，你猜对了，通过对列表对的重复操作来减少列表。我们可以在“functools”模块中找到这个函数，我们必须首先导入它。

```
from functools import reduceex_list = [24, 5, 6, 45, 96, 123, 69]
difference = reduce(lambda x: y: x - y, ex_list)
```

输出:

```
print(difference)-320
```

这个函数取列表中第一项和第二项的差，然后继续下去，直到列表的最后一个元素被求差。类似地，它将 lambda 函数作为要执行的操作，将 ex_list 作为执行函数的序列。