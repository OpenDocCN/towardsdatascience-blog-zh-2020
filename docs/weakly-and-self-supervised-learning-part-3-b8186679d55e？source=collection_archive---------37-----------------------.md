# 弱自我监督学习——第三部分

> 原文：<https://towardsdatascience.com/weakly-and-self-supervised-learning-part-3-b8186679d55e?source=collection_archive---------37----------------------->

## [FAU 讲座笔记](https://towardsdatascience.com/tagged/fau-lecture-notes)关于深度学习

## 自我监督标签

![](img/2131526a0cb268ef27c4ad923814c63b.png)

FAU 大学的深度学习。下图 [CC BY 4.0](https://creativecommons.org/licenses/by/4.0/) 来自[深度学习讲座](https://www.youtube.com/watch?v=p-_Stl0t3kU&list=PLpOGQvPCDQzvgpD3S0vTy7bJe2pf_yJFj&index=1)

**这些是 FAU 的 YouTube 讲座** [**深度学习**](https://www.youtube.com/watch?v=p-_Stl0t3kU&list=PLpOGQvPCDQzvgpD3S0vTy7bJe2pf_yJFj&index=1) **的讲义。这是与幻灯片匹配的讲座视频&的完整抄本。我们希望，你喜欢这个视频一样多。当然，这份抄本是用深度学习技术在很大程度上自动创建的，只进行了少量的手动修改。** [**自己试试吧！如果您发现错误，请告诉我们！**](http://autoblog.tf.fau.de/)

# 航行

[**上一讲**](/weakly-and-self-supervised-learning-part-1-ddfdf8377f1d) **/** [**观看本视频**](https://youtu.be/EqMwbP7Smxg) **/** [**顶级**](/all-you-want-to-know-about-deep-learning-8d68dcffc258) **/** [**下一讲**](/weakly-and-self-supervised-learning-part-4-2fbfd10280b3)

![](img/19109e7eb029067892521848b4c1a6e5.png)

自我监督的人脸模型学习。使用 [gifify](https://github.com/vvo/gifify) 创建的图像。来源: [YouTube](https://youtu.be/h8jWocE_3tI) 。

欢迎回到深度学习！所以今天，我们想开始谈论被称为自我监督学习的想法。我们希望通过自我监督获得标签，并将在接下来的几个视频中研究这个术语的实际含义以及核心思想。

![](img/870f5db3303dc4b98d7bb0042c6b83fe.png)

AI 的革命不会被监督。 [CC 下的图片来自](https://creativecommons.org/licenses/by/4.0/)[深度学习讲座](https://www.youtube.com/watch?v=p-_Stl0t3kU&list=PLpOGQvPCDQzvgpD3S0vTy7bJe2pf_yJFj&index=1)的 4.0 。

这是弱自我监督学习的第三部分。今天，我们实际上开始谈论自我监督学习。有一些关于自我监督学习的观点，你可以把它们分成两部分。你可以说，一部分是如何获得自我监督的标签，另一部分是你对损失进行处理，以便嵌入这些标签。我们有适合自我监督的特殊损失。所以，我们先从定义说起。动机是你可以说，传统上，机器学习领域的人相信，监督当然是产生最佳结果的方法。但是，我们有大量我们需要的标签。所以，你可以很快得出结论，人工智能革命不会被监督。这在 Yann LeCun 的以下声明中非常明显。“人类和动物的大部分学习都是无监督学习。如果智能是一块蛋糕，无监督学习就是蛋糕，有监督学习就是蛋糕上的糖衣，强化学习就是蛋糕上的樱桃。”当然，这是由生物学中的观察以及人类和动物如何学习来证实的。

![](img/323285c6553e5591b6ee951e98c7d42a.png)

自我监督的理念。来自[深度学习讲座](https://www.youtube.com/watch?v=p-_Stl0t3kU&list=PLpOGQvPCDQzvgpD3S0vTy7bJe2pf_yJFj&index=1)的 [CC BY 4.0](https://creativecommons.org/licenses/by/4.0/) 下的图片。

自我监督的想法是，你试图利用你已经掌握的关于你的问题的信息，想出一些替代标签，让你做训练过程。Yann LeCun 在这张幻灯片上的主要观点可以总结如下:你试图从过去预测未来，你也可以从最近的过去预测未来，你从现在预测过去或者从底部预测顶部。此外，一个选项可以是从可见物中预测被遮挡物。你假装有一部分输入你不知道，并预测。这实际上允许您提出一个代理任务。使用代理任务，您已经可以执行培训了。好的一面是你根本不需要任何标签，因为你本质上使用了数据的结构。

![](img/3cade9de9c7f755ed17be2136fb01ae7.png)

Yann LeCun 对自我监督学习的定义。 [CC 下的图片来自](https://creativecommons.org/licenses/by/4.0/)[深度学习讲座](https://www.youtube.com/watch?v=p-_Stl0t3kU&list=PLpOGQvPCDQzvgpD3S0vTy7bJe2pf_yJFj&index=1)的 4.0 。

本质上，自我监督学习是一种无监督的学习方法。但时不时地，你需要清楚地表明，你正在一个已经研究了几十年的领域里做一些新的事情。所以，你可能不会再提到无监督这个术语，Yann LeCun 实际上提出了自我监督学习这个术语。他意识到无人监管是一个复杂而令人困惑的术语。因此，尽管这些想法以前就已经存在，术语自我监督学习已经建立。使用这个术语来专注于一种特定的无监督学习是有意义的。所以，你可以说这是无监督学习的一个子类。它以受监督的方式使用伪任务的借口代理。这实质上意味着你可以使用所有的监督学习方法，并且你有自动生成的标签。然后，它们可以被用来衡量正确性，以创造一个损失，以训练你的重量。其想法是，这对于下游任务是有益的，如检索、监督或半监督分类等。顺便说一下，在这种广义的定义中，你也可以认为像生成对抗网络这样的生成模型也是某种自我监督的学习方法。所以基本上，Yann LeCun 有一个非常好的想法，用一种新的方式来构建这种学习。如果你这样做，这当然非常有帮助，因为你可以清楚地表明你正在做一些新的事情，你不同于许多已经存在很长时间的无监督学习方法。

![](img/5f6dc2562c222e2f6049c7456aa09bf3.png)

不同自我监督任务的概述。来自[深度学习讲座](https://www.youtube.com/watch?v=p-_Stl0t3kU&list=PLpOGQvPCDQzvgpD3S0vTy7bJe2pf_yJFj&index=1)的 [CC BY 4.0](https://creativecommons.org/licenses/by/4.0/) 下的图片。

所以，让我们来看看这些想法。当然，有这些托词任务，您可以使用基于生成的方法。所以，你可以使用 GANs，你可以做像超分辨率方法这样的事情。在那里，您向下采样并尝试预测更高分辨率的图像。你可以做修补方法或着色。当然，这也适用于视频。您可以使用基于上下文的方法。在这里，您尝试解决像拼图游戏或聚类这样的问题。在基于语义标签的方法中，您可以尝试估计移动对象或预测相对深度。此外，还有跨通道方法，您可以尝试使用来自多个通道的信息。你有一个链接的传感器系统，假设你有一个深度相机和一个 RGB 相机。然后，您可以将两者联系起来，并尝试从另一个预测一个。如果你有一个附加的传感器，假设你有一辆车，你正在移动，你有一个 GPS 传感器或任何其他传感系统，它会告诉你你的车是如何移动的，那么你可以尝试从实际的视频序列中预测自我运动。

![](img/3ae7f1fb7bf655790ad72b0338420d77.png)

自我监督的汽车场景分析。使用 [gifify](https://github.com/vvo/gifify) 创建的图像。来源: [YouTube](https://youtu.be/rbZ8ck_1nZk) 。

因此，让我们更详细地研究一下这个问题，看看基于图像的自我监督学习技术，以完善表示学习，这是第一个想法，也是生成性的。例如，您可以在非常容易生成标签的地方对图像进行着色。

![](img/a8c0d7a38e3586f1afbd024689a60ad6.png)

学习颜色。来自[深度学习讲座](https://www.youtube.com/watch?v=p-_Stl0t3kU&list=PLpOGQvPCDQzvgpD3S0vTy7bJe2pf_yJFj&index=1)的 [CC BY 4.0](https://creativecommons.org/licenses/by/4.0/) 下的图片。

从彩色图像开始，计算通道的平均值，得到灰度值图像。然后，你再尝试预测一下原来的颜色。你可以使用一种场景和编码器/解码器的方法来预测正确的颜色图。

![](img/704ed4d95a2d6f4ea5a37ea60b114218.png)

学习图像补丁。 [CC 下的图片来自](https://creativecommons.org/licenses/by/4.0/)[深度学习讲座](https://www.youtube.com/watch?v=p-_Stl0t3kU&list=PLpOGQvPCDQzvgpD3S0vTy7bJe2pf_yJFj&index=1)的 4.0 。

此外，你也可以去修补。你可以遮挡图像的一部分，然后试着预测它们。这实际上导致了这样一个任务，您尝试预测一个完整的图像，然后将实际的完整图像与生成器创建的预测进行比较。例如，你可以在 GAN 类型的损失设置中训练这些东西。我们有一个鉴别器，然后告诉你这是否是一个好的修复结果。

![](img/c0279cb0d7078c141c24276d7984b6c6.png)

学习拼图。 [CC 下的图片来自](https://creativecommons.org/licenses/by/4.0/)[深度学习讲座](https://www.youtube.com/watch?v=p-_Stl0t3kU&list=PLpOGQvPCDQzvgpD3S0vTy7bJe2pf_yJFj&index=1)的 4.0 。

还有关于空间语境的想法。在这里，一个非常常见的方法是解决一个拼图游戏。你取一个小块，或者实际上，你取图像的九个小块，你基本上试图预测你是否有中心小块，比方说，对于猫的脸，你有一个。然后，您希望预测所显示的修补程序的 ID。所以，你放入两张图片，试着预测第二块的正确位置。请注意，这有点棘手，因为有一个简单的解决方案。如果您有持续的边界模式，就会发生这种情况。如果你有连续的纹理，那么在下一个补片中实际的补片很容易被发现，因为纹理是连续的。因此，您应该使用足够大的间隙来解决这个问题。颜色可能也很棘手。您可以使用色差，并通过将绿色和洋红色向灰色移动来预处理图像，或者您可以随机删除两个颜色通道，以避免您只是了解颜色。

![](img/a81ba03bfe7d2dbcab6e8da87c8f2082.png)

学习更难的谜题。来自[深度学习讲座](https://www.youtube.com/watch?v=p-_Stl0t3kU&list=PLpOGQvPCDQzvgpD3S0vTy7bJe2pf_yJFj&index=1)的 [CC BY 4.0](https://creativecommons.org/licenses/by/4.0/) 下的图片。

这是拼图游戏的改进版本。在 jigsaw puzzle++中，想法是你基本上随机化补丁的顺序，并且你试图预测每个补丁的正确位置。现在，最酷的事情是，如果你有 9 块瓷砖，那么你就有 9 块！可能的排列。这是 30 多万。因此，我们可以为这项任务创建大量的标签，你会看到，以正确的方式完成这项任务实际上是非常关键的。所以，不仅仅是你有很多排列。

![](img/8b6030710ced6316dfd999141bcb5bd4.png)

问题的正确选择至关重要。 [CC 下的图片来自](https://creativecommons.org/licenses/by/4.0/)[深度学习讲座](https://www.youtube.com/watch?v=p-_Stl0t3kU&list=PLpOGQvPCDQzvgpD3S0vTy7bJe2pf_yJFj&index=1)的 4.0 。

您还需要确保有一个合适的平均汉明距离。你可以看看你是否真的遵从了这个想法，那么这就有所不同了。如果你有一个太低的平均汉明距离，拼图任务精度不是很高。如果你增加它，拼图任务的准确性也会增加。这种高精度很有可能也会应用到感兴趣的实际任务中。在这里，这是一个检测任务，在 jigsaw 任务精度很高的情况下，您还可以构建一个更好的检测器。

![](img/ba689699bc90639f099147863e948f76.png)

学习形象定位。来自[深度学习讲座](https://www.youtube.com/watch?v=p-_Stl0t3kU&list=PLpOGQvPCDQzvgpD3S0vTy7bJe2pf_yJFj&index=1)的 [CC BY 4.0](https://creativecommons.org/licenses/by/4.0/) 下的图片。

嗯，我们还会对哪些想法感兴趣呢？当然，你可以用旋转做类似的事情。然后，你试着预测图像的正确旋转。这也是一个非常便宜的标签，你可以生成。

![](img/bfaef25ae4263f19a3a3daee1435789c.png)

在扭曲中学习等价。 [CC 下的图片来自](https://creativecommons.org/licenses/by/4.0/)[深度学习讲座](https://www.youtube.com/watch?v=p-_Stl0t3kU&list=PLpOGQvPCDQzvgpD3S0vTy7bJe2pf_yJFj&index=1)的 4.0 。

让我们看一下上下文相似性。这里的想法是，你想弄清楚这个图像是来自相同还是不同的上下文。所以，你可以选择一个输入补丁，然后扩大它。您可以使用不同的增强方式，如颜色对比度的变化、轻微的移动和一般的像素转换:您还可以添加噪声，这基本上为每个补丁提供了大量其他补丁，它们应该显示相同的内容。现在，您可以用其他补丁重复这一过程，这样您就可以构建一个大型数据库。有了这些补丁，你就可以训练它是否是同一个补丁，然后你可以区分这几个好的类，并以类似的方式训练你的系统。

![](img/82345901ede06f3b5507e161c4cae8ed.png)

聚类作为标签。来自[深度学习讲座](https://www.youtube.com/watch?v=p-_Stl0t3kU&list=PLpOGQvPCDQzvgpD3S0vTy7bJe2pf_yJFj&index=1)的 4.0CC 下的图片。

另一种方法是使用集群。这是我的同事文森特·克里斯特林的作品。他感兴趣的是为作者识别构建更好的特征。所以，他从检测关键点开始。然后关键点允许你提取补丁。同时，如果使用像 SIFT 这样的算法检测到关键点，则这些关键点会带有一个特征描述符。在未来的描述符上，您可以执行聚类，您得到的聚类可能已经是相当好的训练样本。因此，您可以使用 cluster-ID 来训练一个 ResNet 来预测相应的补丁。这样，您可以使用完全无标签的数据集，进行聚类，生成伪标签，并训练您的系统。文森特已经表明，这实际上给了相当多的性能，以改善表征学习。

![](img/d1fb8e5bb77b99fcd5ddfd20081a2fa3.png)

迭代聚类学习。 [CC 下的图片来自](https://creativecommons.org/licenses/by/4.0/)[深度学习讲座](https://www.youtube.com/watch?v=p-_Stl0t3kU&list=PLpOGQvPCDQzvgpD3S0vTy7bJe2pf_yJFj&index=1)的 4.0 。

这个想法后来得到了进一步发展。你可以交替进行。这个想法被称为 DeepCluster。现在，你接受一些输入，你有一个网络，然后你基本上从一个未经训练的网络开始。您可以对生成的特征进行聚类，通过聚类，例如简单的 k-means，您可以生成伪标签，从而允许反向传播和表示学习的训练。现在，当然，如果你从随机初始化开始，聚类可能不是很好。因此，您希望在分类和聚类之间交替进行。然后，这也允许你建立一个非常强大的卷积神经网络。琐碎的解决方案也有一些你想避免的问题。因此，您希望重新分配空的聚类，当然，您可以使用一些技巧，例如通过分配给它的聚类大小的倒数来对输入的贡献进行加权。

![](img/0da255a6a129059bd0e9970cab4bd244.png)

具有最佳运输的集群。来自[深度学习讲座](https://www.youtube.com/watch?v=p-_Stl0t3kU&list=PLpOGQvPCDQzvgpD3S0vTy7bJe2pf_yJFj&index=1)的 [CC BY 4.0](https://creativecommons.org/licenses/by/4.0/) 下的图片。

你甚至可以进一步发展这个想法。这导致最佳运输的自我标记[24]。在这里，他们基本上进一步发展了 DeepCluster。他们没有使用聚类，而是使用 Sinkhorn-Knopp 算法来确定伪标签。这里的想法是你试图预测最佳运输。这里，我们可以看到一个最优运输问题的例子。假设，您在仓库 A 和仓库 b 中有供应品。它们各有 25 台笔记本电脑，您在商店 1 和商店 2 中各有 25 台笔记本电脑的需求。然后，您可以看到您想要将这些笔记本电脑运送到各自的商店。当然，你从仓库 A 拿走最近的一台和所有的笔记本电脑，在这种情况下，去商店 1，仓库 B 的所有笔记本电脑去商店 2。

![](img/b4d7e61b90d16761ec78461ff8d8d693.png)

聚类方法的比较。 [CC 下的图片来自](https://creativecommons.org/licenses/by/4.0/)[深度学习讲座](https://www.youtube.com/watch?v=p-_Stl0t3kU&list=PLpOGQvPCDQzvgpD3S0vTy7bJe2pf_yJFj&index=1)的 4.0 。

这个算法的好处是，你可以找到它的线性版本。所以，你可以用线性代数来表达所有这些，这意味着你也可以把它嵌入到神经网络中。如果您将 DeepCluser 与最优传输进行比较，那么您可能需要记住，如果您没有单独的聚类损失，这可能会导致退化的解决方案。此外，请记住，聚类方法会最小化网络也试图优化的相同交叉熵损失。

![](img/4cfadf1e84e83ad7b31e36fc8715c675.png)

综合和多任务学习也非常有用。来自[深度学习讲座](https://www.youtube.com/watch?v=p-_Stl0t3kU&list=PLpOGQvPCDQzvgpD3S0vTy7bJe2pf_yJFj&index=1)的 [CC BY 4.0](https://creativecommons.org/licenses/by/4.0/) 下的图片。

嗯，还有几个主意。我们知道这些食谱就像多任务学习一样。也可以做多任务学习，进行自我监督学习。这里的一个例子是你使用合成图像。所以，你有一些合成图像，你可以生成深度，表面法线，或者轮廓。然后，你可以使用这些作为标签，以训练你的网络，并产生一个良好的表现。此外，您还可以在一种 GAN 设置中最小化真实数据和合成数据之间的特征空间域差异。这也导致了非常好的表征学习。

![](img/bc69128c15143655cc8f9711b6bf8ec4.png)

在这个深度学习讲座中，更多令人兴奋的事情即将到来。 [CC 下的图片来自](https://creativecommons.org/licenses/by/4.0/)[深度学习讲座](https://www.youtube.com/watch?v=p-_Stl0t3kU&list=PLpOGQvPCDQzvgpD3S0vTy7bJe2pf_yJFj&index=1)的 4.0 。

下一次，我们想谈谈如何处理这些损失，并使它们更适合自我监督的学习任务。我们将特别看到，对比损失对此非常有用。非常感谢大家的收听，下期视频再见。拜拜。

![](img/6cbe379aa95eb1f80824e988dc4f05e6.png)

更多的自我监督的人脸建模。使用 [gifify](https://github.com/vvo/gifify) 创建的图像。来源: [YouTube](https://youtu.be/h8jWocE_3tI) 。

如果你喜欢这篇文章，你可以在这里找到更多的文章，更多关于机器学习的教育材料，或者看看我们的深度学习讲座。如果你想在未来了解更多的文章、视频和研究，我也会很感激关注 [YouTube](https://www.youtube.com/c/AndreasMaierTV) 、 [Twitter](https://twitter.com/maier_ak) 、[脸书](https://www.facebook.com/andreas.maier.31337)或 [LinkedIn](https://www.linkedin.com/in/andreas-maier-a6870b1a6/) 。本文以 [Creative Commons 4.0 归属许可](https://creativecommons.org/licenses/by/4.0/deed.de)发布，如果引用，可以转载和修改。如果你有兴趣从视频讲座中生成文字记录，试试[自动博客](http://autoblog.tf.fau.de/)。

# 参考

[1] Özgün Çiçek, Ahmed Abdulkadir, Soeren S Lienkamp, et al. “3d u-net: learning dense volumetric segmentation from sparse annotation”. In: MICCAI. Springer. 2016, pp. 424–432.
[2] Waleed Abdulla. Mask R-CNN for object detection and instance segmentation on Keras and TensorFlow. Accessed: 27.01.2020\. 2017.
[3] Olga Russakovsky, Amy L. Bearman, Vittorio Ferrari, et al. “What’s the point: Semantic segmentation with point supervision”. In: CoRR abs/1506.02106 (2015). arXiv: 1506.02106.
[4] Marius Cordts, Mohamed Omran, Sebastian Ramos, et al. “The Cityscapes Dataset for Semantic Urban Scene Understanding”. In: CoRR abs/1604.01685 (2016). arXiv: 1604.01685.
[5] Richard O. Duda, Peter E. Hart, and David G. Stork. Pattern classification. 2nd ed. New York: Wiley-Interscience, Nov. 2000.
[6] Anna Khoreva, Rodrigo Benenson, Jan Hosang, et al. “Simple Does It: Weakly Supervised Instance and Semantic Segmentation”. In: arXiv preprint arXiv:1603.07485 (2016).
[7] Kaiming He, Georgia Gkioxari, Piotr Dollár, et al. “Mask R-CNN”. In: CoRR abs/1703.06870 (2017). arXiv: 1703.06870.
[8] Sangheum Hwang and Hyo-Eun Kim. “Self-Transfer Learning for Weakly Supervised Lesion Localization”. In: MICCAI. Springer. 2016, pp. 239–246.
[9] Maxime Oquab, Léon Bottou, Ivan Laptev, et al. “Is object localization for free? weakly-supervised learning with convolutional neural networks”. In: Proc. CVPR. 2015, pp. 685–694.
[10] Alexander Kolesnikov and Christoph H. Lampert. “Seed, Expand and Constrain: Three Principles for Weakly-Supervised Image Segmentation”. In: CoRR abs/1603.06098 (2016). arXiv: 1603.06098.
[11] Tsung-Yi Lin, Michael Maire, Serge J. Belongie, et al. “Microsoft COCO: Common Objects in Context”. In: CoRR abs/1405.0312 (2014). arXiv: 1405.0312.
[12] Ramprasaath R. Selvaraju, Abhishek Das, Ramakrishna Vedantam, et al. “Grad-CAM: Why did you say that? Visual Explanations from Deep Networks via Gradient-based Localization”. In: CoRR abs/1610.02391 (2016). arXiv: 1610.02391.
[13] K. Simonyan, A. Vedaldi, and A. Zisserman. “Deep Inside Convolutional Networks: Visualising Image Classification Models and Saliency Maps”. In: Proc. ICLR (workshop track). 2014.
[14] Bolei Zhou, Aditya Khosla, Agata Lapedriza, et al. “Learning deep features for discriminative localization”. In: Proc. CVPR. 2016, pp. 2921–2929.
[15] Longlong Jing and Yingli Tian. “Self-supervised Visual Feature Learning with Deep Neural Networks: A Survey”. In: arXiv e-prints, arXiv:1902.06162 (Feb. 2019). arXiv: 1902.06162 [cs.CV].
[16] D. Pathak, P. Krähenbühl, J. Donahue, et al. “Context Encoders: Feature Learning by Inpainting”. In: 2016 IEEE Conference on Computer Vision and Pattern Recognition (CVPR). 2016, pp. 2536–2544.
[17] C. Doersch, A. Gupta, and A. A. Efros. “Unsupervised Visual Representation Learning by Context Prediction”. In: 2015 IEEE International Conference on Computer Vision (ICCV). Dec. 2015, pp. 1422–1430.
[18] Mehdi Noroozi and Paolo Favaro. “Unsupervised Learning of Visual Representations by Solving Jigsaw Puzzles”. In: Computer Vision — ECCV 2016\. Cham: Springer International Publishing, 2016, pp. 69–84.
[19] Spyros Gidaris, Praveer Singh, and Nikos Komodakis. “Unsupervised Representation Learning by Predicting Image Rotations”. In: International Conference on Learning Representations. 2018.
[20] Mathilde Caron, Piotr Bojanowski, Armand Joulin, et al. “Deep Clustering for Unsupervised Learning of Visual Features”. In: Computer Vision — ECCV 2018\. Cham: Springer International Publishing, 2018, pp. 139–156\. A.
[21] A. Dosovitskiy, P. Fischer, J. T. Springenberg, et al. “Discriminative Unsupervised Feature Learning with Exemplar Convolutional Neural Networks”. In: IEEE Transactions on Pattern Analysis and Machine Intelligence 38.9 (Sept. 2016), pp. 1734–1747.
[22] V. Christlein, M. Gropp, S. Fiel, et al. “Unsupervised Feature Learning for Writer Identification and Writer Retrieval”. In: 2017 14th IAPR International Conference on Document Analysis and Recognition Vol. 01\. Nov. 2017, pp. 991–997.
[23] Z. Ren and Y. J. Lee. “Cross-Domain Self-Supervised Multi-task Feature Learning Using Synthetic Imagery”. In: 2018 IEEE/CVF Conference on Computer Vision and Pattern Recognition. June 2018, pp. 762–771.
[24] Asano YM., Rupprecht C., and Vedaldi A. “Self-labelling via simultaneous clustering and representation learning”. In: International Conference on Learning Representations. 2020.
[25] Ben Poole, Sherjil Ozair, Aaron Van Den Oord, et al. “On Variational Bounds of Mutual Information”. In: Proceedings of the 36th International Conference on Machine Learning. Vol. 97\. Proceedings of Machine Learning Research. Long Beach, California, USA: PMLR, Sept. 2019, pp. 5171–5180.
[26] R Devon Hjelm, Alex Fedorov, Samuel Lavoie-Marchildon, et al. “Learning deep representations by mutual information estimation and maximization”. In: International Conference on Learning Representations. 2019.
[27] Aaron van den Oord, Yazhe Li, and Oriol Vinyals. “Representation Learning with Contrastive Predictive Coding”. In: arXiv e-prints, arXiv:1807.03748 (July 2018). arXiv: 1807.03748 [cs.LG].
[28] Philip Bachman, R Devon Hjelm, and William Buchwalter. “Learning Representations by Maximizing Mutual Information Across Views”. In: Advances in Neural Information Processing Systems 32\. Curran Associates, Inc., 2019, pp. 15535–15545.
[29] Yonglong Tian, Dilip Krishnan, and Phillip Isola. “Contrastive Multiview Coding”. In: arXiv e-prints, arXiv:1906.05849 (June 2019), arXiv:1906.05849\. arXiv: 1906.05849 [cs.CV].
[30] Kaiming He, Haoqi Fan, Yuxin Wu, et al. “Momentum Contrast for Unsupervised Visual Representation Learning”. In: arXiv e-prints, arXiv:1911.05722 (Nov. 2019). arXiv: 1911.05722 [cs.CV].
[31] Ting Chen, Simon Kornblith, Mohammad Norouzi, et al. “A Simple Framework for Contrastive Learning of Visual Representations”. In: arXiv e-prints, arXiv:2002.05709 (Feb. 2020), arXiv:2002.05709\. arXiv: 2002.05709 [cs.LG].
[32] Ishan Misra and Laurens van der Maaten. “Self-Supervised Learning of Pretext-Invariant Representations”. In: arXiv e-prints, arXiv:1912.01991 (Dec. 2019). arXiv: 1912.01991 [cs.CV].
33] Prannay Khosla, Piotr Teterwak, Chen Wang, et al. “Supervised Contrastive Learning”. In: arXiv e-prints, arXiv:2004.11362 (Apr. 2020). arXiv: 2004.11362 [cs.LG].
[34] Jean-Bastien Grill, Florian Strub, Florent Altché, et al. “Bootstrap Your Own Latent: A New Approach to Self-Supervised Learning”. In: arXiv e-prints, arXiv:2006.07733 (June 2020), arXiv:2006.07733\. arXiv: 2006.07733 [cs.LG].
[35] Tongzhou Wang and Phillip Isola. “Understanding Contrastive Representation Learning through Alignment and Uniformity on the Hypersphere”. In: arXiv e-prints, arXiv:2005.10242 (May 2020), arXiv:2005.10242\. arXiv: 2005.10242 [cs.LG].
[36] Junnan Li, Pan Zhou, Caiming Xiong, et al. “Prototypical Contrastive Learning of Unsupervised Representations”. In: arXiv e-prints, arXiv:2005.04966 (May 2020), arXiv:2005.04966\. arXiv: 2005.04966 [cs.CV].