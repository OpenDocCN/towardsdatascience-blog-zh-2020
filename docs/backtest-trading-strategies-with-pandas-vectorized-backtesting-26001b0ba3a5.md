# 熊猫交易策略的回溯测试——向量化回溯测试

> 原文：<https://towardsdatascience.com/backtest-trading-strategies-with-pandas-vectorized-backtesting-26001b0ba3a5?source=collection_archive---------7----------------------->

## 一种快速有效的交易策略评估方法

啊哈！你在市场上发现了一种模式，这种模式有可能让你成为下一个沃伦巴菲特……现在该怎么办？在设计了一个新的交易策略，可以系统地从这个模式中获利后，现在是时候问自己:“如果五年前我有这个策略会怎么样？”。

今天，我将讨论矢量化回溯测试如何帮助您“回答”这个问题，如何使用 Pandas 实现它，以及您应该注意哪些微妙之处。对于在这方面有一些经验的人，可以直接跳到第 5 部分。

([你可以在这里找到完整的代码和其他资源](https://www.yaoleixu.com/quant-finance)

# 1.什么是回溯测试？

> B acktesting 用于模拟交易策略的过去表现。

回溯测试的核心概念是通过回到过去来模拟给定的交易策略，并像在过去一样执行交易。产生的利润通常通过一些指标(如最大提款、夏普比率、年化回报等)与基准业绩进行比较。).根据策略的性质，应该使用不同的基准和度量标准，但这本身就是一个完整的主题，所以我在这里不会详细讨论。

# **2。什么回测不是**

> B acktesting 不是过去绩效的准确指标，不应作为研究工具，尤其是对没有经验的人而言。

有趣的是，回溯测试并不能很好地表明如果你能回到过去，你今天会有多富有。除了时间旅行，几乎不可能精确复制过去的表现，因为有很多因素太复杂了，无法精确建模(滑点，交易的市场影响等)。).此外，对于没有经验的人来说，回溯测试可能充满偏差(例如，前瞻偏差、幸存者偏差等)。)可以使战略看起来比实际情况好得多(见 5.3 节)。

最后，回溯测试不是一个研究工具。不要仅仅因为在回溯测试中看起来更好就随意改变你的策略参数。你应该将市场的历史表现视为随机变量的多种可能实现之一。在没有合理的经济逻辑的情况下，将你的参数用于回溯测试将不可避免地导致过度拟合。

# **3。事件驱动与矢量化回溯测试**

> V 结构化回测比事件驱动回测快得多，应该用于策略研究的探索阶段。

对策略进行回溯测试有两种主要方法:(I)事件驱动和(ii)矢量化。事件驱动的回溯测试通常涉及到使用一个随时间迭代的循环，模拟一个根据市场信号发送订单的代理。这种基于循环的方法非常灵活，允许您模拟订单执行中的潜在延迟、滑动成本等。

相反，矢量化回溯测试收集与策略相关的数据，并将它们组织成向量和矩阵，然后通过线性代数方法(计算机非常擅长的方法)进行处理，以模拟过去的表现。

由于线性代数的表达能力，矢量化回溯测试比事件驱动的回溯测试快得多，尽管它缺乏灵活性。通常，向量化的回溯测试更适合于初始的探索性策略分析，而事件驱动的回溯测试更适合于更深入的分析。

# **4。关于日志返回的注释**

> 可以使用对数收益的累积和来计算策略绩效。

在深入研究 python 代码之前，理解对数回报的概念很重要，对数回报被定义为时间间隔( *t-1，t* )内资产的对数价值的差异:

![](img/f618499342d825c9fab6063ed3b66772.png)

由于许多原因(平稳性、对数正态性等)，对数收益在定量金融中非常有用。)，但出于回溯测试的目的，我们将使用它的时间累加属性来计算累积回报(即过去的表现)。

例如，考虑一项资产在几天内的对数收益 *(t=3，2，1* )。为了计算该资产在整个期间的总收益，我们简单地将所有日收益相加( *r[3]+r[2]+r[1]* )。这是因为日志价格信息被取消，如下所示:

![](img/e5ff4e4453745277546b07eb8c5524ef.png)

因此，如果我想知道资产增长了多少，我只需要用指数函数去掉最终的自然对数。在上面的例子中，资产价值增加了一个系数( *p[3]/p[0]* )。

上述论点也适用于你的策略。例如，如果您的策略在 *T* 天内产生了对数回报( *r[0]，r[1]，…，r[T]* ，那么策略的回溯测试可以通过对数回报的简单累积和以及指数函数来计算。

# **5。熊猫的矢量化回溯测试**

## 5.1.单一资产回溯测试

假设我们想对价格移动平均线交叉策略的表现进行回溯测试，如果短期价格平均线高于长期价格平均线，则您做多标的资产，反之亦然。我们可以用 6 个步骤来计算策略绩效:

(1)计算标的资产的对数收益。在这个例子中，我们正在查看 AAPL 股票的对数价格差异，该股票存储在按时间索引的 Pandas 系列中。

```
rs = prices.apply(np.log).diff(1)
```

(2)计算信号

```
w1 = 5 # short-term moving average window
w2 = 22 # long-term moving average window
ma_x = prices.rolling(w1).mean() - prices.rolling(w2).mean()
```

(3)将信号转化为实际交易头寸(多头或空头)

```
pos = ma_x.apply(np.sign) # +1 if long, -1 if shortfig, ax = plt.subplots(2,1)
ma_x.plot(ax=ax[0], title='Moving Average Cross-Over')
pos.plot(ax=ax[1], title='Position')
```

![](img/833876399e25ec0287deb8e37361825b.png)

作者图片

(4)通过根据头寸调整基础资产的对数回报来计算策略回报(策略回报的计算并不精确，但在实际水平上通常是足够好的近似值)。此外，请注意，位置已经移动了 1 个时间步长，这样做是为了避免前瞻偏差(参见第 5.3 节)。)

```
my_rs = pos.shift(1)*rs
```

(5)通过使用累积和并应用指数函数来计算回测。

```
my_rs.cumsum().apply(np.exp).plot(title=’Strategy Performance’)
```

![](img/8cf50303817dc26494cd88b312cb1fde.png)

作者图片

## 5.2.多资产回溯测试

类似于单个资产的情况，我们可以使用 Pandas 计算资产组合的回测。这里唯一的不同是我们正在处理一个熊猫数据框架，而不是熊猫系列。

让我们首先计算每个资产的信号和位置，如下面的代码所示。请注意，我们已经调整了头寸，因此在任何给定时间，投资组合头寸的总和都不超过 100%(即假设没有使用杠杆)。

```
rs = prices.apply(np.log).diff(1)w1 = 5
w2 = 22
ma_x = prices.rolling(w1).mean() - prices.rolling(w2).mean()pos = ma_x.apply(np.sign)
pos /= pos.abs().sum(1).values.reshape(-1,1)fig, ax = plt.subplots(2,1)
ma_x.plot(ax=ax[0], title='Moving Average Cross-Overs')
ax[0].legend(bbox_to_anchor=(1.1, 1.05))
pos.plot(ax=ax[1], title='Position')
ax[1].legend(bbox_to_anchor=(1.1, 1.05))
```

![](img/2e4d83373fc3f02bf1329b6398be6ffc.png)

作者图片

我们可以从单个资产的角度来看该策略的表现:

```
my_rs = (pos.shift(1)*rs)my_rs.cumsum().apply(np.exp).plot(title=’Strategy Performance’)
```

![](img/0f119e45b955893e9d9204600250b43c.png)

作者图片

我们还可以看看该策略在投资组合层面的总体表现:

![](img/22faa319440c608efab4ad20e450661f.png)

作者图片

## 5.3。关于前瞻偏差的快速注释

在上面的例子中，位置向量/矩阵总是移动 1 个时间步长。这样做是为了避免前瞻性偏见，这种偏见会使战略看起来比实际情况好得多。

这个想法是:在时间 *t* ，你有一个信号，它包含的信息直到并包括 *t* ，但是你实际上只能在 *t+1* 交易那个信号(例如，如果你的信号包含一家公司在周一晚上发布的收益信息，那么你只能在周二早上交易那个信息)。

如下所示，前瞻偏见会使战略看起来比实际更有希望(尤其是对于“动量”战略)。

```
my_rs1 = (pos*rs).sum(1)
my_rs2 = (pos.shift(1)*rs).sum(1)my_rs1.cumsum().apply(np.exp).plot(title='Look-Ahead Bias Performance')
my_rs2.cumsum().apply(np.exp).plot()
plt.legend(['With Look-Ahead Bias', 'Without Look-Ahead Bias'])
```

![](img/947c88a838c66c69a7eb0b119480ce7d.png)

作者图片

## 5.4.评估对延迟的鲁棒性

有了向量化回溯测试，评估策略对延迟的稳健性就容易多了(即在信号发出后 T 个时间点开始交易)。然而，这种分析在高频交易级别更有用，因为延迟会严重影响业绩。

```
lags = range(1, 11)
lagged_rs = pd.Series(dtype=float, index=lags)for lag in lags:
    my_rs = (pos.shift(lag)*rs).sum(1)
    my_rs.cumsum().apply(np.exp).plot()
    lagged_rs[lag] = my_rs.sum()plt.title(‘Strategy Performance with Lags’) 
plt.legend(lags, bbox_to_anchor=(1.1, 0.95))
```

![](img/46f61ba4b2bcdff4024841ebbd480522.png)

作者图片

## 5.5.模拟交易成本

交易成本通常以固定百分比报价。为了将这些纳入回测，我们可以简单地计算出投资组合在连续时间步之间的变化，然后直接从我们的策略回报中减去相关的交易成本。

```
tc_pct = 0.01 # assume transaction cost of 1% delta_pos = pos.diff(1).abs().sum(1) # compute portfolio changes
my_tcs = tc_pct*delta_pos # compute transaction costsmy_rs1 = (pos.shift(1)*rs).sum(1) # don't include costs
my_rs2 = (pos.shift(1)*rs).sum(1) - my_tcs # include costsmy_rs1.cumsum().apply(np.exp).plot()
my_rs2.cumsum().apply(np.exp).plot()
plt.legend(['without transaction costs', 'with transaction costs'])
```

![](img/4aa9f37f801e50d03844efb3cfca2b09.png)

作者图片

我希望你喜欢这篇文章！如果你想看更多这样的内容，请关注我。

此外，查看[我的网站](https://www.yaoleixu.com/quant-finance)的完整代码和其他资源。

***注来自《走向数据科学》的编辑:*** *虽然我们允许独立作者根据我们的* [*规则和指导方针*](/questions-96667b06af5) *发表文章，但我们不认可每个作者的贡献。你不应该在没有寻求专业建议的情况下依赖一个作者的作品。详见我们的* [*读者术语*](/readers-terms-b5d780a700a4) *。*