# 关于 Python 函数你可能不知道的 4 件事

> 原文：<https://towardsdatascience.com/4-things-you-might-not-know-about-python-functions-1ff988accd67?source=collection_archive---------41----------------------->

## Python 函数远比你想象的有趣

![](img/fcba8c3934093d22d8b5d3f4093e14f1.png)

来源:https://unsplash.com/photos/feXpdV001o4

Python 作为一种多范式编程语言，因其符合任何程序员风格的能力而备受喜爱，这无疑使它成为世界上最流行的编程语言之一。尽管函数的性质千变万化，但它仍然是语言必不可少的一部分，真正理解语言的这一部分对掌握语言本身大有帮助。这就是为什么在本文中，我将讨论 Python 函数的四个鲜为人知的方面，这将有望让您对 Python 的威力有新的认识，无论您是 Python 老手还是完全的新手。

## 1.反思你的功能

在编程上下文中，自省是检查您用更多代码编写的代码。这里要记住的关键是 Python 函数本质上是对象，这意味着它们有相关联的属性和方法，这些属性和方法可以给你关于函数本身的信息。这些属性中的大多数是在声明函数时创建的，但是您也可以在事后添加新的属性，就像您对任何其他对象所做的那样。

```
def example():
   """ This is an example docstring
   through which I can provide more information
   about the function
   """    print("Hello World")example.__doc__       #Returns the docstring attribute 
example.__annotation__dir(example)#['__annotations__', '__call__', '__class__', '__closure__', '__code__', '__defaults__', '__delattr__', '__dict__', '__dir__', '__doc__', '__eq__', '__format__', '__ge__', '__get__', '__getattribute__', '__globals__', '__gt__', '__hash__', '__init__', '__init_subclass__', '__kwdefaults__', '__le__', '__lt__', '__module__', '__name__', '__ne__', '__new__', '__qualname__', '__reduce__', '__reduce_ex__', '__repr__', '__setattr__', '__sizeof__', '__str__', '__subclasshook__']# These are all the built-in function attributes 
```

事实上，我们定义的任何函数都包含了大量的属性。要查看所有这些函数，只需将您的函数传递给 dir()函数(如上所示)，就会返回所有可用方法和属性的数组。

一些有趣的属性如下:

*   功能。__defaults__:返回所有默认参数值的元组
*   功能。__doc__:返回文档字符串
*   功能。__name__:以字符串形式返回函数的名称
*   功能。__globals__:返回一个字典，其中包含函数可以访问的所有全局变量
*   功能。__code__:返回 __code__ 对象，该对象本身具有各种参数，与函数中的实际代码相关(使用 dir()函数来检查该对象的所有相关方法和属性)。

用函数自省可以做的事情还有很多，我无法用一篇文章的篇幅来介绍，所以一定要打开你的 IDE，开始深入研究你自己的函数！

## 2.函数类型注释

Python 的灵活性部分来自于它作为动态类型编程语言的本质，在函数的上下文中，这意味着您可以传入任何类型的参数(字符串、整数、数组、布尔值)，并且错误只会在运行时出现(例如，如果您试图将字符串添加到函数中的数组，此时您的程序将会抛出一个 TypeErrror)。

这是一种更灵活的编程方法，因为在静态类型的语言中，如 Java 和 C++，函数参数的类型必须预先确定，并且事后不能更改。这使得编程更加严格，需要更多的远见，但它有利于在运行时消除错误，并节省运行时错误检查的性能成本。

然而，一些 Python 程序员选择使用函数注释，通过为所有函数输入和输出提供建议的类型，来帮助 Python 表现得更像静态类型语言(尽管代码仍然是动态类型的)。例如:

```
def a_function(a: 'Int', b: 'String') -> "Repeats a String":
   return a * b# The annotations inside the brackets indicate what type each argument should be. (Notice the use of colons, as opposed to the equals signs, as in the case of argument defaults)# The annotation after the function indicates the type of the return valuea_function.__annotations__#{'a': 'Int', 'b': 'String', 'return': 'Repeats a String'}
# Returns a dictionary of all the associated annotations 
```

通过使用注释，您仍然可以将不正确的数据类型传递给函数，但是您至少可以得到正确类型的指示，从而减少用户出错的机会。您还可以通过在函数上运行 help()函数来访问这些注释，或者等效地，使用 Jupyter 笔记本中的 Shift + Tab 快捷键，它会返回文档字符串、注释和其他函数信息。

## 3.Args，Kwargs，参数排序和可迭代解包

在解释*args 和**kwargs 的值和用法之前，我需要先解释一下 Python 中打包和解包的概念。

假设我们有一个值数组，我们希望将每个值存储到单独的变量中。我们可以这样写:

```
a, b, c = [1,2,3]#a = 1
#b = 2
#c = 3
```

然而，当数组的长度未知时，这个操作变得更加棘手，因为我们不知道需要多少个独立的变量。此外，我们可能希望将第一个值存储为单个变量，但将其余的值保存在一个新数组中。这就是 Python 中解包功能的来源。使用' * '符号，我们可以将剩余的值存储在一个数组中，如下所示:

```
a, *b = [1,2,3,4,5,6]# a = 1
# b = [2,3,4,5,6]
```

星号基本上是说:“取任何尚未赋值的值，并将其存储到一个变量中”。这种技术适用于任何可迭代对象，但在函数中打包参数的情况下尤其有用。它允许我们拥有不确定数量的位置和关键字参数，然后我们可以在函数表达式中索引/迭代这些参数。例如:

```
def my_func(*args, **kwargs):
   sum(args) # Note that although it is convention to use the variable names args and kwargs, you can name the variables whatever you'd like
```

请注意，当谈到函数参数时，我们必须按照特定的顺序放置参数:

1.  位置参数
2.  *参数
3.  关键字参数
4.  * *克瓦查

这意味着，在我们将 a *args 放入函数参数之后，后面的参数将被自动视为关键字参数。另一个例子很好地说明了这一点:

```
def myfunc(a, b, *args, kw, **kwargs):
    print(args)
    print(kwargs)myfunc(1, 2, 3, 4, 5, kw = 6, kw2 = 7, kw3 = 8)# (3, 4, 5)
# {'kw2': 7, 'kw3': 8}
```

如您所见，所有在前两个位置参数(a 和 b)之后但在第一个关键字参数(kw)之前的位置参数将存储在一个元组中，该元组可以作为变量“args”引用。*args 参数实际上用尽了所有剩余的位置参数。同样，kw 之后的所有关键字参数都将存储在一个字典中，可以用变量“kwargs”引用该字典。

*Args 和**Kwargs 在您不确定一个函数将接受多少个参数作为输入的情况下非常有用，并且允许您在用 Python 编写函数时更加灵活。

## 4.利用 Lambda 表达式

Lambda 函数是 Python 对匿名函数的实现。如果您不熟悉这个概念，匿名函数本质上是普通函数的单一使用版本，没有名称，通常用于传递给更高阶的函数。

创建 lambda 函数的语法如下:

```
lambda [parameters]: expression#for example:lambda x: x + 3# Takes a value x, and returns x + 3
```

lambda 关键字表示您正在创建一个内联函数。然后，向 lambda 函数提供参数，类似于在函数定义的括号中提供的内容。最后，在冒号之后，编写表达式本身，在调用 lambda 函数时对其求值。

lambda 函数的一个很好的用例是在 map 或 filter 中，因为它有助于编写高度 Pythonic 化和紧凑的代码。例如，如果我们要过滤数组中大于 10 的值:

```
arr = [1,3,6,2,13,15,17]list(filter(arr, lambda x: x > 10))# the above function will return a new list where the array values are greater than 10 
```

这就是我所说的将函数传递给高阶函数的意思，因为这里我们将 lambda 函数传递给 filter()函数，后者将 iterable 和函数作为位置参数。

实际上，您也可以使用 lambda 函数创建一个常规函数，只需将它赋给一个变量名。该函数现在可以像任何其他函数一样被重用和调用。

我希望你能从这篇文章中学到一些新的东西，如果你对我写的东西有任何问题，欢迎在下面发表评论！