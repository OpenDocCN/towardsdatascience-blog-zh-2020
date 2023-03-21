# 用 PyMC3 了解飞机事故趋势

> 原文：<https://towardsdatascience.com/understanding-aircraft-accidents-trends-with-pymc3-b1ca0e4c5d33?source=collection_archive---------36----------------------->

![](img/8ecb20cce21236a073c147db1c2679a2.png)

图片由 [Free-Photos](https://pixabay.com/photos/?utm_source=link-attribution&utm_medium=referral&utm_campaign=image&utm_content=828826) 来自 [Pixabay](https://pixabay.com/?utm_source=link-attribution&utm_medium=referral&utm_campaign=image&utm_content=828826)

使用 PyMC3 直观地探索历史上的航空事故，应用频率主义者的解释并验证变化的趋势。

# 前言

今年 8 月 7 日，一架从迪拜(阿联酋)飞往 Kozhikode(印度喀拉拉邦)执行遣返任务的印度航空快运飞机在暴雨中滑出跑道，坠入山谷[1]。

随后 35 英尺的坠落将飞机摔成两半。该航班共搭载了 180 名乘客，其中 18 人在事故中丧生。其余 172 人不同程度受伤，接受治疗[2]。

对这一可怕事故的官方调查自然将是一项事实调查任务，并将试图弄清楚哪里出了问题，谁该负责。

# 动机

在这个故事之后，我开始谷歌最近的飞机事故，以了解背景，并从全球角度看待这些事件。

这次搜索让我找到了许多网页，上面有飞机坠毁的[照片和视频](https://www.baaa-acro.com/)，坠毁统计表，事故调查报告，以及不同航空业专家在此类灾难性事故后的原话。

这次调查的底线是，我们正处于一个日益安全的飞行环境中。[监管、设计、机械和电子安全](https://www.agcs.allianz.com/news-and-insights/expert-risk-articles/how-aviation-safety-has-improved.html)措施比以往任何时候都更加严格，从而使飞行成为一种相对更安全的交通方式。

但是我想亲自用这些数字来验证这个结论。

这个练习的动机问题是—

> 与过去相比，最近一段时间飞行变得相对安全了吗？

# 数据源

我在维基百科和 T2 国家运输安全委员会上查看了公开的空难数据，并创建了一个数据集来满足这次演习的需要。

> 整个练习和数据集可以在我的 [GitHub](https://github.com/ani-rudra-chan/Aircraft-Accidents-with-PyMC3.git) 资源库中找到。

*现在切换到第一人称复数……*

# 工作工具

为了回答这个激励性的问题，我们把这个任务分成两部分—

1.  Python 中的探索性数据分析(EDA)。
2.  Python 中的概率编程(PyMC3)。

# 探索性数据分析

在这一部分中，我们回顾了过去的飞机坠毁事件，这些事件构成了我们进行分析的时间序列。一些需要记住的事情-

1.  《国际民用航空公约》将飞机事故与飞机事故区分开来。区别主要在于是否发生了死亡事故。
2.  在这次练习中，我们的重点仅限于**事故**的发生，而不是其原因。
3.  我们研究了从 1975 年到 2019 年的商业飞机事故。

## 事故和死亡趋势

![](img/9714d72350abc3d4e5f353adccfb4645.png)

图 1-从 1975 年到 2019 年每年的事故和死亡人数。

从历史时间序列来看，我们可以直观地感受到自 1978 年以来每年事故数量的下降。1987 年至 1989 年期间，事故数量似乎略有上升，此后数量稳步下降。2017 年是事故发生率最低的一年，也是航空史上最安全的一年。2017 年后，这些数字似乎略有增加。

另一个明显的趋势是死亡人数随着时间的推移而下降。20 世纪 70 年代和 80 年代是飞行的危险时期，平均每年有 2200 起飞机事故导致死亡。但是随着时间的推移，我们看到这个数字已经急剧减少。

当这种下降趋势被放在航空旅客数量上升的背景下看待时(图 1 中绿色阴影区域)，我们对航空安全有了更好的了解。

## 每百万乘客的死亡人数

![](img/91eb60800e83be43ee0b269cdc5505df.png)

图 2——每年百万乘客出行中的死亡人数

当从航空旅客人数上升的角度来看死亡人数的下降时，我们会看到一个明显的下降趋势。每年乘飞机旅行的每百万乘客中的死亡人数已经从百万分之五急剧下降到百万分之一以下。

*(免责声明:贝叶斯人，准备好那撮盐)*

## 每起事故死亡人数

![](img/f3683b518b652329fd0d1707768dc8ee.png)

图 3-每起飞机事故死亡人数的变化

飞机安全的另一个衡量标准是每起事故的死亡人数。虽然可能有许多外部因素(外部因素)影响特定事故中的死亡人数——天气、碰撞性质、一天中的时间等。——我们仍然将这一指标视为对飞机安全性的粗略评估。

1995 年以后，趋势似乎略有下降，但从图表中并不能立即观察到。我们还看到，1985 年、1996 年、2014 年和 2018 年是涉及重大撞车事故的致命年份，因为每次撞车事故的平均死亡人数很大。

## 变动率

![](img/c7e88e86d3b789134a29a8dfc30eea0c.png)

图 4 —事故数量的年度百分比变化

在我们开始动机问题的概率测试之前，最后一个证据是事故的年变化率。

如果我们真的生活在安全的时代，那么我们期望图表显示一系列连续增加的绿色条。这样的窗口只在 1979-80 年、1980-84 年、1999-00 年、2006-07 年和 2013-14 年观察到。从 1980 年至 1984 年和 1996 年至 2000 年，可以看到相对安全旅行的延长期。

如果我们看一下 1995 年以后的变化率，我们会发现事故同比有很大程度的下降(红色柱很少，绿色柱更多)。

似乎一些外部因素(如飞机设计的变化、民航规章、更好的空中交通管制技术等。)可能导致了 1995 年以后的这种下降。

# 概率规划

从我们的数据研究中，我们发现每十年的飞机事故数量持续下降，我们用一些统计方法验证了这一趋势。

我们还看到，1995 年可能是航空业的一个转折点。*我们如何验证这个假设？*

在有限的数据和事件的不可重复性(*让我们假设我们无法模拟这些事故一百万次*)的情况下，这样做的一个有趣的技术是使用概率技术，如马尔可夫链蒙特卡罗(MCMC)。

实现这些技术的方法之一是借助 Python 中的 PyMC3 库。

## 快速入门

PyMC3 是 Python 中的一个库，帮助我们进行概率编程。这并不意味着编程是概率性的(*它仍然是一个非常确定的过程！相反，我们采用概率分布和贝叶斯方法。*

这种技巧是建立在一种[](https://www.analyticsvidhya.com/blog/2016/06/bayesian-statistics-beginners-simple-english/)****的世界观之上的。我们从一个关于某个过程或参数的信念(*称为先验概率*)开始，并在几千次运行(*又称随机抽样*)后更新这个信念(*称为后验概率*)。这种方法与频率主义者看待事物的方式相反(就像我们在 EDA 中所做的*)。*****

****这个过程的第二个基础是 [**马尔可夫链蒙特卡罗**](https://en.wikipedia.org/wiki/Markov_chain_Monte_Carlo) (MCMC)的随机抽样方法。这是一套算法，允许我们从先验概率分布中取样，并生成数据来测试我们的先验信念并更新它们。****

> ****PyMC3 [网页上提供的文档](https://docs.pymc.io/notebooks/getting_started.html)和[Susan Li](/hands-on-bayesian-statistics-with-python-pymc3-arviz-499db9a59501)的这个实践方法对于高层次理解库和技术是极好的。坎姆·戴维森-皮隆写的《黑客的贝叶斯方法》这本书真的很有帮助，如果你想动手的话。****

## ****好吧，让我们测试一下****

****我们从建立我们对事故的先验信念开始—****

*******飞机事故遵循怎样的分布？*******

****这里我们假设事故遵循泊松分布。****

```
**P(x|lambda) = (lambda^x)*(exp^-lambda)/(lambda!)x: number of accidents
lambda: rate of occurrence of the accident**
```

*******发生率会是多少？*******

****给定我们的初始假设，我们进一步假设这个发生率大约是整个数据集的平均发生率的倒数。****

****换句话说，****

```
**lambda = 1/(mean of number of accidents from 1975 to 2019)**
```

*******最初的转折点会是什么？*******

****转折点是前一年发病率高，后一年发病率低。我们最初假设，从 1975 年到 2019 年的每一年都有相等的概率(*取自离散的均匀分布*)被认为是一个转折点。****

****有了这些先验的信念，我们实例化了这个模型—****

```
**import pymc3 as pm
import arviz as azyears = np.arange(1975, 2020)
with pm.Model() as accident_model:

    alpha = 1/df.loc[df.year>=1975].num_crashes.mean()
    # Setting the prior belief for the inflection point
    change_point = pm.DiscreteUniform(
                    'change_point', lower=1975, upper=2020)

    # Setting prior belief for the rate of occurrence, ie, lambda, before and after inflection
    rate_before = pm.Exponential('before_change', alpha)
    rate_after = pm.Exponential('after_change', alpha)

    # Allocate appropriate Poisson rates to years before and after current
    rate = pm.math.switch(change_point >= years, rate_before, rate_after)accidents = pm.Poisson("accidents", rate, observed=df.loc[df.year>=1975].num_crashes)**
```

****我们用不掉头采样器(螺母)对这些分布进行了至少 10，000 次采样—****

```
**with accident_model:
    trace = pm.sample(10000, return_inferencedata=False)**
```

## ****测试结果****

****![](img/44c513db87284bde959eff8e79873f19.png)****

****图 5-更新我们对可能的变化点和发生率的先前信念的结果。****

****我们看到，在采样 10，000 次后，我们最初认为所有年份都有同等机会被视为转折点的想法得到了更新。结果表明，1997 年(而不是 1995 年)最有可能成为航空事故史上的转折点。****

****并且已经更新了最初的假设，即发生率是 45 年平均值的倒数。1997 年被认为是一个转折点，因为事故发生率从大约每年 **300 起变成了每年 165 起！******

****那么这些预测有多大把握呢？****

****![](img/b6cc18d9dbc9956310fe087b99db2ca0.png)****

****图 6 —预测值的不确定性****

****概率编程的 USP 是，预测是用一撮盐做出来的！与频率主义者的预测不同，贝叶斯方法的预测带有不确定性(*更现实*)。****

****我们的模型显示，94%的高密度区间(HDI)在 1996 年和 1999 年之间，1997 年是平均值。换句话说，1997 年成为转折点的概率较大。****

****在这一转折点之前，类似的 94%人类发展指数发生率为每年 295 至 312 起事故；1997 年以后的事故每年在 158 至 172 起之间。****

## ******最近的过去******

****由于我们的激励问题仅限于“最近的时间”，我们将该模型应用于 2000 年至 2019 年的数据(*假设最近 20 年足够近*)。****

****![](img/f08bc841a2c8fb0ea7e86d0852b39608.png)****

****图 7-2000 年至 2019 年间事故的模型结果****

****我们观察到，2012 年很可能是一个转折点(94%的人类发展指数是 2010 年至 2013 年)，2012 年之前的事故率接近每年 180 起，2012 年之后大约每年 120 起。****

****![](img/7567221cbc8b955ebf42c449bbb6ca00.png)****

****图 8-2000 年至 2019 年间事故模型结果的不确定性****

# ****裁决****

****所以通过进行这个小练习，我能够满足我的好奇心并回答这个激励性的问题—****

> ****如果每年的低航空事故率是航空安全的唯一指标，那么在 1997 年后，事故率大幅下降，在过去 20 年中，2012 年后数字进一步下降。****

****尽管每年的事故数量很少，但与 20 年前相比，现在飞行相对更安全。****

# ****参考****

****[1][https://indianex press . com/article/India/air-India-express-kozhikode-Cali cut-crash-live-updates-6544679/](https://indianexpress.com/article/india/air-india-express-kozhikode-calicut-crash-live-updates-6544679/)****

****[2][https://indianex press . com/article/India/kozhikode-plane-crash-air-India-express-flight-touched-down-beyond-safe-zone-death-toll-rises-to-186546584/](https://indianexpress.com/article/india/kozhikode-plane-crash-air-india-express-flight-touched-down-beyond-safe-zone-death-toll-rises-to-186546584/)****

*****再见！*****