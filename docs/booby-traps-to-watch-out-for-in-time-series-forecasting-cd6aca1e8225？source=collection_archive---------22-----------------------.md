# 使用机器学习进行时间序列预测时要小心的陷阱

> 原文：<https://towardsdatascience.com/booby-traps-to-watch-out-for-in-time-series-forecasting-cd6aca1e8225?source=collection_archive---------22----------------------->

## 代价高昂的错误会使基于机器学习的时间序列预测模型在生产中变得不准确

![](img/2a4deb02d20e2b6b74263b7ec07d1b9e.png)

图片来自 [Tralfaz](https://tralfaz.blogspot.com/2012/03/cat-concerto-expressions.html)

T ime 系列预测在数据科学方面算是老手，有着扎实的统计学基础。甚至在 Python 使得在行业中应用 ML &数据科学更容易之前，它就已经存在了。它在能源预测、金融等方面有多种应用。仅举几个例子。但是如果你没有在你的数据科学入门课程中读到过 ARIMA、霍尔特-温特斯，我不会责怪你，因为他们不像 ML 模型那样耀眼。

然而，最近，许多研究表明，使用更闪亮的最大似然模型，如[神经网络](https://pdf.sciencedirectassets.com/280203/1-s2.0-S1877050917X00215/1-s2.0-S1877050917326170/main.pdf?X-Amz-Security-Token=IQoJb3JpZ2luX2VjEDMaCXVzLWVhc3QtMSJGMEQCICSHvXHm7ST7OWAFdGSA%2F6gdxTiBNMtrgzt25i3FmfhJAiAQzl1gkWBWPBopzAmb3V7Mzs9LOkaFI89GWAuEoQJhrCq9AwjM%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F8BEAIaDDA1OTAwMzU0Njg2NSIMgYpAF9ZZsyD02HK%2FKpEDyPEfBVdfMOesj61lUeaYJnttCa4CX8Jtbqv10voLAxdic46FPwJcRWCfWFyCtTDeTi7oFSJvHqY4d7bYds0sO9xPPn49Zbt%2FGbqw03zSIiPgVkuCRhBb46mMjflJBpKJkdrFclqcP5UVHcyFnC4H3jRQOU5tcyNjLpBnY3CCTKPLmOluUYoLWQwNEqfPvr3kAnUir5sspIkWaXFHkFCKTGRqjddf5FHz%2BZIYjtAlIXEgAL5ENxlo40%2BpnGm27SZEdJnM4qh5aQ2KJn%2Fmg7aekvlKNPSibo5dSCWKG4Sm8Nb%2FVgiAjfaFsenXsW8PSBEl%2F2x2u%2F9oeDqW5Zk2I%2Fp3D2hkIq1diBFNhQCpsX2%2F7VdD8vRKTRL%2Fm1v4dSFDAqUVBV%2Fs4k6M18%2B2R6d6eEmfgfTqpe9VyFjq81QTIChd7QRUX36gvZhKATQuifMJwEB%2Fe8hjtnzNiZH%2F%2F9FcG36mZ%2B4YhGacrVsgZiNHtsG1yLvZloOlw15kdYU0CemclqY2iN8uhX9jl5EToEqMw2ungPowr%2BrD8QU67AGMjElsLpT%2FEh3SIJetO71oGY3TE4BnUU%2BpGC9f%2Fn03IcKImEl%2BJ7zGVcKCYsAnVdmZumJGBIXxJmPeAKc%2B5l5vbh5Kz9ItBeEvCADA6jxTW9%2FV87HYSadx7YQxRU3csY8hTjGOermOC1k6TEl0e0CoJWbEVsYyVHDn7P9HuUn4UncB0GGpAmiIy910dCkX0WRxVB5fB1S9F6AASJb0G%2BtFUDxFJwu2Jjcri2gI7S6fhLrxh7VrHCLleVaz6bvmyfQXrS%2F35rXEEBy8Yqz9ie3%2FZ3xcsUIGH4a2zVls4SfLJStVZTWRDcUZyKn9mA%3D%3D&X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Date=20200129T035144Z&X-Amz-SignedHeaders=host&X-Amz-Expires=300&X-Amz-Credential=ASIAQ3PHCVTYV3XJQBM5%2F20200129%2Fus-east-1%2Fs3%2Faws4_request&X-Amz-Signature=95ff32e12e90f17b20800c3375ca9b34eb77fdd99269ba13e450042156b6a95d&hash=8a6301d8c61b71bfefb7d77d4e883a6ddd741621a41da3a49158371609aa461f&host=68042c943591013ac2b2430a89b270f6af2c76d8dfd086a07176afe7c76c2c61&pii=S1877050917326170&tid=spdf-a3288f06-ab32-4b7d-93f0-dcc04faae201&sid=1ad2079523ece048f17b1028e60086648241gxrqa&type=client)进行预测是有希望的。我的团队也决定冒险进入这个领域，将 ML 技术应用于预测问题。我有更传统的 ML 方法和应用的背景，例如预测模型、分类和 NLP，直到最近才有机会处理时间序列数据。这种传统主义意味着我必须改变处理问题的方式。更重要的是，我陷入了一些被传统的 ML 方法/途径完全掩盖的陷阱。

让我们来讨论一下这些陷阱。

1.  **数据科学 101:将数据集分为训练/验证/测试**

构建机器学习模型的传统方法需要获取数据，清理数据，并将其分为训练、验证和测试。虽然这种方法在时间序列预测中可能仍然有效，但有必要寻找潜在的地雷。

首先，让我们看看用于预测功率(KW)的典型特征；一个时间序列问题。任何时间序列预测问题中最高度相关的特征之一是先前值，即从当前时间 *T* 的滞后值。在功率预测的情况下，已经看到前 6 个值和上周的相应值与当前时间的负荷值高度相关。下图对此进行了说明:

![](img/d2dd5d026b53d2c4d7b26c401177e5fd.png)

示例滞后值(负载-千瓦)

你可以在上图中看到每个滞后值是如何形成的；跨特征集形成的对角线是时间序列预测的一个非常明显的属性。每个滞后值都是在规定的滞后时间*的目标值，如 t-1、t-2* 等。现在让我们想象我们在 *15:00 分割这个特征集。****16:00–18:00***将成为我们的测试/验证集(在我们的示例中无关紧要)。现在，如果我们想要预测这 3 个小时标记( *16:00、17:00、18:00* )中每个小时标记的电力负荷，我们将需要 ***16:00*** 的负荷值来预测 *17:00* (因为 ***滞后(t-1)*** 是**负荷[*16:00】【T24)***

> 我们的模型不应该知道真实世界场景中的实际值

![](img/15f4eabb91fdc952c37270348e263e5c.png)

但是，等一下，当我们不知道 16:00 的实际值时，我们怎么能在 17:00 和 18:00 得到这些滞后值呢？(请记住，我们预测的是 16:00 时的值)。换句话说，我们的模型公然作弊。你也几乎可以把这等同于用训练数据测试你的模型。如上图所示，这些值在生产中对我们不可用。因此，简单地分割数据集并验证/测试我们的模型是不可能的。解决这个问题的一种方法是每次动态构造测试行，将预测值用作下一行的滞后值，依此类推。这个问题也直接把我们引到了下一点。

2) **错误分析:随时间恶化**

回到我们之前的例子，我们知道我们不能使用实际值，所以我们将不得不用我们的预测值来凑合。正如任何回归/ML 问题一样，我们知道我们永远不可能获得 100%的准确性。这意味着我们的每一个预测都将带有一个误差，这个误差将会在接下来的所有测试行中传播。当我们在预测值上绘制实际值时，这种影响将是可见的，即我们的误差将随着预测时间的推移而增加。一个自然的结果将是尝试并过度拟合模型上的数据以提高性能，但我们应该避免这样做，并意识到这个错误是由于我们拥有的特征集造成的。我们有办法避免这种情况(只预测短期或稍微复杂的动态来预测下一个 h 步骤)，但我将把这个讨论留到以后。但这为下一个陷阱埋下了伏笔。

**3)列车模型一次**

![](img/6457aa17071eb922f74cc8a373200879.png)

图片来自[来源](https://mlinproduction.com/model-retraining/)

在大多数机器学习问题中，我们识别相关特征，训练一次模型，然后将其部署到生产中。例如，我们想要开发一个 ML 模型来对手写数字进行分类，这是一个相当众所周知的问题。我们根据大量不同的数据来训练我们的模型。我们假设用于分类这些数字的特征不会随着时间而改变。因此，一旦我们在训练集上训练了模型，我们就简单地对我们的测试值进行分类，而不做任何改变。在时间序列中，情况并非如此。

![](img/dd3d083f3b335e878429ab436e55c701.png)

图片来自[来源](https://www.e-education.psu.edu/ebf301/node/457)

让我们再次回到电力负载的例子。我们不需要成为时间序列预测专家就知道，在炎热的夏季(更多空调)和寒冷的冬季(更多供暖)，电力需求会增加。时间序列数据的这种波动是常见的。你不能指望数据一直保持不变。因此，每次我们想要预测数值时，都需要重新训练模型。这是为了纳入任何可能发生的重大波动(高能源价格、经济衰退等)。请看上图，了解历史上的能源消耗以及未来二十年的预测。快速浏览一下历史数据，就可以看出消耗是如何随时间变化的。同样，在没有其他随时可能变化的外部因素的情况下，我们也不能指望需求只是简单地上升。因此，牢记这一点并用最新的时间序列数据重新训练模型是非常重要的。

# 摘要

在处理时间序列数据和传统的机器学习模型时，必须记住这些要点。对于时间序列预测专家来说，这并没有让他们大开眼界，但是对于像我这样更传统的数据科学家来说，这是一个很好的教训。在处理时间序列数据时，我们应该意识到模型中的这些不确定性。