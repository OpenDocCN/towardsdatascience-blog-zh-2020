# Python 中的静态类型

> 原文：<https://towardsdatascience.com/static-typing-in-python-55aa6dfe61b4?source=collection_archive---------5----------------------->

## Python 初学者

## 轻松进行类型检查

![](img/92011e48968149247a51bd6d487578d9.png)

由 [Battlecreek 咖啡烘焙师](https://unsplash.com/@battlecreekcoffeeroasters?utm_source=medium&utm_medium=referral)在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄的照片

Python 是一种 ***动态*** 类型化语言。这意味着在给变量赋值时，**没有必要声明变量的类型。例如，当你用字符串值`'Tom'`初始化对象时，你不需要声明对象`major`的数据类型为`string`。**

```
major = 'Tom'
```

在像 C 这样的 ***静态*** 类型化语言中，你必须声明一个对象的数据类型。字符串被声明为字符数组。

```
char major[] = "Tom";
```

用像 Python 这样的动态类型语言编码肯定更灵活，但是人们可能想要注释对象的数据类型并强制类型约束。如果一个函数只需要整数参数，那么向函数中抛出字符串可能会导致程序崩溃。

尽管这是动态类型的主要缺陷之一，Python 3 为程序员引入了几个注释工具来指定和约束对象的数据类型。

# 函数注释

让我们以一个非常简单的函数`foo`为例:

```
def foo(n, s='Tom'):
    return s*n
```

该函数以`n`和`s`为参数，返回`s*n`。虽然它看起来像一个简单而无意义的乘法函数，但是请注意`s`的默认值是`'Tom'`，它是一个字符串，而不是一个数字。我们可能推断这个函数打算返回一个多次重复字符串`s`的字符串，确切地说是`n`次。

```
foo(3, 'Tom') # returns 'TomTomTom'
```

这个功能比较混乱。您可能想写冗长的**注释**和**文档字符串**，解释函数并指定参数和返回值的数据类型。

```
def foo(n, s='Tom'):
    """Repeat a string multiple times.
    Args:
        n (int): number of times
        s (string): target string
    Returns:
        (string): target string repeated n times.
    """
    return s*n
```

![](img/d5fc85428914d6938e9b5eb0b1b95c5c.png)

美国宇航局在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 拍摄的照片

Python 为你使用符号`:`和`->`做 ***可选标注*** 提供了一种更简洁的方式。

```
def foo(n: int, s: str='Tom') -> str:
    return s*n
```

函数`foo`的注释可从函数的`__annotations__`属性中获得。这是一个将参数名映射到它们的注释表达式的字典。这允许**通过运行代码来手动**类型检查，而不是自己查看源代码。非常方便。

```
foo.__annotations__
# {'n': int, 's': str, 'return': str}
```

# 可变注释

除了函数参数和返回值之外，还可以用某种数据类型来注释变量。你也可以注释变量而不用任何值初始化它们！

```
major: str='Tom' # type:str, this comment is no longer necessary
i: int
```

最好使用这种内置语法而不是注释来注释变量，因为注释在许多编辑器中通常是灰色的。

用于注释更高级类型的变量，如`list`、`dict`等。，你需要从模块`typing`中导入它们。类型名称大写，如`List`、`Tuple`、`Dict`等..

```
from typing import List, Tuple, Dictl: List[int] = [1, 2, 3]
t1: Tuple[float, str, int] = (1.0, 'two', 3)
t2: Tuple[int, ...] = (1, 2.0, 'three')
d: Dict[str, int] = {'uno': 1, 'dos': 2, 'tres': 3}
```

列表、元组或字典中的元素也可以被注释。那些大写的类型采用方括号`[]`中的参数，如上所示。

`List`接受一个参数，它是列表中所有元素的注释类型。固定大小元组中的元素可以逐个进行注释，而可变大小元组中的元素可以通过省略号`...`进行注释。我们还可以在字典中指定键和条目的类型。

![](img/3ecbdb2540b1c1e31f15ba6f13426a10.png)

照片由[杨奇煜·巴赞内格](https://unsplash.com/@fbazanegue?utm_source=medium&utm_medium=referral)在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄

# 高级注释

我们提到过`List`只取一个参数。注释一个混合了`int`和`float`元素的列表怎么样？`Union`就是答案。

```
from typing import Union
l2: List[Union[int, float]] = [1, 2.0, 3]
```

它还支持任何用户定义的类作为注释中的类型。

```
class FancyContainer:
    def __init__(self):
        self.answer = 42fc: FancyContainer = FancyContainer()
```

一个 ***可调用的*** 也可以使用上述技术进行注释。可调用的是可以被调用的东西，就像函数一样。

```
from typing import Callable
my_func: Callable[[int, str], str] = foo
```

# 警告

首先，类型注释不能完全取代文档字符串和注释。出于可读性和再现性的目的，仍然需要对您的函数进行简要的描述和解释。启用类型注释可以避免让复杂的注释充满数据类型之类的信息。

第二，有件事我应该一开始就告诉你。Python 解释器实际上不会自动进行任何类型检查。这意味着那些注释在运行时没有任何作用，即使你试图传递一个“错误”类型的对象给一个函数。

那么你有没有浪费自己的时间去学习类型注释呢？号码

有许多 Python 模块可以在运行前强制执行这些约束。mypy 是目前最常用的类型检查器，没有运行时开销。

![](img/723ace0b4a90b86a2b3f3783a924cfb5.png)

[起义](https://unsplash.com/@revolt?utm_source=medium&utm_medium=referral)在[广场](https://unsplash.com?utm_source=medium&utm_medium=referral)拍照

# 外卖

Python 中的类型注释有助于**调试**和可选的**类型检查**，后者模拟静态类型。它在项目开发中变得越来越流行，但在更普通的 Python 程序员中仍然很少见。

虽然没有注释并不一定会降低代码性能，但出于健壮性、可读性和可再现性的目的，它仍然被认为是一种良好的实践。

我们仅仅触及了 Python 中静态类型和类型注释的皮毛。Python 3.9 即将对变量注释进行一些升级，所以请继续关注[注册我的时事通讯](http://edenau.mailchimpsites.com/)以接收我的新文章的更新。

连 Python 3.8 都没准备好？我掩护你。

[](/6-new-features-in-python-3-8-for-python-newbies-dc2e7b804acc) [## Python 3.8 中针对 Python 新手的 6 项新特性

### 请做好准备，因为 Python 2 不再受支持

towardsdatascience.com](/6-new-features-in-python-3-8-for-python-newbies-dc2e7b804acc) 

感谢阅读！你觉得这些功能有趣有用吗？下面留言评论！您可能还会发现以下文章很有用:

[](/4-easy-to-use-introspection-functions-in-python-49bd5ee3a2e8) [## Python 中 4 个易于使用的自省函数

### 如何在运行时检查对象

towardsdatascience.com](/4-easy-to-use-introspection-functions-in-python-49bd5ee3a2e8) [](/4-common-mistakes-python-beginners-should-avoid-89bcebd2c628) [## Python 初学者应该避免的 4 个常见错误

### 我很艰难地学会了，但你不需要

towardsdatascience.com](/4-common-mistakes-python-beginners-should-avoid-89bcebd2c628)