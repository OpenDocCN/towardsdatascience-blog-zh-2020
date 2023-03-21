# SKlearn 中最强大的组合拳:Pipeline & GridSearchCV

> 原文：<https://towardsdatascience.com/the-most-powerful-one-two-punch-in-sklearn-pipeline-gridsearchcv-9fd5b66f1819?source=collection_archive---------46----------------------->

![](img/68afb43012afcd72c4020e4a462db56e.png)

[澳门图片社](https://unsplash.com/@macauphotoagency?utm_source=medium&utm_medium=referral)在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄的照片

## 额外的好处:没有更多的意大利面条代码

有没有被自己的意大利面卡住过？

听起来很美味。

但是对于编码人员来说，这是非常令人沮丧的。

我将尝试为您提供两种工具，它们将缩短您的代码，并让您重新控制您必须经历的许多迭代，以优化您的机器学习工作流，而不是逆来顺受。

你的模型构建代码是什么样的？

我的过去是这样的:

# 管道

上面的代码可以通过 Sklearn 中的[管道](https://scikit-learn.org/stable/modules/generated/sklearn.pipeline.Pipeline.html#sklearn.pipeline.Pipeline)模块以一种更加简洁和容易的方式编写。

# GridSearchCV

假设我们希望优化我们的分类器，并通过不同的参数进行迭代，以找到最佳模型。最好的方法之一是通过 SKlearn 的 [GridSearchCV](https://scikit-learn.org/stable/modules/generated/sklearn.model_selection.GridSearchCV.html) 。它可以从您输入的参数集中为您提供最佳参数。

# 强力组合

继续拳击的比喻，把这个钩子和刺拳放在一起会形成一个强大的组合。

它肯定会让你大吃一惊。

我们将有一个非常干净灵活的机器学习工作流程，只需要几行代码:

最后，我们可能希望使它更加通用，这样我们就可以迭代几个不同的模型、参数和特性操作。

使用这个函数，我们现在可以有一个非常短的主函数:

请享受使用这一个二打孔机学习组合！

以前发表的文章包括:

*   [这会让你明白数据科学到底有多难](/this-will-make-you-understand-how-hard-data-science-really-is-f4330bb8f672)
*   [在阿姆斯特丹开办 Airbnb 之前，你需要知道这一点](https://medium.com/@essens/you-need-to-know-this-before-you-start-an-airbnb-in-amsterdam-50e402aab986)