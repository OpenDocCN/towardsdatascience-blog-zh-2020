# ARMA、VAR 和 Granger 因果关系的乐趣

> 原文：<https://towardsdatascience.com/fun-with-arma-var-and-granger-causality-6fdd29d8391c?source=collection_archive---------18----------------------->

## 使用 Stata、R 和 Python

![](img/1fd7e8aa461ba178ef6efcdc3156c075.png)

杰克·希尔斯在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上的照片

> 将你在日食期间看到的东西与夜晚的黑暗相比较，就像将海洋比作一滴泪珠。~温迪·马斯

温迪大概是在暗示，在昏暗的月蚀中，视觉的清晰度比普通的夜晚要清晰得多。或者，与裹尸布时期的黑暗相比，普通的夜晚算不了什么。同样，我觉得时间序列的概念会一直模糊不清，直到数学让它变得更容易理解。即使在日常生活中使用合法数学的重复出现实际上类似于模糊，然而通过数学证明澄清时间序列的强度与利用可测量的统计工具来对抗和设想一些信息相比不算什么。然而，在我的读者跳入大海之前，我希望他们今天能留下“泪珠”。解释是每个夜晚都有月亮，而在晚霞中漫步正是感性的，直到科学交出一些光来看透。这篇文章背后的动机是通过各种工具精确地看到一点时间序列(就像每个夜晚一样)，期望数字澄清的模糊不清是不常见的和遥远的，但无疑是重要的，以另一种方式看一看天空。

**议程**

![](img/e083759fe50b7a74877cc227a62185c6.png)

文章议程，图片来源:(图片来自作者)

**白噪声**

在文学术语中，白噪声是所有频率的声音同时产生的声音。这是可以听到的，我们也一定听到了。这里有一个[链接](https://www.youtube.com/watch?v=EY5OQ2iVA50)如果我们还没有。在我们的例子中，白噪声是满足三个条件的时间序列；均值为零、方差不变且序列中两个连续元素之间不相关的序列。白噪声序列是不可预测的，因此表明它不需要任何进一步的分析。为了测试第三个条件，我们通常使用 ACF(自相关函数)图，它测试两个连续观察值之间的相关性。如果相关性不为零，那么该滞后处的棒线将**穿过每一侧的边界线**。

**R 视图**

![](img/2a7b749c5a0ba138e31232097b4c4517.png)

白噪音系列，图片来源:(图片来自作者)，图片工具:R

第一个系列具有近似零均值和 1 方差，而第二个系列具有近似 0.5 均值和 0.7 方差。ACF 图没有跨越阈值边界的波段，除了第 0 个滞后(这是观察值与其自身的相关性)。第二个系列的 ACF 图具有明显的滞后相关性。视觉上，我们也可以验证第二系列中的变化。

**R**[代码 ](https://gist.github.com/arimitramaiti/218783f86d984fd3081600d9331b7989)

**Python 视图**

![](img/27785f57a14d3017056db26dde5d0bb9.png)

白噪音系列，图片来源:(图片来自作者)，图片工具:Python

**Python** [**代码**](https://gist.github.com/arimitramaiti/87c24c4842386561341b92cbebef5ee4)

*哪个看起来更简单？哪一个更像满月？*

**平稳性**

> 所有的碱都是碱，但不是所有的碱都是碱。~学校化学

平稳时间序列遵循与白噪声序列几乎相似的条件。固定系列必须满足三个条件:具有恒定平均值、恒定方差且无季节性模式的序列。*基本上，一个统计性质如均值、方差、自相关等都随时间恒定的序列*。因此，所有白噪声序列基本上是平稳的，但所有平稳序列并不总是白噪声，因为均值可以是大于零的常数。任何时间序列建模的关键步骤是在尝试拟合模型之前将原始序列转换为平稳序列。无需等待日食，公平地说，这类似于在拟合 OLS 模型之前高斯马尔可夫假设是必要的。谢伊·帕拉奇在这里做了出色的工作解释了稳定的原因*海洋确实是无限的*。底线是用非平稳数据估计模型可能会产生不正确的模型估计。接下来我们来看一些非平稳序列的例子。

![](img/d9ec5c95458586c560b3e3243f03b24e.png)

非平稳时间序列例子，图片来源:(图片来自作者)，图片工具:R，图片代码:[链接](https://gist.github.com/arimitramaiti/bb8358faa99e863af5dda894ae142bfc)

**流行的平稳性测试**

在我们测试平稳性条件之前，最好的事情是了解可以将原始时间序列转换为平稳序列的流行方法。最传统和方便的方法被称为“差异”和“增长率”。还有更多定制的方法，如“去除趋势分量”、“电平移动检测”等，这些方法默认情况下都包含在当今高端时间序列特定的机器学习包中。

**差异**是从序列中去除随机趋势或季节性的一种非常常见的做法。一阶差分意味着在滞后 1 时简单地取当前和先前观测值的差，而二阶差分在滞后 2 时做同样的事情。**增长率**方法(有时称为一期简单回归)主要用于金融或宏观经济数据(如人均 GDP、消费支出)。它采用与前一时间段相关的当前观察值的对数(例如 log[xt÷xt-1])。

增广的 Dickey-Fuller (ADF)是最常用的平稳性检验，它遵循自己的增广的 Dickey-Fuller 分布，而不是常规的 t 分布或 z 分布。**零假设**是数列**不平稳**，而**替代假设**是数列**平稳**。

**R 中的 ADF 测试**

![](img/597341aa9b84161ce80d7504f31501f0.png)

ADF 测试，图片来源:(图片来自作者)，图片工具:R，[链接](https://raw.githubusercontent.com/arimitramaiti/datasets/master/articles/a5_data.csv)到数据

**R** [**代码**](https://gist.github.com/arimitramaiti/294378141e885447f420a1d8197154fd)

**Python 中的 ADF 测试**

![](img/71113caf33adb262f3eb5dcba2581a42.png)

ADF 测试，图片来源:(图片来自作者)，图片工具:Python，[链接](https://raw.githubusercontent.com/arimitramaiti/datasets/master/articles/a5_data.csv)到数据

**Python** [**代码**](https://gist.github.com/arimitramaiti/87e9644e5e554e7853d22f8764652000)

*我们是否注意到使用这两种工具我们得到了相同的* ***结果*** *，然而在 R & Python 中从 ADF 测试中获得的 p 值仍然有一些差异？嗯，一个可能的答案是两个软件使用的默认延迟是不同的。*

**在 Stata (Dickey-Fuller)中进行 DF 测试**

![](img/77882a25ee9a4c018f2fa863ded7aee2.png)

DF 测试，图片来源:(图片来自作者)，图片工具:Stata，[链接](https://raw.githubusercontent.com/arimitramaiti/datasets/master/articles/a5_data.csv)到数据

滞后=0 时的 ADF 检验成为 Dickey-Fuller 检验。结果可能或可能不总是相似的，但是，在这个特定的例子中，我们观察到相似性(即，原始序列不是静止的，而变换后的序列是静止的)。**在使用 Stata** 时，**零假设**是相同的，即数列**不平稳**，而**替代假设**是数列**平稳**。判定规则是，如果检验统计量的绝对值小于 5%临界(来自 ADF/DF 分布),那么我们不能拒绝零，并得出序列不是平稳的结论。因此，上图左侧的两个图表的绝对值小于检验统计量，因此我们无法拒绝零假设。

**Stata** [**代码**](https://gist.github.com/arimitramaiti/0c32eb4fd1d6614ed8c500a303d4f991)

**ARMA 过程**

自回归(AR)模型意味着序列的当前值取决于序列的过去值。对于一个简单的 AR(1)模型，假设序列的当前值取决于它的第一个滞后。参数系数值、相应的标准误差及其 p 值暗示了相关性的强度。假设误差项正态分布，均值为零，方差为σ平方常数。而移动平均(MA)模型意味着一个序列的当前值取决于该序列过去的误差。AR 模型可以表示为无限 MA 模型，反之亦然。ARMA 是一种混合模型，其中当前值考虑了序列中的 AR 和 MA 效应。

![](img/fda0761538209d30382a5290aea89bf8.png)

基本的 Box-Jenkins 流程图，图片来源:(图片来自作者)

George Box 和 Gwilym Jenkins 提供了一个工作流来预测时间序列数据。尽管该系统具有各种其他表示和定制，但是基本的概述将保持几乎相似。根据问题的性质和预测算法的复杂程度，此工作流可以扩展到分析工作台中的每个细节级别。上面的流程图是我的代表，只是为了记住主要步骤，我敦促我的读者在处理越来越多的时间序列问题时，也做出自己的流程图。

在我让我的读者更进一步之前，我想提一下，阿卡克信息准则(AIC)、贝叶斯信息准则(BIC)或汉南-奎因信息准则(HQIC)在帮助选择最佳模型方面做了同样的工作。我们总是努力寻找一个不太节俭的模型(具有较少的参数)，但同时选择上述度量的相对较小的值。IC 越小，残差越小。虽然 AIC 是最常见的，但 BIC 或 HQIC 在惩罚更多参数方面对大数据更先进和更强大。

**Stata 中的 ARMA**

![](img/9bb2a717863f60b72da425da2a7d3bc5.png)

ARMA(2，1)的输出，图像源:(来自作者的图像)，图像工具:Stata，[链接](https://raw.githubusercontent.com/arimitramaiti/datasets/master/articles/a5_data.csv)到数据

![](img/4e8dc0c2af6352485f64de68349abfa9.png)

模型迭代输出，ARMA，图片来源:(图片来自作者)，图片工具:Stata，[链接](https://raw.githubusercontent.com/arimitramaiti/datasets/master/articles/a5_data.csv)到数据

ARMA 过程的 Stata 代码

![](img/0e893f761a4ab38e0575244682413b4c.png)

AIC，BIC 得分图，GDP 增长率，ARMA，图片来源:(图片来自作者)，图片工具:Python，[链接](https://raw.githubusercontent.com/arimitramaiti/datasets/master/articles/a5_data.csv)到数据

AIC 在 ARMA (4，2)最小，BIC 在 ARMA (2，0)最小。BIC 提出了一个不太节俭的模型，有 3 个参数，而 AIC 模型有 8 个参数。ARMA(2，0)的 sigma 系数为 0.0052，大于 ARMA(4，2)的 0.0049。ARMA(2，0)的最大对数似然为 461.51，低于 ARMA(4，2)的 466.40。然而，以五个额外参数为代价，我们可能选择 **ARMA(2，0)** 。适马是模型假设的恒定标准差，用于模型在幕后使用的创新或随机冲击(不相关的零均值随机变量)。在 Stata 中，它是 sigma，但在 R 中，同样的东西显示为 sigma 的平方。

![](img/494d8254b0f2a7c4abd0ec7989384c11.png)

ACF 和残差图，ARMA(2，0)，图像源:(来自作者的图像)，图像工具:Stata，[链接](https://raw.githubusercontent.com/arimitramaiti/datasets/master/articles/a5_data.csv)到数据

用于检查模型残差中自相关的 Stata 代码

**R 中的 ARMA**

![](img/36bb061604f21af0939c3206c20a96ad.png)

ACF 图，GDP 增长率残差，ARMA(p，q)，图片来源:(图片来自作者)，图片工具:R，[链接](https://raw.githubusercontent.com/arimitramaiti/datasets/master/articles/a5_data.csv)到数据

![](img/0dfce5b32b87f38e484e79c5951d4241.png)

AIC，BIC 得分图，GDP 增长率，ARMA，图片来源:(图片来自作者)，图片工具:R，[链接](https://raw.githubusercontent.com/arimitramaiti/datasets/master/articles/a5_data.csv)到数据

![](img/3303631c14e06b8cbf7569dca07a2437.png)

ARMA 适马估计，GDP 增长率，ARMA，图片来源:(图片来自作者)，图片工具:R，[链接](https://raw.githubusercontent.com/arimitramaiti/datasets/master/articles/a5_data.csv)到数据

*虽然我们的目标是获得一个随机冲击方差最小的模型，但是选择红线意味着相对大量的参数。这样做的成本高于蓝线，因为蓝线需要维护的参数较少。*

ARMA 过程的 r 代码

**Python 中的 ARMA**

![](img/1565e995a469c9c4ef63386fbba836e0.png)

ACF 图，GDP 增长率残差，ARMA(p，q)，图片来源:(图片来自作者)，图片工具:Python，[链接](https://raw.githubusercontent.com/arimitramaiti/datasets/master/articles/a5_data.csv)到数据

![](img/ecc7561e4808fb99df960ada65acf3ba.png)

AIC，BIC，HQIC 得分图，GDP 增长率，ARMA，图片来源:(图片来自作者)，图片工具:Python，[链接](https://raw.githubusercontent.com/arimitramaiti/datasets/master/articles/a5_data.csv)到数据

ARMA 过程的 Python 代码

**对 ARMA 的观察**

![](img/cd3fcdc99984f4df07e5eb689e446fb8.png)

图片来源: (图片来自作者)，图标来源:( [Stata](https://www.google.com/search?q=stata+icon&safe=active&sxsrf=ALeKk009zuLZ9NYZfTQyI4sYOMciZ1OLYA:1602173259501&tbm=isch&source=iu&ictx=1&fir=e7JObCDad1xjWM%252CILUZVjjbJS8lFM%252C_&vet=1&usg=AI4_-kTA0h70sVhNVapDx1c8RWaun3iX8w&sa=X&ved=2ahUKEwi56IDAsKXsAhWazzgGHVH5DqQQ9QF6BAgNEFE&biw=1366&bih=625#imgrc=e7JObCDad1xjWM) ， [R](https://www.google.com/search?q=rstudio+icon&tbm=isch&ved=2ahUKEwiyufbCsKXsAhVQWisKHVNRAd8Q2-cCegQIABAA&oq=rstudio+icon&gs_lcp=CgNpbWcQAzICCAAyAggAMgYIABAHEB4yBggAEAcQHjIGCAAQBxAeMgYIABAFEB4yBggAEAUQHjIECAAQHjIECAAQGDIECAAQGDoECAAQQzoICAAQBxAFEB46BggAEAgQHlD_xQJYpdUCYMraAmgAcAB4AIABvwGIAb8GkgEDMC43mAEAoAEBqgELZ3dzLXdpei1pbWfAAQE&sclient=img&ei=UTl_X_LMJdC0rQHTooX4DQ&bih=625&biw=1366&safe=active#imgrc=eXBjG6YYM22OHM) ， [Python](https://www.google.com/search?q=python+logo&tbm=isch&ved=2ahUKEwib1571sKXsAhUTRysKHUOmCwIQ2-cCegQIABAA&oq=python+logo&gs_lcp=CgNpbWcQAzIFCAAQsQMyBQgAELEDMgIIADICCAAyAggAMgIIADICCAAyAggAMgIIADICCAA6BAgjECc6BAgAEENQmYQDWK6QA2DLkgNoAHAAeACAAYMBiAGGB5IBAzAuOJgBAKABAaoBC2d3cy13aXotaW1nwAEB&sclient=img&ei=uzl_X9uNCJOOrQHDzK4Q&bih=625&biw=1366&safe=active#imgrc=TwZF3yMqP3cXhM) )

易于访问:这一点非常清楚，因为 Stata 是一个经过授权的设备，R 和 Python 都是开源的。从今以后，接触任何开源工具都比坐等授权编程简单。

用户友好性:Stata 提供了一个图形用户界面(GUI ),这也是它对学徒或过渡人员更有吸引力的原因。它就像 SPSS、SAS 或 Eviews。它类似于 SPSS、SAS 或 Eviews。我以某种方式了解了我在学习上的差距，我需要学习的主题，以及可用技术的目录。相反，R 和 Python 都是脚本工具，除非我主动，否则它们不会交互。任何前端有图形用户界面的设备在开始的时候都不那么笨拙。结构变得更加精炼，不像在脚本工具中偶然发现的那样。焦点从解释输出转移到首先实际获得输出。我想这是任何此类工具价格高昂的主要原因。

易学性:上述用户友好性的优势无疑使通过 Stata 学习变得简单而有趣。然而，在这种情况下，R & Python 的顺序可能不是固定的，因为它也取决于一个人接触该工具的顺序。我先接触了 R，后来接触了 Python，然而，事情也可能与大多数人相反。

可视化:python 可视化工具的舒适性及其魅力是毋庸置疑的，因为它是一个开源的免费软件。相反，R 显然是接班人。然而，不知何故，我从这个以及其他一些练习中感觉到，尽管与默认的 plot 方法相比，ggplot 可以挽救很多次，但是一开始就利用 python 会更好。如果 python 中有类似于 R 时间序列绘图功能的东西，可以自动调整 x 轴标签，那就更好了。他们确实停用了 sns.ts plots 并引入了 sns.lineplot()，但它并不是每次都与 r 匹配。

ARMA 估计:Stata 在这种特定情况下估计得更好。警告消息“可能的收敛问题:最优给定码= 1”在通过 R 的 ARIMA/ARMA 估计中非常常见，但在大多数情况下，它确实会生成除 p 值和标准误差之外的大多数输出。除此之外，异方差校正的标准误差有些难以实现，除非手动编码。与 R 不同，在 python 中可以很容易地访问异方差校正的标准误差，但是，statsmodels.tsa.arima_model 中的 ARIMA 函数通常会令人惊讶地停止工作。它迫使我们改用 SARIMAX。这就是为什么我们得到的 ARMA(1，1)与 Stata 或 r 中的 ARMA(2，0)非常接近。普通 ARMA/ARIMA 不允许由于滞后多项式反演而覆盖强制平稳性或可逆性。理想情况下，ARMA 过程也可以转换为滞后多项式项，并且它不能放在此函数中，这与 SARIMAX 不同。详细讨论可以在[这里](https://github.com/statsmodels/statsmodels/issues/6225)找到。

可伸缩性:在大多数情况下，python 比任何其他工具都更受青睐。在这种特定情况下，我不知何故发现了在各阶之间迭代以估计模型系数的过程，并且在 5 乘 5 矩阵上绘制残差相对比 R 更容易、更轻便。老实说，在这个示例中使用 R 也很有趣，我无意贬低这个几乎同样强大的工具。

**向量自动回归**

> 乍一看，VAR 似乎是单变量自回归模型的简单多变量推广。乍一看，它们被证明是现代宏观经济学中的关键实证工具之一。~Del Negro 和 Schorfheide

研究人员发现,( p，q)阶的 ARMA 过程可以转化为(p)阶的 AR 过程。因此，任何两个或多个序列的 AR(p)过程都可以转化为 VAR(p)过程。在我们上面的例子中，我们看到了两个不同的序列，120 个时间点的人均 GDP 和消费支出。这两个单独的序列会有自己的 ARMA(p，q)过程。基于 Box-Jenkins，我们推导出，一旦我们在每个序列中实现平稳性，我们可以在 AR(p)和 MA(q)的不同值上迭代该序列，并选择 ARMA(p，q)的阶数，其中我们具有最小的 AIC、BIC 或 HQIC。在这种情况下，人均 GDP 的平稳序列是 GDP 增长率，根据我们使用的工具，它遵循 ARMA(2，0)或 ARMA(1，1)。此外，平稳的消费支出序列可以是其增长率遵循 ARMA(1，0)过程。

![](img/80d6186c69c3abef420f68ffd7f5c61b.png)

AIC，BIC 得分图，消费增长率，ARMA，图片来源:(图片来自作者)，图片工具:R，[链接](https://raw.githubusercontent.com/arimitramaiti/datasets/master/articles/a5_data.csv)到数据

当我们要处理多个线性时间序列时，需要向量自回归的基本框架。如果我们考虑两个序列 X 和 Y，那么 VAR 运行两个回归模型。第一个方程组根据 X 和 Y 的过去滞后值以及 X 的一些误差项来回归 X。类似地，在第二个方程组中，我们根据 Y 和 X 的过去滞后值以及 Y 的一些误差项来回归 Y。滞后的阶数可以从 0 阶到 p 阶，就像 AR(p)模型或 ARMA(p，q)模型一样。如果我们有 3 个多元线性时间序列要处理，那么我们将有 3 个回归方程组，如果我们有 4 个多元线性时间序列要处理，那么我们将有 4 个回归方程组，因此如果有 k 个多元线性时间序列要处理，那么我们将有 k 个回归方程组。

![](img/751c55d6cb95e16dae3fa7428eef3174.png)

VAR(2)模型，图片来源:(图片来自作者)，图片灵感:[链接](https://www.econometrics-with-r.org/16-1-vector-autoregressions.html)

**格兰杰因果关系**

1956 年，诺伯特·维纳引入了一个概念，即在测量两个线性时间序列时，如果一个人可以通过使用两个序列的过去信息来预测第一个序列，而不是仅使用第一个序列的过去信息而不使用第二个序列，那么他可以推断第二个序列导致了第一个序列。第一个实际工作是由克莱夫·格兰杰完成的，此后这种方法被命名为格兰杰因果关系。经济学家格韦克在 1982 年也做了进一步的发展，被称为格韦克-格兰杰因果关系。因此，这一概念进一步扩展了风险值模型的使用案例，人们可以从统计上检验一个时间序列是否是另一个时间序列的原因。如果我们有证据证明第一个数列格兰杰因果第二个数列，我们可以推断第一个数列是因，第二个数列是果。话虽如此，效果必须取决于原因的过去价值。此外，在存在结果的过去值的情况下，原因的过去值必须有助于识别结果的当前值。

**Stata 中的 VAR**

![](img/95566e72bc33d6e734f128bc5b7ce812.png)

VAR(1)的输出，图像源:(来自作者的图像)，图像工具:Stata，[链接](https://raw.githubusercontent.com/arimitramaiti/datasets/master/articles/a5_data.csv)到数据

![](img/636c2fe868200ce707c54254348b51bc.png)

模型迭代输出，VAR，图片来源:(图片来自作者)，图片工具:Stata，[链接](https://raw.githubusercontent.com/arimitramaiti/datasets/master/articles/a5_data.csv)到数据

VAR 模型估计的 Stata 代码

AIC 在 VAR(1)最小，AIC = -1870.592。

BIC 在 VAR(1)最小，BIC = -1853.92。AIC 和 BIC 都提出了 VAR(1)模型。

![](img/93bb89c29a5e39dd542168bb19fe3009.png)

VAR(1)上的拉格朗日乘数测试，图片来源:(图片来自作者)，图片工具:Stata，[链接](https://raw.githubusercontent.com/arimitramaiti/datasets/master/articles/a5_data.csv)到数据

上述测试类似于我们在发现 ARMA 模型后看到的 ACF 相关图。目标几乎类似于检查来自模型的误差之间是否有任何相关性。换句话说，我们不希望任何给定时间点的误差项与其滞后误差值之间有任何修正。这个检验的无效假设是错误没有序列相关性，而另一个假设是错误有序列相关性。决策规则是，如果 p>0.05，那么我们无法拒绝零假设，并得出错误确实没有序列相关性的结论。测试代码如下所示。

用于检查模型残差中自相关的 Stata 代码

**Stata 中的格兰杰因果关系**

一旦 VAR 模型被识别和估计，我们可能要测试 VAR(1)模型的因果关系假设。零假设是自变量和因变量之间没有短期因果关系。另一个假设是自变量和因变量之间存在短期因果关系。判定规则是如果 p <0.05 then reject the Null and conclude there is causality, else conclude there is no short-run causality.

![](img/f51fc7449f2e823b66a342edabf9af2a.png)

VAR(1) Stability Test, Image Source : (Image from Author), Image Tool: Stata, [将](https://raw.githubusercontent.com/arimitramaiti/datasets/master/articles/a5_data.csv)链接到数据

建议在因果关系检验之前检查所选的 VAR 模型是否稳定。如果模型不稳定，那么它可能意味着要么进入模型的序列不是平稳的，要么需要进行长期因果关系的协整检验，这实质上意味着建立一个向量误差修正模型。然而，在这种情况下选择的 VAR(1)是稳定的。

![](img/6d7cde0f93518a1413deb8b6c4fd8473.png)

VAR(1)的 Granger 检验，图片来源:(图片来自作者)，图片工具:Stata，[链接](https://raw.githubusercontent.com/arimitramaiti/datasets/master/articles/a5_data.csv)到数据

检验格兰杰因果关系的 Stata 代码

*在 VAR(1)中，我们观察到有确凿的证据表明，总体而言，GDP 增长率与消费支出增长率之间存在一定的因果关系。有趣的是，反之亦然(从消费增长到 GDP 增长)并不成立。*

因此，我们有证据表明，GDP 的增长率是消费支出增长率的格兰杰原因。话虽如此，效果(消费支出增长率)必须取决于原因(GDP 增长率)的过去值。此外，原因的过去值(国内生产总值增长率)必须有助于在存在结果的过去值(消费支出增长率)的情况下确定结果的当前值。

**R 中的 VAR**

![](img/f743f3d73c13e372ff597383d76819d9.png)

VAR 模型选择，图片来源:(图片来自作者)，图片工具:R，[链接](https://raw.githubusercontent.com/arimitramaiti/datasets/master/articles/a5_data.csv)到数据

基于信息准则可变滞后阶数选择的 r 码

在 Stata 中，我们对 p 从 1 到 5 的范围迭代 VAR 模型，并逐个收集信息标准度量的值。然而，在 R 中，包“vars”允许我们提及我们想要使用的滞后的数量，它将根据信息标准为我们提供最佳顺序。我们在 ARMA 案例中已经看到，每个工具都不会评估相同的信息标准值，因此在某些度量的建议顺序上可能会有一些差异。在这种情况下，我们看到 AIC 提出了一个 VAR(3)模型，而不是 Stata 提出的 VAR(1)。然而，BIC(也称为 Schwarz IC)和 HQIC 提出了 r 中的 VAR(1)模型。因此，我们现在将 VAR(1)模型拟合到我们的联合收割机系列，并查看其输出。

![](img/62d6c27095419bf511fa3b265a11eeec.png)

VAR(1)的输出，图像源:(来自作者的图像)，图像工具:R，[链接](https://raw.githubusercontent.com/arimitramaiti/datasets/master/articles/a5_data.csv)到数据

风险值模型估计的 r 代码

一旦我们估计了 R 中的模型并简要查看了输出，就该测试模型产生的误差是否具有序列自相关了。这个检验的无效假设是错误没有序列相关性，而另一个假设是错误有序列相关性。决策规则是，如果 p>0.05，那么我们无法拒绝零假设，并得出错误确实没有序列相关性的结论。

![](img/26a3932d1419fe330a6bc3cb85f99dd7.png)

VAR(1)上的序列相关检验，图片来源:(图片来自作者)，图片工具:R，[链接](https://raw.githubusercontent.com/arimitramaiti/datasets/master/articles/a5_data.csv)到数据

用于检查模型残差中自相关的 r 代码

我们现在知道 VAR 模型的误差没有序列相关性。因此，我们不需要重新考虑 VAR 的模型顺序，这意味着我们可以测试因果关系。

![](img/28311abe6676c1e25ca9be38826bbcc5.png)

VAR(1)的 Granger 检验，图片来源:(图片来自作者)，图片工具:R，[链接](https://raw.githubusercontent.com/arimitramaiti/datasets/master/articles/a5_data.csv)到数据

*在 VAR(1)中，我们发现有确凿的证据表明，总体而言，GDP 增长率与消费支出增长率之间存在一定的因果关系。有趣的是，反之亦然(从消费增长到 GDP 增长)并不成立。*

> 注:即时因果关系是为了测试原因的未来值(国内生产总值增长率)是否必须有助于在存在效果的过去值(消费支出增长率)的情况下确定效果的当前值

检验格兰杰因果关系的 r 代码

**Python 中的 VAR**

![](img/6b011a705ef36e335d19d0bf4825ba59.png)

VAR 模型选择，图片来源:(图片来自作者)，图片工具:Python，[链接](https://raw.githubusercontent.com/arimitramaiti/datasets/master/articles/a5_data.csv)到数据

基于信息准则可变滞后阶数选择的 Python 代码

我们得到了与 r 几乎相同的建议。我们现在将 VAR(1)模型拟合到我们的联合收割机系列，并查看其输出。

![](img/a051da6243e7d650b8760fc2295112fa.png)

VAR(1)的输出，图片来源:(图片来自作者)，图片工具:Python，[链接](https://raw.githubusercontent.com/arimitramaiti/datasets/master/articles/a5_data.csv)到数据

用于 VAR 模型估计的 Python 代码

一旦我们在 Python 中估计了模型并简要查看了输出，就该测试模型产生的误差是否具有序列自相关了。这个检验的无效假设是错误没有序列相关性，而另一个假设是错误有序列相关性。决策规则是，如果 p>0.05，那么我们无法拒绝零假设，并得出错误确实没有序列相关性的结论。在 Python 的例子中，我们确实得到了 p 值 0.15，因此我们得出错误没有序列相关性的结论。

用于检查模型残差中自相关的 Python 代码

我们现在知道 VAR 模型的误差没有序列相关性。因此，我们不需要重新考虑 VAR 的模型顺序，这意味着我们可以测试因果关系。

Python 代码检查格兰杰因果关系

*在 VAR(1)中，我们观察到有确凿的证据表明，总体而言，GDP 增长率与消费支出增长率之间存在一定的因果关系。有趣的是，反之亦然(从消费增长到 GDP 增长)并不成立。*

我们也可以使用相同的函数和不同的参数在 Python 中执行即时因果关系测试。

**对 VAR &因果关系的观察**

风险值估计:在估计风险值(1)模型时，所有三种工具的反应相似。我没有发现哪一个能大幅度地战胜另一个。在几乎所有的工具中，选择风险值滞后阶数的基本结构，通过风险值模型估计建议的滞后阶数，从而测试自相关性、因果性和稳定性，似乎是轻量级的。

VAR 模型的输出:后模型估计 Stata 生成的输出或者 R 的汇总结果相对来说比 Python 更详细。例如，Python VAR 输出不提供相应系统方程的模型方程。为了从残差自相关结果、稳定性或因果关系中获得结果，我发现 Python 要求单独输入属性值，这与 Stata 或 r 相比有点麻烦。

这篇文章的目的不是传达数学证明的疏忽，而是更多地关注掌握 ARMA 或 VAR 技术的实践。在有经验的人员的帮助下探索正式的教科书，然后冒险进行实践练习是非常重要的。然而，我的意图是帮助我的读者在他们的计划阶段，我们一直在寻找一个起点，然后迷路。本文有趣的部分是通过不同的工具尝试样本数据，并注意其中的差异或相同之处。有趣的是，在发现这样的问题并决定对问题建模之前，我们的工具箱中应该有不止一个工具。

**参考文献**

*瑞财金融时间序列分析*

*Shumway&Stoffer*时间序列分析及其应用

*分享时间序列分析知识的网络和开放视频平台*

*原始数据来源，经合组织数据库*

*anind ya s .*[*Chakrabarti*](https://sites.google.com/site/homepageasc/home)*教授，经济领域，IIM-艾哈迈达巴德*

> 如果你能看透时间的种子，说哪些谷物会生长，哪些不会，请告诉我。~威廉·莎士比亚