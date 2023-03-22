# 我使用 rTweet 使用 Twitter Premium Full Archive API 的体验

> 原文：<https://towardsdatascience.com/my-experience-with-twitter-premium-full-archive-api-using-rtweet-f6309f789902?source=collection_archive---------41----------------------->

## 从高级 API 访问数据有几个方面不是常识

![](img/917e9a2c5696d5ab7a2c78626255d91c.png)

图片来源:Unsplash

Twitter 的高级存档 API 允许访问超过 30 天的推文，可以追溯到 2006 年。这是一种重要的数据资源，自然也很昂贵。人们应该在开始使用它之前了解某些事情，因为每个请求都是付费的，并且有一个有限的请求配额。

从 premium API 访问数据的整个过程在 Twitter 的开发者页面以及 R 和 Python 库的文档中都有很好的记录，但是有几个方面没有得到很好的解释或者完全被忽略了。

在我的项目中，我需要访问两到三个月前的推文，所以我购买了 Twitter 的高级访问权限以获得完整的存档。我使用了“rtweet”包，这是 R 的库，可以访问 Twitter 的 API。它有很好的文档记录，并且可能是唯一允许访问高级 API 的 R 包(但是它有令人痛苦的[缺陷](https://github.com/ropensci/rtweet/issues/368))。

然而，在你开始走这条路之前，有一些要点你应该知道。

1.  **成本+税—** 此处[给出的成本表](https://developer.twitter.com/en/pricing/search-fullarchive)不含税。我每月花 224 美元购买了 250 个请求，并额外支付了 44 美元。也许很多人都知道成本不包括税，但他们应该在某个地方说出来，特别是对于那些可能申请补助金/资金来支付这笔费用的人。
2.  **最近到以前的推文—** 在构建获取推文的查询时，rtweet 函数 search_full archive 允许输入日期范围。该查询如下所示:

```
tweet <- search_fullarchive("#beyonce or #katyperry)(lang:en or lang:hi)" , n = 3000, fromDate = "201912120000", toDate = "201912120000", env_name = "curate", safedir = NULL, parse = TRUE, token = token)
```

Twitter 的 premium API 获取用户查询中提到的最近时间，然后一直工作到开始时间。这种方法不允许全天发布更大范围的推文，而只能在很短的时间内发布。

3.**推文配额—****Twitter Premium API 允许每个请求访问 500 条推文。而且，你也可以得到 500 的倍数的推文。例如，(这是我的理解)在上面的查询中，我在一个请求中请求了 3000 条推文，这将被计为=3000/500 = 6 个请求。人们需要注意这个数学问题，因为订阅允许一定数量的请求和一定数量的推文，在我的情况下，每月 250 到 125，000 条推文。如果你用完了请求，你将无法消耗你的 tweets 配额。在实验和查询构建中，很有可能一些请求会耗尽。**

**4. **rTweet 消耗更多请求的速度更快—** 我最近发现 rTweet 软件包有一个缺陷，它消耗的请求比需要的多。我不确定他们是否已经修好了。但是，由于这个错误，我丢失了大量的推文。你可以[在这里阅读关于这个 bug 的](https://github.com/ropensci/rtweet/issues/368)。**

**当我开始使用 Twitter Premium API 时，我没有发现这些要点，因此我付出了惨痛的代价。**

**希望有帮助。**