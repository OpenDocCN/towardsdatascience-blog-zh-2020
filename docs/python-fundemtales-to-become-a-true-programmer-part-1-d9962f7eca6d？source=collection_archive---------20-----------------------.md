# 成为真正程序员的 Python 基础，第 1 部分

> 原文：<https://towardsdatascience.com/python-fundemtales-to-become-a-true-programmer-part-1-d9962f7eca6d?source=collection_archive---------20----------------------->

## 您必须知道的事情-提高您的 Python 技能

![](img/fb6c9c0791a642e39f2f38a26fd63fc3.png)

图像[来源](https://www.everypixel.com/image-413910224566160174)

# Python 路径

Python 有这种魅力。人们近乎浪漫地谈论它，说它是“简单的英语”。它是如此“简单易懂”,以及引入开源社区奇妙模块的力量——一切尽在掌握之中。

## 嗯，难道不是吗？

这是毫无疑问的，但我的朋友是第一级。

第一级是每个 python 开发者成为真正的 python 开发者的必经之路，我希望在这篇文章结束时，**带你进入更高级的层次，并向你展示你必须走的路。**

![](img/62b9368e2bf12d82d2ce3043bf868892.png)

照片由[大卫·马尔库](https://unsplash.com/@davidmarcu?utm_source=medium&utm_medium=referral)在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄

> "知道特征并不能让一个人成为专家，理解特征才能做到."

## 起初

这篇文章将随着时间的推移而进步，但是我想从更好地理解类开始。

类被认为是一个对象的蓝图，init 创建这个对象，或者说构造它(很多构建的类比参与进来)。

这个定义是公平的，但是它遗漏了 python 类的一个重要方面。

## 这个物体是什么？

![](img/873df9f12e49a38b649c0859daf5449b.png)

照片由[元素 5 数码](https://unsplash.com/@element5digital?utm_source=medium&utm_medium=referral)在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄

您创建的这个对象可能会有属性和自我操作，但是 python 通过“dunder 方法”给了您更多

## 基础知识

让我们从简单的事情开始——假设我们需要一个类来表示一个多项式，它可能看起来像这样:

我们希望能够将它们相加或相减，甚至以简单适当的方式将它们打印到屏幕上，这里有三个最基本的“dunder 方法”，您可以添加适当的方法，并且可以使用这些操作！

如果这看起来是因为 zip 函数而完成的，不用担心，zip 只是将两个可重复项捆绑成一个可重复项的方法。

对我们来说最重要的是理解**语法并不重要**，你认识到数据模型函数(“dunder 方法”)允许你操作对象本身。

这是我在这里提出的至关重要的概念，它开始很简单，但会变得越来越有趣。

# 第三级——世界是一个对象

这句话是俗语，从 python 3 开始，一切都是对象，但这句流行语是什么意思呢？

![](img/617772f806ed4468a2f62dc5309cdef4.png)

乔恩·泰森在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上的照片

这里有一个典型的例子来说明它的意义。假设我们想测量一个函数运行的时间。

现在我解决了一个局部问题，如果我想把它加到 20 个函数上呢？这种方式是错误的，它没有考虑到 python 中的一切都是对象，包括函数 sub。
我可以调用子函数并修改它，调用的子函数会在函数上添加一个时间检查。

第 5 步——核心概念，第 7 步，是以语法的方式。

第 5 步和第 7 步没有区别。是一样的；这个 ***叫做装饰器*，**装饰器让程序员 ***在自己的函数中添加*** ***功能性*** ， ***而不触及核心函数本身。***

更多关于“args”、“kwargs”的数据，请点击此处:

[](https://medium.com/swlh/change-the-way-you-write-python-code-with-one-extra-character-6665b73803c1) [## 用一个额外的字符改变你写 Python 代码的方式

### 一个小小的语法变化，是你编码技能的一大步

medium.com](https://medium.com/swlh/change-the-way-you-write-python-code-with-one-extra-character-6665b73803c1) 

## 不要急于求成！

![](img/da5891cfb25f192f238d44c6128b3ec4.png)

[霍尔格连杆](https://unsplash.com/@photoholgic?utm_source=medium&utm_medium=referral)在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍照

同时这也暗示了第 2 篇文章(目前是最后一篇)的开始概念。
下一篇文章将更深入地挖掘这种语言的核心和特性，并希望让您更好地理解 python 是什么，而不仅仅是一些花哨的导入和看起来像这样的东西:
我希望您喜欢这篇文章，我知道我喜欢，我想把所有东西都挤在一篇文章中，但它感觉太紧凑了，我希望在下一篇文章中看到您，我也希望这篇文章给您带来一些新的东西！

在这篇文章的第二部分之前，请阅读以下内容:

[](https://medium.com/swlh/python-collections-you-should-always-be-using-b579b9e59e4) [## 您应该始终使用的 Python 集合

### Python collections 是一个被低估的库，它可以将您的编码提升到下一个级别。

medium.com](https://medium.com/swlh/python-collections-you-should-always-be-using-b579b9e59e4) 

**感谢阅读！**