# 探索性数据分析是数据科学的重要组成部分

> 原文：<https://towardsdatascience.com/exploratory-data-analysis-is-a-significant-part-of-data-science-7f3b173c04d2?source=collection_archive---------31----------------------->

## 数据科学，数据可视化

## 你将发现**探索性数据分析** (EDA)，你可以使用的技术和策略，以及为什么你应该在你的下一个问题中执行 EDA。

![](img/17106dbfac2276efcb9609029c5e03cf.png)

由[卢克·切瑟](https://unsplash.com/@lukechesser?utm_source=medium&utm_medium=referral)在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄的照片

D数据科学无所不在，以先进的**统计**和**机器学习**方法。无论有多长时间的数据需要分析，调查的需求都是显而易见的。

然而，任何数据科学任务的一个重要关键部分是探索性数据分析，这一点往往被低估

> “探索性数据分析是一种态度，一种灵活的状态，一种寻找我们认为不存在的东西以及我们认为存在的东西的意愿。”约翰·w·图基

# 构建与数据的关系

![](img/3b000d43e74e5c9950a0b161922fbdf5.png)

照片由 [Dương Hữu](https://unsplash.com/@huuduong?utm_source=medium&utm_medium=referral) 在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄

在你可以建立数据模型和测试你的假设之前，你必须建立与数据的关系。

您可以通过投入时间总结、绘制和调查来自该领域的真实数据来建立这种关系。这种建模前调查的方法论被称为**探索性数据分析**。

在预先投入时间处理数据的过程中，您可以利用数据格式、值和关系来构建直觉，这有助于稍后阐明观察结果和建模结果。

它被称为**探索性数据分析**，因为你正在调查你对数据的理解，收集创造数据的潜在过程如何工作的本能，并激发你可以用作建模理由的问题和想法。

该过程可用于检查数据，识别异常值，并提出处理异常值的具体策略。在数据上投入时间，您可以发现值中的损坏，这可能标志着数据记录过程中的缺陷。

# 探索性数据分析的开始

![](img/8ed2c990058bb64dd0fd92cde3ef977e.png)

西蒙·米加吉在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上的照片

E 探索性数据分析是贝尔实验室的约翰·图基发明的，作为一种在数据假设产生之前有效利用洞察工具解决问题的方法。

目标是理解问题以创建可测试的**假设**。考虑到所有因素，像图表和汇总统计数据这样的结果只是为了提高您的理解能力，而不是为了向普通观众展示数据中的关系。

# 探索性数据分析策略

![](img/49db8a5de1e9a054ab896edd1e57c5bd.png)

照片由[费利佩·费塔多](https://unsplash.com/@furtado?utm_source=medium&utm_medium=referral)在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 拍摄

探索性数据分析通常使用代表性的数据样本进行。您不必利用所有的数据访问。

在原始数据上投入时间。

从目测数字表开始是明智的。浏览表可以快速显示每个数据属性的类型、明显的反常情况以及值中的大异常值，并开始提出候选关系来调查属性之间的关系。

可以使用简单的**单变量**和**多变量**方法来透视数据。

例如，我认为必须具备的方法有:

*   ***汇总(平均值、中值、最小值、最大值、q1、q3)***
*   ***直方图***
*   ***折线图***
*   ***盒须图***
*   **散点图 **

除了摘要，另外，看看数据的转换和数据的重新缩放。

# 比如，问很多关于数据的问题

![](img/032bacf66b2e0c97cc42a48c3c054f72.png)

[迈特·沃尔什](https://unsplash.com/@two_tees?utm_source=medium&utm_medium=referral)在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍照

*   ***你看到了什么价值观？***
*   ***你看到哪些发行版？***
*   ***你看到了哪些关系？***
*   ***你有什么样的数据，如何对待不同类型的数据？***
*   ***数据中缺失了什么，你会如何管理？***
*   ***离群值在哪里，出于什么原因，考虑它们对你来说是个好主意？***
*   ***您将如何添加、更改或删除功能以充分利用您的数据？***

# 专心理解

![](img/df1ad1abe10c8daf4c4c31c51a81ead7.png)

[米米·蒂安](https://unsplash.com/@mimithian?utm_source=medium&utm_medium=referral)在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍照

你不是在写报告，而是在试图理解问题。

结果最终会被丢弃，留给你的应该是对数据更突出的解释和直觉，以及建模时要研究的一长串假设。

代码不应该是美丽的，但他们应该是正确的。利用可复制的脚本和标准包。

你不必跳入复杂的统计方法或图表。保持简单，花时间研究数据。

像 **SQL** 这样的查询接口可以帮助你用一个数据样本快速地处理很多假设场景。

模型可能与问题和你对数据和问题的理解不相上下。

管理**缺失值**的一些常用技术包括

*   ***如果有过多的空值，我们可以设置一个阈值来删除整列。***
*   ***删除缺少值的实例。***
*   ***删除缺少值的属性。***
*   ***使用平均值、中值、所有缺失值的模式输入属性。***
*   ***用线性回归的方法输入属性缺失值。***

回忆**齐夫定律**可以辅助**离群值**。不定期出现的接近尾部终点的值可能是异常。一种选择是尝试转型。

平方根和对数转换都需要大量的数字。如果异常值是因变量，这可以使假设更好地工作，如果**异常值**是自变量，这可以减少单个点的影响。

# **结论**

![](img/5395666e5639fff56de91af03d62af8d.png)

凯利·西克玛在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上的照片

在任何数据分析中，数据挖掘都是发现数据模式的关键步骤。它为您的整体分析过程奠定了坚实的基础。图基经常将 EDA 比作侦探工作。

数据分析师或数据科学家的角色是从尽可能多的角度关注数据，直到一个可以想象的数据故事变得显而易见，而不管这样的描述是否会在下面的示例中得到证实。

*现在，把你的想法放在****Twitter*******Linkedin****，以及****Github****！！**

****同意*** *还是* ***不同意*** *与 Saurav Singla 的观点和例子？想告诉我们你的故事吗？**

**他乐于接受建设性的反馈——如果您对此分析有后续想法，请在下面的* ***评论*** *或联系我们！！**

**推文*[***@ SauravSingla _ 08***](https://twitter.com/SAURAVSINGLA_08)***Saurav _ Singla****，还有明星*[***SauravSingla***](https://github.com/sauravsingla)*马上！**