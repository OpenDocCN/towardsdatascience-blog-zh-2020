# 我在学习 Python 时犯的 4 个编码错误

> 原文：<https://towardsdatascience.com/4-coding-mistakes-i-did-when-i-was-learning-python-bbc6824bdd8c?source=collection_archive---------24----------------------->

## 更好的编程

## 一起学习我最纠结的 Python 概念

![](img/3802cc78a2aa82a6600dea1812b84af1.png)

照片由 [NeONBRAND](https://unsplash.com/@neonbrand?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) 在 [Unsplash](/s/photos/mistake?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) 上拍摄

几年前，我开始了学习 Python 的冒险，我已经知道了一些其他的编程语言，比如 PHP(第一种将我引入 web 开发的语言)，JavaScript(我已经很擅长了，并且正在编写一个 UI 库)和 C#，这是我当时收入的来源。

我通过自己开发一个应用程序来学习 Python，因此我将许多 JavaScript 和 C#的做事方式融入到我的代码中，这很糟糕，尽管有时它会工作。我花了一些时间，阅读他人的代码，并与他人合作，实际上提高了语言水平。今天我想和你一起回顾一下我在学习 Python 时犯的一些错误(代码方面的)。

# #1 误解 Python 范围

Python 范围解析基于所谓的 **LEGB** 规则，这是 **L** ocal、 **E** nclosing、 **G** lobal、 **B** 内建的简写。尽管看起来非常简单，但当时我还是有点迷惑，例如，考虑下面的例子:

```
x = 5
def foo():
    x += 1
    print(x)foo()-----------
Output
-----------
Traceback (most recent call last):
  File "<stdin>", line 1, in <module>
  File "<stdin>", line 2, in foo
UnboundLocalError: local variable 'x' referenced before assignment
```

对于上面的代码，我希望它能够工作，并修改全局变量`x`以最终打印`6`。但是，事情可能会变得更奇怪，让我们来看看下面修改过的代码:

```
y = 5
def foo_y():
    print(y)foo_y()-----------
Output
-----------
5
```

这到底是怎么回事？在一段代码中，全局变量`X`给出了一个`UnboundLocalError`，然而，当我们试图打印变量时，它工作了。原因与范围有关。当你在一个作用域(比如函数作用域)中给一个变量赋值时，这个变量就变成了这个作用域的局部变量，并且隐藏了外部作用域中任何相似命名的变量。这是我们拍摄《T4》时第一个场景中发生的事情。

如果我们打算像函数`foo()`一样访问全局变量`x`，我们可以这样做:

```
x = 5
def foo():
    global x
    x += 1
    print(x)foo()-----------
Output
-----------
6
```

通过使用关键字`global`允许内部作用域访问全局作用域中声明的变量，这意味着变量没有在任何函数中定义。类似地，我们可以使用`nonlocal`来产生类似的效果:

```
def foo():
    x = 5
    def bar():
        nonlocal x
        x += 1
        print(x)
    bar()foo()-----------
Output
-----------
6
```

`nonlocal`正如`global`允许你从外部作用域访问变量，然而，在`nonlocal`的情况下，你可以绑定到一个父作用域或全局作用域上的一个对象。

# #2 迭代时修改列表

虽然这个错误不仅在 Python 中很常见，但我发现它在新的 Python 开发人员中很常见，甚至对一些有经验的开发人员也是如此。虽然有时可能看起来不那么明显，但在某些情况下，我们最终会修改当前正在迭代的数组，导致不恰当的行为，或者如果我们幸运的话，我们会得到一个错误并很容易注意到它。

但是让我给你一个例子来说明我的意思，假设给定一个数组，你需要减少数组，只包含偶数元素，你可以尝试这样做:

```
def odd(x): return bool(x % 2)
numbers = [n for n in range(10)]
for i in range(len(numbers)):
    if odd(numbers[i]):
        del numbers[i]-----------
Output
-----------
Traceback (most recent call last):
  File "<stdin>", line 2, in <module>
IndexError: list index out of range
```

在所描述的场景中，当在迭代过程中删除一个列表或数组的元素时，我们会得到一个错误，因为我们试图访问一个不再存在的项目。这是一种不好的做法，应该避免，在 python 中有更好的方法来实现类似的事情，其中包括:

```
def odd(x): return bool(x % 2)
numbers = [n for n in range(10)]
numbers[:] = [n for n in numbers if not odd(n)]
print(numbers)-----------
Output
-----------
[0, 2, 4, 6, 8]
```

您也可以使用`filter`函数来实现同样的功能，虽然它可以工作，但有些人认为这不是 Pythonic 的工作方式，我也有点同意，但我不想卷入这场讨论。我宁愿给你选择，你可以研究和决定:

```
def even(x): return not bool(x % 2)
numbers = [n for n in range(10)]
numbers = list(filter(even, numbers))
numbers-----------
Output
-----------
[0, 2, 4, 6, 8]
```

# #3 闭包中的变量绑定

我想从我在 twitter ( [@livecodestream](https://twitter.com/livecodestream) )上发布的一个测试开始，我问人们他们认为下面的片段会是什么结果:

```
def create_multipliers():
    return [lambda x : i * x for i in range(5)]for multiplier in create_multipliers():
    print(multiplier(2))-----------
Output
-----------
8
8
8
8
8
```

对许多人来说，包括我自己，当我们第一次遇到这个问题时，我们认为结果会是:

```
0
2
4
6
8
```

然而，代码实际上导致了完全不同的结果，我们对此感到非常困惑。实际发生的是 Python 会做一个后期绑定行为，根据这个行为，在调用内部函数的时候会查找闭包中使用的变量值。所以在我们的例子中，无论何时调用任何返回的函数，在调用它的时候都在周围的范围内查找`i`的值。

这个问题的解决方案可能看起来有点粗糙，但它确实有效

```
def create_multipliers():
    return [lambda x, i=i : i * x for i in range(5)]for multiplier in create_multipliers():
    print(multiplier(2))-----------
Output
-----------
0
2
4
6
8
```

通过使用 lambda 函数的默认参数来传递`i`的值，我们可以生成函数来执行所需的行为。我对这个解决方案感到非常困惑，我仍然认为它不是很优雅，但是，有些人喜欢它。如果你知道这个问题的另一个可能的解决方案，请在评论中告诉我，我很乐意阅读。

#4 名称与 Python 标准库模块冲突这个问题实际上在我刚开始的时候相当普遍，即使是现在，有时我也会犯这个错误。这个问题是由于将一个模块命名为与 Python 附带的标准库中的一个模块同名而引起的。(例如，您的代码中可能有一个名为 email.py 的模块，这将与同名的标准库模块相冲突)。

也许名称冲突本身不会对您的代码产生任何问题，但有时我们会覆盖 Python 标准库的函数或模块，这些函数或模块稍后会在已安装的库中使用，并且它会因抛出错误或行为不当而发生冲突。无论如何，这是一个糟糕的情况。

一个典型的错误如下:

```
a = list()
print(a)list = [1, 2, 3] # This is where we break it
a = list()-----------
Output
-----------
[]
Traceback (most recent call last):
  File "<stdin>", line 1, in <module>
TypeError: 'list' object is not callable
```

通过简单地创建一个名为`list`的变量，我们中断了对`list`函数的访问。而且，即使有其他的访问方式(比如`__builtins__.list()`，我们也要避免这种名字。

# 结论

本文并没有涵盖开发人员在用 Python 编码时会犯的所有常见错误，而是那些我最纠结的事情。如果您想了解更多关于如何编写出色的 Python 代码以及避免其他一些错误的信息，我推荐您阅读:

[](https://medium.com/swlh/make-your-code-great-python-style-3223c1160299) [## 让你的代码变得伟大，Python 风格

### 让我们一起学习一些简单的技巧来编写更多的 Pythonic 代码。

medium.com](https://medium.com/swlh/make-your-code-great-python-style-3223c1160299) 

感谢阅读！