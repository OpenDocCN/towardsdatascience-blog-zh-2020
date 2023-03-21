# 立即开始使用 Python 的 Unittest 模块

> 原文：<https://towardsdatascience.com/start-using-pythons-unittest-module-today-17ba0537c5a0?source=collection_archive---------62----------------------->

## Python 单元测试完整演练

![](img/bcbbc69668969e0a0cba428d102fc570.png)

乔恩·泰森在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上的照片

# 单元测试

单元测试是代码的基本运行。这是**不是 QA。这种**方式，你**验证你已经写了质量代码**，而不是在别人试图使用它的时候就会崩溃的东西。

## 为什么要测试？

最近，在一家初创公司，我需要为一位同事编写库代码，我给他库代码和一个单元测试脚本。

每次他抱怨的时候，单元测试**为我节省了时间**来检查他代码中的错误。因为我的工作了，这节省了很多时间，时间是你最宝贵的财富。

![](img/c9bcbb99f6719a5a3343d50f63645e43.png)

照片由 [insung yoon](https://unsplash.com/@insungyoon?utm_source=medium&utm_medium=referral) 在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄

单元测试是开发的重要方面**。没有你的代码的第一次运行，你怎么知道它是否工作？由于单元测试通常很快很短，通过正确地管理单元测试，您可以看到您是否破坏了旧代码或其他人的代码！**

提供如何使用你的代码的例子将会很容易，因为你已经有一个了！

## 错误而常见的方式

让我们从展示错误的和常规的方法开始。这里有一个简单的类对象:

如你所见，这个类做的不多。它接受一个数字，并对给定的输入进行幂运算。现在，一个标准的开发人员会如何验证这些代码呢？

![](img/0f3689048cf391637fba309943f5ca48.png)

胡安·鲁米普努在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄的照片

## 这个方法好像不太对；以下是几个主要原因。

*   这是**不可持续的**——只是分散在许多文件中的小测试，没有任何方法来跟踪甚至重用这些测试。
*   这很混乱，并且给我们的程序增加了未使用的代码块。
*   它确保单元测试只发生一次，**而不是在**发布的每个新版本中**验证。**

你可能会说——他可以删除它，但是他所有的单元测试工作都是徒劳的，你不会在每个版本中重用它，来改进你未来的代码，或者把你的工作片段给其他用户。

## 正确的方法

![](img/87e44e66d0b89a315ff9d790bb5fc9e2.png)

由 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上 [Krisjanis Mezulis](https://unsplash.com/@krisijanis?utm_source=medium&utm_medium=referral) 拍摄的照片

有许多 python 测试模块，你可以为它们中的许多给出有力的论据，我喜欢 **python unittest** 有许多原因，它的**简单**、**快速**、**有效**，并且在**标准库中！**

## 让它工作！

*   测试类必须**从 unittest 继承。测试案例**
*   每个测试**功能**必须在**测试 _** 中开始
*   **unittest.main()** 是运行**来测试结果的函数。**

> 三个小小的规则，你就有了一个合适的单元测试，很简单，不是吗？

## 选项

![](img/ae37aa49d5e0cf806bcb99938df2c081.png)

维多利亚诺·伊斯基耶多在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄的照片

不仅仅是断言相等。您可以检查任何您喜欢的东西，从数字到类型，提出正确的异常，任何您可以想象的东西，都在一个简单的标准库模块中，由 Python 开发团队维护。

# 最后的想法

对于每个开发人员来说，单元测试是一个必不可少的工具，Python 单元测试是一个很好的模块。

适当的单元测试将在许多方面节省你的时间，包括未来的错误，破坏同事的代码，甚至是向他人展示他们如何使用你的系统的伟大片段，这种方式增加了你编写的程序实用和有益的几率。

如果你喜欢这些文章，请发表评论，关于 Python 标准库的更多内容，请就你关心的主题发表评论，我会写下来。

我希望你喜欢它！