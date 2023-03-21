# 一种安装和加载 R 包的有效方法

> 原文：<https://towardsdatascience.com/an-efficient-way-to-install-and-load-r-packages-bc53247f058d?source=collection_archive---------25----------------------->

![](img/cac728e88e246fe59075fb5e54e9caee.png)

照片由[克劳迪奥·施瓦茨](https://unsplash.com/@purzlbaum?utm_source=medium&utm_medium=referral)拍摄

# 什么是 R 包，如何使用？

与其他程序一样，r 默认只提供基本功能。因此，您经常需要安装一些“扩展”来执行您想要的分析。这些扩展是由 R 用户开发和发布的函数和数据集的集合，称为**包**。它们通过添加新的功能来扩展现有的 base R 功能。r 是开源的，所以每个人都可以编写代码并将其发布为一个包，每个人都可以安装一个包并开始使用包中内置的函数或数据集，所有这些都是免费的。

为了使用一个包，需要通过运行`install.packages("name_of_package")`将它安装在你的计算机上(不要忘记在包的名字周围加上`""`，否则，R 会寻找以那个名字保存的对象！).安装软件包后，您必须加载软件包，只有在加载后，您才能使用它包含的所有函数和数据集。要加载一个包，运行`library(name_of_package)`(这一次包名两边的`""`是可选的，但是如果你愿意，仍然可以使用)。

# 安装和加载 R 包的低效方法

根据你使用 R 的时间长短，你可能会使用有限数量的软件包，或者相反，使用大量的软件包。随着你使用越来越多的软件包，你很快就会开始有(太多)多行代码来安装和加载它们。

下面是我博士论文中的代码预览，展示了当我开始研究 R 时，R 包的安装和加载是什么样子的(为了缩短代码，只显示了一小部分):

```
# Installation of required packages
install.packages("tidyverse")
install.packages("ggplot2")
install.packages("readxl")
install.packages("dplyr")
install.packages("tidyr")
install.packages("ggfortify")
install.packages("DT")
install.packages("reshape2")
install.packages("knitr")
install.packages("lubridate")# Load packages
library("tidyverse")
library("ggplot2")
library("readxl")
library("dplyr")
library("tidyr")
library("ggfortify")
library("DT")
library("reshape2")
library("knitr")
library("lubridate")
```

你可以猜到，随着我需要越来越多的分析包，代码变得越来越长。此外，我倾向于重新安装所有的软件包，因为我在 4 台不同的计算机上工作，我不记得哪个软件包已经安装在哪个机器上了。每次打开我的脚本或 R Markdown 文档时重新安装所有的包都是浪费时间。

# 更有效的方法

后来有一天，我的一个同事跟我分享了他的一些代码。我很高兴他这样做了，因为他向我介绍了一种更有效的安装和加载 R 包的方法。他允许我分享这个技巧，所以下面是我现在用来执行安装和加载 R 包任务的代码:

```
# Package names
packages <- c("ggplot2", "readxl", "dplyr", "tidyr", "ggfortify", "DT", "reshape2", "knitr", "lubridate", "pwr", "psy", "car", "doBy", "imputeMissings", "RcmdrMisc", "questionr", "vcd", "multcomp", "KappaGUI", "rcompanion", "FactoMineR", "factoextra", "corrplot", "ltm", "goeveg", "corrplot", "FSA", "MASS", "scales", "nlme", "psych", "ordinal", "lmtest", "ggpubr", "dslabs", "stringr", "assist", "ggstatsplot", "forcats", "styler", "remedy", "snakecaser", "addinslist", "esquisse", "here", "summarytools", "magrittr", "tidyverse", "funModeling", "pander", "cluster", "abind")# Install packages not yet installed
installed_packages <- packages %in% rownames(installed.packages())
if (any(installed_packages == FALSE)) {
  install.packages(packages[!installed_packages])
}# Packages loading
invisible(lapply(packages, library, character.only = TRUE))
```

这段安装和加载 R 包的代码在几个方面更有效:

1.  函数`install.packages()`接受一个向量作为参数，所以过去每个包的一行代码现在变成了包含所有包的一行代码
2.  在代码的第二部分，它检查一个包是否已经安装，然后只安装缺少的包
3.  关于包的加载(代码的最后一部分)，使用`lapply()`函数一次性调用所有包的`library()`函数，这使得代码更加简洁。
4.  加载包时的输出很少有用。`invisible()`功能删除该输出。

从那天起，每当我需要使用一个新的包时，我简单地把它添加到代码顶部的向量`packages`，它位于我的脚本和 R Markdown 文档的顶部。无论我在哪台计算机上工作，运行整个代码都将只安装缺失的包并加载所有的包。这大大减少了安装和加载我的 R 包的运行时间。

# 最有效的方法

# `{pacman}`包装

这篇文章发表后，有读者通知我关于`{pacman}`包的事情。在阅读了文档并亲自试用之后，我了解到`{pacman}`中的函数`p_load()`会检查是否安装了某个包，如果没有，它会尝试安装该包，然后加载它。它还可以同时应用于多个包，所有这一切都以一种非常简洁的方式进行:

```
install.packages("pacman")pacman::p_load(ggplot2, tidyr, dplyr)
```

在[曲柄](https://cran.r-project.org/web/packages/pacman/index.html)上找到更多关于此包装的信息。

# `{librarian}`包装

与`{pacman}`一样，`{librarian}`包中的`shelf()`函数自动安装、更新和加载尚未安装在单个函数中的 R 包。该功能接受来自 CRAN、GitHub 和 Bioconductor 的软件包(仅当安装了 Bioconductor 的`Biobase`软件包时)。该函数还接受多个包条目，以逗号分隔的未加引号的名称列表的形式提供(因此包名周围没有`""`)。

最后但同样重要的是，`{librarian}`包允许在每个 R 会话开始时自动加载包(感谢`lib_startup()`函数)，并通过关键字或正则表达式在 CRAN 上搜索新包(感谢`browse_cran()`函数)。

下面是一个如何安装缺失的包并用`shelf()`函数加载它们的例子:

```
# From CRAN:
install.packages("librarian")librarian::shelf(ggplot2, DesiQuintans/desiderata, pander)
```

对于 CRAN 包，提供不带`""`的普通包名，对于 GitHub 包，提供用`/`分隔的用户名和包名(即`desiderata`包所示的`UserName/RepoName`)。

在 [CRAN](https://cran.r-project.org/web/packages/librarian/index.html) 上找到更多关于这个包的信息。

感谢阅读。我希望这篇文章能帮助你以更有效的方式安装和加载 R 包。

和往常一样，如果您有与本文主题相关的问题或建议，请将其添加为评论，以便其他读者可以从讨论中受益。

特别感谢 Danilo 和 James 告诉我关于 `*{pacman}*` *和* `*{librarian}*` *包的信息。*

**相关文章:**

*   我的数据符合正态分布吗？关于最广泛使用的分布以及如何检验 R 中的正态性的注释
*   [R 中的 Fisher 精确检验:小样本的独立性检验](https://www.statsandr.com/blog/fisher-s-exact-test-in-r-independence-test-for-a-small-sample/)
*   [R 中独立性的卡方检验](https://www.statsandr.com/blog/chi-square-test-of-independence-in-r/)
*   [如何在简历中创建简历时间线](https://www.statsandr.com/blog/how-to-create-a-timeline-of-your-cv-in-r/)

*原载于 2020 年 1 月 31 日*[*https://statsandr.com*](https://statsandr.com/blog/an-efficient-way-to-install-and-load-r-packages/)*。*