# 通过网络抓取构建数据集的更快捷方式

> 原文：<https://towardsdatascience.com/a-quicker-way-to-build-datasets-through-web-scraping-4ce9e51e16d8?source=collection_archive---------26----------------------->

## 通过抓取 Yelp 评论来比较 Autoscraper 和 Selenium + BeautifulSoup

![](img/62256a2dc181e09d7bdd312fcc05ffff.png)

托马斯·毕晓普在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上的照片

如果你想跳过 HTML 标签的挖掘，直接进入抓取，这里是要点。**注意，scraper 试图与你想要的列表中的每一项进行精确匹配。**否则，请继续阅读有关网络抓取的简短背景知识，这对于抓取网站很有用，以及抓取时您可能会遇到的一些挑战。

# 背景

我最近在研究美国的泡沫茶趋势。我想看看饮料订单的变化，精品和特许泡泡茶店开业的时间，以及顾客对这些店的评价。很自然地，我转向 Yelp。但是一些限制很快让我退缩了；我被限制在 Yelp API 上的前 1000 个商家，每个商家我只能得到三个 Yelp 精选评论。

从商业的角度来看，这是有意义的——你不希望其他企业轻易窥探你的成功和失败，并重复这些。但它也展示了网络抓取的更大不幸。一方面，这是为我们感兴趣的课题获取数据的好方法。另一方面，公司很少希望机器人在他们的网站上互动。即使他们提供了 API，他们也不总是公开所有需要的信息。

针对网络抓取的安全性也提高了。从标题数据验证，到验证码挑战，再到机器学习指导的行为方法，已经投入了大量工作来使机器人更难祸害网站。最后，大多数页面不会立即用 HTML 提供有用的信息。相反，它们是通过 JavaScript 执行注入的，并且只有在检测到“真实用户”时才注入。

因此，值得信赖的过程:

1.  手动导航到网站
2.  突出显示感兴趣的部分
3.  右键单击并检查元素
4.  复制 html 标记或 XPath，并使用 BeautifulSoup 之类的库来解析 HTML

# 该过程

这是我项目中的一个例子。比方说，你想解析 Yelp 上关于某个泡泡茶店的评论[https://www . Yelp . com/biz/chun-yang-tea-flushing-new York-flushing？osq = bubble % 20 tea&sort _ by = date _ desc](https://www.yelp.com/biz/chun-yang-tea-flushing-new-york-flushing?osq=bubble%20tea&sort_by=date_desc)(我不属于这家泡泡茶店，也不属于 Yelp。这是我目前项目中的一个样本，分析美国的泡沫茶趋势。)

为了在动态网页上执行任何 JavaScript，您首先需要设置一个无头浏览器，或者模拟真实浏览器的标题。

然后，将它连接到一个可以从 HTML 和 XML 文件中提取数据的库。

为了找出将我引向评论的特定标签`lemon--span__373c0__3997G raw__373c0__3rKqk`，我按照上面编号的步骤列表，突出显示一个示例评论，检查元素，并四处窥探感兴趣的标签。

# 问题是

这实际上是非常脆弱的，因为网站可能会随时改变它们呈现页面的方式。更重要的是，这个过程是多余的，因为我们需要首先手动找到想要的标签，然后才能设置一个自动化的 webscraper。

# 另一种方法

与其搜索 HTML 标记或 XPath，为什么不使用简单的匹配规则自动进行手动搜索呢？这是 [Alireza Mika](https://medium.com/@alirezamika) 的 [AutoScraper 库](https://github.com/alirezamika/autoscraper)的基本前提。让我们完成搜集 Yelp 评论的相同任务，这次是用 AutoScraper 库。

`AutoScraper`对象#3 接受目标 url #1 和想要的项目列表#2，并围绕提供的参数#4 建立一些规则。请注意，它只会标记与感兴趣的项目完全匹配的标签；因此，我列入通缉名单的评论是 Yelp 网页上第一篇完整的评论。

在打印`results`时，你将会看到 webscraper 找到的所有元素。您可以使用以下命令在同一个或另一个 url 上再次获取这些结果。

`scraper.get_result_similar(url)`

你可能会注意到，刮刀已经获得了一些无关的信息。您可以微调其选择，方法是首先根据刮刀已学习的规则对其进行分组:

```
groups = scraper.get_result_similar(url, grouped=True)
```

由于`groups`是一个字典，你可以通过调用`print(groups.keys())`得到规则的名称

然后，您可以使用特定的规则键入字典:

`groups['rule_io6e']`

如果指定规则的结果准确地反映了期望的结果，你可以选择用方法调用来保持规则:

```
scraper.keep_rules('rule_io6e')
```

最后，您可以将模型保存到文件中以备后用。

```
scraper.save('yelp-reviews')
```

# 最后

当然，这个库并不是万无一失的。如果没有浏览器模拟器，点击量大的网页(如谷歌的商业评论)将很难实现自动化。

由于 AutoScraper 希望通缉列表中的物品与网站上出现的物品完全匹配，因此很难确定一个不断更新的值，如股票价格。

然而，作为开发人员，我们的部分职责是将解决方案沿着管道连接在一起，以迭代或创建新产品。希望你可以利用这个抓取库，无论是在 scrapy 这样的框架中，还是只是为了简化你的下一个抓取任务。

不管你如何决定刮一个网站，这样做负责任！

1.  **不要用请求轰炸网站**。这是网络抓取新手犯的头号错误:他们试图通过一次从多个服务器发送多个请求来加快抓取速度——这是一种 DDoS 攻击！
2.  尊重 robots.txt:你通常可以通过在索引页面添加`robots.txt`来找到它们。比如[https://www.yelp.com/robots.txt](https://www.yelp.com/robots.txt)
3.  使用提供的 API！