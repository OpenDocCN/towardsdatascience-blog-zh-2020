# 新冠肺炎隔离期间免费下载施普林格书籍的软件包

> 原文：<https://towardsdatascience.com/a-package-to-download-free-springer-books-during-covid-19-quarantine-6faaa83af13f?source=collection_archive---------35----------------------->

## 查看如何下载在新冠肺炎隔离期间免费提供的所有(或部分)Springer 书籍

![](img/c66e372b0dd23f94443da57620b0eaa3.png)

斯坦尼斯拉夫·康德拉蒂耶夫拍摄的照片

**更新:促销活动已经结束，因此无法通过 r 下载图书。如果您没有及时下载图书，您仍然可以通过此** [**链接**](https://drive.google.com/drive/folders/1JC15m__PbPaowQ7k2zS1-Us72yvROCQs) **获得图书。**

# 介绍

你可能已经看到斯普林格继新冠肺炎·疫情之后免费发行了大约 500 本书。据斯普林格称，这些教科书将至少免费提供到 7 月底。

在这个声明之后，我已经从他们的网站上下载了几本统计学和 R 编程的教科书，我可能会在接下来的几周内再下载一些。

在这篇文章中，我展示了一个为我节省了大量时间的包，它可能会引起我们很多人的兴趣:由[雷南·泽维尔·科尔特斯](http://renanxcortes.github.io/)开发的`[{springerQuarantineBooksR}](https://github.com/renanxcortes/springerQuarantineBooksR)` [包](https://github.com/renanxcortes/springerQuarantineBooksR)。 [1](https://www.statsandr.com/blog/a-package-to-download-free-springer-books-during-covid-19-quarantine/#fn1)

这个软件包可以让你轻松下载新冠肺炎隔离期间免费提供的所有(或部分)Springer 书籍。

有了这么多高质量的资源和我收集的关于冠状病毒的[顶级 R 资源，我们没有任何借口在隔离期间不阅读和学习。](https://www.statsandr.com/blog/top-r-resources-on-covid-19-coronavirus/)

在这篇文章中，我展示了:

*   如何一次性下载所有可用的教科书
*   给定特定的标题、作者或主题，如何下载书籍的子集

事不宜迟，下面是这个包在实践中的工作方式。

# 装置

安装完`{devtools}`包后，您可以从 GitHub 安装`{springerQuarantineBooksR}`包并加载:

```
# install.packages("devtools")
devtools::install_github("renanxcortes/springerQuarantineBooksR")
library(springerQuarantineBooksR)
```

# 一次下载所有书籍

首先，用`setwd()`功能设置保存所有书籍的路径，然后用`download_springer_book_files()`功能一次性下载所有书籍。请注意，这需要几分钟时间(取决于您的互联网连接速度)，因为所有书籍加起来差不多有 8GB。

```
setwd("path_of_your_choice") # where you want to save the books
download_springer_book_files() # download all of them at once
```

你会在一个名为“springer_quarantine_books”的文件夹里找到所有下载的书籍(PDF 格式)，按类别整理。 [2](https://www.statsandr.com/blog/a-package-to-download-free-springer-books-during-covid-19-quarantine/#fn2)

如果您想下载 EPUB 版本(或 PDF 和 EPUB 版本)，请在函数中添加`filetype`参数:

```
# for EPUB version:
download_springer_book_files(filetype = "epub")# for both PDF and EPUB versions:
download_springer_book_files(filetype = "both")
```

默认情况下，它只下载英文书籍。但是，也可以通过添加参数`lan = 'ger'`来下载所有德语书籍:

```
download_springer_book_files(lan = 'ger')
```

请注意，总共有 407 个独特的英语标题和 52 个德语标题。

# 创建一个斯普林格图书表

像我一样，如果你不知道 Springer 提供了哪些书，也不想下载所有的书，你可能想在下载任何书之前有一个已发行书籍的概述或列表。为此，您可以使用`download_springer_table()`函数将包含 Springer 提供的所有图书的表格加载到 R 会话中:

```
springer_table <- download_springer_table()
```

然后可以用`{DT}`包改进该表，以:

*   只保留最少的信息，
*   允许按书名、作者、分类或年份搜索书籍，
*   允许下载可用书籍列表，以及
*   例如，使 Springer 链接可点击

```
# install.packages("DT")
library(DT)springer_table$open_url <- paste0(
  '<a target="_blank" href="', # opening HTML tag
  springer_table$open_url, # href link
  '">SpringerLink</a>' # closing HTML tag
)springer_table <- springer_table[, c(1:3, 19, 20)] # keep only relevant informationdatatable(springer_table,
  rownames = FALSE, # remove row numbers
  filter = "top", # add filter on top of columns
  extensions = "Buttons", # add download buttons
  options = list(
    autoWidth = TRUE,
    dom = "Blfrtip", # location of the download buttons
    buttons = c("copy", "csv", "excel", "pdf", "print"), # download buttons
    pageLength = 5, # show first 5 entries, default is 10
    order = list(0, "asc") # order the title column by ascending order
  ),
  escape = FALSE # make URLs clickable
)
```

该表不能在介质上导入，所以参见[原始帖子](https://www.statsandr.com/blog/a-package-to-download-free-springer-books-during-covid-19-quarantine/)中的表。供您参考，该表允许您查看 Springer 提供的教科书(以及一些信息)，并允许您找到您最有可能感兴趣的教科书。

请注意，您可以使用`download_springer_table(lan = "ger")`函数为德语书籍创建一个类似的表。

# 仅下载特定的书籍

现在你对你感兴趣的书有了更好的了解，你可以按书名、作者或主题下载它们。

## 按标题

假设您只对下载一本特定的书感兴趣，并且您知道它的书名。例如，假设您想下载名为《所有统计学》的书:

```
download_springer_book_files(springer_books_titles = "All of Statistics")
```

如果您有兴趣下载多本书，请运行以下命令:

```
download_springer_book_files(
  springer_books_titles = c(
    "All of Statistics",
    "A Modern Introduction to Probability and Statistics"
  )
)
```

或者，如果您没有特定的书名，但是您有兴趣下载书名中带有“统计学”一词的所有书籍，您可以运行:

```
springer_table <- download_springer_table()library(dplyr)
specific_titles_list <- springer_table %>%
  filter(str_detect(
    book_title, # look for a pattern in the book_title column
    "Statistics" # specify the word
  )) %>%
  pull(book_title)download_springer_book_files(springer_books_titles = specific_titles_list)
```

**提示:**如果您想下载标题中带有“统计学”或“数据科学”字样的所有书籍，请将上述代码中的`"Statistics"`替换为`"Statistics|Data Science"`。

## 按作者

如果您想要下载特定作者的所有书籍，您可以运行:

```
springer_table <- download_springer_table()# library(dplyr)
specific_titles_list <- springer_table %>%
  filter(str_detect(
    author, # look for a pattern in the author column
    "John Hunt" # specify the author
  )) %>%
  pull(book_title)download_springer_book_files(springer_books_titles = specific_titles_list)
```

## 按主题

您还可以下载涵盖某一特定科目的所有教材(参见汇总表中`subject_classification`栏中的所有科目)。例如，以下是如何下载“统计学”主题的所有书籍:

```
springer_table <- download_springer_table()# library(dplyr)
specific_titles_list <- springer_table %>%
  filter(str_detect(
    subject_classification, # look for a pattern in the subject_classification column
    "Statistics" # specify the subject
  )) %>%
  pull(book_title)download_springer_book_files(springer_books_titles = specific_titles_list)
```

# 丰富

下面列出了为改进套件而可能实施的功能:

*   增加了下载一本书所有版本的可能性。目前，只能下载最新版本。
*   增加了下载停止后继续下载的可能性。目前，如果再次执行代码，下载将从头开始。
*   增加了按题目下载书籍的可能性。目前只能通过[标题](https://www.statsandr.com/blog/a-package-to-download-free-springer-books-during-covid-19-quarantine/#by-title)、[作者](https://www.statsandr.com/blog/a-package-to-download-free-springer-books-during-covid-19-quarantine/#by-author)或[主题](https://www.statsandr.com/blog/a-package-to-download-free-springer-books-during-covid-19-quarantine/#by-subject)来实现。

如果您有其他改进的想法，请随时在 GitHub 上打开一个 pull 请求。

# 感谢

我要感谢:

*   雷南泽维尔科尔特斯(和所有贡献者)提供了这个包
*   被用作`{springerQuarantineBooksR}`包灵感的`[springer_free_books](https://github.com/alexgand/springer_free_books)` Python 项目
*   最后但同样重要的是，斯普林格免费提供他们的许多优秀书籍！

感谢阅读。我希望这篇文章能帮助你下载和阅读更多新冠肺炎检疫期间斯普林格提供的高质量材料。

和往常一样，如果您有与本文主题相关的问题或建议，请将其添加为评论，以便其他读者可以从讨论中受益。

1.  我感谢作者允许我在博客中展示他的包。 [↩](https://www.statsandr.com/blog/a-package-to-download-free-springer-books-during-covid-19-quarantine/#fnref1)
2.  请注意，您可以通过指定参数`destination_folder = "name_of_your_choice"`来更改文件夹名称。 [↩](https://www.statsandr.com/blog/a-package-to-download-free-springer-books-during-covid-19-quarantine/#fnref2)

# 相关文章

*   [比利时的新冠肺炎](https://www.statsandr.com/blog/covid-19-in-belgium/)
*   [如何创建针对您所在国家的简单冠状病毒仪表板](https://www.statsandr.com/blog/how-to-create-a-simple-coronavirus-dashboard-specific-to-your-country-in-r/)
*   [新型新冠肺炎冠状病毒前 50 名资源](https://www.statsandr.com/blog/top-r-resources-on-covid-19-coronavirus/)
*   [如何在 R 中创建带有自动亚马逊附属链接的互动书单？](https://www.statsandr.com/blog/how-to-create-an-interactive-booklist-with-automatic-amazon-affiliate-links-in-r/)
*   [如何在 R 中一次对多个变量进行 t 检验或方差分析，并以更好的方式传达结果](https://www.statsandr.com/blog/how-to-do-a-t-test-or-anova-for-many-variables-at-once-in-r-and-communicate-the-results-in-a-better-way/)

*原载于 2020 年 4 月 26 日*[*https://statsandr.com*](https://statsandr.com/blog/a-package-to-download-free-springer-books-during-covid-19-quarantine/)*。*