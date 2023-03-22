# 用于访问公共数据的优秀 Python 库

> 原文：<https://towardsdatascience.com/great-python-libraries-for-accessing-public-data-adef88073be?source=collection_archive---------42----------------------->

## 使用 Python 访问公共数据

![](img/56ae2160fe4862b7c8b148133d72f437.png)

由[像素](https://www.pexels.com/photo/grayscale-photography-of-people-walking-in-train-station-735795/)上的[摄影记者](https://www.pexels.com/@skitterphoto)拍摄的照片

有许多优秀的 API 允许用户访问公共数据。这些数据可用于为金融模型生成信号，分析公众对特定主题的情绪，并发现公众行为的趋势。在这篇文章中，我将简要讨论两个允许你访问公共数据的 python 库。具体来说，我将讨论 Tweepy 和 Pytrends。

## 使用 Tweepy 获取公共推文

Tweepy 是一个 python 库，允许您通过 Twitter API 访问公共 tweets。创建 twitter 开发人员帐户和应用程序后，您可以使用 Tweepy 来获取包含您指定的关键字的 tweets。例如，如果你有兴趣了解公众对当前选举状况的看法，你可以调出最近所有关键词为“选举”的推文。Tweepy 对象返回 tweet 文本，可用于创建情感评分。除了政治之外，这还可以用于各种垂直行业，包括医疗保健、金融、零售、娱乐等。我之前写的一篇文章， [*患者对药品的情绪来自推特*](/extracting-patient-sentiment-for-pharmaceutical-drugs-from-twitter-2315870a0e3c) ，用推特分析了大众对热门药品的公众情绪。在另一篇文章[*Python*](https://link.medium.com/xee6Ryo3fab)中关于小丑(2019 电影)的推文分析中，我分析了 2019 电影*小丑的公众情绪。*我鼓励你去看看这些教程，自己进行一些数据整理和分析。申请 Twitter 开发者账户和创建 Twitter 应用程序的步骤在这里[列出](https://projects.raspberrypi.org/en/projects/getting-started-with-the-twitter-api/4)。tweepy 的文档可以在这里找到[。](https://tweepy.readthedocs.io/en/latest/getting_started.html)

## 使用 Pytrends 提取趋势主题数据

Pytrends 是一个 python 库，允许您访问表示某个关键字或主题在 Google 上被搜索的次数的数据。与 Tweepy 类似，您可以提供关键字和位置信息，Pytrends 对象将返回一个表示规范化 Google 搜索值的时间序列索引。这在零售和金融领域也有应用，但查看不同地区的关键词趋势也很有趣。关于 Pytrends 的友好介绍，请查看 Python 中的 [*冠状病毒 Google Trends*](/tracking-coronavirus-engagement-with-google-trends-in-python-5a4b08bc6977)和 Python 中的 [*使用 Google Trends API 选择万圣节服装*](/choosing-a-halloween-costume-using-the-google-trends-api-in-python-a3206b78a2a2) 。Pytrends 的文档可以在[这里](https://pypi.org/project/pytrends/)找到。

## 结论

总之，在这篇文章中，我们讨论了两个可以用来提取公共数据的 python 库。Tweepy 库非常适合从 Twitter 中提取推文，进行情感分析和趋势分析。Pytrends 库非常适合分析全球特定时间段和地区的 Google 趋势主题。我希望你觉得这篇文章很有趣。感谢您的阅读！