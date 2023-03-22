# 如何评价自己的炒股模式

> 原文：<https://towardsdatascience.com/how-to-evaluate-your-stock-trading-model-e7e5e51d4320?source=collection_archive---------48----------------------->

## 提示:不要使用传统的损失函数

![](img/367da768a913697e0ef6a6c6bcb8aa5d.png)

在 [Unsplash](https://unsplash.com/s/photos/stock-trading?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) 上由 [Austin Distel](https://unsplash.com/@austindistel?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) 拍摄的照片

***来自《走向数据科学》编辑的提示:*** *虽然我们允许独立作者根据我们的* [*规则和指导方针*](/questions-96667b06af5) *发表文章，但我们并不认可每个作者的贡献。你不应该在没有寻求专业建议的情况下依赖一个作者的作品。详见我们的* [*读者术语*](/readers-terms-b5d780a700a4) *。*

当我偶尔观察到“使用 LSTMs 预测股价”时，准确性和盈利性的衡量标准是亏损，通常是均方误差函数。对于以预测值的准确性为最终目标的数据集来说，这一切都很好。

在股票交易的情况下，盈利是最终目标。那么如何才能量化这个价值呢？

# 概念:

评估模型的最佳值应该与项目的最终目标——盈利能力直接相关。所以我们知道脚本应该能够评估估计的盈利能力。

此外，为了进行盈利能力评估，程序必须对以前从未见过的数据进行处理。这意味着我们必须对数据进行测试，以检查一个人能赚多少钱。

有了利润估算程序的两个最重要的特征，让我们开始创建这个程序。

# 代码:

我为不同类型的交易准备了三种不同的脚本:

*   固定时间交易
*   正常股票交易
*   二元期权交易

## 固定时间交易:

```
def trading_simulator(trade_time,trade_value,close_price,sma_one,sma_two):
    intersections,insights = intersection(sma_1,sma_2)
    profit = 0
    logs = []
    for i in range(len(insights)):
        index = intersections[i]
        if insights[i] == buy:
            if index+trade_time < len(fx_data):
                if fx_data[index][-1] < fx_data[index+trade_time][-1]:
                    profit += trade_value * 0.8
                    logs.append(trade_value*0.8)
                elif fx_data[index][-1] > fx_data[index+trade_time][-1]:
                    profit -= trade_value
                    logs.append(-trade_value)
        elif insights[i] == sell:
            if index+trade_time <= len(fx_data):
                if fx_data[index][-1] > fx_data[index+trade_time][-1]:
                    profit += trade_value * 0.8
                    logs.append(trade_value*0.8)
                elif fx_data[index][-1] < fx_data[index+trade_time][-1]:
                    profit -= trade_value
                    logs.append(-trade_value)
        profit = profit
    return profit,logs
```

这个交易程序使用两个 SMA 值的交集来测试如果基于对两个定义的 SMA 值的交集的理解以及准确性(所有交易中盈利的交易数量)进行交易，将会赚多少钱。

这个程序基于固定时间交易的利润估计:一种交易策略，其中你预测在下一个时间框架内，如果价格将增加或减少。您可以修改程序，或者只是从这里复制整个脚本来测试利润估计:

```
import requests
import numpy as np
from matplotlib import pyplot as plt
import datetime
API_KEY = 'YOURAPIKEYHERE'
from_symbol = 'EUR'
to_symbol = 'USD'def sell():
    pyautogui.click(1350,320)

def buy():
    pyautogui.click(1350,250)

def SMA(prices,value):
    means = []    count = 0
    while value+count <= len(prices):
        pre_val = prices[count:value+count]
        count +=1
        means.append(np.mean(pre_val))
    return meansdef intersection(lst_1,lst_2):
    intersections = []
    insights = []
    if len(lst_1) > len(lst_2):
        settle = len(lst_2)
    else:
        settle = len(lst_1)
    for i in range(settle-1):
        if (lst_1[i+1] < lst_2[i+1]) != (lst_1[i] < lst_2[i]):
            if ((lst_1[i+1] < lst_2[i+1]),(lst_1[i] < lst_2[i])) == (True,False):
                insights.append(buy)
            else:
                insights.append(sell)
            intersections.append(i)
    return intersections,insightsdef trading_simulator(trade_time,trade_value,close_price,sma_one,sma_two):
    intersections,insights = intersection(sma_1,sma_2)
    profit = 0
    logs = []
    for i in range(len(insights)):
        index = intersections[i]
        if insights[i] == buy:
            if index+trade_time < len(fx_data):
                if fx_data[index][-1] < fx_data[index+trade_time][-1]:
                    profit += trade_value * 0.8
                    logs.append(trade_value*0.8)
                elif fx_data[index][-1] > fx_data[index+trade_time][-1]:
                    profit -= trade_value
                    logs.append(-trade_value)
        elif insights[i] == sell:
            if index+trade_time <= len(fx_data):
                if fx_data[index][-1] > fx_data[index+trade_time][-1]:
                    profit += trade_value * 0.8
                    logs.append(trade_value*0.8)
                elif fx_data[index][-1] < fx_data[index+trade_time][-1]:
                    profit -= trade_value
                    logs.append(-trade_value)
        profit = profit
    return profit,logsclose_price = []
r = requests.get(
        '[https://www.alphavantage.co/query?function=FX_INTRADAY&from_symbol='](https://www.alphavantage.co/query?function=FX_INTRADAY&from_symbol=') +
        from_symbol + '&to_symbol=' + to_symbol +
        '&interval=1min&outputsize=full&apikey=' + API_KEY)
jsondata = json.loads(r.content)
pre_data = list(jsondata['Time Series FX (1min)'].values())
fx_data = []
for data in pre_data: 
    fx_data.append(list(data.values()))
fx_data.reverse()
for term in fx_data:
    close_price.append(float(term[-1]))sma_1 = SMA(close_price,2)
sma_2 = SMA(close_price,1)
profit,logs = trading_simulator(1,10,close_price,sma_1,sma_2)
profit
```

为了让程序工作，你必须用你自己的 API 密匙替换 API 密匙参数。

## 正常交易:

正常的交易是买入或卖出一定数量的股票，所赚的利润是股票价格的差额。

```
def estimate_profits(standard_qty,model,data,close,open_,high,low):
    close_prices = close
    open_prices = open_
    low_prices = low
    high_prices = high
    pred_close = list()
    pred_open = list()
    orders = list()
    profits = list()

    for i in range(len(data)):
        pred = model.predict(data[i])[0]
        open_price,close_price = pred
        if open_price > close_price:
            side = -1
        elif open_price < close_price:
            side = 1
        qty = standard_qty
        orders.append([side,qty])
        pred_close.append(close_price)
        pred_open.append(open_price)

    for i in range(len(data)):
        sign = 0
        mult = 0
        side,qty = orders[i][0],orders[i][1]
        dif = close_prices[i] - open_prices[i]
        if dif > 0:
            sign = 1
        else:
            sign = -1
        if sign == side:
            mult = 1
        else:
            mult = -1
        profit = dif*mult*qty
        profits.append(profit)
    return profits
```

该程序计算资产在制造时和下一次制造时之间的差异。它将这一差额乘以股票数量来计算利润。你可以调整时间步长，测试更长时间的交易策略。

## 二元期权交易:

```
def trading_simulator(open_prices):
    profit = 0
    insights = []
    logs = []
    for i in range(len(pred)):
        if float(open_prices[i]) > pred[i][0]:
            insights.append('sell')
        elif float(open_prices[i]) < pred[i][0]:
            insights.append('buy')
    for i in range(len(insights)):
        if insights[i] == 'sell':
            if float(open_prices[i]) > y[i]:
                profit += 8
                logs.append(8)  
            else:
                profit -= 10
                logs.append(-10)

        if insights[i] == 'buy':
            if float(open_prices[i]) < y[i]:
                profit += 8
                logs.append(8)  
            else:
                profit -= 10
                logs.append(-10)

    return profit,logs
```

该函数计算利润，考虑交易金额的固定 80%利润。它计算当前值和下一个时间步长值之间的差值。

# 结论:

我真的希望人们可以使用程序的盈利能力来评估他们的模型，以便真正了解交易算法是否有效。

# 我的链接:

如果你想看更多我的内容，点击这个 [**链接**](https://linktr.ee/victorsi) 。