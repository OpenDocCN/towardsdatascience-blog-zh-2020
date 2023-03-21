# 自动时间序列分析。R 中的 Arima

> 原文：<https://towardsdatascience.com/time-series-analysis-with-auto-arima-in-r-2b220b20e8ab?source=collection_archive---------6----------------------->

## 使用自动 Arima 的 ARIMA 误差回归

![](img/63ea593ea35a0d86383aff9857bba94e.png)

随着最近一波数据分析和数据科学工具的出现，预测、预测、聚类和建模变得太容易了。使用复杂的方法强制迭代不同的模型来挑选最佳的模型，这是现在常见的做法，而且非常容易做到。就拿像 *xgboost* 或 *randomforest* 这样的算法来说吧，这些算法迭代数百种可能性，在几分钟内挑选出最佳结果。它们总是停留在一个点上，我们有时想当然地认为这是算法能做到的最好的结果，并且它已经为我们的预测找到了最好的解决方案。但是，为了让所有这些自动算法发挥最大能力，在将数据输入算法之前，需要考虑几个步骤。这篇文章将探讨如何利用 R 中的[预测](https://cran.r-project.org/web/packages/forecast/forecast.pdf)包中的 ARIMA 自动选择算法，并将提供一个操作时间序列数据并为建模和预测做好准备的建议性指南。

# **ARIMA 组件的简要说明:**

ARIMA 是自动回归(AR)综合(I)移动平均(MA)的缩写，表示 ARIMA 模型有三个组成部分。我将在这篇文章中非常简要地介绍这些组件，如果你需要更深入地理解这些概念，我建议你使用[这些](https://otexts.com/fpp2/index.html) [资源](https://robjhyndman.com/)，它们可以从[免费获得](http://r-statistics.co/Time-Series-Analysis-With-R.html)。

ARIMA 模型可以用两种形式表示:非季节性模型，其中模型以(p，d，q)的形式显示订单，其中:

p =自回归模型的阶数

d =差分的阶数

q =移动平均线的阶数

**自回归模型(AR):**

自回归模型类似于回归模型，但在这种情况下，回归变量是具有特定滞后的同一个因变量。

**差异(一):**

为了让 ARIMA 发挥最佳性能，它需要数据保持稳定。这意味着平均值和方差在整个集合中是恒定的。差分用于转换数据，使其保持不变。

**移动平均线(MA):**

移动平均线广泛应用于时间序列分析，是一个已经广为人知的概念。它包括获取特定滞后的一系列点的平均值，可以表示为:

![](img/510cba32d6fd08db0ab92e581618ff0d.png)

*来源:*[*【https://otexts.com/fpp2/moving-averages.html】*](https://otexts.com/fpp2/moving-averages.html)

其中 m = 2k +1，

k =所用点前后平均值中使用的值的数量，以及

t =计算趋势周期的时间。

在 ARIMA，使用移动平均模型

**季节性 ARIMA 款(SARIMA):**

这些模型考虑到了数据的季节性，并在季节性模式上执行相同的 ARIMA 步骤。因此，如果数据每个季度都有一个季节性模式，那么 SARIMA 将得到所有点的(P，D，Q)顺序和每个季度的(P，D，Q)顺序。

**带有 ARIMA 误差的回归模型:**

现在真正的交易开始了。ARIMA 模型非常适合利用历史数据盲目预测未来。但是，有时候我们不需要盲目预测，有时候我们有变量可以帮助我们预测未来的行为。回归模型在这种情况下是理想的。

那就让我们客观地看待这件事吧。假设您有带有季节模式的时间序列数据，该数据具有可用作回归变量的属性。这里有一个例子:以 FRED 记录的从 1999 年第四季度到 2019 年 Q1 期间的电子商务总销售额和美国零售总额的季度数据为例，我们看到电子商务数据比美国零售总额先被汇总和报告，因此让我们使用电子商务数据作为回归变量，美国零售总额作为因变量来预测 2019 年 Q2 的美国零售销售额。

现在让我们比较两个数据集。通过计算每个季度的增长百分比，我们可以将数据标准化并比较它们的行为。这看起来是这样的:

![](img/a26d17e15eed97e2c3a4656244fe86c5.png)

*资料来源:数据摘自 https://fred.stlouisfed.org/tags/series?美联储经济数据网站 1999 年第四季度至 2019 年 Q2 (* [)同期数据 t =季度% 3br 交易+交易](https://fred.stlouisfed.org/tags/series?t=quarterly%3Bretail+trade))。

如你所见，两组数据之间有明显的滞后，但这种滞后似乎是一致的。这是使用带有 ARIMA 误差的回归模型的完美场景，因为它包含了这种方法的两个优点:包含趋势的大量季节性数据和提供与因变量一致相关性的回归变量。在这种情况下，R 中预测包的 [auto.arima](https://www.rdocumentation.org/packages/forecast/versions/8.10/topics/auto.arima) 允许我们相对容易地实现这种类型的模型。这是我们的指南真正开始的地方。

首先，我们让 auto.arima 完成它的工作，为我们的数据选择最佳模型。然后，我们使用默认参数拟合 auto.arima 模型。该模型旨在使用零售变量作为回归变量来预测 2018 年第二季度和 2019 年第二季度之间美国的总销售额。

```
#Fitting an auto.arima model in R using the Forecast packagefit_basic1<- auto.arima(trainUS,xreg=trainREG_TS)
forecast_1<-forecast(fit_basic1,xreg = testREG_TS)
```

![](img/27a44a7d134fb276737e8cb11f41dd95.png)

使用零售销售数据作为回归变量来预测美国总销售额，拟合 ARIMA 误差的回归模型的结果。

![](img/1a94d4dbfcf6f8802f4429057d7041db.png)

现在我们来看看模型的残差。残差将告诉我们模型是否能够捕捉数据提供的所有信息。

![](img/42a0be867f0fa0298ce8cd47d36b6de8.png)

一个故事的三个图表:1)残差的时间序列图，2)前 20 个滞后的 ACF 图，3)残差的分布。

似乎模型在残差中留下了信息。第一个也是最后一个图表向我们展示了残差看起来并不是[白噪声](https://otexts.com/fpp2/wn.html)，而是在它们之间有一些相关性，第二个图表证实了这一点。前 12 个滞后超过由[自相关函数](https://otexts.com/fpp2/autocorrelation.html)建立的阈值。让我们通过以下 4 个步骤调整我们的模型来解决这个问题:

**1。STL 分解:**

Auto.arima 能够决定用于训练模型的数据是否需要季节差异。然而，有时数据可能无法清楚地表达其行为，auto.arima 需要在正确的方向上推动才能理解它。这就是 STL 分解派上用场的地方。STL 作为一个附加过程工作，其中数据被分解成趋势、季节性和余数，并且数据的每个组成部分可以被分开进行分析。

通过将数据分解成这三个部分，我们能够通过测量每个部分的强度来衡量它在整个数据中的权重。因此，例如，我们可以测量数据的季节性强度，并使用以下公式获得分数:

![](img/e8fc7db029c3822caeee1f6748707d37.png)

来源:[https://otexts.com/fpp2/seasonal-strength.html](https://otexts.com/fpp2/seasonal-strength.html)

高于 0.64 的季节性强度一直被视为强劲，是我用来决定数据是否具有足够季节性强度的阈值。更多关于这个的可以在[这里](https://otexts.com/fpp2/seasonal-strength.html)[找到](https://robjhyndman.com/hyndsight/tscharacteristics/)。

使用这个阈值，我们可以告诉 auto.arima 何时开始研究季节性模型。下面是一个使用我们零售数据的例子:

![](img/140339995be09ea73fc51ba27e20c94e.png)

*使用 STL 分解，我们可以测量数据的季节性和趋势部分的强度。*

正如您在所提供的表格中看到的，数据具有很高的季节性。因此，如果我们知道 ARIMA 必须是季节性的，那么让我们告诉 auto.arima 从哪里开始。

auto.arima 函数具有 arima 函数每一阶的参数，这些参数由它们在(P，D，q) (P，D，Q)表示中的值来表示。因此，让我们用季节模式的一阶差分来强制 auto.arima 迭代 arima 模型。我们可以通过将参数 D=1 指定为 auto.arima 函数的参数之一来实现这一点。

> 注意:在大多数情况下，auto.arima 能够做到这一点，而无需任何规范，因为它使用 u-test 来决定是否使用 SARIMA 模型，但在某些情况下，尤其是在可用数据有限且季节性不明显的情况下，该函数可能需要我们的帮助。这在使用 auto.arima 对具有不同特征的多个数据集进行预测时非常有用，通过测量季节性强度，我们可以指定 auto.arima 函数从何处开始查找。

**2。近似值:**

下一个参数是近似参数。这个非常简单，但是有很大的不同。auto.arima 挑选最佳模型的方法是拟合几个模型并计算其 [AICc 得分](https://otexts.com/fpp2/selecting-predictors.html)。得分最低的模型获胜。但是，为了使函数能够更快地找到解决方案，算法会跳过一些步骤并逼近结果，以便拟合更少的模型。这在包含大量数据的数据集上很有用，但会牺牲性能以提高速度。将此参数更改为 FALSE 会使 auto.arima 更加努力地寻找正确的解决方案。

**3。λ:**

更好地理解数据和处理异常值或大范围数据的一种方法是转换数据。对于时间序列，go to 变换技术是 Box Cox 变换。Box Cox 转换有助于确定基于 lambda 转换数据的最佳方式。

这里的 Lambda 用于表示为数据选择最佳转换的数字。数据的最佳变换是使数据最接近正态分布的变换。

![](img/914b203d4b636506263d8d2d13b47a6b.png)

[*公式*](https://www.statisticshowto.datasciencecentral.com/box-cox-transformation/) *为一盒考克斯变换。如果值不为 0，则使用幂变换，如果值为 0，则使用对数变换。*

在 R 中，这个简单的步骤可以帮助您为数据选择理想的λ:

```
Lambda<- BoxCox.lambda(trainUS)
```

然后我们的 auto.arima 函数让我们指定 lambda 的值作为它的一个参数。

```
auto.arima(trainUS, xreg = trainREG_TS,D=1, approximation = FALSE, **lambda = Lambda**)
```

**4。均值和漂移:**

这两种其他方法允许将常数添加到模型中，并允许考虑更复杂的模型。下面简单介绍一下这两者。

[漂移](https://otexts.com/fpp2/simple-methods.html):仅当差值大于 0 时可用，并允许拟合具有变化平均值的模型。

[平均值](https://otexts.com/fpp2/simple-methods.html):允许考虑具有非零平均值的模型。

默认情况下，R 将它们设置为 FALSE，再次选择速度而不是性能。将这些参数设置为 TRUE 允许模型更努力地工作，但是要小心过度拟合。

# 决赛成绩

好了，现在我们知道了:

1.  最初的 auto.arima 模型在残差中留下了大量信息。
2.  auto.arima 可以通过调整几个参数来更好地工作。
3.  并且可以转换数据以避免数据中的趋势影响最终结果。

让我们试着用所有学到的信息重新调整我们的数据，看看我们会得到什么:

```
#Fitting the model with the new parametersfit_complex1<- auto.arima(trainUS,xreg=trainREG_TS,D=1,approximation = F,allowdrift = T,allowmean = T,lambda = Lambda)forecast_complex1<-forecast(fit_complex1,xreg = testREG_TS)checkresiduals(fit_complex1)
```

![](img/2cb4337e2ac6655079be9529ad8e1432.png)

现在很明显，这个模型能够从数据中获取更多的信息，从而产生更好的结果。

```
accuracy(f = forecast_complex1,x = testUS)
```

![](img/35a6362f47e3952c06e235561fc7a634.png)

使用零售销售数据作为回归变量来预测美国总销售额和新参数，拟合 ARIMA 误差的回归模型的结果。

![](img/4a71a13caf3e080d512fce22e6530248.png)

# 要记住的事情:

有几件事需要记住。

*   时间序列数据具有连续性和相关性，任何缺失值都会严重影响您的模型。
*   SARIMA 模型在处理高度季节性的数据时非常有效，但是对于线性或高度非结构化的时间序列数据，有更好的方法。在这些情况下，像 FB Prophet、树模型或神经网络这样的方法可以比 SARIMA 执行得更好。
*   你的模型的输出和你的输入一样好。只有当变量之间存在明显的相关性时，在 ARIMA 模型中加入回归变量才有意义。
*   ARIMA 模型中的残差反映了模型的性能，在对其进行评估时应予以考虑。函数 [**检查残差**](https://www.rdocumentation.org/packages/forecast/versions/8.10/topics/checkresiduals) 、 [**ACF**](https://www.rdocumentation.org/packages/forecast/versions/8.10/topics/Acf) 和 **PACF** 便于跟踪您的模型在残差中留下的信息。

**本文使用的代码可在此处获得[。](https://github.com/luislo92/Time_Series_Analysis_Medium)