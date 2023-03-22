# 使用 Python 进行回溯测试

> 原文：<https://towardsdatascience.com/backtesting-with-python-109cab7ee737?source=collection_archive---------27----------------------->

## 回溯测试移动平均线

在本帖中，我们将使用 Python 对简单的移动平均(MA)策略进行回溯测试。在我最近的一篇文章中，我展示了如何使用 Python 计算和绘制移动平均策略。现在，我们将学习通过回溯测试我们的算法来模拟移动平均线策略在过去几个月的表现。

![](img/7a7b9327802010eaa81bab1b4d21a7f2.png)

照片由[负空间](https://www.pexels.com/@negativespace?utm_content=attributionCopyText&utm_medium=referral&utm_source=pexels)从[像素](https://www.pexels.com/photo/party-glass-architecture-windows-34092/?utm_content=attributionCopyText&utm_medium=referral&utm_source=pexels)

# 重述移动平均线策略

让我们首先快速回顾一下我们在上一篇文章中构建的内容。我们的模型很简单，我们构建了一个脚本来计算和绘制短期移动平均线(20 天)和长期移动平均线(250 天)。

我推荐你看一下这篇文章，了解更多关于移动平均线和如何构建 Python 脚本的细节。在这篇文章中，我将只发布获取所选股票的移动平均线和股价的代码:

```
import requests
import pandas as pd
import matplotlib.pyplot as plt

def stockpriceanalysis(stock):
    stockprices = requests.get(f"https://financialmodelingprep.com/api/v3/historical-price-full/{stock}?serietype=line")
    stockprices = stockprices.json()

#Parse the API response and select only last 1200 days of prices
    stockprices = stockprices['historical'][-1200:]

#Convert from dict to pandas datafram

    stockprices = pd.DataFrame.from_dict(stockprices)
    stockprices = stockprices.set_index('date')
    #20 days to represent the 22 trading days in a month
    stockprices['20d'] = stockprices['close'].rolling(20).mean()
    stockprices['250d'] = stockprices['close'].rolling(250).mean()

stockpriceanalysis('aapl')
```

# 回溯测试策略

为了执行回溯测试，我们将:

*   当**短期移动平均线穿越长期移动平均线**上方时，做多 100 只股票(即买入 100 只股票)。这就是所谓的[黄金交叉。](https://www.investopedia.com/terms/g/goldencross.asp)
*   几天后卖出股票。例如，我们会保留库存 20 天，然后出售。
*   计算利润

这是一个非常简单的策略。我们将有所选股票的每日收盘价。该策略也可以用于分钟或小时数据，但我将保持简单，并基于每日数据执行回溯测试。

因为我不期望有很多进入点，也就是当我们购买股票时，为了简单起见，我将忽略交易成本。

还记得我们之前的帖子吗，如果我们通过将股票名称作为参数传递给 analyse 来运行脚本，我们将获得一个名为 ***stockprices*** 的 Pandas 数据帧，其中包含最近 1200 天的收盘价和移动平均线。

```
stockpriceanalysis('aapl')

#Outcome stored in a DataFrame call stockprices
	        close	20d	         250d
date			
2020-02-07	320.03	316.7825	224.76192
2020-02-10	321.55	317.3435	225.36456
2020-02-11	319.61	317.4760	225.96228
2020-02-12	327.20	318.2020	226.58788
```

# Python 中的回溯测试策略

为了建立我们的回溯测试策略，我们将首先创建一个包含我们每个多头头寸利润的列表。

首先(1)，我们创建一个新列，它将包含数据帧中所有数据点的 *True* ，在该数据帧中，20 天移动平均线与 250 天移动平均线相交。

当然，我们只对交叉发生的第一天或第二天感兴趣(即 20 日均线越过 250 日均线)。因此，我们定位交叉发生的第一个或第二个日期(行)( 2)。

正如在这篇[文章](https://economictimes.indiatimes.com/markets/stocks/news/moving-average-trading-rules/articleshow/71749811.cms?from=mdr)中所述，我们将使用两天规则([，即只有在被多一天的收盘](https://economictimes.indiatimes.com/markets/stocks/news/moving-average-trading-rules/articleshow/71749811.cms?utm_source=contentofinterest&utm_medium=text&utm_campaign=cppst)确认后，我们才开始交易，并且只有当连续两天 20 日均线高于 250 日均线时，我们才会保留该日期作为进场点。

这种方法将帮助我们避免日常交易噪音波动。当这种情况发生时，我们将在列 *firstbuy* 中设置入口点，其中值等于 True:

```
#Start longposiiton list

longpositions = []
# (1)
stockprices['buy'] =(stockprices['20d'] > stockprices['250d'])
# (2)
stockprices['firstbuy'] =   ((stockprices['buy'] == True) & (stockprices['buy'].shift(2) == False)& (stockprices['buy'].shift(1) == True))

# (3) identify the buying points 
buyingpoints = np.where(stockprices['firstbuy'] == True)
print(buyingpoints)

#Outcome
(array([ 307,  970, 1026]),)
```

规则*(股票价格【买入】。shift(2) == False)* ，帮助我们找出交叉发生后的第一个日期。

现在我们有了变量*买入点* (3)，我们应该用我们的长期策略进入市场的日期。

数组 *buyingpoints* 中的每个元素代表我们需要做多的行。因此，我们可以通过它们循环得到接近的价格，并购买 100 只股票(4)。

然后，我们保留这些股票 20 天(5)，并以+20 天收盘价卖出这 100 只股票。

```
#(4)
for item in buyingpoints[0]:
    #close price of the stock when 20MA crosses 250 MA 
    close_price = stockprices.iloc[item].close

    #Enter long position by buying 100 stocks
    long = close_price*100

    # (5) sell the stock 20 days days later:
    sellday = item + 20
    close_price = stockprices.iloc[sellday].close

    #Sell 20 days later:
    sell = close_price*100

    # (6) Calculate profit
    profit = sell - long

    longpositionsprofit.append(profit)
```

最后，我们计算利润，并将策略的结果添加到 *longpositionprofit* 数组(6)中。

# 我们战略的结果

要了解我们的策略执行得如何，我们可以打印出多头头寸利润列表并计算总额:

```
print(longpositionsprofit)
#outcome
(array([ 307,  970, 1026]),)

print(sum(longpositionsprofit))
#outcome
2100.0
```

太好了，我们对苹果的回溯测试策略显示，在 1200 多天里，我们进入了一个多头头寸，并在 20 天后总共卖出了三次。

第一次，我们获得了 307 美元的利润，第二次，970 美元，最后一个多头头寸，我们获得了 1026 美元的利润。总共是 2100 美元。

一点也不差。但是如果我们在 1200 天前买了这只股票并一直持有到今天会怎么样呢？通过在我们的*股票价格*数据框架中获得最后一个可用价格和第一个可用价格，我们可以很容易地计算出买入和持有的利润。

```
firstdayprice = stockprices.iloc[0].close
lastdayprice = stockprices.iloc[-1].close

profit = 100*(lastdayprice - firstdayprice)
profit

#Outcome
15906.999999999996
```

有趣的是，只要持有股票 1，200 天，我们的利润就是 15，906 美元加上年度股息。比我们采用移动平均线策略要高得多。

# 包扎

我们使用了一个简单的策略，当 20 日均线穿过 250 日均线时买入股票。然后，我们把股票保留了 20 天，然后才卖出。

我们可能还遵循了其他策略。例如，我们可以在移动平均线交叉时买入股票，并持有到最后。或者，如果 250 天移动平均线低于 20 天移动平均线，我们可以卖掉股票。

我会让你知道如何试验和测试这些其他的策略。很高兴在我的 Twitter 账户上收到您的反馈。

查看下面的 Python 脚本，对任何公司的移动平均线策略进行回溯测试。只要把苹果的 ***换成其他公司的*** 就行了

```
#Moving Averages and backtesting 
import requests
import pandas as pd
import matplotlib.pyplot as plt
import numpy as np

def stockpriceanalysis(stock):
    stockprices = requests.get(f"https://financialmodelingprep.com/api/v3/historical-price-full/{stock}?serietype=line")
    stockprices = stockprices.json()

#Parse the API response and select only last 1200 days of prices
    stockprices = stockprices['historical'][-1200:]

#Convert from dict to pandas datafram

    stockprices = pd.DataFrame.from_dict(stockprices)
    stockprices = stockprices.set_index('date')
    #20 days to represent the 22 trading days in a month
    stockprices['20d'] = stockprices['close'].rolling(20).mean()
    stockprices['250d'] = stockprices['close'].rolling(250).mean()

    ###STARTING BACKTESTING STRATEGY
    #Start longposiiton list
    longpositionsprofit = []

    shortpositions = []

    stockprices['buy'] =(stockprices['20d'] > stockprices['250d'])

    #We find the first True since it is first day when the line has crossed. Also, ensure that we have at least two days where sticoprices
    stockprices['firstbuy'] =   ((stockprices['buy'] == True) & (stockprices['buy'].shift(2) == False)& (stockprices['buy'].shift(1) == True))

    buyingpoints = np.where(stockprices['firstbuy'] == True)

    for item in buyingpoints[0]:
        #close price of the stock when MA crosses
        close_price = stockprices.iloc[item].close
        #Enter long position
        long = close_price*100
        sellday = item + 20
        close_price = stockprices.iloc[sellday].close
        #Sell 20 days later:
        sell = close_price*100

        #Calculate profti
        profit = sell - long

        longpositionsprofit.append(profit)

    #Result of Moving Average Strategy of going long
    print(longpositionsprofit )
    print(str(sum(longpositionsprofit)) + ' is the profit of Moving Averag Strategy')

    #Result of just holding the stock over 1200 days
    firstdayprice = stockprices.iloc[0].close
    lastdayprice = stockprices.iloc[-1].close

    profit = 100*(lastdayprice - firstdayprice)
    print(str(profit) + ' is the profit of holding over the selected period of 1200 days')

#Change apple for the ticker of any company you want to backtest
stockpriceanalysis('AAPL')
```

*原载于 2020 年 3 月 8 日 https://codingandfun.com**[*。*](https://codingandfun.com/backtesting-with-python/)*