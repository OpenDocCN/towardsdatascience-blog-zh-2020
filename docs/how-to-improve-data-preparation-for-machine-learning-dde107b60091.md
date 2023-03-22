# 如何提高机器学习的数据质量？

> 原文：<https://towardsdatascience.com/how-to-improve-data-preparation-for-machine-learning-dde107b60091?source=collection_archive---------29----------------------->

## 建立更好模型的秘密。

![](img/5f2840f36e8f5fc2d39489ea7a5051b1.png)

弗兰基·查马基在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄的照片

The ultimate goal of every data scientist or Machine Learning evangelist is to create a better model with higher predictive accuracy. However, in the pursuit of fine-tuning hyperparameters or improving modeling algorithms, data might actually be the culprit. There is a famous Chinese saying “工欲善其事，必先*利*其器” which literally translates to — To do a good job, an artisan needs the best tools. So if the data are generally of poor quality, regardless of how good a Machine Learning model is, the results will always be subpar at best.

**为什么数据准备如此重要？**

![](img/3c33b28a1c0c0b33a76c95e767fe986f.png)

由 [Austin Distel](https://unsplash.com/@austindistel?utm_source=medium&utm_medium=referral) 在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄的照片

> 众所周知，数据分析过程中的数据准备是一项重要但不重要的任务，超过一半的数据科学家认为清理和组织数据是他们工作中最不愉快的部分。

[数据科学家和专家的多项调查](https://blog.ldodds.com/2020/01/31/do-data-scientists-spend-80-of-their-time-cleaning-data-turns-out-no/#:~:text=And%20an%20August%202014%20New,and%20preparing%20unruly%20digital%20data%E2%80%9C.)确实证实了常见的 80/20 比喻——即 80%的时间陷入准备数据的琐碎家务中，从收集、清理到发现数据的洞察力(数据争论或咀嚼)；只留下 20%用于建模和构建算法的实际分析工作。

因此，数据分析过程的致命弱点实际上是花费在数据准备上的不合理的时间。对于数据科学家来说，这可能是构建有意义模型的生产力的一大障碍。对于企业来说，这可能是对资源的巨大打击，因为对数据分析的投资只能看到剩余五分之一的分配专用于最初的意图。

![](img/2ec3782496374f03e7a36412ba37b97d.png)

听说过 GIGO(垃圾进，垃圾出)？这正是这里发生的事情。数据科学家带着一组给定的数据完成一项任务，并期望构建最佳模型来实现任务的目标。但是在任务进行到一半时，他意识到无论模型有多好，他都不可能获得更好的结果。经过反复研究，他发现数据质量存在问题，并开始清理数据，使其“干净可用”。当数据最终再次拟合时，日期线正在慢慢到来，资源开始枯竭，他只剩下有限的时间来构建和完善他被雇佣的实际模型。

这类似于产品召回。当发现已经上市的产品存在缺陷时，往往为时已晚，不得不召回产品以确保消费者的公共安全。在大多数情况下，缺陷是供应链中使用的组件或成分的质量控制疏忽的结果。例如，[笔记本电脑](https://www.zdnet.com/article/hp-expands-laptop-battery-recall-due-to-fire-and-burn-hazards/)因电池问题被召回，或者[巧克力](https://www.marketingweek.com/major-cadbury-launch-in-disarray-after-dairy-milk-salmonella-recall/)因乳制品污染被召回。无论是物理产品还是数字产品，我们在这里看到的惊人的相似之处是，总是原材料受到指责。

**但是如果数据质量是个问题，为什么不直接改善它呢？**

要回答这个问题，我们首先要了解什么是数据质量。

> 数据质量的定义有两个方面。第一，**独立质量**作为基于固有特性和特征的呈现的数据视图和真实世界中的相同数据之间的一致性的度量；第二，依赖于**的应用**的质量——衡量数据是否符合用户预期目的的需求。

假设你是一名大学招聘人员，试图招聘应届毕业生从事初级工作。你有一份相当准确的联系人名单，但当你浏览名单时，你意识到大多数联系人都是 50 岁以上的人，认为你不适合接近他们。通过应用这个定义，这个场景只实现了完整定义的前半部分——这个列表具有准确性，并且包含了良好的数据。但是它不符合第二个标准——无论多么精确的数据都不适合应用程序。

在本例中，准确性是我们用来评估数据内在质量的维度。还有更多不同的维度。为了让您了解同行评审文献中通常研究哪些维度，在研究了涉及 32 个维度的 15 种不同的数据质量评估方法后，这里有一个直方图显示了前 6 个维度。

![](img/254c9bb2a2cb83ce68f5434694a16428.png)

**数据质量评估的系统方法**

![](img/ec8689ef7200faaa7c7ba545ab59e504.png)

如果你没有计划，你就计划失败。没有良好的规划，一个好的系统方法是不可能成功的。要有一个好的计划，你需要对业务有一个彻底的**理解**，尤其是在与数据质量相关的问题上。在前面的例子中，应该意识到联系人列表虽然是正确的，但是具有数据质量问题，不适用于实现所分配任务的目标。

问题明确后，需要调查的数据质量维度应该是**定义的**。这可以使用经验方法来完成，如在利益相关者中进行调查，以找出哪个维度对数据质量问题最重要。

一套评估步骤也应随之进行。**设计**一种实现方式，以便这些步骤可以将基于所选维度的评估映射到实际数据。例如，以下五项要求可用作示例:

[1]时间范围—决定收集调查数据的时间间隔。

[2]定义—定义如何区分好数据和坏数据的标准。

[3]汇总—如何量化评估数据。

[4]可解释性——评估数据的数学表达式。

[5]阈值—选择一个分界点来评估结果。

一旦评估方法到位，就该动手进行实际评估了。在**评估**之后，可以建立一个报告机制来评估结果。如果数据质量令人满意，则数据适合于进一步分析。否则，必须修改数据，并可能再次收集数据。下图中可以看到一个例子。

![](img/eda35339ac3f396838b42b0118cf7cea.png)

**结论**

对于所有的数据质量问题，没有一个放之四海而皆准的解决方案，正如上面概述的定义，数据质量方面有一半是高度主观的。然而，在数据质量评估的过程中，我们总是可以使用系统的方法来评估和评价数据质量。虽然这种方法很大程度上是客观的，并且相对通用，但是仍然需要一些领域知识。例如在数据质量维度的选择中。对于用例 A，数据的准确性和完整性可能是数据的关键方面，但是对于用例 B，这些方面可能不太重要。