# 小行星计划(第二部分)——测试驱动开发

> 原文：<https://towardsdatascience.com/asteroid-project-part-2-test-driven-development-ed7af6c1820e?source=collection_archive---------38----------------------->

## [用 Python 进行空间科学](https://towardsdatascience.com/tagged/space-science-with-python)

## [系列教程的第 22 部分](https://towardsdatascience.com/tagged/space-science-with-python)继续我们科学项目的第二部分。在我们深入研究 Python 和近地对象之前，让我们看一下测试驱动开发

![](img/0f207eb34e3b3546c407abbd2974f9be.png)

照片由[马库斯·斯皮斯克](https://unsplash.com/@markusspiske?utm_source=medium&utm_medium=referral)在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 拍摄

# 前言

*这是我的 Python 教程系列“用 Python 进行空间科学”的第 22 部分。教程会话中显示的所有代码都上传到*[*GitHub*](https://github.com/ThomasAlbin/SpaceScienceTutorial)*上。尽情享受吧！*

# 介绍

不，我们还没有开始任何近地天体(NEO)相关的 Python 开发或实现。在砍树之前，我们需要先把斧子磨快。在我们的例子中:让我们深入一些开发概念，这些概念将导致一个可持续的长期项目和 Python 库。

*你是怎么编码的？*大多数开发人员(无论是业余程序员还是专业人士)都喜欢看到快速的结果；他们开发一个原型，一些可点击的界面，或者演示页面来实现第一个产品里程碑。在专业工作环境中，这也可能是由外部因素造成的，如截止日期、产品所有者、业务合作伙伴或期望在特定时间框架内获得结果的客户。由于专业背景和知识的不同，有些人可能会互相扯皮。

*结果呢？*例如:[意大利面条代码](https://en.wikipedia.org/wiki/Spaghetti_code)测试不够好，难以维护、升级和理解！

在我们的科学项目中，我们希望开发一个可靠且可持续的 Python 解决方案。从事近地天体研究的业余或专业天文学家可以使用的工具。开发人员和编码人员也可以使用它来创建可以与库合并的新功能。

但是我们如何确保(从一开始)我们即将到来的库的可持续性和可维护性呢？嗯，除了适当的项目计划和项目结构( [PEP8 格式化](https://www.python.org/dev/peps/pep-0008/)、 [PEP257](https://www.python.org/dev/peps/pep-0257/) 和 [Numpy Docstrings](https://numpydoc.readthedocs.io/en/latest/format.html) )之外，敏捷工作的世界中还有一个概念，它看起来很乏味，有些无聊……:测试驱动开发(TDD)。

# 测试驱动开发

TDD 逆转了“经典编程方法”。先编码后测试的范例变为**先测试后编码**。这些规则看似简单，却令人困惑:

1.  **为一个类/函数编写一个单独的(单元)测试/ …**
2.  **编写产品代码以成功通过失败的单元测试**
3.  **在所有单元测试成功之前，不要继续其他功能**

这些都是苛刻的规定。首先，我需要为一个不存在的函数定义一个单元测试。这个测试显然失败了。现在我开发一些代码来成功完成单元测试。当测试通过后，另一个单元测试被添加，最终对同一功能再次证明其健壮性或泛型实现。它一次又一次地成功了吗？很好！您的功能似乎适合以后使用。如果没有，重新编码，直到所有测试都通过。

如果单个类或函数的所有单元测试都通过了，那么为编码项目的下一个元素定义并编写新的单元测试。

这种方法听起来可能很无聊。一个人想要在短时间内成就大事！为什么要用这种乏味的方法呢？开拓精神在哪里？即使你在做一个真正创新的产品或想法，你也需要基本的功能；完全独立于你想要达到的目标。测试第一确保你的工作有一个坚实的基础可以依靠。从长远来看，一个有几个初始错误的代码维护起来会更费时，例如，如果你的科学工作依赖于它…嗯…在最坏的情况下，你不得不撤回你的分析或数值模拟。

## TDD 模式

![](img/82f9c1c469e41fc18a543cd64110955a.png)

凯文·Ku 在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄的照片

理论上，TDD 有三种工作/编码模式(这些模式很容易合并成另一种)。让我们假设您基于产品的需求创建了一个单元测试，并且您需要开发相应的产品代码:

*   *明显实现*

*显而易见的实现*是最琐碎的模式。基于一个单元测试的例子，你可以清楚地得到一个通用的函数来解决问题。示例:存储为字符串的数值需要转换为实际的浮点数。您马上就会发现，通过应用 float('23.15 ')，可以将' 23.15 '转换为 23.15。

*   *假的吧*

一个单元测试需要通过！你不知道如何做，所以你只是伪造结果！您设置一些常量，并返回断言测试所期望的硬编码答案。当然你*造假*(结果)。然而，基于这个虚假的常数，你试图回溯到实际的输入。最终，经过一些思考和研究，你会发现*明显的实现*在开始时并不明显。

*   *三角测量*

*Fake it* 方法假设基于伪造常数和单个测试，开发人员将最终确定单元测试问题的通用解决方案。然而，如果一个问题太复杂，就需要更多的测试来覆盖更多的情况。越来越多的硬编码解决方案有助于*三角测量*特定问题的实际通用解决方案。假设您有从网站下载文件的任务。单元测试调用了一个函数，该函数需要返回一个类似“下载成功”的消息，但是您不知道从哪里开始。此时你可以伪造结果。创建一个包含下载功能的函数，创建一个带有条目“Download succeeded”的字符串并返回它。之后，添加另一个单元测试，例如下载的静态文件的散列值。同样，您通过伪造模拟文件的结果哈希值来再次伪造结果。越来越多的测试被添加进来，要求你检查服务器的响应等等。随着更多的测试和更多需要的功能，你开发的代码需要满足所有的长期需求。虚假的解决方案将转变为通用的解决方案。

## 重构和文档

最后，如果测试通过，考虑重构你的代码。在 Python 中，你应该遵循 PEP8 标准，确保高水平的代码样式…

[](https://www.python.org/dev/peps/pep-0008/) [## PEP 8 风格的 Python 代码指南

### Python 编程语言的官方主页

www.python.org](https://www.python.org/dev/peps/pep-0008/) 

…提供注释以解释您的代码正在做什么，并提供 PEP257 风格(和/或 Numpy Doc 风格或您认为合适的其他风格)的文档:

[](https://www.python.org/dev/peps/pep-0257/) [## PEP 257 -文档字符串约定

### 这个 PEP 的目的是标准化文档字符串的高层结构:它们应该包含什么，以及如何表达…

www.python.org](https://www.python.org/dev/peps/pep-0257/)  [## numpydoc 文档字符串指南- numpydoc v1.2.dev0 手册

### 本文描述了用于 Sphinx 的 numpydoc 扩展的文档字符串的语法和最佳实践。注意…

numpydoc.readthedocs.io](https://numpydoc.readthedocs.io/en/latest/format.html) 

重构和记录你的代码也会帮助你理解你自己的代码。也许有些部分包含单元测试没有涵盖的逻辑错误？也许你错过了一个重要的特征？编码、重构和文档化齐头并进。编程不应该是一个问题的纯粹抽象，还应该是人类可读的，以确保长期的可维护性和可持续性。

此外，剖析您的函数可以帮助您提高产品的性能。

# 结论与展望

今天我们讨论了测试驱动开发的一些基本原则，简称 TDD。TDD 将帮助我们为我们的空间科学近地天体项目开发一个可持续的 Python 库，提供经过测试的准确结果。在我们深入这个项目的科学部分之前，下次我们将看一个 TDD 的例子。在那里，我们将一步一步地完成提到的 TDD 过程，因为在项目过程中，你只会看到一些编码会话的结果和成功的单元测试(也许一个额外的视频博客会对一些感兴趣的读者有用？).

如果你纠结于编码，如果你迷失在自己的开发过程、对象和函数中，记得阅读 Python 的 *Zen:*

```
The Zen of Python, by Tim PetersBeautiful is better than ugly.
Explicit is better than implicit.
Simple is better than complex.
Complex is better than complicated.
Flat is better than nested.
Sparse is better than dense.
Readability counts.
Special cases aren’t special enough to break the rules.
Although practicality beats purity.
Errors should never pass silently.
Unless explicitly silenced.
In the face of ambiguity, refuse the temptation to guess.
There should be one — and preferably only one — obvious way to do it.
Although that way may not be obvious at first unless you’re Dutch.
Now is better than never.
Although never is often better than *right* now.
If the implementation is hard to explain, it’s a bad idea.
If the implementation is easy to explain, it may be a good idea.
Namespaces are one honking great idea — let’s do more of those!
```

托马斯

# 文献和参考资料

[](https://www.springer.com/de/book/9783642042874) [## 测试驱动的开发——敏捷实践的实证评估

### 敏捷方法在工业界和研究界正获得越来越多的关注。许多行业正在转型…

www.springer.com](https://www.springer.com/de/book/9783642042874)