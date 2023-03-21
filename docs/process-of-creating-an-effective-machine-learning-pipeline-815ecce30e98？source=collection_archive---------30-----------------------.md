# 开始 ML 项目前你需要知道的 8 件事。

> 原文：<https://towardsdatascience.com/process-of-creating-an-effective-machine-learning-pipeline-815ecce30e98?source=collection_archive---------30----------------------->

## 从心中的目标开始。斯蒂芬·科维

![](img/16be13d413530241be45d4d7a3219f19.png)

由[弗兰基·查马基](https://unsplash.com/@franki?utm_source=medium&utm_medium=referral)在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄的照片

尝试为你的 ml 项目创建一个管道，这样其他人就可以从你的工作中获益。

> 为什么管道这个词有误导性，因为它意味着单向的，但是让我们看看如何以循环的方式思考 ML 管道。

**注意:-** *本文旨在指导您思考过程(Python 模块)以及创建有效管道所需回答的正确问题。* [*这是一篇更高级的文章，提供了创建生产级管道的示例技术*](/architecting-a-machine-learning-pipeline-a847f094d1c7) *，但是我建议您在开始之前浏览一下。*

> 要以互动的方式了解 ML 管道，请查看[****资源管理器****](https://shrouded-harbor-01593.herokuapp.com) ****。****

> **🌀有一段时间，我认为 ML 管道是这些复杂的设计架构，只能由具有高编码和系统设计技能的软件工程师来完成，但在最近的技术如[亚马逊 SageMaker](https://aws.amazon.com/sagemaker/) 、 [Apache Kafka](https://kafka.apache.org) 、[微软 Azure](https://www.googleadservices.com/pagead/aclk?sa=L&ai=DChcSEwio75_1hcPrAhUD1sAKHXg0CIsYABABGgJpbQ&ohost=www.google.com&cid=CAASE-RoeWWdatn-pEYmivthWbGFS6Q&sig=AOD64_0fZ9yz6zrNsPeVVLgMzeyO0s0Vmw&q&adurl&ved=2ahUKEwiVmpn1hcPrAhUJCc0KHUj9CeQQ0Qx6BAgkEAE) 等的帮助下，这完全不是事实。管道可以以简单且经济的方式实现自动化和维护。🌀**

**管道是一种组织和自动化你的代码的方式，这样不仅你未来的自己，而且其他人也可以很容易地理解和集成它。ML 管道的一些目标是:**

> ****耐* ***新错误*** *。* **⦿实时处理。* *****计算*******工作量*** *平衡* ***。******⦿可扩展****[*消息驱动*](https://blog.sapiensworks.com/post/2013/04/19/Message-Driven-Architecture-Explained-Basics.aspx) *。*********

****在你开始深入研究、建立模型和预测未来之前，问自己这些问题。****

# ****① ML 问题框架:****

> ****你想解决的商业问题是什么？****
> 
> ****你真的需要机器学习的方法来解决这个问题吗？****

****这里有一些提示，让你知道你是否需要一种机器学习方法。****

*   ****有没有你想了解的重复模式？****
*   ****您有理解这些模式所需的数据吗？****
*   ****了解要进行的预测类型，无论是二元/多分类问题，还是需要预测连续值(回归)，如股票价格。****

****现在是时候询问领域期望并测试您的假设了。****

****在这个阶段，你问的问题越多，你的模型就越好。****

*   ****影响你预测的重要特征是什么？****
*   ****它们有重叠的特征吗？****
*   ****如何测试你的模型(这个不像拆分数据那么简单)？****
*   ****可以帮助您理解您正在解决的领域和问题的问题。****

****让我们继续无聊的部分，开始预测未来吧。****

# ****②数据收集与整合:****

****无论你或你的团队在哪里收集数据，都可能存在一些干扰。因此，您应该配备能够帮助您正确清理和集成数据的工具，并且应该能够处理所有类型的数据。****

****虽然这样做并不有趣，但却能产生有趣的结果。因为真实数据不是由数字和分类特征组成的，而是由垃圾和其他特征组成的。****

******便捷的数据清理工具:**[制表](https://pypi.org/project/tabulate/)[Scrubadub](https://scrubadub.readthedocs.io/en/stable/index.html)[ftfy](https://github.com/LuminosoInsight/python-ftfy)[更多](https://mode.com/blog/python-data-cleaning-libraries/)链接****

*****获取您的数据状态:*****

*   ****加载数据`pd.read_{filetype}('filename')`****
*   ****获取数据的形状`data.shape`****
*   ****得到数据的统计信息`data.describe()`****
*   ****了解数据类型和信息`data.info()`、`data.dtypes`****
*   ****了解缺失值的数量`data.isnull().sum()`****

*****呜呜！我的数据看起来惊人，我迫不及待地运行我的模型并得到预测。没那么快！*****

# ****③资料准备:****

****这就是数据科学家的真正技能发挥作用的地方。****

****从你的训练数据中随机抽取一个样本(任何不需要太多时间的大小)，你需要深入挖掘，并且应该能够回答你的所有问题，例如。****

*   ****有什么特点？****
*   ****它符合你的期望吗？****
*   ****有足够的信息做出准确的预测吗？****
*   ****你应该删除一些功能吗？****
*   ****有没有什么特征没有被恰当地表现出来？****
*   ****标签在训练数据中分类正确吗？****
*   ****数据不正确会发生什么？****
*   ****是否有任何缺失值或异常值等。****

****如果你做了适当的功课，你将能够建立你的模型，而不用担心错误的预测。****

****我们如何通过编程来回答这些问题？这将带我们进入下一步👇****

# ****④数据可视化与分析:****

****我喜欢可视化，当你在图表上绘制你的数据时，它揭示了看不见的模式、异常值和我们直到现在才能够看到的东西。直方图和散点图等基本图可以告诉您许多关于数据的信息****

****你需要知道的一些事情是:****

*   ****试图理解标签/目标摘要。****
*   ****检测比例和分布的直方图: [Data.plot.hist()](https://pandas.pydata.org/pandas-docs/stable/reference/api/pandas.DataFrame.plot.hist.html)****
*   ****散点图了解方差/相关性和趋势: [Data.plot.scatter()](https://pandas.pydata.org/pandas-docs/stable/reference/api/pandas.DataFrame.plot.scatter.html)****
*   ****检测异常值的盒式图: [Data.boxplot()](https://pandas.pydata.org/pandas-docs/stable/reference/api/pandas.DataFrame.boxplot.html)****
*   ****通过 [Data.corr()](https://pandas.pydata.org/pandas-docs/stable/reference/api/pandas.DataFrame.corr.html?highlight=corr#pandas.DataFrame.corr) (默认为 Pearson)获取相关系数，并通过 [sns.heatmap(Data.corr())](https://seaborn.pydata.org/generated/seaborn.heatmap.html) 绘制这些值的热图，以便更好地理解特征和目标列之间的关系****
*   ****在 FacetGrid 上绘制分类图: [sns.catplot()](https://seaborn.pydata.org/generated/seaborn.catplot.html)****

****你不想伤害你的模特吧。如果您的数据包含缺失值或异常值是不正确的数据类型，您的模型将受到影响。为了顺利运行您的模型，您需要承担清理它的负担。****

****需要考虑的事项:****

*   ****用均值、众数输入缺失值或使用非缺失值预测缺失值或[深挖](http://www.stat.columbia.edu/~gelman/arm/missing.pdf)、[链接](/6-different-ways-to-compensate-for-missing-values-data-imputation-with-examples-6022d9ca0779)、[链接](https://machinelearningmastery.com/statistical-imputation-for-missing-values-in-machine-learning/)****
*   ****通过移除或[深挖](/ways-to-detect-and-remove-the-outliers-404d16608dba) [链接](https://cxl.com/blog/outliers/)、[链接](https://www.kdnuggets.com/2017/01/3-methods-deal-outliers.html)来处理异常值****
*   ****寻找相关特征等..****

****现在是你期待已久的部分。选择要运行的模型。****

****选择一个模型并不容易，因为模型的范围很广，而且所有的模型都有自己的基本假设。选择符合数据集假设的模型也是数据科学家的工作。****

****[机器学习模型](/all-machine-learning-models-explained-in-6-minutes-9fe30ff6776a)****

****下一步是什么？我现在可以运行我的模型了吗？是的，如果你不想要更好的结果(你不愿意冒任何代价的风险)。下一个部分可以说是机器学习管道中关键且耗时的部分。这只是经验之谈，但我会尽可能多的解释来帮助你开始。准备好！****

****![](img/51f6c383cf22bea5a39bd12290e30621.png)****

****雷切尔·亨宁在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上的照片****

# ****⑤特征选择和工程:****

****特征工程的主要思想来自于提问和你从上面的可视化过程中学到的东西。****

******值得问的问题有:******

*   ****这些特征有意义吗？****
*   ****我如何设计基于可视化的新特性？****
*   ****我可以通过合并旧特征来创建新特征吗？****

******特征工程技巧:******

*   ****将相似的列组合成一列。****
*   ****将分类转换为数字变量，如一个单词中的字母计数或一键编码、LabelEncoder 等。****
*   ****将时间转换成天数或月数。****
*   ****缩放数字特征，以便模型容易找到局部最小值。****
*   ****宁滨把数据列成组。(它对于处理异常值也很有用)****

****深挖——[ML 精通](https://machinelearningmastery.com/discover-feature-engineering-how-to-engineer-features-and-how-to-get-good-at-it/)、[中等](/feature-engineering-for-machine-learning-3a5e293a5114)、 [Kaggle](https://www.kaggle.com/search?q=feature+engineering) 、[精英](https://elitedatascience.com/feature-engineering)、 [Elite2](https://elitedatascience.com/feature-engineering-best-practices)****

****终于……哇哦！终于准备好接受训练了吗？ ***排序*** *:*****

# ****⑥模型训练:****

****训练模型的步骤:****

****1.将数据分成**训练、开发、测试:******

*   ****运行模型时，确保随机化数据以克服偏差。****
*   ****创建一个测试集，该测试集接近地代表训练集(分层测试集，相同的日期范围，等等..)****
*   ****使用交叉验证。****

****2.**偏差**和**方差:******

*   ****拟合不足=低方差和高偏差=简单模型****
*   ****过度拟合=低偏差和高方差=复杂模型****
*   ****[超参数调整](/understanding-hyperparameters-and-its-optimisation-techniques-f0debba07568)帮助您平衡模型偏差和方差。****

****万岁，你做到了……..！****

****该领工资了。运行模型。****

****库: [Mxnet](https://mxnet.apache.org/versions/1.6/) ， [Scikit-Learn](https://scikit-learn.org/stable/) ， [TensorFlow](https://www.tensorflow.org/) ， [PyTorch](https://pytorch.org/) ， [Cloud AutoML](https://cloud.google.com/automl) ， [Pycaret](https://pycaret.org/) ， [Theano](http://deeplearning.net/software/theano/) ， [Keras](https://keras.io/) ， [SciPy](https://www.scipy.org/) ， [NLTK](https://www.nltk.org/) ， [MLlib](https://spark.apache.org/mllib/) ，[xgboos](https://xgboost.readthedocs.io/en/latest/)****

****抱歉！还没完成。有一些细节需要评估和解释，让你脱颖而出，例如:****

# ****⑦模型评估:****

****有不同的指标来评估你的模型，有时你可能必须想出你自己的指标来评估模型的性能。无论如何，这里有一些通用的度量标准，可以帮助你理解模型中哪里出错了，或者模型对于现实世界的准确性如何，或者理解为什么模型会在第一时间工作。[选择正确的指标](https://medium.com/usf-msds/choosing-the-right-metric-for-machine-learning-models-part-1-a99d7d7414e4)****

*****不要只运行单个模型，运行多个回归/分类模型，并根据这里的指标选择最佳模型。*****

******回归度量:******

> ****RMSE，MSE，调整后的[挖深](/regression-an-explanation-of-regression-metrics-and-what-can-go-wrong-a39a9793d914)，[视角](/metrics-to-understand-regression-models-in-plain-english-part-1-c902b2f4156f)****

******分类指标:******

> ****混淆矩阵、精确度、召回率、F1 分数、ROC、AUC [深入挖掘](https://medium.com/@MohammedS/performance-metrics-for-classification-problems-in-machine-learning-part-i-b085d432082b)****

*****结尾:*****

****面对它你已经框定了问题，清理了数据，做了数据可视化，选择了重要的特性，做了特性工程。你一次又一次地通过调整超参数和在开发设备上测试来训练你的模型，还剩下什么？****

# ****⑧预测:****

****`**Model.Predict(Test_Data)**`和部署(这是一个完全独立的主题)****

> ****你的脑海中仍然会有一个疑问。****

******一切都很好，但这一切都只是理论，我该如何实施？******

*****答:正如我之前所说，管道只是一个以系统方式解决问题的思考过程，以便将来易于调试:有一些工具，如*[***Amazon SageMaker***](https://aws.amazon.com/sagemaker/)**这些工具在高层次上处理所有这些步骤，并为您提供额外的功能和工具来组织、监控和跟踪您的模型******

## *****技巧*****

*   *****当您想要测试多个模型时，建议在小数据集上测试模型，而不是在整个数据集上运行。*****
*   *****检查对模型有帮助的重要特征，并继续在这些特征上进行工程设计。*****
*   *****删除与目标变量没有任何关联的特征。*****
*   *****使用最大投票或百分比集成不同的模型以实现更好的预测和随机化。*****
*   *****建模不仅仅是一个一次性的步骤，你需要正确的可执行代码来重新运行你的模型，以适应当前的需求。*****

*****查看这个博客(即将发布),在这里这些步骤被用来解决一个真实世界的例子。*****

*******参考:*******

1.  *****如何构建你的代码*****
2.  *****[Github 给你的必备文件](/i-had-no-idea-how-to-build-a-machine-learning-pipeline-but-heres-what-i-figured-f3a7773513a)*****

> *****👏在链接下方评论你的最佳项目。👏*****