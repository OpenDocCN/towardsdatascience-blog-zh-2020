# 其他 ML 术语:标签泄漏

> 原文：<https://towardsdatascience.com/other-ml-jargons-label-leakage-9e85b22c6fd0?source=collection_archive---------46----------------------->

## 其他 ML 术语

## 机器学习中的标签泄漏及其对模型性能的影响

*你是否被一场完美或近乎完美的模特表演压垮过？那种快乐是不是被那个出卖了你的功能给破坏了？*

简而言之，当你想要预测的信息直接或间接出现在你的训练数据集中时，就会发生**标签泄漏**或**目标泄漏**或简单地说**泄漏**。它导致模型过度表现其泛化误差，并极大地提高了模型的性能，但使模型对任何现实世界的应用都无用。

![](img/a99126254cb34c0b87829b69f62957a4.png)

照片由 [Erlend Ekseth](https://unsplash.com/@er1end?utm_source=medium&utm_medium=referral) 在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄

## **数据泄露是如何发生的**

最简单的例子是用标签本身训练模型。在实践中，在数据收集和准备过程中，不经意间引入了目标变量的间接表示。触发结果的特征和作为目标变量的直接结果的特征是在数据挖掘过程中收集的，因此应该在进行探索性数据分析时手动识别。

数据泄露的主要指标是**“好得令人难以置信”**模型。该模型很可能在预测期间表现不佳，因为它是该模型的次优版本。

数据泄漏不仅仅是因为训练特征是标签的间接表示。也可能是因为来自验证或测试数据的一些信息保留在训练数据中，或者来自未来的一些信息保留在历史记录中。

## **标签泄漏问题示例:**

1.  预测一个人是否会开立一个具有表示此人是否有相关银行账号的特征的银行账户的概率。
2.  在客户流失预测问题中，一个叫做“面试官”的特征是客户是否流失的最佳指标。这些模型表现不佳。原因是这位“面试官”是一名销售人员，只有在客户确认他们打算流失后才会被指派。

## **如何打击标签泄露**

1.  移除它们或添加噪声以引入可以平滑的随机性
2.  使用交叉验证或确保使用验证集在看不见的实例上测试您的模型。

3.使用管道，而不是缩放或转换整个数据集。当基于所提供的整个数据集缩小特征时，例如，使用最小-最大缩放器，然后*应用训练和测试分割，缩放的测试集也包含来自缩放的训练特征的信息，因为使用了整个数据集的最小值和最大值。因此，总是建议使用管道来防止标签泄漏。*

4.根据保留数据测试您的模型并评估性能。就基础设施、时间和资源而言，这是最昂贵的方法，因为整个过程必须使用正确的方法再次进行。

## **结论:**

数据泄漏是最常见的错误之一，在特征工程、处理时间序列、数据集标注以及巧妙地将验证集信息传递给训练集时可能会发生。重要的是，机器学习模型仅暴露于预测时可用的信息。因此，建议仔细挑选特征，在应用转换之前拆分数据，避免在验证集上拟合转换，并使用管道。

**参考:**

1.  [https://www . cs . umb . edu/~ ding/history/470 _ 670 _ fall _ 2011/papers/cs 670 _ Tran _ preferred paper _ leaking data mining . pdf](https://www.cs.umb.edu/~ding/history/470_670_fall_2011/papers/cs670_Tran_PreferredPaper_LeakingInDataMining.pdf)
2.  [https://machine learning mastery . com/data-leakage-machine-learning/](https://machinelearningmastery.com/data-leakage-machine-learning/)
3.  [https://bridged . co/blog/7-用于创建培训数据的最佳实践/](https://bridged.co/blog/7-best-practices-for-creating-training-data/)