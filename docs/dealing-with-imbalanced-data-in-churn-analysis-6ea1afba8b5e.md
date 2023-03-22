# 流失分析中不平衡数据的处理

> 原文：<https://towardsdatascience.com/dealing-with-imbalanced-data-in-churn-analysis-6ea1afba8b5e?source=collection_archive---------32----------------------->

## 我们的实习团队在为一家 SaaS 公司进行客户保持分析时如何处理不平衡数据。

![](img/9f6dc94733642b9867ae07eb431d7b2a.png)

让我们看看如何在你的[小众类](https://pixabay.com/illustrations/be-unique-change-different-idea-4909103/)中找到模式

在我们的[加州大学戴维斯分校 MSBA 实习项目](/improving-saas-with-data-science-e2cf3720bc90)的客户保持分析部分，我们必须应对的主要挑战之一是我们数据中的不平衡。

今天，我将讲述:

*   什么是不平衡数据？
*   什么时候成问题了？
*   如何处理不平衡的数据？
*   我们应用的解决方案如何影响我们的项目。

# 什么是不平衡数据？

可用于分类问题的数据集有一个标签(例如，客户流失、糖尿病等。)和一组输入(如产品特性、健康指标等。)标签表明了类别，因此您可能会有一个“搅动的”与“活跃的”类型的客户。

严格地说，如果你的数据没有对半分割，那么它就不是“平衡的”。*这很好*。没有完美的数据集！甚至不是分类问题教程很流行的[泰坦尼克](https://www.kaggle.com/c/titanic/data)或者[酒质](https://archive.ics.uci.edu/ml/datasets/wine+quality)数据集。

一些数据集的分类比例为 90/10(即 90%的活跃客户，10%的流失客户)，一些数据集的分类比例为 95/5，甚至 99/1。这将*非常*不平衡。

# 什么时候成问题了？

由于不平衡的数据可能有不同的比率，我们开始想知道什么比率是我们必须评估的实际问题。你的少数民族班有 10%的案例是个问题吗？5%?不到 1%？

这个答案和我每次问有见识的人一个有趣的问题得到的答案是一样的: ***看情况！***

对于如何划分你的类没有标准的界限。根据你正在解决的商业问题的类型和行业的类型，拆分可能会变得更有意义。例如，对于客户保留问题，80/20 的分割可能是可以接受的，但是对于医疗保健问题就不太好了。欺诈检测、在泰坦尼克号沉船中幸存、流失分析或 COVID spread 都可能在用于各自分类问题的类中使用不同的拆分。

在我们的实习项目中，这种不平衡不像 1%的少数阶级(被搅动的顾客)那样严重，但这仍然是我们决定解决的问题。

# 如何处理不平衡的数据？

有许多方法可以处理不平衡的数据。拥有人工智能硕士和博士学位的杰森·布朗利(Jason Brownlee)更详细地讲述了这个主题，以及许多其他关于[机器学习掌握](https://machinelearningmastery.com/)的有趣的机器学习教程。他还推荐这本[书](https://www.amazon.com/dp/1118074629?tag=inspiredalgor-20)“深入研究一些关于处理阶级失衡的学术文献”。

今天，我们将学习 3 种方法:

1.  检查您的指标
2.  平衡您的数据
3.  什么也不做

## 1.检查您的指标

![](img/b5696ce17e73a9e88a01426761abe374.png)

重要的是[深入了解](https://pixabay.com/illustrations/audit-report-verification-magnifier-3737447/)性能指标，理解准确性和精确性之间的区别。

当我们获得分类模型的结果时，我们通常首先查看的是我们所做的所有预测中正确预测的数量。这似乎很直观，因为我们想看看我们的模型表现如何。这是模型的**精确度**:预测总数的总*正确*预测。然而，我们需要记住，只看*和*的准确性是不够的！

> **“准确性不是处理不平衡数据集时使用的衡量标准。我们已经看到这是一种误导。”杰森·布朗利**

在这篇[帖子](https://machinelearningmastery.com/classification-accuracy-is-not-enough-more-performance-measures-you-can-use/)中，布朗利详细解释了为什么仅仅靠准确性不能给你足够的信息来做出决定。在我们的案例中，当进行流失分析并预测将流失的客户时，准确性意味着在所有客户中，我们正确地将多少实际流失的客户标记为“流失”(**真阳性**)以及将多少未流失的客户标记为“未流失”(**真阴性**):

因为有如此多的客户没有流失，高数量的真实否定使得准确率非常高，这将是误导，因为我们希望专注于那些确实离开的客户，并试图发现我们可以做些什么来防止其他客户发生这种情况。

精度在这里派上了用场。精确度是:在所有被标记为“流失”的客户中，我们正确标记了多少？这是被正确标记的客户(**真阳性**)与所有被标记为“流失”的客户(**真阳性**和**假阳性**)的比率。

对于我们来说，在处理数据中的不平衡之前和之后进行回顾，这个指标更有意义。如果你想了解更多关于性能指标的信息，计算机工程专业的学生 Salma Ghoneim 在这篇[文章](/accuracy-recall-precision-f-score-specificity-which-to-optimize-on-867d3f11124)中对准确性、召回率、精确度、F 值和特异性做了出色的解释。

此外，您可能希望检查受试者操作者特征曲线下的面积(AUC ROC 或 AUROC ),以确定真阳性和假阳性之间的关系。另一篇[帖子](/understanding-auc-roc-curve-68b2303cc9c5)很好地解释了这个重要的概念，StatQuest 创始人 Josh Starmer 的这篇[清晰的解释](https://www.youtube.com/watch?v=4jRBRDbJemM)带你一步一步地了解细节。)

## 2.平衡您的数据

![](img/75538e0d3654a24e0fd3cf322971f045.png)

[平衡数据](https://pixabay.com/illustrations/brain-heart-brain-icon-3269655/)可以用不同的方式完成！

*   **欠采样:**欠采样包括从你的过表示类中删除观察值。在这种情况下，这些将是活跃的客户。当你有一大堆数据时(布朗利称之为*“几万或几十万个实例或更多”* [)，这样做是有意义的，但在这个项目中，情况并非如此。](https://machinelearningmastery.com/tactics-to-combat-imbalanced-classes-in-your-machine-learning-dataset/)
*   **过采样:**过采样包括添加来自未被充分代表的类的观察副本。在这种情况下，这些将是搅动的客户。这是一个不错的方法，因为它平衡了数据，但我们仍然可以做得更好。
*   **SMOTE** :这样更好！ [SMOTE](https://machinelearningmastery.com/smote-oversampling-for-imbalanced-classification/) 代表合成少数过采样技术。这种方法创建数据的合成样本，因此 SMOTE 使用距离测量来创建数据点的合成样本，而不是复制观测值，这些数据点不会离您的数据点太远。我们在客户流失分析中使用这个来平衡数据。
*   **ADASYN** : ADASYN 代表自适应合成采样方法，它是一种对你的数据集应用 SMOTE 的改进方法。 [ADASYN](https://machinelearningmastery.com/smote-oversampling-for-imbalanced-classification/) 查看数据的密度分布，以确定将要生成的样本数量。在我们的流失分析中，我们最初使用 SMOTE，但当我们了解到 ADASYN 是一种创建合成样本的改进方法后，我们将我们的方法切换到 ADASYN。

## 3.什么也不做

![](img/9de0c1f22bd471783426540d31094ee3.png)

有时不同的视角比典型的解决方案更好。

这里什么都不做并不意味着我们解决了这个问题！这意味着，你可以考虑**其他方法**，比如收集更多数据(如果可能的话)或者使用不同的机器学习模型，而不是解决你数据中的不平衡。决策树和随机森林是很好的选择。你也可以试试 C4.5，C5.0 或者 CART，就像这个[帖子](https://machinelearningmastery.com/tactics-to-combat-imbalanced-classes-in-your-machine-learning-dataset/)上建议的那样。

美女(还有挑战！)的问题是决定使用哪种方法既能最大限度地提高模型性能，又能揭示数据中的洞察力。

# 我们应用的解决方案如何影响我们的项目。

一旦我们使用 **ADASYN** 平衡数据集，我们就会看到在模型性能方面更好的结果。这使我们能够继续分析和评估不同模型如何回答我们的问题:*哪些关键特征与离开/留在这家 SaaS 公司的客户最相关？*我们能够找到隐藏在回答这个问题的数据中的金块，并帮助我们向这家 SaaS 公司的利益相关者提供可行的建议。

# 大团圆结局？

从那以后我们愉快地继续实习了吗？*没有*。我们在这个项目中遇到了更多的挑战，这让它变得更加有趣。**剧透提醒**:我们*确实*交付了强劲的成果，我们的利益相关者非常高兴，我们目前正在完成[项目](/improving-saas-with-data-science-e2cf3720bc90)和我们的 [MSBA 项目](https://gsm.ucdavis.edu/msba-masters-science-business-analytics)*。*