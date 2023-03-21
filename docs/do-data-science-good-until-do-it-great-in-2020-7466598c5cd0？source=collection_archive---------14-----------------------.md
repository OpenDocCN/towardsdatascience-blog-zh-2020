# 2020 年如何管理数据科学项目

> 原文：<https://towardsdatascience.com/do-data-science-good-until-do-it-great-in-2020-7466598c5cd0?source=collection_archive---------14----------------------->

## 成功 DS 项目管理的 4 个要素

![](img/a5123edce174cb27da6e7772bd16ae5f.png)

**************************************************************************************************************************************

> 祝新年快乐！
> 
> 【2020 年 5 月又是成就和繁荣的一年。

**************************************************************************************************************************************

# 反光

学数据科学和做数据科学是两码事。在学校，统计学教授教我们如何曲线拟合“**完美的**”机器学习模型，但不教我们如何务实，如何管理项目，以及如何倾听客户的需求。

正规教育有时会让我们失望，以一种可怕的方式让我们失望。

当与其他更有经验的数据科学家交谈时，对初级数据实践者最常见的抱怨是缺乏行业工作能力。

知道 DS 和实际做 DS 之间存在着巨大的差距。在这篇文章中，我想分享我从管理项目评估项目中学到的 4 个非技术成功要素。

# 背景

作为一名博士生，我接受了 5 年多的研究设计和数据科学培训。几年前，我在一家教育公司从事统计咨询工作。

这家公司为弱势学生提供社交礼仪和职业发展方面的培训。它以非政府组织的方式运作，严重依赖公众捐款和政府拨款。尽管有着很棒的节目，该公司发现自己陷入了财务困境，因为他们无法量化社会影响。他们的资助申请已经几次被公司和基金会拒绝。

顺便说一句，社会效果这种无形的概念总是很难量化。项目评估人员需要精通数据，并具备丰富的领域知识。

> 我的工作是想出一个有效的研究设计来量化他们的好工作。
> 
> 从零开始。嗯，算是吧。

这说起来容易做起来难。当我第一次开始的时候，我惊讶地发现数据团队是如此的毫无准备和混乱。缺乏准备使得他们无法重新设计工作流程。举一个例子，作为教育培训提供商，他们甚至没有**计划目标**和**学习成果陈述(LOS)** 。

对于那些不太熟悉这个领域的人来说，这两个要素是项目评估的基本要素。没有 LOS，我们就无法证明该计划是否达到了目标。

![](img/09e8dcbb169bdbd6d75e044331b6b66a.png)

照片由 [Neven Krcmarek](https://unsplash.com/@nevenkrcmarek?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) 在 [Unsplash](https://unsplash.com/s/photos/ingredient?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) 上拍摄

## 成分 1:真正理解商业问题

首先要做的事。我与领导层坐下来，制定他们的具体目标。为了量化结果，我开发了一套新的度量标准，包括三个维度:*知识、态度和行为*。

*   **知识 T3。** 什么是职业操守？其中一个关键是教育学生什么是职业精神。
*   ***态度*** *。*什么是专业设置中的正确态度？这与 LOS 一致。
*   ***行为。*** 你如何表现得很专业？而且，这与 LOS 是一致的。

这个过程大约需要 1-1.5 周。

有了这些目标，下一步就是收集数据。我要求他们提供所有的数据，任何调查等等。此外，我要求进入他们的数据库和以前收到的不成功的资助申请的拒绝信。

这是关键的一步。拒绝信提供了失败的原因，并为我们指明了未来的方向。收集他们已经做了什么的信息肯定有助于我们理解还应该做什么。没有必要重新发明轮子。

不出所料，拒绝的最常见原因是缺乏可量化的结果。该公司提供了一些描述性数据，但没有推断性数据。描述性数据表明学生在小组中的表现，而推断性数据表明该计划是否有所不同。

在一系列的帖子中，我介绍了推导因果推理的实验方法([因果 VS 关联](/the-turf-war-between-causality-and-correlation-in-data-science-which-one-is-more-important-9256f609ab92)、[自然实验](/research-note-what-are-natural-experiments-methods-approaches-and-applications-ad84e0365c75)、 [RDD](/the-crown-jewel-of-causal-inference-regression-discontinuity-design-rdd-bad37a68e786) 、 [ITS](/what-is-the-strongest-quasi-experimental-method-interrupted-time-series-period-f59fe5b00b31) 、[做了](/does-minimum-wage-decrease-employment-a-difference-in-differences-approach-cb208ed07327))。

## 成分 2:更简单的解决方案是更好的解决方案

初级数据科学家喜欢陷入这个常见的陷阱:更喜欢花哨、复杂的解决方案，以显示他们能做什么，而不是可以解决问题的简单解决方案。

> 简单回归的 10 层深度学习模型？
> 
> 真的吗？？？

这在项目评估领域至关重要。我们希望通过我们的伟大工作给利益相关者和基金会留下深刻印象。用更难解释的模型来混淆它们是我最不愿意做的事。

考虑到这一点，我采用了一个简单的准 A/B 测试，并包含了一些呈现结果的常用方法。

*   **平均分数。**比较治疗组和对照组的平均值。
*   **双样本 T 检验。**确保实验组具有可比性。
*   **成熟度检查**。作为一个稳健的检验，我消除了成熟效应，因为对照组的参与者在实验期间保持不变。
*   **配对 T 检验**。数据分析显示，参加该计划后，有适度的、统计上显著的增长。

![](img/1313dcd7b9879ff5e7c8f1d1958f97cf.png)

Jason Rosewell 在 [Unsplash](https://unsplash.com/s/photos/communication?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) 上拍摄的照片

## 成分三:沟通是王道

在整个过程中，最具挑战性的部分是将模糊的业务问题转化为可测试的 DS 问题。这真的不容易。我和团队坐下来谈了几次，交换了关于什么是可测试的，什么是不可测试的想法。这个过程持续了几个星期，我们不得不不不情愿地放弃一些想法，专注于少数可行的想法。

在集思广益的过程中，我的非技术同事帮助我更好地了解公司的结构和独特的需求。作为回报，我帮助解释什么是可行的统计测试。伟大的想法不断涌现，对话持续很长时间。

作为团队中的数据员，我经常带着我的耳朵，而不是我的嘴巴，在说出来之前，倾听并了解他们的需求和关注。

正如所说，成功的项目管理的 20%来自好的模型，80%来自非技术部分。

同意！

![](img/9788995666959becb2d0655baeef339a.png)

照片由 [Aron 视觉效果](https://unsplash.com/@aronvisuals?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)在 [Unsplash](https://unsplash.com/s/photos/time?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) 上拍摄

## 成分 4:时间管理

这是最后一个非技术成分，但却是必不可少的。为了让事情继续下去，我们的时间表很紧，必须在大约 2 个月的时间内提供可行的结果。最大值。

> 在一年的时间里交付完美的结果？
> 
> 当然，但是公司会破产。

在短时间内，我重新设计了整个工作流程，使其更加实用，包括参与者招募、模型选择、数据分析、稳健测试和起草项目报告。

*   **采样**。我建议不要随机抽样，而是接触潜在的学校，挑选同意参与的学校。另一方面，只要我们小心概括的范围，项目评估并不那么依赖随机抽样。
*   **案例选择**。在选定的学校中，我们选择了两个在多方面彼此相似的班级，如性别比例、种族和家庭年收入。一个是对照组，一个是治疗组。
*   **型号选择**。没有花哨的模型，只是两样本配对 T 检验。

![](img/c39690bca5e21a6e2c92b15c588fbc59.png)

照片由 [Clem Onojeghuo](https://unsplash.com/@clemono2?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) 在 [Unsplash](https://unsplash.com/s/photos/takeaway?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) 上拍摄

# 结论和要点

总的来说，我对结果很满意，享受做这件事的每一秒钟。学习商业很有趣，与各种利益相关者交谈更好，并且产生真实的影响是最棒的部分！

由于我的工作，这家公司能够量化他们的良好工作，并在几个月内筹集到超过预期金额的资金。

**外卖**

> 第一课:真正理解商业问题
> 
> 第二课:更简单的解决方案是更好的解决方案
> 
> 第三课:沟通是王道
> 
> 第四课:良好的时间管理

# 喜欢读这本书吗？

> 请在 [LinkedIn](https://www.linkedin.com/in/leihuaye/) 和 [Youtube](https://www.youtube.com/channel/UCBBu2nqs6iZPyNSgMjXUGPg) 找到我。
> 
> 还有，看看我其他关于人工智能和机器学习的帖子。