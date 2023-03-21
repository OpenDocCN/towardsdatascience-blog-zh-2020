# 如何以及为什么在 Python3 中使用 f 字符串

> 原文：<https://towardsdatascience.com/how-and-why-to-use-f-strings-in-python3-adbba724b251?source=collection_archive---------16----------------------->

![](img/97774bb63ee133f9f51ffbc3a05eaaa1.png)

图片由[s·赫尔曼&f·里希特](https://pixabay.com/users/pixel2013-2364555/?utm_source=link-attribution&utm_medium=referral&utm_campaign=image&utm_content=5009533)从[皮克斯拜](https://pixabay.com/?utm_source=link-attribution&utm_medium=referral&utm_campaign=image&utm_content=5009533)拍摄

## [蟒蛇短裤](https://towardsdatascience.com/tagged/python-shorts)

## 使用 Python 3 中新功能的简单指南

[Python](https://amzn.to/2XPSiiG) 为我们提供了多种风格的编码。

随着时间的推移，Python 定期推出新的编码标准和工具，这些标准和工具更加符合 Python 禅宗的编码标准。

> 漂亮总比难看好。

在这一系列名为 [**Python Shorts**](https://towardsdatascience.com/tagged/python-shorts) 的帖子中，我将解释 Python 提供的一些简单但非常有用的构造，一些基本的技巧，以及我在数据科学工作中经常遇到的一些用例。

这个帖子是专门关于在 Python 3.6 ***中介绍的在 Python*** *中使用 f 字符串的 ***。****

# 3 种常见的打印方式:

让我用一个简单的例子来解释一下。假设你有一些变量，你想在一个语句中打印它们。

```
name = 'Andy'
age = 20
print(?)
----------------------------------------------------------------
Output: I am Andy. I am 20 years old
```

您可以通过多种方式做到这一点:

一个非常简单的方法是在打印函数中使用`+`进行连接。但这很笨拙。我们需要将数字变量转换成字符串，并在连接时注意空格。它看起来并不好，因为当我们使用它时，代码的可读性受到了一点影响。

```
name = 'Andy'
age = 20
print("I am " + name + ". I am " + str(age) + " years old")
----------------------------------------------------------------
I am Andy. I am 20 years old
```

![](img/1c2dae6e943276e23642aef3c378df24.png)

来源: [Pixabay](https://pixabay.com/photos/art-art-supplies-artist-blue-brush-1478831/)

**b) %格式:**第二个选项是使用`%`格式。但它也有自己的问题。首先，它不可读。您需要查看第一个`%s`，并尝试在列表末尾找到相应的变量。想象一下，如果你想打印一个很长的变量列表。

```
print("I am %s. I am %s years old" % (name, age))
```

**c) str.format():使用`str.format()`**

```
print("I am {}. I am {} years old".format(name, age))
```

这里我们用{}来表示列表中对象的占位符。它仍然有同样的可读性问题，但是我们也可以使用`str.format`:

```
print("I am {name}. I am {age} years old".format(name = name, age = age))
```

如果这看起来有点重复，我们也可以使用字典:

```
data = {'name':'Andy','age':20}
print("I am {name}. I am {age} years old".format(**data))
```

# 第四种方式用 f

![](img/279cd4912ed58e9f028b4e78ece6bc55.png)

[威廉-简·豪斯曼](https://unsplash.com/@willemjanhuisman?utm_source=medium&utm_medium=referral)在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上的照片

从 Python 3.6 开始，我们有了一个新的格式化选项，这使得它变得更加简单。我们可以简单地使用:

```
print(f"I am {name}. I am {age} years old")
```

我们只需在字符串的开头添加`f`,并使用{}来包含我们的变量名，就可以得到所需的结果。

f string 提供的一个附加功能是**我们可以将表达式**放在{}括号中。例如:

```
num1 = 4
num2 = 5
print(f"The sum of {num1} and {num2} is {num1+num2}.")
---------------------------------------------------------------
The sum of 4 and 5 is 9.
```

这非常有用，因为您可以在这些括号中使用任何类型的表达式。 ***表达式可以包含字典或函数。*** 一个简单的例子:

```
def totalFruits(apples,oranges):
    return apples+orangesdata = {'name':'Andy','age':20}apples = 20
oranges = 30print(f"{data['name']} has {totalFruits(apples,oranges)} fruits")
----------------------------------------------------------------
Andy has 50 fruits
```

同样，你可以使用`’’’`来使用 ***多行字符串*** 。

```
num1 = 4
num2 = 5
print(f'''The sum of 
{num1} and 
{num2} is 
{num1+num2}.''')---------------------------------------------------------------
The sum of 
4 and 
5 is 
9.
```

格式化字符串的一个日常用例是**格式化浮点**。您可以使用 f 字符串来实现，如下所示

```
numFloat = 10.23456678
print(f'Printing Float with 2 decimals: {numFloat:.2f}')-----------------------------------------------------------------
Printing Float with 2 decimals: 10.23
```

# 结论

直到最近，我一直使用 Python 2 来完成我的所有工作，所以无法了解这个新特性。

但是现在，随着我转向 Python 3，f strings 已经成为我格式化字符串的首选语法。它很容易编写和阅读，并且能够合并任意表达式。在某种程度上，这个新功能至少符合 3 个 PEP 概念

> 漂亮比难看好，简单比复杂好，可读性很重要。

如果你想了解更多关于 [Python](https://amzn.to/2XPSiiG) 3 的知识，我想向你推荐一门来自密歇根大学的关于学习 [**中级 Python**](https://bit.ly/2XshreA) 的优秀课程。一定要去看看。

我以后也会写更多这样的帖子。让我知道你对这个系列的看法。在[](https://medium.com/@rahul_agarwal)**关注我或者订阅我的 [**博客**](http://eepurl.com/dbQnuX) 了解他们。一如既往，我欢迎反馈和建设性的批评，可以通过 Twitter [@mlwhiz](https://twitter.com/MLWhiz) 联系。**

**此外，一个小小的免责声明——这篇文章中可能会有一些相关资源的附属链接，因为分享知识从来都不是一个坏主意。**