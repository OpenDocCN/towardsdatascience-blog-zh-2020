# 数据科学的顶级 R 库

> 原文：<https://towardsdatascience.com/top-r-libraries-for-data-science-29b4e9f4907c?source=collection_archive---------21----------------------->

## 面向数据科学的流行 R 库概述

![](img/03ae88be6c1cbad884aa83d5f39d8838.png)

照片由来自 [Pexels](https://www.pexels.com/photo/working-pattern-internet-abstract-1089438/?utm_content=attributionCopyText&utm_medium=referral&utm_source=pexels) 的 [Markus Spiske](https://www.pexels.com/@markusspiske?utm_content=attributionCopyText&utm_medium=referral&utm_source=pexels) 拍摄

> 数据科学是让数据变得有用的学科

当我们谈论数据科学的顶级编程语言[](/top-programming-languages-for-ai-engineers-in-2020-33a9f16a80b0)**时，我们经常发现 Python 是最适合这个话题的。当然，对于大多数以数据科学为中心的任务来说，Python 无疑是一个极好的选择，但是还有另一种专门为数据科学提供卓越的数字处理能力的编程语言，那就是 r .**

****除了提供强大的统计计算之外，R 还提供了大量资源丰富的库，准确地说超过了 16000 个，满足了数据科学家、数据挖掘者和统计学家的需求。此外，在本文中，我们将介绍一些顶尖的数据科学 R 库。****

****[](https://blog.digitalogy.co/r-for-data-science/) [## 数据科学应该选择 R 的 6 个理由？

### 最近更新由克莱尔 d。人工智能创新的加速增长已经导致几个…

blog.digitalogy.co](https://blog.digitalogy.co/r-for-data-science/)**** 

# ****数据科学最佳 R 库****

****r 在**数据挖掘者和统计学家**中非常受欢迎，部分原因是 r 自带的**广泛的库**，这些工具和函数能够**在很大程度上简化统计任务**，使得诸如**数据操作、可视化、网页抓取、机器学习**等任务变得轻而易举。下面简要介绍了一些库:****

# ****1. **dplyr******

****[**dplyr 包**，](https://www.rdocumentation.org/packages/dplyr/versions/0.7.8#:~:text=dplyr%20is%20a%20grammar%20of,cases%20based%20on%20their%20values.)也称为数据操作的**语法**，本质上提供了数据操作常用的**工具和函数，包括以下函数:******

*   ******filter():** 根据标准过滤您的数据****
*   ******mutate():** 添加新变量，作为现有变量的函数****
*   ******select():** 根据名称选择变量****
*   ******summary():**帮助汇总来自多个值的数据****
*   ******arrange():** 用于重新排列行的顺序****
*   ****此外，您可以使用 **group_by()函数**，它可以返回根据需求分组的结果。如果您热衷于检查 dplyr 包，您可以从[**tidy verse**](https://dplyr.tidyverse.org/)**中获得它，或者使用**命令“install.packages("dplyr ")直接安装该包。********

# ****2.tidyr****

****[**tidyr**](https://blog.rstudio.com/2014/07/22/introducing-tidyr/#:~:text=tidyr%20is%20new%20package%20that,Each%20column%20is%20a%20variable.) 是[**tidy verse**](https://www.tidyverse.org/)**生态系统**中的核心包之一，顾名思义就是**用来收拾杂乱的数据**。现在，如果你想知道什么是整洁的数据，让我为你澄清一下。一个整齐的数据表示每一列都是变量，每一行都是观测值，每一个单元格都是奇异值。****

****根据 tidyr 的说法，整理数据是一种存储将在整个 tidyverse 中使用的数据的方式，可以帮助您节省时间并提高分析效率。你可以从 [**tidyverse**](https://tidyr.tidyverse.org/) 或者通过下面的**命令“install.packages("tidyr ")”来获得这个包。******

****[](/top-google-ai-tools-for-everyone-60346ab7e08) [## 面向所有人的顶级谷歌人工智能工具

### 使用谷歌人工智能中心将想法变为现实

towardsdatascience.com](/top-google-ai-tools-for-everyone-60346ab7e08) 

# 3. **ggplot2**

[**ggplot2**](http://ggplot2) 是用于数据可视化的顶级 R 库**之一，并被全球成千上万的用户积极用于**创建引人注目的图表、图形和绘图**。这种流行背后的原因是 ggplot2 是为了简化可视化过程而创建的，它从开发人员那里获取最少的输入，例如要可视化的数据、样式和要使用的原语，而将其余的留给库。**

结果是一个图表，可以毫不费力地呈现复杂的统计数据，实现即时可视化。如果你想给你的图表增加更多的可定制性，你可以使用像[**RStudio**](https://rstudio.com/) 这样的 ide 来进行更精细的控制。你可以通过 tidyverse 集合或者通过**命令“install.packages("ggplot2 ")”使用独立库来获得 [**ggplot2。**](https://ggplot2.tidyverse.org/)**

**阅读 R 文档，了解 ggplot2 功能-**

[](https://www.rdocumentation.org/packages/ggplot2/versions/3.1.0) [## ggplot2

### 一个基于“图形语法”的“声明式”创建图形的系统。你提供数据，告诉…

www.rdocumentation.org](https://www.rdocumentation.org/packages/ggplot2/versions/3.1.0) 

# 4.润滑剂

对于数据科学来说，R 是一种优秀的编程语言，但是在某些领域，R 可能感觉不够完善。其中一个领域是日期和时间的处理。对于任何大量使用 R 中的日期和时间的人来说，可能会发现它的内置功能很麻烦。

为了克服这一点，我们有一个方便的包叫做[**lubridate**](https://cran.r-project.org/web/packages/lubridate/vignettes/lubridate.html)**。**这个包不仅处理 R 中的标准日期和时间，还提供了额外的增强功能，如时间段、夏令时、闰日、支持各种时区、快速时间解析和许多辅助功能。如果你的项目需要处理时间和日期，你可以从 tidyverse 获得 [**lubridate 包，或者用**“install . packages(" lubridate ")”命令安装这个包。****](https://lubridate.tidyverse.org/)

**阅读此处的文档:**

[](https://www.rdocumentation.org/packages/lubridate/versions/1.7.4) [## 润滑剂

### 功能与日期时间和时间跨度:快速和用户友好的日期时间数据解析，提取和…

www.rdocumentation.org](https://www.rdocumentation.org/packages/lubridate/versions/1.7.4) 

# 5.格子木架

[**点阵**](https://cran.r-project.org/web/packages/lattice/lattice.pdf) 是另一个优雅而又**强大的数据可视化库**专注于多元数据。这个库的特别之处在于，除了**处理常规可视化**，lattice 还准备好了对非标准情况和需求的支持。由于是 R 的格子图形的实际实现，它允许你**创建格子图形**，甚至提供选项来根据你的需求调整图形。lattice 默认自带 R，但是有一个 lattice 的高级版本叫做 [**latticeExtra**](https://cran.r-project.org/web/packages/latticeExtra/index.html) ，如果你想扩展 lattice 提供的核心特性，这个版本可能会派上用场。

# 6.最低贷款利率(minimum lending rate)

R(mlr) 中的**机器学习，是一个在 **2013** 发布，并在 2019** 更新为 [**mlr3**](http://mlr3) 的库，拥有更新的技术，更好的架构，以及**核心设计。到目前为止，该库提供了一个框架来处理几种分类、回归、支持向量机和许多其他机器学习活动。**

mlr3 面向机器学习从业者和研究人员，方便各种 [**机器学习算法**](/a-tour-of-machine-learning-algorithms-466b8bf75c0a) 的基准测试和部署，没有太多麻烦。对于那些希望扩展甚至组合现有学习者并微调任务最佳技术的人来说，mlr3 是一个完美的选择。可以使用**命令“install.packages("mlr3 ")”安装 mlr3。**

**这里提到了广泛的功能—**

 [## mlr 包| R 文档

### 接口到大量的分类和回归技术，包括机器可读的参数…

www.rdocumentation.org](https://www.rdocumentation.org/packages/mlr/versions/2.13) 

# 7.**插入符号**

**分类和回归训练**的简称， [**caret**](http://caret) 库提供了几个函数，针对棘手的回归和分类问题优化模型训练的过程。caret 附带了几个额外的工具和功能，用于数据分割、变量重要性估计、特征选择、预处理等任务。使用 caret，您还可以测量模型的性能，甚至可以根据您的需求使用各种参数(如 **tuneLength 或 tuneGrid)来微调模型行为。这个包本身很容易使用，并且只加载必要的组件。这个库可以用**命令“install.packages("caret ")”来安装。****

[](/python-vs-and-r-for-data-science-4a32580846a4) [## Python vs(和)R 用于数据科学

### 用一组预定义的因素详细比较 Python 和 R

towardsdatascience.com](/python-vs-and-r-for-data-science-4a32580846a4) 

# 8.**埃斯奎塞**

[**esquisse**](https://cran.r-project.org/web/packages/esquisse/readme/README.html) 本身并不是一个库，而是强大的数据可视化库 **ggplot2** 的一个 addin。您可能想知道为什么在 ggplot2 中需要这个，让我来为您解释一下。ggplot2 已经足够智能了，但是如果您需要一个额外的可视化层，esquisse 是正确的选择。esquisse 允许您简单地拖放所需的数据，选择所需的定制选项，这样您就有了一个在短时间内构建的定制图，并准备好导出到您选择的应用程序。使用 esquisse，您可以创建可视化效果，如**条形图、直方图、散点图、sf 对象**。您可以使用“install.packages("esquisse ")”将 esquisse 添加到您的环境**。**

# 9.**闪亮的**

[**闪亮的**](https://github.com/rstudio/shiny) 是来自 RStudio 的一个 web 应用框架**，它允许开发者在最少的 web 开发背景下使用 R 创建交互式 web 应用。有了 shiny，你可以**构建网页、交互式可视化、仪表盘，甚至在 R 文档上嵌入小部件**。shiny 还可以很容易地用 CSS 主题、JavaScript 动作和 htmlwidgets 进行扩展，以增加定制。它附带了许多吸引人的内置小部件，用于呈现 R 对象的绘图、表格和输出，无论你在 shiny 中编写什么，都可以即时生效，消除了那些烦人的频繁页面刷新。如果你对这些特性感兴趣并想尝试一下，你可以使用**命令“install.packages("shiny ")”来让自己变得闪亮。****

# 10.**滚轮**

如果你正在寻找一个工具来**从网站上抓取数据**并且也是以一种可理解的格式，不用再找了， [**Rcrawler**](https://cran.r-project.org/web/packages/Rcrawler/Rcrawler.pdf) 是你的正确选择。借助 Rcrawler 强大的**网页抓取、数据抓取、数据挖掘能力**，不仅可以抓取网站、抓取数据，还可以分析任何网站的网络结构，包括其内部和外部超链接。如果你想知道为什么不使用 [**rvest**](https://cran.r-project.org/web/packages/rvest/rvest.pdf) 的话，Rcrawler 包是 rvest 的升级版，因为它可以遍历网站上的所有页面并提取数据，这在试图从一个来源一次性收集所有信息时非常有用。这个包可以用**命令“install.packages("Rcrawler ")”来安装。**

# 11. **DT**

[**DT 包**](https://cran.r-project.org/web/packages/DT/DT.pdf) 充当了 **JavaScript 库的包装器，称为 DataTables** ，对于 R. DT，你可以将 R 矩阵中的数据转换成 HTML 页面上的交互表格，方便数据的搜索、排序和过滤。这个包的工作原理是让主函数(即 datatable()函数)为 R 对象创建一个 HTML 小部件。DT 允许通过“options”参数进行进一步的微调，甚至对表进行一些额外的定制，所有这些都不需要深入编码。可以使用命令“install.packages("DT ")”来安装 DT 包。

# 12.**阴谋地**

如果你想创造出抢尽风头的交互式可视化效果， [plotly](https://cran.r-project.org/web/packages/plotly/plotly.pdf) 将是你的完美选择。使用 plotly，您可以从各种图表和图形中创建令人惊叹的、值得出版的可视化效果，如**散点图和折线图、条形图、饼图、直方图、热图、等高线图、时间序列**，只要您能想到，Plotly 都能做到。plotly 可视化构建在 plotly.js 库之上，也可以通过 Dash 在 web 应用程序中显示，在 Jupyter 笔记本中显示，或者保存为 HTML 文件。如果您有兴趣试用这个包，您可以使用命令“install.packages("plotly ")”来安装它。

## ***其他值得 R 库—***

*   生物导体
*   针织工
*   看门人
*   随机森林
*   e1071
*   stringr
*   数据表
*   RMarkdown
*   Rvest

# 结论

在整篇文章中，我们介绍了一些涵盖常见数据科学任务的顶级 R 库，如可视化、语法、机器学习模型训练和优化。我们知道这并不是一个详尽的列表，也绝不是 R 拥有的庞大的图书馆生态系统的全部。CRAN 是所有东西的储存库，它有数千个同样强大和足智多谋的库，可以满足您的特定需求，提供详细的信息和文档，如果您需要找到一个库，我们强烈建议您尝试一下 CRAN。

> ***注:*** *为了消除各种各样的问题，我想提醒你一个事实，这篇文章仅代表我想分享的个人观点，你有权不同意它。如果我错过了任何重要的库，请在评论区告诉我。*

# 更多有趣的阅读—

我希望这篇文章对你有用！下面是一些有趣的读物，希望你也会喜欢

[](/best-python-libraries-for-machine-learning-and-deep-learning-b0bd40c7e8c) [## 机器学习和深度学习的最佳 Python 库

### 现代机器学习模型和项目的 Python 库

towardsdatascience.com](/best-python-libraries-for-machine-learning-and-deep-learning-b0bd40c7e8c) [](/data-science-books-you-must-read-in-2020-1f30daace1cb) [## 2020 年必读的数据科学书籍

### 看看吧，你为什么要读它们？

towardsdatascience.com](/data-science-books-you-must-read-in-2020-1f30daace1cb) [](/best-data-science-tools-for-data-scientists-75be64144a88) [## 数据科学家的最佳数据科学工具

### 数据科学工具，使任务可以实现

towardsdatascience.com](/best-data-science-tools-for-data-scientists-75be64144a88) [](/top-python-libraries-for-data-science-c226dc74999b) [## 面向数据科学的顶级 Python 库

### 面向数据科学的流行 Python 库概述

towardsdatascience.com](/top-python-libraries-for-data-science-c226dc74999b) [](/best-python-ides-and-code-editors-you-must-use-in-2020-2303a53db24) [## 2020 年你必须使用的最好的 Python IDEs 和代码编辑器

### 具有显著特性的顶级 Python IDEs 和代码编辑器

towardsdatascience.com](/best-python-ides-and-code-editors-you-must-use-in-2020-2303a53db24) 

> ***关于作者***
> 
> ***克莱尔 D*** *。在*[***digital ogy***](https://www.digitalogy.co/)***—****是一个内容制作者和营销人员。这是一个技术采购和定制匹配市场，根据全球各地的特定需求，将人们与预先筛选的&顶尖开发人员和设计师联系起来。连接****Digitalogy****on*[***Linkedin***](https://www.linkedin.com/company/digitalogy)*[***Twitter***](https://twitter.com/DigitalogyCorp)*[***insta gram***](https://www.instagram.com/digitalogycorp)*。*******