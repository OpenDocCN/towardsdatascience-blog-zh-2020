# Python 是如何工作的？

> 原文：<https://towardsdatascience.com/how-does-python-work-6f21fd197888?source=collection_archive---------1----------------------->

## 简单解释 Python 代码的执行与旧的编程语言有何不同。

![](img/c684bb0d3a76417c1ba53f05d790962b.png)

图片来源—[https://encrypted-tbn0.gstatic.com/images?q = tbn:and 9 gcqaq 5 hozajawskwfboxnonyww-mg 4d XL 7 CWC-1 gufkyvvitnvh 8 sa&s](https://encrypted-tbn0.gstatic.com/images?q=tbn:ANd9GcQAQ5hOZAjAWsKwFbOXNONYWW-Mg4dxL7cWc-1gufkYvviTnvH8SA&s)

作为一名机器学习工程师，我使用 Python 已经一年多了。最近也开始学 C++，为了好玩。这让我意识到 Python 是多么的简单和直观。我对 Python 与其他语言的不同之处及其工作原理更加好奇。在这篇博客中，我试图了解 Python 的内部工作原理。

Python 最初是 Guido Van Rossum 的爱好项目，于 1991 年首次发布。作为一种通用语言，Python 正在为网飞和 Instagram 等许多公司提供支持。在[的一次采访](https://youtu.be/7kn7NtlV6g0?t=910)中，Guido 将 Python 与 Java 或 Swift 等语言进行了比较，并表示后两者是软件开发人员的绝佳选择——他们的日常工作是编程，但 Python 是为那些日常工作与软件开发无关，但他们主要是为了处理数据而编码的人而设计的。

当你阅读 Python 的时候，你经常会遇到这样的词——*编译* *vs* *解释，字节码 vs 机器码，动态类型 vs 静态类型，垃圾收集器等等。*维基百科将 Python 描述为

> **Python** 是一种[解释的](https://en.wikipedia.org/wiki/Interpreted_language)、[高级](https://en.wikipedia.org/wiki/High-level_programming_language)、[通用](https://en.wikipedia.org/wiki/General-purpose_programming_language)、[编程语言](https://en.wikipedia.org/wiki/Programming_language)。它是[动态类型化的](https://en.wikipedia.org/wiki/Dynamic_programming_language)和[垃圾收集的](https://en.wikipedia.org/wiki/Garbage_collection_(computer_science))。

## 解释语言

当你用 C/C++写程序时，你必须编译它。编译包括将你的人类可理解的代码翻译成机器可理解的代码，或*机器代码。*机器码是可由 CPU 直接执行的指令的基础级形式。成功编译后，您的代码会生成一个可执行文件。执行该文件会逐步运行代码中的操作。

在很大程度上，Python 是一种解释型语言，而不是编译型语言，尽管编译是一个步骤。Python 代码，用**写的。py** 文件首先被编译成所谓的*字节码*(将进一步详细讨论)，它与**一起存储。pyc** 或**。pyo** 格式。

它不是像 C++那样把源代码翻译成机器码，而是把 Python 代码翻译成字节码。这个字节码是一组低级指令，可以由一个**解释器**执行。在大多数 PC 中，Python 解释器安装在*/usr/local/bin/Python 3.8 .*不是在 CPU 上执行指令，而是在虚拟机上执行字节码指令。

## 为什么解释？

解释语言的一个普遍优点是它们是独立于平台的。只要 Python 字节码和虚拟机的版本相同，Python 字节码就可以在任何平台(Windows、MacOS 等)上执行。

动态类型是另一个优势。在像 C++这样的静态类型语言中，您必须声明变量类型，任何差异，比如添加一个字符串和一个整数，都会在编译时被检查。在像 Python 这样的强类型语言中，检查变量类型和执行的操作的有效性是解释器的工作。

## 解释语言的缺点

动态类型提供了很大的自由度，但同时也使代码有风险，有时很难调试。

Python 经常被指责‘慢’。虽然这个术语是相对的，并且有很多争议，但是速度慢的原因是因为解释器必须做额外的工作，将字节码指令翻译成可以在机器上执行的形式。一篇 [StackOverflow](https://stackoverflow.com/questions/1694402/why-are-interpreted-languages-slow) 的帖子用一个类比更容易理解—

> 如果你可以用你的母语和别人交谈，这通常比让翻译把你的语言翻译成其他语言让听者理解要快得多。

## 垃圾回收到底是什么？

在早期的编程语言中，内存分配是相当手工的。很多时候，当你在程序中使用不再使用或不再被引用的变量时，它们需要从内存中清除。垃圾收集器会帮你做这件事。它自动释放空间，无需你做任何事情。内存管理有两种方式——

简单地说，它跟踪对一个对象的引用次数。当这个数字下降到零时，它会删除该对象。这被称为**引用计数**。这在 Python 中是不能禁用的。

在对象引用自身或者两个对象相互引用的情况下，一个叫做[的代垃圾收集](https://rushter.com/blog/python-garbage-collector/)的过程会有所帮助。这是传统的引用计数无法做到的。

## 什么是 __pycache__？

在您的个人项目或 GitHub 上，您可能会多次看到名为 __pycache__ 的文件夹被自动创建。

```
/folder - __pycache__ - preprocess.cpython-36.pyc - preprocess.py
```

如您所见，该文件名与 __pycache__ 文件夹外的文件名相同。的。pyc 扩展告诉我们文件包含 preprocess.py 的字节码。名字 **cpython** 表示解释器的类型。CPython 意味着解释器是用 C 语言实现的。类似地，JPython 是用 Java 实现的 Python 解释器。

但是为什么首先要创建文件夹呢？嗯，它稍微提高了 Python 程序的速度。除非您更改 Python 代码，否则可以避免重新编译成字节码，从而节省时间。

我已经开始了我的个人博客，我不打算在媒体上写更多令人惊叹的文章。通过订阅 [thenlp.space](https://thenlp.space/) 支持我的博客