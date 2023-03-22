# 新精算师必须了解机器学习

> 原文：<https://towardsdatascience.com/new-actuaries-must-know-about-machine-learning-846dd65647d9?source=collection_archive---------17----------------------->

![](img/a07b3efe273a1397838787052603c14c.png)

在 [Unsplash](https://unsplash.com/s/photos/laptop?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) 上由[斯特凡·斯特凡·契克](https://unsplash.com/@cikstefan?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)拍摄的照片

## 这个职业正在发展

机器学习(ML)是一个不可回避的话题——它也在精算行业引起了轰动。机器学习可以简单地定义为“对计算机编程的科学(和艺术)，以便它们可以从数据中学习”，这是 A. Géron 在他的 2019 年[书](https://www.amazon.co.uk/Hands-Machine-Learning-Scikit-Learn-TensorFlow/dp/1492032646/ref=pd_lpo_14_t_0/257-3076774-3184456?_encoding=UTF8&pd_rd_i=1492032646&pd_rd_r=dfb4d6ef-830b-45b4-a51e-655f75eb07c5&pd_rd_w=2JphL&pd_rd_wg=NXcYK&pf_rd_p=7b8e3b03-1439-4489-abd4-4a138cf4eca6&pf_rd_r=WR7PA4BFNEHM2DCHCMYC&psc=1&refRID=WR7PA4BFNEHM2DCHCMYC)中提供的。事实证明，它是一个无价的工具，拥有海量数据的公司可以利用它来提取洞察力，以改进他们的产品和服务。

精算师的情况也是如此。精算师偶尔会被描述为第一批数据科学家，因为他们长期从事数据和建模技术的工作。这两个研究领域之间显然有很大的重叠——这就是为什么越来越多的精算师发现自己在日常工作中应用机器学习工具。无论是为了释放传统上被忽视的数据的潜力(例如，从文本字段或图像中提取洞察力，而不仅仅是冰冷的硬数字数据)，还是为了利用更强大的技术和算法来利用他们已经拥有的数据，有一点是明确的-机器学习将会继续存在。

# 只有一个问题

尽管自 20 世纪 90 年代以来，一些机器学习算法一直在后台悄悄工作，但似乎这个世界已经迅速变得越来越多地与人工智能有关。商业用例非常丰富，人工智能和人工智能存在于各种产品和服务中(为我们的手机、我们最喜欢的网站和我们每天依赖的工业生产过程提供动力)。但是 ML 在精算领域也有应用:分析死亡率经验以发现新趋势，保险产品定价，预测金融数据…

然而，需要教授给培训中的精算师的材料仍然是静态的。世界在前进，这种教育脱节在加剧。精算大纲看起来有过时的危险。

# 解决方案

也就是到 2019 年。教学大纲被修改了——引入了新鲜的新材料，重要性日益下降的旧材料被淘汰了。瞧，精算师协会(IFoA)科目 CS2 考试的新增加内容之一是机器学习，以及科目 CM1 中密切相关的数据分析主题。[这些变化](https://www.actuaries.org.uk/documents/curriculum-2019-guide?aa)是为了“确保课程是相关的、最新的，并反映在不断变化的全球商业环境中精算师所需的技能、知识和属性。”这听起来是一个足够好的理由——但是这种新材料符合这种说法吗？

根据[新大纲](https://www.actuaries.org.uk/documents/syllabus-2019)，机器学习现在占 CS2 考试的 10%，涵盖五个学习目标，即:

*   ML 的分支及其解决的问题类型
*   关于从数据中学习的高级概念
*   关键技术的描述和示例
*   将 ML 技术应用于简单问题
*   了解其他非精算定量研究人员(数据科学家、统计学家……)的观点

听起来像是对这个主题的全面介绍。让我们来看看核心阅读材料(CS2 考试的可测试材料)中涉及了哪些关键主题。

![](img/10ec925fa73fa68756bb0ea8179b17c0.png)

照片由[女牛仔](https://unsplash.com/@cowomen?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)在 [Unsplash](https://unsplash.com/s/photos/discussion?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) 上拍摄

## **洗钱的定义和范围**

相当合理的是，我们从一些基本的定义和解释开始，关于 ML 对于什么样的问题是有用的。在经典方法可以解决的情况下，开发一个花哨的 ML 算法没有多大意义——同样，如果在数据中没有真正的模式可以检测，ML 也没有什么可以提供的。

在从一些具体的例子(如定向广告、预测选举、预测贷款违约)开始之后，材料变得更加正式——将机器学习过程描述为逼近一个目标函数，该目标函数将一组可测量的变量映射到输出。这些材料不怕揭示算法和问题的数学本质——这对于那些只听说过 ML 的不精确术语的人来说可能是一个受欢迎的变化(或者只是被各种相关的流行词汇反复击中头部)。

## **关键概念概述**

接下来是对该领域关键概念的讨论。特别是:

*   损失函数
*   评估模型的方法(准确度、精确度、召回率、混淆矩阵等)
*   参数和超参数
*   训练集、验证集和测试集
*   过度拟合
*   模型正则化

这些概念被描述得很好——但这仅仅是一个描述。机器学习是那些你需要亲自动手才能真正掌握概念以及它们为什么重要的领域之一。建议使用一些优秀的 ML 在线资源或书籍来查看这些概念的端到端实现，并真正让您的理解更上一层楼。

## **机器学习的分支**

不同类型的最大似然算法之间的本质区别是在这里，涵盖监督，无监督，半监督和强化学习技术。这一部分还处理回归与分类和生成与判别模型，以及额外的理论，例子和精算应用。同样，这是一个很好的主题调查，但你会想做一些进一步的研究，以巩固一些概念。观看视频，阅读书籍和博客——试着为自己建立联系，以便最大限度地利用这些材料。

## **机器学习过程**

ML 项目的一般步骤在注释中给出了合理的范围，这是正确的。机器学习不仅仅是开发、训练和评估模型——在成功部署 ML 解决方案的过程中，还有很多事情要做。在这里我们讨论:

*   收集数据
*   探索性数据分析
*   数据准备
*   模特培训
*   验证和测试
*   提高模型性能
*   文件和再现性的重要性

精算专业的学生将会对他们在课程中遇到的模型的数学驾轻就熟——但是知道这只是 ML 故事中的一章是至关重要的。如果您想继续将您的知识应用到真实世界的业务用例中，您需要对全局有一个坚实的把握。

## **关键算法**

接下来，我们将讨论一些关键算法，特别是:

*   惩罚广义线性模型
*   朴素贝叶斯分类
*   决策树
*   k 均值聚类

这方面有大量的材料，这是一件好事——它建立在前面介绍的一些概念之上。更重要的是，它建立在本课程其他部分所涵盖的一些主题之上。那些熟悉 CS1 中的回归、广义线性模型和主成分分析以及 CS2 中其他地方的比例风险模型的人会很高兴看到这些概念的扩展。

## **其他定量研究者的观点**

最后，我们讨论了一个 ML 从业者的观点如何不同于更传统的研究者和建模者，例如统计学家。本节讨论了团队之间沟通的困难——特别是当不同的术语可能用于本质上相同的概念时，或者当两个团队可能对分析的不同方面感兴趣时。机器学习绝对是一个跨学科的研究领域，因此从业者将经常与其他背景的专业人士交流。“缩小”来讨论一些更普遍但同样重要的问题，这些问题是在现实世界中开发和应用 ML 解决方案时出现的，这很好地完成了材料，因为我们从理论的深度开始，以务实的方式结束。

![](img/c7278b099435a4622dab8ca3d1431ae2.png)

由[路](https://unsplash.com/s/photos/discussion?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)上[车头](https://unsplash.com/@headwayio?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)拍摄

# **那么到底是什么出现在了考试中呢？**

任何一个学生(勇于承认这一点的人)都会知道，更多的努力倾向于放在那些经过实际测试的材料上。严格来说，阅读材料中的所有内容都是可考查的，但这是自从新材料加入后，两次讨论中出现的内容。

【2019 年 4 月 —关于监督与非监督学习的简短问题以及各自的示例。关于模型性能度量的计算和解释的更长的问题。

**2019 年 9 月** —讨论列车验证测试方法的一个更长的问题。评估机器学习在给定的示例场景中是否合适。讨论具有更多参数的更复杂模型的利弊。

# 密码在哪里？

不让学生参与一些更实用的、端到端的 Python 或 R ML 实践项目，这似乎是一个错失的机会——尤其是现在 R 的数据分析和精算统计现在已经成为不止一次而是两次 IFoA 考试的一部分。即使是该学院全新的数据科学[证书](https://www.actuaries.org.uk/learn-and-develop/lifelong-learning/certificate-data-science/certificate-data-science-syllabus)也不需要编写任何代码就能完成。一方面，这可能看起来完全疯狂——毕竟，学习数据科学或机器学习的最佳方式可以说是让自己沉浸其中，并开始摆弄你感兴趣的数据集。

然而，值得注意的是，IFoA 并没有试图创造数据科学家。相反，他们的目标是“通过实例和案例研究，帮助处于职业生涯任何阶段的精算师获得对数据科学工具和技术的基本理解，以及如何将它们应用到精算实践中”(见[此处](https://www.actuaries.org.uk/learn-and-develop/lifelong-learning/certificate-data-science/certificate-data-science-frequently-asked-questions-faqs)，以及关于数据科学证书的其他常见问题)。从本质上来说，你不必成为机器学习的专家，但你需要知道它是什么，以及随着它在行业和整个社会中变得越来越普遍，它是如何使用的。

# **前景**

精算的角色正在演变。IFoA 主席约翰·泰勒在去年主持了该行业内的快速数据科学扩张。很明显，这只是第一步——更新整个职业当然不是一件容易的事情，但不可否认这是正确的做法。数据科学和机器学习技术在 IFoA 学生课程中的正式化是巩固精算师作为专业人士的地位的举措，可以为客户增加真正的价值——随着精算师继续在更远的行业工作，这只会证明越来越有用。

# 更多信息和积分

**Andrew Hetherington** 是英国伦敦的一名见习精算师和数据爱好者。

*   在 [LinkedIn](https://www.linkedin.com/in/andrewmhetherington/) 上和我联系。
*   看看我在 [GitHub](https://github.com/andrewhetherington/python-projects) 上鼓捣什么。

由[斯特凡·斯特凡·契克、](https://unsplash.com/@cikstefan?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) [女同胞](https://unsplash.com/@cowomen?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)和[前进](https://unsplash.com/@headwayio?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)拍摄的照片。