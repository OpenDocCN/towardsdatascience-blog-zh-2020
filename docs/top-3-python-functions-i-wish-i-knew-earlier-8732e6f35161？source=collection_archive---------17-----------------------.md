# 我希望早点知道的 3 大 Python 函数

> 原文：<https://towardsdatascience.com/top-3-python-functions-i-wish-i-knew-earlier-8732e6f35161?source=collection_archive---------17----------------------->

## 你不知道你不知道什么。

尽管 Python 是最简单的编程语言之一，但它仍然有各种各样的内置函数，这些函数不为广大读者所知——主要是因为大多数书籍或在线课程都没有涉及到它们。

![](img/caee77ec185424fa7aac0a5d86c92b0a.png)

[西恩·佛利](https://unsplash.com/@_stfeyes?utm_source=medium&utm_medium=referral)在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上的照片

然而，这并不意味着它们是不相关的。

本文的目的是向您介绍一些鲜为人知的功能，并展示如何以最简单的方式实现它们。目标读者至少了解 Python 和一般编程的基础，并致力于进一步提高他/她的技能。

这篇文章的结构如下:

1.  基本设置
2.  功能#1: `isinstance()`
3.  功能#2: `hasattr()`
4.  功能#3: `exec()`
5.  结论

所以，事不宜迟，我们开始吧！

# 基本设置

现在，我们不需要导入任何东西，因为本文完全是基于 Python 的，但是我们仍然需要声明一些要使用的东西。

对于下面的大多数例子，我们将使用这个简单的类:

```
class Circle():
   def __init__(self):
       self.radius = None
```

现在里面什么都没有发生——这很好。我们仍然可以用它来演示这些函数是关于什么的。

好的，你在跟踪我吗？好，我们开始吧。

# 功能#1: `isinstance()`

官方文档[1]很好地解释了这个函数的含义:

> 如果 object 参数是 classinfo 参数或其(直接、间接或虚拟)子类的实例，则返回 True。

如果这对你来说太专业了，这里有一个更清晰的解释。假设您已经声明了一个函数，但不确定它将返回什么——例如，如果一切顺利，您需要一个浮点数，如果出错，您需要一个包含错误消息的字符串。`isinstance()`前来救援。

对于实用部分，我们将使用上面声明的类，并创建它的一个实例:

```
circle = Circle()
```

现在我们可以使用`isinstance()`来检查`circle`对象是否属于`Circle`类型:

```
if isinstance(circle, Circle):
   print('yes')**>>> yes**
```

这可能是有用的信息。请记住，您还可以检查某些内容是否是某种内置类型，如字符串、列表、元组等。

# 函数#2: hasattr()

这个也是很有用的。用一句话来说明它的用途，我认为如果对象**包含指定的属性**，那么`hasattr()`简单地返回 True，否则返回 False。

让我们来看看实际情况。我们已经声明了一个`Circle`类，并给了它一些属性— `radius`。我们现在可以使用`hasattr()`来检查属性是否存在于实例中。

代码如下:

```
circle = Circle()if hasattr(circle, 'radius'):
    print('yes')**>>> yes**
```

差不多就是这些了。这个函数的用例很大程度上取决于你所做的编程类型。作为一名数据科学家，我有时会在我的类中动态地创建属性，因此以某种方式检查属性是否存在是一件好事。

好吧，我们继续。

# 功能#3: exec()

如果您有 SQL 背景，那么您可能对**动态查询构建**的概念很熟悉。我们在 Python 中也有类似的东西——可能对大多数开发人员来说不太有用，但是知道有这种功能还是很好的。

与前两个函数不同，这里我们不需要我们的`Circle`类。我们将把两行 Python 代码放入一个多行字符串中，并对它们调用`exec`。

代码如下:

```
statement = '''a = 10; print(a + 5)'''
exec(statement)**>>> 15**
```

那么，这里刚刚发生了什么？

我们的语句作为 Python 代码逐行执行，尽管它实际上是一个字符串。

同样，您可能会发现这个函数不如前两个函数有用，这取决于您所做的工作类型。

# 结论

这就是你要的——一些我作为初学者不知道的功能，但是现在发现它们很有用。如果我早点知道它们，它们可能会节省我一些时间，或者至少让我的代码看起来更优雅，但事实就是这样。

感谢阅读，保重。

*喜欢这篇文章吗？成为* [*中等会员*](https://medium.com/@radecicdario/membership) *继续无限制学习。如果你使用下面的链接，我会收到你的一部分会员费，不需要你额外付费。*

[](https://medium.com/@radecicdario/membership) [## 通过我的推荐链接加入 Medium-Dario rade ci

### 作为一个媒体会员，你的会员费的一部分会给你阅读的作家，你可以完全接触到每一个故事…

medium.com](https://medium.com/@radecicdario/membership) 

# 参考资料:

[1][https://docs.python.org/3/library/functions.html#isinstance](https://docs.python.org/3/library/functions.html#isinstance)