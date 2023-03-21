# Yellowbrick 简介:可视化机器学习模型预测的 Python 库

> 原文：<https://towardsdatascience.com/introduction-to-yellowbrick-a-python-library-to-explain-the-prediction-of-your-machine-learning-d63ecee10ecc?source=collection_archive---------10----------------------->

## 您将 f1 分数提高到了 98%！但这是否意味着你的模型表现更好呢？

![](img/0b891b3997ff09328cb9161facd44627.png)

照片由 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上的 [Dainis Graveris](https://unsplash.com/@dainisgraveris?utm_source=medium&utm_medium=referral) 拍摄

# 动机

恭喜你！您刚刚训练了一名模特，并将 f1 分数提高到 98%！但这到底意味着什么呢？f1 分数的提高是否表明你的车型表现更好？

你知道 f1-score 是召回率和精确度之间的调和，但是在正面预测中有多少是错误的呢？负面预测中有多少是错误的？

如果你想优化你的机器学习模型，将 f1 分数提高到 99%，你应该重点提高哪个类别才能提高 1%？

获得这些见解将有助于您了解您的机器学习结果，并知道您应该采取哪些措施来改进模型。理解机器学习的最好方法之一是通过情节。这时黄砖就变得有用了。

# 什么是 Yellowbrick？

[Yellowbrick](https://www.scikit-yb.org/en/latest/) 是一个机器学习可视化库。本质上，Yellowbrick 使您更容易:

*   选择功能
*   调谐超参数
*   解释你的模型的分数
*   可视化文本数据

能够用图分析数据和模型将使您更容易理解您的模型，并找出增加对您的目标有意义的分数的后续步骤。

在本文中，我们将通过一个分类问题来了解 yellowbrick 提供了哪些工具来帮助您解释分类结果。

要安装 Yellowbrick，请键入

```
pip install yellowbrick
```

我们将使用占有率，即用于从温度、湿度、光线和 CO2 进行二元分类(房间占有率)的实验数据。地面实况占用率是从每分钟拍摄的带时间戳的照片中获得的。

# 将数据可视化

## 等级特征

数据中每对要素的相关程度如何？特征的二维排序利用一种每次考虑特征对的排序算法。我们使用皮尔逊相关来评分，以检测共线性关系。

根据数据，湿度与相对湿度密切相关。光线与温度密切相关。这是有意义的，因为这些特性通常是相互关联的。

## 阶级平衡

分类模型面临的最大挑战之一是训练数据中类别的不平衡。对于一个不平衡的类，我们的高 f1 分数可能不是一个好的评估分数，因为分类器可以简单地**猜测所有多数类**来获得高分。

因此，可视化类的分布是很重要的。我们可以利用`ClassBalance`用条形图来可视化班级的分布

似乎被归类为未被占用的数据比被占用的数据要多得多。了解了这一点，我们就可以利用分层抽样、加权等多种技术来处理阶层失衡问题，以获得更丰富的结果。

# 可视化模型的结果

现在我们回到这个问题:98%的 f1 分数到底意味着什么？f1 分数的提高会给你的公司带来更多的利润吗？

Yellowbrick 提供了几个工具，您可以使用它们来可视化分类问题的结果。其中一些你可能听说过或没听说过，对解释你的模型非常有帮助。

## 混淆矩阵

在未被占用的班级中，错误预测的百分比是多少？在被占领的阶级中，错误预测的百分比是多少？困惑矩阵有助于我们回答这个问题

看起来被占领的阶层有更高比例的错误预测；因此，我们可以尝试在被占用的班级中增加正确预测的数量来提高分数。

## ROCAUC

想象一下，我们将 f1 分数提高到 99%，我们怎么知道它实际上更好呢？混淆矩阵可能会有所帮助，但除了比较两个模型在每一类中的正确预测百分比，是否有更简单的方法来比较两个模型的性能？这时 ROC AUC 将会有所帮助。

ROCAUC 图允许用户可视化分类器灵敏度和特异性之间的**权衡。ROC 曲线在 Y 轴上显示真阳性率，在 X 轴上显示假阳性率。**

因此，理想点是图的左上角**:假阳性为零，真阳性为一。曲线下面积(AUC)越高，模型通常越好。**

考虑到我们的 ROC 曲线在左上角附近，我们模型的表现确实不错。如果我们观察到不同的模型或不同的超参数导致 ROC 曲线比我们当前的更靠近左上角，我们可以保证我们的模型的性能实际上提高了。

# 我们如何改进模型？

现在我们了解了模型的性能，我们如何着手改进模型呢？为了改进我们的模型，我们可能需要

*   防止我们的模型欠拟合或过拟合
*   找到对评估者来说最重要的特征

我们将探索 Yellowbrick 提供的工具来帮助我们找出如何改进我们的模型

## 验证曲线

一个模型可以有许多超参数。我们可以选择准确预测训练数据的超参数。寻找最佳超参数的好方法是通过网格搜索选择这些参数的组合。

但是我们怎么知道那些超参数也会准确预测测试数据呢？绘制单个超参数对训练数据和测试数据的影响，对于确定估计量对于某些超参数值是欠拟合还是过拟合非常有用。

验证曲线可以帮助我们找到最佳点，在该点上，低于或高于该超参数的**值将导致**对数据的拟合不足或拟合过度****

从图中我们可以看出，虽然最大深度数越大，训练分数越高，但交叉验证分数也越低。这是有意义的，因为决策树变得越深越适合。

因此，最佳点将是交叉验证分数不降低的地方，即 1。

## 学习曲线

数据越多，模型性能越好吗？并非总是如此，估计量可能对方差引起的误差更敏感。这时候学习曲线是有帮助的。

一条**学习** **曲线**显示了具有不同数量训练样本的估计器的**训练分数**与**交叉验证测试分数**的关系。

从图中，我们可以看到，大约 8700 个训练实例的数量产生了最好的 f1 分数。训练实例的数量越多，f1 分数越低。

## 特征重要性

拥有更多功能并不总是等同于更好的模型。**模型的特征**越多，模型对方差引起的**误差**越敏感。因此，我们希望选择生成有效模型所需的最少特征。

消除特征的常用方法是消除对模型最不重要的特征。然后，我们重新评估该模型在交叉验证过程中是否确实表现得更好。

**特性重要性**非常适合这项任务，因为它可以帮助我们可视化模型特性的**相对重要性**。

似乎光线是决策树分类器最重要的特征，其次是二氧化碳和温度。

考虑到我们的数据中没有太多的特征，我们不会消除湿度。但是，如果我们的模型中有许多特征，我们应该消除那些对模型不重要的特征，以防止由于方差导致的错误。

# 结论

恭喜你！您刚刚学习了如何创建有助于解释模型结果的图。能够理解你的机器学习结果，会让你更容易找到下一步提高其性能的措施。Yellowbrick 的功能比我在这里提到的更多。为了了解更多关于如何使用 Yellowbrick 来解释你的机器学习模型，我鼓励你查看文档[这里](https://www.scikit-yb.org/en/latest/)。

这个回购的源代码可以在[这里](https://github.com/khuyentran1401/Data-science/blob/master/data_science_tools/Yellowbrick.ipynb)找到。

我喜欢写一些基本的数据科学概念，并尝试不同的算法和数据科学工具。你可以在 [LinkedIn](https://www.linkedin.com/in/khuyen-tran-1ab926151/) 和 [Twitter](https://twitter.com/KhuyenTran16) 上和我联系。

如果你想查看我写的所有文章的代码，请点击这里。在 Medium 上关注我，了解我的最新数据科学文章，例如:

[](/how-to-fine-tune-your-machine-learning-models-with-ease-8ca62d1217b1) [## 如何有效地微调你的机器学习模型

### 发现为您的 ML 模型寻找最佳参数非常耗时？用这三招

towardsdatascience.com](/how-to-fine-tune-your-machine-learning-models-with-ease-8ca62d1217b1) [](/find-common-words-in-article-with-python-module-newspaper-and-nltk-8c7d6c75733) [## 用 Python 模块 Newspaper 和 NLTK 查找文章中的常用词

### 使用 newspaper3k 和 NLTK 从报纸中提取信息和发现见解的分步指南

towardsdatascience.com](/find-common-words-in-article-with-python-module-newspaper-and-nltk-8c7d6c75733) [](/sentiment-analysis-of-linkedin-messages-3bb152307f84) [## 使用 Python 和情感分析探索和可视化您的 LinkedIn 网络

### 希望优化您的 LinkedIn 个人资料？为什么不让数据为你服务呢？

towardsdatascience.com](/sentiment-analysis-of-linkedin-messages-3bb152307f84) [](https://medium.com/analytics-vidhya/detailed-tutorials-for-beginners-web-scrap-movie-database-from-multiple-pages-with-beautiful-soup-5836828d23) [## 网刮电影数据库与美丽的汤

### 利用你的数据库预测下一部热门电影

medium.com](https://medium.com/analytics-vidhya/detailed-tutorials-for-beginners-web-scrap-movie-database-from-multiple-pages-with-beautiful-soup-5836828d23) [](/cython-a-speed-up-tool-for-your-python-function-9bab64364bfd) [## cy thon——Python 函数的加速工具

### 当调整你的算法得到小的改进时，你可能想用 Cython 获得额外的速度，一个…

towardsdatascience.com](/cython-a-speed-up-tool-for-your-python-function-9bab64364bfd)