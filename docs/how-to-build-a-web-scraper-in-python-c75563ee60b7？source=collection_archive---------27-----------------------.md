# 如何用 Python 构建 Web Scraper

> 原文：<https://towardsdatascience.com/how-to-build-a-web-scraper-in-python-c75563ee60b7?source=collection_archive---------27----------------------->

## 快速抓取、汇总谷歌搜索引擎结果

![](img/bc0a192c647e9243d73f864f93a352ae.png)

照片由[像素](https://www.pexels.com/photo/pattern-branches-tree-design-39494/?utm_content=attributionCopyText&utm_medium=referral&utm_source=pexels)的[皮克斯拜](https://www.pexels.com/@pixabay?utm_content=attributionCopyText&utm_medium=referral&utm_source=pexels)拍摄

# 网页抓取

网络抓取是分析师筛选和收集大量公共数据的绝佳工具。通过使用与讨论主题相关的关键词，一个好的网页抓取器可以非常快速地收集大量数据，并将其聚集成一个数据集。Python 中有几个库使得这一点非常容易实现。在这篇文章中，我将展示一个架构，我一直用它来抓取和总结搜索引擎数据。这篇文章将被分成以下几个部分…

*   链接抓取
*   内容抓取
*   内容总结
*   建设管道

所有的代码都将在这里提供。

# 链接抓取

首先，我们需要一种方法来收集与我们收集数据的主题相关的 URL。幸运的是，Python 库 googlesearch 使得收集响应初始 google 搜索的 URL 变得很容易。让我们构建一个类，使用这个库来搜索我们的关键字，并将固定数量的 URL 附加到一个列表中，以便进一步分析…

# 内容抓取

这可以说是 web scraper 最重要的部分，因为它决定了网页上的哪些数据将被收集。使用 urllib 和 beautiful soup (bs4)的组合，我们能够在链接抓取器类中检索和解析每个 URL 的 HTML。Beautiful soup 让我们指定想要从中提取数据的标签。在下面的例子中，我建立了一个 URL 请求，用 bs4 解析 HTML 响应，并存储在段落(

)标签中找到的所有信息…

# 内容总结

在这里，我们创建了一个文本摘要，它是从驻留在我们的内容抓取器中的每个页面的 HTML 中提取的。为此，我们将使用库的组合，主要是 NLTK。我们生成摘要的方式相对来说很简单，有很多方法可以改进这种方法，但这是一个很好的开始。在对填充词进行一些格式化和无效化之后，单词被标记化并按频率排序，生成一些旨在准确概括文章的句子…

# 建设管道

这是我们把所有东西放在一起的部分。一个类将根据需要实例化每个其他组件的实例，以构建和实现我们的 web scraper。WebScraper 类有几个参数…

*   **搜索** —字符串，搜索引擎查询
*   **n** —整数，要分析的 URL 源的数量
*   **sl**—整数，总结的句子长度
*   **fall_through** — Boolean，是否多线程化进程
*   **Write _ file**—布尔型，将摘要写入文件

现在让我们实例化并运行这个 WebScraper 类的一个实例…

```
Analyst(‘AAPL’, 10, 3, False, False)
```

运行前面的代码会产生以下输出…

```
Analyzing:  http://t1.gstatic.com/images?q=tbn:ANd9GcSjoU2lZ2eJX3aCMfiFDt39uRNcDu9W7pTKcyZymE2iKa7IOVaIAnalyzing:  https://en.wikipedia.org/wiki/Apple_Inc.
Analyzing:  https://www.bloomberg.com/news/articles/2020-08-26/apple-plans-augmented-reality-content-to-boost-tv-video-serviceAnalyzing:  https://www.marketwatch.com/story/apple-stock-rises-after-wedbush-hikes-target-to-new-street-high-of-600-2020-08-26Analyzing:  https://www.marketwatch.com/story/tesla-and-apple-have-had-a-great-run-heres-why-theyre-poised-to-rocket-even-higher-in-the-next-year-2020-08-26Analyzing:  https://finance.yahoo.com/quote/AAPL/Analyzing:  https://seekingalpha.com/article/4370830-apple-sees-extreme-bullishness
Analyzing:  https://seekingalpha.com/news/3608898-apples-newest-street-high-price-target-700-bull-caseAnalyzing:  https://www.marketwatch.com/investing/stock/aaplAnalyzing:  https://stocktwits.com/symbol/AAPLencoding error : input conversion failed due to input error, bytes 0x9D 0x09 0x96 0xA3
encoding error : input conversion failed due to input error, bytes 0x9D 0x09 0x96 0xA3Value Error***For more information you can review our Terms of Service and Cookie Policy.For inquiries related to this message please contact our support team and provide the reference ID below.***URL ErrorValue Error"China remains a key ingredient in Apple's recipe for success as we estimate roughly 20% of iPhone upgrades will be coming from this region over the coming year."
Ives points to recent signs of momentum in China, which he expects will continue for the next six to nine months.
Real-time last sale data for U.S. stock quotes reflect trades reported through Nasdaq only.***By comparison, Amazon AMZN, +0.58% has split its stock three times, rallying an average of 209% the following year.
Apple�s history isn�t quite as stellar as all those, with its four previous splits resulting in an average gain of 10.4% in the following year.
Real-time last sale data for U.S. stock quotes reflect trades reported through Nasdaq only.***The truck and fuel cell maker �could be a major horse in the EV race,� wrote the analyst, while voicing concerns about the stock�s valuation.
stocks edged lower Wednesday, a day after the S&P 500 set its first record close since February, after Federal Reserve officials highlighted the uncertainties facing the economy.
stock-market benchmarks mostly opened higher on Wednesday, pushing the key benchmarks to further records after an economic report came in better than expected.***Apple is the world's largest information technology company by revenue, the world's largest technology company by total assets, and the world's second-largest mobile phone manufacturer after Samsung.
Two million iPhones were sold in the first twenty-four hours of pre-ordering and over five million handsets were sold in the first three days of its launch.
The same year, Apple introduced System 7, a major upgrade to the operating system which added color to the interface and introduced new networking capabilities.***Howley also weighs in on Nintendo potentially releasing an upgraded Switch in 2021.***[Finished in 5.443s]
```

我们已经成功地从关于 AAPL 的热门搜索结果中提取了一些摘要。如控制台输出所示，一些站点阻止了这种类型的请求。然而，这是一个全面的 Python web 抓取入门指南。