# 图形深度学习—第二部分

> 原文：<https://towardsdatascience.com/graph-deep-learning-part-2-c6110d49e63c?source=collection_archive---------51----------------------->

## [FAU 讲座笔记](https://towardsdatascience.com/tagged/fau-lecture-notes)关于深度学习

## 从光谱域到空间域

![](img/0d74f3c55c2f485ddd75fb2c9ae4bad3.png)

FAU 大学的深度学习。下图 [CC BY 4.0](https://creativecommons.org/licenses/by/4.0/) 来自[深度学习讲座](https://www.youtube.com/watch?v=p-_Stl0t3kU&list=PLpOGQvPCDQzvgpD3S0vTy7bJe2pf_yJFj&index=1)

**这些是 FAU 的 YouTube 讲座** [**深度学习**](https://www.youtube.com/watch?v=p-_Stl0t3kU&list=PLpOGQvPCDQzvgpD3S0vTy7bJe2pf_yJFj&index=1) **的讲义。这是与幻灯片匹配的讲座视频&的完整抄本。我们希望，你喜欢这个视频一样多。当然，这份抄本是用深度学习技术在很大程度上自动创建的，只进行了少量的手动修改。** [**自己试试吧！如果您发现错误，请告诉我们！**](http://autoblog.tf.fau.de/)

# 航行

[**上一讲**](/graph-deep-learning-part-1-e9652e5c4681) **/** [**观看本视频**](https://youtu.be/oMOKFi2Lb4c) **/** [**顶级**](/all-you-want-to-know-about-deep-learning-8d68dcffc258) **/** [**下一讲**](/known-operator-learning-part-1-32fc2ea49a9)

![](img/db1f45a2ba10c32968a343124f8c9ce0.png)

图形深度学习和物理模拟很好地结合在一起。使用 [gifify](https://github.com/vvo/gifify) 创建的图像。来源: [YouTube](https://youtu.be/2Bw5f4vYL98) 。

欢迎回到深度学习。所以今天，我们想继续讨论图的卷积。我们将研究第二部分，现在我们看看我们是否必须停留在这个谱域，或者我们是否也可以回到空间域。让我们看看我为你准备了什么。

![](img/b89ae60667a5664ac895783acb24d7ad.png)

通过正确选择多项式矩阵，U 消失。 [CC 下的图片来自](https://creativecommons.org/licenses/by/4.0/)[深度学习讲座](https://www.youtube.com/watch?v=p-_Stl0t3kU&list=PLpOGQvPCDQzvgpD3S0vTy7bJe2pf_yJFj&index=1)的 4.0 。

记住，我们用这个多项式来定义谱域中的卷积。我们已经看到，通过计算拉普拉斯矩阵的特征向量，我们能够找到适当的傅立叶变换，然后给出图形配置的频谱表示。然后，我们可以在谱域中进行卷积，并将其转换回来。这有点昂贵，因为我们必须计算 **U** 。对于 **U，**我们必须对整个对称矩阵进行特征值分解。此外，我们已经看到，我们不能使用快速傅立叶变换的技巧，因为这不一定适用于我们的 **U** 。

那么，我们现在该如何选择我们的 k 和θ，才能摆脱 **U** ？那么，如果我们选择 k 等于 1，θ下标 0 到 2θ，θ下标 1 到-θ，我们得到下面的多项式。因此，我们仍然有这样的配置，我们有在傅立叶空间变换的 **x** ，乘以我们的多项式，表示为矩阵乘以这里的傅立叶逆变换。现在，让我们来看看 **G** 帽子的配置。 **G** hat 其实可以表示为 2 乘以θ乘以**λ**的 0 次方。记住**λ**是对角矩阵。所以，我们把每个元素的 0 次方化。这实际上是一个单位矩阵，我们减去θ乘以**λ**的 1 次方。嗯，这其实只是**λ**。然后，我们可以这样表示我们的完全矩阵 **G** hat。当然，我们可以从左侧和右侧拉我们的 **U** ，这给出了下面的表达式。现在，我们利用θ实际上是一个标量的性质。所以，我们可以把它拉到前面。0 次方的**λ**被抵消，因为这本质上是一个单位矩阵。右边项的**λ**依然存在，但我们也可以把θ拉出来。嗯，UU 转置正好抵消了。这又是一个单位矩阵，我们可以用我们定义的拉普拉斯对称图形。你可以看到我们刚刚找到了它，在我们的方程里。所以，我们也可以用这个来代替。你看现在 **U** 突然没了。因此，我们可以再次取出θ，剩下的就是，我们有两倍的单位矩阵减去图的拉普拉斯对称形式。如果我们现在插入与原始邻接矩阵和度矩阵相关的对称版本的定义，我们可以看到我们仍然可以插入这个定义。然后，其中一个单位矩阵抵消，我们最终得到单位加 **D** 的-0.5 次方乘以 **A** 乘以 **D** 的-0.5 次方。所以，记住 **D** 是对角矩阵。我们可以很容易地反转对角线上的元素，我们也可以对元素求平方根。所以，这完全没问题。这样我们就不会有 **U** 出现在这里。我们可以用图形拉普拉斯矩阵以这种非常好的方式来表达整个图形卷积。

![](img/8f330e3e86aa11803eb54bf20464b378.png)

走向空间域卷积的步骤。 [CC 下的图片来自](https://creativecommons.org/licenses/by/4.0/)[深度学习讲座](https://www.youtube.com/watch?v=p-_Stl0t3kU&list=PLpOGQvPCDQzvgpD3S0vTy7bJe2pf_yJFj&index=1)的 4.0 。

现在让我们再分析一下这个术语。因此，我们可以在左侧看到这个等式，我们看到我们可以在谱域中进行卷积，我们可以将 **G** hat 构造为拉普拉斯滤波器的多项式。然后，我们可以看到，对于特定的选择，k 等于 1，θ下标 0 等于 2θ，θ下标 1 等于-θ。然后，这一项突然只依赖于标量值θ。通过所有这些技巧，我们摆脱了傅立叶变换 **U** 转置。因此，我们突然可以用这种简化的方式来表示图的卷积。

![](img/586f866834cbc431806fc59e6aa15b61.png)

用 GCN 实现图的卷积。 [CC 下的图片来自](https://creativecommons.org/licenses/by/4.0/)[深度学习讲座](https://www.youtube.com/watch?v=p-_Stl0t3kU&list=PLpOGQvPCDQzvgpD3S0vTy7bJe2pf_yJFj&index=1)的 4.0 。

这是基本的图形卷积运算，你可以在参考文献[1]中找到。你可以对标量值这样做，你使用你的度矩阵，并把它插入这里。你用你的邻接矩阵，把它插入这里。然后，你可以针对θ进行优化，以便找到卷积的权重。

![](img/e490d24e1c13dac3e69c1f6fcf72d4af.png)

还有直接的空间变体。来自[深度学习讲座](https://www.youtube.com/watch?v=p-_Stl0t3kU&list=PLpOGQvPCDQzvgpD3S0vTy7bJe2pf_yJFj&index=1)的 4.0CC 下的图片。

好吧，现在的问题是“真的有必要从谱域激发图形卷积吗？”答案是“不”。所以，我们也可以在空间上激发它。

![](img/60abff369b262b71cc267c833a0e2401.png)

绘制卷积图的简单方法。来自[深度学习讲座](https://www.youtube.com/watch?v=p-_Stl0t3kU&list=PLpOGQvPCDQzvgpD3S0vTy7bJe2pf_yJFj&index=1)的 4.0CC 下的图片。

好吧，我们来看下面这个概念。对数学家来说，图是流形，但却是离散的。我们可以离散流形，并使用拉普拉斯矩阵进行频谱卷积。这让我们想到了谱图卷积。但是作为一个计算机科学家，你可以把一个图解释为一组通过边连接的节点和顶点。我们现在需要定义如何通过它的邻居聚集一个顶点的信息。如果我们这样做，我们得到了空间图形卷积。

![](img/ed1031ff0a17ff2bc67ccbd8b740bf15.png)

聚合器是空间图形卷积的关键。 [CC 下的图片来自](https://creativecommons.org/licenses/by/4.0/)[深度学习讲座](https://www.youtube.com/watch?v=p-_Stl0t3kU&list=PLpOGQvPCDQzvgpD3S0vTy7bJe2pf_yJFj&index=1)的 4.0 。

这是怎么做到的呢？[2]中显示的一种方法是 GraphSAGE。在这里，我们本质上定义了一个感兴趣的顶点，并定义了邻居如何对感兴趣的顶点做出贡献。所以从技术上讲，我们在节点 v 和第 k 层使用特征向量来实现这一点。这可以描述为 **h** k 下标 v。因此，对于第零层，这包含输入。这只是您的图表的原始配置。然后，我们需要能够聚合，以便计算下一层。这是通过在前一层上的空间聚合函数来完成的。因此，您使用所有的邻居，并且通常您定义这个邻域，使得连接到所考虑的节点的每个节点都包括在这个邻域中。

![](img/338acb5cbed0a392de1d7ef337c5e7cb.png)

GraphSAGE 算法。来自[深度学习讲座](https://www.youtube.com/watch?v=p-_Stl0t3kU&list=PLpOGQvPCDQzvgpD3S0vTy7bJe2pf_yJFj&index=1)的 4.0CC 下的图片。

这一行把我们带到了 GraphSAGE 算法。在这里，您从图表和输入特征开始。然后，您执行以下算法:您在 **h** 0 处初始化，只需输入图形配置。然后，迭代这些层。你迭代节点。对于每个节点，运行聚合函数，以某种方式计算所有邻居的汇总。然后，结果是一个特定维度的向量，然后你得到聚合向量和向量的当前配置，你把它们连接起来，然后乘以一个权重矩阵。这然后通过非线性运行。最后，你通过激活的数量来缩放。然后在所有层上迭代，最后，您得到输出 **z** ，这是您的图形卷积的结果。

![](img/71e70e97be690776dc7316f4b1b1677b.png)

GraphSAGE 的不同聚合器。 [CC 下的图片来自](https://creativecommons.org/licenses/by/4.0/)[深度学习讲座](https://www.youtube.com/watch?v=p-_Stl0t3kU&list=PLpOGQvPCDQzvgpD3S0vTy7bJe2pf_yJFj&index=1)的 4.0 。

聚合器的概念是开发这种算法的关键，因为在每个节点中，你可能有不同数量的邻居。一个非常简单的聚合器会简单地计算平均值。当然，你也可以使用 GCN 聚集器，这样我们又回到了光谱表示。这样，可以建立空间域和光谱域之间的联系。此外，您可以使用一个池聚合器，然后使用，例如，最大池，或者您使用循环网络，如 LSTM 聚合器。

![](img/9a0ee188e9b9edef2776e050e09b473d.png)

欢迎来到图形深度学习的世界。来自[深度学习讲座](https://www.youtube.com/watch?v=p-_Stl0t3kU&list=PLpOGQvPCDQzvgpD3S0vTy7bJe2pf_yJFj&index=1)的 [CC BY 4.0](https://creativecommons.org/licenses/by/4.0/) 下的图片。

你已经看到有各种各样的聚合器。这也是为什么有这么多不同的图形深度学习方法的原因。你可以把它们细分成某些种类，因为有光谱的，有空间的，还有循环的。所以，这本质上是如何处理图形卷积神经网络的关键。那么，我们到底想做什么呢？你可以用这些算法中的一个，应用到一些网格上。当然，这也可以在非常复杂的网格上完成，我会在下面放几个参考文献，你可以看到什么样的应用可以完成。例如，您可以使用这些方法来处理关于冠状动脉的信息。

![](img/58e09d17d86aa775e7b3e3961a52ac68.png)

在这个深度学习讲座中，更多令人兴奋的事情即将到来。 [CC 下的图片来自](https://creativecommons.org/licenses/by/4.0/)[深度学习讲座](https://www.youtube.com/watch?v=p-_Stl0t3kU&list=PLpOGQvPCDQzvgpD3S0vTy7bJe2pf_yJFj&index=1)的 4.0 。

下次在深度学习中，只剩下几个话题了。我想向你们展示的一件事是，你如何将先前的知识嵌入到深层网络中。这也是一个非常好的想法，因为它允许我们将我们从理论和信号处理中了解的许多东西与我们的深度学习方法相融合。当然，我也有一些参考资料，如果你有时间，请通读一下。他们更详细地阐述了我们在这里提出的想法。我还会在这段视频的描述中加入一些图片参考。非常感谢大家的聆听，下节课再见。拜拜。

![](img/03eb05de228cb5d91bb5d2d94b5c970c.png)

许多更重要的概念在这里被省略了。因此，请继续阅读下面的图形深度学习。使用 [gifify](https://github.com/vvo/gifify) 创建的图像。来源: [YouTube](https://youtu.be/2Bw5f4vYL98) 。

如果你喜欢这篇文章，你可以在这里找到[更多的文章](https://medium.com/@akmaier)，在这里找到更多关于机器学习的教育材料[，或者看看我们的](https://lme.tf.fau.de/teaching/free-deep-learning-resources/)[深度](https://www.youtube.com/watch?v=p-_Stl0t3kU&list=PLpOGQvPCDQzvgpD3S0vTy7bJe2pf_yJFj) [学习](https://www.youtube.com/watch?v=p-_Stl0t3kU&list=PLpOGQvPCDQzvgpD3S0vTy7bJe2pf_yJFj&index=1) [讲座](https://www.youtube.com/watch?v=p-_Stl0t3kU&list=PLpOGQvPCDQzvgpD3S0vTy7bJe2pf_yJFj)。如果你想在未来了解更多的文章、视频和研究，我也会很感激关注 YouTube、Twitter、脸书、LinkedIn 或 T21。本文以 [Creative Commons 4.0 归属许可](https://creativecommons.org/licenses/by/4.0/deed.de)发布，如果引用，可以转载和修改。如果你有兴趣从视频讲座中生成文字记录，试试[自动博客](http://autoblog.tf.fau.de/)。

# 谢谢

非常感谢迈克尔·布朗斯坦对 2018 小姐的精彩介绍！特别感谢 Florian Thamm 准备了这组幻灯片。

# 参考

[1]基普夫，托马斯和马克斯韦林。"图卷积网络的半监督分类."arXiv 预印本 arXiv:1609.02907 (2016)。
[2]汉密尔顿、威尔、之桃·英和朱尔·莱斯科维奇。"大型图上的归纳表示学习."神经信息处理系统进展。2017.
[3]沃尔泰林克、杰尔默·m、蒂姆·莱纳和伊万娜·伊什古姆。"用于心脏 CT 血管造影中冠状动脉分割的图形卷积网络."医学成像图形学习国际研讨会。施普林格，查姆，2019。
[4]吴，，等.图神经网络综述 arXiv 预印本 arXiv:1901.00596 (2019)。
【5】布朗斯坦、迈克尔等在 SIAM Tutorial Portland (2018)举办的讲座《图形和流形上的几何深度学习》

# 图像参考

[a][https://de . serlo . org/mathe/functionen/funktionsbegriff/funktionen-graphen/graph-function](https://de.serlo.org/mathe/funktionen/funktionsbegriff/funktionen-graphen/graph-funktion)【b】[【https://www.nwrfc.noaa.gov/snow/plot_SWE.php?id=AFSW1](https://www.nwrfc.noaa.gov/snow/plot_SWE.php?id=AFSW1)
【c】[https://tennis Bei Olympia . WordPress . com/meilensteine/Steffi-graf/](https://tennisbeiolympia.wordpress.com/meilensteine/steffi-graf/)
【d】[https://www.pinterest.de/pin/624381935818627852/](https://www.pinterest.de/pin/624381935818627852/)
【e】[https://www . ui heregif](https://www.uihere.com/free-cliparts/the-pentagon-pentagram-symbol-regular-polygon-golden-five-pointed-star-2282605)
【I】https://www . researchgate . net/publication/306293638/figure/fig 1/AS:396934507450372 @ 1471647969381/Example-of-centline-extracted-left-and-coronal-artery-tree-mesh-construction . png
【j】[https://www . eurorad . org/sites/default/files/styles/stylesitok=hwX1sbCO](https://www.eurorad.org/sites/default/files/styles/figure_image_teaser_large/public/figure_image/2018-08/0000015888/000006.jpg?itok=hwX1sbCO)