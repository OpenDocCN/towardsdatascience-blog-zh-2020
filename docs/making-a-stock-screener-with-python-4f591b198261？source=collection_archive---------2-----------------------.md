# 用 Python 制作股票筛选程序！

> 原文：<https://towardsdatascience.com/making-a-stock-screener-with-python-4f591b198261?source=collection_archive---------2----------------------->

## 学习如何用 Python 制作一个基于 Mark Minervini 的趋势模板的强大的股票筛选工具。

*免责声明:本文内容基于* [*理查德·莫格伦的 Youtube 频道*](https://m.youtube.com/channel/UCYqMAKiU3tFijWnyqAxG4Cg) *纯属教育目的，不应视为专业投资建议。自行决定投资。*

股票筛选是为你的特定交易策略寻找完美股票的好方法。然而，当我试图使用 Python 寻找股票筛选工具时，我几乎找不到任何功能性的自动化代码。所以我创建了这篇文章，帮助其他人根据马克·米纳尔维尼的趋势模板(选择最佳股票的 8 条原则)制作一个简单易懂的股票筛选 Python 程序。特别是在当前市场波动的情况下，我希望这个代码能帮助你做交易。

![](img/809e6ceae6804131ff4f3be27612e9b3.png)

图片来自 [Pixabay](https://pixabay.com/?utm_source=link-attribution&amp;utm_medium=referral&amp;utm_campaign=image&amp;utm_content=1853262) 的[像素](https://pixabay.com/users/Pexels-2286921/?utm_source=link-attribution&amp;utm_medium=referral&amp;utm_campaign=image&amp;utm_content=1853262)

在我进入代码之前，我想谈谈股票筛选标准。

1.  证券的当前价格必须高于 150 和 200 天的简单移动平均线。
2.  150 天简单移动平均线必须大于 200 天简单移动平均线。
3.  200 天简单移动平均线必须至少有 1 个月的上升趋势。
4.  50 天简单移动平均线必须大于 150 简单移动平均线和 200 简单移动平均线。
5.  当前价格必须大于 50 天简单移动平均线。
6.  当前价格必须比 52 周低点高出至少 30%。
7.  当前价格必须在 52 周高点的 25%以内。
8.  IBD RS 评级必须大于 70(越高越好)。RS 评级是衡量一只股票在过去一年中相对于所有其他股票和整体市场的价格表现的指标。查看这篇[文章](https://www.investopedia.com/terms/r/relativestrength.asp)了解更多信息。

你可以在 Mark Minervini 的博客文章中读到更多关于这个模板的内容。

现在你已经熟悉了标准，我们可以进入代码了。首先，导入以下依赖项。如果您的机器上没有安装以下模块之一，请在您的终端中使用“pip install (module name)”下载它们。

导入这些依赖项，并使用`yf.pdr_override.`覆盖弃用警告

首先，我们必须导入我们将在程序中使用的依赖项，如 yahoo_fin(获取报价机列表)和 pandas_datareader.data(获取历史股票数据)。

接下来，我们必须为程序的其余部分设置变量。我在下面概述了每个变量的含义。

*   票贩子:标准普尔 500 所有的票贩子
*   index_name:标准普尔 500 雅虎财经符号
*   start_date:历史数据的开始日期(正好一年前)
*   end_date:历史数据的结束日期(今天)
*   exportList:我们将为每只股票收集的值
*   returns _ multiples:查看每只股票相对于市场表现的列表(将用于计算 RS 评级)

for 循环的开始。

现在，我们可以计算过去一年标准普尔 500 指数的累计回报，并将该值与同期标准普尔 500 每只股票的累计回报进行比较。IBD 相对强弱指标本质上是计算一只股票在特定时间段内相对于市场和其他股票的表现。由于该指标仅在 IBD 服务中使用，我们可以通过将每只股票的累计回报率除以指数的累计回报率来估计 IBD RS，然后为每只股票创建一个百分制排名。例如，如果 AAPL 在特定时期内的表现优于 MSFT，那么它的 RS 就会更高。在 Mark Minervini 的趋势模板中，他寻找 RS 值为 70 或更高的股票(市场中表现最好的 30%的股票)。

因为在这个程序中，RS 度量是用相对于给定列表中其他股票的百分比值来计算的，所以最好有一个更大数量股票的列表，以使 RS 值更准确。在这个程序中，我们选择了标准普尔 500 指数中的 500 只股票，这样就有了足够大的样本量。

为了加快这个过程，我们可以下载每只股票过去一年的历史数据，而不是不断地向雅虎财经发出请求(这可能会导致错误)。for 循环末尾的 time.sleep(1)也有助于抑制请求涌入时的潜在错误。最后，在使用分位数进行数据处理之后，我们得到了一个数据框架，其中包含给定列表中表现最好的 30%的股票以及它们各自的 RS 值。

我们可以只包括通过 Minervini 趋势模板条件 8(RS 值大于 70)的前 30%的股票，而不是计算每只股票的指标。从这里，我们可以计算出创造条件所需的指标。当前收盘价通过采用最后一天的调整收盘价来使用。过去一年的高点和低点是通过找出过去 260 个交易日(大约一年)数据框架中的最大值和最小值得到的。移动平均值是通过计算相应天数的滚动平均值来使用的。其余的代码实际上是按照前面提到的原则执行 screener。如果一只股票通过了每一个条件，我们可以将它添加到我们的通过 Minervini 趋势模板的股票导出列表中！

最后，为了方便起见，这段代码将打印出所有符合要求的股票的数据帧，并将股票下载到 excel 文件中。现在你知道如何创建一个有史以来最好的交易者使用的股票筛选工具了！

下面的 GitHub 要点包含了该程序的所有代码。我希望这个算法将来对你有用。非常感谢您的阅读！

如果你喜欢这篇文章，可以看看下面我写的其他一些 Python for Finance 文章！

[](/parse-thousands-of-stock-recommendations-in-minutes-with-python-6e3e562f156d) [## 使用 Python 在几分钟内解析数千份股票推荐！

### 了解如何在不到 3 分钟的时间内解析顶级分析师的数千条建议！

towardsdatascience.com](/parse-thousands-of-stock-recommendations-in-minutes-with-python-6e3e562f156d) [](/stock-news-sentiment-analysis-with-python-193d4b4378d4) [## 用 Python 进行股票新闻情绪分析！

### 对财经新闻进行秒级情感分析！

towardsdatascience.com](/stock-news-sentiment-analysis-with-python-193d4b4378d4) [](/creating-a-finance-web-app-in-3-minutes-8273d56a39f8) [## 在 3 分钟内创建一个财务 Web 应用程序！

### 了解如何使用 Python 中的 Streamlit 创建技术分析应用程序！

towardsdatascience.com](/creating-a-finance-web-app-in-3-minutes-8273d56a39f8)