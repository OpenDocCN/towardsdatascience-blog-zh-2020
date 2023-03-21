# 使用 Python 和 Newscatcher 构建您的第一条新闻数据管道

> 原文：<https://towardsdatascience.com/build-your-first-news-data-pipeline-with-python-newscatcher-5fa18b8c5083?source=collection_archive---------35----------------------->

## 新闻捕手

## Newscatcher python 包可以让你从 3000 多个主要的在线新闻网站自动收集最新的新闻数据。

![](img/f86c1173ac19ef3bc684b6458a3cc1f6.png)

照片由[昆腾·德格拉夫](https://unsplash.com/@quinten149?utm_source=medium&utm_medium=referral)在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 拍摄

当我写这篇文章时，许多人不得不在家工作，有些人在此期间有很多空闲时间。你可以利用这段时间建立你的投资组合，提高你的技能或者开始一个副业。

[Newscatcher](https://github.com/kotartemiy/newscatcher) 包使得新闻文章数据**的收集和标准化变得容易，而不需要任何外部依赖**。它是在我们开发名为 [Newscatcher API](https://newscatcherapi.com/) 的主要[数据即服务](https://www.safegraph.com/blog/data-as-a-service-bible-everything-you-wanted-to-know-about-running-daas-companies?source=search_post---------0)产品时构建的。我们是开发者优先的团队，因此，我们尽可能开源，以便编码人员可以免费部分复制我们已经完成的工作。

使用我们的包的方法很简单，你**传递网站 URL 作为输入，获取最新的文章作为输出**。对于每篇文章，您都有一个 ***标题、完整的 URL、简短的描述、发表日期、作者*** 和一些更多的变量。

# 快速启动

你需要 Python 3.7 以上版本

通过 PyPI 安装软件包:

```
pip install newscatcher
```

从包中导入 ***新闻捕捉器*** 类:

```
from newscatcher import Newscatcher
```

例如，我们想看看**nytimes.com**的最后一篇文章是什么

你必须传递 URL 的基本形式——没有`www.`，既没有`https://`，也没有在 URL 末尾的`/`。

```
news_source = Newscatcher(‘nytimes.com’)last_news_list = news_source.news
```

***news _ source . news***是 ***feedparser 的列表。FeedParserDict*** 。 [Feedparser](https://github.com/kurtmckee/feedparser) 是一个规范 RSS 提要的 Python 包。在我的另一篇中型文章中，我更详细地解释了什么是 RSS 以及如何使用 feedparser 处理它:

[](/collecting-news-articles-through-rss-atom-feeds-using-python-7d9a65b06f70) [## 使用 Python 通过 RSS/Atom 提要收集新闻文章

### 或者如何停止依赖数据提供商

towardsdatascience.com](/collecting-news-articles-through-rss-atom-feeds-using-python-7d9a65b06f70) 

需要知道的一件重要事情是**每个 RSS/Atom 提要可能有自己的一组属性**。尽管 feedparser 在结构化方面做得很好，但是不同的新闻发布者的文章属性可能会有所不同。

对于 nytimes.com 的**，物品属性列表如下:**

```
article = last_news_list[0]
article.keys()dict_keys(['title', 'title_detail', 'links', 'link', 'id', 'guidislink', 'summary', 'summary_detail', 'published', 'published_parsed', 'tags', 'media_content', 'media_credit', 'credit'])
```

在上面的代码中，我们获取名为 ***last_news_list*** 的列表中的第一篇文章，并检查它的键(每篇文章都是一个字典)。

当我们浏览主要属性时:

```
print(article.title)Coronavirus Live Updates: New York City Braces for a Deluge of Patients; Costs of Containment Growprint(article.summary)Soldiers in Spain found nursing home patients abandoned. Officials warned that New York was experiencing a virus “attack rate” of five times that elsewhere in the United States. Washington edged closer to a $2 trillion relief package.print(article.link)[https://www.nytimes.com/2020/03/24/world/coronavirus-updates-maps.html](https://www.nytimes.com/2020/03/24/world/coronavirus-updates-maps.html)print(article.published)Tue, 24 Mar 2020 10:35:54 +0000
```

最有可能的是，你会在所有新闻出版商的文章数据中找到上述所有属性。

**目前，最大的新闻出版商都在 Newscatcher 上。**因此，你可以建立一个端到端的数据项目。此外，您还可以做许多不同的事情，例如:

*   收集新闻文章并将其保存到某种数据库的管道(带重复数据删除)
*   +添加一个 NLP 层(例如，命名实体识别)
*   +跟踪某个特定的主题
*   +刮一篇文章的全文
*   +可视化汇总数据

我建议在使用 **newscatcher** 包时使用以下包/技术:

1.  [Elasticsearch](https://www.elastic.co/) 存储和查询数据
2.  [Newscaper3k](https://github.com/codelucas/newspaper) python 包自动抓取文章全文
3.  [NLP 的空间](https://github.com/explosion/spaCy)

给那些想提高投资组合的人一个小提示。能够向招聘人员展示你有能力做一个端到端的项目会让你的简历脱颖而出。然而，你的工作的商业价值也很重要。因此，试着做一些服务于任何真实案例商业价值的东西。祝你好运。

[](https://medium.com/@kotartemiy) [## 艺术媒体

### 阅读媒介上 Artem 的作品。固执己见的数据专家。newscatcherapi.com 和 politwire.com 的联合创始人…

medium.com](https://medium.com/@kotartemiy) 

如果你想支持我们的团队，你可以通过注册 Newscatcher API 的封闭测试版来帮助我们。我们的 API 允许你搜索过去发布的最相关的新闻。

[报名参加公测](https://newscatcherapi.com/)