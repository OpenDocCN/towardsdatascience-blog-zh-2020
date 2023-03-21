# Python 异常和异常处理初学者指南

> 原文：<https://towardsdatascience.com/exception-handling-in-python-85f49801b131?source=collection_archive---------25----------------------->

## Python 中的异常处理是通过使用 try except 子句来完成的

![](img/e1cfbddb26a081df30c0856b1c27f912.png)

照片由 [Hitesh Choudhary](https://unsplash.com/@hiteshchoudhary?utm_source=medium&utm_medium=referral) 在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄

# 介绍

在本教程中，我们将学习如何使用 try except 子句在 Python 中处理异常。

但是首先，什么是例外？🤔

异常是当代码的执行导致意外结果时，由我们的代码引发的错误。通常，异常会有一个错误类型和一条错误消息。一些例子如下。

```
ZeroDivisionError: division by zero
TypeError: must be str, not int
```

`ZeroDivisionError`和`TypeError`是错误类型，冒号后的文本是错误消息。错误消息通常描述错误类型。

每一个好的代码和应用程序都会处理异常，以保持用户体验的流畅和无缝。那么，让我们来学习如何做到这一点，好吗？🙂

# 内置异常

在 Python 中，所有内置的异常都是从`BaseException`类中派生出来的。直接继承`BaseException`类的异常类有:`Exception`、`GeneratorExit`、`KeyboardInterrupt`和`SystemExit`。在本教程中，我们将重点关注`Exception`类。

在许多情况下，我们希望定义自己的自定义异常类。这样做的好处是，我们可以通过用甚至非程序员也能理解的简单英语来指出程序中出现的错误。

要定义一个定制的异常类，我们需要从`Exception`类继承，而不是像官方 Python 3 文档[中建议的那样从`BaseException`类继承。](https://docs.python.org/3/library/exceptions.html)

酷毙了。现在我们对异常有了一些基本的了解，让我们来看一些例子。

# 常见例外

现在，我们将看看在学习 Python 编程时可能遇到的一些最基本和最常见的异常。

## 值错误

当我们传入一个正确类型的值，但该值是错误的时，就会引发这个异常。举个例子，假设我们想把一个字符串转换成一个整数。

```
int("5")  // 5
```

这很好，对吧？现在，看看当我们传递一个不能转换成整数的字符串时会发生什么。

```
int("five")Traceback (most recent call last):
  File "<input>", line 1, in <module>
ValueError: invalid literal for int() with base 10: 'five'
```

现在说得通了吧？在第二个例子中，尽管参数的类型是好的，但是值是不正确的。

## 索引错误

这个异常告诉我们，我们正试图通过超出范围的索引来访问集合中的项目或元素(例如，字符串或列表对象)。

```
some_list = [1, 2, 3, 4]
some_list[5]Traceback (most recent call last):
  File "<input>", line 1, in <module>
IndexError: list index out of range
```

在这个例子中，`some_list`的最大索引是 3，我们要求程序访问索引为`[5]`的元素。因为该索引不存在(或者超出范围)，所以抛出`IndexError`异常。

## 类型错误

与`ValueError`不同，当我们传递给函数或表达式的参数或对象的类型是错误的类型时，就会引发这个异常。

```
100 + "two hundred"Traceback (most recent call last):
  File "<input>", line 1, in <module>
TypeError: unsupported operand type(s) for +: 'int' and 'str'
```

这里的错误信息非常清楚。它告诉我们`+`操作符不能用于类型为`int`和`str`的对象。

## 名称错误

每当我们试图使用一个不存在或尚未定义的变量时，就会抛出这个异常。

```
a = "defined"print(b)Traceback (most recent call last):
  File "<input>", line 1, in <module>
NameError: name 'b' is not defined
```

同样，错误信息非常清楚。这个不用多解释了。🙂

还有许多更常见的异常，但我希望这已经是足够的例子了。

现在，你可能会问，写着`Traceback (most recent call last)`的前几行是什么？好吧，让我们在下一节讨论一下。

![](img/1891a749a0836de145f862aaca0d00bd.png)

照片由[马丁·比约克](https://unsplash.com/@martenbjork?utm_source=medium&utm_medium=referral)在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄

# 回溯(或堆栈跟踪)

在上一节的所有示例异常中，有一点值得一提，那就是异常类型和消息之前的部分。这部分被称为回溯或堆栈跟踪。

```
Traceback (most recent call last):
  File "<input>", line 1, in <module>
```

当我们的代码比上面的例子复杂得多时，这是很有用的，我敢打赌一定会如此，尤其是我们的应用程序代码。回溯或堆栈跟踪本质上告诉我们代码执行的顺序，从最新的(顶部)到最早的(底部)。

让我们看一些来自 Python 官方[文档](https://docs.python.org/2/library/traceback.html)的例子。

```
Traceback (most recent call last):
  File "<doctest...>", line 10, **in** <module>
    lumberjack()
  File "<doctest...>", line 4, **in** lumberjack
    bright_side_of_death()
IndexError: tuple index out of range
```

这里，回溯告诉我们导致`IndexError`异常的最后一次代码执行位于第 10 行，特别是在调用`lumberjack()`函数时。

它还告诉我们，在调用`lumberjack()`函数之前，我们的程序从第 4 行开始执行`bright_side_of_death()`函数。

在真实世界的应用程序中，我们的代码由许多模块组成，回溯将有助于调试。因为它指导我们哪里出了问题，在哪里以及执行的顺序。

# 用户定义的异常

如前所述，Python 允许我们定义自己的定制异常。为此，我们需要定义一个继承了`Exception`类的类。

```
class MyCustomException(Exception):
    def __init__(self, code, message):
        self.code = code
        self.message = message
```

在讨论这个问题的同时，让我们看看如何在代码中引发一个异常。我们现在将定义一个抛出`MyCustomException`的函数。

```
def process_card(card_type):
    if card_type == "amex":
        raise MyCustomException("UNSUPPORTED_CARD_TYPE", "The card type used is not currently supported.")
    else:
        return "OK"
```

现在，让我们通过传递`"amex"`作为参数来调用`process_card`，这样它将抛出`MyCustomException`。

```
process_card("amex")Traceback (most recent call last):
  File "<input>", line 1, in <module>
  File "<input>", line 3, in process_card
MyCustomException: ('UNSUPPORTED_CARD_TYPE', 'The card type used is not currently supported.')
```

完美！因此，为了从我们的代码中抛出一个异常，要使用的关键字是`raise`关键字。

好了，我希望现在你已经很好地理解了什么是异常，尤其是在 Python 中。接下来，我们要学习如何处理它。

![](img/ffbf5e1ffa30d28f245e4601c5c75624.png)

Andrés Canchón 在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 拍摄的照片

# 用 Try Except 子句处理异常

Python 为我们提供了 try except 子句来处理代码可能引发的异常。try except 子句的基本结构如下。

```
try:
    // some codes
except:
    // what to do when the codes in try raise an exception
```

简单地说，try except 子句基本上是在说，“尝试这样做，除非有错误，然后改为这样做”。

对于如何处理从`try`块抛出的异常，有几个选项。我们来讨论一下。

## 重新引发异常

让我们看看如何编写 try except 语句，通过重新引发异常来处理异常。

首先，让我们定义一个函数，它接受两个输入参数并返回它们的和。

```
def myfunction(a, b):
    return a + b
```

接下来，让我们将它包装在一个 try except 子句中，并传递错误类型的输入参数，这样函数将引发`TypeError`异常。

```
try:
    myfunction(100, "one hundred")
except:
    raiseTraceback (most recent call last):
  File "<input>", line 2, in <module>
  File "<input>", line 2, in myfunction
TypeError: unsupported operand type(s) for +: 'int' and 'str'
```

这与我们不在 try except 子句中包装函数的情况非常相似。

## 捕捉某些类型的异常

另一个选择是定义我们想要具体捕捉哪些异常类型。为此，我们需要将异常类型添加到`except`块中。

```
try:
    myfunction(100, "one hundred")
except TypeError:
    print("Cannot sum the variables. Please pass numbers only.")Cannot sum the variables. Please pass numbers only.
```

酷毙了。它开始变得更好看了，不是吗？

现在，为了更好，我们实际上可以记录或打印异常本身。

```
try:
    myfunction(100, "one hundred")
except TypeError as e:
    print(f"Cannot sum the variables. The exception was: {e}")Cannot sum the variables. The exception was: unsupported operand type(s) for +: 'int' and 'str'
```

很好。🙂

此外，如果我们想以同样的方式处理这些异常类型，我们可以在一个`except`子句中捕获多个异常类型。

让我们将一个未定义的变量传递给我们的函数，这样它将引发`NameError`。我们还将修改我们的`except`块来捕获`TypeError`和`NameError`，并以同样的方式处理任一异常类型。

```
try:
    myfunction(100, a)
except (TypeError, NameError) as e:
    print(f"Cannot sum the variables. The exception was {e}")Cannot sum the variables. The exception was name 'a' is not defined
```

注意，我们可以添加任意多的异常类型。基本上，我们只需要传递一个包含类似`(ExceptionType1, ExceptionType2, ExceptionType3, ..., ExceptionTypeN)`的异常类型的`tuple`。

通常，我们会有另一个没有特定异常类型的`except`块。这样做的目的是捕捉未处理的异常类型。一个用例是当向第三方 API 发出 Http 请求时，我们可能不知道所有可能的异常类型，但是，我们仍然希望捕捉并处理它们。

让我们修改我们的 try except 块，以包含另一个 except 块，该块将捕获其余的异常类型。

```
try:
    myfunction(100, a)
except (TypeError, NameError) as e:
    print(f"Cannot sum the variables. The exception was {e}")
except Exception as e:
    print(f"Unhandled exception: {e}")
```

如果我们的函数引发了除`TypeError`和`NameError`之外的任何异常，它将转到最后一个`except`块并打印`Unhandled exception: <unhandled exception here>`。

![](img/6ec13a0b1a174fb2d1e4f076cec39989.png)

Photo by [傅甬 华](https://unsplash.com/@hhh13?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

# 包裹

哇！大家学习 Python 中的异常和异常处理做得很好。我希望那不是太糟糕😆

如您所知，我们仍然可以通过添加`else`和`finally`块来扩展我们的 try except 子句。在未来的教程中会有更多的介绍。或者你想自己看，可以去看看这个[教程](https://docs.python.org/3/tutorial/errors.html)。

一如既往，请随意发表任何评论或提出任何问题。🙂