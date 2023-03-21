# 从头开始学习 Python Lambda

> 原文：<https://towardsdatascience.com/learn-python-lambda-from-scratch-f4a9c07e4b34?source=collection_archive---------33----------------------->

## Python 中 lambda 函数的注意事项

![](img/b0ee4aecac3b466e0f15cbac21ecb4c7.png)

照片由[本在](https://unsplash.com/@benwhitephotography)[上写下](https://unsplash.com/)un splash

当处理元素列表时，Python 程序员通常有三种选择:常规的 *for 循环*、*列表理解*和 *lambda 函数*。对于 Python 初学者来说，lambda 函数通常被认为是“一个很酷的特性”，因为它的语法很短，而且与函数式编程很相似。但是对于初学者来说，这通常不是很简单。另一方面，即使对于长期使用 Python 的人来说，也不是每个人都一定熟悉 lambda，因为他们总能找到简单的替代方法。

相对于 lambda 函数，我个人更喜欢*list/dict*comprehension。在本文中，我们将一起从头开始学习 Python Lambda 及其注意事项，无论您是初级还是高级 Python 程序员。我们开始吧！

## 什么是 Lambda 函数？

Python 中的 Lambda 函数是一个小型匿名函数。它不同于其他函数式编程语言，在其他函数式编程语言中，lambda 函数增加了功能。根据 [Python 设计 Q & A](https://docs.python.org/3/faq/design.html) :

> 如果你懒得定义一个函数，Python lambdas 只是一种速记符号。

我们都喜欢做懒惰的程序员，但仅此而已吗？

## 创建 Lambda 函数

lambda 函数是用一个表达式定义的，而不是我们通常在 Python 中使用的`def`。任何 lambda 函数都遵循相同的规则:

```
lambda arguments: expression
```

它由三部分组成:

*   `lambda`:任意 lambda 函数的关键字
*   `arguments`:输入函数的参数。该函数允许多个输入参数。
*   `expression`:函数在单个表达式中将做什么。Lambda 只接受一个表达式，但可以生成多个输出。

***论据***

Python lambda 函数支持各种输入参数，就像一个`def`函数一样。

[lambda-arguments.py](https://gist.github.com/highsmallxu/8128edd32bd48402583329f4453caa5d)

***表情***

Lambda 函数只接受单个表达式。因为它是一个表达式，所以可能会也可能不会被赋予名称。在下面的例子中，第二个 lambda 函数被赋予值`sum_lambda`。但是第三个 lambda 函数没有名字。正如你所看到的，它仍然工作，但是代码第一眼很难读懂。

[λfunction . py](https://gist.github.com/highsmallxu/b0e3c89f9ae24ab3642278c37683f002)

需要记住的一点是， **lambda 函数不能包含任何语句**。区分表达式和语句的一个简单的技巧是，如果你可以打印它或者给它赋值，那么它就是一个表达式。否则就是声明。

在 Python 中，语句包括`return`、`try`、`assert`、`if`、`for`、`raise`等。如果 lambda 函数包含一个语句，程序将引发`SyntaxError`异常。

## 检查 Lambda 函数

那么，lambda 函数和常规函数有什么不同呢？让我们检查功能！

我们将使用上一个例子中的函数。让我们首先检查两个函数的类型。好吧，他们属于`function`类这就说得通了。

```
>> print(type(sum))
>> <class 'function'>
>> print(type(sum_lambda))
>> <class 'function'>
```

然后，让我们检查一下表示法。`sum_lambda`的名字是`<lambda>`而不是“函数名”。即使我们给 lambda 函数赋值，它仍然是一个匿名函数。这种行为将导致一个我们将在最后讨论的限制。(先想想)

```
>> sum
>> <function sum at 0x10d2f30e0>
>> sum_lambda
>> <function <lambda> at 0x10d390200>
```

## 将 Lambda 函数与 Python 高阶函数一起使用

Lambda 函数通常与 Python 高阶函数结合使用。根据维基百科，

> 高阶函数是至少执行下列操作之一的函数:a)将一个或多个函数作为参数。b)返回一个函数作为结果。

在 Python 中，有几个内置的高阶函数，如`map`、`filter`、`reduce`、`sorted`、`sum`、`any`、`all`。其中，`map`、`filter`和`reduce`最常与 lambda 函数一起使用。

***地图***

函数有两个参数:一个输入映射函数和一个 iterable。映射函数将应用于 iterable 中的每个元素。`map()`函数返回包含映射元素的迭代器。

映射函数可以用简洁的λ函数来表示。

[map-lambda.py](https://gist.github.com/highsmallxu/1a9fc67ffa0087bbee95699a1756e565)

***滤镜***

`filter()`函数采用与`map()`相同的参数:一个输入过滤函数和一个 iterable。过滤函数将应用于每个元素，`filter()`函数返回一个包含过滤元素的迭代器。

下面是一个使用`def`函数和 lambda 函数过滤偶数的例子。

***减少***

`reduce()`函数来自 Python 内置模块`functools`。它实际上是将一个函数累积应用于 iterable 中的所有元素，并生成一个值。函数有 3 个参数:一个输入函数，一个可迭代函数和一个可选的初始化函数。

第三个例子定义了一个初始值设定项 0，每次都将 dictionary 元素中的值添加到初始值设定项中。如果初始化器没有被定义，那么默认情况下程序将把第一个元素作为初始化器，这将引发一个`TypeError`异常。

[减少-λpy](https://gist.github.com/highsmallxu/ee00a3909be0a297b6c228de28998982)

## 在 pytest 中使用 Lambda 函数

您可能有一个函数`generate`生成一个随机值列表，这些值依赖于一个外部系统。当你进行单元测试时，你不希望`generate`函数与外部系统通信。此外，您需要来自`generate`函数的可重复结果，而不是每次的随机值。

在 Pytest 中，您可以使用 [monkeypatch](https://docs.pytest.org/en/stable/monkeypatch.html) fixture 来修补生成这些随机值的部分。另外，使用 lambda 函数作为补丁函数使得代码更加简洁。

下面是示例代码。它使用 lambda 函数`lambda _: 0`来修补`random.randint()`。lambda 函数总是生成 0，它不关心输入参数，所以输入参数可以用`_`来表示。

[pytest-lambda.py](https://gist.github.com/highsmallxu/3e5e72476450ddd94a7890b10e163231)

## 列表理解与λ函数

lambda 函数的一个很好的选择是列表理解。对于`map()`、`filter()`、`reduce()`，都可以用列表理解来完成。列表理解是一种介于常规 for 循环和 lambda 函数之间的解决方案。在我看来，这是一个简洁而直观的解决方案。

[list-comprehension.py](https://gist.github.com/highsmallxu/50588b57f6322e3785450108c5f2dbe4)

很好奇大家对列表理解和 lambda 函数的看法。你更喜欢哪一个，为什么？

## 性能比较

了解 lambda 函数的性能并与常规的`def`函数进行比较也很有趣。Lambda 函数需要的代码少，会对性能有影响吗？

比较内容包括:

1.  创建函数(v.s .常规函数)的时间
2.  调用函数的时间(v.s .常规函数)
3.  是时候调用高阶函数了，比如`map()` (v.s. for-loop 和 list comprehension)

我使用 Python 内置函数`[timeit](https://docs.python.org/3.8/library/timeit.html)`对代码片段计时。首先，我使用`def`和 lambda 函数创建一个函数来计算一个数的平方。结果发现创建一个函数的时间几乎是一样的。

然后我调用这两个函数。它们比创建一个函数花费更长的时间，但是 lambda 函数在这里没有赢得任何时间。

最后一个是比较 for-loop *、* list comprehension 和 lambda 函数的性能。由于从映射对象到列表的转换，Lambda 函数花费的时间最长。在这种情况下，列表理解表现最好。

[lambda-compare-map.py](https://gist.github.com/highsmallxu/0f8cc87c8b5895653ae97eb8c85e2af2)

*速度性能可以帮助我们决定选择哪种代码风格，但不应该是唯一的因素。我们还需要考虑代码的可读性和可维护性。*

## 使用 Lambda 函数的限制

到目前为止，我们已经讨论了 lambda 函数的一些用例。它的简写符号使代码更加优雅和简洁。然而，总是有得有失。Python 开发者要理解 lambda 函数的局限性，避免“不必要的优化”。

1.  在追溯中丢失信息

Lambda 函数是匿名函数，所以没有真正的函数名。当引发异常时，这可能会丢失信息。回溯只显示`<lambda>`而不是有意义的函数名。

[λ-exception . py](https://gist.github.com/highsmallxu/97b7b76cc5f66b57517a07f4e915ee2e)

2.调试麻烦

在 IDE 中调试`print(list(map(lambda x: x ** 2, [1, 2, 3])))`这样的语句并不容易。一种解决方法是给 lambda 函数添加一个“装饰器”。通常，我们会给一个`def`函数添加一个像`@debug`这样的装饰器，但是对于 lambda 函数来说这是不可能的。但实际上，decorator 是一个函数的 wapper，所以它可以将 lambda 函数作为输入参数。

下面是一个将`debug`装饰器应用到`def`函数和 lambda 函数的例子。修饰 lambda 函数有助于理解 lambda 函数在高阶函数中的行为。

[lambda-debug.py](https://gist.github.com/highsmallxu/bbe78aafacedb8075bf161064cdb7c20)

3.不符合 PEP8

如果您将 Flake8 这样的 linter 应用到您的程序中，您将会得到一个警告:

> 不要分配 lambda 表达式，使用 def (E731)

Flake8 给出了一个非常合理的解释,这与我之前的观点相联系:

> 这样做的主要原因是调试。Lambdas 在回溯中显示为`<lambda>`，其中函数将显示函数名。

4.不支持注释

根据 [PEP 3107](https://www.python.org/dev/peps/pep-3107/#lambda) ，lambda 的语法不支持注释。一种解决方法是使用代表函数的`typing.Callable`。例如，`Callable[[int, int], str]`表示一个带有两个类型为`int`的输入参数的函数，返回值的类型为`str`。我们可以用它来暗示 lambda 函数的类型。

下面是我们如何在高阶函数中提示 lambda 函数的类型。

[λ-hint . py](https://gist.github.com/highsmallxu/ee22a7f43843aee2f4c0d8e57a9d5144)

我希望你喜欢这篇文章，并且理解 Python lambda 函数的一些注意事项！如果你有任何想法，请在下面留下你的评论。

## 参考

[](https://realpython.com/python-lambda) [## 如何使用 Python Lambda 函数——真正的 Python

### 参加测验“Python 和其他编程语言中的 lambda 表达式源于 Lambda 演算，是一种……

realpython.com](https://realpython.com/python-lambda) [](https://realpython.com/python-reduce-function/) [## Python 的 reduce():从函数式到 Python 式——真正的 Python

### 在本教程中，您将学习到:函数式编程是一种编程范式，它基于将一个问题分解成…

realpython.com](https://realpython.com/python-reduce-function/)