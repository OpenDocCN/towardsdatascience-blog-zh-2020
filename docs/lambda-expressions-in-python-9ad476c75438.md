# Python 中的 Lambda 表达式

> 原文：<https://towardsdatascience.com/lambda-expressions-in-python-9ad476c75438?source=collection_archive---------26----------------------->

## 如何用 Python 编写 lambda(或匿名)函数

![](img/8898ce2089b70d2d03e1a30121dc9af6.png)

马库斯·斯皮斯克在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上的照片

## 介绍

假设我们正在编码，需要编写一个简单的函数。然而，我们只会使用这个函数一次，因此似乎没有必要为这个任务创建一个带有 def 关键字的完整函数。这就是 lambda 表达式的用武之地。

## 什么是 lambda 表达式？

[λ表达式](https://docs.python.org/3/reference/expressions.html)用于创建匿名函数，或者没有名字的函数。当我们需要创建一个只需要使用一次的函数(一次性函数)并且可以用一行代码编写时，它们非常有用。Lambda 函数可以有任意数量的参数，但只能有一个表达式。它们通常具有产生函数对象的这种格式:

> lambda 参数:表达式

## 使用 def 关键字创建函数

假设我们想写一个函数，它接受一个数作为输入，并返回这个数的平方。我们可以通过使用 def 关键字来做到这一点:

我们使用 def 关键字来定义这个函数。我们将这个函数命名为*平方*。这个函数有一个参数， *num* ，它使用**运算符返回这个数的平方。

## 带有一个参数的 Lambda 表达式

现在让我们把这个函数写成一个 lambda 表达式:

就是这样！我们首先从 lambda 关键字开始，然后是参数 *num* ，一个冒号，以及您希望该函数返回的内容，即 num**2。

注意，这个函数是匿名的，或者没有名字。所以我们不能在以后调用这个函数。另外，我们没有写退货。冒号后的所有内容都是将被返回的表达式的一部分。

如果我们想将 lambda 函数赋给一个变量，以便以后可以调用它，我们可以通过使用赋值运算符来实现:

然后，我们可以调用这个函数，就像调用用 def 关键字定义的函数一样。例如:

```
square(3) # will return 9 as the output
```

[](/best-way-to-solve-python-coding-questions-376539450dd2) [## 解决 Python 编码问题的最佳方式

### 了解如何有效解决 Python 编码问题！

towardsdatascience.com](/best-way-to-solve-python-coding-questions-376539450dd2) 

## 具有多个参数的 Lambda 表达式

让我们做一个 lambda 函数，它有两个参数，而不是只有一个。首先，我们将使用 def 关键字创建一个返回两个数之和的函数，然后我们将把它写成一个 lambda 表达式:

正如我们所见，如果我们希望我们的函数在一个 lambda 表达式中有多个参数，我们只需用逗号分隔这些参数。

就像使用 def 关键字创建的表达式一样，lambda 表达式不需要任何参数。例如，如果我们想要一个 lambda 表达式，它不接受任何参数，并且总是返回 True，我们可以这样写:

## Lambda 表达式中的条件语句

我们也可以在 lambda 表达式中包含 if else 语句。我们只需要确保它们都在一行上。例如，让我们创建一个函数，它接受两个数字并返回其中较大的一个:

我们的 lambda 表达式接受两个数字， *num1* 和 *num2* ，如果 *num1* 大于 *num2* ，则返回 *num2* 。显然，这个函数不考虑数字是否相等，因为在这种情况下，它将只返回 *num2* ，然而，我们只是说明如何在 lambda 表达式中使用条件语句。

[](/zip-function-in-python-da91c248385d) [## 在 Python 中压缩和解压缩 Iterables

### 如何在 Python 中压缩和解压缩可迭代对象

towardsdatascience.com](/zip-function-in-python-da91c248385d) 

## 那其他如果呢？

从技术上讲，我们不能在 lambda 表达式中使用 elif 语句。但是，我们可以将 if else 语句嵌套在 else 语句中，以获得与 elif 语句相同的结果。例如，如果我们还想检查 *num1* 是否大于 *num2* ，如果 *num2* 大于 *num1* ，或者(意味着它们是否相等)，我们可以使用下面的 lambda 表达式:

所以如果我们的 lambda 表达式发现*num 1*>num 2，就会返回 *num1* 。如果这个条件为假，它将继续执行 else 语句。在这个 else 语句中(在括号内)，它首先检查*num 1*<*num 2*是否为真。如果条件为真，它将返回 *num2* 。如果条件为假，它将返回 else 语句之后的内容，在本例中是字符串“它们相等”。

## 最后一个音符

Lambda 表达式在接受另一个函数作为参数的函数中非常有用。例如，在 map、filter 和 reduce 函数中，我们可以传入一个 lambda 表达式作为函数。

映射、过滤和减少功能的详细概述:

[](/using-map-and-filter-in-python-ffdfa8b97520) [## 在 Python 中使用地图和过滤器

### 如何使用 python 中内置的地图和过滤功能

towardsdatascience.com](/using-map-and-filter-in-python-ffdfa8b97520) [](/using-reduce-in-python-a9c2f0dede54) [## 在 Python 中使用 Reduce

### 如何使用 python 中的 reduce 函数

towardsdatascience.com](/using-reduce-in-python-a9c2f0dede54) 

如果你喜欢阅读这样的故事，并想支持我成为一名作家，考虑注册成为一名媒体成员。每月 5 美元，你可以无限制地阅读媒体上的故事。如果你用我的 [*链接*](https://lmatalka90.medium.com/membership) *报名，我就赚一小笔佣金。*

[](https://lmatalka90.medium.com/membership) [## 通过我的推荐链接加入媒体——卢艾·马塔尔卡

### 阅读卢艾·马塔尔卡的每一个故事(以及媒体上成千上万的其他作家)。您的会员费直接支持…

lmatalka90.medium.com](https://lmatalka90.medium.com/membership) 

## 结论

在本教程中，我们学习了什么是 lambda 表达式，如何用零参数、单参数和多参数来编写它们。我们还学习了如何在 lambda 表达式中使用 if else 语句。