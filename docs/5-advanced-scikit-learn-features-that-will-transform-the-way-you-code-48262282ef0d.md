# 5 个高级 sci kit-了解将改变您编码方式的特性

> 原文：<https://towardsdatascience.com/5-advanced-scikit-learn-features-that-will-transform-the-way-you-code-48262282ef0d?source=collection_archive---------43----------------------->

![](img/23a4b30ec1b52c4e4a44e2edb9b96669.png)

*照片由* [*安德烈斯·达利蒙提*](https://unsplash.com/@dallimonti?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) *上* [*Unsplash*](https://unsplash.com/s/photos/cockpit?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)

很少有软件包能成功达到 sklearn 所达到的水平。这不仅是因为它们提供了几乎所有常用的 **ML 算法**，也是因为它们提供这些算法的方式。Sklearn 的核心代码是用 **Cython** 编写的，提供了优化的性能。他们的 **API** 被设计成提供一致性、可读性和可扩展性。在核心的 ML 算法之上，sklearn 为您提供了创建**端到端管道**的附加功能。如果有一个形容词可以形容这个包，它应该是“**”。**

*如果你曾经和 sklearn 合作过，你大概会对 fit，predict，transform 等常用方法比较熟悉。也许您还会熟悉一些其他的预处理方法。但是这个软件包的功能远远超出了常用的功能。*

***这篇文章的目标是** **突出 sklearn** 的一些非常强大但不太为人所知的特性。这些功能将使您能够释放 sklearn 的最大潜力。您将快速了解这些功能是什么以及如何使用它们。将提供一个非常短的代码片段，随后是更多详细信息的参考。代码片段的目的只是为了说明**的功能和语法**。这些片段并不代表完整的工作流程。最后，使用了 *0.22.1* 。*

# *1.管道*

*您的模型将总是由**多个连续阶段**组成，其中一个阶段的输出将是下一个阶段的输入。例如，高维输入的分类器通常包括归一化、维度减少和分类模型。*

*Sklearn 的管道提供了一个**优雅的包装器**来链接这些连续的步骤。当您将使用管道时，您将不必担心管理中间对象。您需要做的就是指定步骤并调用`fit`方法。在持久化你的模型的时候，你只需要 pickle 一个对象，就是管道。使用管道将提高代码的可读性，减少错误，并减轻训练模型的持久性。*

*[阅读更多](https://scikit-learn.org/stable/modules/compose.html)*

# *2.内嵌目标转换器*

*在某些情况下，在训练你的模型之前，你可以从目标的**非线性变换**中受益匪浅。例如，对于重尾目标的对数变换通常是非常明智的步骤。当使用模型预测新数据时，您需要确保您**对预测的这个转换**进行逆运算。*

*这里有一些好消息:您不需要使用 Pandas 或 Numpy 来创建这些转换。您可以**使用 Sklearn** 直接应用目标变换，如下图所示:*

**下面的事情会在引擎盖下自动发生:* *一边训练:* `regressor.fit(X,func(y))` *一边预测:* `inverse_func(regressor.predict(X))`*

*[阅读更多](https://scikit-learn.org/stable/modules/generated/sklearn.compose.TransformedTargetRegressor.html)*

# *3.特征联合*

*即使使用 sklearn 提供的顺序步骤，您也不会被每个步骤只有一个转换器所限制。你可以使用**多个变压器**和**连接**的结果在一个单一的步骤。*

*在上面的流水线示例中，我们在训练之前使用了单个 PCA 来转换标准化数据。让我们举一个例子，除了线性 PCA 之外，我们还想使用内核 PCA:*

*也可以在管线中插入特征联合。当然，你也可以编写自己的变形金刚。*

*[阅读更多](https://scikit-learn.org/0.18/auto_examples/hetero_feature_union.html)*

# *4.滚动预测的链接模型*

*有时您会面临这样的情况，您需要链接多个模型，例如第一个模型的**输出是第二个模型的**输入。这种链接的一个非常常见的用例是在时间序列模型中:如果我们需要预测两个时间步，`y(t+1)`的预测将是在`y(t+2)`进行预测的输入。*

*使用 sklearn，您可以选择自动创建链接。功能为`RegressorChain`或`ClassifierChain`。您的`y`将不是一个数组，而是一个包含多个相关目标的矩阵。`RegressorChain`将自动包括**前一个目标，以预测下一个**。在预测过程中，该链将根据上一个目标的预测来预测下一个目标。您需要做的就是像往常一样使用`fit`和`predict`方法。*

*[阅读更多](https://machinelearningmastery.com/multi-output-regression-models-with-python/)*

# *5.使用排列的特征重要性*

*特征重要性通常是我们能够拥有并呈现给最终用户的最重要的建模洞察力之一。但是，根据您使用的算法，获取它们并不总是直截了当的。*

*置换可用于**推断每个特征的重要性**，而不考虑建模方法。背后的核心思想非常直观:单个特征被随机洗牌，模型得分的降低被量化。变化越大，特性越重要。*

*[阅读更多](https://scikit-learn.org/stable/modules/permutation_importance.html)*

> ***现在轮到你了:在 Sklearn 中分享另一个让你的代码变得更好的高级方法。把这个方法写在下面的评论里***