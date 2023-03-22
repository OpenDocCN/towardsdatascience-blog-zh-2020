# 可以在股市中使用数据科学吗？

> 原文：<https://towardsdatascience.com/how-to-use-data-science-on-the-stock-market-9d97a4d348d6?source=collection_archive---------7----------------------->

## 通过关注金融市场来解释数据科学概念

![](img/332587a99b01eb1bebdca97e3e1edfde.png)

[M. B. M.](https://unsplash.com/@m_b_m?utm_source=medium&utm_medium=referral) 在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄的照片

数据科学是当今的热门学科。每个人都是数据。它能做什么以及如何提供帮助。很多时候，数据被表示为数字，这些数字可以表示许多不同的东西。这些数字可能是销售额、库存量、消费者数量，以及最后但绝对不是最不重要的——现金*。*

*这就把我们带到了金融数据，或者更具体地说是股票市场。谈到交易，股票、商品、证券等等都非常相似。我们买进，卖出，持有。这一切都是为了盈利。问题是:*

> *当在股票市场上进行这些交易时，数据科学如何帮助我们？*

> *[在这里注册一个中级会员，可以无限制地访问和支持像我这样的内容！在你的支持下，我赚了一小部分会费。谢谢！](https://marco-santos.medium.com/membership)*

# *股票市场的数据科学概念*

*谈到数据科学，有许多词汇和短语或行话是很多人不知道的。我们是来解决所有这些问题的。本质上，数据科学涉及统计、数学和编程知识。如果你对了解这些概念感兴趣，我会在整篇文章中链接一些资源。*

*现在让我们直接进入我们都想知道的问题——使用数据科学对市场进行分析。通过分析，我们可以确定哪些股票值得投资。我们来解释一些以金融和股市为中心的数据科学概念。*

## *算法*

*![](img/bfd6eb3faa6a224f3152cc8183ae1229.png)*

*[Franck V.](https://unsplash.com/@franckinjapan?utm_source=medium&utm_medium=referral) 在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄的照片*

*在数据科学和编程中，[算法被广泛使用](https://www.investopedia.com/articles/active-trading/101014/basics-algorithmic-trading-concepts-and-examples.asp)。算法是为了执行特定任务的一组规则。你可能听说过算法交易在股票市场很流行。算法交易使用交易算法，这些算法包括一些规则，例如*只在股票当天下跌 5%后买入，或者在股票首次买入时价值下跌 10%后卖出*。*

*这些算法都能够在没有人工干预的情况下运行。他们经常被称为交易机器人，因为他们的交易方法基本上是机械的，他们的交易不带感情。*

*如果你想看一个创建交易算法的例子，那么看看下面的文章:*

*[](https://medium.com/swlh/coding-your-way-to-wall-street-bf21a500376f) [## 我用 Python 编写了一个简单的交易算法，你也可以

### 用 Python 实现算法交易和自动化

medium.com](https://medium.com/swlh/coding-your-way-to-wall-street-bf21a500376f) 

## 培养

![](img/5aef97dc2aa469209691c5ff8ef6ae54.png)

照片由[梅根·霍尔姆斯](https://unsplash.com/@yellowteapot?utm_source=medium&utm_medium=referral)在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 拍摄

这不是你的典型训练。对于数据科学和机器学习，训练涉及使用选定的数据或数据的一部分来“训练”机器学习模型。[整个数据集通常分为两个不同的部分，用于训练和测试](https://developers.google.com/machine-learning/crash-course/training-and-test-sets/splitting-data)。这种分割通常是 ***80/20*** ，整个数据集的 80%用于训练。该数据被称为*训练数据*或*训练集*。为了让机器学习模型准确地做出预测，它们需要从过去的数据(训练集)中学习。

如果我们试图使用机器学习模型来预测精选股票的未来价格，那么我们会给模型提供过去一年左右的股票价格来预测下个月的价格。

## 测试

![](img/4f48be74369571250545afabc2cf3ff2.png)

本·穆林斯在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 拍摄的照片

在用训练集训练了一个模型之后，我们想要知道我们的模型执行得有多好。这就是另外 20%的数据的来源。这些数据通常被称为*测试数据*或*测试集*。为了验证模型的性能，我们将模型的预测与测试集进行比较。

例如，假设我们根据一年的股票价格数据训练一个模型。我们将使用 1 月到 10 月的价格作为我们的训练集，11 月和 12 月将作为我们的测试集(*这是拆分年度数据的一个非常简单的例子，由于季节性等原因通常不应该使用*)。在根据 1 月至 10 月的价格训练我们的模型后，我们将让它预测未来两个月的价格。这些预测将与 11 月 22 日至 12 月的实际价格进行比较。预测和真实数据之间的误差是我们在修改模型时想要减少的。

## 功能和目标

![](img/fcb68a216cd9bd0152744b91d42346c3.png)

照片由 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上的 [NeONBRAND](https://unsplash.com/@neonbrand?utm_source=medium&utm_medium=referral) 拍摄

在数据科学中，数据通常以表格格式显示，如 Excel 表或数据框。这些数据点可以代表任何东西。柱子起着重要的作用。假设我们在一列中有股票价格，在另一列中有市净率、交易量和其他财务数据。

在这种情况下，股票价格将是我们的**目标**。其余的列将是**特征**。在数据科学和统计学中，目标变量被称为因变量。这些特征被称为独立变量。**目标**是我们想要预测的未来值，而**特征**是机器学习模型用来进行这些预测的。* 

# *建模:时间序列*

*![](img/a9797dd43da28c97a8eb1051a5e8353c.png)*

*由[大卫·科恩](https://unsplash.com/@davidcohen_official?utm_source=medium&utm_medium=referral)在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 拍摄的照片*

*数据科学大量使用的一个东西是一个叫做“ [*建模*](https://www.mathsisfun.com/algebra/mathematical-models.html)*的概念。建模通常使用数学方法来考虑过去的行为，以预测未来的结果。当涉及到股票市场的金融数据时，这个模型通常是一个*时间序列模型*。[但什么是时间序列？](https://www.itl.nist.gov/div898/handbook/pmc/section4/pmc4.htm)**

**时间序列是一系列数据，在我们的例子中，这是股票的价格值，按时间顺序排列，可以是每月、每天、每小时甚至每分钟。大多数股票图表和数据是时间序列。因此，在对这些股票价格建模时，数据科学家通常会实施一个时间序列模型。**

**创建时间序列模型涉及使用机器学习或深度学习模型来获取价格数据。然后对这些数据进行分析并拟合到模型中。该模型将使我们能够预测未来一段时间内的股票价格。如果你想看到这一点，那么看看下面的文章，详细介绍了预测比特币价格的机器学习和深度学习方法:**

**[](/predicting-prices-of-bitcoin-with-machine-learning-3e83bb4dd35f) [## 我试图用机器学习来预测比特币的价格

### 利用时间序列模型预测加密货币价格

towardsdatascience.com](/predicting-prices-of-bitcoin-with-machine-learning-3e83bb4dd35f) [](/predicting-bitcoin-prices-with-deep-learning-438bc3cf9a6f) [## 我尝试了深度学习模型来预测比特币价格

### 使用神经网络预测比特币价格

towardsdatascience.com](/predicting-bitcoin-prices-with-deep-learning-438bc3cf9a6f) 

# 建模:分类

![](img/89578331dbc2ff61af68eb2521c77c4c.png)

Shane Aldendorff 在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄的照片

机器学习和数据科学中的另一种模型被称为[分类模型](https://en.wikipedia.org/wiki/Statistical_classification)。使用分类的模型被给予特定的数据点，然后预测或分类这些数据点所代表的内容。

对于股市或股票，我们可以给机器学习模型不同的财务数据，如市盈率，每日交易量，总债务等，以确定股票从根本上来说是否是一个好的投资。该模型可能会根据我们给出的财务状况将该股票分类为买入、持有或卖出。

如果你想看股票分类模型的例子，可以看看下面的文章:

[](https://medium.com/swlh/teaching-a-machine-to-trade-stocks-like-warren-buffett-part-i-445849b208c6) [## 我建立了一个机器学习模型，像沃伦·巴菲特一样交易股票(第一部分)

medium.com](https://medium.com/swlh/teaching-a-machine-to-trade-stocks-like-warren-buffett-part-i-445849b208c6) [](https://medium.com/@marcosan93/teaching-a-machine-to-trade-stocks-like-warren-buffett-part-ii-5d06427b13f7) [## 我建立了一个机器学习模型，像沃伦·巴菲特一样交易股票(第二部分)

medium.com](https://medium.com/@marcosan93/teaching-a-machine-to-trade-stocks-like-warren-buffett-part-ii-5d06427b13f7) 

# 过度拟合和欠拟合

![](img/210edec01c59f57b6496be1a2dfc544d.png)

Photo by [青 晨](https://unsplash.com/@jiangxulei1990?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

在评估一款车型的性能时，[在寻找*恰到好处*的时候，误差有时会达到“*太热*或者“*太冷*”的地步。](https://www.geeksforgeeks.org/underfitting-and-overfitting-in-machine-learning/) **过度拟合**发生在模型预测过于复杂，以至于错过了目标变量和特征之间的关系。**欠拟合**发生在模型与数据拟合不够，预测过于简单的时候。

这些是数据科学家在评估他们的模型时需要注意的问题。用金融术语来说，过度拟合是指模型不能跟上股市趋势，也不能适应未来。欠拟合是指模型基本上开始预测整个股票历史的简单平均价格。换句话说，适应不足和适应过度都会导致糟糕的未来价格预测。

# 关闭

我们涵盖的主题是常见的关键数据科学和机器学习概念。这些主题和概念对于学习数据科学非常重要。还有更多的概念需要讨论。如果你熟悉股票市场，并且对数据科学感兴趣，我们希望这些描述和例子是有用的和可以理解的。

> [**不是中等会员？点击这里支持他们和我！**](https://medium.com/@marco_santos/membership)

***来自《走向数据科学》编辑的提示:*** *虽然我们允许独立作者根据我们的* [*规则和指导方针*](/questions-96667b06af5) *发表文章，但我们并不认可每个作者的贡献。你不应该在没有寻求专业建议的情况下依赖一个作者的作品。详见我们的* [*读者术语*](/readers-terms-b5d780a700a4) *。***