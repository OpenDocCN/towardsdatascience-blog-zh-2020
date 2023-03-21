# 偏见如何扭曲你的预测

> 原文：<https://towardsdatascience.com/how-sampling-biases-might-be-ruining-your-predictions-f9e021d79723?source=collection_archive---------30----------------------->

## 理解数据及其分布是精确模型的关键。

![](img/721bfc638042a9b0499e44ddc0f738ba.png)

图像通过 [Unsplash](https://unsplash.com/@gwendal)

[**抽样偏倚**](https://en.wikipedia.org/wiki/Sampling_bias) 是一种 [**偏倚**](https://en.wikipedia.org/wiki/Bias_(statistics)) 的术语，其中样本的采集方式使得预期[人群](https://en.wikipedia.org/wiki/Statistical_population)中的某些成员比其他人具有更低的 [**抽样概率**](https://en.wikipedia.org/wiki/Sampling_probability) 。这在社会科学或临床研究中最为人所知，例如，当某些人比其他人更喜欢参与研究时。假设你正在做一个心理学或市场研究问题的调查。一些具有特定属性的个人可能对参与本次调查更感兴趣，因为他们可能会直接受到治疗或产品成功的影响。在提供资金的情况下，有需要的个人可能更愿意参与。这两种情况都会将[](https://en.wikipedia.org/wiki/Skewness)**偏态带入到收集的数据中，并可能导致错误的结论( [**选择偏差**](https://en.wikipedia.org/wiki/Selection_bias) )。需要注意的是[统计建模](https://en.wikipedia.org/wiki/Statistical_model)是关于描述 [**概率分布**](https://en.wikipedia.org/wiki/Probability_distribution)；[**机器学习**](https://en.wikipedia.org/wiki/Machine_learning) 是将内容数据压缩为规则的工具箱。由于采样偏差，收集的数据集中的概率分布偏离了人们在野外实际观察到的真实自然分布，并且当从这些数据中开发模型时，由于提取的规则不正确，预计预测性能会很差。**

> **重要的是要意识到统计建模就是描述概率分布。**

**[一个遇到的例子来自一份旨在预测慢性肾病和透析患者死亡率的临床研究出版物](https://academic.oup.com/ndt/advance-article-abstract/doi/10.1093/ndt/gfaa128/5854486?redirectedFrom=fulltext)。数据的一个主要问题是，许多患者因为接受移植而退出研究。结果是病人丢失，他们的结果是未知的；这个过程叫做 [**审查**](https://en.wikipedia.org/wiki/Censoring_(statistics)) 。如果审查系统地发生，即只有特定的受试者丢失( [**流失**](https://en.wikipedia.org/wiki/Selection_bias#Attrition) )，审查会带来偏倚。最后，最终的模型声称，12 岁后死亡的概率显著增加。然而，在这项研究中，老年患者比年轻患者多，因此有理由认为，随着老年人在人口中的数量增加，更多的老年人死亡将被记录在案。在数据科学场景中，大量数据被放入一个罐中进行提取，跟踪所有这些情况变得非常困难。这只有通过使用[模型检验技术](https://scikit-learn.org/stable/inspection.html)、[相关矩阵](https://docs.scipy.org/doc/scipy/reference/generated/scipy.stats.spearmanr.html)和[直方图](https://matplotlib.org/3.1.0/api/_as_gen/matplotlib.pyplot.hist.html)来探索数据和模型才能发现，这也是为什么 [**可解释的人工智能**](https://en.wikipedia.org/wiki/Explainable_artificial_intelligence) 备受期待的原因之一，即模型以与发生在 [**决策树**](https://en.wikipedia.org/wiki/Decision_tree) 、[中的方式解释它们的预测然而，许多高性能的语音和图像识别模型都是基于](https://www.technologyreview.com/2017/04/11/5113/the-dark-secret-at-the-heart-of-ai/)[神经网络](https://en.wikipedia.org/wiki/Artificial_neural_network)的黑盒。**

> **机器学习是把数据的内容压缩成规则的工具箱。**

**偏见甚至可能发生在自然科学领域，但是自然科学家对此并不太了解，因为他们习惯于在严格控制的条件下进行实验，例如在实验室里。有一次，研究人员试图开发化学传感器的校准功能。他们从一个特定的地方(一个汽车交通繁忙的意大利城市的主要街道)收集数据，并开发了一个模型，该模型将几个化学传感器测量的数据作为电压和环境因素(温度和湿度)作为输入映射到参考浓度。然而，他们没有意识到所有的化合物都是相关的(*ρ*0.8)，因为它们源于同一个化学过程——发动机中的燃烧。现在，在重新定位这样的传感器时，预测性能可能会急剧下降，因为分子之间可能存在具有不同关系的其他化学过程，这相当于说它们的分布在其他一些地方可能是不同的( [**光谱偏差**](https://en.wikipedia.org/wiki/Spectrum_bias) )。每当一个模型被训练的分布发生变化时，例如由于空间或时间效应，这个模型的有效性显然就失效了。**

> **由于采样偏差，收集的数据集中的概率分布偏离了人们在野外实际观察到的真实自然分布。**

**[然而，即使采样已经正确执行，也可能存在偏差，这仅仅是因为人类存在偏差](https://www.nytimes.com/2019/11/19/technology/artificial-intelligence-bias.html)。这种偏差是用于模型建立的数据的一部分，越来越多的人意识到人工智能中数据诱导的偏差。例如，尽管是一个真诚的人，但仅仅是一个社会团体的成员就可能增加犯罪的可能性；[仅仅因为一个子群体(如按种族分层)的登记犯罪频率增加，这个子群体中的一个人就已经可疑了](https://www-nytimes-com.cdn.ampproject.org/c/s/www.nytimes.com/2020/06/24/technology/facial-recognition-arrest.amp.html)。即使这样一个变量不是模型的一部分，一个相关的变量(如工资/财富)可以作为一个替代品。人力资源部的另一个例子会影响招聘过程；由于更多的高管是男性，与女性相比，他们中的更多人可能会被贴上“有资格”担任管理角色的标签，而女性可能会发现自己受到一个自动系统的歧视，该系统只是因为在模型开发期间使用了扭曲/有偏见的数据，就为公开的领导职位汇集候选人。[此外，大多数产品由男性开发这一事实可能会对女性产生影响](https://www.theguardian.com/commentisfree/2018/mar/13/women-robots-ai-male-artificial-intelligence-automation)。正在开发嵌入人工智能(如自动决策)的解决方案的研究人员和机构，应该负责保证没有人受到歧视——但目前根本没有监管。**

> **每个模型的好坏取决于它所接受的训练数据。**

**总之，偏见可能在你意识不到的情况下随时发生，每个模型的好坏取决于训练数据的好坏。意识到这种持续存在的风险并理解正在处理的数据的分布，是建立具有有意义预测的良好模型的关键。**

**![](img/a0d6dda9ce34b1cde2c2c6d7052d079e.png)**

**卢克·切瑟在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄的照片**