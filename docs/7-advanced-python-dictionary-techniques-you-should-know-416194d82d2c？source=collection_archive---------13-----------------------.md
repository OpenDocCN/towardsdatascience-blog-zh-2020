# 你应该知道的 7 种高级 Python 字典技术

> 原文：<https://towardsdatascience.com/7-advanced-python-dictionary-techniques-you-should-know-416194d82d2c?source=collection_archive---------13----------------------->

## 根据以下提示掌握 Python 字典

![](img/5e54d4b09bf3d73ca84746402b3a96a5.png)

在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上由 [Hitesh Choudhary](https://unsplash.com/@hiteshchoudhary?utm_source=medium&utm_medium=referral) 拍摄的照片

Python 字典是非常强大的数据结构——在这篇文章中，你将学习七种技术来帮助你掌握它们！

让我们开始吧。

# 1.合并两本词典

从 Python 3.5 开始，可以很容易地合并两个字典，使用**kwargs:

合并两本词典。资料来源:Nik Piepenbreier

从 **Python 3.9** 开始，您可以使用 union 操作符(|)来组合两个字典:

合并两本词典。资料来源:Nik Piepenbreier

# 2.检查字典中是否存在关键字

要查看字典中是否存在某个键，可以使用关键字中的*。*

例如，让我们看看字典中是否存在一些键:

检查字典成员。资料来源:Nik Piepenbreier

# 3.从词典中删除一项

要轻松删除词典，您可以使用 pop 功能。pop 函数返回被删除的键的值。

Pop 接受两个参数(要删除的键，如果找不到键，则为默认值)。

让我们尝试删除一些项目:

从词典中删除一项。资料来源:Nik Piepenbreier

最后一个 print 语句返回一个错误，因为键不存在并且没有提供默认值。

# 4.词典释义

Python 字典理解可以让您轻松地迭代键和值。

一个很容易解释的例子来说明这一点，就是构建一个字典，其中的键是一个数字，值是数字的平方:

掌握词典释义。资料来源:Nik Piepenbreier

在[这篇深度文章](https://datagy.io/python-dictionary-comprehensions/)中了解更多关于字典理解的知识！

# 5.从词典中删除空条目

让我们使用列表理解来从字典中删除所有的空条目。当一个项的值为 None 时，它被认为是空的。

从字典中删除空条目。资料来源:Nik Piepenbreier

# 6.使用 Get 而不是字典访问器

要访问 Python 字典项，可以使用方括号符号。这很有效——直到它失效。

如果字典中不存在某个键，就会抛出 AttributeError。

这可能会扰乱您的代码。

但是，使用 get 方法只会返回 None:

如何为字典使用 get？资料来源:Nik Piepenbreier

# 7.基于值过滤字典

假设你有一本关于人们身高的字典，你想过滤掉低于某一高度的人。

您可以通过 for 循环或者(更简单的)字典理解来实现这一点。

我们把 170cm 以下的都过滤掉吧。

根据字典的值过滤字典。资料来源:Nik Piepenbreier

# 结论

感谢阅读！在本教程中，您学习了 Python 字典的 7 种技术。

如果你对掌握 Python 列表感兴趣，可以看看我的另一个教程:

[](/advanced-python-list-techniques-c6195fa699a3) [## 你应该知道的 8 种高级 Python 列表技术！

### 让全能榜单更加强大！

towardsdatascience.com](/advanced-python-list-techniques-c6195fa699a3)