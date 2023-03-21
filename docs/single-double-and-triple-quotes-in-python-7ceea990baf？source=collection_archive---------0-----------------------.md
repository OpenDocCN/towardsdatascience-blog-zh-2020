# Python 中的单引号、双引号和三引号

> 原文：<https://towardsdatascience.com/single-double-and-triple-quotes-in-python-7ceea990baf?source=collection_archive---------0----------------------->

![](img/3f853a3582a95f6425a8d91742a632e4.png)

扬·阿莱格在 [Unsplash](https://unsplash.com/s/photos/triple?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) 上的照片

## 有选择总是好的——单引号和双引号在 Python 中可以互换使用。

我们所有的 Python 程序员都知道，在 Python 中，与字符串声明相关的单引号和双引号的使用。然而，并不是所有人都知道三重引号的某些用法。

这篇简短的文章回顾了 Python 中单引号、双引号和三引号的用法。

# 单引号和双引号

## 基本用法

单引号和双引号最常见的用法是通过包含一系列字符来表示字符串。如下面的代码所示，我们分别使用单引号和双引号创建这两个字符串。

```
>>> quotes_single = 'a_string'
>>> quotes_double = "a_string"
>>> quotes_single == quotes_double
True
```

正如您所注意到的，使用单引号和双引号创建的字符串是相同的。换句话说，当我们声明一个字符串时，我们可以交替使用单引号和双引号。然而，应该注意的是，我们不想把它们混在一起，因为这是一个语法错误。

```
>>> "mixed quotes'
  File "<stdin>", line 1
    "mixed quotes'
                 ^
SyntaxError: EOL while scanning string literal
>>> 'mixed quotes"
  File "<stdin>", line 1
    'mixed quotes"
                 ^
SyntaxError: EOL while scanning string literal
```

## 逃避行为

像其他编程语言一样，当字符串包含引号这样的特殊字符时，我们需要对它们进行转义。下面是一个逃跑失败的例子。

```
>>> 'It's a bad example.'
  File "<stdin>", line 1
    'It's a bad example.'
        ^
SyntaxError: invalid syntax
```

我们如何修复这个错误？一种是通过在单引号前放置一个反斜杠来转义单引号。另一种是使用双引号而不是单引号作为括住的引号。两种方式如下所示。

```
>>> 'It\'s a good example.'
"It's a good example."
>>> "It's a good example."
"It's a good example."
```

类似地，如果字符串包含双引号，我们可以使用单引号来表示字符串，这样我们就不必对双引号进行转义。下面给出一个例子。

```
>>> 'She said, "Thank you!"'
'She said, "Thank you!"'
```

然而，如果字符串中既有单引号又有双引号，如果您没有对与整个字符串所使用的引号相同的引号进行转义，就会出现语法错误。

```
>>> print('She said, "Thank you! It's mine."')
  File "<stdin>", line 1
    print('She said, "Thank you! It's mine."')
                                    ^
SyntaxError: invalid syntax
>>> print('She said, "Thank you! It\'s mine."')
She said, "Thank you! It's mine."
```

# 三重引号

## 包含单引号和双引号的封闭字符串

正如上一节末尾提到的，我们需要根据字符串使用的引号来转义单引号或双引号。实际上，我们可以使用三重引号(即三重单引号或三重双引号)来表示既包含单引号又包含双引号的字符串，以消除对任何。

```
>>> print('''She said, "Thank you! It's mine."''')
She said, "Thank you! It's mine."
```

需要注意的是**当一个字符串以单引号或双引号开始或结束，并且我们想要对这个字符串使用三重引号时，我们需要使用与开始或结束引号不同的引号。**例如，对于上面代码片段中的字符串，使用三重双引号会导致语法错误。在这种情况下，我们想要使用上面的三个单引号。

```
>>> print("""She said, "Thank you! It's mine."""")
  File "<stdin>", line 1
    print("""She said, "Thank you! It's mine."""")
                                                 ^
SyntaxError: EOL while scanning string literal
```

## 多行字符串

三重引号的另一个用例是表示多行字符串。下面给出一个例子。在这种情况下，您可以使用三重单引号或双引号。

```
>>> print("""Hello
... World
... !""")
Hello
World
!
```

虽然我们可以通过使用下面的`\n`符号创建多行字符串来达到相同的效果，但是使用`\n`符号会使字符串更难阅读。相比之下，使用三重引号可以准确地写出字符串，因此可读性更好。

```
>>> print('Hello\nWorld\n!')
Hello
World
!
```

此外，用三重引号括起来的字符串的一个有用的应用是在多行字符串中指定一些注释，例如，作为函数定义的一部分，如下所示。

```
>>> def multiple_line_comment(a, b):
...     '''
...     a is a string # other additional description
...     b is a list of integers # other additional description
...     '''
...     pass
... 
>>> print(multiple_line_comment.__doc__)

    a is a string # other additional description
    b is a list of integers # other additional description
```

我们可以清楚地知道函数的注释是什么。

# 结论

本文回顾了单引号、双引号和三引号在 Python 中的常见用法。下面是这些用例的快速总结。

## 单引号和双引号

*   括起字符串。单引号或双引号都可以。
*   使用单引号作为括起来的引号可以消除字符串中转义双引号的需要，反之亦然。

## 三重引号

*   将包含单引号和双引号的字符串括起来，这样就不需要转义。
*   括起多行字符串。

感谢您阅读本文，祝您用 Python 愉快地编码。