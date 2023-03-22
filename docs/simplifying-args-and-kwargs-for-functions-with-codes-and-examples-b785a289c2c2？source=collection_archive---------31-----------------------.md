# 用代码和例子简化函数的 args 和 kwargs！

> 原文：<https://towardsdatascience.com/simplifying-args-and-kwargs-for-functions-with-codes-and-examples-b785a289c2c2?source=collection_archive---------31----------------------->

## 理解 python 编程和机器学习的*args 和*kwargs 的完整概念。

![](img/bd3ef4430beb23e2391a18029b542603.png)

[Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上 [AltumCode](https://unsplash.com/@altumcode?utm_source=medium&utm_medium=referral) 拍摄的照片

函数是 python 和面向对象编程不可或缺的一部分。

Python 是一种优雅而简单的语言，它为用户提供了多种更简单有效的编码方式。

其中一个概念是在 python 中包含*args 和*kwargs。这些向函数传递可变数量参数的方法使得 python 编程语言对于复杂问题非常有效。

正确使用它们的用途很多。函数中的这两个参数将非常有用，即使在机器学习和深度学习的概念中，同时尝试为各自的项目构建您的自定义层或函数。

在本文中，我们旨在通过代码和示例直观地理解这些概念。所以，事不宜迟，让我们开始深入了解这些概念。

在开始编程之前，让我们简单回顾一下函数这个主题。

# 什么是功能？

**函数**是写在程序中的一段代码，这样它们可以被多次调用。函数的主要用途是在同一个程序中可以被多次重复调用，而不需要一遍又一遍地编写相同的代码。然而，你也可以用它来为你的程序提供更多的结构和更好的整体外观。

使用关键字 **'def，**定义函数，可以使用已定义或未定义的参数调用这些函数。当您调用特定的函数时，python 编译器会解释将要返回的任何值。

下面是一个典型函数的代码块—

```
def func(parameters if required):
   # code here
   return # solution
```

在 Python 中，我们可以使用特殊符号向函数传递可变数量的参数。这有两种可能性，如下所示:

1.  *参数(非关键字参数)
2.  **kwargs(关键字参数)

让我们分别理解这些概念，从*args 开始，然后到**kwargs。

# *参数

python 中函数定义中的特殊语法 **args* 用于向函数传递可变数量的参数。它用于传递一个无关键字、可变长度的参数列表。

为什么您的函数需要这个参数选项？

假设篮子里有 3 个水果，你写一个包含 3 个变量的函数，定义如下:

您将得到以下输出。

```
Apple Mango Banana
```

一切看起来都很好，直到你意识到你还买了一些葡萄，你想把它也添加到你的功能中。你可以添加一个额外的变量，再次调用它，继续这个过程。然而，你购买的水果越多，你就不得不一遍又一遍地更新。这就是*args 函数发挥作用的地方。

您可以简单地执行此功能，如下所示:

语法是使用符号*接受可变数量的参数。按照惯例，它通常与 args 一词连用。*args 允许您接受比您之前定义的形式参数更多的参数。

这个过程简单有效。*args 的概念非常有用，如本节所述。让我们继续**kwargs 部分，并详细理解这个概念。

# *** *夸脱**

python 函数定义中的特殊语法 ***kwargs* 用于传递带关键字的可变长度参数列表。我们使用带有双星的 *kwargs* 这个名字。原因是双星号允许我们传递关键字参数(以及任意数量的关键字参数)。

**kwargs 是一种类似字典的操作，将元素值映射到它们各自的。让我们用一个类似水果的例子来理解这个概念。假设您想按水果摆放的顺序或您最喜欢的顺序对它们进行排序。这个过程可以按照下面代码块的建议来完成:

你会得到下面的输出—

```
first Apple
second Mango
third Banana
fourth Grapes
```

关键字参数是在将变量传递给函数时为变量提供名称的地方。人们可以把 *kwargs* 看作是一个字典，它将每个关键字映射到我们传递给它的值。这就是为什么当我们迭代 kwargs 时，它们似乎没有任何打印顺序。

对于更复杂的项目，您可以同时使用*args 和*kwargs 函数来执行这些任务。我强烈建议您亲自尝试一下，以便更好地理解这些概念。

![](img/d668bb40d828073d855ecd42954383cc.png)

照片由[思想目录](https://unsplash.com/@thoughtcatalog?utm_source=medium&utm_medium=referral)在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄

# 结论:

总结一下我们在本文中讨论的内容，函数是 python 和面向对象编程不可或缺的一部分。我们简要介绍了 python，并对函数在 Python 编程语言中的作用有了基本的了解。

详细讨论了*args 和*kwargs 的概念，以及这些参数选择在 python 编程中到底有多有用。在这些主题的帮助下，我们可以编写更高效的代码来处理复杂的任务。

如果你对今天的话题有任何疑问，请在下面的评论中告诉我。我会尽快回复你。如果我错过了什么，请随时告诉我。您希望我在以后的文章中介绍这一点。

查看其他一些可能对您的编程之旅有用的文章！

[](/simple-fun-python-project-for-halloween-ff93bbd072ad) [## 简单有趣的万圣节 Python 项目！

### 这是一个有趣的“不给糖就捣蛋”的游戏，让你在万圣节愉快地学习 python 编程

towardsdatascience.com](/simple-fun-python-project-for-halloween-ff93bbd072ad) [](/5-unique-python-modules-for-creating-machine-learning-and-data-science-projects-that-stand-out-a890519de3ae) [## 5+独特的 Python 模块，用于创建脱颖而出的机器学习和数据科学项目！

### 超过 5 个酷 Python 库模块的指南，用于创建令人敬畏的机器学习和数据科学项目。

towardsdatascience.com](/5-unique-python-modules-for-creating-machine-learning-and-data-science-projects-that-stand-out-a890519de3ae) [](/understanding-advanced-functions-in-python-with-codes-and-examples-2e68bbb04094) [## 用代码和例子理解 Python 中的高级函数！

### 详细了解 python 中的匿名函数和高级函数及其实际应用…

towardsdatascience.com](/understanding-advanced-functions-in-python-with-codes-and-examples-2e68bbb04094) 

谢谢你们坚持到最后。我希望你们喜欢阅读这篇文章。我希望你们都有美好的一天！

## 参考资料:

1.  [极客对极客](https://www.geeksforgeeks.org/args-kwargs-python/)
2.  油管（国外视频网站）