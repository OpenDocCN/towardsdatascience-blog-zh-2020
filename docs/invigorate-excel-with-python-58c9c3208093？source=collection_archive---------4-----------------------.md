# 用 Python 激活 Excel

> 原文：<https://towardsdatascience.com/invigorate-excel-with-python-58c9c3208093?source=collection_archive---------4----------------------->

![](img/3f633c4fefe09db1e538998755e45904.png)

LP。照片[由 wonder](https://unsplash.com/@wonderlane?utm_source=medium&utm_medium=referral)和[在 Unspl](https://unsplash.com?utm_source=medium&utm_medium=referral) 灰上拍摄

## 三大集成方法以及您可以利用它们做什么

Microsoft Excel —你要么喜欢它，要么讨厌它！如果你和大多数人一样，你可能在没有听说过 Excel 的情况下经历了早期的生活。你可能已经上了大学，以优异的成绩毕业，但仍然对 Excel 知之甚少。

那是在你进入工业界并找到工作之前。然后，您发现如果 Excel 消失一个小时，整个世界都会停止运转！

现在，您可能知道 Excel 几乎没有做不到的事情！我在投资银行工作了很多年，我可以告诉你，每次我认为我已经看到了一切，我会看到别人整理的另一个电子表格！可能性真的是无穷无尽。

然而，目前 Excel 的主要局限在于大型数据集。数据集越大，在 Excel 中面临的困难就越多。在一个数据驱动、即时满足的世界里，人们希望事情立即发生；他们讨厌等待！人们还期望不断突破界限，提供不断提高的功能水平。

同时，人们讨厌改变。人们在 Excel 中很舒服，他们并不真的想远离它。因此，我们的工作是使这一过渡更加容易。提供更快的速度、更多的功能，而无需离开电子表格。

在 Excel 能够支持大数据之前，这就是 Python 的用武之地！将 Excel 与 Python 集成在一起，使我们能够通过 Excel 为用户提供更强大的功能。它允许我们的用户保持在他们熟悉的、容易理解的 Excel 世界中，同时 Python 可以处理一些繁重的工作！它为这个数据驱动的世界提供了一个中间步骤，直到 Excel 和我们技术含量较低的同事赶上来。

接下来，让我们探索一些可用的选项，用 Python 的强大功能来激活 Excel！

![](img/4e9e860e7a224dbed710d17055b138bf.png)

由[凯利·西克玛](https://unsplash.com/@kellysikkema?utm_source=medium&utm_medium=referral)在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄

# Excel 作为用户界面

将您的用户群从电子表格过渡到 21 世纪并不总是一件容易的事情。这是一个需要时间的旅程，但作为 It 专业人员，我们有责任帮助我们的用户群度过难关。在这个过程中，我们需要大量的指导、信任和保证。

首先，您可以考虑保持用户界面(UI)不变。换句话说，让我们不去管电子表格，但是让我们把任何后端处理从 VBA 转移到 Python。

> 考虑给 Excel…插上翅膀！

使用名为 xlwings 的 Python 包，可以无缝集成 Excel 和 Python。我们的用户可以继续使用 excel，但是每个表单控件按钮或用户定义的 Excel 函数都可以调用我们的 Python 脚本。

[](/how-to-supercharge-excel-with-python-726b0f8e22c2) [## 如何用 Python 为 Excel 增压

### 如何用 xlwings 集成 Python 和 Excel

towardsdatascience.com](/how-to-supercharge-excel-with-python-726b0f8e22c2) 

该应用程序易于安装，并奇妙的使用。如果你想了解更多，我已经链接了我以前的深入教程。

# 寻找新的 Excel

随着技术、数据和数据科学工具的爆炸，出现了一种新的用户:超级用户！

超级用户了解技术、数据，他们可以编写代码。他们要求表达自己和解决一些问题的自由。他们非常乐意离开他们的电子表格，使用新技术。那么，如何让他们灵活地做到这一点呢？

> 考虑给他们… Jupyter 笔记本！

Jupyter Notebook 允许他们利用 Python 创建可共享的、交互式的、基于 web 的文档，其中可以包含实时代码、可视化和文本。至于可以利用的数据，您可以继续使用您的企业数据源和数据库。

[](/jupyter-is-the-new-excel-a7a22f2fc13a) [## Jupyter 是新的 Excel

### 为什么交易者和金融专业人士需要学习 Python

towardsdatascience.com](/jupyter-is-the-new-excel-a7a22f2fc13a) 

同样，该应用程序易于安装，并且非常易于学习使用。如果你想了解更多，我已经链接了 Semi Koen 的博客。

# Excel 作为输入

不管怎么说，人们都喜欢 Excel，数据生成很可能会继续使用电子表格。然而，随着分析所需数据的增加，人们可以立即感觉到 Excel 地狱的末日即将来临。数据操作将花费很长时间，而枢纽操作将是永恒的。前提是 Excel 没有先崩溃！

这是 Python 可以提供帮助的另一个例子。使用流行的 pandas 库，可以快速地将电子表格中的数据加载到 pandas 数据框架或 SQL 数据库中。这两种解决方案都支持快速、轻松的数据分析和探索。

我在下面链接了几篇关于这个主题的博文，如果你想了解更多的话。

[](https://medium.com/financeexplained/from-excel-to-databases-with-python-c6f70bdc509b) [## 用 Python 从 Excel 到数据库

### 了解如何使用 Python 进行快速数据分析

medium.com](https://medium.com/financeexplained/from-excel-to-databases-with-python-c6f70bdc509b) [](https://medium.com/financeexplained/from-excel-and-pandas-dataframes-to-sql-f6e9c6b4a36a) [## 从 Excel 和 Pandas 数据框架到 SQL

### 如何使用您的 SQL 知识来利用 Python 的熊猫

medium.com](https://medium.com/financeexplained/from-excel-and-pandas-dataframes-to-sql-f6e9c6b4a36a) 

# 要考虑的其他库

在结束我们的讨论之前，让我们简要介绍一下其他一些流行的特定于 Excel 的 Python 库。我的观点是，我们上面提到的库将能够满足大多数用例，但是，如果您正在寻找 Excel 特定的功能(如格式、过滤器等)，您可能希望尝试探索以下一些库:

## [openpyxl](https://openpyxl.readthedocs.io/en/stable/)

读取和写入 Excel 2010 文件的库。您可以编写新的工作表，编辑现有的工作表，以及在 Excel 中使用鼠标进行几乎任何操作。它的超能力？它几乎支持任何 Excel 扩展！

## [xlrd](https://xlrd.readthedocs.io/en/latest/)

用于读取 Excel 文件中的数据和格式化信息的库。

## [xlsxwriter](https://xlsxwriter.readthedocs.io/)

可能是最完整的所有 Excel Python 库。

格式，条件格式，图表，合并单元格，过滤器，评论，与熊猫集成只是它提供的一些功能。如果您希望通过 Python 脚本使用 Excel 的全部功能，可以从这里开始！

# 结论

Excel 是一个在工业中广泛使用的神奇工具。如果你不熟悉，你最好开始！Excel 的主要限制是大数据，但我相信用不了多久，微软就会出手拯救世界。然而，在此之前，我们可以使用 Python 来帮助解决我们可能遇到的一些问题。

您是否使用不同的 Python 库来与 Excel 集成？让我和你的读者朋友们知道吧！

如果您喜欢这篇文章，您可能也会喜欢:

[](/big-data-says-everybody-lies-b7e9e28376c2) [## 大数据显示每个人都会撒谎

### 为什么大数据如此重要，为什么您也应该关注它

towardsdatascience.com](/big-data-says-everybody-lies-b7e9e28376c2) [](/learn-how-to-quickly-create-uis-in-python-a97ae1394d5) [## 了解如何用 Python 快速创建 ui

### 最后，你可以在 10 分钟内找到一个图书馆

towardsdatascience.com](/learn-how-to-quickly-create-uis-in-python-a97ae1394d5) [](https://medium.com/better-programming/how-disney-heroes-help-me-solve-problems-bc4d3e230bfb) [## 迪士尼英雄如何帮助我解决问题

### 学会使用迪士尼方法解决问题

medium.com](https://medium.com/better-programming/how-disney-heroes-help-me-solve-problems-bc4d3e230bfb)