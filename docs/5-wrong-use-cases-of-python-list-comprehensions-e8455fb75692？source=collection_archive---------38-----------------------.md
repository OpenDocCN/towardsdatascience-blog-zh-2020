# Python 列表理解的 5 个错误用例

> 原文：<https://towardsdatascience.com/5-wrong-use-cases-of-python-list-comprehensions-e8455fb75692?source=collection_archive---------38----------------------->

## 知道什么时候不使用列表理解

![](img/e36162156baf4863bd1b8db84da03b65.png)

照片由[加里·本迪格](https://unsplash.com/@kris_ricepees?utm_source=medium&utm_medium=referral)在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄

列表理解无疑是 Python 最强大的特性。它们是 pythonic 式的，功能强大，并提供可读性。

但是这很容易为滥用它们打开方便之门。或者说是各种形式的虐待。这样做，你的代码只能是非 pythonic 化的，很难被对等体破译。

在下一节中，我们将看看使用 Python 列表理解的几种不正确的方法及其替换。

# 当你只是改变状态，而不是列表

许多程序员在开始使用 Python 时，在并不真正需要的地方开始使用列表理解。

例如，在下面的例子中，使用列表理解并不是最好的主意，因为我们没有对列表做任何事情。

```
vehicles = [ car, bike, truck ]
[ v.setLights(true) for v in vehicles ]names = [ "A", "B", "C" ]
[ print(i) for n in names ]
```

在这种情况下，最好使用经典的 for 循环，因为它可以防止创建不必要的列表。列表理解并不意味着指定命令或设置元素的状态。

# 当你只是在转换价值观的时候

你可能想转换一个字符串的字符或者简单地添加一串数字。在这两种情况下，选择列表理解并不意味着它也是 pythonic 式的。

让我们考虑下面的例子:

```
total = 0
listOne = [1,2,3]
[total := total + x for x in listOne]
```

虽然 Python 3.8 的赋值操作符`:=`允许我们使用列表理解对列表元素求和，但是当 Python 提供了内置的`sum()`函数时，为什么要重新发明一个轮子呢？上面的列表理解可以通过一段非常短的代码来完成:

```
sum(listOne)
```

类似地，当你只是将一个字符串拆分成字符(或者转换它们)时，使用列表理解是多余的，而且完全没有必要:

```
str = "Fred"
chars = [x for x in str]#use this instead:
chars = list(str)
```

# 当理解与庞大的逻辑嵌套在一起时

列表理解无论有多好，当它们嵌套在一起时都会变成一种痛苦。当逻辑冗长时，可读性问题只会进一步恶化。

Python 列表的理解最好是留给简洁的逻辑，以符合 python 代码。这意味着，如果需要的话，可以放弃嵌套列表理解，而使用嵌套循环。

下面的例子仅仅展示了嵌套循环是如何变得更容易理解，并且有时是长嵌套理解的很好的替代品。

```
list = [[1,2,3],[-1,-5,6]]flatten = [item
           for sublist in list
           for item in sublist
           if item > 0]#using loops
flatten1 = []for rows in list:
    for sublist in rows:
        if sublist > 0:
           flatten1.append(sublist)
```

# 当你在内存中加载整个列表时，尽管你并不需要它

列表理解可以让你很容易地创建列表，但是它们会一直占用内存。这使得它们非常低效，尤其是在处理大型数据集时。

考虑下面的例子，您很容易遇到内存不足的错误。

```
sum([i * i for i in range(1000)])
```

建议使用生成器表达式，因为它一次生成一个值:

```
sum(i* i for i in range(1000))
```

将列表存储在内存中的另一个不好的例子是，当您只根据特定条件查找单个元素时。下面的列表理解单句效率很低。

```
first = [x for x in list 
         if x.get("id")=="12"]
```

我们不仅可以用生成器表达式来代替它，还可以设置`next`函数，以便在第一个元素匹配给定条件时返回:

```
first = next((x for x in list if x.get("id")=="12"), None)
```

使用`itertools`函数和生成器表达式是一种有效的退出方式，而不是使用不支持`break`或`continue`的列表理解。

# 当你过度使用列表理解时

同样，您会惊讶地发现陷入列表理解并将其用于解压缩元组是多么容易。

下面的例子只是恢复了另一个不正确的用例，我们将元组值拆分成嵌套列表:

```
myList **=** [('A', 1), ('B', 2), ('C', 3)]result = [[ i for i, j in myList ],
       [ j for i, j in myList ]]
```

Python 已经为我们提供了一个特定的内置工具，可以让我们解压缩元组。只需使用解包操作符`*`来析构元组，并将其传递给`zip`函数来创建单独的列表。

```
result **=** list(zip(*****myList))# [['A', 'B', 'C'], [1, 2, 3]]
```

# 结论

Python 列表理解非常强大，但是它们很容易被过度使用，从而使代码变得非 python 化。

我希望以上不正确的方法能帮助你更好地判断什么时候不使用列表理解。

这一次到此为止。感谢阅读。