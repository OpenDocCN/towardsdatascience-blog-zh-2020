# Python 装饰者 5 分钟指南

> 原文：<https://towardsdatascience.com/5-minute-guide-to-decorators-in-python-b5ca0f2c7ce7?source=collection_archive---------23----------------------->

## 立即掌握更高级的主题

毫无疑问，Python decorators 是更高级、更难理解的编程概念之一。这并不意味着您应该避免学习它们——因为您迟早会在生产代码中遇到它们。这篇文章将帮助你立刻掌握这个概念。

![](img/4238a12f5f537ea6d8f6cd0088accb6f.png)

[萨巴·萨亚德](https://unsplash.com/@sabasayad?utm_source=medium&utm_medium=referral)在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 拍摄的照片

阅读这篇文章不会花费你超过 5 分钟的时间，如果你按照代码来做的话，可能需要 10 分钟。在那段时间里，您将从基础开始了解装饰器的概念——常规的 Python 函数。

此外，这篇文章旨在逐步提高你的理解水平，这样你就不会在这个过程中感到困惑。在开始编写代码之前，我们将快速了解一下装饰者是什么。稍后，在文章的结尾，我们将讨论装饰者的一些好处和用例，所以不要错过。

中间部分留给概念本身。开始吧！

# 什么是装修工？

装饰器是一个函数，它接受另一个函数并扩展后一个函数的行为，而不需要显式修改它。Python 修饰器可以使代码更短，更有 Python 风格。这是一个高级主题，一旦您理解了这个概念，就可以使编码变得更容易。

在我们深入探讨之前，让我们快速回顾一下基础知识。

## 装饰器中使用的函数

函数是编写可重复使用的特定代码的一种方式。它们存在于几乎每一种编程语言中，是每一个程序的重要组成部分。

下面是一个函数定义示例:

```
def function_name(args):
    #code
```

现在我们知道了什么是函数，下一个要理解的主题是函数中的函数:

```
def calc(name='add'):
    print('now you are inside the calc() function') def sub():
        return 'now you are in the sub() function' def divide():
        return 'now you are in the divide() function'
```

与此类似，我们也可以从另一个函数中返回一个函数:

```
def calc(name='add'):
    print('Now you are in the calc() function') def add():
        return 'Now you are in the add() function' def divide():
        return 'Now you are in the divide() function' if name == 'add':
        return add
    else:
        return divide
```

在上面的代码中，我们可以很容易地看到，在 if/else 子句的帮助下，我们可以在函数中返回函数。您需要理解的最后一点是，装饰器将一个函数作为另一个函数的参数:

```
def welcome():
    return 'Welcome to Python!'def do_something_before_welcome(func):
    print('Something before executing welcome()')
    print(func())

do_something_before_welcome(welcome) **Output:
>>> Something before executing welcome()
>>> Welcome to Python!**
```

在我们深入到装饰者之前，这差不多是你应该知道的全部。现在有趣的部分开始了。

## 装饰师蒸馏

现在我们有了理解装饰者所需的知识。让我们快速创建一个:

```
def my_decorator(func):
    def wrapper():
        print('Before function call')
        func()
        print('After function call')
        return wrapperdef say_where():
    print('say_where() function') say_where = my_decorator(say_where)**Output:
>>> Before function call
>>> say_where() function
>>> After function call**
```

读完上面的例子后，你有望更好地理解 decorators，因为我们刚刚应用了之前所涉及的函数的所有基础知识。

Python 允许我们通过@符号更容易地使用 decorators 有时称为 pie 语法。

让我们看看如何将它应用到上面的例子中:

```
def my_decorator(func):
    def wrapper():
        print('Before function call')
        func()
        print('After function call')
    return wrapper@my_decorator
def say_where():
    print('say_where() function')**Output:
>>> Before function call
>>> say_where() function
>>> After function call**
```

所以， *@my_decorator* 只是`say_where = my_decorator(say_where)`更简单的说法。这就是如何将装饰器应用到函数中。

现在我们对 python 装饰器有了一个清晰的概念，但是为什么我们首先需要它们呢？让我们在下一节回顾一些好处。

# 装修工——为什么？

我们已经讨论了装饰者的*如何*部分，但是*为什么*部分你可能还是有点不清楚。这就是为什么我准备了几个真实世界的装饰用例。

## 分析、日志记录和工具

我们经常需要具体地度量正在发生的事情，并记录量化不同活动的度量标准。通过在封闭的函数或方法中总结这些值得注意的事件，装饰者可以很容易地处理这个非常具体的需求。

## 验证和运行时检查

对于所有专业的 Python 类型系统来说，有一个缺点。这意味着一些错误可能会试图潜入，而更多的静态类型语言(如 Java)会在编译时捕捉到这些错误。除此之外，您可能希望对进出的数据执行更复杂的自定义检查。装饰者可以让你轻松地处理所有这些，并一次将它应用于许多功能。

## 制作框架

当您编写 decorators 时，您可以选择利用它们的简单语法，这让您可以向语言添加难以利用的语义。最好能有扩展语言结构的选项。许多众所周知的开源系统都利用了这一点。web 框架 *Flask* 利用它将 URL 路由到处理 HTTP 请求的能力。

我希望这三个用例已经让你相信装饰者在现实世界的任务中是多么重要。说到这里，我们来到了本文的结尾。让我们在下一部分总结一下。

# 在你走之前

一开始，装饰者并不是一个容易理解的概念。对我来说，这个概念需要多次阅读(和动手任务)才能感到足够自信，但一旦你到达那里，一切都是值得的。

所以，慢慢来，不要着急。确保首先理解这些函数，以及我们今天讨论的所有内容。从那里开始扩展很容易。

我还想提一下，这是更高级的 Python 概念系列中的第一篇，接下来将讨论生成器和并行性等主题。它们对于可伸缩的、干净的编程环境也是必不可少的，所以一定要保持关注。

感谢阅读。

*喜欢这篇文章吗？成为* [*中等会员*](https://medium.com/@radecicdario/membership) *继续无限制学习。如果你使用下面的链接，我会收到你的一部分会员费，不需要你额外付费。*

[](https://medium.com/@radecicdario/membership) [## 通过我的推荐链接加入 Medium-Dario rade ci

### 作为一个媒体会员，你的会员费的一部分会给你阅读的作家，你可以完全接触到每一个故事…

medium.com](https://medium.com/@radecicdario/membership) 

[**加入我的私人邮件列表，获取更多有用的见解。**](https://mailchi.mp/46a3d2989d9b/bdssubscribe)