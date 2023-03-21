# Pandas 中类似 SQL 的窗口函数

> 原文：<https://towardsdatascience.com/sql-window-functions-in-python-pandas-data-science-dc7c7a63cbb4?source=collection_archive---------6----------------------->

## 所有熊猫窗口功能的单一位置

![](img/5b6dccc9efc61802a083ee011cdc6619.png)

在 [Unsplash](https://unsplash.com/s/photos/row-number?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) 上由 [Waldemar Brandt](https://unsplash.com/@waldemarbrandt67w?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) 拍照

在 SQL 世界中，窗口函数非常强大。然而，没有一个写得很好的和统一的熊猫的地方。在熊猫网站的[上非常详细地介绍了用熊猫编写类似 SQL 代码的基础知识。然而，Pandas 指南缺乏对 SQL 及其 Pandas 对等物的分析应用程序的良好比较。](https://pandas.pydata.org/docs/getting_started/comparison/comparison_with_sql.html)

在这篇文章中，我将介绍一些窗口函数的 SQL 等价物，并带你了解熊猫的窗口函数。

# 。groupby 是你的朋友

如果你做大数据分析，并且发现自己需要使用 Pandas，那么可能很难从富有表现力的 SQL 语言过渡到 Pandas 的数据操作方式。

我打算这篇文章简短而甜蜜，主要关注实现的战术方面，而不是细节。这篇文章假设你熟悉窗口和分区函数。让我们把最常见的 SQL 分析函数翻译成熊猫。

## 每个组(分区)中的行号

Pandas 相当于每个分区中的行号，带有多个 sort by 参数:

> SQL:

```
ROW_NUMBER() over (PARTITION BY ticker ORDER BY date DESC) as days_lookback
---------
ROW_NUMBER() over (PARTITION BY ticker, exchange ORDER BY date DESC, period) as example_2
```

> 熊猫:

```
df['days_lookback'] = df.sort_values(['date'], ascending=False)\
             .groupby(['ticker'])\
             .cumcount() + 1
---------
df['example_2'] = df.sort_values(['date', 'period'], \
             ascending=[False, True])\
             .groupby(['ticker', 'exchange'])\
             .cumcount() + 1
```

## 每个组(分区)中的最后/下一个记录

熊猫相当于超前和滞后窗口功能:

> SQL:

```
LAG(price) over (PARTITION BY ticker ORDER BY date) as last_day_px
---------
LEAD(price) over (PARTITION BY ticker ORDER BY date) as next_day_px
```

> 熊猫:

```
df['last_day_px'] = df.sort_values(by=['date'], ascending=True)\
                       .groupby(['ticker'])['price'].shift(1)
---------
df['next_day_px'] = df.sort_values(by=['date'], ascending=True)\
                       .groupby(['ticker'])['price'].shift**(-1)**
```

## 各组内的百分位数等级

百分比等级/密集等级或等级窗口函数的 Pandas 等价物:

> SQL:

```
PERCENT_RANK() OVER (PARTITION BY ticker, year ORDER BY price) as perc_price
```

> 熊猫:

```
df['perc_price'] = df.groupby(['ticker', 'year'])['price']\
                        .rank(pct=True)
```

## 每组内的运行总和

Pandas 相当于滚动求和、运行求和、求和窗口函数:

> SQL:

```
SUM(trade_vol) OVER (PARTITION BY ticker ORDER BY date ROWS BETWEEN 3 PRECEEDING AND CURRENT ROW) as volume_3day
---------
SUM(trade_vol) OVER (PARTITION BY ticker ORDER BY date ROWS BETWEEN UNBOUNDED PRECEEDING AND CURRENT ROW) as cum_total_vol
---------
SUM(trade_vol) OVER (PARTITION BY ticker) as total_vol
```

> 熊猫:

```
df['volume_3day'] = df.sort_values(by=['date'], ascending=True)\
                       .groupby(['ticker'])['trade_vol']\
                       .rolling(3, min_periods = 1).sum()\
                       .reset_index(drop=True, level=0)
---------
df['cum_total_vol'] = df.sort_values(by=['date'], ascending=True)\
                       .groupby(['ticker'])['trade_vol']\
                       .cumsum()
---------
df['total_vol'] = df.groupby(['ticker', 'year'])['trade_vol']\
                        .transform('sum')
```

## 每组内的平均值

Pandas 相当于平均线、移动平均线(移动平均线)窗口函数:

> SQL:

```
AVG(trade_vol) OVER (PARTITION BY ticker) as avg_trade_vol,
---------
AVG(price) OVER (PARTITION BY ticker ORDER BY date ROWS BETWEEN 19 PRECEDING AND CURRENT ROW) as ma20
```

> 熊猫:

```
df['avg_trade_vol'] = df.groupby(['ticker', 'year'])['trade_vol']\
                        .transform('mean')
---------
df['ma20'] = df.sort_values(by=['date'], ascending=True)\
                    .groupby('ticker')['price']\
                    .rolling(20, min_periods = 1).mean()\
                    .reset_index(drop=True, level=0)
```

[*。transform*](https://pandas.pydata.org/pandas-docs/stable/user_guide/groupby.html#transformation) 是 Pandas 中一个强大的命令，我邀请您在这里了解更多信息——

[](/pandas-groupby-aggregate-transform-filter-c95ba3444bbb) [## 熊猫小组详细解释了

### 了解如何掌握所有熊猫的分组功能，如聚集(聚合)，转换和过滤-代码指南…

towardsdatascience.com](/pandas-groupby-aggregate-transform-filter-c95ba3444bbb) 

就是这样！上面的例子应该给你提供了一个 Pandas 窗口函数的参考。

为便于测试，上述示例的数据集示例如下:

```
import pandas as pd
data = {
    'year':[2019, 2019, 2020, 2020, 2020, 2020, 2019, 2020, 2020, 2020],
    'date':['2019-12-30', '2019-12-31', '2020-01-02', '2020-01-03', '2020-01-06', '2020-01-07', '2019-12-31', '2020-01-02', '2020-01-03', '2020-01-06'],
    'ticker':['BYND', 'BYND', 'BYND', 'BYND', 'BYND', 'BYND', 'FB', 'FB', 'FB', 'FB'],
    'exchange':['Q', 'Q', 'Q', 'Q', 'Q', 'Q', 'N', 'N', 'N', 'N'],
    'price':[74.15, 75.60, 75.64, 75.41, 74.59, 83.89, 205.25, 209.78, 208.67, 212.60],
    'trade_vol':[2548100, 2002000, 2221700, 1628700, 2324700, 12044600, 8953500, 12077100, 11188400, 17058900],
}
df = pd.DataFrame(data)
```