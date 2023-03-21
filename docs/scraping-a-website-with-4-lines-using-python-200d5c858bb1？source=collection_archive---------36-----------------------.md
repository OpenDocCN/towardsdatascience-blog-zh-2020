# 用 Python 实现 4 行新闻的网络抓取

> 原文：<https://towardsdatascience.com/scraping-a-website-with-4-lines-using-python-200d5c858bb1?source=collection_archive---------36----------------------->

## 抓取网站的简单方法

![](img/523fcfdd1b27f0c494d8d97a70340dee.png)

由 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上的[absolute vision](https://unsplash.com/@freegraphictoday?utm_source=medium&utm_medium=referral)拍摄

在本文中，我将向您展示如何从各种来源收集和删除新闻。所以，与其花很长时间为每个网站写抓取代码，不如用 [**newspaper3k**](https://newspaper.readthedocs.io/en/latest/) 自动提取结构化信息。如果你不愿意看这篇文章，你可以在 github 的[](https://github.com/fahmisalman/News-Scraping)**上看到我的完整代码。**

**让我们开始吧，第一步是安装将要使用的包，即使用 pip 的 newspaper3k。打开终端(Linux / macOS)或命令提示符(windows)并键入:**

```
$ pip install newspaper3k
```

**安装完成后，打开代码编辑器，导入包含以下代码的包**

```
>>> from newspaper import Article
```

**在这篇文章中，我将从《纽约时报》的标题为' [*的*](https://www.nytimes.com/2020/05/10/us/ahmaud-arbery-georgia.html) *[**中摘抄一条新闻，标题为“一生与**](https://www.nytimes.com)* *艾哈迈德·阿贝里一起奔跑”，结尾是一段文字:“我们失去了莫德* *'* ”。**

**接下来，输入要抓取的链接**

```
>>> article = Article('https://www.nytimes.com/2020/05/10/us/ahmaud-arbery-georgia.html’)
```

**您可以选择使用哪种语言。即便如此，报纸也能相当好地检测和提取语言。如果没有给出具体的语言，报纸会自动检测语言。**

**但是，如果您想使用一种特定的语言，请将代码更改为。**

```
>>> article = Article(‘[https://www.nytimes.com/2020/05/10/us/ahmaud-arbery-georgia.html'](https://www.nytimes.com/2020/05/10/us/ahmaud-arbery-georgia.html'), ‘en’) # English
```

**然后用下面的代码解析文章**

```
>>> article.download()>>> article.parse()
```

**现在一切都准备好了。我们可以开始使用几种方法来提取信息，从文章作者开始。**

```
>>> article.authors['Richard Fausset']
```

**接下来，我们将获得文章发表的日期。**

```
>>> article.publish_datedatetime.datetime(2020, 5, 10, 0, 0)
```

**从文章中获取全文。**

```
>>> article.text‘Mr. Baker was with Mr. Arbery’s family at his grave site . . .’
```

**你也可以从文章中获取图片链接。**

```
>>> article.top_image'https://static01.nyt.com/images/2020/05/10/us/10georgia-arbery-1/10georgia-arbery-1-facebookJumbo-v2.jpg'
```

**此外，newspaper3k 还提供了简单文本处理的方法，例如获取关键字和摘要。首先，使用初始化。nlp()方法。**

```
>>> article.nlp()
```

**获取关键字。**

```
>>> article.keywords['running',
 'world',
 'needed',
 'arbery',
 'lifetime',
 'site',
 'baker',
 'text',
 'wished',
 'maud',
 'arberys',
 'lost',
 'pandemic',
 'upended',
 'ends',
 'ahmaud',
 'unsure',
 'mr']
```

**接下来是新闻摘要，但是该功能仅限于几种语言，如[文档](https://newspaper.readthedocs.io/en/latest/)中所述。**

```
>>> article.summary'Mr. Baker was with Mr. Arbery’s family at his grave site.\nIt was Mr. Arbery’s birthday.\nA pandemic had upended the world, and Mr. Baker felt adrift.\nSometimes he wished to be called a musician, and sometimes he did not.\nHe was unsure what he would become or how he would get there.'
```

**这篇文章的结论是，与我们必须花很长时间为每个网站编写抓取代码相比，使用 newspaper3k 包，我们可以很容易地在 python 中抓取各种来源的新闻。通过使用这个包，我们可以从新闻中检索所需的基本信息，如作者、出版日期、文本和图像。还有一些获取新闻关键词和摘要的方法。最重要的是，newspaper3k 可以用各种语言搜集新闻。**