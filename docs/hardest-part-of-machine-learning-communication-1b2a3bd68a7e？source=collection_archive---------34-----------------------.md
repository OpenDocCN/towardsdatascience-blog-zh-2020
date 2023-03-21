# 机器学习最难的部分？交流。

> 原文：<https://towardsdatascience.com/hardest-part-of-machine-learning-communication-1b2a3bd68a7e?source=collection_archive---------34----------------------->

## 为了确保你的机器学习项目通过商业审查——确保你很好地沟通它。

一天，我们为我的一个客户建立了一个随机森林模型，可以预测任何用户的生命周期价值。和大多数模型一样，我们意识到沟通是关键。

![](img/b2f2331494679da5b2e44b19931ce62e.png)

[优 X 创投](https://unsplash.com/@youxventures?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)在 [Unsplash](https://unsplash.com/s/photos/meeting?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) 上的照片

## **从一开始就沟通**

好的，首先，沟通从来都不是事后的想法。实际上，您希望从一开始就将您的模型传达给涉众。问题是，建立生产化的机器学习模型是一个非常耗时的过程。而且，你需要利益相关者 100%的认同。否则，相信我，你的模型想法，类似于被鹈鹕吞食的小海龟，会很快死去。

## **“你的模型做好了吗？”(真诚地，利益相关者)**

![](img/8a69a811d2446f0b68800366cc5934d1.png)

照片由 [s](https://unsplash.com/@youxventures?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) tock123 在 [Unsplash](https://unsplash.com/s/photos/meeting?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) 上拍摄

建立一个具有可接受误差指标的强大模型需要几个月的时间。你猜怎么着？—这是一个很长的时间要求。利益相关者会追问你的进展，并想知道你的时间是否能得到更好的利用。

先做最重要的事情—确保首先有一个非常可靠的模型推介和使用案例。有一个 MVP 的概念——它代表“最低价值主张”就模型而言，MVP 是一个经过训练的基本模型，它以某种数据格式给出一些预测。

因此，确保以增量的方式交付您的模型。请不要为一个算法构建复杂的自动化管道，除非你已经交付了一些更基本的东西。不求完美。将有形的数据呈现在利益相关者面前，从而让他们体验他们可以利用这些数据做的所有事情。这为你接下来的步骤赢得了越来越多的支持。

或者，涉众可能意识到数据对他们没有用，他们理所当然地终止了项目。这将确保你不会在一个没人关心的模型上工作——这可能很有趣，但会毁了你在一家公司的职业生涯。

## **我需要一个模特吗？**

求求你，求求你——一定要做好做模型的成本收益分析。我知道，作为数据科学家，我们对执行机器学习项目所涉及的复杂性和学习感到兴奋。但是，一定要认识到，机器学习模型的收益必须足够大，以证明建立一个完整的机器学习模型所花费的大量时间是合理的。所以，一定要寻求最简单的解决方案。首先避免机器学习解决方案，看看是否有更简单的解决方案就足够了。只有当问题很重要，没有机器学习无法解决时，你才着手进行机器学习项目。

## **沟通障碍**

当利益相关者听你说话时，他们脑子里只有一件事“这如何影响或关系到我的业务？”因此，相应地调整你的信息。此外，业务利益相关者没有你所拥有的技术背景，所以这里是你对随机森林算法的详细解释——“bla bla Lala la bla bla bla Lala bla Lala bla”。

![](img/0f9e6a9f6208c6b4873d8828ff0e869a.png)

照片由[在](https://unsplash.com/@helloquence?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) [Unsplash](https://unsplash.com/s/photos/meeting?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) 上按顺序拍摄

那么，这是否意味着完全不谈模型，只陈述模型的误差？不，我们的目标是从模型中抽象出来，让任何人都能理解。举例来说，有两种解释重力的方式——你可以写出所有的物理方程，或者说“这是一种当我们放下东西时使它们落下的力”。选择容易的一个。

## **我们是如何做到的**

好了，我已经说够了该做什么不该做什么。这就是我们所做的。

我们意识到利益相关者并不太关心我们所使用的算法的本质。因此，我们省略了细节，用抽象的术语讨论了模型。

![](img/56e1710892327685b60c0a2055abdfb5.png)

我们内部使用的指标

我们团队内部使用的误差指标是 RMSE。然而，RMSE 的想法对利益相关者来说有点复杂和难以理解。此外，我们在用户层面建立了模型，但随后我们将其扩展到真正精细的群组中。因此，我们选择了一个不同的、更适合利益相关者的指标。

我们选择的标准是实际值与预测值的百分比差异。然后，我们会观察误差在不同人群中的分布。然而，在内部，作为一个团队，当我们调整模型时，我们使用 RMSE 作为我们试图最小化的指标。但是，当我们与利益相关者交谈时，我们展示了一个不那么吓人的、更合适的百分比差异指标。

而且，因为 RMSE 是一个更敏感的误差指标，无论我们做什么来降低 RMSE，都会降低队列百分比差异的下游指标。