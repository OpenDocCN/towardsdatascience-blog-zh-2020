# 7 枚蟒蛇皮复活节彩蛋

> 原文：<https://towardsdatascience.com/7-easter-eggs-in-python-7765dc15a203?source=collection_archive---------7----------------------->

## 在家自娱自乐的无数方式

![](img/eb4045a731a5f047dbe6c5f51c51b482.png)

[S & B 冯兰森](https://unsplash.com/@blavon?utm_source=medium&utm_medium=referral)在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上的照片

在冠状病毒爆发期间，我们大多数人都在家工作。你们中的许多人可能已经厌倦了整天呆在家里。我能感受到你。

Python 可能只是你构建项目、模拟和自动化的一个工具，它可能真的很有趣。

感谢令人惊叹的 Python 社区，我们可以在这种开源语言中找到许多隐藏的功能和复活节彩蛋。以下是其中的 7 个。

# 1.最简单的“Hello World”

学习编程语言要做的第一件事就是在屏幕上打印出“Hello World”。用 Python 怎么做 Hello World？`print('Hello World!')`？

原来 Python 开发者藏了一个模块，只要导入这个模块就可以做 Hello World！试试这个:

```
>>> import __hello__
Hello World!
```

那行代码只包含 16 个字符，包括空格键！这可以说是最简单的 Hello World 程序之一。

请注意，您不能在 Python 程序中重新导入模块，因此您可以在一次运行中打印一次消息。但我想这可能意味着一些深刻的东西…

# 2.Python 的禅

这是人教版 20 中提出的。PEP 代表 Python 增强提案。

Python 是你最喜欢的语言吗？你喜欢 Python 的什么？是设计的问题吗？Python 设计的指导原则可以用 20 条格言来描述，您可以通过以下方式找到其中的 19 条:

```
>>> import this
```

![](img/505f4c45519bf552215a58ce7e64aa50.png)

安妮·斯普拉特在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上的照片

失踪的那个呢？我想你不可能知道世界上的一切。以下是前三句格言:

```
Beautiful is better than ugly.
Explicit is better than implicit.
Simple is better than complex.
```

你不得不感谢开发人员为制作这样一种优雅且人类可读的编程语言所付出的努力。

这也是迄今为止唯一一个在 [Python 开发者指南](https://www.python.org/dev/peps/pep-0020/)中被称为“复活节彩蛋”的“官方”复活节彩蛋。

# 3.this.py

还记得 Python 之禅的前 3 行吗？

当你深入挖掘，找到模块文件`this.py`时，它是我见过的最漂亮、最显式、最简单的代码。

点击查看文件[。干得好。](https://github.com/python/cpython/blob/master/Lib/this.py)

# 4.体验反重力

你可以用 Python 中的一行代码体验反重力！

```
import antigravity
```

真的！试试吧！请记住，你可能被困在外太空，花几个小时浏览 xkcd 网络漫画。

![](img/b3cadae81dc4ce5ada8c84e124b46085.png)

[freestocks](https://unsplash.com/@freestocks?utm_source=medium&utm_medium=referral) 在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄的照片

# 5.吊带

与许多其他编程语言不同，Python 在构造语句、函数和循环时并不真正使用花括号`{}`。但是他们将来可能会改变。

`__future__`模块包含不兼容的变更，这些变更将在可预见的将来强制执行。例如，在 Python 2.6 或 2.7 中从`__future__`导入`print_function`允许你创建一个带参数`print()`的函数，就像在 Python 3 中一样。

让我们从`__future__`开始看看牙套是如何工作的:

```
>>> from __future__ import braces
SyntaxError: not a chance
```

打得好。

# 6.混杂

无穷大和 NaN 的散列。

```
>>> hash(float('inf'))
314159
>>> hash(float('nan'))
0
```

不知怎么的，我在 Reddit 上找到了这个复活节彩蛋。

![](img/c100433c10669b89ab57bffe1c49f079.png)

照片由[比·费尔滕-莱德尔](https://unsplash.com/@marigard?utm_source=medium&utm_medium=referral)在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄

老实说，我期待着生命、宇宙和一切的答案。

# 7.巴里叔叔

关于运营商选择的争论已经持续了 [*四两*](https://www.urbandictionary.com/define.php?term=42) 之久。让我们解决这个问题。

这是人教版 **401** 中提出的。你可能会感觉到将要发生什么。

一位知名的 Python 开发者 Barry Warsaw(又名 Barry 大叔)被‘选中’成为终身友好的语言大叔，简称 FLUFL。伟大的缩写。

他做了一些“修改”，用菱形算子`<>`替换不等式算子`!=`。

如果你同意 Barry 叔叔的观点，你可以导入这个有趣的库，`<>`语法将变得有效，而`!=`将导致语法错误。

```
>>> from __future__ import barry_as_FLUFL>>> 0 != 1
SyntaxError: with Barry as BDFL, use '<>' instead of '!='>>> 0 <> 1
True
>>> 1 <> 1
False
```

愚人节快乐！可怜的巴里……我向你保证上面的例子没有打字错误。

![](img/27396b09957c290e2b9ad61f46de32d0.png)

照片由 [Agnes Horváth](https://unsplash.com/@magneticart?utm_source=medium&utm_medium=referral) 在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄

这个列表还可以继续下去，但是我会在这里停下来，让您自己去探索它们。

感谢阅读！如果您想收到我的新文章的更新，您可以使用下面的滑块订阅我的时事通讯:

如果你还在读这篇文章，你可能对 Python 感兴趣。以下文章可能有用:

[](/5-python-features-i-wish-i-had-known-earlier-bc16e4a13bf4) [## 我希望我能早点知道的 5 个 Python 特性

### 超越 lambda、map 和 filter 的 Python 技巧

towardsdatascience.com](/5-python-features-i-wish-i-had-known-earlier-bc16e4a13bf4) [](/4-common-mistakes-python-beginners-should-avoid-89bcebd2c628) [## Python 初学者应该避免的 4 个常见错误

### 我很艰难地学会了，但你不需要

towardsdatascience.com](/4-common-mistakes-python-beginners-should-avoid-89bcebd2c628) [](https://medium.com/better-programming/stop-abusing-args-and-kwargs-in-python-560ce6645e14) [## 停止在 Python 中滥用*args 和**kwargs

### 他们会回来缠着你

medium.com](https://medium.com/better-programming/stop-abusing-args-and-kwargs-in-python-560ce6645e14) 

注意安全。