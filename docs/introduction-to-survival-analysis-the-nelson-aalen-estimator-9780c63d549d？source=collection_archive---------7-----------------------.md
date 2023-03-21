# 生存分析导论:尼尔森-艾伦估计量

> 原文：<https://towardsdatascience.com/introduction-to-survival-analysis-the-nelson-aalen-estimator-9780c63d549d?source=collection_archive---------7----------------------->

![](img/a1922160f6ef55d5442e1ae7369cc0b7.png)

由[约书亚·厄尔](https://unsplash.com/@joshuaearle?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)在 [Unsplash](https://unsplash.com/s/photos/mountain?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) 拍摄的照片

## 了解如何使用非参数方法来估计累积风险函数！

在[之前的文章](/introduction-to-survival-analysis-the-kaplan-meier-estimator-94ec5812a97a)中，我已经描述了 Kaplan-Meier 估计量。简单回顾一下，这是一种逼近真实生存函数的非参数方法。这一次，我将重点关注可视化生存数据集的另一种方法——使用风险函数和 Nelson-Aalen 估计量。同样，我们将使用`lifetimes`库的便利来快速创建 Python 中的情节。

# 1.尼尔森-艾伦估计量

利用卡普兰-迈耶曲线，我们近似计算了生存函数，说明了在某个时间 *t.* 内不发生感兴趣事件(例如，死亡事件)的概率

另一种方法是使用**风险函数**来可视化来自以生存为中心的数据集的综合信息，风险函数可以被解释为受试者在一小段时间间隔内经历感兴趣事件的概率，假设受试者已经生存到所述间隔的开始。有关危险功能的更详细描述，请参见[这篇文章](/introduction-to-survival-analysis-6f7e19c31d96)。

不幸的是，我们不能将生存函数的 Kaplan-Meier 估计转换成风险函数。然而，我们可以使用累积风险函数的另一个非参数估计量——Nelson-Aalen 估计量。简而言之，它用于估计在一定时间内预期事件的累计数量。它之所以是累积的，是因为估计的和比逐点估计要稳定得多。

Nelson-Aalen 估计值可计算如下:

![](img/bb45551106244dcfdd3962abfbe1ea62.png)

其中 d_i 代表在时间 *t* 的感兴趣事件的数量，而 n_i 是处于危险中的观察的数量。所有这些项自然与 Kaplan-Meier 估计公式中的项相似。

Nelson-Aalen 估计量，或者更一般地，将风险函数随时间可视化，并不是一种非常流行的生存分析方法。这是因为——与生存函数相比——对曲线的解释不那么简单和直观。然而，风险函数对于更高级的生存分析方法非常重要，例如 Cox 回归。这就是为什么理解这个概念很重要，我将试着提供一些关于它的见解。我们可以说累积风险函数:

*   衡量到某个时间点为止累积的风险总量 *t.*
*   如果事件是可重复的，则提供我们在数学上预期感兴趣的事件在某个时间段内发生的次数。这可能有点令人困惑，所以为了使陈述简单一点(但不太现实)，你可以把累积风险函数想象成一个人到时间 *t，*为止的预期死亡次数，如果这个人可以在每次死亡后复活而不重置时间的话。正如我所说的，这不太现实，但这同样适用于机器故障等。

最后一个可能有助于获得累积风险函数直觉的概念是**浴缸曲线**，或者更确切地说是它的组成部分。这条曲线代表了许多电子消费产品的生命周期。浴盆曲线的风险率是通过结合以下因素得出的:

*   产品首次推出时的早期“婴儿死亡率”失败率，
*   在产品的设计寿命期间，具有恒定故障率的随机故障率，
*   产品超过预期寿命时的“磨损”故障率。

![](img/aa7f73521386d1da22012308c9d86f82.png)

[来源](https://en.wikipedia.org/wiki/Bathtub_curve#/media/File:Bathtub_curve.svg)

而上图代表的是危险率(不是累计的！)，Nelson–Aalen 估计曲线的形状让我们了解了风险率如何随时间变化。

例如，累积风险函数的凹形表明我们正在处理一种“婴儿死亡率”事件(图中的红色虚线)，其中失败率在早期最高，随着时间的推移而降低。另一方面，累积风险函数的凸形意味着我们正在处理“磨损”类事件(黄色虚线)。

我相信这些理论足以理解累积风险函数的 Nelson-Aalen 估计量。是时候编码了！

# 2.Python 中的示例

为了保持一致，我们继续使用上一篇文章中开始的流行的*电信客户流失*数据集。为了简洁起见，请参考文章中对数据集的描述以及对其应用转换的推理。首先，我们加载所需的库。

然后，我们加载数据:

`lifelines`使计算和绘制 Nelson-Aalen 估计量的过程非常简单，我们只需运行以下几行代码来绘制累积风险函数。

该代码生成以下图形:

![](img/51ad9e8096ac539147ed8a4ef7135924.png)

我认为，基于 Nelson-Aalen 估计值的累积风险函数的形状可能表明，我们正在处理的风险函数类似于浴缸曲线。这是因为我们看到，在开始和接近结束时，变化率都较高，而在客户与公司的生命周期中间，变化率或多或少趋于平缓(稳定在一个恒定的水平)。

我们还可以通过使用拟合的`NelsonAalenFitter`对象的`cumulative_hazard_`方法来轻松访问累积风险函数。

该库提供的另一个有趣的功能是 events 表，它汇总了每个时间点发生的事情。我们可以通过运行`naf.event_table`来获得它，结果如下:

![](img/8ba493166829a5b3b4e13acb6b7ff649.png)

与 Kaplan-Meier 案例类似，我们也将绘制支付方式的每个变量的累积风险函数。由于`lifelines`提供了一种统一的方法来处理用于生存分析的不同工具，代码只需要少量的修改。

对于两种自动支付类别:银行转账和信用卡，累积风险函数的形状非常相似。

![](img/976310b456bb48e7c3fc0f9f6c9e37b5.png)

**注**:在理论介绍中，我们提到用累积风险函数代替风险函数的原因是前者的精度更高。然而，`lifelines`提供了一种通过应用核平滑器从累积函数中导出风险函数的方法。那么，问题出在哪里？为此，我们需要指定带宽参数，由此产生的风险函数的形状高度依赖于所选的值。我将引用作者对这种方法的评论:“*没有明显的方法来选择一个带宽，不同的带宽产生不同的推论，所以这里最好非常小心。我的建议是:坚持累积风险函数。”。*如果你仍然感兴趣，请查看[文档](https://lifelines.readthedocs.io/en/latest/Survival%20analysis%20with%20lifelines.html)。

# 3.结论

在这篇文章中，我试图提供一个估计累积风险函数的介绍和一些关于结果解释的直觉。虽然 Nelson-Aalen 估计值远不如 Kaplan-Meier 生存曲线流行，但在使用更高级的生存分析方法(如 Cox 回归)时，理解它可能会非常有帮助。

您可以在我的 [GitHub](https://github.com/erykml/medium_articles/blob/master/Statistics/nelson_aalen.ipynb) 上找到本文使用的代码。一如既往，我们欢迎任何建设性的反馈。你可以在推特上或者评论里联系我。

如果您对这篇文章感兴趣，您可能也会喜欢本系列中的其他文章:

[](/introduction-to-survival-analysis-6f7e19c31d96) [## 生存分析导论

### 了解生存分析的基本概念，以及可以用来做什么任务！

towardsdatascience.com](/introduction-to-survival-analysis-6f7e19c31d96) [](/introduction-to-survival-analysis-the-kaplan-meier-estimator-94ec5812a97a) [## 生存分析导论:卡普兰-迈耶估计量

### 了解用于生存分析的最流行的技术之一，以及如何用 Python 实现它！

towardsdatascience.com](/introduction-to-survival-analysis-the-kaplan-meier-estimator-94ec5812a97a) [](/level-up-your-kaplan-meier-curves-with-tableau-bc4a10ec6a15) [## 用 Tableau 提升你的 Kaplan-Meier 曲线

### 方便访问整个公司的生存分析！

towardsdatascience.com](/level-up-your-kaplan-meier-curves-with-tableau-bc4a10ec6a15) 

# 4.参考

[1][https://stats . stack exchange . com/questions/60238/直觉累积危险功能生存分析](https://stats.stackexchange.com/questions/60238/intuition-for-cumulative-hazard-function-survival-analysis)