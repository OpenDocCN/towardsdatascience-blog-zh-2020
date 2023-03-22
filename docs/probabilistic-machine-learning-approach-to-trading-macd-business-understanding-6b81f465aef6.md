# 交易的概率机器学习方法+ MACD 商业理解

> 原文：<https://towardsdatascience.com/probabilistic-machine-learning-approach-to-trading-macd-business-understanding-6b81f465aef6?source=collection_archive---------6----------------------->

## 在文章的最后，你会知道如何用概率的方式构造一个交易问题。此外，您将通过听取专家的意见来学习如何改进特征工程流程。最后，我们将用真实数据训练一个随机森林来应用这些概念！。(奖励音轨:很酷的可视化效果)。

# 0)我们的动机

> 故事是这样的:我们是投资者，我们有，比如说，1，199 美元(很快你就会明白为什么我用的是 1，199 美元而不是 1，000 美元)，我们的目标是最大限度地利用它们(我们会想要别的吗？).我们决定进入加密市场，因为它的波动性。为了维持我们的合作关系，我们需要持续盈利。我们希望使用机器学习，同时不忘记交易者使用的业务理解。现在，我们唯一需要知道的是如何做到的(哇…我肯定会在我的下一场自由泳比赛中使用它)。我们能做到吗？我们来想办法吧！

# 0.a)我能从这里学到新的东西吗？

快速回答:**是的**，那是我的本意。我邀请你在接下来的 18 分钟里——根据中等估计——和我在一起。如果你现在没有时间，就把这个链接加到你的记事本上，但是为了充分利用这里，我希望你能集中所有的注意力。我知道我要求很多，但我向你保证这是值得的。那么，我为什么要写这个呢？：

**第一个**:我看过很多与交易相关的文章，这些文章用令人难以置信的模型训练，使用令人印象深刻的算法，很酷的可视化，以及出色的写作风格。但我没有找到更详细的解释，这是一个简单的例子，基本理解如何在现实世界中使用它们。

**第二个**:大量的这些文章可以归类如下:“用你自己的交易机器人设定你的策略”和“预测股票价格的最佳机器学习模型”

“交易机器人”类型很好理解能够充分反映指标和可观察图表行为之间的相互作用的逻辑:**商业理解。**另一方面，机器学习模型非常擅长使用指标作为变量来捕捉更广泛的市场场景:**数据科学知识。**

![](img/7bd0abd4c2049238fc9d7632e7ac5de3.png)

从[夹住](https://imgflip.com/memegenerator)。

然而，我错过的是这些概念的融合。交易机器人的问题是，简单的刚性策略可能没有其他指标的支持。另一方面，“股票价格”类型关注指标，但它们缺乏背景，没有背景，它们通常是无用的，或者至少不太有效。

# 0.b)议程

正如你所看到的，我会试着给你我对如何解决这些问题的看法。为了将重点放在最近暴露的概念上，我将留下模型训练部分的一些细节(如模型选择或超参数调整)。测试的想法将是，如果我们设法抓住这些业务概念并将它们与我们的数据科学知识相结合，我们可以在多大程度上提高我们的准确性。我们的议程:

1.  我将首先向你介绍一些必要的概念。在开始考虑训练我们的模型之前，只是一些基本的东西。
2.  我们将讨论费用如何通过影响我们的预期回报来影响我们的决策。
3.  我们将讨论捕获风险管理程序的候选模型的要求。
4.  最后，我们将描述如何获取市场的商业解释，然后用它来增强我们的模型。这里是工作完成的地方，最令人兴奋的部分！如果你了解背景，你可以快速阅读其他部分，但重点是这里。*你不会后悔的。*
5.  我们将简要回顾一下我们的主要观点。

# 1)风险管理

首先，让我们从它在交易环境中的定义开始。

[*风险管理*](https://tradingstrategyguides.com/trading-risk-management-strategy/) *:“是用于减轻或保护您的个人交易账户免受失去所有账户余额的危险的过程。”*

这个过程涉及不同的决策，如果你愿意，它可以获得难以置信的复杂性。目前，这超出了我们的范围，所以我们将只关注其中的两个:

*   [**预期收益**](https://en.wikipedia.org/wiki/Expected_return) **:** 是交易收益的期望值。
*   **最大风险**:你愿意为一笔交易放弃的总资金的最大数额。

# 1.a)预期回报

要将我们的赌注转化为计算好的走势，我们需要评估这种情况，以了解我们的胜算。因为我们不是赌徒，我们需要定义我们的[止损点](https://www.investopedia.com/articles/stocks/09/use-stop-loss.asp) (S/L)和[止盈点](https://www.investopedia.com/terms/t/take-profitorder.asp) (T/P)。这些是我们定义的结束我们交易的价值观。

想象一个乌托邦世界，新冠肺炎被根除，以太坊再次达到 1000 美元。那时，我们决定买一些。此外，我们认为，如果价格下跌超过 20%，我们可能处于危险的境地，所以如果价格超过 800 美元，我们将出售它。另一方面，如果价格上涨超过 25%，我们会很乐意卖掉它，获得 250 美元的利润。

![](img/daedb2e1480e235bc96515cce2eb94ca.png)

情绪状态，S/L 红线和 T/P 绿线。此处代码[。](https://github.com/MauricioLetelier/Trading_And_Visualizations/blob/master/Emotional_states.ipynb)

如果我们忘记了时间变量，在每笔交易中，我们只有两种结果，我们可以赢，也可以输，取决于哪个价格先到达(S/L 或 T/P)。想象一下，我们认为我们有 45%的机会赢，55%的机会输。预期回报计算如下。

![](img/e46a2bf4078a21fa6705a6b760ab6434.png)

来自[维基百科](https://en.wikipedia.org/wiki/Expected_return)的公式。

![](img/9c3e11cd1d2dec25c265dc36d76c2d12.png)

这是我自己做的。

你可以看到，这是所有给定案例的概率和它们的回报的乘积之和。在我们的例子中，每笔交易的预期回报是 2.5 美元。这意味着，如果机会是正确的，我们重复这个过程，例如，100 次，我们将获得大约 250 美元的利润。

# 1.b)最大风险

另一个重要的任务是确定我们愿意在一次交易中损失的最大金额。这是因为，如果我们在一次交易中冒险投入 20%的资金，我们很有可能会输掉前三次交易。我们最终会拿走一半的钱，热泪盈眶，我们美好的友谊也可能就此结束。

![](img/696690e0b537298496d1fd08a5d305de.png)

[Icons8 团队](https://unsplash.com/@icons8?utm_source=medium&utm_medium=referral)在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上的照片

为了避免这种情况，因为从这样的损失中恢复是困难的，也因为我非常珍惜我们的友谊，我建议你使用典型的 1%法则。这意味着，如果我们有 1，199 美元，我们不允许每笔交易损失超过 11.99 美元。

我是一个喜欢冒险的人，所以我强迫你在每笔交易中，我们的亏损场景将是定义的最大 1%的亏损(这是为了我们的例子简单起见，我没有那么糟糕)。在这样做的时候，有一些重要的事情需要考虑，独立于我们的结果，我们需要支付费用。多么痛苦的现实啊！

# 2)现实生活

假设我们为菜鸟交易者使用市场上最好的费用。我们选择[币安](https://www.binance.com/)。他们对接受者和制造者收取 0.1%的佣金。这意味着，如果我们用 1.000 [USDT](https://blockgeeks.com/guides/what-is-tether/) (这里是[为什么](https://www.quora.com/Is-it-possible-to-convert-my-Bitcoin-to-USD-using-Binance) USDT 而不是美元)购买以太坊，我们需要支付 1 USDT 费用，仅仅是为了购买。

![](img/bc7f8a067a9c8ee858f83e3d0299d378.png)

来自[币安](https://www.binance.com/en/fee/schedule)的费用表。

我们的 S/L 和 T/P 将分别为-1%和 1%(请记住，它可以是您想要评估的任何 S/L 和 T/P 点)。如果价值达到 990，我们有义务出售，但是不要忘记，我们需要为第二次交易支付 0.99 USDT。在这种情况下，我们损失了 11.99 USDT，这是我们 1，199 美元初始资本所能承受的最大金额，现在完全说得通了。

如果价格首先到达 T/P 点，我们得到 10 USDT 的利润和 2.01 USDT 的费用，这意味着我们将得到 7.99 USDT 的正结余。鉴于这种情况，我们需要在这方面投资的积极成果的可能性有多大？如果你快速回答了 60%,说明你拥有令人印象深刻的快速计算技能，下面是数学程序，假设 P(n)是负面结果的概率，P(p)是正面结果的概率，并且
1-P(n)=P(p)。

![](img/4c15eab6c0d97dbc9733501e52b79733.png)

这也是我自己做的

我们需要我们的预期收益(1)大于或等于零。否则，我们将持续亏损。由此得出的一个重要结论是，如果我们提高 S/L 和 T/P 百分比，我们不需要 P(p)那么高。例如，如果我们用 3%和-3%来评估我们的最后一个场景，我们只需要 53.3%来进行交易。

这就是为什么我羡慕每一个能在 [RobinHood](https://robinhood.com/us/en/) 上创建账户的人。我不被允许，因为我“来自美国之外”如你所见，无费用交易让你处于更有利的位置。你不会开始输。不过，我已经抱怨够了，让我们去看看是什么让我们来到这里。

# 3)使用机器学习定义赔率

我们谈了很多关于预期回报的问题，所以我们肯定会使用这种方法，但这不是唯一的方法。事实上，我见过的最常见的方法是使用波动性的测量值(在[这里](/using-reinforcement-learning-to-trade-bitcoin-for-massive-profit-b69d0e8f583b)有清楚的解释)来估计交易的风险。我告诉你这个是因为这是标准的近似值——这里不会用来维护结构——但也是一个很好的选择。

我们都很聪明，但在密码市场没有多少经验。所以我们明白，我们的勇气和我们曾经看过的[布林线](https://medium.com/coinmonks/becoming-a-trader-data-scientist-transforming-bollinger-bands-part-4-8aab6fbc2f2d)文章，不足以定义与我们的 S/L 和 T/P 点相关的概率。现在我们的目标是思考哪种机器学习算法最适合这个问题。

我们看到这个问题由一个积极的和一个消极的结果组成。因此，我们可以断言这是一个标准的二进制分类问题。此外，我们需要一个分类器，能够给出在特定情况下每种结果的概率。理想情况下是这样的:

![](img/b8a4c3c3b6bdbccdbde3c8eb6b4dbc9d.png)

在理想世界中，你的模型给出了每一点的 P(p)。这里的代码。

为什么那样？因为我们需要知道几率来检查我们交易的预期收益是否为正。对我们来说，一个简单的“将要上涨”的信号是不够的。这种类型的模型被归类为[一类概率估计](https://www.quora.com/What-is-class-probability-estimation-in-machine-learning)。交易不是赢得所有的进场。这是概率问题！

尽管我们可以出于不同的动机(KNN，SVM，所有不同的自回归模型，K-Means，以及许多其他模型)放弃许多选项，但我们仍然有一些优秀的候选人。

我们的一些选择是朴素贝叶斯分类器、逻辑回归、决策树、随机森林或不同类型的神经网络。候选人少了，工作量也就少了。测试他们所有人的结果将会是惊人的。正如我之前告诉您的，这超出了本文定义的范围。但是对于我们的例子，我们将训练一个随机森林。

# 4)如何捕捉交易逻辑作为机器学习问题的变量？

我想用一个例子让你们对此有所了解。主要的想法是理解我们的专家提供的见解:交易者！如果他们一整天都在做这些，我们需要保持谦卑，听听他们要告诉我们什么。这是这个问题中的关键过程，也是你愿意解决的任何问题中的关键过程**。我们需要这样的商业环境。**

![](img/c05a3df0276fe48fb475fcfb2f88251a.png)

来自[img lip](https://imgflip.com/memegenerator/170569436/I-too-am-extraordinarily-humble)。上下文[此处](https://knowyourmeme.com/memes/i-too-am-extraordinarily-humble)。

我们的目标是做最好的功能工程！。创建变量**的过程需要**基于商业概念。只有在这之后，我们才能应用所有常见的转换，如标准化或规范化(只有最后一部分才有意义！).

这一部分的目标是比较第一种简单方法的性能，然后是交易机器人近似方法的性能，最后，尝试将所有部分结合在一起。我们将假设我们只能持有[多头头寸](https://www.investopedia.com/ask/answers/100314/whats-difference-between-long-and-short-position-market.asp)，正因为如此，我们只对正面结果的预测感兴趣。

> **PS:我会把这部分代码全部留在我的**[**github**](https://github.com/MauricioLetelier/Trading_And_Visualizations)**上。尽管有一些例外，代码将是通用的，以获得流动性。此外，由于抽样的随机性，一些结果可能会发生变化(我测试过，变化不是很大)。**

# 4.1)我们的数据

我们将从币安检索我们的数据，你可以阅读这个博客来创建你的 API-KEY。[这里的](https://python-binance.readthedocs.io/en/latest/)是 python-币安的文档。我们将使用 5 分钟间隔的数据。下面是收集数据的代码(3 行数据，我正在转换成 Flash？).

来自币安的 5 分钟间隔数据。(我们的例外)。

# 4.2)我们如何标记我们的 0 和 1？

我做了一个方法，决定先发生什么:增加 0.5%还是减少 0.5%。逻辑如下:该方法寻找接下来的 100 个时间段(对于 5 分钟的数据，超过 8 小时)并检查价格是否超出我们定义的阈值，仅考虑第一个达到的值。

我只是使用了接近的价格，因为高值和低值可能同时高于或低于我们的目标，给我们带来一些麻烦。不出所料，并不是在所有的情况下，都有结果，但是百分比并不高，我们可以去掉那些数据。

> 技术说明:收集的数据涵盖了截至 2020 年 4 月 22 日的前 500 天。我用 1 标记+0.5%的结果，用 0 标记-0.5%的结果。如你所见，数据没有明显的不平衡。在总共 143，542 个数据点(120，245 个用于训练，23，297 个用于测试结果)中，数据点 49.85%为阴性，49.82%为阳性，1.26%没有结果。

![](img/4cabbc7baebb877afdafccde817f05b2.png)

# 4.3)指示器

[TA 包](https://github.com/bukosabino/ta)有一个方法可以构造很多有用的指标。最棒的是，我们需要选择的所有参数都有自己的默认值。然而，如果我们愿意，我们可以改变它们。

```
import pandas as pd 
import numpy as np
import ta
df=pd.read_csv('Binance-ETHUSDT-22-04-2020')
df['timestamp']=pd.to_datetime(df['timestamp'],format='%Y-%m-%d %H:%M:%S')
df.set_index('timestamp',inplace=True)
df=df.astype(float)
df1 = ta.add_all_ta_features(
    df, open="open", high="high", low="low", close="close", volume="volume")
```

# 4.4) MACD

交易者是业务理解者，如果我们的模型仅仅基于技术分析——这可能反映了新闻——我们需要了解指标！。但是正如我之前告诉你的，这些人如此努力地将他们联系起来定义策略，以至于他们最终获得了对组合的直觉。我们的工作是将直觉转化为数学！。

作为一个例子，我想和你谈谈移动平均线收敛发散( [MACD](https://www.investopedia.com/terms/m/macd.asp) )指标。这是交易者之间最常见和广泛传播的方式之一。MACD 的计算方法是从短期均线中减去长期指数移动平均线([均线](https://www.investopedia.com/terms/e/ema.asp))。之后，你创建一条“信号线”，这是 MACD 的均线。最常用的参数分别是 26、12 和 9。

![](img/dd184822365ddde5faed1ed49eb960ce.png)

4 月 10 日和 11 日以太坊价格和指标。

使用 mplfinance 你可以像交易者一样实现可视化。上图中，你可以在主面板上看到一个普通的 OHLC [烛台](https://www.investopedia.com/terms/c/candlestick.asp)。在下面的面板中，我们有两件事情正在进行，直方图上的量，以及用虚线表示的指标(灰色的 MACD 和青色的 MACD _ 信号)。

你可能会感到困惑，因为通常的表示法是用两根 MACDs 的差值来创建柱状图。但是不要觉得失落！直方图是[体积](https://medium.com/coinmonks/data-science-project-cryptocurrencies-part-2-volume-and-data-source-b42ac1d6ec12)。我只是将图表中的信息量最大化(我们不需要线条上包含信息的直方图，不是吗？).

# 4.5)我们能单独相处吗？

好吧，很酷的指示器，很酷的可视化，但是如何使用它们呢？这是我们必须尝试一些策略的部分。有了我们的数据科学，知识就足够了？我们来想办法吧！下面的部分是给你一些变量的背景。我把一些箱子里的指示器分开了。我们可以检查这些值本身是否在这些范围内给了我们积极或消极结果的线索。

![](img/a920977c4c8e282634e0ecc184faf15f.png)

MACD 宾斯，分开的结果。

![](img/f0998a605095c45b8f8377c38cc14cf4.png)

MACD _ 信号仓，由结果分开。

![](img/51ca5f0f89bea1460dafca8f5afe8850.png)

MACD 信号和 MACD 的区别

在左侧的箱上出现正面结果而在右侧出现负面结果的概率似乎更高。但似乎没有什么是防弹系统。我们看到了这一点，并且我们确信我们能够有所作为。所以我们可以继续前进。

是时候训练我们的第一个模型了！假设我们认为结果只取决于我们刚刚看到的三个测量值(显然不是)，但我们仍然可以训练我们的模型，看看它如何发展。首先，我们将平衡积极和消极的结果，然后，使用我们钟爱的标准标度转换我们的变量。培训数据将一直使用到 2020 年 2 月 1 日。其余的将用于测试结果。

> 技术说明:“初始”将是 2 月之前的指标数据框架，“指标”是整个数据框架，但对于[120245:]它只是测试数据。

```
Initial = Initial.sample(frac=1)
X_train = Initial[["trend_macd", "trend_macd_signal", "trend_macd_diff"]]
y_train = np.where(Initial["Outcome"] == "+0.5%", 1, 0)
X_test = indicator[120245:][["trend_macd", "trend_macd_signal", "trend_macd_diff"]]
y_test = np.where(indicator[120245:]["Outcome"] == "+0.5%", 1, 0)
from sklearn.preprocessing import StandardScaler
from sklearn.metrics import confusion_matrix
sc = StandardScaler()
X_train = sc.fit_transform(X_train)
X_test = sc.transform(X_test)Classifier1 = RandomForestClassifier(
    max_depth=2,
    min_samples_leaf=400,
    max_features=2,
    min_samples_split=4,
    n_estimators=1000,
    random_state=28,
)
Classifier1 = Classifier1.fit(X_train, y_train)
y_pred1 = Classifier1.predict(X_test)confusion_matrix(y_test, y_pred1)
```

![](img/4a50056a2c5a30b850a0b982514f5688.png)

混淆矩阵

垂直的数字代表真实的结果，水平的数字代表预测。例如，在这种情况下，我们预测了 9505 个积极的结果，其中 53.8%的预测是正确的。正确分配坏的结果有一些小麻烦，但是我们可以暂时忘记它。

你还记得我们讨论预期收益的所有前奏吗？现在是时候使用它了！。我们可以使用 predict_proba 来获取正面结果的概率。下图显示了给定某个 P(p)的成功概率，在这种情况下，我们知道 P(p)>0.5=0.538。理论上讲，预测概率越高，我们的胜算就越大。

![](img/1a9a0881250134ea1c9c1076ff784667.png)

在每个矩形的顶部，您可以看到满足该条件的案例数量。概率越大，涉及的案例越少。但总的来说，我们找到了我们所期望的！P(p)越大，成功率越大。如果我们每次都用这个策略进去，P(p)>0.6，我们最终会得到**131**235 的正确条目！。

# 4.6)贸易-Bot 战略方法

每个指标都有不同的使用方法，这取决于和你交谈的交易者。但是，有一个话题有一些共识。协议规定，当 MACD 越过信号线上方，也就是最后一条线低于零时，就是进场信号。如果你走得更远，他们也会告诉你这只是一个进场信号，如果市场的趋势和信号的方向一致。

还有多种方法可以检验市场趋势，我们会坚持其中一种。比较 200 周期均线和收盘价。如果价格高于均线，我们认为这是一个上升趋势。对于相同的测试数据，这是一个简单的过滤器，可以过滤出满足这三个条件的情况。

为了让这变得更有挑战性，我将向您展示我们测试数据的前 4 天(对于 GIF 大小来说不会更多)。下面你可以看到两个新的东西。首先是 200 期均线虚红线。另一个新的事情是，我会标记进场点，只要它们满足所有条件(如果交易结果是负面的，用红色表示，如果是正面的，用绿色表示)。

![](img/149f479236498c69a3054ab3b346bfe8.png)

这—不太好— 4 天反映了整个期间最终发生的事情。204 次符合条件， **97 次**符合正面结果。这不是我们所期望的，但我们不必忘记这只是硬性规定。在下一节中，我们将更深入地探讨这一战略的精神。

# 4.7)将所有功能整合在一起！

好吧，我们的近似有体面的结果，而交易机器人不是那么好。但是如果我们想用真正的费用投资，那些结果是不够的。我们能改进吗？这就是我们在这里的原因！我们的工作是了解正在发生的事情，也许如果我们停下来想一想，我们的结果会更好。

规则背后的直觉是什么？我听一些交易者说，预期 MACD 信号在零线后面的原因是因为他们预期有一个更高斜率的交叉。因此，如果我们加入一个变量来反映 MACD 上升的力度，我们可能会得到一些更好的结果。

这可以通过将指标与最近时期之间的差异作为一个变量来实现。为了反映这一点，我们将使用移位方法来减去实际值与之前的 1、3 和 5 个周期。但是这些概念也适用于其他两个组件。所以我们会对每个变量都这样做。这将为我们提供 9 个额外的变量，它们的任务是反映这些指标的来源。

同样，我们可以理解趋势也很重要，但也许没有必要把它作为一个二元变量。相反，我们可以用收盘价减去 200 均线，然后除以收盘价，得到它们之间的百分比差。让我们看看会发生什么！

```
indicator['trend_macd_diff1']=indicator['trend_macd_diff']-indicator['trend_macd_diff'].shift(1)
indicator['trend_macd_diff3']=indicator['trend_macd_diff']-indicator['trend_macd_diff'].shift(3)
indicator['trend_macd_diff5']=indicator['trend_macd_diff']-indicator['trend_macd_diff'].shift(5)
indicator['trend_macd_signal1']=indicator['trend_macd_signal']-indicator['trend_macd_signal'].shift(1)
indicator['trend_macd_signal3']=indicator['trend_macd_signal']-indicator['trend_macd_signal'].shift(3)
indicator['trend_macd_signal5']=indicator['trend_macd_signal']-indicator['trend_macd_signal'].shift(5)
indicator['trend_macd1']=indicator['trend_macd']-indicator['trend_macd'].shift(1)
indicator['trend_macd3']=indicator['trend_macd']-indicator['trend_macd'].shift(3)
indicator['trend_macd5']=indicator['trend_macd']-indicator['trend_macd'].shift(5)
indicator['trend']=(indicator['200MA']-indicator['Close'])/indicator['Close']
Initial = Initial.sample(frac=1)
X_train = Initial[
    [
        "trend_macd",
        "trend_macd_signal",
        "trend_macd_diff",
        "trend_macd_diff1",
        "trend_macd_diff3",
        "trend_macd_diff5",
        "trend_macd_signal1",
        "trend_macd_signal3",
        "trend_macd_signal5",
        "trend_macd1",
        "trend_macd3",
        "trend_macd5",
        "trend",
    ]
]
y_train = np.where(Initial["Outcome"] == "+0.5%", 1, 0)
X_test = indicator[120245:][
    [
        "trend_macd",
        "trend_macd_signal",
        "trend_macd_diff",
        "trend_macd_diff1",
        "trend_macd_diff3",
        "trend_macd_diff5",
        "trend_macd_signal1",
        "trend_macd_signal3",
        "trend_macd_signal5",
        "trend_macd1",
        "trend_macd3",
        "trend_macd5",
        "trend",
    ]
]
y_test = np.where(indicator[120245:]["Outcome"] == "+0.5%", 1, 0)
from sklearn.preprocessing import StandardScalersc = StandardScaler()
X_train = sc.fit_transform(X_train)
X_test = sc.transform(X_test)
from sklearn.ensemble import RandomForestClassifierClassifier = RandomForestClassifier(
    max_depth=11,
    min_samples_leaf=400,
    max_features=11,
    min_samples_split=4,
    n_estimators=500,
    random_state=28,
)
Classifier.fit(X_train, y_train)
y_pred = Classifier.predict(X_test)from sklearn.metrics import confusion_matrixconfusion_matrix(y_test, y_pred, normalize="true")
```

![](img/8133472ffde18f872cbf899a5a6bcbac.png)

在不利结果方面，它似乎做得更好，但在积极结果方面，我们的表现稍好一些(53.98%的真阳性)。嗯……这可能会令人沮丧，但是嘿！不要在看 P(p)图之前就放弃。也许会给我们带来好消息，谁知道呢？

![](img/6d3604d378f98740b71a3ab65050a00a.png)

我就知道！！这个柱状图的出现是为了拯救世界！。不出所料，成功率越来越高！我们可以看到，与我们制作的第一个相比，这个模型有了明显的改进。为什么？因为它可以更好地分离出具有更高成功率的条目概率的情况。

假设我们选择策略:在 P(p)>0.62 时买入。我们的计划会成为一个炸弹！我们得到了 188 个信号，其中 **124 个**成功(有效 **65.95%** )。重要的是，我们利用所学的一切，设法制定出一个好的策略！。好了，我要放松了，今天太兴奋了。

我们做到了这一点，**只用了一个指标**！如果我们认为模型可以包含其他具有不同概念的基本指标，如交易量或波动性，这是有希望的。但是那个家伙，那是完全不同的故事。

# 5)后会有期！

我度过了一段美好的时光，并与你分享，我希望这对你有所帮助。如果你因为结束而高兴，给我你的最后一分钟！我喜欢仪式，用我们的主要外卖做一个结束仪式会提升我的精神:

*   我们定义了风险管理程序的一小部分，理解了预期回报的重要性。
*   我们遭受了费用对我们决策的影响。
*   我们看到了为这项任务选择类别概率估计模型的重要性。
*   我们知道如何根据业务概念改进我们的功能工程，在关键的地方获得更高的成功率。
*   你现在可以做酷酷的动作可视化了！

这就是我给你的全部内容，我知道这是基本概念，但主要目的是即使你对交易了解不多，你也能理解这一点。现实生活中的系统也有很多方法可以使这些策略更加稳健。此外，我做的很多决定都会引发争论，这是我所期待的！我很乐意讨论你的不同想法，给我一个你的想法的回复！

如果我感受到你的热情支持，我会带着新鲜的内容回来！

由于你给了我很大的支持，我想出了一个延续:

[](/optimizing-the-target-variable-an-example-for-trading-machine-learning-models-48a1587d7b9a) [## 优化…目标变量(？)交易机器学习模型的例子

### 我们的目标是静态的吗？

towardsdatascience.com](/optimizing-the-target-variable-an-example-for-trading-machine-learning-models-48a1587d7b9a) 

> 这是强制性的，你不要用这个策略投资或交易，因为这篇文章的主要目的是谈论商业理解。使用交易的想法:它只是一个工具，用来访问真实的问题和真实的数据。如果你遵循这个未经证实的系统，你很容易失去你的存款！。

如果你喜欢，就在 [Medium](https://medium.com/@maletelier) 和 [Linkedin](http://www.linkedin.com/in/maletelier) 上关注我。如果你想给我写信，我最近在[推特](https://twitter.com/maletelier)上。我很乐意与你交谈！

***来自《走向数据科学》编辑的提示:*** *虽然我们允许独立作者根据我们的* [*规则和指南*](/questions-96667b06af5) *发表文章，但我们并不认可每个作者的贡献。你不应该在没有寻求专业建议的情况下依赖一个作者的作品。详见我们的* [*读者术语*](/readers-terms-b5d780a700a4) *。*