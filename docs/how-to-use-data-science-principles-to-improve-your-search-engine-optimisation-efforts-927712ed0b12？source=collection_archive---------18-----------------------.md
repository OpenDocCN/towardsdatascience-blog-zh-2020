# 如何使用数据科学原理来提高您的搜索引擎优化工作

> 原文：<https://towardsdatascience.com/how-to-use-data-science-principles-to-improve-your-search-engine-optimisation-efforts-927712ed0b12?source=collection_archive---------18----------------------->

## 已经有一些关于这方面的文章了，其中很多文章提到了“是什么”和“为什么”，但是在“如何”方面有一个缺口。以下是我对这个问题的尝试。

![](img/9e2b0b79ae2e2b2ab6335496c2e5a34d.png)

斯蒂芬·菲利普斯-Hostreviews.co.uk 在 Unsplash 上拍摄的照片

搜索引擎优化(SEO)是一门学科，它使用围绕搜索引擎如何工作而获得的知识来构建网站和发布内容，这些内容可以在正确的时间由正确的人在搜索引擎上找到。

有些人说你并不真的需要 SEO，他们采取了一种[梦想的领域](https://en.wikipedia.org/wiki/Field_of_Dreams)‘建立它，他们就会来’的方法。到 2020 年底，SEO 行业的规模预计将达到 800 亿美元。至少有一些人喜欢两面下注。

一个经常被引用的统计数据是，谷歌的排名算法包含超过 [200 个用于网页排名的因素](https://www.impactbnd.com/blog/seo-statistics)，SEO 经常被视为其从业者和搜索引擎之间的“军备竞赛”。随着人们寻找下一个“大事件”并将自己纳入部落([白帽](https://www.wordstream.com/white-hat-seo#:~:text=Generally%2C%20white%20hat%20SEO%20refers,bounds%20as%20defined%20by%20Google.)、[黑帽](https://blog.hubspot.com/marketing/black-hat-seo)和[灰帽](https://www.wordstream.com/gray-hat-seo))。

SEO 活动及其过多的工具产生了大量的数据。作为背景，行业标准的爬行工具[尖叫青蛙](https://www.screamingfrog.co.uk/seo-spider/)有 26 个不同的报告，充满了你甚至认为不重要(但却重要)的事情的网页指标。这对芒格来说是一个很大的数据，从中可以找到有趣的见解。

SEO 思维模式也很好地适应了数据科学的理想，即管理数据，使用统计和算法来获得见解和讲述故事。20 年来，SEO 从业者一直在研究所有这些数据，试图找出下一个最好的方法，并向客户展示价值。

尽管可以访问所有这些数据，在 SEO 中仍然有很多猜测，虽然一些人和机构测试不同的想法以查看什么表现良好，但很多时候它归结为团队中具有最佳跟踪记录和整体经验的人的意见。

在我的职业生涯中，我发现自己经常处于这种位置，这是我现在想要解决的问题，因为我自己已经获得了一些数据科学技能。在这篇文章中，我会给你提供一些资源，让你在 SEO 工作中采用更多的数据导向方法。

## SEO 测试

SEO 中最常被问到的一个问题是“我们已经在客户的网站上实现了这些改变，但是它们有效果吗？”。这经常导致一种想法，如果网站流量上升了，那就是“有效的”，如果流量下降了，那就是“季节性的”。这不是一个严谨的方法。

更好的方法是将一些数学和统计学放在后面，用数据科学的方法进行分析。数据科学概念背后的许多数学和统计学可能很难，但幸运的是，有许多工具可以提供帮助，我想介绍一个由谷歌开发的工具，名为[因果影响](https://opensource.googleblog.com/2014/09/causalimpact-new-open-source-package.html)。

因果影响包原本是一个 [R 包](https://google.github.io/CausalImpact/CausalImpact.html)，然而，有一个 [Python 版本](https://google.github.io/CausalImpact/CausalImpact.html)，如果那是你的毒药，那就是我将在这篇文章中经历的。要使用 Pipenv 在 Python 环境中安装它，请使用以下命令:

```
pipenv install pycausalimpact
```

如果你想了解更多关于 Pipenv 的知识，可以看我写的一篇文章[这里](/is-there-a-way-back-to-windows-after-using-a-mac-for-data-science-ecb7fe329846)，否则，Pip 也可以工作得很好:

```
pip install pycausalimpact
```

## 什么是因果影响？

Causal Impact 是一个库，用于在发生“干预”时对时间序列数据(如网络流量)进行预测，这种“干预”可能是宣传活动、新产品发布或已经到位的 SEO 优化。

您将两个时间序列作为数据提供给工具，一个时间序列可能是经历了干预的网站部分在一段时间内的点击量。另一个时间序列作为对照，在这个例子中，它是没有经历干预的网站的一部分在一段时间内的点击量。

当干预发生时，您还向工具提供数据，它所做的是根据数据训练一个模型，称为[贝叶斯结构时间序列模型](https://en.wikipedia.org/wiki/Bayesian_structural_time_series)。这个模型使用对照组作为基线，试图建立一个预测，如果干预没有发生，干预组会是什么样子。

关于其背后的数学的原始论文在这里，然而，我推荐看下面这个由谷歌的一个家伙制作的视频，它更容易理解:

## 在 Python 中实现因果影响

在如上所述将库安装到您的环境中之后，使用 Python 的因果影响非常简单，正如在下面由[保罗·夏皮罗](https://gist.github.com/pshapiro)写的笔记本中可以看到的:

Python 的因果影响

在引入包含对照组数据、干预组数据的 CSV 并定义前/后周期后，您可以通过调用以下命令来训练模型:

```
ci = CausalImpact(data[data.columns[1:3]], pre_period, post_period)
```

这将训练模型并运行预测。如果您运行该命令:

```
ci.plot()
```

您将会看到一个类似这样的图表:

![](img/f300b4eda742330a8a40b4499d59099b.png)

训练因果影响模型后的输出

这里有三个面板，第一个面板显示了干预组和对没有干预会发生什么的预测。

第二个面板显示了逐点效应，这意味着发生的事情和模型做出的预测之间的差异。

最后一个面板显示了模型预测的干预的累积效果。

另一个需要知道的有用命令是:

```
print(ci.summary('report'))
```

这将打印出一份完整的报告，易于阅读，非常适合汇总并放入客户幻灯片中:

![](img/a88dda9907376d7931163fde53b0305f.png)

因果影响的报告输出

## 选择控制组

建立你的控制组的最好方法是使用一种叫做[分层随机抽样](https://www.investopedia.com/terms/stratified_random_sampling.asp)的方法随机挑选不受干预影响的页面。

Etsy 发表了一篇关于[他们如何使用因果影响进行 SEO 分割测试的文章，他们推荐使用这种方法。随机分层抽样顾名思义就是从人群中随机抽取样本。但是，如果我们抽样的东西以某种方式被分割，我们会尽量保持样本中的比例与这些部分在总体中的比例相同:](https://codeascraft.com/2016/10/25/seo-title-tag-optimization/)

![](img/57ef46751e948d74d05eb2de2c6fdaf5.png)

图片由 [Etsy](https://codeascraft.com/2016/10/25/seo-title-tag-optimization/) 提供

分层抽样分割网页的理想方法是使用会话作为度量。如果您将页面数据作为数据框加载到 Pandas 中，您可以使用 lambda 函数来标记每个页面:

`df["label"] =``df["Sessions"].apply(lambda``x:"Less than 50"``if``x<=50``else``("Less than 100"``if``x<=100``else``("Less than 500"``if``x<=500``else``("Less than 1000"``if``x<=1000``else``("Less than 5000"``if``x<=5000``else`

从那里，您可以在 sklearn 中使用 test_train_split 来构建您的控制和测试组:

`from` `sklearn.model_selection import` `train_test_split`

`X_train, X_test, y_train, y_test =` `train_test_split(selectedPages["URL"],selectedPages["label"], test_size=0.01, stratify=selectedPages["label"])`

注意**分层**已经设置好了，如果你已经有了想要测试的页面列表，那么你的样本页面应该和你想要测试的页面数量相等。此外，样本中的页面越多，模型就越好。如果使用的页面太少，模型就越不精确。

值得注意的是，JC Chouinard 提供了关于如何使用类似于 Etsy 的方法在 Python 中完成所有这些工作的良好背景:

[](https://www.jcchouinard.com/seo-ab-test-python-causalimpact-tag-manager/) [## 使用 Python+causal impact+Tag Manager-JC choui nard 进行 SEO 分割测试

### 在本指南中，我将为您提供用 Python、R、…

www.jcchouinard.com](https://www.jcchouinard.com/seo-ab-test-python-causalimpact-tag-manager/) 

## 结论

有一些不同的用例可以使用这种类型的测试。第一种是使用分割测试来测试正在进行的改进，这类似于上面 [Etsy](https://codeascraft.com/2016/10/25/seo-title-tag-optimization/) 使用的方法。

第二个是测试作为正在进行的工作的一部分在现场进行的改进。这类似于在这篇[文章](https://markedmondson.me/finding-the-roi-of-title-tag-changes-using-googles-causalimpact-r-package)中概述的方法，但是使用这种方法，你需要确保你的样本量足够大，否则你的预测将会非常不准确。所以请记住这一点。

这两种方法都是进行 SEO 测试的有效方法，前者是一种正在进行的优化的 A/B 分割测试，后者是对已经实现的东西的测试。

我希望这能让你对如何将数据科学原理应用到你的 SEO 工作中有所了解。请务必阅读这些有趣的主题，并尝试想出其他方法来使用这个库来验证您的努力。如果你需要这篇文章中使用的 Python 的背景，我推荐这个[课程](https://www.jcchouinard.com/python-for-seo/)。