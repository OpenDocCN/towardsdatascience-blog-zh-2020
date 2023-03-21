# 如何从始至终接近一个机器学习项目

> 原文：<https://towardsdatascience.com/how-to-approach-a-machine-learning-project-from-beginning-to-end-96c30a029873?source=collection_archive---------53----------------------->

## 受博士框架启发的循序渐进指南

![](img/016c87c803e64b8733af1ee7149e65eb.png)

Kolleen Gladden 在 [Unsplash](https://unsplash.com/s/photos/finish-line?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) 上拍摄的照片

# 目录

1.  介绍
2.  那么为什么框架很重要呢？
3.  机器学习生命周期

# 介绍

你有项目想法却不知道从何下手吗？或者也许你有一个数据集，想建立一个机器学习模型，但你不确定如何接近它？

在这篇文章中，我将谈论一个概念框架，你可以用它来接近**任何**机器学习项目。这个框架受到了[理论框架](https://www.phdassistance.com/blog/why-is-theoretical-framework-important-in-research/)的启发，与你可能在网上看到的[机器学习生命周期](https://www.javatpoint.com/machine-learning-life-cycle)的所有变体非常相似。

# 那么为什么框架很重要呢？

机器学习的框架很重要，原因有很多:

*   它创建了一个标准化的过程来帮助指导数据分析和建模
*   它允许其他人了解问题是如何处理的，并修正旧的项目
*   它迫使一个人更深入地思考他们试图解决的问题。这包括要测量的变量是什么，限制是什么，以及可能出现的潜在问题。
*   它鼓励人们在工作中更加彻底，增加发现和/或最终结果的合法性。

有了这几点，再来说框架！

# 机器学习生命周期

虽然机器学习生命周期有许多变化，但它们都有四个一般步骤:规划、数据、建模和生产。

## 1.规划

![](img/64f8ae76bedc3d562f800bc1525c727b.png)

格伦·卡斯滕斯-彼得斯在 [Unsplash](https://unsplash.com/s/photos/plan?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) 上拍摄的照片

在你开始**任何**机器学习项目之前，有许多事情你需要计划。在这种情况下，术语“计划”包含许多任务。通过完成这一步，你将对你试图解决的问题有更好的理解，并能对是否继续这个项目做出更明智的决定。

规划包括以下任务:

*   **说出你试图解决的问题。**这似乎是一个简单的步骤，但你会惊讶地发现，人们经常试图为一个并不存在的问题或一个实际上并不存在的问题找到解决方案。
*   **定义你为了解决问题而努力实现的商业目标。**目标应该是可衡量的。“成为世界上最好的公司”不是一个可衡量的目标，但“减少欺诈交易”是一个可衡量的目标。
*   **确定目标变量(如果适用)和你可能想要关注的潜在特征变量**。例如，如果目标是减少欺诈性交易的数量，您很可能需要欺诈性和非欺诈性交易的**标记数据**。您可能还需要像交易时间、帐户 ID 和用户 ID 这样的特性。
*   **考虑任何限制、突发事件和风险**。这包括但不限于资源限制(缺少资金、员工或时间)、基础设施限制(例如缺少训练复杂神经网络的计算能力)和数据限制(非结构化数据、缺少数据点、无法解释的数据等)
*   **建立你的成功指标**。你如何知道你已经成功实现了你的目标？如果你的机器学习模型有 90%的准确率是成功的吗？85%呢？准确性是最适合您的业务问题的衡量标准吗？*查看我的文章* [*数据科学家用来评估他们模型的几个指标*](/how-to-evaluate-your-machine-learning-models-with-python-code-5f8d2d8d945b) *。*

如果你完成了这一步，并对项目有信心，那么你可以进入下一步。

## 2.数据

![](img/834a977dc60d3f426ed39fd932fcc604.png)

由 [Markus Spiske](https://unsplash.com/@markusspiske?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) 在 [Unsplash](https://unsplash.com/s/photos/data?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) 上拍摄的照片

这一步的重点是获取、探索和清理数据。更具体地说，它包括以下任务:

*   **收集并整合您在规划阶段指定的数据**。如果您从多个来源获取数据，您需要将数据合并到一个表中。
*   **争论你的数据。**这需要清理和转换您的数据，使其更适合 EDA 和建模。您需要检查的一些内容包括缺失值、重复数据和噪声。
*   **进行探索性数据分析(EDA)** 。也称为数据探索，这一步基本上已经完成，因此您可以更好地了解您的数据集。*如果你想了解更多关于 EDA 的知识，* [*你可以阅读我的关于进行探索性数据分析的指南*](/an-extensive-guide-to-exploratory-data-analysis-ddd99a03199e) *。*

## 3.建模

![](img/36884615ec4f14403e4e213291c01df1.png)

照片由 [Isaac Smith](https://unsplash.com/@isaacmsmith?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) 在 [Unsplash](https://unsplash.com/s/photos/graph?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) 上拍摄

一旦数据准备就绪，就可以继续构建模型了。这有三个主要步骤:

*   **选择您的模型**:您选择的模型最终取决于您试图解决的问题。例如，无论是回归问题还是分类问题，都需要不同的建模方法。如果你想了解各种机器学习模型，请查看我的文章'[*6 分钟解释所有机器学习模型*](/all-machine-learning-models-explained-in-6-minutes-9fe30ff6776a) *'*
*   **训练你的模型**:一旦你选择了你的模型并分割了你的数据集，你就可以用你的训练数据来训练你的模型。
*   **评估您的模型**:当您觉得您的模型已经完成时，您可以根据您决定的预先确定的成功标准，使用测试数据来评估您的模型。

## 4.生产

![](img/58550550a012efcb652720ead84dac4d.png)

亚历山大·杜默在 [Unsplash](https://unsplash.com/s/photos/production?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) 上拍摄的照片

最后一步是生产你的模型。这一步在课程和网上谈得不多，但对企业来说尤其重要。没有这一步，您可能无法从您构建的模型中获得全部价值。在这一步中，有两个主要问题需要考虑:

*   **模型部署:**部署机器学习模型，称为模型部署，简单地说就是将机器学习模型集成，并将其集成到现有的生产环境中，在那里它可以接受输入并返回输出。
*   **模型监控:**模型监控是机器学习生命周期中的一个操作阶段，在模型部署之后，它需要“监控”您的 ML 模型，例如错误、崩溃和延迟，但最重要的是，确保您的模型保持预定的预期性能水平。

# 感谢阅读！

## 特伦斯·申

*创始人*[*ShinTwin*](https://shintwin.com/)*|我们来连线一下*[*LinkedIn*](https://www.linkedin.com/in/terenceshin/)*|项目组合这里是*[](http://terenceshin.com/)**。**