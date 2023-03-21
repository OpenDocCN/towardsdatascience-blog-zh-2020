# 教程:用 23 行 Python 在几秒钟内刮出 100 个标题

> 原文：<https://towardsdatascience.com/tutorial-scrape-100-headlines-in-seconds-with-23-lines-of-python-14047deb1a98?source=collection_archive---------32----------------------->

## Scrapy 库的网页抓取快速，简单，而且非常强大。

![](img/5fb83e5f0fff82e32eef49da810c6635.png)

照片由[本斯·巴拉-肖特纳](https://unsplash.com/@benballaschottner?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)在 [Unsplash](https://unsplash.com/s/photos/spider-web?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) 拍摄

如果你需要做任何类型的网页抓取，Scrapy 几乎是不可能错过的。借助并行请求、用户代理欺骗、robots.txt 策略等内置特性，您只需几行代码就可以构建一个强大的 web scraper。

在本教程中，我将展示如何创建一个基本的 Scrapy 蜘蛛，收集新闻文章的标题。在大多数网站上，你可以做到这一点，而不用担心付费墙、僵尸检测或奇怪的解析技巧，所以我将把这些担心留给另一个教程。

## 数据

[这里的](https://gist.github.com/jackbandy/208028b404d8c6a6f822397e306a5a34)是一个有 100 个随机 URL 的文件，来自我从 [NewsAPI](https://newsapi.org) 收集的数据集。前几行如下所示:

## Python 库

我们将使用三个开源库(及其依赖项): [Pandas](https://github.com/pandas-dev/pandas) 、 [Scrapy](https://github.com/scrapy/scrapy) 和 [Readability](https://github.com/buriy/python-readability) 。假设您已经安装了[pip](https://pip.pypa.io/en/stable/installing/)，您可以通过在终端中运行以下命令来确保您的计算机已经安装了这些:

```
pip install scrapy pandas readability-lxml
```

然后，创建要组织项目的文件夹，并导航到该文件夹:

```
mkdir headline_scraper
cd headline_scraper
```

现在，我们可以创建蜘蛛。Scrapy 有一个`startproject`命令可以设置整个项目，但是我发现它可能会很臃肿。我们将通过制作我们自己的蜘蛛来保持简单，正如所承诺的，它只有 23 行(包括注释和空白)🙂).以下是一个名为`headline_scraper.py`的文件的内容:

与普通的 python 脚本不同，我们需要使用 scrapy 的`runspider`命令来运行文件。使用`-o`标志选择保存输出的位置:

```
scrapy runspider headline_scraper.py -o scraped_headlines.csv
```

就这些了！下面是对代码中发生的事情的解释。

# 遍历代码

## 初始化

除了导入库和声明`PATH_TO_DATA`(指向存储 100 个 URL 的[要点](https://gist.github.com/jackbandy/208028b404d8c6a6f822397e306a5a34)的链接)之外，下面是最初几行要做的事情:

*   `class HeadlineSpider(scrapy.Spider)`创建了一个新的类“HeadlineSpider”，它继承了 scrapy [蜘蛛](https://docs.scrapy.org/en/latest/topics/spiders.html)的基本功能
*   `name="headline_spider"`将新蜘蛛命名为“头条 _ 蜘蛛”，供 Scrapy 参考
*   `start_urls=read_csv(PATH_TO_DATA).url.tolist()`使用 pandas 的 csv 阅读器下载包含 100 个 url 的文件，然后获取“URL”列并将其转换为 python 列表。`start_urls`是任何蜘蛛都需要的，但在这种情况下，它们是唯一会被访问的网址。

(如果您想使用具有自己的 URL 的不同文件，只需用您文件的位置替换`PATH_TO_DATA`。然后，确保 url 列被标记为“URL”，或者将`.url`替换为`.name_of_your_column`，它可以在您的机器上，即`PATH_TO_DATA=/Users/Jack/Desktop/file.csv`

现在我们已经初始化了蜘蛛，我们需要告诉它当它到达一个网页时实际要做什么。

## 从语法上分析

要真正提取标题，我们必须实现`parse`方法。每当 Scrapy 从列表`start_urls`中检索到一个 url 时，它会自动向这个方法发送 http 响应，然后完全由您决定如何处理它。这个版本的功能如下:

*   `doc=Document(response.text)`这一行使用[可读性的](https://github.com/buriy/python-readability)功能来解析 html。你也可以使用其他的库，比如 [BeautifulSoup](https://www.crummy.com/software/BeautifulSoup/) 或者[报纸](https://github.com/codelucas/newspaper/)来获取这个部分。
*   `yield`本质上是一个创建输出的 return 方法，您可以把它想象成生成的电子表格中的一行。
*   `'short_title':doc.short_title()`而它下面的几行只是简单的设置了返回对象的不同属性。您可以将`'short_title'`视为结果电子表格中的一列，内容将是`doc.short_title()`的输出(例如，“冠状病毒爆发如何影响全球经济”)

同样，这部分是非常可定制的。如果您想尝试获得文章的全文，只需更改 yield 行以包含如下的`doc.summary()`。

```
yield {
    'full_text': doc.summary(),
    'short_title': doc.short_title(),
    'full_title': doc.title(),
    'url': response.url
}
```

这将在结果中增加一个名为`full_text`的列。csv 文件，可读性最好的提取文章文本的尝试。

# 结果

下面是运行`scrapy runspider headline_scraper.py -o scraped_headlines.csv:`的输出文件

嘣！至少大部分情况下。Scrapy 是非常容错的，所以只有 95/100 的 URL 返回。没有回来的 5 遇到了某种 http 错误，就像你有时在浏览网页时看到的 404 错误。此外，一些行(我数了 7 行)写着“警告”和“你是机器人吗？”而不是提供真实的标题。(彭博似乎有非常有效的机器人检测。)

有许多策略来处理 bot 检测，其中一些已经在 Scrapy 中实现。如果您感兴趣，请告诉我，我可以在以后的教程中介绍这些策略。

![](img/5f311e9cd8ca64c586bde09a320da3f3.png)

照片由 [Divyadarshi Acharya](https://unsplash.com/@lincon_street?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) 在 [Unsplash](https://unsplash.com/s/photos/web-scrape?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) 上拍摄

*如果您对代码有任何疑问、意见或问题，欢迎回复本文或在*[*Twitter*](https://twitter.com/jackbandy)*上给我发消息。谢谢！*