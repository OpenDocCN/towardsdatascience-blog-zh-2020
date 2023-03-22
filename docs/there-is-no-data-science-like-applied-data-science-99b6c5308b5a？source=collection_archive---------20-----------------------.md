# 没有像应用数据科学这样的数据科学

> 原文：<https://towardsdatascience.com/there-is-no-data-science-like-applied-data-science-99b6c5308b5a?source=collection_archive---------20----------------------->

## *FastAI 的杰瑞米·霍华德如何向我推销面向大众的应用数据科学*

# 介绍

前阵子我听了莱克斯·弗里德曼对[法斯泰](https://www.fast.ai/)的[杰瑞米·霍华德](https://youtu.be/J6XcP4JOHmk)的采访。在那次采访中，他分享了自己的激情:让神经网络变得非常容易使用，从而让更多的人能够接触到它们。他认为，通过增加试图解决相关问题的人数，从长远来看，更多的问题将会得到解决。他引用的例子是一位精通技术的医生使用深度学习来解决具体的医疗问题。

在这篇文章中，我想阐述我认为一个可访问的应用深度学习平台需要什么。此外，我想深入研究更基础的研究和应用深度学习之间的关系。

![](img/9fc9f039ea315f241c62e0a3d7575242.png)

照片由 [Antoine Dautry](https://unsplash.com/@antoine1003?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) 在 [Unsplash](https://unsplash.com/s/photos/mathematic?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) 上拍摄

# 是什么让深度学习系统变得触手可及

*抽象层次*

一个可访问的系统需要正确处理的最重要的方面之一是接口的抽象层次。对于术语抽象，我指的是用户在使用深度学习系统时必须处理的信息的数量和类型。从用户那里抽象出不需要的信息和细节使得信息负载更易于管理。哪些信息被认为是不需要的，这当然是设计这种界面的关键挑战。

例如，当我们训练一个 [CNN](https://en.wikipedia.org/wiki/Convolutional_neural_network) 来执行图像识别时，应用用户不需要知道[梯度下降如何精确地拟合神经网络](https://ml4a.github.io/ml4a/how_neural_networks_are_trained/)的系数。如果有好的自动评估方法，也不应该要求用户调整神经网络内部的设置。当然，在理想的系统中，如果用户需要，可以访问底层细节。

*软件管理的稳定性和易用性*

安装和更新系统应该很容易。此外，软件应该非常稳定和可预测。奇怪的错误和意想不到的行为是不关注 it 的用户的祸根:他们只是想完成工作，而不是花几天时间小心翼翼地建立一个工作系统。

最近钻研强化学习，结果用了[试剂](https://reagent.ai/)。该系统比研究人员编写的代码进步了一大步，但它肯定不是一个像 [sklearn](https://scikit-learn.org/stable/) 那样可访问且健壮的系统。如果你不能全身心投入到试剂中，那么缺乏可及性会使你很难工作。这限制了我的可用性，因为我一周只能奉献一天。将深度学习工具交到*兼职*用户手中，尤其是领域专家，有很大的潜力让很多现实世界的问题得到解决。

*合理的资源使用*

从较小的数据集和回归等工具到较大的数据集和深度学习，所需的计算资源量呈指数级增长。大公司和研究小组可以投资大量的这种计算，但这对于普通用户来说是不可行的。FastAI 的一个关键设计原则是，它应该在单个 GPU 上运行，这使得投资变得可行，因为您只需要一台像样的游戏或工作站 pc。

# 基础研究呢

随着所有这些关于应用深度学习重要性的讨论，核心研究呢？霍华德的一个挫折是，深度学习研究社区过度关注算法的小增量更新。让学习过程加快百分之五，稍微调整一下网络架构以提高性能。他真的想解决现实世界的问题，而不是拘泥于细枝末节。

当然，如果没有研究，所有花哨的算法和工具都不会首先存在，让 FastAI 捡起来。在我看来，我们两者都需要:研究人员发明解决问题的新方法，行业人士和领域专家可以采用这些工具，并将它们应用到新的环境中。像 FastAI 这样的工具对帮助行业和领域专家做到这一点大有帮助。

**最后**，你对可访问的深度学习系统有什么想法？我们应该把这些工具放在群众手中，还是应该留给专家？

如果你喜欢这篇文章，你可能也会喜欢我的一些更长的文章:

*   [通过规范化扩展您的回归曲目](/expanding-your-regression-repertoire-with-regularisation-903d2c9f7b28)
*   [牛郎星图解构:可视化气象数据的关联结构](/altair-plot-deconstruction-visualizing-the-correlation-structure-of-weather-data-38fb5668c5b1)
*   [面向数据科学的高级函数式编程:用函数运算符构建代码架构](/advanced-functional-programming-for-data-science-building-code-architectures-with-function-dd989cc3b0da)