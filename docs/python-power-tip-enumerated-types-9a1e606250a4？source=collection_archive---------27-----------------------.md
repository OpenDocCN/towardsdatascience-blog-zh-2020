# Python Power Tip:枚举类型

> 原文：<https://towardsdatascience.com/python-power-tip-enumerated-types-9a1e606250a4?source=collection_archive---------27----------------------->

## PYTHON 编程

## 表示有限选项集的正确方式

![](img/e688a660868a8cb6e686d46a4f59f7c4.png)

照片由[路易斯·汉瑟@shotsoflouis](https://unsplash.com/@louishansel?utm_source=medium&utm_medium=referral) 在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 拍摄

通常，程序员希望在他们的代码中表示一些东西，这些东西的值必须属于预先确定的有限的值集合。例子包括:

*   一周中的每一天:星期一，星期二，…，星期天。
*   图像文件格式的类型:JPEG，PNG，GIF，…
*   纸牌游戏中的花色:梅花、方块、红心、黑桃。
*   某[有限状态机](https://en.wikipedia.org/wiki/Finite-state_machine)的状态。

## 不令人满意的替代品

为了解决这样的情况，许多编程语言支持[枚举类型](https://en.wikipedia.org/wiki/Enumerated_type)。Python 直到 2014 年 3 月发布 3.4 版才支持枚举类型。在那之前，Python 程序员不得不求助于一些不太理想的策略。

常见的解决方法是用唯一的整数值定义一些全局变量，如下所示:

然后剩下的代码可以根据需要使用`CLUB`、`DIAMOND`、`HEART`或`SPADE`来表示一副牌。

这种方法存在一些问题:

*   这些全局变量是可变的。必须确保没有代码给它们赋予不同的值。
*   没有类型检查。很容易将其他整数值与一套牌混淆，从而导致错误。

## Python 最后添加了枚举类型

Python 的这一缺陷导致了各种第三方包的产生。在 2005 年有一个[被拒绝的提议](https://www.python.org/dev/peps/pep-0354/)将枚举类型添加到 Python 标准库中。然后，在 2013 年 1 月，语言开发人员之间的一次电子邮件对话再次引发了对标准支持的考虑。结果是 [PEP 435](https://www.python.org/dev/peps/pep-0435/) ，一个向标准库添加新`enum`模块的提议。Python 的创造者吉多·范·罗苏姆于 2013 年 5 月 10 日[批准了](https://mail.python.org/pipermail/python-dev/2013-May/126112.html) PEP 435。第二年`enum`模块首次出现在 Python 3.4 中。

## 定义典型的枚举类型

对于只需要一些可选值的简单情况，您可以遵循以下模式:

这就创建了一个枚举类型`Suit`，它定义了四种花色的符号，称为*成员*。可选的装饰器`@unique`增加了一个健全性检查，确保所有成员都有惟一的值。如果您不小心添加了多个具有相同值的符号，那么在执行类声明时会出现一个`ValueError`异常。例如，以下代码将失败:

运行此代码会产生以下行为:

```
Traceback (most recent call last):
  File "cards2.py", line 2, in <module>
    class Suit(Enum):
  File "/usr/lib/python3.5/enum.py", line 573, in unique
    (enumeration, alias_details))
ValueError: duplicate values found in <enum 'Suit'>: Diamond -> Club
```

此时，您可以很快发现问题所在，即`Diamond`和`Club`都具有相同的值:1。

## 使用枚举类型

像`Suit`这样的枚举类型的成员是常量。在修复了`Suit`中的唯一性问题之后，这里有一些你可以用 Python 解释器尝试的例子。首先，如果您评估其中一个成员，您会看到它的表示:

```
>>> **s = Suit.Spade**
>>> **s**
<Suit.Spade: 4>
```

每个枚举成员还有一个`name`属性，该属性产生字符串形式的名称:

```
>>> **s.name**
'Spade'
```

每个成员还有一个`value`属性，它返回您给它的任何值:

```
>>> **s.value**
4
```

`name`和`value`属性是只读的。这解决了我们上面提到的可变性问题:

```
>>> **s.value = 7**
Traceback (most recent call last):
  File "<stdin>", line 1, in <module>
  File "/usr/lib/python3.5/types.py", line 141, in __set__
    raise AttributeError("can't set attribute")
AttributeError: can't set attribute>>> **s.name = 'Joker'**
Traceback (most recent call last):
  File "<stdin>", line 1, in <module>
  File "/usr/lib/python3.5/types.py", line 141, in __set__
    raise AttributeError("can't set attribute")
AttributeError: can't set attribute>>> **Suit.Heart.value = 9**
Traceback (most recent call last):
  File "<stdin>", line 1, in <module>
  File "/usr/lib/python3.5/types.py", line 141, in __set__
    raise AttributeError("can't set attribute")
AttributeError: can't set attribute
```

枚举成员可以直接相互比较是否相等:

```
>>> **s == Suit.Spade**
True>>> **s == Suit.Heart**
False>>> **s != Suit.Heart**
True
```

这允许您编写代码来处理替代情况，如下所示:

枚举成员充当字典键:

```
>>> **color = { Suit.Heart: 'red', Suit.Diamond: 'red',**
... **Suit.Spade: 'black', Suit.Club: 'black' }**>>> **color[Suit.Club]**
'black'
```

## 类型转换

如果在字符串中有枚举成员的名称，可以使用带括号的类型名称来获取相应的枚举成员。

```
>>> **text = 'Heart'**
>>> **Suit[text]**
<Suit.Heart: 3>
```

类型名还充当一个函数，当给定一个初始值作为参数时，该函数返回相应的成员。

```
>>> **Suit(3)**
<Suit.Heart: 3>
```

## 枚举类型是可枚举的

仅仅从它们的名字来看，枚举类型应该提供枚举它们包含的成员的能力是有意义的。事实也的确如此。类型名充当其成员的枚举数:

```
>>> **for x in Suit:**
...     **print(x)**
... 
Suit.Club
Suit.Diamond
Suit.Heart
Suit.Spade>>> **list(Suit)**
[<Suit.Club: 1>, <Suit.Diamond: 2>, <Suit.Heart: 3>, <Suit.Spade: 4>]
```

## 温和类型检查

我上面提到的一个问题是，可能会混淆不同种类的枚举值。让我们添加另一个枚举类型，看看当我们试图比较它们时会发生什么。

尽管`Suit.Club`和`Animal.Dog`具有相同的关联值 1，但它们并不相等:

```
>>> **Suit.Club == Animal.Dog**
False
```

这是有道理的，因为`Suit`和`Animal`是不同的类型，一般来说不同的类型比较起来是不相等的:

```
>>> **Suit.Club == 1**
False>>> **Suit.Club == 'one'**
False
```

这有助于您避免编写代码混淆动物与卡片套装！

## 整数枚举

上面演示的`Enum`基类通常是创建枚举类型最有用的方法。但是，有时您希望枚举成员的行为类似于整数常量。在这种情况下，您可能不希望抽象类型成为表示特定整数常量的符号集合。也许您有一个外部 API，它将图像文件的格式报告为 1=PNG、2=JPEG 或 3=GIF。

在这种情况下，Python `enum`模块提供了另一个叫做`IntEnum`的基类。下面是如何定义一个不那么挑剔的枚举类型，其中成员的行为类似于整数常量:

现在成员`ImageFormat.JPEG`仍然拥有自己的枚举成员身份:

```
>>> **ImageFormat.JPEG**
<ImageFormat.JPEG: 2>
```

然而，当与整数 2 比较时，它匹配:

```
>>> **ImageFormat.JPEG == 2**
True
```

你甚至可以用它做数学，不像一个`Enum`的成员:

```
>>> **ImageFormat.JPEG + 1**
3>>> **Suit.Club + 1**
Traceback (most recent call last):
  File "<stdin>", line 1, in <module>
TypeError: unsupported operand type(s) for +: 'Suit' and 'int'
```

其他有用的`Enum`功能仍然有效，比如使用名称或值枚举和获取成员:

```
>>> **list(ImageFormat)**
[<ImageFormat.PNG: 1>, <ImageFormat.JPEG: 2>, <ImageFormat.GIF: 3>]>>> **ImageFormat(3)**
<ImageFormat.GIF: 3>>>> **ImageFormat['PNG']**
<ImageFormat.PNG: 1>
```

## 还有很多要学的

我们已经在这里介绍了 Python 枚举类型的基础知识，这足以让您入门。现在，您可以在代码中很好地使用枚举类型了。

然而，还有很多细节和细微差别。我鼓励你阅读下面链接的官方建议和文档页面。这两页都是深入理解这一重要语言特性的好读物。

## 参考

1.  [PEP 435:将 Enum 类型添加到 Python 标准库](https://www.python.org/dev/peps/pep-0435/):这是导致将`enum`模块添加到 Python 3.4+标准库的提议。
2.  [Python 官方文档为](https://docs.python.org/3/library/enum.html) `[enum](https://docs.python.org/3/library/enum.html)` [模块。](https://docs.python.org/3/library/enum.html)