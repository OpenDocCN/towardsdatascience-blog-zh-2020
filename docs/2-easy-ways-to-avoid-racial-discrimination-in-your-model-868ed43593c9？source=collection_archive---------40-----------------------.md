# 在你的模型中避免种族歧视的两个简单方法

> 原文：<https://towardsdatascience.com/2-easy-ways-to-avoid-racial-discrimination-in-your-model-868ed43593c9?source=collection_archive---------40----------------------->

## 关于机器学习公平性的 Python 教程。

![](img/a32854a0f94aa765845f324101954970.png)

火星·马丁内斯在 Unsplash 拍摄的照片

许多人工智能项目的一个高层次目标是沿着公平和歧视的路线解决算法的伦理含义。

# 我们为什么要关心公平？

众所周知，[算法会助长非法歧视](/machine-learning-and-discrimination-2ed1a8b01038)。

例如，每个投资者都希望将更多的资本投入到高投资回报、低风险的贷款中，这可能并不奇怪。

一个现代的想法是使用机器学习模型，根据关于过去贷款结果的少量已知信息，决定未来的贷款请求给借款人最大的机会全额偿还贷款，同时实现高回报(高利率)的最佳权衡。

有一个问题:该模型是根据历史数据训练的，贫穷的未受教育的人，通常是少数族裔或工作经验较少的人，比一般人更有可能屈服于贷款冲销的历史趋势。

因此，如果我们的模型试图最大化投资回报，它也可能针对白人，特定邮政编码的人，有工作经验的人，事实上剥夺了其余人口公平贷款的机会。

这种行为是违法的。

这里可能有两个失败点:

*   基于对数据的有偏见的探索，我们可能无意中将偏见编码到模型中，
*   数据本身可以对人类决策产生的偏差进行编码。

幸运的是，反对不同的治疗方法很容易。

# 方法 1:不同的治疗检查

虽然没有一个定义被广泛认同为公平的好定义，但我们可以使用**统计奇偶性**来测试关于受保护属性(如种族)的公平假设。这是一个完全不同的治疗检查。

让我们考虑一下申请贷款的被称为 ***P*** 的借款人群体，在该群体中有一个已知的黑人借款人子集 ***B*** 。

我们假设在*上有某种分布 ***D*** ，这代表了我们的模型将选择这些借款人进行评估的概率。*

*我们的模型是一个分类器 *m* : *X* → *0，1* 给借款人贴标签。如果 m = 1，那么这个人将注销他的贷款，如果 m = 0，这个人将全额偿还他的贷款。*

**m* 在 *B* 上相对于 *X* ， *D* 的偏差或**统计不平等性**是随机黑人借款人被标记为 1 的概率与随机非黑人借款人被标记为 1 的概率之间的差异。*

*如果统计上的不平等很小，那么我们可以说我们的模型具有统计上的奇偶性。该指标描述了我们的模型对于受保护子集群体 *B* 的公平程度。*

*该函数的输入是一个二进制值数组(如果样本是黑人申请的贷款，则为 1，否则为 0)和第二个二进制值数组(如果模型预测贷款将被冲销，则为 1，否则为 0)。*

# *方法 2:不同的影响检查*

*完全不同的待遇通常被认为是故意的。另一方面，**不同的影响**是无意的。在美国劳动法中，完全不同的影响指的是“在就业、住房和其他领域中，对具有受保护特征的一组人的不利影响大于对另一组人的不利影响，即使雇主或房东适用的规则在形式上是中立的。”*

*完全不同的影响衡量多数群体和受保护群体获得特定结果的条件概率的比率。法律定义提到了 80%的门槛。*

*if P(White | charge off)/P(Black | charge off)<= 80% then the definition of disparate impact is satisfied.*

*The input of the function is an array of binary values (1 if the sample is a loan requested by a Black person, 0 else) and a second array of binary values (1 if the model predicted that the loan will Charge Off, 0 else).*

*The output is True if the model demonstrates discrimination, False else. The degree of discrimination is also provided between 0 and 1.*

# *Conclusion*

*In this article, we introduced statistical parity as a metric that characterizes the degree of discrimination between groups, where groups are defined concerning some protected class (e.g. Black population). We also covered the 80 percent rule to measure disparate impact.*

*Both methods make an easy starting point to check fairness for a classifier model. An advanced understanding is offered in this [机器学习中的公平性教程](/a-tutorial-on-fairness-in-machine-learning-3ff8ba1040cb)。*

*你可以在我下面的后续文章中了解更多关于人工智能的不确定性:*

*[](/wild-wide-ai-responsible-data-science-16b860e1efe9) [## 野生人工智能:负责任的数据科学

### 谁先开枪——新种族还是人类？

towardsdatascience.com](/wild-wide-ai-responsible-data-science-16b860e1efe9) [](/my-deep-learning-model-says-sorry-i-dont-know-the-answer-that-s-absolutely-ok-50ffa562cb0b) [## 深度学习的不确定性。如何衡量？

### 使用 Keras 对认知和任意不确定性进行贝叶斯估计的实践教程。走向社会…

towardsdatascience.com](/my-deep-learning-model-says-sorry-i-dont-know-the-answer-that-s-absolutely-ok-50ffa562cb0b) [](https://medium.com/an-injustice/money-is-not-black-its-colorful-34fd1ba7e43d) [## 钱不是黑色的，是彩色的

### 红线如何仍然阻止美国黑人今天的财富

medium.com](https://medium.com/an-injustice/money-is-not-black-its-colorful-34fd1ba7e43d)*