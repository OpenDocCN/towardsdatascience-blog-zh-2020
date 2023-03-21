# Python 中下划线的 5 种不同含义

> 原文：<https://towardsdatascience.com/5-different-meanings-of-underscore-in-python-3fafa6cd0379?source=collection_archive---------3----------------------->

## 确保使用正确的语法

![](img/0058c08a4ed000d0f564253217e00c9e.png)

Erik Witsoe 在 [Unsplash](https://unsplash.com/) 上拍摄的照片

如果你是一个 Python 程序员，你可能对下划线很熟悉。Python 使用两种类型的下划线:单下划线`_`和双下划线`__`。不要小看 Python 中的下划线，这是一个非常强大的语法。在这篇文章中，我将谈论 5 种不同的下划线模式。

## 单个独立下划线 _

单个下划线`_`是一个 [Python 标识符](https://docs.python.org/3/reference/lexical_analysis.html#identifiers)的有效字符，所以它可以用作变量名。

1.  **代表解释器中的最后一个表达式**

> 根据 [Python 文档](https://docs.python.org/3/reference/lexical_analysis.html#reserved-classes-of-identifiers)，在交互式解释器中使用特殊标识符`_`来存储上次评估的结果。它存储在内置模块中。

这里有一个例子。首先，我们检查`_`没有存储在内置模块中，然后我们编写一个没有变量名的表达式。如果我们再次检查内置模块，我们会在模块中找到`_`并且值是最后一次评估。

```
>>> '_' in dir(__builtins__)
False
>>> 1+1
2
>>> '_' in dir(__builtins__)
True
>>> _
2
```

**2。代表我们不关心的价值观**

单下划线`_`的另一个用例是表示你不关心的或者在程序后面不会用到的值。如果您将类似 Flake8 的 linter 应用到您的程序中，如果您分配了一个变量名但从未使用过，您将从 linter 得到一个错误( [F841](https://flake8.pycqa.org/en/2.6.0/warnings.html) )。把自己不在乎的变量赋给`_`就能解决这个问题。

让我们看一些代码。 *Example1* 使用`_`来表示列表中每个元素的索引。在*例 2* 中，我们只关心来自元组的`year`、`month`和`day`，所以把`_`赋给其余的(时、分、秒)。但是如果我们打印出`_`，我们只会得到最后一个表达式，也就是 59。

来自 Python 3。*，它支持[扩展的可迭代解包](https://www.python.org/dev/peps/pep-3132/)，这意味着我们可以使用`*_`来表示多个值。在*示例 3* 中，`_`实际上代表了我们想要忽略的值的列表。

Lambda 功能也支持`_`。在*例 4* 中，lambda 函数用于 monkeypatch 函数`random.randint`，并且将始终生成相同的输出。在这种情况下，输入参数不再重要。

**3。用于数字分组的可视分隔符**

从 Python 3.6 开始，下划线`_`也可以用作数字分组的可视分隔符。正如 [PEP515](https://www.python.org/dev/peps/pep-0515/) 所说，它适用于整数、浮点和复数文字。

```
integer = 1_000
amount = 1_000_000.1
binary = 0b_0100_1110
hex = 0xCAFE_F00D
>>> print(amount)
1000000.1
>>> print(binary)
78
>>> print(hex)
3405705229
```

## 单前导下划线 _var

> 根据 [PEP8](https://www.python.org/dev/peps/pep-0008/) ，单前导下划线`_var`供内部使用。从 M 导入*不导入名称以下划线开头的对象。

`_`在变量或方法名前是一个弱内部使用指示器。它警告开发人员不要公开导入和使用这个变量、方法或函数。然而，Python 并没有完全阻止它们被导入和使用。

在下面的例子中，我们有一个公共变量`external`和一个私有变量`_internal`。如果我们做*通配符导入* `from m import *`，不包括`_internal`变量(main1.py)。但是如果我们显式导入`_internal`(main 2 . py)或者使用*常规导入* (main3.py)就可以了。

[下划线-单前导. py](https://gist.github.com/highsmallxu/56d949910ef69b5c7a60df66505c2099)

单个前导下划线在类中被大量使用。程序员可以创建*私有*变量和方法，但是和前面的例子一样，这些变量和方法仍然可以从外部使用。

[下划线-单前导-class.py](https://gist.github.com/highsmallxu/4bfc051dfc49c7969759ca22ba2a02fb)

一般来说，单个前导下划线只是一种命名约定，表示变量或函数供内部使用。如果程序员真的想的话，他们仍然可以导入这个名字。

## 单个尾随下划线`var_`

使用单个尾随下划线的原因只有一个，那就是避免与 Python 关键字冲突。这在 [PEP8](https://www.python.org/dev/peps/pep-0008/) 中也有提及。

我们可以使用`keyword`模块来检查 Python 中的关键字列表。如果你想给一个变量名叫做`global`，那么你应该把它命名为`gloabl_`。

[下划线-关键字. py](https://gist.github.com/highsmallxu/31c1a41ebc33e9cd25381ae595580dd7)

## 双前导下划线`__var`

到目前为止，我们讨论的模式基本上是不同的命名约定，但是使用双前导下划线，Python 的行为会有所不同。

Python 解释器将对带有前导下划线的标识符进行名称处理。**名称篡改是在一个类中覆盖这些标识符的过程，以避免当前类及其子类之间的名称冲突。**

综上所述，`__var`在类中会有不同的名字。让我们看看下面的例子。该示例使用内置函数`[dir](https://docs.python.org/3/library/functions.html#dir)`返回当前本地范围内的名称列表。在列表的最后，我们看到了`_hour`、`day`、`month`和`year`，这意味着我们可以通过`time._hour`或`time.year`直接检索这些属性。

然而，对于`__minute`，它却另有故事。其名称被覆盖为`_Time__minute`。

[下划线-双前导. py](https://gist.github.com/highsmallxu/f747c5738e690df8914f297ddc1edf8b)

这将产生两个后果:

1.  属性`__day`在类`Time`之外不可访问。如你所见，`__day`没有被`Time`类识别，它被替换为`_Time__day`。

```
print(time.__day)# AttributeError: 'Time' object has no attribute '__day'print(time._Time__day)
# 10
```

2.属性`__day`不能在`Time`的子类中被覆盖。在下面的例子中，我们创建了一个从类`Time`扩展而来的子类`TimeSubClass`，并使用`dir`来检查本地名称。

在输出中，你可以找到`_TimeSubclass__day`、`_Time__day`、`_month`和`year`。这意味着`_month`和`year`已经被新值覆盖，但`__day`没有。

[下划线-双前导-subclass.py](https://gist.github.com/highsmallxu/1745f0dfa4d1bc97e83387cf32a20737)

让我们检查一下数值。`_TimeSubclass__day`被赋予新值，而`_Time__day`仍为原始值。

```
print(time_subclass._TimeSubclass__day)
# 30
print(time_subclass._Time__day)
# 1
```

虽然不能通过直接访问获得值，但是仍然可以通过方法获得值，在方法中，返回`self.__day`。对`@property`也有效。

[下划线-double-method.py](https://gist.github.com/highsmallxu/2bf0645235fd57720d77e11f01deac39)

## 双前导和尾随下划线`__var__`

与单尾随下划线不同，双尾随下划线没有特殊含义。你大概可以把`var__`看成是`var_`的延伸。

然而，双前导和尾部下划线`__var__`是完全不同的，它是 Python 中非常重要的模式。Python 不会对这样的属性应用名称处理，但是带有双前导和尾随下划线的**名称在 Python** 中是为特殊用途而保留的。他们被称为**神奇的名字。**你可以查看 [Python 文档](https://docs.python.org/3/reference/datamodel.html)来获得魔法名称列表。像`__init__`、`__call__`、`__slots__`这些名字都是神奇的方法。

这些魔法属性和魔法方法是允许被覆盖的，但是你需要知道你在做什么。让我们看一个例子。`__str__`是一个返回对象的字符串表示的方法。当你做`print()`或者`str()`的时候这个方法被调用。在这个例子中，我们覆盖了字符串表示，然后我们将在输出中得到一个新的格式。

[下划线-双前导-尾随. py](https://gist.github.com/highsmallxu/9643fdb67fcee926fa213c4e6cf5a455)

你可以有一个自定义的名字，像`__day__`一样有双下划线。Python 会把它作为一个常规的属性名，不会对它应用名称篡改。但是，没有很强的理由就起这样的名字，并不是一个好的做法。

## 结论

在本文中，我们讨论了 Python 中下划线的 5 种不同模式。我们来总结一下。

1.  单独立下划线`_`单前导下划线`_var`单尾随下划线`var_`。

带有单下划线的模式基本上是命名约定。Python 解释器不会阻止它们在模块/类之外被导入和使用。

2.双前导下划线`__var`双前导和尾随下划线`__var__`。

带有双下划线的模式更加严格。它们要么不允许被覆盖，要么需要一个好的理由来这样做。请注意，双尾下划线`var__`没有特殊含义。

我希望你喜欢这篇文章！如果你有任何想法，请在下面留下你的评论。

## 参考

[](https://www.datacamp.com/community/tutorials/role-underscore-python) [## 下划线(_)在 Python 中的作用

### 许多 Python 开发人员不知道中下划线(_)的功能。它帮助用户编写 Python…

www.datacamp.com](https://www.datacamp.com/community/tutorials/role-underscore-python) [](https://dbader.org/blog/meaning-of-underscores-in-python) [## Python-dbader.org 中下划线的含义

### Python 中单下划线和双下划线(“dunder”)的各种含义和命名约定，如何命名…

dbader.org](https://dbader.org/blog/meaning-of-underscores-in-python)