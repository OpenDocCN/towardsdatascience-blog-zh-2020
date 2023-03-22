# 仅用 2 行 Python 代码访问菲律宾股票数据

> 原文：<https://towardsdatascience.com/access-philippine-stock-data-with-only-2-lines-of-python-309780382b8d?source=collection_archive---------13----------------------->

## 介绍 fastquant，一个方便访问和分析菲律宾股票数据的工具

![](img/6f9f3b3c0c98bf3e5e52b5cd4db86519.png)

[M. B. M.](https://unsplash.com/@m_b_m?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) 在 [Unsplash](https://unsplash.com/s/photos/stocks?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) 上拍摄的照片

作为一名具有金融背景的数据科学家，我一直着迷于通过交易股票赚钱的想法，因为这似乎很简单。为什么？你只需要知道两件事就能赚钱——在低点买入，在高点卖出。

那么，你怎么知道股票何时会上涨或下跌呢？通过分析股票的相关数据(如价格、收益和其他经济指标)。从理论上讲，如果你能以某种方式识别价格的(可预测的)模式，你应该能够识别购买股票的低点。这是一个尝试机器学习和其他统计方法来模拟金融时间序列数据和测试交易策略的绝佳机会。现在我只需要数据。

**但问题来了:**

> 我找不到一个免费的 API 来提供可靠的和最新的菲律宾公司的财务数据。

[Yahoo Finance](https://finance.yahoo.com/quote/AYAAF/history?p=AYAAF&.tsrc=fin-srch) 很奇怪，因为它以美元报告，并将数字四舍五入到价格不变的程度，其他来源可以给我图表(例如 [Investagrams](https://www.investagrams.com/Stock/PSE:JFC) )，但不能编程访问我用 python 分析数据所需的实际原始数据。

# 使用 fastquant 访问和分析股票数据

为了解决这个问题，我决定创建一个免费的 python 包，允许任何人获取菲律宾公司的财务数据。fastquant 能够提供任何上市股票截至最近一个交易日的定价数据。

在本文的其余部分，我将演示如何使用简单的移动平均交叉策略，使用 *fastquant* 对 Jollibee Food Corp. (JFC)进行简单的分析。

## 装置

安装非常简单，因为这个包是在 PyPi 上注册的！

```
# Run this on your terminal
pip install fastquant

# Alternatively, you can run this from jupyter this way
!pip install fastquant
```

## 获取菲律宾股票数据

有了 fastquant，你可以用两行代码获得任何 PSE 上市公司的财务数据。

在本例中，我们获取了 Jollibee Food Corp. (JFC)从 2018 年 1 月 1 日到 2019 年 1 月 1 日(1 年期)的定价数据。

```
from fastquant import get_pse_data
df = get_pse_data("JFC", "2018-01-01", "2019-01-01")print(df.head())

#             open   high    low  close        value
#dt                                                 
#2018-01-03  253.4  256.8  253.0  255.4  190253754.0
#2018-01-04  255.4  255.4  253.0  255.0  157152856.0
#2018-01-05  255.6  257.4  255.0  255.0  242201952.0
#2018-01-08  257.4  259.0  253.4  256.0  216069242.0
#2018-01-09  256.0  258.0  255.0  255.8  250188588.0
```

至此，fastquant 已经完成了它的主要目的——轻松访问菲律宾股票数据。在接下来的步骤中，我们将重点对 JFC 数据进行简单的分析。

## 绘制每日收盘价

为了绘制 Jollibee 的价格，我们实际上可以直接使用由 *get_pse_data* 返回的 dataframe，但是我们仍然需要导入 *matplotlib* 包来添加标题，同时控制其 fontsize(设置为 20)。

```
# Import pyplot from the matplotlib module
from matplotlib import pyplot as plt# Plot the daily closing prices
df.close.plot(figsize=(10, 6))
plt.title("Daily Closing Prices of JFC\nfrom 2018-01-01 to 2019-01-01", fontsize=20)
```

![](img/41ae3ac567c6211341e2fa4889420ed3.png)

请注意，2018 年的峰值出现在 3 月和 12 月左右，而低谷出现在 7 月和 10 月左右。

## 使用简单的移动平均线(SMA)交叉策略进行分析

在本节中，我们将尝试直观地评估 SMA 交叉策略的性能。有很多方法可以实现这个策略，但是我们会用一个“价格交叉”的方法来实现 30 天的 SMA。

在这种情况下，当收盘价从下方穿过简单移动平均线时，它被认为是“买入”信号，当收盘价从上方穿过简单移动平均线时，它被认为是“卖出”信号。

```
# Derive the 30 day SMA of JFC's closing prices
ma30 = df.close.rolling(30).mean()# Combine the closing prices with the 30 day SMA
close_ma30 = pd.concat([df.close, ma30], axis=1).dropna()
close_ma30.columns = ['Closing Price', 'Simple Moving Average (30 day)']# Plot the closing prices with the 30 day SMA
close_ma30.plot(figsize=(10, 6))
plt.title("Daily Closing Prices vs 30 day SMA of JFC\nfrom 2018-01-01 to 2019-01-01", fontsize=20)
```

![](img/43167b9e95508cf9ceff21accdff4c79.png)

那么我们如何知道我们的 SMA 价格交叉策略是有效的呢？视觉上，我们可以通过观察“卖出”信号是否发生在股价开始下跌之前，以及“买入”信号是否发生在股价开始上涨之前来评估这一点。

如果你看上面的图表，它看起来非常有效——收盘价线实际上在 3 月左右(第一个高峰的月份)开始几乎持续保持在 30 天 SMA 下方，并在 7 月下旬(第一个低谷的月份)开始保持在 30 天 SMA 上方。

现在，你可能会想，应该有一个更严格的方法来评估交易策略的数字精度。这个过程(如下图所示)被称为*回溯测试*，是投资专业人士根据经验验证不同交易策略有效性的方式。换句话说，它允许我们比较不同的交易策略，并让我们能够根据历史表现选择最好的一个。

![](img/305bcf0309bc07c8eade7e3b6bd8d731.png)

假设我们采用最小最大支持阻力交易策略(2017 年 1 月 1 日至 2019 年 1 月 1 日)，进行回溯测试

当然，重要的是要记住，历史表现并不总是会转化为未来的表现，所以应该始终考虑建仓的风险。无论如何，我不打算在这里讨论回溯测试的细节，但是我打算在以后的帖子中讨论它。

# 结论

祝贺您到达这篇博文的结尾。到目前为止，您应该已经知道如何 1)使用 fastquant 获得菲律宾公司的财务数据，2)绘制公司股票随时间变化的收盘价，3)直观地分析简单移动平均线交叉策略的有效性。

我们还简要地谈到了回溯测试，以及如何用它来比较不同交易策略的有效性。同样有趣的是应用机器学习和基于统计的预测方法来制定人工智能增强的交易策略。在以后的文章中会有更多关于这些和其他高级主题的内容！

你可以在这个[笔记本](https://github.com/enzoampil/fastquant/blob/master/lessons/fastquant_lesson1_accessing_pse_data.ipynb)上找到上面执行的脚本。如果您想试用 fastquant，了解更多关于该方法的信息，或者甚至为改进该模块做出贡献，请随时查看 [github repo](https://github.com/enzoampil/fastquant) 。

感谢您阅读这篇文章，**如果您对 fastquant 或任何与将数据科学应用于金融相关的问题有任何疑问，请在下面**随意评论。也可以通过邮件联系我(*Lorenzo . ampil @ Gmail . com)*[Twitter](https://twitter.com/AND__SO)，[LinkedIn](https://www.linkedin.com/in/lorenzoampil/)；然而，期待我更快地回复评论:)