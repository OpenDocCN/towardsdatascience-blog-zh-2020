# 我希望在成为数据科学家之前学到的 3 课

> 原文：<https://towardsdatascience.com/3-lessons-i-wish-id-learned-before-becoming-a-data-scientist-79ff3206d1dd?source=collection_archive---------27----------------------->

## 不要犯和我一样的错误

在过去的五年里，我从事过多种数据工作，我觉得分享我的一些主要经验是个好主意，因为我看到其他作者也是这样做的。我主要在“数据”是业务支持功能的公司工作，所以我的经历可能与 100%数据驱动的公司不同。还有，这只是个人经验:不同意随便！

# 第 1 课—数据与数据科学家

我学到的第一课是人们或多或少为我准备的。数据比一切都重要:在开始之前确保你理解了数据。然而，当听到这个消息时，我从来没有想到许多真实数据的不正确程度。

> 真实世界的数据通常是完全错误的

![](img/b5d9e19d5f142bfde69809857d132a2d.png)

问问这家伙你的数据在实践中是怎么收集的！在 [Unsplash](https://unsplash.com/s/photos/blue-collar-worker?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) 上[阿三 S.](https://unsplash.com/@ahsan19?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) 的照片

而且不仅仅是数据问题，还是公司问题。有时候，人们似乎花钱收集毫无意义的数据。例如，我曾经参与过一些项目，其目标是预测一个目标变量，即生产线上缺陷的百分比。

项目进行了四个月，我发现测量我们目标值的跟踪器完全错误:生产线的操作人员使用检测器的方式与他们应该使用的方式不同，导致数据出现巨大偏差。结果:项目被取消了，我可以丢掉四个月的工作。

> 要了解数据是如何真正生成的，您需要询问合适的人:您的用户或生成数据的现场工作人员

结论:数据往往是错误的。观察个人观察和深入研究数据收集过程的工作方式具有巨大的价值。不是理论上的，是实践上的！如果可能的话，去访问数据生成。在开始之前，请每天使用这些工具的人给你一个可用性评估。你真的想在一开始就发现这一点，而不是在最后。

# 第 2 课—机器学习 KPI 与业务逻辑

我的第二课是关于我们数据科学家工具箱中的统计和机器学习模型。

> 理解你的模型比你想象的要重要得多。

我明白了，理解你的模型比你想象的要重要得多。在数据科学中，我们总是在谈论准确性分数。它们非常重要。此外，提高模型的准确性是我的工作，所以我们不能忽视它。

![](img/ae9cbfbace0533f2c8d50a7f0d15ef83.png)

著名的黑天鹅效应是众所周知的，但却被忽视了。在 [Unsplash](https://unsplash.com/s/photos/black-swan?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) 上[阿三 S.](https://unsplash.com/@ahsan19?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) 的照片

> 有些事件非常罕见，可能不会出现在你的训练数据中:你知道你的模型会做什么吗？

但我们都知道，当我们建立一个模型时，准确性就像乐透一样。有时是好的，有时是坏的。最坏的情况:有些事件非常罕见，可能不会出现在你的训练数据中。这也被称为黑天鹅效应。

我的经验是:当你的生产模型变坏的时候，你发现有时会出现一个罕见的值，你的模型开始做出非常糟糕的决定，你会希望你已经检查了你的模型的实际拟合度。

# 第 3 课—数据科学家与他的客户

我得到的最糟糕的教训是:许多客户、利益相关者、经理对第一课和第二课根本不感兴趣。有时候，数据科学最难的部分是证明什么可以做，什么不可以做，以及某些项目需要多少时间。

> 有时候数据科学最难的部分是证明你能做什么或不能做什么。

![](img/ddd532522a4bd7195608068b6a211b2c.png)

一个著名的网络迷因。

我听说过这样的情况，在选定的 KPI 上表现优异的模型会被送到 prod，不管数据科学家是否同意。你可以说 KPI 是错的，但有时客户会非常关注 KPI，而不是拥有一个可靠的解决方案。

![](img/5ebaca85e7fb804f99152e3b19c36751.png)

与你的客户合作，而不是与他们作对。照片由[塞巴斯蒂安·赫尔曼](https://unsplash.com/@officestock?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)在 [Unsplash](https://unsplash.com/s/photos/business-negotiation%2C?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) 上拍摄

在数据科学中，通常很难预先知道一个项目是否可行。在这个领域，理由、沟通和反馈循环非常重要，适应不同类型的客户是最基本的。

> 渐进式的工作方式改善了与利益相关者的合作，并能使最终结果更好。

我知道了一种渐进的工作方式可以改善与利益相关者的合作:从构建一个最小的解决方案开始——无论是数据分析还是工作模型——并询问利益相关者的反馈。然后，如果他们想继续你提议的方向，增加你的产品，直到你的利益相关者发现它足够好。

![](img/a1daa335780fa3ff51afb5900c6079bc.png)

使用增量产品交付一步一步从一无所有到一个伟大的产品。艾萨克·史密斯在 [Unsplash](https://unsplash.com/s/photos/cycle-up-graph?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) 上拍摄的照片

这种增量和反馈循环实践基于**敏捷方法**，避免了数据科学项目最终没有好的解决方案的部分风险。你甚至可以根据这种方法制定合同，在没有事先确定总金额的情况下，说明每一产品增量或每一花费时间的价格。这在一开始看起来很冒险，但是它可以让你的最终结果好得多。

*感谢阅读。不要犹豫，在评论中分享你的学习，这样我们就可以互相学习了！*