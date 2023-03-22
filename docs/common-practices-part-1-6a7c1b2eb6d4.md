# 常见实践—第 1 部分

> 原文：<https://towardsdatascience.com/common-practices-part-1-6a7c1b2eb6d4?source=collection_archive---------41----------------------->

## [FAU 讲座笔记](https://towardsdatascience.com/tagged/fau-lecture-notes)关于深度学习

## 优化者和学习率

![](img/48d384acfccebb0aad5f408764175b3c.png)

FAU 大学的深度学习。下图 [CC BY 4.0](https://creativecommons.org/licenses/by/4.0/) 来自[深度学习讲座](https://www.youtube.com/watch?v=p-_Stl0t3kU&list=PLpOGQvPCDQzvgpD3S0vTy7bJe2pf_yJFj&index=1)

**这些是 FAU 的 YouTube 讲座** [**深度学习**](https://www.youtube.com/watch?v=p-_Stl0t3kU&list=PLpOGQvPCDQzvgpD3S0vTy7bJe2pf_yJFj&index=1) **的讲义。这是与幻灯片匹配的讲座视频&的完整抄本。我们希望，你喜欢这个视频一样多。当然，这份抄本是用深度学习技术在很大程度上自动创建的，只进行了少量的手动修改。如果你发现了错误，请告诉我们！**

# 航行

[**上一讲**](/regularization-part-5-b4019720b020) **/** [**观看本视频**](https://youtu.be/R5FB6kwC-To) **/** [**顶级**](/all-you-want-to-know-about-deep-learning-8d68dcffc258) **/** [**下一讲**](/common-practices-part-2-23dad51e2aae)

欢迎大家来到今天的深度学习讲座！今天，我们想谈谈常见的做法。你需要知道的在实践中实现一切的东西，

![](img/839f4ca0c097c7056deff75a7b5992e7.png)

接下来几节课的概述。 [CC 下的图片来自](https://creativecommons.org/licenses/by/4.0/)[深度学习讲座](https://www.youtube.com/watch?v=p-_Stl0t3kU&list=PLpOGQvPCDQzvgpD3S0vTy7bJe2pf_yJFj&index=1)的 4.0 。

因此，我对接下来的几个视频和我们将关注的主题有一个小的概述。因此，我们将思考我们目前面临的问题以及我们已经走了多远。我们将讨论训练策略、优化和学习率，以及一些调整策略、架构选择和超参数优化。一个真正有用的技巧是集合。通常人们不得不处理阶级不平衡，当然，也有非常有趣的方法来处理这个问题。最后，我们来看看评估，以及如何得到一个好的预测器。我们还评估网络的实际运行情况。

![](img/4661812f901eac1e3b6c83b83b562aa1.png)

神经网络训练概述。 [CC 下的图片来自](https://creativecommons.org/licenses/by/4.0/)[深度学习讲座](https://www.youtube.com/watch?v=p-_Stl0t3kU&list=PLpOGQvPCDQzvgpD3S0vTy7bJe2pf_yJFj&index=1)的 4.0 。

到目前为止，我们已经看到了如何训练网络的所有细节。我们必须完全连接卷积层、激活函数、损失函数、优化、正则化，今天我们将讨论如何选择架构、训练和评估深度神经网络。

![](img/6e1ea52f892a001932860170dd719e02.png)

只有在我们设置了关于培训过程的所有其他重要选项之后，才会查看测试数据。来自[深度学习讲座](https://www.youtube.com/watch?v=p-_Stl0t3kU&list=PLpOGQvPCDQzvgpD3S0vTy7bJe2pf_yJFj&index=1)的 [CC BY 4.0](https://creativecommons.org/licenses/by/4.0/) 下的图片。

第一件事就是测试。“理想情况下，测试数据应保存在保险库中，仅在数据分析结束时取出。”哈斯蒂和他的同事们正在讲授统计学学习的要素。

![](img/53691a91a3811d932313890dfe86628b.png)

过拟合神经网络很容易实现。因此，我们必须谨慎地做出许多选择。 [CC 下的图片来自](https://creativecommons.org/licenses/by/4.0/)[深度学习讲座](https://www.youtube.com/watch?v=p-_Stl0t3kU&list=PLpOGQvPCDQzvgpD3S0vTy7bJe2pf_yJFj&index=1)的 4.0 。

所以，首先，神经网络很容易过度拟合。记住 ImageNet 随机标签。当您使用测试集进行模型选择时，真正的测试集误差和泛化能力可能会被大大低估。因此，当我们选择架构时——通常是模型选择中的第一个元素——这永远不应该在测试集上进行。我们可以对数据的一个较小的子集进行初步实验，试图找出什么有效。当你做这些事情的时候，千万不要在测试集上工作。

![](img/0e8c2f8d594059bc1f53b2cd97ef88a9.png)

使用数字渐变检查您的渐变实现。 [CC 下的图片来自](https://creativecommons.org/licenses/by/4.0/)[深度学习讲座](https://www.youtube.com/watch?v=p-_Stl0t3kU&list=PLpOGQvPCDQzvgpD3S0vTy7bJe2pf_yJFj&index=1)的 4.0 。

让我们来看看几个训练策略:在训练之前，检查你的梯度，检查损失函数，检查自己的层实现，它们正确地计算结果。如果您实现了自己的图层，请比较解析梯度和数值梯度。你可以用[中心差分来表示数字渐变](/lecture-notes-in-deep-learning-feedforward-networks-part-3-d2a0441b8bca)。你可以用相对误差代替绝对差异，并考虑数字。使用双精度进行检查，暂时调整损失函数，如果观察到非常小的值，适当选择您的 *h* 作为步长。

![](img/d0d752e719d8c4e6efefd51158d78112.png)

梯度调试的更多提示。 [CC 下的图片来自](https://creativecommons.org/licenses/by/4.0/)[深度学习讲座](https://www.youtube.com/watch?v=p-_Stl0t3kU&list=PLpOGQvPCDQzvgpD3S0vTy7bJe2pf_yJFj&index=1)的 4.0 。

然后，我们有几个额外的和建议:如果你只使用几个数据点，那么损失函数的不可微部分的问题会更少。您可以在短时间内训练网络，然后再执行梯度检查。可以先查梯度，再用正则项。所以，你首先关闭正则项，检查梯度，最后用正则项。此外，关闭数据增强并退出。因此，您通常在相当小的数据集上进行这种检查。

![](img/9cececc7d6b55d99d1e4154ac702b997.png)

也检查你的初始化。来自[深度学习讲座](https://www.youtube.com/watch?v=p-_Stl0t3kU&list=PLpOGQvPCDQzvgpD3S0vTy7bJe2pf_yJFj&index=1)的 4.0CC 下的图片。

初始化的目标是对层进行正确的随机初始化。因此，您可以在关闭正则化的情况下计算未训练网络上每个类的损失，当然，这应该会给出一个随机分类。所以在这里，我们可以将这种损失与随机选择课程时的损失进行比较。它们应该是相同的，因为你是随机初始化的。重复多次随机初始化，以检查初始化是否没有问题。

![](img/5d0ac02080aa6b05f402c4d46b93cbed.png)

训练前测试训练设置。 [CC 下的图片来自](https://creativecommons.org/licenses/by/4.0/)[深度学习讲座](https://www.youtube.com/watch?v=p-_Stl0t3kU&list=PLpOGQvPCDQzvgpD3S0vTy7bJe2pf_yJFj&index=1)的 4.0 。

我们去训练吧。首先，您检查架构是否总体上能够学习任务。因此，在使用完整数据集训练网络之前，您需要数据的一个小子集。也许 5 到 20 个样本，然后尝试使网络过载以获得零损失。用这么少的样本，你应该能够记住整个数据集。尽量做到零损失。然后，你知道你的训练程序实际上是有效的，你真的可以降到零损失。或者，您可以关闭正则化，因为它可能会阻碍这种过度拟合过程。现在，如果网络不能过度拟合，你可能在实现中有一个 bug，或者你的模型可能太小。因此，您可能希望增加参数/模型容量，或者只是模型可能不适合此任务。此外，首先要了解数据、丢失和网络的行为。

![](img/b7ed4b797dd5c7b2bc4bf0b316fd9016.png)

损耗曲线有助于识别爆炸和消失梯度。来自[深度学习讲座](https://www.youtube.com/watch?v=p-_Stl0t3kU&list=PLpOGQvPCDQzvgpD3S0vTy7bJe2pf_yJFj&index=1)的 [CC BY 4.0](https://creativecommons.org/licenses/by/4.0/) 下的图片。

记住，我们应该监控损失函数。这些是典型的损失曲线。确保没有爆炸或消失的渐变。你想要有适当的学习率，所以检查学习率以识别学习曲线中的大跳跃。如果您有非常嘈杂的曲线，尝试增加批量大小。噪音损失曲线可能与过小的小批量有关。

![](img/14f4effc96292291b98185dfcc40b611.png)

监控验证损失将有助于您在训练期间发现过度拟合。 [CC 下的图片来自](https://creativecommons.org/licenses/by/4.0/)[深度学习讲座](https://www.youtube.com/watch?v=p-_Stl0t3kU&list=PLpOGQvPCDQzvgpD3S0vTy7bJe2pf_yJFj&index=1)的 4.0 。

接下来，获取验证数据集并监控验证损失。你记得，这个图像:随着时间的推移，你的训练损失当然会下降，但是测试损失会上升。当然，您从来没有在测试数据集上计算过，但是您将验证集作为测试损失的替代。然后，您可以确定您的网络中是否出现过拟合。如果训练和验证有分歧，你有过度拟合。所以，你可能想增加正则化或尝试早期停止。如果训练和验证损失很接近，但非常高，您可能有欠拟合。因此，减少正则化并增加模型大小。您可能想要保存中间模型，因为您可以在以后的测试中使用它们。

![](img/a31874799466d49917b562721a4c4d00.png)

查看经过训练的卷积核可以帮助识别噪声模式检测器。 [CC 下的图片来自](https://creativecommons.org/licenses/by/4.0/)[深度学习讲座](https://www.youtube.com/watch?v=p-_Stl0t3kU&list=PLpOGQvPCDQzvgpD3S0vTy7bJe2pf_yJFj&index=1)的 4.0 。

此外，在训练期间，监控重量和激活。跟踪权重更新的相对幅度。他们应该在一个合理的范围内，也许是 10⁻。在卷积层中，您可以检查前几层的滤波器。它们应该朝着平滑和规则的过滤器发展。你可能想检查一下。你需要像这里一样的过滤器，在右手边。左手边的那些包含相当多的噪声，并且这可能不是非常可靠的特征。你可以从这里开始建造一个噪音探测器。所以这可能是个问题。此外，检查大量饱和激活。请记住，死亡可能会发生。

![](img/ab6fba91b393cd703143afa213e0c475.png)

小费选择优化。来自[深度学习讲座](https://www.youtube.com/watch?v=p-_Stl0t3kU&list=PLpOGQvPCDQzvgpD3S0vTy7bJe2pf_yJFj&index=1)的 [CC BY 4.0](https://creativecommons.org/licenses/by/4.0/) 下的图片。

所以让我们看一下优化和学习率。你想选择一个优化器。现在批量梯度下降需要大内存，太慢，更新太少。所以人们追求的是典型的随机梯度下降。这里，损失函数和梯度变得非常嘈杂，特别是，如果你只使用你的一个样本。你想要小批量的。小批量是两全其美的。它具有频繁但稳定的更新，并且梯度的噪声足以避开局部最小值。因此，您希望根据您的问题和优化调整小批量大小，以产生更平滑或更嘈杂的梯度。此外，您可能希望使用动量来防止振荡并加速优化。超参数的影响相对简单。我们的建议是你从小批量、梯度下降和动量开始。一旦你有了一个好的参数集，你就可以使用 Adam 或者其他优化器来优化不同的权重。

![](img/28b9e7e2aedb8ea68a0258f64c77355c.png)

时刻关注亏损曲线。 [CC 下的图片来自](https://creativecommons.org/licenses/by/4.0/)[深度学习讲座](https://www.youtube.com/watch?v=p-_Stl0t3kU&list=PLpOGQvPCDQzvgpD3S0vTy7bJe2pf_yJFj&index=1)的 4.0 。

记住观察损失曲线。如果你的学习速率设置不正确，你在网络的训练中就有麻烦了。对于几乎所有基于梯度的优化器，你必须设置η。所以，我们经常直接在损失曲线中看到，但这是一个简化的视图。因此，我们实际上希望有一个自适应的学习速率，然后逐步用更小的步骤找到最优。正如我们已经讨论过的，你需要调整学习速度。

![](img/6b5c13196421454308b0602ce8e756a0.png)

关于如何提高学习速度的提示。来自[深度学习讲座](https://www.youtube.com/watch?v=p-_Stl0t3kU&list=PLpOGQvPCDQzvgpD3S0vTy7bJe2pf_yJFj&index=1)的 [CC BY 4.0](https://creativecommons.org/licenses/by/4.0/) 下的图片。

现在，学习率衰减是你必须以某种方式设置的另一个超参数。你要避免振荡以及过快的冷却。所以，有几个衰变策略。逐步衰减每 n 个时期，你以某一因子降低学习率，如 0.5，常数值如 0.01，或者当验证误差不再降低时，你降低学习率。在每个时期都有指数衰减，你可以用这个指数函数来控制衰减。还有 1/t 衰减，在时间 t，你基本上用 1 / (1 + *kt* )来缩放初始学习速率。逐步衰减是最常见的，超参数也很容易解释。二阶方法目前在实践中并不常见，因为它们不能很好地扩展。这么多关于学习率和一些相关的超参数。

![](img/3ddc6d52088dc2b7639db5736dc85071.png)

在这个深度学习讲座中，更多令人兴奋的事情即将到来。来自[深度学习讲座](https://www.youtube.com/watch?v=p-_Stl0t3kU&list=PLpOGQvPCDQzvgpD3S0vTy7bJe2pf_yJFj&index=1)的 [CC BY 4.0](https://creativecommons.org/licenses/by/4.0/) 下的图片。

下一次在深度学习中，我们将进一步研究如何调整我们刚刚发现的所有超参数。你会发现这些提示对你自己的实验非常有价值。非常感谢大家的聆听，下节课再见！

如果你喜欢这篇文章，你可以在这里找到更多的文章，或者看看我们的讲座。如果你想在未来了解更多的文章、视频和研究，我也会很感激关注 [YouTube](https://www.youtube.com/c/AndreasMaierTV) 、 [Twitter](https://twitter.com/maier_ak) 、[脸书](https://www.facebook.com/andreas.maier.31337)或 [LinkedIn](https://www.linkedin.com/in/andreas-maier-a6870b1a6/) 。本文以 [Creative Commons 4.0 归属许可](https://creativecommons.org/licenses/by/4.0/deed.de)发布，如果引用，可以转载和修改。

# 参考

[1] M. Aubreville，M. Krappmann，C. Bertram 等，“用于组织学细胞分化的导向空间转换器网络”。载于:ArXiv 电子版(2017 年 7 月)。arXiv: 1707.08525 [cs。简历】。
【2】詹姆斯·伯格斯特拉和约舒阿·本吉奥。“随机搜索超参数优化”。在:j .马赫。学习。第 13 号决议(2012 年 2 月)，第 281-305 页。
【3】让·迪金森·吉本斯和 Subhabrata Chakraborti。“非参数统计推断”。载于:国际统计科学百科全书。斯普林格，2011 年，第 977-979 页。
[4]约舒阿·本吉奥。“深度架构基于梯度训练的实用建议”。《神经网络:交易的诀窍》。斯普林格出版社，2012 年，第 437-478 页。
[5]·张，Samy Bengio，Moritz Hardt 等，“理解深度学习需要反思泛化”。载于:arXiv 预印本 arXiv:1611.03530 (2016)。
[6]鲍里斯·T·波亚克和阿纳托利·B·朱迪斯基。“通过平均加速随机逼近”。摘自:SIAM 控制与优化杂志 30.4 (1992)，第 838-855 页。
【7】普拉吉特·拉马钱德兰，巴雷特·佐夫，和阔克诉勒。“搜索激活功能”。载于:CoRR abs/1710.05941 (2017 年)。arXiv: 1710.05941。
[8] Stefan Steidl，Michael Levit，Anton Batliner 等，“所有事物的衡量标准是人:情感的自动分类和标签间的一致性”。在:过程中。ICASSP 的。电气和电子工程师协会，2005 年 3 月。