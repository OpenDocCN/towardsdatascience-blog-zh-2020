# 生成式和判别式机器学习模型有什么区别？

> 原文：<https://towardsdatascience.com/the-insiders-guide-to-generative-and-discriminative-machine-learning-models-34d5791d53d3?source=collection_archive---------29----------------------->

## 数据科学，机器学习

## **生成型**和**鉴别型**是机器学习模型的类别

![](img/bf927251f8ef75dbdfa7c4e701ad15e0.png)

照片由[摄影爱好](https://unsplash.com/@photoshobby?utm_source=medium&utm_medium=referral)在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄

在本文中，我们将看看生成模型和判别模型之间的区别，它们是如何对比的，以及它们之间的区别。

# **生成模型和判别模型之间的区别是什么，它们如何对比，以及它们之间的区别是什么？**

**判别机器学习**是在可能的输出选择中识别钻机输出。给定一些数据，通过学习参数来完成。最大化 P(X，Y)的联合概率。

**分类**又称为**判别建模**。这往往是在理由；模型必须跨类分离输入变量的实例。它必须挑选或调用给定实例属于哪个类。

**无监督模型**总结输入变量的分布。此外，能够习惯于在输入分布中创建或生成新的实例。因此，这些不同的模型被视为**生成模型**。

一个变量可能具有已知的数据分布，如**高斯分布**。

生成模型也能够总结数据分布。这用于生成符合输入变量分布的新变量。

创成式设置中的简单模型需要的信息更少。然后是一个复杂的区别性设置，反之亦然。

沿着这些思路，判别模型在条件预测方面胜过生成模型。同样，判别模型应该比生成模型更规范化。

# **第一个例子**

![](img/00386992672abc8a18e62486919d5d13.png)

照片由[陈茂三潭](https://unsplash.com/@tranmautritam?utm_source=medium&utm_medium=referral)在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄

例如，有两个孩子，托尼和马克。两人都去了宠物店，以确定猫和狗的区别。两者都特别注意颜色、大小、眼睛颜色、毛发大小、声音，这些都是宠物的特征。

两张照片一张在猫中间，一张在狗中间，马克问哪一张是哪一张。马克写下了几个条件。如果声音像猫叫，眼睛是蓝色或绿色。

它有棕色或黑色的条纹，那么这种动物可能是猫。

由于他的简单规则，他发现哪一个是猫，哪一个可能是狗。

现在不是给托尼看两张照片，而是两张白纸，让他画一只猫和一只狗的样子。托尼画了这幅画。

现在，给定任何一张照片，托尼也能分辨出哪一张可能是猫。哪一个可能是一只狗支持了他创作的图画。对于检测任务来说，绘图是一项耗时的任务，哪一个可能是猫。

但是，如果只有一些狗和猫似乎托尼和马克意味着低训练数据。在这种情况下，如果一张照片上的棕色狗有蓝眼睛的条纹。

马克有可能会把它标为一只猫。而托尼有她的画，他可以更好地发现这张照片是一只狗。

如果托尼多听一些像特写这样的东西，就会创作出更好的素描。但是，如果更多的例子显示使用猫和狗的数据集，马克会比托尼更好。

马克在观察时一丝不苟。假设你让他听更多的特色。它将创建更复杂的规则，称为过度拟合。因此，找到一只猫和一只狗的机会会增加，但托尼不会这样。

如果在参观宠物店之前，他们没有被告知。只有两种动物意味着没有标记数据。

马克会彻底失败，因为他不知道要找什么，而托尼却能画出草图。这是一个巨大的优势，有时被称为**半监督**。

这表明，马克是鉴别性的，托尼是生成性的。

# **再比如**

![](img/04ffc6672011919335bcd5dd916fcf91.png)

照片由[晨酿](https://unsplash.com/@morningbrew?utm_source=medium&utm_medium=referral)在[破浪](https://unsplash.com?utm_source=medium&utm_medium=referral)

将语音分类到一个语言模型。

确定语言模型差异的判别方法。而不用学习语言和对语言进行分类。

生成方法意味着学习每一种语言。所以用你学到的知识来分类。

# **生成型和判别型模型的数学方程式是什么？**

![](img/20e3c2dc876225608eb4f36cf06f29a3.png)

照片由 [Antoine Dautry](https://unsplash.com/@antoine1003?utm_source=medium&utm_medium=referral) 在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄

鉴别式机器学习实际上是在训练一个模型。在可能的输出选择中区分正确的输出。这是通过学习最大化条件概率 P(Y|X)的模型参数来完成的。

生成机器学习是训练一个模型来学习参数，最大化 P(X，Y)的联合概率。

通常在概率模型中以分解形式 P(Y)学习，P(X|Y)在大多数情况下被简化。假设条件独立 P(Y)，P(X|Y)。

# **有哪些不同种类的判别型和生成型机器学习算法？**

![](img/2b838887fcb1c1bce0b289cda106000f.png)

由[马库斯·斯皮斯克](https://unsplash.com/@markusspiske?utm_source=medium&utm_medium=referral)在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄的照片

**不同型号**

*   逻辑回归
*   随机森林
*   支持向量机(SVM)
*   传统神经网络
*   最近邻

**创成式模型**

*   隐马尔可夫模型(HMM)
*   朴素贝叶斯
*   贝叶斯网络
*   高斯混合模型(GMM)

# **在实施之前，应该问以下哪些问题？**

![](img/e1c84e8f2ea2f495512c27a618e2ae6a.png)

乔恩·泰森在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上的照片

*   哪个模型训练需要的数据少？
*   哪个模型可以生成数据？
*   什么时候应该使用这种模型？
*   哪个模型对极值更敏感？
*   哪个模型更容易过拟合？
*   哪个模型可以用更少的时间训练？
*   哪个模型学习条件概率？
*   在不确定的情况下，哪种模型更好？
*   当特征有关系时，哪个模型更好？
*   当需要解释性模型时，哪种模型更好？
*   在优化所需分类精度时，哪种模型更好？
*   标签数据不可用时，哪种模型更好？
*   标签数据可用时，哪个模型更好？
*   哪种模式进行起来简单快捷？

# **生成式和判别式在深度学习中是如何工作的？**

![](img/ff1e22c666bf9f608302bda2191477dd.png)

照片由[照片爱好](https://unsplash.com/@photoshobby?utm_source=medium&utm_medium=referral)在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄

**在生成对抗网络** **【甘】**中，生成器和鉴别器一起训练。生成器生成一批样本，这些样本与真实数据集一起提供给鉴别器进行分类。

# 区分性量词的缺点是什么？

![](img/2e833c6986b360142fef63e33319e384.png)

由[内森·杜姆劳](https://unsplash.com/@nate_dumlao?utm_source=medium&utm_medium=referral)在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄的照片

它缺乏生成性的优雅，如先验、结构、不确定性。感觉就像黑匣子，变量之间的关系似乎不是显而易见的。

# **结论**

![](img/7e2410a9a1631030f121427d7a0dda7d.png)

由[奥斯汀·迪斯特尔](https://unsplash.com/@austindistel?utm_source=medium&utm_medium=referral)在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄的照片

生成方法和判别方法是两种广泛的方法。生成包括建模和判别求解分类。生成模型更优雅，更有解释力。

模型的丰富性并不总是好的一面。拟合更多的参数需要更长的时间、更多的空间和更多的计算。鉴别模型比生成模型更需要被正则化。

生成性设置中的简单模型比区别性设置中的高级模型需要更少的数据。

*现在，把你的想法放在****Twitter*******Linkedin****，以及****Github****！！**

****同意*** *还是* ***不同意*** *与绍拉夫·辛拉的观点和例子？想告诉我们你的故事吗？**

**他对建设性的反馈持开放态度——如果您对此分析有后续想法，请在下面的* ***评论*** *或伸出手来！！**

**推文*[***@ SauravSingla _ 08***](https://twitter.com/SAURAVSINGLA_08)*，评论*[***Saurav _ Singla***](http://www.linkedin.com/in/saurav-singla-5b412320)*，还有明星*[***SauravSingla***](https://github.com/sauravsingla)*马上！**