# 如何从零开始构建股票交易机器人

> 原文：<https://towardsdatascience.com/how-to-get-started-building-a-stock-cryptocurrency-forex-trading-program-2abbf0a4729f?source=collection_archive---------4----------------------->

## 交易策略，资源，和以前做过的人的建议。

![](img/498ccace146d51ef24cbc260035d856d.png)

尼古拉斯·霍伊泽在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄的照片

想不出策略？不确定使用哪个 API 和包？不想多此一举？我从头开始建立了一个股票日交易程序( [github repo](https://github.com/arshyasharifian/investbot) )，并希望分享一些有用的资源以及一些关于如何开始的建议。我知道开始一个新的项目，尤其是在一个外国领域，是具有挑战性的，我希望这篇文章能帮助拉平学习曲线。退一步说，我也想讨论一下我对“好”项目的标准。你可以做很多项目，那么为什么要做这个呢？

> ***免责声明:*** 这并不代表理财建议。您使用下面的算法、策略或想法进行的任何投资都是自担风险的。我对与本文包含的信息相关或由其引起的任何后果不承担任何责任。

# 大纲:

*   [为什么我认为建立一个交易机器人是一个“好”项目](#51f2):在花时间建立一个投资机器人之前，重要的是要问一下这是否是你时间的一个好投资。
*   [如何开始](#ce51):我提供一些示例伪代码来帮助你开始。
*   [资源](#0213):github repos、网站、其他媒体文章、youtube 视频、书籍和播客的列表，它们给了我极大的帮助。

# 为什么我认为建立一个交易机器人是一个“好”项目:

## 1.整合感兴趣的技术

鉴于买卖股票/加密货币/外汇的复杂性几乎是无限的，引入新技术的空间很大。你是一名 ***数据科学家*** 并且想要分析埃隆马斯克的一些推文或者识别 SEC 文件中的关键词吗？一个有抱负的 ***云工程师*** 希望使用云在 EC2 实例上全天候运行您的脚本？一个 ***后端开发者*** 想给终端用户提供一个 API，从你的算法中获取何时买卖的信号？一个 ***ETL 或数据工程师*** 想要使用 Kafka、Spark、DynamoDB 等大数据工具，并构建一个管道来传输价格数据，并将其放入 NoSQL/SQL 数据库。一位 ***前端开发人员或财务分析师*** 对使用 DASH 或 React+Flask 向最终用户展示算法的性能感兴趣？你想加入一些机器学习吗？希望你能明白。

## 2.合作的

通过将您的程序分成感兴趣的领域，轻松地与他人协作。我想任何项目都可以是合作的，但是考虑到这种挑战的复杂性，人们可以选择他们的兴趣并简单地应对这些挑战。这个项目的总体目标很明确，即**赚钱**，这也很有帮助。有时很难合作，因为人们迷失在细节中，忘记了整体目标。无论你是建立一个数据管道，创建仪表板，还是建立一些机器学习模型，目标都是明确的。

## 3.成功的明确衡量标准:$$$

有时候很难衡量成功与否，但是对于这个项目来说，知道这个项目赚了多少钱或者赔了多少钱才是最终的指标。坦率地说，从学习的角度来看，这是一个双赢的局面。如果你赚了钱，想想怎么才能赚更多的钱或者少亏一些。如果你在亏钱，想一想你如何能赚更多的钱或损失更少。没错，没有区别。总是有改进的空间，所有的努力都是一样的。有时候对于其他项目，很难知道你的改变是否有益。成功可能依赖于用户的反馈，或者仅仅是观点的问题。相比之下，这个项目很棒，因为成功和失败都很明显。然而，重要的是要记住市场是无限复杂的。虽然衡量进步很容易，但这并不意味着取得进步很容易。

## 4.容易被招聘者/未来雇主/奶奶理解

有一个大家都能理解的项目就好。几乎每个人都投资股票市场。低买高卖的目标很容易理解。当雇主询问项目时，大多数人会漫无边际地谈论他们已经实施的一些技术，而忽略了招聘人员或招聘经理的细节。这并不是说这项技术不重要，而是这个项目的最终目标是让解释这项技术变得更容易。例如，“我想限制股票报价机的范围，所以我使用 K 均值聚类来聚类我所有成功的交易，并找到相似的股票”，而不是“我实现了 K 均值聚类”。即使你不知道 K-Means 聚类是什么，你也能明白它的用途。

## 5.潜在有利可图(但不太可能)

你可以通过构建一个成功的算法来赚钱。几个月前，当我第一次开始这个项目时，我确信建造一些有利可图的东西主要是运气和机会。我仍然基本上是这样认为的，但我相信*创造一些有利可图的东西是可能的。我绝不会拿我输不起的钱去冒险，我强烈建议其他人也采取同样的心态。赚钱不应该是这个项目的目标，但这是一个不错的附带利益和令人向往的目标。*

## 6.无限复杂

这与我之前关于轻松整合任何技术的观点一致。无限的复杂性意味着你永远不会完成。(我认为这是半杯水)。总会有新的策略、技术、指标和度量标准需要整合和测试。这是一个永无止境的游戏。随你怎么解释，但我觉得这很有趣，也很令人兴奋。

# 如何入门？

## 定义策略

![](img/1a6165427541c8b65972e1a32a00074f.png)

JESHOOTS.COM 在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上[的照片](https://unsplash.com/@jeshoots?utm_source=medium&utm_medium=referral)

制定一个有效的策略。请注意，我没有说创建一个有利可图的策略。事实是你会重复。定义策略将有助于提供一些可以改进的框架。这是我创建的一个股票交易策略的例子:

我在我写的另一篇文章[中解释了这个策略，这篇文章讲述了我的股票交易算法](https://medium.com/ai-in-plain-english/stock-day-trading-bot-2f0ecc6e581a)的最初表现。现在重复这个策略并提供更多的细节。一旦你觉得自己有了可以轻松实现的东西，就开始思考如何实现。这时，我觉得我可以开始实施:

> 注意:包括我希望在实现过程中使用的 API 调用。这可能比伪代码阶段当前需要的更具体。

## 你将如何实施你的战略？

什么 API、包和其他资源有助于或者有必要实现这个伪代码？这可能吗？这是我做了一些研究，发现有 [Robinhood](https://robin-stocks.readthedocs.io/en/latest/) (不推荐) [TD Ameritrade](https://developer.tdameritrade.com/account-access/apis) 和[羊驼](https://alpaca.markets/)的股票 API 可以执行买卖单。对于新闻故事，我正在考虑使用 Python 模块 Beautiful Soup 和 Selenium 和 Scrapy 做一些网络抓取。我可以使用雅虎财经 API 来获取移动平均线和跟踪交易量。为了构建一些机器学习模型，我可能会使用 scikit-learn、pandas 和 numpy(我不建议一开始就过多关注机器学习)。我在 Jupyter 笔记本上写了我的初始程序，用 Github 作为我的回购。

现在，在这一点上，你可能会开始钻研新的技术和平台，如结合一些云或使用气流或 kubeflow，但我建议专注于尽快实施。如果您对整合其他技术感兴趣并且更有经验，您可以在整合技术之前，对您计划使用的技术进行概念验证(POCs)。如果您在一个小组中工作，有些人可能只关注做概念验证，看看什么最有效。

## 迭代:快速失败

简单地经历实现伪代码的过程会教会你很多东西。你开始明白设计中的瓶颈和改进在哪里。并且可能已经了解了有用的新 API、包或框架。我也推荐做一些[纸上交易](https://alpaca.markets/docs/trading-on-alpaca/paper-trading/)(模拟交易)来测试你的程序的表现。记住，没有什么比现场交易更好的了。当你进行实时交易时，有很多因素会影响你的程序的表现。[阅读我之前的一篇文章，其中描述了我在构建算法时面临的一些挑战。](https://medium.com/swlh/reflections-of-a-day-trading-bot-developer-5302966bc6c9)

# 资源

![](img/fcc008a6fac5efa0a16cc90b40369edb.png)

照片由 [Fikri Rasyid](https://unsplash.com/@fikrirasyid?utm_source=medium&utm_medium=referral) 在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄

下面是一些帮助我开始的资源，可能对你也有帮助。同样，有些人可能会提供具体的建议，而其他人则为您提供一些领域知识和专业技能。

> 注意:我会不断更新下面有用的资源列表。

## Github Repos

1.  [为 Quants(定量金融)精心策划的令人疯狂的库、包和资源列表](https://github.com/wilsonfreitas/awesome-quant)
2.  [新闻 API](https://github.com/kotartemiy/newscatcher)
3.  [特朗普推特兑现](https://github.com/maxbbraun/trump2cash)
4.  [刮新闻文章](https://github.com/codelucas/newspaper)

## 其他媒体文章

1.  [股市 API](https://medium.com/@andy.m9627/the-ultimate-guide-to-stock-market-apis-for-2020-1de6f55adbb)
2.  [股票市场策略(高级)](https://medium.com/swlh/coding-your-way-to-wall-street-bf21a500376f)
3.  [指标和更高水平](https://medium.com/swlh/400-trading-algorithms-later-bc76279bc525)
4.  [如何获取指标](/get-up-to-date-financial-ratios-p-e-p-b-and-more-of-stocks-using-python-4b53dd82908f)值(使用抓取从 Finviz)
5.  [使用指示器的例子](/options-trading-technical-analysis-using-python-f403ec2985b4)
6.  [启发我的算法的刷单策略实现](https://medium.com/automation-generation/concurrent-scalping-algo-using-async-python-8df9f31e22f1)
7.  [财经网页抓取示例](http://francescopochetti.com/scrapying-around-web/)
8.  [构建比特币流媒体后端](https://medium.com/@arshyasharifian/real-time-bitcoin-price-streaming-using-oracle-cloud-services-2f3fcb3adfb2)
9.  [流式股票数据](https://medium.com/analytics-vidhya/stream-stock-data-6bac816ca6d0)
10.  [埃隆·马斯克发推特赚钱](https://medium.com/datadriveninvestor/elons-lethal-mistake-predicting-the-stock-price-of-tesla-with-twitter-and-machine-learning-5e89282ce75f?source=userActivityShare-699fed23e2a0-1589866825&_branch_match_id=750147715456955943)

## 网站:

1.  点击一个报价器，查看专业交易者使用的所有指标和过滤器。这可能会启发您在自己的过滤算法中使用哪些指标。
2.  市场观察:日内交易者的热门新闻来源。(收集这些故事并做一些情感分析可能会很有趣)。
3.  SEC 文件:如果你输入一个股票代码，你会看到该公司所有的官方文件。(我们正在考虑从这些文件中提取一些，为我们的股票交易算法提供信息)。
4.  [Subreddit AlgoTrading](https://www.reddit.com/r/algotrading/) :算法交易的 Subreddit。这里也有一些很棒的资源。这里有一个关于最好的 API 交易平台的[讨论。](https://www.reddit.com/r/algotrading/comments/g5u88q/what_is_the_best_free_stock_trading_api/)[交易算法 Python 介绍](https://np.reddit.com/r/algotrading/comments/5selh0/python_for_algorithmic_trading_and_investing/)。

## 油管（国外视频网站）

1.  [熊牛交易者](https://youtu.be/3ekMVdin3HI?t=205):新手交易者培训视频
2.  [Forrest Knight](https://www.youtube.com/watch?v=vkljN8jeWV0&t=273s) :构建日交易算法的资源
3.  [关于人工智能在外汇领域的有趣 TED 演讲](https://www.youtube.com/watch?v=lzaBbQKUtAA)

## 书

1.  [闪光男孩](https://www.amazon.com/dp/B00HVJB4VM/ref=dp-kindle-redirect?_encoding=UTF8&btkr=1):大家之所以要对股市赚钱持怀疑态度。

## 播客:

这是一些描述高级策略的播客。(你也可以谷歌一下“日内交易播客”——有很多)。

1.  [交易欲望](https://overcast.fm/+FJZ5c1JHk):采访日交易系统开发者
2.  [顶尖高手交易](https://www.stitcher.com/podcast/wwwstitchercompodcastonlinetradingtowin/online-trading-to-win)

[![](img/9feb2f59889762b163b5adb2600d8f85.png)](https://www.buymeacoffee.com/arshyasharifian)

如果你喜欢你所读的

## **子栈:**

我最近创建了一个子栈来学习如何用 Python 编程和解决 LeetCode 问题。请点击这里查看:

1.  [我的子栈](https://arysharifian.substack.com/)

***注来自《走向数据科学》的编辑:*** *虽然我们允许独立作者根据我们的* [*规则和指导方针*](/questions-96667b06af5) *发表文章，但我们不认可每个作者的贡献。你不应该在没有寻求专业建议的情况下依赖一个作者的作品。详见我们的* [*读者术语*](/readers-terms-b5d780a700a4) *。*