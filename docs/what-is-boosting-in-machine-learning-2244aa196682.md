# 机器学习中的 Boosting 是什么？

> 原文：<https://towardsdatascience.com/what-is-boosting-in-machine-learning-2244aa196682?source=collection_archive---------13----------------------->

## 机器学习模型的超级英雄

![](img/79197c866855f33fd38b52a6f9eb99ef.png)

# 提升机器学习模型简介

在本帖中，我们将看到对 Boosting 算法的简单而直观的解释:它们是什么，为什么它们如此强大，一些不同的类型，以及它们如何被训练和用于进行预测。

*我们将避开所有沉重的数学负担，去寻找一个清晰、简单、但深入易懂的解释。然而，额外的材料和资源将留在文章的最后，以防你想更深入地研究这个话题。*

*在我们开始之前，这里有一些额外的资源，可以让你的机器学习生涯一飞冲天*

```
*Awesome Machine Learning Resources:- For* ***learning resources*** *go to* [***How to Learn Machine Learning***](https://howtolearnmachinelearning.com/books/machine-learning-books/)*!* *- For* ***professional******resources*** *(jobs, events, skill tests) go to* [***AIgents.co 
— A career community for Data Scientists & Machine Learning Engineers***](https://aigents.co/)
```

[](https://z-ai.medium.com/subscribe) [## 订阅我的专属列表！

### 订阅我的专属列表！获取您喜欢的所有新鲜文章<3! By signing up, you will create a Medium…

z-ai.medium.com](https://z-ai.medium.com/subscribe) 

# What is Boosting in Machine Learning?

**传统上，**构建一个机器学习应用程序包括采用一个**单个学习者**，如逻辑回归器、决策树、支持向量机或人工神经网络，向它提供数据，并通过这些数据教它执行某项任务。

然后 ***集成方法*** 诞生了，它涉及到使用**多个学习器**来单独增强其中任何一个学习器的性能。这些方法可以被描述为使用一组**弱学习者***(*那些平均成绩仅比随机模型*稍好的人)在一起的技术，以便**创建一个更强的、聚合的学习者**。*

*一般来说，集成方法是通过**对个体决策树**的变体进行分组来构建的，我们将在后面看到。*

****助推模式*** 属于这种 ***合奏方式的家族。****

*Boosting 最初被命名为 ***假设 Boosting*** ，它的思想是对用于训练我们弱学习者团队的数据进行过滤或加权，以便每个新学习者给予更多的权重，或者只使用先前学习者分类较差的观察值进行训练。*

*通过这样做**，我们的模型**团队**学会了对各种数据做出准确的预测**，而不仅仅是对最普通或最容易的观察。此外，如果其中一个单独的模型非常不擅长根据某种观察结果做出预测，这也没关系，因为其他 N-1 个模型很可能会弥补这一点。*

*增强**不应与*打包*** 混淆，后者是集成方法的另一个主要家族:在打包中，使用随机性并行训练弱学习器，而在增强中，**顺序训练学习器**，以便能够执行上一段中描述的数据加权/过滤任务。*

*![](img/af40a33a75d06f375c192b801a67da20.png)*

*装袋与增压*

*正如我们从前面的图像中可以看到的，在提升模型中的**可以具有不同的重要性**或权重(用学习者的不同大小来表示)，而在打包中的**所有学习者在最终决策中具有相同的权重**。*

*此外，**在 boosting 中，数据集被加权**(由数据点的不同大小表示)，从而被分类器 *n* 错误分类的观察值在模型 *n + 1* 的训练中被赋予更大的重要性，而**在装袋中，训练样本从整个群体中随机选取**。*

*既然我们已经了解了什么是 boosting，以及它与 bagging 的不同之处，**让我们来看看它为什么如此有效！***

# *为什么助推这么有效？*

*总的来说，**集成方法减少了我们机器学习模型的偏差和方差**。如果你不知道什么是偏差和方差，不要担心，我让你看完了这篇文章。*

*集成方法通过消除对单个估计器的依赖来帮助**提高机器学习模型**的稳定性和性能**。这可以用一个装袋的例子看得很清楚:*随机森林。****

***随机森林顾名思义就是由一组个体** [**决策树**](/decision-trees-explained-3ec41632ceb6) **组成一个森林**。这些单独的树很容易过量输入数据，尽管它们是非常简单和直观的模型，但它们不太擅长预测。*

*然而，**如果我们使用许多树，这些问题就会消失**，因为每棵树都使用不同的数据样本和不同的特征进行训练，从而产生一个整体上更强大、更稳健的模型。*

***对于 boosting，其工作方式与**相同，但在 bagging 中，每个模型都是独立训练的，而在 boosting 中，N 个模型是顺序训练的，考虑到前一个模型的成功，并增加前一个模型误差最大的数据的权重，这使得**后续模型专注于最困难的数据观察。***

*此外，在加权训练样本上表现最好的的**个体模型将变得更强(获得更高的权重)，因此对最终预测具有更大的影响。***

*好吧，这听起来很可爱，但是这些模特实际上是如何训练的？*

# *助推模型是如何训练出来的？*

*训练过程取决于我们使用的增强算法(Adaboost vs LigthGBM vs XGBoost…)，但通常遵循以下模式:*

1.  ***所有数据样本都以相同的权重开始。这些样本用于训练一个单独的模型(比如一个决策树)。***
2.  *计算每个样本的预测误差，增加具有较大误差的那些样本的**权重，以使它们对后续个体模型的训练更重要。***
3.  *根据这个**个体模型**在预测上的表现，它**被赋予一个重要性/权重或发言权**。一个输出非常好的预测的模型将在最终决策中有很大的发言权。*
4.  ***加权数据传递给后验模型，重复**和 2)和 3)。*
5.  *编号 4)被重复，直到我们已经达到一定数量的模型，或者直到误差低于一定阈值。*

*![](img/1a73ea60172027026c80b758a0f3d21f.png)*

*训练助推模型*

*在某些情况下，boosting 模型使用针对每个学习者的**特定固定权重**(称为学习率)进行训练，而不是给每个样本一个单独的权重，模型被训练为试图预测样本的先前预测和目标变量的真实值之间的差异。这种差异就是我们所说的*。**

**稍后，当我们看到不同种类的助推模型时，我们将更多地讨论这一点，然而，家庭的主要特征仍然存在:**对许多个体学习者进行顺序训练，以创建更强大的聚合模型。****

**厉害！现在我们知道了如何训练 Boosting 模型，让我们看看如何使用它们对新数据进行预测。**

# **增压模型如何进行预测？**

**助推模型根据新数据做出预测的方式非常简单。当我们获得一个具有其特征的新观察结果时，它会通过每个单独的模型，让**每个模型做出自己的预测**。**

**然后，考虑到这些模型中每一个的权重，所有这些**预测被缩放和组合**，并给出最终的全局预测。**

**![](img/6ac908dd3b9a59d7d2e451a11cd77590.png)**

**使用助推模型进行预测**

**最后，让我们探索一下**最常见的增压模型的特征。****

# **不同的增压模式**

**![](img/4d1aa004bf775b936329487d838416aa.png)**

*****AdaBoost****的简称，* AdaBoost 通过前面描述的顺序训练、预测和更新误分类样本和相应弱模型的权重的精确过程来工作。**

**它主要与 ***决策树树桩*** 一起使用:只有一个根节点和两个叶节点的决策树，其中只评估数据的一个特征。正如我们所见，通过仅考虑我们数据的一个特征来进行预测，**每个树桩都是一个非常非常弱的模型**。然而，通过组合它们中的许多，可以建立非常鲁棒和精确的集合模型。**

**如果你想了解更多关于 AdaBoost 的信息，可以看看下面 StatQuest 制作的 [**视频。**](https://www.youtube.com/watch?v=LsK-xG1cLYA)**

**![](img/af0f34e3ad788f95de2fca13eb355912.png)**

**与 AdaBoost 非常相似， ***梯度提升机器*** 依次训练弱学习器，添加越来越多的估计器，但是**不是调整数据的权重，而是试图预测先前估计器产生的残差。****

**因此，我们不再有样本权重，所有弱模型都有相同的发言权或重要性。同样，大多数时候，决策树被用作基础预测器，然而，它们不是树桩，而是更大的固定大小的树。GBM 使用一个学习率，并朝着更好的结果迈出一小步，在概念上类似于梯度下降法。**

**同样，如果你想深入了解，可以查看 StatQuest 的[**视频。**](https://www.youtube.com/watch?v=3CC4N4z3GJc)**

**![](img/96d76b8a53d3499db5755e3cbe8beae1.png)**

**是 ***极端梯度提升*** 的缩写，就像在梯度提升中一样，我们使我们的树适合先前树预测的残差，然而，XGBoost 没有使用传统的、固定大小的决策树，而是使用一种不同的树: ***XGBoost 树*** 我们可以称它们为。**

**它通过计算结束于 leave 节点的观察值之间的相似性得分来构建这些树**。此外，XGBoost 允许正则化，减少我们的单个树和整个集合模型可能的过度拟合。****

**最后， **XGBoost 被优化**以推动提升树算法的计算资源的极限，使其在时间和计算方面成为非常**高性能**和**快速的算法**。**

**可以看下面的视频 [***XGBoost Part 1:回归***](https://www.youtube.com/watch?v=OtD8wVaFm6E) ，更深入的了解 XGBoost 到底是怎么回事。**

**![](img/16907f3d8ec10c2ea9f4fc6eb166c054.png)**

****光梯度推进机、**简称 ***LigthGBM*** 、是梯度推进算法改进的又一个转机。它没有像 XGBoost 中那样对决策树使用逐层增长策略，而是使用**逐叶增长策略** y，这使它有机会比其他基于树的算法实现更高的每跳错误减少率。此外，与 XGBoost 相比，LigthGBM 通常更快，特别是在大型数据集上。**

**你可以在它的 [**官方 docu 页面了解更多。**](https://lightgbm.readthedocs.io/en/latest/)**

**[](https://z-ai.medium.com/subscribe) [## 订阅我的专属列表！

### 订阅我的专属列表！获得所有你喜欢的新鲜文章<3! By signing up, you will create a Medium…

z-ai.medium.com](https://z-ai.medium.com/subscribe) 

# Conclusion and additional Resources

That is it! As always, I hope you**享受帖子**，我设法帮助你理解什么是助推，它是如何工作的，以及为什么它如此强大。

如果您想了解有关该主题的更多信息，您可以在这里找到一些附加资源:

*   [*关于假说助推的思考，迈克尔·卡恩斯。*](https://www.cis.upenn.edu/~mkearns/papers/boostnote.pdf)
*   [*关于 MachineLearningMastery 的渐变提升的温和介绍。*](http://gradient-boosting-algorithm-machine-learning/)
*   [*在线学习的决策理论推广及其在 Boosting*](https://pdf.sciencedirectassets.com/272574/1-s2.0-S0022000000X00384/1-s2.0-S002200009791504X/main.pdf?X-Amz-Security-Token=IQoJb3JpZ2luX2VjEFsaCXVzLWVhc3QtMSJHMEUCIGkkd3d0DRMaSvR4%2F5C6dx5z082G5avFo4f2cbH1tuZbAiEAzm8gSH5KVmbTJ%2Bd%2BRUO1LtPR4BuISathYMpThTOERw8qvQMIxP%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARADGgwwNTkwMDM1NDY4NjUiDDZlAg5ielOpeAqGkiqRAwA3ivmRpa64tsFGOi17WCztqopHaujNkkWKWrso6NIH2%2BwMvkqVc8VqTZE96yrr6QrxAKGrsm1FoKOEj6IZscywR8OtoFKVMWR385K98PS7g7HJZCp7IKRybj2nAsb1bn7vhDLmFDYLBJClGX6Kjay5GVugtmhfxGARVHUB9zpNvIA7XUr7UHjYMawL3m543%2FHx2jNtUgQaeu%2F6Yxd4cC20ESRFexyAtWU7RTQM00e%2BB%2BAf6yGT9CEPHFa7gTizAAzdyuQqFVlDOfaMKwwtY4VcRKW%2FKPnqbo612DIuJBi30zHvdS%2BRgvp6qanFhmiojjqV5P40ebivsmDEfu%2FP8Tg%2FJcdCqNwcIRZMjQUC%2BPaVVPp3hx8QXSLJMGJvutqy4pYYVsB6%2FsFFHlI7KN16R6v9ISYLgS%2BokMoz8KDM8Yr%2FTaggqC4z4irOxpyHE7JOtJqPRKwkIb7UNU%2F1eBbF4TmrslIQGlC5kc7EmvA%2B7HG2HqYFLbc5FgcVV4YXNfoiMbEpm6Gw5ZaNFvF0UlosIJAmMI3I7%2FYFOusBuuZWGzOk6ar5bhOWLtV6IOFuXrYVgqMSy1Xz7zhw0TiyGJs9LAC3O8NBXTodzE8Psf4qithLsoCmc4Q3VDZTd5TXfOtVY8cT8XZ9z68nzpB6DpnYAzZaoyIsO11LCjcMnCTNJ6bZVM8DddV5Y72P%2Bb4FlVnskLTGNQRRfSb6FTQ7Iltdi987X7NT%2BZ7TfL%2BDPRrJb4%2FBbu%2BRMOnEOWKcg%2BGUsFlix98UfTc0vIj2jwmn3wtZZxdhVsDksOgOgtZtRNMNGHq1zhOvFRktFq0QmuAvy9%2FeBec2qznGSMV0HW%2BZKJS%2FQHBLbeVQGw%3D%3D&X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Date=20200606T184755Z&X-Amz-SignedHeaders=host&X-Amz-Expires=300&X-Amz-Credential=ASIAQ3PHCVTYQGSOLGFP%2F20200606%2Fus-east-1%2Fs3%2Faws4_request&X-Amz-Signature=db5248ef0c47e23281878cb2a57dd2015555e7ca4f7fd6f38699494fb32bf5d2&hash=bd1980e97c94e6d34df94e21dc1440e18ddaea26a3dbdd850504e7a6fe8ce9bc&host=68042c943591013ac2b2430a89b270f6af2c76d8dfd086a07176afe7c76c2c61&pii=S002200009791504X&tid=spdf-685376e6-64ba-4411-928e-eebd7815a13b&sid=6c4a4f885c56b345609bb3e69ac26d325129gxrqb&type=client) 中的应用
*   [*quanta re:升压 vs 装袋*](https://quantdare.com/what-is-the-difference-between-bagging-and-boosting/)
*   [*最好的机器学习书籍回顾*](https://howtolearnmachinelearning.com/books/machine-learning-books/)

*如果你喜欢这篇文章，请随时在@jaimezorno* *上关注我。还有，你可以看看我其他关于数据科学和机器学习的帖子* [*这里*](https://medium.com/@jaimezornoza?source=post_page---------------------------) *。好好读！*

*如果你想了解更多关于机器学习和人工智能的知识* [***关注我上媒***](https://medium.com/@jaimezornoza) *，敬请关注我的下期帖子！另外，你可以查看* [***这个资源库***](https://howtolearnmachinelearning.com/) *来获得更多关于机器学习和人工智能的资源！*

*   封面图片来自[](https://unsplash.com/)*。*
*   *所有其他图像都是自己制作的。***