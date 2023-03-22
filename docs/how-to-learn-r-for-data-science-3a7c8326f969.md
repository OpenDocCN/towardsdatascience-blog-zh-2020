# R 编程语言:简介

> 原文：<https://towardsdatascience.com/how-to-learn-r-for-data-science-3a7c8326f969?source=collection_archive---------45----------------------->

## 约翰·霍普斯金 DS 专业化系列

## 什么是 R，R 与 Python，以及学习 R 的最好方法

![](img/ce522877d5b1c3f7a3bbc116c4273483.png)

由 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上 [Greg Rakozy](https://unsplash.com/@grakozy?utm_source=medium&utm_medium=referral) 拍摄的照片

```
[Full series](https://towardsdatascience.com/tagged/ds-toolbox)[**Part 1**](/the-data-scientists-toolbox-part-1-c214adcc859f) - What is Data Science, Big data and the Data Science process[**Part 2**](/how-to-learn-r-for-data-science-3a7c8326f969) - The origin of R, why use R, R vs Python and resources to learn[**Part 3**](/a-crash-course-on-version-control-and-git-github-5d04e7933070) - Version Control, Git & GitHub and best practices for sharing code.[**Part 4**](/the-six-types-of-data-analysis-75517ba7ea61) - The 6 types of Data Analysis[**Part 5**](/designing-experiments-in-data-science-23360d2ddf84) - The ability to design experiments to answer your Ds questions[**Part 6**](/what-is-a-p-value-2cd0b1898e6f) - P-value & P-hacking[**Part 7**](/big-data-its-benefits-challenges-and-future-6fddd69ab927) - Big Data, it's benefits, challenges, and future
```

*本系列基于约翰·霍普斯金大学在 Coursera 上提供的* [*数据科学专业*](https://www.coursera.org/specializations/jhu-data-science) *。本系列中的文章是基于课程的笔记，以及出于我自己学习目的的额外研究和主题。第一门课，* [*数据科学家工具箱*](https://www.coursera.org/learn/data-scientists-tools) *，笔记会分成 7 个部分。关于这个系列的注释还可以在这里找到*[](http://sux13.github.io/DataScienceSpCourseNotes/)**。**

## *介绍*

*如果您是 R 编程语言的新手，或者您想了解更多，那么这篇文章是为您准备的，受 Coursera 上的 DS 工具箱课程的启发，我将简要说明 R 在数据科学中的使用，包括关于它的重要性以及它与 Python 相比如何的细节。在文章的最后，我还会分享一些学习 r 的资源。希望你喜欢阅读这篇文章！*

# *R 编程语言的起源*

*许多编程语言的创造都是受手头某个特定问题的启发，一般来说，是为了让编程更直观。对 R 来说，它是为统计学家建立一种更容易理解的语言。当时有一种高度复杂的语言叫做 SAS，是为统计学家设计的，但是语法和功能太难使用了。两位杰出的开发人员看到了这个机会，并创造了 R，这是 SAS 的一个免费、开源的替代品，更容易编写和简化。*

# *R 是什么？*

*快速浏览一下维基百科就能明白这一点*

> *统计学家和数据挖掘者广泛使用 R 语言来开发统计软件和数据分析*

*定义是不言自明的，R 是一种面向统计分析的简单语言——就像 Flutter 用于构建移动应用程序，React 用于漂亮的网站——一般来说，R 提供了一种简单的方法来获取数据，并将其转化为有用的统计数据、令人敬畏的图表和用于预测和推断的统计学习模型。*

*今天，R 已经不是学术环境中统计学家的语言了，它有无数的扩展来服务于不同领域的不同目的。例如，它可以用于工程、营销、金融、保险等等。*

# *为什么用 R？*

*本课程主要给出了 4 个原因，分别是:*

## *1.流行*

*r 是统计分析的典型语言，随着其功能的不断增加和更新，以及数据科学的蓬勃发展，它已经成为数据科学家的顶级语言。*

## *2.免费和开源*

*像大多数语言一样，它是开源的，可以免费使用。像 IBM SPSS 这样的统计软件是要花钱的，所以让所有人都可以使用 R 是件好事。*

## *3.广泛的功能*

*r 真的很万能。除了统计和绘图，它的广泛功能包括制作网站、分析语言等等。有了正确的包，你几乎可以用 R 做任何你想做的事情。*

## *4.巨大的社区*

*像 Python 一样，它是开源的，有大量扩展功能的包。这样做的一个好处是，当你在 R 中遇到问题时，你可以去论坛寻求帮助。*

# *R 怎么用？*

## *安装 R*

*你可以在 [CRAN](https://cloud.r-project.org) 下载 R，它代表 **C** 综合**R**AC**N**网络*

## *RStudio*

*R 的官方 IDE 是 [RStudio](http://www.rstudio.com/download) ，它让 R 编程变得超级简单有趣。*

## *TidyVerse*

*鉴于数据科学的繁荣， [Tidyverse](https://www.tidyverse.org) 应运而生。这是一个供数据科学使用的 R 包集合，它扩展了基础的功能。如果你已经有编程背景，并且正在学习数据科学的 R，那么使用 TidyVerse 是理想的。但是，如果没有编程基础，您应该先学习 base R，然后再学习 TidyVerse。*

# *R vs Python*

*r 和 Python 是数据科学中的两大巨头，出现在许多热烈的讨论中。我将简单介绍一下要点，并在后面的参考资料中链接更详细的比较。*

*R 是为统计学家打造的，所以 R 将在统计分析和建模方面占据上风，特别是 TidyVerse 包——用 ggplot2 进行数据操作和绘图，以及用 R notebook 和 RStudio 中的 Markdown 生成报告。然而，它确实有一个陡峭的学习曲线*

*Python 是一种更通用的语言，是万能的。它在生产和其他方面，如软件开发，建立网站，机器学习和深度学习(PyTorch)方面具有优势。*

*由于这两种语言各有利弊，大多数人通常会出于不同的目的使用这两种语言，这取决于他们的专业和用例。*

## *那么，应该学哪个呢？*

*有许多不同的意见，你应该学习。但是我认为最重要的事情是首先掌握编程的基础，因为所有的编程语言都是在编程和计算机科学的基本概念下工作的。*

*即使在那之后，你决定先学习哪种语言并不那么重要，R 和 Python 都被广泛采用和使用，你将使用哪种语言取决于你的专业和你做的项目。因此，**开始学习**是个好主意，这样你就不会浪费时间去研究、思考和实际掌握技能来保障你的未来。*

*缩小范围的一种方法是确定你在数据科学领域的首选角色，通过研究你的理想工作，你应该能够了解你选择的公司首选什么语言。假设你想成为一名数据分析师，你应该专注于拥有尖端软件包的 R。但是如果你更喜欢构建模型和生产代码，你应该关注 Python。*

# *我如何学习 R？*

*约翰·霍普斯金大学提供的[数据科学专业](https://www.coursera.org/specializations/jhu-data-science)是一个很好的起点，因为它为你提供了你需要了解的关于数据科学基础知识的一切，并让你从零到 R 中的英雄。除了这门课程，你还可以从数据科学的书[**R**](https://r4ds.had.co.nz)**开始。***

*为了更好地理解机器学习和统计，你也可以阅读《统计学习简介》这本书，这本书教你用 R 语言构建各种统计学习模型，但前提是你必须在数据科学专业化方面取得相当大的进展。*

*除此之外，你还应该经常利用互联网上的大量资源——比如 YouTube 教程、文章等。—并且总是尝试真正巩固和实用化你所学的东西，记笔记(手写笔记以便更好地保留)，并在学习会话后用代码复制你所学的内容。*

*对我来说，在线学习最重要的一课是，你的目标不应该是证书或炫耀你在简历上获得的新技能，而是学习的**技能和**学习如何学习**，这在这个快节奏的世界中非常重要，因为数据科学的最新工具和技术可能会在未来几年发生变化。***

*这篇学习 R 的指南并不是最好的**当然，这只是我个人学习 R 的计划，我希望你会发现这也是学习用 R 写代码的技巧的好方法！我希望你学习愉快。***

# *摘要*

*![](img/572a9f2b47cf3c9a5d8b64f86612ced5.png)*

*照片由 [Jonatan Pie](https://unsplash.com/@r3dmax?utm_source=medium&utm_medium=referral) 在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄*

*总之，对于统计分析和建模来说，R 是一种很棒的语言，至今仍被许多人使用。决定学习并坚持学习可能是你一生中最好的决定之一。我目前正在学习 R，我认为这是一门很好的语言，我迫不及待地想做好模型和漂亮的图形。*

*学习数据科学有时会非常困难，你会觉得自己不够好(冒名顶替综合症)。保持你的动机和决心的一种方法是记住你为什么首先学习它，对我个人来说——在这个数据科学和人工智能时代确保你的未来，并在这个过程中获得惊人的技能。*

*你还必须明白，学习它是一个终生的旅程和过程，它不是一个四年制的本科学位就能获得的。一张纸上的分数并不代表你的心态和哲学，也不能证明你的技能和能力(在现实世界中)。这个领域总是在变化，更重要的是要适应并专注于学习如何学习，而不是教科书上的定义和方法。*

*正如 ***苏格拉底*** 所说:*

> *"教育是点燃火焰，而不是填满容器."*

*以及 ***爱因斯坦*** :*

> *"智力的发展应该从出生开始，只有在死亡时才停止."*

*因此，如果你一直在考虑成为一名数据科学家，学习 R 是一个很好的起点。祝你一切顺利。*

*感谢您的阅读，我希望这篇文章是有教育意义的，让您对 R 编程语言有所了解。*

# *R 的资源*

*[](https://github.com/qinwf/awesome-R/blob/master/README.md) [## qinwf/awesome-R

### awesome R 包和工具的精选列表。灵感来自令人敬畏的机器学习。下载量排名前 50 的 CRAN

github.com](https://github.com/qinwf/awesome-R/blob/master/README.md) [](https://www.dataquest.io/blog/learn-r-for-data-science/) [## 通过 5 个步骤正确学习 R——在 Dataquest 学习数据科学

### r 是一种越来越流行的编程语言，尤其是在数据分析和数据科学领域。但是…

www.dataquest.io](https://www.dataquest.io/blog/learn-r-for-data-science/) 

## r 代表数据科学

[](https://r4ds.had.co.nz) [## r 代表数据科学

### 这本书将教你如何用 R 做数据科学:你将学习如何把你的数据放入 R，把它放入最…

r4ds.had.co.nz](https://r4ds.had.co.nz) 

# Python vs R

来源:[数据营团队](https://medium.com/u/e18542fdcc02?source=post_page-----3a7c8326f969--------------------------------)

[](https://www.datacamp.com/community/blog/when-to-use-python-or-r) [## Python vs. R for Data Science:有什么区别？

### 如果你是数据科学的新手，或者你的组织是新手，你需要选择一种语言来分析你的数据和一个…

www.datacamp.com](https://www.datacamp.com/community/blog/when-to-use-python-or-r) 

来源:[数据驱动科学](https://medium.com/u/31663dbeb217?source=post_page-----3a7c8326f969--------------------------------)

[](https://medium.com/@datadrivenscience/python-vs-r-for-data-science-and-the-winner-is-3ebb1a968197) [## Python 对 R 的数据科学:赢家是..

### 关于:数据驱动科学(DDS)为在人工智能(AI)领域建立职业生涯的人提供培训。跟随…

medium.com](https://medium.com/@datadrivenscience/python-vs-r-for-data-science-and-the-winner-is-3ebb1a968197) 

## 如果您对学习数据科学感兴趣，请查看“超学习”数据科学系列！

[](https://medium.com/better-programming/how-to-ultralearn-data-science-part-1-92e143b7257b) [## 如何“超级学习”数据科学—第 1 部分

### 这是一个简短的指南，基于《超学习》一书，应用于数据科学

medium.com](https://medium.com/better-programming/how-to-ultralearn-data-science-part-1-92e143b7257b) 

## 查看这些关于数据科学资源的文章。

[](/top-20-youtube-channels-for-data-science-in-2020-2ef4fb0d3d5) [## 2020 年你应该订阅的 25 大数据科学 YouTube 频道

### 以下是你应该关注的学习编程、机器学习和人工智能、数学和数据的最佳 YouTubers

towardsdatascience.com](/top-20-youtube-channels-for-data-science-in-2020-2ef4fb0d3d5) [](/top-20-free-data-science-ml-and-ai-moocs-on-the-internet-4036bd0aac12) [## 互联网上 20 大免费数据科学、ML 和 AI MOOCs

### 以下是关于数据科学、机器学习、深度学习和人工智能的最佳在线课程列表

towardsdatascience.com](/top-20-free-data-science-ml-and-ai-moocs-on-the-internet-4036bd0aac12) [](/top-10-popular-github-repositories-to-learn-about-data-science-4acc7b99c44) [## 了解数据科学的十大热门 GitHub 存储库。

### 以下是 GitHub 上一些关于数据科学的最佳资源。

towardsdatascience.com](/top-10-popular-github-repositories-to-learn-about-data-science-4acc7b99c44) [](/top-20-data-science-discord-servers-to-join-in-2020-567b45738e9d) [## 2020 年将加入的 20 大数据科学不和谐服务器

### 以下是 Discord 上用于数据科学的最佳服务器列表

towardsdatascience.com](/top-20-data-science-discord-servers-to-join-in-2020-567b45738e9d) [](/33-data-scientists-to-follow-on-twitter-77f70c59339f) [## 2020 年在 Twitter 上关注的顶级数据科学家

### 这里有一个你应该关注的杰出数据科学家的列表。

towardsdatascience.com](/33-data-scientists-to-follow-on-twitter-77f70c59339f) 

# 联系人

如果你想了解我的最新文章[，请通过媒体](https://medium.com/@benthecoder07)关注我。

也关注我的其他社交资料！

*   [领英](https://www.linkedin.com/in/benthecoder/)
*   [推特](https://twitter.com/benthecoder1)
*   [GitHub](https://github.com/benthecoder)
*   [Reddit](https://www.reddit.com/user/benthecoderX)

请关注我的下一篇文章，记得**注意安全**！*