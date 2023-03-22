# Python 比 r 好。

> 原文：<https://towardsdatascience.com/python-is-better-than-r-f7bb963a1c85?source=collection_archive---------37----------------------->

## 原因如下。

![](img/941136f77c29886542ec101b45ad5c19.png)

戴维·克洛德在[Unsplash](https://unsplash.com/s/photos/python?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)【1】上拍摄的照片。

# 目录

1.  介绍
2.  原因如下
3.  摘要
4.  参考

# 介绍

虽然说 Python 比 R 好对我来说是真的，但对你来说可能不是真的。出于各种原因，你当然会认为 R 比 Python 更有用。即使你反对我的声明，我仍然希望开始一个对话，让我们都能看到两种编程语言的好处。对于数据科学家来说，我相信 Python 比 R 有更多的好处。我确实意识到 R 有一些独特而强大的统计库，很可能会盖过 Python 库:然而；通过使用 Python，整个数据科学过程能够与数据工程师、软件工程师和机器学习工程师一起扩展，从而获得更多好处。

下面，我将讨论为什么我认为 Python 比 r 更好的五个主要原因。这些原因包括:可伸缩性、Jupyter 笔记本、库包、集成以及成为跨职能团队成员的能力。

# 原因如下

*   ***扩展性***

可伸缩性是在数据科学中使用的一个巨大优势。因为大多数数据科学家经常与工程部门的其他员工一起工作，建模可以变得更容易部署，模型的一般、整体流程也是如此。例如，一个典型的数据科学家可能只专注于执行建模，甚至是一次性的输出。然而，在建模之前有一个步骤，你很可能需要在训练你的机器学习模型之前完成。这一步是数据工程部分。在这个过程中，您可以从 SQL 数据库中自动读入新数据，以便您的模型在训练时总是最新的。流程的另一面是部署方面。第一次部署一个模型可能是相当令人生畏的，特别是因为它在学校里的教授不像建模过程那么多。

> 软件工程师和机器学习工程师可以和你并肩工作，因为 Python。

您可以创建气流定向非循环图(DAG ),当特定计划中有新数据或满足某些参数时，该图可以自动训练模型(例如，如果我们获得 100 条新的传入数据记录，则仅训练该模型)。一旦模型被训练，它可以评估新的数据，然后可以通过使用 Python 将这些数据输出到 SQL 表中。

*   ***Jupyter 笔记本***

或者另一个类似的数据科学可视化工具，能够解释 Python。您可以运行代码单元、添加注释、创建标题，以及添加可以改进笔记本功能的小部件。你在这里写和分享的代码就是 Python。能够在 Jupyter 笔记本上用这种编程语言编码是数据科学家的一大胜利。

*   ***库包***

有几个功能强大的常用包可以用 Python 访问。想到的一些是 sklearn(也称为 sci-kit learn)和 TensorFlow。

> [*sk learn*](https://scikit-learn.org/stable/)*【2】*

这个强大的数据科学库打包了分类和回归模型，随时可用于您的数据集。

— *分类*

Sklearn 对分类的定义是:识别一个物体所属的类别。一些流行的算法包括支持向量机(SVM)、最近邻和随机森林。Sklearn 还概述了垃圾邮件检测和图像回归，作为他们最受欢迎的应用程序用例。

— *回归*

Sklearn 对回归的定义是:预测一个与对象相关联的连续值属性。流行的回归算法包括支持向量回归(SVR)和最近邻法，以及药物反应和股票价格等应用。

> [*张量流*](https://www.tensorflow.org/)*【3】*

对于深度学习，这个库是我建模更复杂情况的首选。人们可以用这个流行而强大的库进行的一些主要项目有:神经网络、一般对抗网络和注意力集中的神经机器翻译。

*   ***集成***

因为我在大部分数据科学项目中使用 Python，所以我成功地集成了模型。py 文件转换成面向对象的编程格式。这些文件是以模块化的方式有条不紊地开发的。用 Python 调用 API 有些简单，因为网站上有很多文档可以帮助获取网站/公司数据。

*   ***交叉功能***

这个原因在某种程度上是可伸缩性和集成的结合。如果您想在本地执行数据科学流程，并将结果交给利益相关方，这很好，但是使用 Python，您可以与来自工程领域的不同专家一起做更多事情。

> 当我第一次开始编码时，是在 R 中，当我为了部署的目的向数据工程师和软件工程师展示我的过程和代码时，需要一些额外的时间来准确描述代码背后的数据科学。

我还会发现，与我一起工作的大多数帮助我部署模型的工程师已经在使用 Python，因此他们可以相当容易地翻译我的数据科学代码，即使他们并不完全理解模型是如何工作的。

# 摘要

![](img/5c63acf00277c662fb96b2ff7fafea0e.png)

克里斯里德在[Unsplash](https://unsplash.com/s/photos/python?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)【4】上的照片。

如您所见，选择使用 Python 的数据科学家有很多好处。虽然这两种编程语言都非常有用和成功，但我从个人经验中发现 Python 比 r 更好。这些主要原因包括但不限于:可伸缩性、Jupyter 笔记本、库包、集成和交叉功能。最终，语言的选择取决于数据科学家，但本文的目标是展示我如何使用 Python 进行数据科学家项目，以及为什么使用 Python 比 R 编程更好。

*我希望你觉得这篇文章既有趣又有用。感谢您的阅读！*

# 参考

[1]照片由 [David Clode](https://unsplash.com/@davidclode?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) 在[Unsplash](https://unsplash.com/s/photos/python?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)(2018)上拍摄

[2] sklearn， [sklearn 主页](https://scikit-learn.org/stable/)，(2020)

[3] TensorFlow， [TensorFlow 主页](https://www.tensorflow.org/)，(2020)

[4]Chris Ried 在 [Unsplash](https://unsplash.com/s/photos/python?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) 上拍摄的照片，(2018)