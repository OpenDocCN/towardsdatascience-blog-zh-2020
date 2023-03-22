# 掌握 Python 中*args 和**kwargs 的 10 个例子

> 原文：<https://towardsdatascience.com/10-examples-to-master-args-and-kwargs-in-python-6f1e8cc30749?source=collection_archive---------0----------------------->

## 如何使用和不使用它们

![](img/7d424405e5d148b83daf3f3db2881779.png)

安德鲁·西曼在 [Unsplash](https://unsplash.com/s/photos/many?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) 上拍摄的照片

函数是 Python 中的构建块。它们接受零个或多个参数并返回值。就参数如何传递给函数而言，Python 相当灵活。*args 和**kwargs 使处理参数变得更加容易和简洁。

重要的部分是“*”和“**”。你可以用任何单词来代替 args 和 kwargs，但是通常的做法是用 args 和 kwargs。因此，没有必要进行不必要的冒险。

在这篇文章中，我们将讨论 10 个例子，我认为这些例子将使*args 和**kwargs 的概念变得非常清楚。

## 示例 1

考虑下面这个对两个数字求和的函数。

```
def addition(a, b):
   return a + bprint(addition(3,4))
7
```

这个函数只对两个数求和。如果我们想要一个三四个数相加的函数呢？我们甚至不想限制传递给函数的参数数量。

在这种情况下，我们可以使用*args 作为参数。

> *args 允许函数接受任意数量的位置参数。

```
def addition(*args):
   result = 0
   for i in args:
      result += i
   return result
```

传递给加法函数的参数存储在一个元组中。因此，我们可以迭代 args 变量。

```
print(addition())
0print(addition(1,4))
5print(addition(1,7,3))
11
```

## 示例 2

在第二个例子之前，最好解释一下位置论元和关键字论元的区别。

*   位置参数仅由名称声明。
*   关键字参数由名称和默认值声明。

调用函数时，必须给出位置参数的值。否则，我们会得到一个错误。

如果我们没有为关键字参数指定值，它将采用默认值。

```
def addition(a, b=2): #a is positional, b is keyword argument
   return a + bprint(addition(1))
3def addition(a, b): #a and b are positional arguments
   return a + bprint(addition(1))
TypeError: addition() missing 1 required positional argument: 'b'
```

我们现在可以做第二个例子。可以同时使用*args 和命名变量。下面的函数相应地打印传递的参数。

```
def arg_printer(a, b, *args):
   print(f'a is {a}')
   print(f'b is {b}')
   print(f'args are {args}')arg_printer(3, 4, 5, 8, 3)
a is 3
b is 4
args are (5, 8, 3)
```

前两个值给 a 和 b，其余的值存储在 args 元组中。

## 示例 3

Python 希望我们把关键字参数放在位置参数之后。在调用函数时，我们需要记住这一点。

考虑下面的例子:

```
arg_printer(a=4, 2, 4, 5)SyntaxError: positional argument follows keyword argument
```

如果我们给位置参数赋值，它就变成了关键字参数。因为它后面是位置参数，所以我们得到一个语法错误。

## 实例 4

在下面的函数中，选项是一个关键字参数(它有一个默认值)。

```
def addition(a, b, *args, option=True):
   result = 0
   if option:
      for i in args:
      result += i
      return a + b + result
   else:
      return result
```

如果选项为真，此函数执行加法运算。由于默认值为 True，除非 option parameter 声明为 False，否则该函数将返回参数的总和。

```
print(addition(1,4,5,6,7))
23print(addition(1,4,5,6,7, option=False))
0
```

## 实例 5

**kwargs 收集所有没有明确定义的关键字参数。因此，除了关键字参数之外，它的操作与*args 相同。

> **kwargs 允许函数接受任意数量的关键字参数。

默认情况下，**kwargs 是一个空字典。每个未定义的关键字参数都作为键值对存储在**kwargs 字典中。

```
def arg_printer(a, b, option=True, **kwargs):
   print(a, b)
   print(option)
   print(kwargs)arg_printer(3, 4, param1=5, param2=6)
3 4
True
{'param1': 5, 'param2': 6}
```

## 实例 6

我们可以在函数中同时使用*args 和**kwargs，但是*args 必须放在**kwargs 之前。

```
def arg_printer(a, b, *args, option=True, **kwargs):
   print(a, b)
   print(args)
   print(option)
   print(kwargs)arg_printer(1, 4, 6, 5, param1=5, param2=6)
1 4
(6, 5)
True
{'param1': 5, 'param2': 6}
```

## 例 7

我们可以使用*args 和**kwargs 打包和解包变量。

```
def arg_printer(*args):
   print(args)
```

如果我们将一个列表传递给上面的函数，它将作为一个单独的元素存储在 args tuple 中。

```
lst = [1,4,5]arg_printer(lst)
([1, 4, 5],)
```

如果我们在 lst 前加一个星号，那么列表中的值会被解包，单独存储在 args tuple 中。

```
lst = [1,4,5]arg_printer(*lst)
(1, 4, 5)
```

## 实施例 8

我们可以将多个 iterables 与单个元素一起解包。所有值都将存储在 args 元组中。

```
lst = [1,4,5]
tpl = ('a','b',4)arg_printer(*lst, *tpl, 5, 6)
(1, 4, 5, 'a', 'b', 4, 5, 6)
```

## 示例 9

我们也可以用关键字参数进行打包和解包。

```
def arg_printer(**kwargs):
   print(kwargs)
```

但是作为关键字参数传递的 iterable 必须是一个映射，比如字典。

```
dct = {'param1':5, 'param2':8}arg_printer(**dct)
{'param1': 5, 'param2': 8}
```

## 实例 10

如果我们还将额外的关键字参数与字典一起传递，它们将被合并并存储在 kwargs 字典中。

```
dct = {'param1':5, 'param2':8}
arg_printer(param3=9, **dct){'param3': 9, 'param1': 5, 'param2': 8}
```

## 结论

总结一下我们所讲的内容:

*   函数中有两种类型的参数，即位置参数(仅由名称声明)和关键字参数(由名称和默认值声明)。
*   调用函数时，必须给出位置参数的值。关键字参数是可选的(如果没有指定，则采用默认值)。
*   *args 收集未显式定义的位置参数，并将它们存储在元组中
*   * *除了关键字参数，kwargs 与*args 的作用相同。它们存储在字典中，因为关键字参数存储为名称-值对。
*   Python 不允许位置参数跟在关键字参数后面。因此，我们首先声明位置参数，然后声明关键字参数。

感谢您的阅读。如果您有任何反馈，请告诉我。

## 参考

[https://calmcode.io/args-kwargs/introduction.html](https://calmcode.io/args-kwargs/introduction.html)