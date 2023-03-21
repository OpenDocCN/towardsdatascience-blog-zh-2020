# 常见实践—第 2 部分

> 原文：<https://towardsdatascience.com/common-practices-part-2-23dad51e2aae?source=collection_archive---------44----------------------->

## [FAU 讲座笔记](https://towardsdatascience.com/tagged/fau-lecture-notes)关于深度学习

## 超参数和集合

![](img/446e9acfa0ebffc80b8de96ef6c45410.png)

FAU 大学的深度学习。下图 [CC BY 4.0](https://creativecommons.org/licenses/by/4.0/) 来自[深度学习讲座](https://www.youtube.com/watch?v=p-_Stl0t3kU&list=PLpOGQvPCDQzvgpD3S0vTy7bJe2pf_yJFj&index=1)

**这些是 FAU 的 YouTube 讲座** [**深度学习**](https://www.youtube.com/watch?v=p-_Stl0t3kU&list=PLpOGQvPCDQzvgpD3S0vTy7bJe2pf_yJFj&index=1) **的讲义。这是与幻灯片匹配的讲座视频&的完整抄本。我们希望，你喜欢这个视频一样多。当然，这份抄本是用深度学习技术在很大程度上自动创建的，只进行了少量的手动修改。如果你发现了错误，请告诉我们！**

# 航行

[**上一讲**](/common-practices-part-1-6a7c1b2eb6d4) **/** [**观看本视频**](https://youtu.be/APqhOI6TUyI) **/** [**顶级**](/all-you-want-to-know-about-deep-learning-8d68dcffc258) **/** [**下一讲**](/common-practices-part-3-f4853b0ac977)

欢迎大家来深度学习！因此，今天我们想进一步了解常见实践，特别是在本视频中，我们想讨论架构选择和超参数优化。

![](img/21d18c792ca256e49500885e77e3596c.png)

记住:测试数据还在保险库中！ [CC 下的图片来自](https://creativecommons.org/licenses/by/4.0/)[深度学习讲座](https://www.youtube.com/watch?v=p-_Stl0t3kU&list=PLpOGQvPCDQzvgpD3S0vTy7bJe2pf_yJFj&index=1)的 4.0 。

请记住，测试数据仍在保险库中。我们不碰它。然而，我们需要以某种方式设置我们的超参数，你已经看到有大量的超参数。

![](img/6aac3f9115a1ab97d93dbf04c0acbd55.png)

许多超参数与我们的训练过程相关。 [CC 下的图片来自](https://creativecommons.org/licenses/by/4.0/)[深度学习讲座](https://www.youtube.com/watch?v=p-_Stl0t3kU&list=PLpOGQvPCDQzvgpD3S0vTy7bJe2pf_yJFj&index=1)的 4.0 。

您必须选择架构、层数、每层的节点数、激活函数，然后您拥有优化中的所有参数:初始化、损失函数等等。优化器仍然有选项，如梯度下降的类型、动量、学习率衰减和批量大小。在正则化中，有不同的正则化 L2 和 L1 损失，批量归一化，辍学，等等。你想以某种方式算出这些不同种类的程序的所有参数。

![](img/c9dd2a6ea3453d8f81f0ac0d3e10d8f3.png)

架构的选择对您的系统至关重要。来自[深度学习讲座](https://www.youtube.com/watch?v=p-_Stl0t3kU&list=PLpOGQvPCDQzvgpD3S0vTy7bJe2pf_yJFj&index=1)的 [CC BY 4.0](https://creativecommons.org/licenses/by/4.0/) 下的图片。

现在，让我们选择一个架构和损失函数。第一步是考虑问题和数据。特征看起来像什么？你期望什么样的空间相关性？什么样的数据增强有意义？课程将如何分配？关于目标应用程序，什么是重要的？然后，你从简单的架构和损失函数开始，当然，你做你的研究。首先尝试知名车型。它们正在被出版，并且有如此多的论文在那里。因此，没有必要事事亲力亲为。在图书馆呆一天可以节省数小时、数周甚至数月的实验时间。做研究。它真的会节省你的时间。通常他们只是不发表论文，但在非常好的论文中，不仅仅是科学结果，他们还分享源代码，有时甚至是数据。试着找到那些文件。这对你自己的实验有很大帮助。因此，您可能需要改变架构，使其适应您在文献中发现的问题。如果你改变了什么，找到好的理由为什么这是一个合适的改变。有相当多的论文似乎将随机变化引入到架构中。[后来，事实证明，他们所做的观察基本上是随机的](https://arxiv.org/abs/1809.10486)，他们只是运气好，或者在自己的数据上做了足够多的实验，以获得改进。通常，也有一个合理的理由来解释为什么特定的变化会提高性能。

![](img/7f3f684274e79fe8f4da2648a8d3bdb6.png)

超参数搜索的搜索策略。 [CC 下的图片来自](https://creativecommons.org/licenses/by/4.0/)[深度学习讲座](https://www.youtube.com/watch?v=p-_Stl0t3kU&list=PLpOGQvPCDQzvgpD3S0vTy7bJe2pf_yJFj&index=1)的 4.0 。

接下来，您需要进行超参数搜索。所以你记得学习率衰减，正规化，辍学，等等。这些都要调音。尽管如此，网络可能需要几天或几周的时间来训练，你必须搜索这些超参数。因此，我们建议使用对数刻度。例如对于η，这里是 0.1，0.01 和 0.001。你可以考虑网格搜索或随机搜索。所以在网格搜索中，你会有相等的距离步长，如果你看参考文献[2]，他们已经表明随机搜索比网格搜索有优势。首先，它更容易实现，其次，它对影响很大的参数有更好的探索。所以，你可能想看看，然后相应地调整你的策略。所以超参数是高度相互依赖的。您可能希望使用从粗到细的搜索。你在开始时在一个非常粗糙的尺度上进行优化，然后使它变得更精细。你可以只训练网络几个时期，然后将所有的超参数带入合理的范围。然后，您可以使用随机和网格搜索进行细化。

![](img/768fba34880610beb5a8fba1ee2b0876.png)

集成旨在融合多个独立的分类器。来自[深度学习讲座](https://www.youtube.com/watch?v=p-_Stl0t3kU&list=PLpOGQvPCDQzvgpD3S0vTy7bJe2pf_yJFj&index=1)的 [CC BY 4.0](https://creativecommons.org/licenses/by/4.0/) 下的图片。

一个非常常见的可以给你一点点性能提升的技术是集成。这也能真正帮助你获得你仍然需要的额外的一点点性能。到目前为止，我们只考虑了单个分类器。集成的想法是使用许多这样的分类器。如果我们假设 *N* 个分类器是独立的，执行正确预测的概率将是 1 — p。现在，看到 *k* 个错误的概率是 *N* 选择 *k* 乘以 *p* 到 *k* 的幂次(1 — *p* 到*的幂次(N* — *k* )。这是一个二项分布。所以，多数意为 *k* > *N* /2 错的概率是 *N* 选择 *k* 乘以 *p* 到 *k* 乘以(1 — *p* )的幂( *N* — *k* )之和。

![](img/3181aae5d55167bcb4100cf93c77a601.png)

分类器的集合比单个分类器更强。 [CC 下的图片来自](https://creativecommons.org/licenses/by/4.0/)[深度学习讲座](https://www.youtube.com/watch?v=p-_Stl0t3kU&list=PLpOGQvPCDQzvgpD3S0vTy7bJe2pf_yJFj&index=1)的 4.0 。

所以，我们在下面的图中想象一下。在这个图表中，你可以看到如果我采用更多的弱分类器，我们会变得更强。让我们把他们出错的概率设为 0.42。现在，我们可以计算这个二项分布。在这里，你可以看到，如果我选择 20，我得到的概率约为 14%,多数是错误的。如果我选择 N=32，我得到小于 10%的概率多数是错误的。如果我选择 70，超过 95%的情况下大多数将是正确的。因此，你可以看到，对于较大的 n，这个概率是单调递减的，如果 n 接近无穷大，那么精度将趋向于 1。这里最大的问题当然是独立性。因此，通常情况下，我们会遇到从相同的数据中生成独立分类器的问题。所以，如果我们有足够的数据，我们就可以训练许多独立的弱分类器。那么，我们如何把它作为一个概念来实现呢？我们必须以某种方式产生 N 个独立的分类器或回归器，然后我们通过多数或平均来组合预测。我们如何实际生产这样的组件？

![](img/7942dd2e78dbefc6aeb724b5211af9a1.png)

不同的局部最小值产生不同的模型。 [CC 下的图片来自](https://creativecommons.org/licenses/by/4.0/)[深度学习讲座](https://www.youtube.com/watch?v=p-_Stl0t3kU&list=PLpOGQvPCDQzvgpD3S0vTy7bJe2pf_yJFj&index=1)的 4.0 。

嗯，你可以选择不同的型号。在这个例子中，我们看到了一个非凸函数。显然，它们有不同的局部极小值。因此，不同的局部最小值会导致不同的模型，然后我们可以将它们结合起来。此外，你可以尝试一个循环学习率，然后随着学习率上下波动，以避开某些局部极小值。这样，您就可以尝试找到不同的局部最小值，并存储它们以进行组合。

![](img/f8c40bf73fcb37a563781b9905b8dfe9.png)

创建不同的或多或少独立的分类器或回归器的一些想法。来自[深度学习讲座](https://www.youtube.com/watch?v=p-_Stl0t3kU&list=PLpOGQvPCDQzvgpD3S0vTy7bJe2pf_yJFj&index=1)的 [CC BY 4.0](https://creativecommons.org/licenses/by/4.0/) 下的图片。

这样，您还可以在优化的不同点提取不同的模型检查点。稍后，您可以重新使用它们进行组装。此外，权重的移动平均 **w** 可以生成新模型。你甚至可以走这么远，结合不同的方法。所以，我们仍然有传统机器学习方法的完整目录，你也可以训练它们，然后将它们与你的新深度学习模型相结合。通常，如果您只需要一点点，这是一个简单的性能提升。顺便说一下，这也是最终帮助人们打破网飞挑战的想法。前两队差点打破挑战，他们组队训练合奏。这样他们一起打破了挑战。

![](img/00c8660756ee7347163c02fa77b13507.png)

在这个深度学习讲座中，更多令人兴奋的事情即将到来。 [CC 下的图片来自](https://creativecommons.org/licenses/by/4.0/)[深度学习讲座](https://www.youtube.com/watch?v=p-_Stl0t3kU&list=PLpOGQvPCDQzvgpD3S0vTy7bJe2pf_yJFj&index=1)的 4.0 。

因此，下一次在深度学习中，我们将讨论课堂失衡，这是一个非常常见的问题，以及如何在您的培训过程中处理这一问题。非常感谢您的收听，再见！

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