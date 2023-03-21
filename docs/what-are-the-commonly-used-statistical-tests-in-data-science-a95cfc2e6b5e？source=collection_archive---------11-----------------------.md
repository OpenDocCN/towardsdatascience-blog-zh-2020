# 数据科学中常用的统计检验有哪些

> 原文：<https://towardsdatascience.com/what-are-the-commonly-used-statistical-tests-in-data-science-a95cfc2e6b5e?source=collection_archive---------11----------------------->

## 数据科学家的便利收藏

![](img/441fbf9e4110a98cef927b0b7b92c1c3.png)

[JESHOOTS.COM](https://unsplash.com/@jeshoots?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)在 [Unsplash](https://unsplash.com/s/photos/test?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) 上拍照

业务分析和数据科学融合了许多专业领域。来自多个领域和教育背景的专业人士正在加入分析行业，追求成为数据科学家。

我职业生涯中遇到的两种数据科学家。关注算法和模型细节的人。他们总是试图理解幕后的数学和统计学。想要完全控制解决方案及其背后的理论。另一类人对最终结果更感兴趣，不看理论细节。他们着迷于新的和先进的模型的实现。倾向于解决手头的问题而不是解决方案背后的理论。

这两种方法的信徒都有自己的逻辑来支持他们的立场。我尊重他们的选择。

在这篇文章中，我将分享一些数据科学中常用的统计测试。不管你相信哪种方法，知道其中的一些是有好处的。

在统计学中，从任何探索中得出推论有两种方法。参数估计是方法之一。这里，通过各种方法计算总体参数的未知值。另一种方法是假设检验。它有助于我们测试由一些先验知识猜测的参数值。

我将列出一些你在数据科学中经常会遇到的统计测试程序。

> "检验一个假设有效性的唯一相关方法是将其预测与经验进行比较."—米尔顿·弗里德曼

# 作为一名数据科学家，我真的需要了解假设检验吗？

在数据科学的大多数决策过程中，我们都知道或不知不觉地使用假设检验。这里有一些证据来支持我的说法。

作为数据科学家，我们所做的数据分析可以分为四大领域

1.  探索性数据分析

2.回归和分类

3.预测

4.数据分组

这些领域中的每一个都包括一些统计测试。

# 探索性数据分析

这是数据科学不可避免的一部分，每个数据科学家都要在其中花费大量时间。它为创建机器学习和统计模型奠定了基础。EDA 中涉及统计测试的一些常见任务是—

1.  正态性检验

2.异常值测试

3.相关性测试

4.匀性检验

5.分布相等性检验

这些任务中的每一项都涉及到在某一点上对假设的检验。

**1。如何检验正常？**

统计学中正态性无处不在。我们在统计学中使用的大多数理论都是基于正态假设。正态性意味着数据应该遵循一种特定的概率分布，即正态分布。它有特定的形状和特定的功能。

在方差分析中，我们假设数据是正态的。在进行回归时，我们期望残差服从正态分布。

为了检查数据的正态性，我们可以使用夏皮罗-维尔克检验。此测试的零假设是—数据样本的分布是正态的。

Python 实现:

```
import numpy as np
from scipy import stats
data = stats.norm.rvs(loc=2.5, scale=2, size=100)
shapiro_test = stats.shapiro(data)
print(shapiro_test)
```

**2。如何检验一个数据点是否是离群值？**

当我开始任何新的数据科学用例时，我必须适应一些模型，我做的例行任务之一是检测响应变量中的异常值。异常值对回归模型影响很大。对于异常值，需要采取谨慎的消除或替换策略。

如果离群值明显偏离数据的其余部分，则离群值可以是全局离群值。如果它仅偏离源于特定上下文的数据点，则称为上下文异常值。此外，当一组数据点与其他数据点偏离很大时，它们也可能是异常值。

Tietjen-Moore 检验对于确定数据集中的多个异常值非常有用。此测试的零假设是—数据中没有异常值。

Python 实现:

```
import scikit_posthocs
x = np.array([-1.40, -0.44, -0.30, -0.24, -0.22, -0.13, -0.05, 0.06, 0.10, 0.18, 0.20, 0.39, 0.48, 0.63, 1.01])
scikit_posthocs.outliers_tietjen(x, 2)
```

**3。如何检验两个变量相关系数的显著性？**

在数据科学中，我们处理许多解释因变量行为的自变量。自变量之间的显著相关性可能会影响变量的估计系数。这使得回归系数的标准误差不可靠。这损害了回归的可解释性。

当我们计算两个变量之间的相关性时，我们应该检查相关性的显著性。可以用 t 检验来检验。该检验的零假设假设变量之间的相关性不显著。

Python 实现:

```
from scipy.stats import pearsonr
data1 = stats.norm.rvs(loc=3, scale=1.5, size=20)
data2 = stats.norm.rvs(loc=-5, scale=0.5, size=20)
stat, p = pearsonr(data1, data2)
print(stat, p)
```

**4。如何在两个数据集中检验一个分类变量的同质性？**

如果我用一个例子来解释同质性检验会很方便。假设你，我们想检查网飞用户的观看偏好对于男性和女性是否相同。你可以用卡方检验来证明相同。你必须检查男性和女性的频率分布是否明显不同。

测试的零假设是两个数据集是同质的。

Python 实现:

```
import scipy
import scipy.stats
from scipy.stats import chisquare
data1 = stats.norm.rvs(loc=3, scale=1.5, size=20)
data2 = stats.norm.rvs(loc=-5, scale=0.5, size=20)
chisquare(data1, data2)
```

**5。如何检查给定的数据集是否遵循特定的分布？**

有时在数据分析中，我们需要检查数据是否遵循特定的分布。甚至我们可能想要检查两个样本是否遵循相同的分布。在这种情况下，我们使用 Kolmogorov-Smirnov (KS)检验。我们经常使用 KS 检验来检验回归模型的拟合优度。

该测试将经验累积分布函数(ECDF)与理论分布函数进行比较。此测试的零假设假设给定数据遵循指定的分布。

Python 实现:

```
from scipy import stats
x = np.linspace(-25, 17, 6)
stats.kstest(x, ‘norm’)
```

# 回归和分类

我们在数据科学中做的大多数建模要么属于回归，要么属于分类。每当我们预测某个值或某个类时，我们都会借助这两种方法。

回归和分类都涉及决策不同阶段的统计测试。此外，数据需要满足一些先决条件，才能胜任这些任务。需要进行一些测试来检查这些条件。

一些与回归和分类相关的常见统计测试是—

1.  异方差检验

2.多重共线性测试

3.回归系数的显著性检验

4.回归或分类模型的方差分析

**1。如何检验异方差性？**

异方差是一个相当沉重的术语。它只是意味着不平等的方差。我举个例子解释一下。假设您正在收集不同城市的收入数据。你会发现不同城市的收入差异很大。

如果数据是异方差的，它会极大地影响回归系数的估计。这使得回归系数不太精确。估计值将与实际值相差甚远。

为了检验数据中的异方差性，可以使用怀特检验。White 的测试考虑了零假设，即数据的方差是常数。

Python 实现:

```
from statsmodels.stats.diagnostic import het_white
from statsmodels.compat import lzip
expr = ‘y_var ~ x_var’
y, X = dmatrices(expr, df, return_type=’dataframe’)
keys = [‘LM stat’, ‘LM test p-value’, ‘F-stat’, ‘F-test p-value’]
results = het_white(olsr_results.resid, X)
lzip(keys, results)
```

**2。如何测试变量中的多重共线性？**

数据科学问题通常包括多个解释变量。有时，这些变量由于其来源和性质而变得相关。此外，有时我们会从相同的潜在事实中创建多个变量。在这些情况下，变量变得高度相关。这就是所谓的多重共线性。

多重共线性的存在会增加回归或分类模型系数的标准误差。它使得一些重要的变量在模型中变得无关紧要。

法勒-格劳伯检验可用于检查数据中多重共线性的存在。

**3。如何检验模型系数是否显著？**

在分类或回归模型中，我们需要识别对目标变量有强烈影响的重要变量。模型执行一些测试，并为我们提供变量的重要程度。

t 检验用于模型中检查变量的显著性。检验的无效假设是-系数为零。您需要检查测试的 p 值，以了解系数的显著性。

Python 实现:

```
from scipy import stats
rvs1 = stats.norm.rvs(loc=5,scale=10,size=500)
stats.ttest_1samp(rvs1, 7)
```

**4。如何检验一个模型的统计显著性？**

在开发回归或分类模型时，我们进行方差分析(ANOVA)。它检查回归系数的有效性。ANOVA 将模型引起的变化与误差引起的变化进行比较。如果模型引起的变化与误差引起的变化显著不同，则变量的影响显著。

f 检验用于做出决定。此测试中的零假设是—回归系数等于零。

Python 实现:

```
import scipy.stats as stats
data1 = stats.norm.rvs(loc=3, scale=1.5, size=20)
data2 = stats.norm.rvs(loc=-5, scale=0.5, size=20)
stats.f_oneway(data1,data2)
```

# 预测

在数据科学中，我们处理两种数据——横截面和时间序列。电子商务网站上的一组客户的简档是横截面数据。但是，电子商务网站中一个项目一年的日销售额将是时间序列数据。

我们经常使用时间序列数据的预测模型来估计未来的销售额或利润。但是，在预测之前，我们会对数据进行一些诊断性检查，以了解数据模式及其对预测的适用性。

作为一名数据科学家，我经常对时间序列数据使用这些测试:

1.  趋势测试

2.平稳性测试

3.自相关测试

4.因果关系检验

5.时间关系测试

**1。如何测试时间序列数据的趋势？**

随着时间的推移，从业务中生成的数据通常会显示上升或下降的趋势。无论是销售或利润，还是任何其他描述业务绩效的绩效指标，我们总是更喜欢估计未来的变化。

为了预测这种变化，你需要估计或消除趋势成分。要了解趋势是否显著，您可以使用一些统计测试。

曼-肯德尔检验可以用来检验趋势的存在性。零假设假设没有显著的趋势。

Python 实现:

```
pip install pymannkendall
import numpy as np
import pymannkendall as mk
data = np.random.rand(250,1)
test_result = mk.original_test(data)
print(test_result)
```

**2。如何检验一个时间序列数据是否平稳？**

非平稳性是大多数时间序列数据的固有特征。在任何时间序列建模之前，我们总是需要测试平稳性。如果数据不稳定，建模后可能会产生不可靠和虚假的结果。会导致对数据理解不佳。

扩充的 Dickey-Fuller (ADF)可用于检查非平稳性。ADF 的零假设是序列是非平稳的。在 5%的显著性水平，如果 p 值小于 0.05，我们拒绝零假设。

Python 实现:

```
from statsmodels.tsa.stattools import adfuller
X = [15, 20, 21, 20, 21, 30, 33, 45, 56]
result = adfuller(X)
print(result)
```

**3。如何检查时间序列值之间的自相关性？**

对于时间序列数据，过去值和现在值之间的因果关系是一种普遍现象。对于金融时间序列，我们经常看到当前价格受到前几天价格的影响。时间序列数据的这一特征是通过自相关来度量的。

要知道自相关性是否足够强，可以测试一下。德宾-沃森测试揭示了它的程度。此测试的零假设假设值之间没有自相关。

Python 实现:

```
from statsmodels.stats.stattools import durbin_watson
X = [15, 20, 21, 20, 21, 30, 33, 45, 56]
result = durbin_watson(X)
print(result)
```

**4。如何测试一个变量对另一个变量有因果关系？**

两个时间序列变量可以共享因果关系。如果你熟悉金融衍生品，一种定义在基础股票上的金融工具，你会知道现货和期货价值有因果关系。他们根据情况相互影响。

格兰杰因果检验可以检验两个变量之间的因果关系。该测试使用回归设置。一个变量的当前值根据另一个变量的滞后值以及自身的滞后值回归。f 检验确定无因果关系的零假设。

Python 实现:

```
import statsmodels.api as sm
from statsmodels.tsa.stattools import grangercausalitytests
import numpy as np
data = sm.datasets.macrodata.load_pandas()
data = data.data[[“realgdp”, “realcons”]].pct_change().dropna()
gc_res = grangercausalitytests(data, 4)
```

**5。如何检查两个变量之间的时间关系？**

两个时间序列有时会一起移动。在金融时间序列中，你会经常观察到衍生品的现货和期货价格一起变动。

这种协同运动可以通过一个叫做协整的特征来检验。这种协整可以用 Johansen 的检验来检验。该检验的零假设假设变量之间没有协整关系。

Python 实现:

```
from statsmodels.tsa.vector_ar.vecm import coint_johansen
data = sm.datasets.macrodata.load_pandas()
data = data.data[[“realgdp”, “realcons”]].pct_change().dropna()
#x = getx() # dataframe of n series for cointegration analysis
jres = coint_johansen(data, det_order=0, k_ar_diff=1
print(jres.max_eig_stat)
print(jres.max_eig_stat_crit_vals)
```

# 数据分组

在现实生活中，我们经常试图在数据点之间寻找相似性。目的是将它们组合在一些桶中，并仔细研究它们以了解不同桶的行为。

这同样适用于变量。我们识别一些潜在的变量，这些变量是由一些可观察的变量组合而成的。

零售店可能有兴趣对其客户进行细分，如成本意识、品牌意识、批量购买者等。它需要根据客户的特征(如交易、人口统计、心理特征等)对客户进行分组。

在这方面，我们经常会遇到以下测试:

1.球形度测试

2.抽样充分性测试

3.聚类趋势测试

**1。如何检验变量的球形度？**

如果数据中的变量数量非常多，这种情况下的回归模型往往表现不佳。此外，识别重要变量变得具有挑战性。在这种情况下，我们试图减少变量的数量。

主成分分析(PCA)是一种减少变量数量和识别主要因素的方法。这些因素将帮助你建立一个降维的回归模型。此外，帮助识别任何感兴趣的对象或事件的关键特征。

现在，变量只有在它们有一定的相关性时才能形成因子。它是通过巴特列氏试验来检验的。该检验的无效假设是——变量不相关。

Python 实现:

```
from scipy.stats import bartlett
a = [8.88, 9.12, 9.04, 8.98, 9.00, 9.08, 9.01, 8.85, 9.06, 8.99]
b = [8.88, 8.95, 9.29, 9.44, 9.15, 9.58, 8.36, 9.18, 8.67, 9.05]
c = [8.95, 9.12, 8.95, 8.85, 9.03, 8.84, 9.07, 8.98, 8.86, 8.98]
stat, p = bartlett(a, b, c)
print(p, stat)
```

**2。如何检验变量的抽样充分性？**

当样本量足够大时，PCA 方法将产生可靠的结果。这被称为抽样充分性。将对每个变量进行检查。

凯泽-迈耶-奥尔金(KMO)检验用于检查整个数据集的抽样充分性。该统计测量变量中可能是普通方差的方差的比例。

Python 实现:

```
import pandas as pd
from factor_analyzer.factor_analyzer import calculate_kmo
a = [8.88, 9.12, 9.04, 8.98, 9.00, 9.08, 9.01, 8.85, 9.06, 8.99]
b = [8.88, 8.95, 9.29, 9.44, 9.15, 9.58, 8.36, 9.18, 8.67, 9.05]
c = [8.95, 9.12, 8.95, 8.85, 9.03, 8.84, 9.07, 8.98, 8.86, 8.98]
df= pd.DataFrame({‘x’:a,’y’:b,’z’:c})
kmo_all,kmo_model=calculate_kmo(df)
print(kmo_all,kmo_model)
```

**3。如何测试数据集的聚类趋势？**

为了将不同桶中的数据分组，我们使用了聚类技术。但是在进行聚类之前，你需要检查数据中是否有聚类趋势。如果数据具有均匀分布，则不适合聚类。

霍普金斯检验可以检查变量的空间随机性。此测试中的零假设是—数据是从非随机、均匀分布中生成的。

Python 实现:

```
from sklearn import datasets
from pyclustertend import hopkins
from sklearn.preprocessing import scale
X = scale(datasets.load_iris().data)
hopkins(X,150)
```

在本文中，我提到了一些数据科学中常用的测试。还有很多其他的我不能提及。如果你发现了一些我在这里没有提到的，请告诉我。

参考:

[https://www . stats models . org/dev/generated/stats models . TSA . stat tools . grangercausalitytests . html](https://www.statsmodels.org/dev/generated/statsmodels.tsa.stattools.grangercausalitytests.html)

[https://pypi.org/project/pyclustertend/](https://pypi.org/project/pyclustertend/)