# 集成方法:比较 Scikit Learn 的投票分类器和堆叠分类器

> 原文：<https://towardsdatascience.com/ensemble-methods-comparing-scikit-learns-voting-classifier-to-the-stacking-classifier-f5ab1ed1a29d?source=collection_archive---------29----------------------->

## *使用 Titanic 数据集比较 scikit 学习投票分类器和堆叠分类器。*

![](img/0181ad352bcb0445097fc3a65d91137c.png)

照片由[佩里·格罗内](https://unsplash.com/@perrygrone?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)在 [Unsplash](https://unsplash.com/s/photos/working-together?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) 上拍摄

他们说，两个脑袋比一个要好。有时在许多机器学习项目中，我们希望使用集成方法来利用协同的力量。投票和堆叠分类器为我们带来了结合 2 个或更多机器学习模型以获得更高预测性能的好处。

**投票分类器**

投票分类器的工作方式类似于选举系统，其中基于一组机器学习模型成员的投票系统对新数据点进行预测。根据 scikit_learn 的文档，可以在硬投票和软投票类型之间进行选择。

硬投票类型应用于多数规则投票的预测类标签。这使用了“多数决定投票”的思想，即做出有利于拥有半数以上投票的人的决定。

**软**表决基于组成集合的各个估计器的预测概率之和的 argmax 来预测类别标签。在良好校准/拟合的分类器的集成的情况下，经常推荐软投票。

例如:如果*模型 1* 预测 ***A*** ，而*模型 2* 预测 ***B*** ，而*模型 3* 预测 ***A*** 。*投票分类器*(with***voting =‘hard’***)返回 ***A*** 。在平局的情况下，投票分类器将基于升序选择类别。

![](img/fb5d918fca829b7be1c2a7d8fa9353e9.png)

由[马库斯·斯皮斯克](https://unsplash.com/@markusspiske?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)在 [Unsplash](https://unsplash.com/s/photos/stacking-stones?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) 上拍摄的照片

**堆垛分级机**

堆叠包括在同一数据集上组合来自多个机器学习模型的预测。我们首先在我们的数据集上指定/建立一些称为基础估计器的机器学习模型，来自这些基础学习器的结果然后作为我们的堆叠分类器的输入。堆叠分类器能够了解我们的基本估计值何时可信或不可信。叠加允许我们通过将每个估计量的输出作为最终估计量的输入来利用每个估计量的强度。

在使用堆叠分类器时，可以选择在基础学习者级别或在最终估计器上应用交叉验证。使用 scikit learn stacking 分类器，基础学习器适用于全 X，而最终估计器使用基础学习器的交叉验证预测来训练。

多层堆叠也是可能的，其中在构建最终估计器之前构建基础学习器的层。

值得注意的是，在考虑集合方法来改进我们的预测之前，建议:

1.  在可能的情况下，获取更多的数据。我们输入到模型中的数据越多，模型需要学习的学习示例就越多。
2.  特征工程。
3.  我们模型的超参数调整。

**方式和方法**

在这个例子中，我们考虑了“泰坦尼克号:机器从灾难中学习”。我们分割训练数据集，以便能够对我们的模型进行自己的评估。数据集也可以在这里找到:【https://www.kaggle.com/c/titanic/data】T2。然后，我们构建一个逻辑回归、最近邻和随机森林算法。

我们使用上述 3 种算法作为 scikit learn 的投票和堆叠分类器的估计器，并比较它们预测的 f1 分数。

*注意:堆叠分类器仅在 0.22 版本的 Scikit-Learn 中可用，而投票分类器从 0.17 版本开始可用。*

**观察结果**

***本例中投票和堆叠分类器的 f1_score 相同。***

1.  在投票分类器和堆叠分类器中，重要的是要确保基本估计量能够很好地给出准确的预测。正如人们所说的，*‘垃圾进，垃圾出’*，如果估计器放入垃圾，那么任何一种集成方法都会产生垃圾。
2.  重要的是要注意，堆叠分类器在基础模型(或估计器)上建立模型，从而增加对数据集的拟合。这也增加了堆积分类器过度装配的趋势，尤其是在多层堆积的情况下。
3.  投票分类器的性能很大程度上取决于基本模型的结果。因此，为了更好地预测，建议使用合适的估计量作为软投票类型的基础模型。
4.  有时候，集合方法并不是真的需要。😉

**结论**

很难确定投票或堆叠分类器哪个更好。明智的做法是了解它们中每一个的用途/工作原理。这种理解将有助于决定在什么时候应用它们是最好的。

我希望这篇文章对你有帮助。这里是我的 [Twitter](https://twitter.com/samsonafo) 句柄和 [Linkedin](https://www.linkedin.com/in/samson-afolabi/) 页面。链接到 github repo [这里](https://github.com/samsonafo/ensemble_methods)。

如果你喜欢这篇文章，你可以考虑给我买☕️.咖啡

Vielen Dank😊