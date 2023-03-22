# R 入门

> 原文：<https://towardsdatascience.com/getting-started-with-r-b464cecd918c?source=collection_archive---------40----------------------->

![](img/9894d915fa8f1d33d715893f96122b73.png)

[https://www.r-project.org/logo/](https://www.r-project.org/logo/)、[知识共享署名-共享 4.0 国际许可](https://creativecommons.org/licenses/by-sa/4.0/) (CC-BY-SA 4.0)

我学过的第一门编码语言是 R，我很快就学会了，在学习 python 的同时，我的 R 知识绝对对我有帮助。我发现 R 和 python 在法语和意大利语方面很相似——虽然肯定有差异，但它们有很多共同点。为了庆祝 R 4 . 0 . 3 版本(标题惊人地叫“兔子-温尼斯怪胎”)在周六发布，我为 R 编译了一些我最喜欢的资源，以及一些额外的非语言特定但有用的资源。所有这些资源不仅精彩，而且免费！我希望你和我一样觉得它们很有帮助！

首先，我们有 R 本身，可以在 [R:统计计算的 R 项目](https://www.r-project.org/)找到。如果你不知道的话，R 是免费的，而且是非常强大的统计和编码软件。要安装，进入 [CRAN 页面](https://cran.r-project.org/mirrors.html)，选择离你最近的位置。r 适用于所有三个主要系统；Mac、Windows 和 Linux。r 很棒，但是使用它最简单的方法是同时获得…

[RStudio](https://rstudio.com/) ！

RStudio 是一个集成开发环境，简称 IDE。和 R 一样，是免费的。基本上，如果 R 是打字机，RStudio 就是文字处理器。它使生活变得容易得多；使用 RStudio，您可以编辑代码、运行单个代码块、在语法不正确(最常见的错误来源之一)时获得警告、利用软件包(稍后将详细介绍)以及轻松访问帮助页面。可以在这里下载[；只需选择“下载 RStudio 桌面”。RStudio 有在线版本，但这些都有相关费用。](https://rstudio.com/products/rstudio/)

![](img/0491cddf8e375b41b7c0ea79655c8d66.png)

[Donny Jiang](https://unsplash.com/@dotnny?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) 在 [Unsplash](https://unsplash.com/s/photos/hexagon-shape?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) 上的照片

继续包装！R 有超过 10，000 个软件包，我可以写多篇文章。开始时，有几个特别有助于展示。

首先是 [tidyverse](https://tidyverse.tidyverse.org/) 。即使你从未用过 R，你也可能听说过 tidyverse。这一组核心包是我日常使用的包。tidyr(用于数据清理)、dplyr(用于数据操作)、ggplot2(用于漂亮的可视化)、hms 和 lubridate(用于任何日期或时间数据)等等，将是你开始的基础。这应该是您在 R 和 RStudio 之后的第一次安装。100/10.

[漩涡](https://swirlstats.com/)很优秀，因为它教你如何使用 R，*在 R* 。一旦你加载并打开了这个包，你可以从各种教程中选择，甚至从 [GitHub 库](https://github.com/swirldev/swirl_courses#swirl-courses)下载额外的课程。这是让你熟悉 r 的一个很棒的方法。唯一的警告是，swirl 只在控制台上运行，并且不是非常 RStudio 笔记本友好。我仍然建议在 RStudio 的控制台中使用它，这只是需要注意的一点。如果你刚开始学 R，你需要这个包。

最后两个是相当基本的，但同样有用。 [Stats](https://www.rdocumentation.org/packages/stats/versions/3.6.2) 包含基本的统计功能，而 [datasets](https://stat.ethz.ch/R-manual/R-patched/library/datasets/html/00Index.html) 有数据集供您在入门时使用。

Hadley Wickham 和 Jennifer Bryan 的书 [R Packages](https://r-pkgs.org/) 也有助于找到适合你需求的特定软件包，Google 也是如此。说到这里，让我们继续看书吧！

[R for Data Science](https://r4ds.had.co.nz/) :可视化、建模、转换、整理和导入数据，作者 Garrett Grolemund 和 Hadley Wickham

它有一步一步的问题，以一种非常自然和可理解的方式进行，并且在大多数部分的结尾还有额外的练习。这本书可以教授 r 的基础知识。如果你正在寻找一种传统的方法，这是一个很好的选择——我们在我的大学统计学课程中使用过这本书。

[R for Data Science:Exercise Solutions](https://jrnold.github.io/r4ds-exercise-solutions/)，作者 Jeffrey B. Arnold &供稿

这本书是 R for Data Science 的非正式指南，包含了 R for Data Science 中练习的示例解决方案。这可能非常有帮助，但是，需要注意的是，一些解决方案可能过于复杂，或者使用尚未涉及的策略，如果您正在按顺序学习 R for Data Science 的话。不过，它是免费的，当你真的被难住的时候，它是一个有用的资源。

数据可视化的基础知识:制作信息丰富和引人注目的图表的初级读本，作者克劳斯·威尔基

数据可视化圣经。是*优秀的*。它很好地分解了数据可视化的基础，并专注于主要原则，同时也解释了可视化的利弊。我每周至少送一次这本书给某人。这是一个相当快速的阅读，并且这本书的在线版本很容易搜索，以找到你想要参考的确切图表或数字。100000/10.我想要一本实体的作为生日礼物。

最后，最后但同样重要的是[今天](https://github.com/rfordatascience/tidytuesday)！TidyTuesday 是每周一次的数据科学和可视化挑战赛，它提供了清理、管理和可视化大型数据集的良好实践。当你厌倦了 R datasets 包时，过去的数据集也是很好的资源，因为它是开源的，所以浏览别人的代码，看看他们清理和可视化的方法会很有帮助。