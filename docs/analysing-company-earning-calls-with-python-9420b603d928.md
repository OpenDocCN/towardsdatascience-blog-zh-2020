# 使用 Python 分析公司收入呼叫

> 原文：<https://towardsdatascience.com/analysing-company-earning-calls-with-python-9420b603d928?source=collection_archive---------32----------------------->

## 构建一个 **Python 脚本来分析公司盈利呼叫**。

在本帖中，我们将**摘录管理层在最近的收入电话会议中陈述的关于**公司**近期和未来**业绩**的主要要点。**

代码将非常简单。我们将通过我们感兴趣的任何公司的股票，结果将是一个令人惊讶的收益电话会议的简短总结。

![](img/908de4ed0a8b0a4f99bb4b9c75e25c25.png)

卡罗琳娜·格拉博斯卡在[Pexels.com](https://www.pexels.com/photo/american-dollar-banknotes-rolled-and-tightened-with-red-band-4386477/)拍摄的照片

# 为什么要分析公司盈利电话

公司盈利电话是了解企业经营状况和下一季度预期的主要信息来源之一。

公司盈利电话是公开的。通常，我们可以访问该公司的网站，并拨入电话，听取管理层的意见。收益电话会议通常安排在公司收益公开发布的同一天。电话会议的结构很简单，管理层概述了上个季度的运营情况和未来的指导。此外，记者可以向最高管理层询问有关业务的问题。

他们听起来很有见地。但是，浏览整个电话可能会很耗时，尤其是当我们对多家公司感兴趣时。

因此，在这篇文章中，我们要做的是使用 Python 自动分析公司盈利呼叫。

# Python 如何帮助分析公司盈利电话？

使用 Python，我们不仅能够执行定量分析，正如我们在我以前的帖子中所学的那样。其中，我们可以，例如，使用[自然语言](https://en.wikipedia.org/wiki/Natural_language_processing)处理来分析语言结构或执行[情感分析](https://www.kaggle.com/ngyptr/python-nltk-sentiment-analysis)。

在本帖中，我们将重点传递公司收入电话的完整记录作为输入。然后，我们将提取关键点，这将有助于我们了解一家公司的发展方向以及最近的表现如何。以下是我们将回答的一些问题:

*   公司上个季度表现如何？
*   销售额的预期增长是多少？
*   哪条线的产品表现更好？
*   公司有望增加净利润吗？
*   还有更多

# Python 脚本分析公司盈利电话

我们通过向 [financialmodelingprep](https://financialmodelingprep.com/developer/docs/#Earning-Call-Transcript) 发出 *get* 请求来开始我们的代码，以便检索最新收益的副本。

在本帖中，我们将分析*苹果*，但是可以随意更改公司名称来分析任何其他公司。此外，取消对变量 *demo* 的注释，并传递您的 API 密钥(注册后您可以在 [financialmodelingprep](https://financialmodelingprep.com/developer/docs/pricing/codingAndFun/) 中免费获得一个)，以便对 API 的请求能够工作。

然后，我们将从 API 获得的完整字符串存储在变量*的副本*中。接下来，我们可以使用 Python [split](https://www.w3schools.com/python/ref_string_split.asp) 方法。该方法将创建一个 Python 列表，其中包含由每个新行(即/n 字符)分割的脚本，如下面的结果所示。

```
import requests
import pandas as pd

#demo= 'your api key'

company = 'AAPL'

transcript = requests.get(f'https://financialmodelingprep.com/api/v3/earning_call_transcript/{company}?quarter=3&year=2020&apikey={demo}').json()

transcript = transcript[0]['content'].split('\n')
print(transcript)

#outcome
["Operator: Good day, everyone. Welcome to the Apple Incorporated Third Quarter Fiscal Yea
```

现在我们有了一个 Python 列表，其中包含了最新的苹果公司的每一句话，我们将把它传递给熊猫数据帧。我们将用熊猫来分析收益记录。我们将搜索单词“expect”以保留包含单词 *expect* 的所有句子。

```
earnings_call = pd.DataFrame(transcript,columns=['content'])
word_to_analyze = 'expect'

analysis = earnings_call[earnings_call['content'].str.contains(word_to_analyze)]
text_earnings = analysis['content'].values
```

然后，在分析变量中，我们使用 *str.contains* 方法过滤掉所有含有*except*单词的句子。你可以随意改变这个词来分析公司的任何其他方面，比如收入、利润。

然后，我们从熊猫系列中提取值，以得到如下所示的句子。只保留带*期待*字样的句子。

```
print(text_earnings)
#outcome
array(["Tim Cook: Thanks, Tejas. Good afternoon, everyone. Thanks for joining the call today. Be...
```

如果你进一步研究变量 text _ incomes，你会发现有些句子还是太长了。我们将循环遍历它们，以便在每次遇到“.”时将它们分开。然后，我们将只打印包含单词*和*的句子:

```
for text in text_earnings:
  for phrase in text.split('. '):
    if word_to_analyze in phrase:
      print(phrase)
      print()

#outcome
Due to the uncertain and ongoing impacts of COVID-19, we did not provide our typical guidance when we reported our results last quarter, but we did provide some color on how we expected the June quarter to play out

In April, we expected year-over-year performance to worsen, but we saw better-than-expected demand in May and June

We expected iPad and Mac growth to accelerate and we saw very strong double-digit growth for these devices this quarter

Wearables growth decelerated as we expected, but still grew by strong double-digits and set a revenue record for a non-holiday quarter

First, results for advertising and AppleCare were impacted by the reduced level of economic activity and store closures to a degree that was in line with our expectations

However, we will provide some additional insight on our expectations for the September quarter for our product categories

On iPhone, we expect to see recent performance continue for our current product lineup, including the strong customer response for iPhone SE

We expect the rest of our product categories to have strong year-over-year performance

For services, we expect the September quarter to have the same trends that we have observed during the June quarter except for AppleCare where during the September quarter a year ago we expanded our distribution significantly

As a consequence, we expect a difficult comp for AppleCare also considering the COVID related point of sale closures this year

For OpEx, we expect to be between $9.8 billion and $9.9 billion

We expect the tax rate to be about 16.5% and OI&E to be $50 million

First one, Tim, when you look at the services business and in terms of your TV+ content production have the movement restrictions impacted the content production efforts? And along the same path four years ago your premonition on services being a $50 billion business in 2020 came sooner than expected, I don't know if you want to make any such forecast four years out and how you think services revenue is going to be

With the strong sales in Mac given the shelter-in-place, do you think the back-to-school season got pulled in by a quarter or do you expect the momentum to still continue? Thank you very much.

Luca Maestri: As I said, when I was talking about providing some commentary for the September quarter, we expect all the non-iPhone product categories to have a very strong year-over-year performance

So we expect the performance that we've seen for Mac in the June quarter to continue.

And then, can you talk a little bit more about the decision to bring Mac Silicon in-house, then the benefits that you
```

# 包扎

最后，我们总结了收益电话会议，以提取关键信息供我们分析。从上面的例子中，我们可以快速而容易地看出:

*   苹果在 5 月和 6 月的需求好于预期
*   苹果实现了非常强劲的收入增长，并创造了新的非假日季度记录
*   在今年余下的时间里，预计会有强劲的同比表现
*   运营支出约为 99 亿美元
*   产品类别绩效

这个脚本很酷的一点是，你可以将 *expect* 替换为要分析的单词(例如 *profits* )，你将得到你感兴趣的任何公司的简短摘要。

在以后的文章中，我们将继续分析盈利电话记录，执行 Python NLTK 情绪分析。

与此同时，你可以继续阅读我在 [Python for Finance 上的其他帖子。](https://codingandfun.com/category/python-for-finance-data-science/)

*原载于 2020 年 10 月 18 日*[*【https://codingandfun.com】*](https://codingandfun.com/analysing-company-earning-calls-with-python/)*。*