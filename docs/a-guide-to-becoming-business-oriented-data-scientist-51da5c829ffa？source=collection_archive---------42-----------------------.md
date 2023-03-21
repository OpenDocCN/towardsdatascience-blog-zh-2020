# 成为面向业务的数据科学家指南

> 原文：<https://towardsdatascience.com/a-guide-to-becoming-business-oriented-data-scientist-51da5c829ffa?source=collection_archive---------42----------------------->

## 如何从商业角度开始思考

![](img/77e08388b31617315a465946789db610.png)

图片由 [Unsplash](https://images.unsplash.com/photo-1556761175-5973dc0f32e7?ixlib=rb-1.2.1&ixid=eyJhcHBfaWQiOjEyMDd9&auto=format&fit=crop&w=2380&q=80) 上的 [Austin Distel](https://unsplash.com/@austindistel) 拍摄

技术技能有助于数据科学从业者解决复杂的问题，如发现评论的情绪或将交易分类为有风险或无风险。强大的数据科学从业者知道如何找到正确的特性，测试模型性能，然后使用适当的技术对其进行扩展。

> 到目前为止，一切听起来都不错，但仅此而已吗？一名优秀的数据科学从业者需要具备的全部技能是什么？

一个完整的数据科学从业者除了在技术上很强之外，还会从业务角度进行思考。这篇博客文章就是关于这个的，从商业的角度思考，将技术解决方案转化为经济上有影响力的解决方案是什么感觉。

# 成为面向业务的数据科学家之旅中的三堂课

![](img/3d091ee51c6a8d0dc16f7583400e9dbf.png)

图片由[马丁·威尔纳](https://unsplash.com/@mwilner)在 [Unsplash](https://images.unsplash.com/photo-1537486336219-a3dd8e2dc6b5?ixlib=rb-1.2.1&ixid=eyJhcHBfaWQiOjEyMDd9&auto=format&fit=crop&w=2378&q=80) 上拍摄

## 1.量化财务影响

这是数据科学从业者需要具备的最重要的业务技能。这是提供一个大概的数字，作为实施推荐功能的业务影响的能力。我们用一个例子来理解。

> 假设你是一名数据科学家，在一家基于移动应用的公司工作。移动应用根据用户识别的潜在兴趣和关注的标签向用户显示图片和视频。该公司通过在应用程序中显示广告来赚取收入。作为一名数据科学家，你被要求改进推荐这些图片和视频的推荐系统。您对以前的系统做了一些改进，并对样本用户进行了测试，以发现改进/增加的时间花费。结果显示，每个用户平均花费的时间提高了 1%。

接下来，您向风险承担者展示了结果，他们对所花时间的增加/改善并不满意。他们觉得改善很少。利益相关者无法做出决定的一个原因是，将建议的推荐系统扩展到整个应用程序涉及到成本，他们不清楚 1%的改善会带来多大的收入。

***通过实施推荐系统的变革，您在向利益相关者清晰展示财务影响方面可以做得更好吗？可以做的一件事是量化收益。让我们看看我们如何能做到这一点，并得到一些大概的数字。***

```
Average time spent by a user on app per day = 20 minsImprovement in time spent per user by implementing the changes in Recommender system = 1%Total user base = 10M users.Average number of adverstisements shown to user per 2 minutes = 1
i.e 0.5 advertisements per minuteAmount earned per advertisements = $0.02Estimated Revenue gain daily on implementing the solution = 
Total_users * additional_1%_time_per_user * advertisements_per_minute * amount_earned_per_advertisement
= 10M * (0.01 * 20) * 0.5 * 0.02 = $20KEstimated Revenue gain monthly = $20K * 30 = $600K
```

展示实施建议推荐系统的每月 60 万美元收益的量化值，将有助于利益相关者更好地决定是否承担实施和扩展建议系统的成本。

## 2.掌握赢得利益相关者信任的艺术

利益相关者通常是了解企业内外的人。他们可能只有非常有限的技术知识，但对什么可行什么不可行有非常强的商业直觉。这些人见证了企业的发展壮大，赢得他们的信任是一门艺术。让我们看看如何做到这一点。

假设我们正在为业务提供一个预测解决方案。在讨论我们的模型做什么或它的准确性之前，我们将展示一些从数据中获得的事实和数字，以向他们表明我们知道我们在做什么。这些事实可能是-

*   每年 11 月至 12 月期间，我们都会看到销售额在节日期间增长大约 X%。
*   与去年同期相比，中国的经济增长率约为 1.5% .
*   我会展示一些图表来增强我对事实和数据的信心。

在陈述了事实和数据之后，我将会谈到所考虑的特征，听取利益相关者的意见，因为商业知识有时会变得非常重要，并提供一种优势。

关于建模细节，我将保持非常简短和高层次的概述，甚至跳过并继续讨论对业务最重要的东西，在许多情况下是准确性或增益或输出。

> 有时，大声说出面临的挑战也很重要，因为这有助于赢得一些时间，否则利益相关者总是急于要交付成果。

## 3.建议更多

面向业务的数据科学从业者不仅很好地完成了要求他做的事情，而且还通过向利益相关者建议还可以做些什么来更进一步。一个好的数据科学从业者向利益相关者展示了更多神奇的数据科学可以改变他们的业务。

> 如果我们以同一家基于移动应用的公司为例，它可以向利益相关者建议向用户智能通知媒体的想法及其利益实现。还可以是基于点击率对广告进行排名，并利用该排名来决定更多地显示哪些广告。

> 不要停下来，继续思考。

# 结论

通过这篇博客，我专注于数据科学的商业方面。我们讨论了如何量化模型的财务影响。我们还讨论了如何赢得利益相关者的信任，以及如何从商业角度思考问题。希望你喜欢这篇文章。

> 如果你有任何疑问，请联系我。我有兴趣了解您正在解决的业务问题，以及您如何创建解决方案并量化其财务影响。

***我的 Youtube 频道获取更多内容:***

[](https://www.youtube.com/channel/UCg0PxC9ThQrbD9nM_FU1vWA) [## 阿布舍克·蒙戈利

### 嗨，伙计们，欢迎来到频道。该频道旨在涵盖各种主题，从机器学习，数据科学…

www.youtube.com](https://www.youtube.com/channel/UCg0PxC9ThQrbD9nM_FU1vWA) 

> ***关于作者-:***
> 
> Abhishek Mungoli 是一位经验丰富的数据科学家，拥有 ML 领域的经验和计算机科学背景，跨越多个领域并具有解决问题的思维方式。擅长各种机器学习和零售业特有的优化问题。热衷于大规模实现机器学习模型，并通过博客、讲座、聚会和论文等方式分享知识。
> 
> 我的动机总是把最困难的事情简化成最简单的版本。我喜欢解决问题、数据科学、产品开发和扩展解决方案。我喜欢在闲暇时间探索新的地方和健身。在 [**中**](https://medium.com/@mungoliabhishek81) 、**[**Linkedin**](https://www.linkedin.com/in/abhishek-mungoli-39048355/)**或**[**insta gram**](https://www.instagram.com/simplyspartanx/)**上关注我，查看我[以前的帖子](https://medium.com/@mungoliabhishek81)。我欢迎反馈和建设性的批评。我的一些博客-********

*   ******[每个数据科学家都应该避免的 5 个错误](/5-mistakes-every-data-scientist-should-avoid-bcc8142d7693)******
*   ******[以简单&直观的方式分解时间序列](/decomposing-a-time-series-in-a-simple-and-intuitive-way-19d3213c420b?source=---------7------------------)******
*   ******[GPU 计算如何在工作中拯救了我？](https://medium.com/walmartlabs/how-gpu-computing-literally-saved-me-at-work-fc1dc70f48b6)******
*   ******信息论& KL 分歧[第一部分](/part-i-a-new-tool-to-your-toolkit-kl-divergence-5b887b5b420e)和[第二部分](/part-2-a-new-tool-to-your-toolkit-kl-divergence-736c134baa3d)******
*   ******[使用 Apache Spark 处理维基百科，创建热点数据集](/process-wikipedia-using-apache-spark-to-create-spicy-hot-datasets-1a59720e6e25)******
*   ******[一种基于半监督嵌入的模糊聚类](/a-semi-supervised-embedding-based-fuzzy-clustering-b2023c0fde7c)******
*   ******[比较哪种机器学习模型表现更好](/compare-which-machine-learning-model-performs-better-4912b2ed597d)******
*   ******[分析 Fitbit 数据，揭开疫情封锁期间身体模式变化的神秘面纱](/analyzing-fitbit-data-to-demystify-bodily-pattern-changes-amid-pandemic-lockdown-5b0188fec0f0)******
*   ******[神话与现实围绕关联](/myths-and-reality-around-correlation-9b359456d8e1)******