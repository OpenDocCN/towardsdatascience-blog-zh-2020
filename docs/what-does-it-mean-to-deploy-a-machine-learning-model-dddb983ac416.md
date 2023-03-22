# 部署机器学习模型意味着什么？

> 原文：<https://towardsdatascience.com/what-does-it-mean-to-deploy-a-machine-learning-model-dddb983ac416?source=collection_archive---------7----------------------->

## 对模型部署和各种类型的简单介绍

![](img/4bc368cd3b3d5696ebea133892455292.png)

图片由 [OshDesign](https://pixabay.com/users/OshDesign-4856490/?utm_source=link-attribution&utm_medium=referral&utm_campaign=image&utm_content=3764059) 来自 [Pixabay](https://pixabay.com/?utm_source=link-attribution&utm_medium=referral&utm_campaign=image&utm_content=3764059)

# 目录

1.  [简介](#b56b)
2.  [什么是模型部署](#f1d5)
3.  [一个 ML 系统的高层架构](#0dc8)
4.  [部署您的模型的不同方法](#ed0d)
5.  [确定部署方法时要考虑的因素](#6b6a)

# 介绍

想象一下，你花了几个月的时间创建了一个机器学习(ML)模型，它可以用近乎完美的 f1 分数来确定一项交易是否是欺诈性的。很好，但是你还没完成。理想情况下，您会希望您的模型能够实时确定交易是否具有欺诈性，以便您能够及时阻止交易。这就是**模型部署**的用武之地。

大多数在线资源侧重于机器学习生命周期的前期步骤，如探索性数据分析(EDA)、模型选择和模型评估。然而，模型部署似乎是一个很少讨论的话题——原因是它可能相当复杂。部署是一个与 EDA、模型选择或模型评估完全无关的主题，因此，没有软件工程或 DevOps 背景的人不会很好地理解它。在本文中，您将了解什么是模型部署，模型的高级架构，部署模型的不同方法，以及在确定部署方法时要考虑的因素。

# 什么是模型部署？

部署机器学习模型，称为模型部署，简单地说就是集成机器学习模型，并将其集成到现有的生产环境(1)中，在那里它可以接收输入并返回输出。部署您的模型的目的是使您可以将经过训练的 ML 模型的预测提供给其他人，无论是用户、管理人员还是其他系统。模型部署与 ML 系统架构密切相关，ML 系统架构是指系统内软件组件的排列和交互，以实现预定义的目标(Opeyemi，2019)。

在部署模型之前，您的机器学习模型在准备部署之前需要达到几个标准:

*   可移植性:这是指你的软件从一台机器或系统转移到另一台机器或系统的能力。可移植模型是一种响应时间相对较短的模型，并且可以用最少的努力重写。
*   **可扩展性**:这是指你的模型可以扩展到多大。可伸缩模型是不需要重新设计来保持其性能的模型。

(1)生产环境是一个术语，用于描述软件和其他产品被最终用户实际投入使用的环境(Techopedia，2012)

# ML 系统的高层体系结构

概括地说，ML 系统有四个主要部分:

1.  **数据层**:数据层提供了对模型所需的所有数据源的访问。
2.  **特征层**:特征层负责以透明、可扩展和可用的方式生成特征数据。
3.  **评分层**:评分层将特征转化为预测。Scikit-learn 是最常用的，也是评分的行业标准。
4.  **评估层**:评估层检查两个模型的等价性，可用于监控生产模型。即，它用于监控和比较训练预测与实况交通预测的匹配程度。

# 部署模型的不同方法

部署您的 ML 模型有三种一般方式:一次性、批处理和实时。

## **一次性**

你并不总是需要不断地训练一个机器学习模型来部署它。有时一个模型只需要一次或者周期性的。在这种情况下，可以在需要时简单地对模型进行专门训练，并将其推向生产，直到它恶化到需要修复。

## **批次**

批量训练允许您不断地拥有模型的最新版本。这是一种可扩展的方法，一次获取一个子样本数据，无需在每次更新时使用完整的数据集。如果您在一致的基础上使用模型，这很好，但不一定需要实时预测。

## **实时**

在某些情况下，您需要实时预测，例如，确定交易是否是欺诈性的。这可以通过使用在线机器学习模型来实现，如使用随机梯度下降的线性回归。

# 确定部署方法时要考虑的因素

在决定如何部署一个 ML 模型时，有许多因素和影响需要考虑。这些因素包括以下内容:

*   预测生成的频率以及需要结果的紧迫性
*   如果预测应该单独生成还是批量生成
*   模型的延迟要求、用户拥有的计算能力以及期望的 SLA
*   部署和维护模型所需的运营影响和成本

> 更多类似的文章，请查看 Datatron 的媒体页面或他们的博客 https://blog.datatron.com/ T2 T3

# 感谢阅读！

如果你喜欢我的工作，想支持我…

1.  支持我的最好方式就是在**媒体** [这里](https://medium.com/@terenceshin)关注我。
2.  在 **Twitter** [这里](https://twitter.com/terence_shin)成为第一批关注我的人之一。我会在这里发布很多更新和有趣的东西！
3.  此外，成为第一批订阅我的新 **YouTube 频道** [这里](https://www.youtube.com/channel/UCmy1ox7bo7zsLlDo8pOEEhA?view_as=subscriber)！*目前还没有视频，但即将推出！*
4.  在 **LinkedIn** 上关注我[这里](https://www.linkedin.com/in/terenceshin/)。
5.  在我的**邮箱列表** [这里](https://forms.gle/UGdTom9G6aFGHzPD9)报名。
6.  看看我的网站，[**terenceshin.com**](https://terenceshin.com/)。

# 参考

Techopedia (2019)。生产环境。*技术百科。*[https://www . techopedia . com/definition/8989/production-environment](https://www.techopedia.com/definition/8989/production-environment)

奥佩耶米，巴米格巴德(2019)。去神秘化的机器学习模型的部署(第 1 部分)。*走向数据科学。*

[https://towards data science . com/deployment-of-machine-learning-model-demystalized-part-1-1181d 91815 D2](/deployment-of-machine-learning-model-demystified-part-1-1181d91815d2)

朱利安·凯尔维齐奇(2019 年)。在生产中部署机器学习模型的不同方法概述。 *KDnuggets。*[https://www . kdnugges . com/2019/06/approads-deploying-machine-learning-production . html](https://www.kdnuggets.com/2019/06/approaches-deploying-machine-learning-production.html)

路易吉·帕楚诺(2020)。部署机器学习模型意味着什么？ *KDnuggets。*[https://www . kdnugges . com/2020/02/deploy-machine-learning-model . html](https://www.kdnuggets.com/2020/02/deploy-machine-learning-model.html)