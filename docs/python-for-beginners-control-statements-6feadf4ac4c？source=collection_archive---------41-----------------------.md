# Python 初学者——控制语句

> 原文：<https://towardsdatascience.com/python-for-beginners-control-statements-6feadf4ac4c?source=collection_archive---------41----------------------->

## 了解从基础到高级的条件执行

![](img/5608394fb8806296d586a067331a43dc.png)

照片由 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上的 [Yancy Min](https://unsplash.com/@yancymin?utm_source=medium&utm_medium=referral) 拍摄

# 介绍

对于编写顺序执行的程序来说，这个世界通常是一个复杂的地方。不管你在做什么项目，都需要跳过一些语句，然后递归地执行一系列语句。

这是任何语言中条件语句出现的地方；通常，大多数语言都有一个 if 语句作为开始的基本步骤，Python 也是如此。不再拖延，让我们探索用 Python 编写条件语句的不同方法。

# ``If``声明

简单来说，if 语句是两个部分的组合:表达式和语句。

**表达式:**需要计算并返回布尔值(真/假)的条件。

**语句:**语句是一个通用的 python 代码，当**表达式**返回 **true 时执行。**

if 语句的语法如下所示:

```
if <**expression**>:
    <**statement**>
```

请始终记住，表达式后面的冒号(:)是强制的，要将它放到操作中，我们可以做如下所示的事情:

python 中简单的 if 语句

如果表达式返回 true，我们还可以执行多个语句，请看:

if 块用多个语句执行

为了有趣，我们可以在一行中编写各种语句，用分号分隔，请看:

在单行中包含多个语句的 if 块

我们还可以使用 if 语句，通过使用`in`操作符来检查一个值是否在字典中。看一看:

# “else”和“elif”

使用 if 语句，我们可以执行一组选定的语句，但是如果我们希望在条件失败时执行另一组语句，该怎么办呢？这时候`else`进来了。看一下语法:

```
if <**expression**>:
    <**statement1**>
else:
    <**statement1**>
```

类似于 if 语句我们需要在 else 后面使用冒号(:)并保持缩进，看一个简单的例子:

简单的 if / else 执行

让事情变得复杂一点，如果我们需要检查不止一个条件呢？那就是`elif`曝光的时候。`elif`充当 if /else 块的组合。

如果前面的条件不满足，并且满足当前表达式，则执行`elif`块下的语句块。看一下语法:

```
if <expression>:
    <statement1>
**elif** <expression>:
    <statement2>
**elif** <expression>:
    <statement3>
    ...
else:
    <statement4>
```

让我们写一段简单的代码来更好地理解它。

简单 elif 分组码

# 使用“get”进行条件执行

到目前为止，我们已经看到了通用的解决方案；为了让事情变得更复杂一点，我们可以在字典上使用`get`函数。这个过程包括将必要的条件和输出作为键值对包含在字典中。

为了更好地理解它，让我们写一个简单的例子，看一看:

在这里，字典表情符号充当指示表达式和语句执行的键值对。当我们以 input 作为第一个参数调用字典上的 get 函数时，它将检查具有该值的键。如果找到了，它打印键值，否则打印第二个参数。这里，第二个参数充当 else case。

# 三元运算符

Python 引入了一个三元运算符来保持最小化。换句话说，我们可以说它是 if / else 块的最简单的书写形式。

当我们深入研究控制语句时，我们需要考虑三件主要的事情:条件表达式、肯定结果和否定结果。整个条件执行都围绕着他们。让我们看看 Python 中三元运算符的语法:

```
<**positive_result**> if <**conditional_****expression**> else <**negative_result**>
```

这个操作符旨在减少样板代码，创建一个更简洁、可读性更强的程序。让我们用三元运算符写一个简单的例子:

# 奖金

要了解更多关于 Python 从基础到高级的知识，请阅读以下文章

*   [“Python 初学者—基础知识”](https://medium.com/android-dev-hacks/python-for-beginners-basics-7ac6247bb4f4)
*   [《Python 初学者——函数》](/python-for-beginners-functions-2e4534f0ae9d)
*   [《Python 初学者——面向对象编程》](https://medium.com/better-programming/python-for-beginners-object-oriented-programming-3b231bb3dd49)

就这些了，希望你能学到一些有用的东西，感谢阅读。

你可以在 [Medium](https://medium.com/@sgkantamani) 、 [Twitter](https://twitter.com/SG5202) 、 [Quora](https://www.quora.com/profile/Siva-Ganesh-Kantamani-1) 和 [LinkedIn](https://www.linkedin.com/in/siva-kantamani-bb59309b/) 上找到我。