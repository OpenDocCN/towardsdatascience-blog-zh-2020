# 数据科学面试蓝图

> 原文：<https://towardsdatascience.com/the-data-science-interview-blueprint-75d69c92516c?source=collection_archive---------13----------------------->

## 在我准备离开舒适的咨询工作几个月后，Deliveroo 取消了我的数据科学经理职位，我没有太多的安全网可以依靠，也不会失业太久。我将分享帮助我在 FaceBook 上获得两个数据科学家职位的一切，希望它可以帮助你们中的一个人，他们也发现自己处于几个月前我所处的不幸境地。

![](img/1bba884ddadbfe057c1e09ab2922f169.png)

资料来源:Unsplash.com

# 1.组织是关键

我在谷歌(和 DeepMind)、优步、脸书、亚马逊面试过“数据科学家”的职位，这是我观察到的典型面试构建主题:

1.  软件工程
2.  应用统计
3.  机器学习
4.  数据争论、操作和可视化

现在没有人指望你在所有这些方面都有超级研究生水平的能力，但是你需要知道足够多的知识来说服你的面试官，如果他们给你这份工作，你有能力胜任。你需要知道多少取决于工作规范，但是在这个竞争越来越激烈的市场中，没有知识会丢失。

我建议使用[概念](https://www.notion.so/)来组织你的工作准备。它非常通用，使您能够利用[间隔重复](https://www.youtube.com/watch?v=Z-zNHHpXoMM&t=1195s)和[主动回忆](https://www.youtube.com/watch?v=YHJzSbLiQNs)原则来确定学习和部署在数据科学家访谈中反复出现的关键主题。Ali Abdaal 有一个关于记笔记的很棒的教程[,目的是在面试过程中最大化你的学习潜力。](https://www.youtube.com/watch?v=ONG26-2mIHU&t=1231s)

我过去常常一遍又一遍地浏览我的想法笔记，尤其是在我面试之前。这确保了关键主题和定义被加载到我的工作记忆中，当遇到一些问题时，我不会浪费宝贵的时间“嗯嗯嗯”的。

# 2.软件工程

不是所有的数据科学家角色都会问你算法的时间复杂度，但所有这些角色都会要求你写代码。数据科学不是一项工作，而是一系列工作，吸引了各行各业的人才，包括软件工程领域。因此，你是在与那些知道如何编写高效代码的人竞争，我建议你在面试前每天至少花 1-2 个小时练习以下概念:

1.  数组
2.  哈希表
3.  链接列表
4.  基于两点的算法
5.  字符串算法(面试官喜欢这些)
6.  二进位检索
7.  分治算法
8.  排序算法
9.  动态规划
10.  递归

不要把算法背下来。这种方法是没用的，因为面试官可以就算法的任何变体向你提问，你会迷失方向。取而代之的是学习每种算法背后的 ***策略*** 。了解什么是**计算**和**空间**复杂性，并了解为什么它们对构建高效代码如此重要。

LeetCode 是我在面试准备期间最好的朋友，在我看来每月 35 美元是物有所值的。你的面试官只能从这么多算法问题中抽取样本，而这个网站涵盖了大量的算法概念，包括可能或已知在过去问过这些问题的公司。还有一个很棒的社区，他们会详细讨论每个问题，并在我遇到无数“困难”的时候帮助我。如果 35 美元的价格太贵，LeetCode 有一个“精简”版本，有一个更小的题库，就像其他伟大的资源 [HackerRank](https://www.hackerrank.com/) 和 [geeksforgeeks](https://www.geeksforgeeks.org/) 一样。

你应该做的是尝试每一个问题，即使这是一个需要花费很长时间的暴力方法。然后看模型解，试着算出最优策略是什么。然后仔细阅读什么是最优策略，试着理解 ***为什么*** 这是最优策略。问自己一些问题，比如“为什么快速排序的平均时间复杂度是 O(n)？”，为什么两个指针和一个 for 循环比三个 for 循环更有意义？

# 3.应用统计

数据科学隐含着对应用统计学的依赖，这种依赖程度取决于你申请的职位。我们在哪里使用应用统计学？它出现在我们需要组织、解释和从数据中获得洞察力的任何地方。

在我的采访中，我深入研究了以下话题，你可以确信我被问到了每个话题:

1.  描述性统计(我的数据遵循什么分布，分布的模式是什么，期望，方差)
2.  概率论(假设我的数据遵循二项式分布，在 10 次点击事件中观察到 5 个付费客户的概率是多少)
3.  假设检验(构成 A/B 检验、T 检验、方差分析、卡方检验等任何问题的基础)。
4.  回归(我的变量之间的关系是线性的吗，偏差的潜在来源是什么，普通最小二乘解背后的假设是什么)
5.  贝叶斯推理(与频率主义方法相比有什么优点/缺点)

如果你认为这是大量的材料，你并不孤单，我被这类面试中预期的知识量和互联网上大量可以帮助我的信息淹没了。当我为面试复习的时候，我想到了两个无价的资源。

1.  [概率统计简介](https://ocw.mit.edu/courses/mathematics/18-05-introduction-to-probability-and-statistics-spring-2014/)，这是一门开放的课程，涵盖了上面列出的所有内容，包括帮助你测试知识的问题和考试。
2.  [机器学习:贝叶斯和优化视角](https://www.amazon.co.uk/Machine-Learning-Optimization-Perspective-Developers/dp/0128015225)作者塞尔吉奥·泽奥多里德斯。这更像是一部机器学习教材，而不是应用统计学的专门入门，但这里概述的线性代数方法确实有助于理解回归的关键统计概念。

你要记住这些东西的方法不是通过记忆，你需要解决尽可能多的问题。Glassdoor 是面试中常见的应用统计问题的一个很好的解决方案。到目前为止，我参加的最具挑战性的面试是 G-Research，但我真的很喜欢为考试而学习，他们的[样本试卷](https://www.gresearch.co.uk/wp-content/uploads/2019/12/Sample-Quant-Exa.pdf)是测试我在应用统计学修订中取得进展的绝佳资源。

# 4.机器学习

现在，我们来到了野兽，这是我们千禧年时代的流行语，也是一个如此广泛的话题，以至于人们很容易在复习中迷失，以至于想要放弃。

本学习指南的应用统计学部分将为您开始学习机器学习奠定非常非常坚实的基础(基本上只是用花哨的线性代数编写的应用统计学)，但在我的采访中，有一些关键概念反复出现。以下是按主题组织的一组(绝非详尽的)概念:

## 度量—分类

1.  [混淆矩阵、准确度、精确度、召回率、灵敏度](/understanding-confusion-matrix-a9ad42dcfd62)
2.  [F1 得分](/understanding-confusion-matrix-a9ad42dcfd62)
3.  [TPR，TNR，FPR，FNR](https://medium.com/datadriveninvestor/confusion-matric-tpr-fpr-fnr-tnr-precision-recall-f1-score-73efa162a25f)
4.  [第一类和第二类错误](https://www.stat.berkeley.edu/~hhuang/STAT141/Lecture-FDR.pdf)
5.  [AUC-ROC 曲线](/understanding-auc-roc-curve-68b2303cc9c5)

## 度量—回归

1.  [总平方和、解释平方和、残差平方和](https://www.statisticshowto.com/residual-sum-squares)
2.  [决定系数及其调整形式](https://www.statisticshowto.com/adjusted-r2/)
3.  [AIC 和 BIC](https://stats.stackexchange.com/questions/577/is-there-any-reason-to-prefer-the-aic-or-bic-over-the-other)
4.  [RMSE、MSE、MAE、MAPE 的优缺点](https://medium.com/analytics-vidhya/forecast-kpi-rmse-mae-mape-bias-cdc5703d242d)

## [偏差-方差权衡，过拟合/欠拟合](/understanding-the-bias-variance-tradeoff-165e6942b229)

1.  [K 近邻算法和偏差-方差权衡中 K 的选择](https://medium.com/30-days-of-machine-learning/day-3-k-nearest-neighbors-and-bias-variance-tradeoff-75f84d515bdb)
2.  [随机森林](/random-forest-a-powerful-ensemble-learning-algorithm-2bf132ba639d)
3.  [渐近性质](https://en.wikipedia.org/wiki/Asymptotic_theory_(statistics)#:~:text=In%20statistics%2C%20asymptotic%20theory%2C%20or,limit%20as%20n%20%E2%86%92%20%E2%88%9E.)
4.  [维度的诅咒](https://en.wikipedia.org/wiki/Curse_of_dimensionality)

## 型号选择

1.  [K 倍交叉验证](https://machinelearningmastery.com/k-fold-cross-validation/)
2.  [L1 和 L2 正规化](/l1-and-l2-regularization-methods-ce25e7fc831c)
3.  [贝叶斯优化](/tl-dr-gaussian-process-bayesian-optimization-5e66a014b693)

## 抽样

1.  [训练分类模型时处理类别不平衡](https://machinelearningmastery.com/tactics-to-combat-imbalanced-classes-in-your-machine-learning-dataset/)
2.  [SMOTE 用于为代表性不足的类别生成伪观测值](https://machinelearningmastery.com/smote-oversampling-for-imbalanced-classification/)
3.  [自变量中的类不平衡](https://en.wikipedia.org/wiki/Oversampling_and_undersampling_in_data_analysis)
4.  [采样方法](https://www.google.com/search?q=stratified+systematic+random+sampling&rlz=1C5CHFA_enGB897GB897&oq=stratified+systematic+rand&aqs=chrome.0.0j69i57j0l6.5332j0j4&sourceid=chrome&ie=UTF-8)
5.  [采样偏差的来源](https://en.wikipedia.org/wiki/Sampling_bias)
6.  [测量采样误差](https://en.wikipedia.org/wiki/Sampling_error)

## **假设检验**

这确实属于应用统计学，但是我怎么强调学习统计能力的重要性都不为过。这在 A/B 测试中非常重要。

## 回归模型

普通线性回归，它的假设，估计量的推导和局限性在应用统计学部分引用的资料中有相当详细的介绍。您应该熟悉的其他回归模型有:

1.  [用于回归的深度神经网络](/deep-neural-networks-for-regression-problems-81321897ca33)
2.  [随机森林回归](/random-forest-and-its-implementation-71824ced454f)
3.  [XGBoost 回归](/regression-prediction-intervals-with-xgboost-428e0a018b)
4.  [时间序列回归(SARIMA 萨里玛)](/understanding-sarima-955fe217bc77)
5.  [贝叶斯线性回归](/introduction-to-bayesian-linear-regression-e66e60791ea7)
6.  [高斯过程回归](/quick-start-to-gaussian-process-regression-36d838810319)

## 聚类算法

1.  [K-表示](/k-means-a-complete-introduction-1702af9cd8c)
2.  [层次聚类](/hierarchical-clustering-explained-e58d2f936323)
3.  [狄利克雷过程混合物模型](/tl-dr-dirichlet-process-gaussian-mixture-models-made-easy-12b4d492e5f9)

## 分类模型

1.  [逻辑回归(**最重要的一个，修改好** )](/logistic-regression-detailed-overview-46c4da4303bc)
2.  [多元回归](/understanding-multiple-regression-249b16bde83e)
3.  [XGBoost 分类](/a-beginners-guide-to-xgboost-87f5d4c30ed7)
4.  [支持向量机](/support-vector-machines-svm-c9ef22815589)

内容很多，但是如果你的应用统计学基础足够扎实的话，很多内容都是微不足道的。我建议了解至少三种不同的分类/回归/聚类方法的来龙去脉，因为面试官总是(以前也曾经)问“我们还可以使用哪些其他方法，有哪些优点/缺点”？这是世界上机器学习知识的一个小子集，但如果你知道这些重要的例子，面试会流 ***很多*** 更顺利。

# 5.数据处理和可视化

“在应用机器学习算法之前，数据角力和数据清洗的一些步骤是什么”？

我们得到了一个新的数据集，你需要证明的第一件事是你可以执行探索性数据分析(EDA)。在你学习任何东西之前，要意识到在数据争论中有一条成功之路:熊猫。如果使用得当，Pandas IDE 是数据科学家工具箱中最强大的工具。学习如何使用 Pandas 进行数据操作的最佳方式是下载许多许多数据集，并学习如何自信地完成以下一系列任务，就像你早上泡咖啡一样。

我的一次采访包括下载一个数据集，清理它，可视化它，执行特征选择，建立和评估一个模型，所有这些都在 ***一个小时*** 内完成。这是一项非常艰巨的任务，我有时会感到不知所措，但我确保在实际尝试面试之前，我已经练习了几周的管道模型制作，所以我知道如果我迷路了，我可以找到我的路。

**建议:**精通这一切的唯一方法是实践，而 [Kaggle 社区](https://www.kaggle.com/)拥有关于掌握 EDA 和模型管道构建的惊人的知识财富。我会查看一些顶级笔记本上的一些项目。下载一些示例数据集并构建您自己的笔记本，熟悉 Pandas 语法。

## 数据组织

生命中有三件确定的事情:死亡、纳税和被要求[合并数据集](https://stackoverflow.com/questions/53645882/pandas-merging-101)，并对所述合并数据集执行[分组和应用](/how-to-use-the-split-apply-combine-strategy-in-pandas-groupby-29e0eb44b62e)任务。熊猫在这方面非常多才多艺，所以请练习练习。

## 数据剖析

这包括感受数据集的“元”特征，例如数据中数字、分类和日期时间特征的形状和描述。您应该始终寻求解决一系列问题，如“我有多少观察值”、“每个特征的分布是什么样的”、“这些特征意味着什么”。这种早期的概要分析可以帮助您从一开始就拒绝不相关的特性，例如具有数千个级别(名称、唯一标识符)的分类特性，这意味着您和您的机器以后的工作会更少(聪明地工作，而不是努力地工作，或者类似的事情)。

## 数据可视化

在这里，你问自己“我的特征分布是什么样的？”。给你一个建议，如果你没有在学习指南的应用统计学部分学习过**箱线图**，那么这里我强调你要学习它们，因为你需要学习如何直观地识别异常值，我们可以稍后讨论如何处理它们。 [**直方图和核密度估计图**](/histograms-vs-kdes-explained-ed62e7753f12) 在查看每个特征的分布特性时是非常有用的工具。

然后我们可以问“我的特征之间的关系看起来像什么”，在这种情况下，Python 有一个名为 [seaborn](https://seaborn.pydata.org/) 的包，其中包含非常漂亮的工具，如 [pairplot](/visualizing-data-with-pair-plots-in-python-f228cf529166) 和视觉上令人满意的关联图的[热图。](/heatmap-basics-with-pythons-seaborn-fb92ea280a6c)

## 处理空值、语法错误和重复的行/列

缺失值在任何数据集中都是必然的事情，并且是由多种不同的因素引起的，每种因素都以其独特的方式导致偏差。关于如何最好地处理缺失值，有一个完整的研究领域(有一次我参加了一个面试，希望我能非常详细地了解缺失值插补的各种方法)。查看[这本关于处理空值的初级读本](/6-different-ways-to-compensate-for-missing-values-data-imputation-with-examples-6022d9ca0779)。

当数据集包含手动输入的信息(如通过表单)时，通常会出现语法错误。这可能导致我们错误地得出结论，分类特征具有比实际存在的更多的级别，因为“hOt”、“Hot”、“hot/n”都被认为是唯一的级别。查看[这本关于处理脏文本数据的初级读本](/nlp-for-beginners-cleaning-preprocessing-text-data-ae8e306bef0f)。

最后，重复的列对任何人都没有用，重复的行会导致过度表示偏差，所以尽早处理它们是值得的。

## 标准化或规范化

根据您正在处理的数据集和您决定使用的机器学习方法，对您的数据进行[标准化或规范化](/normalization-vs-standardization-quantitative-analysis-a91e8a79cebf)可能会很有用，这样不同变量的不同尺度就不会对您的模型的性能产生负面影响。

这里有很多东西要经历，但老实说，帮助我的不是“记住一切”的心态，而是**尽可能多地学习**灌输给我的信心。在公式“点击”之前，我肯定失败了很多次面试，我意识到所有这些东西都不是只有精英才能掌握的深奥概念，它们只是你用来建立令人难以置信的模型并从数据中获得洞察力的工具。

祝你们求职好运，如果你们需要任何帮助，请让我知道，我会尽可能回答邮件/问题。