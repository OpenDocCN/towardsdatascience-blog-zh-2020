# 利用条件期望进行缺失值的实用估计

> 原文：<https://towardsdatascience.com/ds-in-the-real-world-5f77800aff78?source=collection_archive---------23----------------------->

## 基于多元正态条件期望的二阶估计

![](img/9f0be47b2423c2e0905ee6b33f601ef4.png)

迈克·肯尼利在 [Unsplash](https://unsplash.com/?utm_source=medium&utm_medium=referral) 上的照片

处理缺失值是数据科学中一个棘手的问题。有多种方法，下面列出了一些:

*   丢弃包含缺失要素的记录
*   用样本平均值或中位数替代缺失值
*   使用机器学习技术推断期望值，如变分自动编码器[1]

尽管我们可以选择丢弃包含缺失要素的记录，但考虑到这些记录中的非缺失要素可以提供有价值的信息，这通常是不可取的。

考虑到分布的唯一一阶矩，均值替代可以被认为是一种一阶近似方法。

另一方面，基于机器学习的方法通常计算量很大，并且缺乏直观的解释能力。这是一个研究前沿，积极的研究正在进行中。

在这篇文章中，我提出了一个简单的方法，除了利用第一阶矩之外，还利用了第二阶矩，并表明它为我们提供了一个更好的估计量。

# 1 多元正态条件期望法

假设 *k* 维随机向量 X 遵循多元正态分布，

X ~ N(μ，σ)

其中μ是平均向量，σ是 *k*k* 协方差矩阵。如果 X 被分割成两个子向量 X1 和 X2，分别具有大小 *q*1* 和 *(k-q)** 1，则以 X2=x2 为条件的 X1 遵循多元正态分布，具有以下均值和协方差矩阵:

μ(x1|x₂)=μ₁+σ₁₂σ₂₂^(-1)(x₂—μ₂)……………………………………(1)

σ(x1|x₂)=σ₁₁—σ₁₂σ₂₂^(-1)σ₂₁(2)

其中μ₁和μ₂是 X1 和 X2 的先验均值向量，σ₁₁是σ的子矩阵，大小为 *q*q* ，σ₂₂是σ的子矩阵，大小为 *(k-q)*(k-q)* ，σ₁₂是σ的子矩阵，大小为 *q*(k-q)* 。

假设对于特定记录，X1 是缺失值特征的向量，而 X2 是有效值特征的向量。使用上面的μ(X1|x₂方程)我们可以估计 X1 的值，其中μ和σ用样本的均值和协方差矩阵代替，不包括缺失值记录。这可以被认为是一个二阶近似，因为它使用了一阶和二阶矩。可以一次估计多个缺失特征。

## 1.1 分类特征的估计

上面描述的条件期望方法并不直接应用于分类特征，但是通过一键矢量转换，它同样可以使用。步骤是:

1.  将分类特征转换为指示特征的一键向量。例如， *Dayofweek* 转换成 7 个指标特征 *Dayofweek_0，Dayofweek_1，…，Dayofweek_6，*其中*day ofweek _ 0*=**I**【day ofweek 是星期一】等。
2.  将指标特征视为连续的，并使用条件期望方法对分类特征缺失的记录进行估计。
3.  将指标估计值的 *argmax* 作为类别的预测指标。

## 1.2 约束特征的估计

**1.2.1 积极特征**

假设一个特征只取正值，但是等式(1)给出的估计并不能保证肯定。然而，使用适当的变量转换，我们可以加强这种约束。假设 *x* 是一个正值特征。让我们对数变换 *x* 并用数据集*中变换后的特征 *u= log(x)* 替换它。*由于 *u* 可以取正值也可以取负值，因此我们可以估算 *u* 而不是 *x* 。最后我们可以通过 *exp(u)* 回退 *x* ，始终为正。

**1.2.2 有界特性**

假设特征 *x* 被限制在范围(a，b)内。我们可以引入一个无界变量 *u* 使得

*x = a + (b-a)/( 1 + exp(-u) )*

其中 1/(1 + exp(-u))是值在(0，1)内的 Sigmoid 函数。 *u* 用 *x* 表示为 *u = -log( (b-a)/(x-a) -1)* 。然后我们可以在一个数据集中用 *u* 替换 *x* ，估计缺失的*u*s，最后计算缺失的*x*s。

**1.2.3 交叉特征约束**

假设一个特征 X₁总是大于另一个特征 X₂，X₁ > X₂.为了对估计值施加这样的约束，我们可以使用变换后的特征 X₁' = log(X₁-X₂来替换数据集中的 X₁。估计完 X₁'之后，我们就可以退出 X₁了，

X₁ = X₂ + exp(X₁')

这保证了 X₁ > X₂.

# 2 条件期望法的评价

为了评估条件期望方法在实践中的表现如何，使用了 UCI 机器学习库[2]上的*电器能量预测*数据集。这是一个实验数据集，旨在对低能耗建筑中的电器能耗进行回归分析。它有 19735 条记录和 29 个特征，包括两个随机变量 *rv1* 和 *rv2* 。功能描述粘贴在下面:

```
date time year-month-day hour:minute:second
Appliances, energy use in Wh
lights, energy use of light fixtures in the house in Wh
T1, Temperature in kitchen area, in Celsius
RH_1, Humidity in kitchen area, in %
T2, Temperature in living room area, in Celsius
RH_2, Humidity in living room area, in %
T3, Temperature in laundry room area
RH_3, Humidity in laundry room area, in %
T4, Temperature in office room, in Celsius
RH_4, Humidity in office room, in %
T5, Temperature in bathroom, in Celsius
RH_5, Humidity in bathroom, in %
T6, Temperature outside the building (north side), in Celsius
RH_6, Humidity outside the building (north side), in %
T7, Temperature in ironing room , in Celsius
RH_7, Humidity in ironing room, in %
T8, Temperature in teenager room 2, in Celsius
RH_8, Humidity in teenager room 2, in %
T9, Temperature in parents room, in Celsius
RH_9, Humidity in parents room, in %
To, Temperature outside (from Chievres weather station), in Celsius
Pressure (from Chievres weather station), in mm Hg
RH_out, Humidity outside (from Chievres weather station), in %
Wind speed (from Chievres weather station), in m/s
Visibility (from Chievres weather station), in km
Tdewpoint (from Chievres weather station), Â°C
rv1, Random variable 1, nondimensional
rv2, Random variable 2, nondimensional
```

## 2.1 连续特征的缺失值估计

选取了三个特征: *T3* 、*风速、*和 *rv1、*进行评估。随机抽取 10%的记录，对于所选记录，三个特征标记为“缺失”。*日期*特征被替换为两个分类特征*星期*和*月*，然后它们被转换为一个热点向量。然后使用第 1 节中描述的方法估算这些缺失值，并与它们的真实值进行比较。三个估计量的合成均方根误差(RMSE)总结如下:

```
Comparison of RMSEs of Three Missing Value Estimators
-----------------------------------------------------------------**Feature   | Conditional-Expectation | Sample-Median | Sample-Mean**T3        |  0.617                  |   1.961       |  1.957 
Windspeed |  1.872                  |   2.564       |  2.527
rv1       | 14.590                  |  14.589       | 14.589
```

*   对于 *T3* ，条件期望估计量的 RMSE 小于样本中位数或样本均值 RMSE 的三分之一。
*   对于*风速*，条件期望估计器的 RMSE 约为样本中位数或样本均值 RMSE 的 73%。
*   对于 rv1，所有的三个估计器都有大致相同的 RMSE。这是意料之中的，因为 *rv1* 只是一个随机变量。

为了大致了解每个要素与其他要素的相关程度，计算了每个要素与其他要素的平均绝对相关系数，如下表所示:

```
**Feature   |  Average of Absolute Correlation Coefficients**T3        |  0.33
Windspeed |  0.13
rv1       |  0.01
```

*T3* 与其他特征的相关性最高，而 *rv1* 与其他特征的相关性最低。这解释了三个特征的不同水平的估计 RMSEs。

## 2.2 分类特征的缺失值估计

*Dayofweek* ，从*日期*特征中提取，选取进行评估。随机抽取 10%的记录， *Dayofweek* 标记为“缺失”。然后使用第 1.1 节中描述的程序估算这些缺失值。估计精度为 33%。这看起来不高，但是如果我们选择最频繁的 *Dayofweek* Tuesday 作为我们的估计，那么准确率只有 15%！

# 3 但是现实世界的特征不是多元高斯…

的确，很少有真实世界的特征遵循多元正态分布。提出的条件期望方法是基于正态性假设，但这是一个合理的假设吗？这取决于现有的信息。已知多元正态分布是给定随机变量的均值和协方差矩阵的最大熵分布[3]。因此，当唯一已知的信息是一阶和二阶矩时，多元正态分布在实践中是一个合理的假设。我对*电器能源预测*数据集的评估表明，正态性假设可以导致对真实世界缺失值的稳健而直观的估计。

# 参考

[1]约翰·T·麦考伊、史蒂夫·克伦和刘烨·奥雷特。"缺失数据插补及其在模拟铣削回路中的应用."IFAC-PapersOnLine
第 51 卷，第 21 期，第 141–146 页(2018 年)。

[2]电器能源预测数据集。UCIMachine 学习知识库。[http://archive . ics . UCI . edu/ml/datasets/Appliances+能源+预测](http://archive.ics.uci.edu/ml/datasets/Appliances+energy+prediction)。

[3]海姆·松波林斯基。"第三讲:最大化熵和信息."哈佛讲座。【https://canvas.harvard.edu/files/3549960/download? 下载 _frd=1 (2015)。