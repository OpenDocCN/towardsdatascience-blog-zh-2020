# 你应该知道这些(常见的)深度学习术语和术语

> 原文：<https://towardsdatascience.com/you-should-be-aware-of-these-common-deep-learning-terms-and-terminologies-26e0522fb88b?source=collection_archive---------39----------------------->

![](img/41198258312fe091c1523997b6bd84bc.png)

由 [Raphael Schaller](https://unsplash.com/@raphaelphotoch?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) 在 [Unsplash](https://unsplash.com/s/photos/words?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) 拍摄的照片

## 术语

## 作为深度/机器学习实践者，你一定会经常遇到的术语词汇表

# 介绍

我最近浏览了 Juptyter notebook 中展示的一组基于机器学习的项目，并注意到在我参与或审阅的所有笔记本和基于机器学习的项目中，都有一组重复出现的术语和术语。

**你可以把这篇文章看作是在机器学习和深度学习中消除一些噪音的一种方式。期望找到你在大多数基于深度学习的项目中必然会遇到的术语和术语的描述和解释。**

我涵盖了机器学习项目中与以下主题领域相关的术语和术语的定义:

1.  **数据集**
2.  **卷积神经网络架构**
3.  **技巧**
4.  **超参数**

# 1.数据集

![](img/c5618d1b4e79f24ed191ac7fa2bc109c.png)

由[弗兰基·查马基](https://unsplash.com/@franki?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)在 [Unsplash](https://unsplash.com/s/photos/data?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) 上拍摄的照片

**训练数据集**:这是我们用来直接训练神经网络的一组数据集。训练数据是指在训练期间暴露给神经网络的数据集分区。

**验证数据集**:这组数据集在训练中被用来评估网络在不同迭代中的性能。

**测试数据集**:数据集的这个分区在训练阶段完成后评估我们网络的性能。

# 2.卷积神经网络

![](img/98e835adac456ab74c518b877dbb07b3.png)

照片由 [Alina Grubnyak](https://unsplash.com/@alinnnaaaa?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) 在 [Unsplash](https://unsplash.com/s/photos/neural-networks?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) 上拍摄

**卷积层**:卷积是一个数学术语，描述两组元素之间的点积相乘。在深度学习中，卷积运算作用于卷积层中的过滤器/内核和图像数据阵列。因此，卷积层仅包含滤波器和通过卷积神经网络传递的图像之间的卷积运算。

[**批量标准化层**](/batch-normalization-explained-algorithm-breakdown-23d2794511c) :批量标准化是一种技术，通过引入对来自前一层的输入执行操作的附加层来减轻神经网络内不稳定梯度的影响。这些操作对输入值进行标准化和规范化，然后通过缩放和移位操作转换输入值。

[下面的 max-pooling 操作有一个 2x2 的窗口，并滑过输入数据，输出内核感受域内像素的平均值。](/you-should-understand-sub-sampling-layers-within-deep-learning-b51016acd551)

**展平图层**:取一个输入图形，将输入图像数据展平成一维数组。

**密集层**:密集层中嵌入了任意数量的单元/神经元。每个神经元都是一个感知器。

# 3.技术

![](img/fc6aa2f5f23374097c6b955a0471659e.png)

马库斯·斯皮斯克在 [Unsplash](https://unsplash.com/s/photos/code?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) 上拍摄的照片

[**激活函数**](https://en.wikipedia.org/wiki/Activation_function#:~:text=In%20artificial%20neural%20networks%2C%20the,0)%2C%20depending%20on%20input.) :将神经元的结果或信号转化为归一化输出的数学运算。作为神经网络组件的激活函数的目的是在网络中引入非线性。包含激活函数使神经网络具有更大的表示能力和解决复杂的函数。

**整流线性单元激活函数(ReLU)** :一种对神经元的值结果进行转换的激活函数。ReLU 对来自神经元的值施加的变换由公式 ***y=max(0，x)*** 表示。ReLU 激活函数将来自神经元的任何负值钳制为 0，而正值保持不变。这种数学变换的结果被用作当前层的输出，并被用作神经网络内的连续层的输入。

**Softmax 激活函数**:一种激活函数，用于导出输入向量中一组数字的概率分布。softmax 激活函数的输出是一个向量，其中它的一组值表示一个类或事件发生的概率。向量中的值加起来都是 1。

[**漏失**](/understanding-and-implementing-dropout-in-tensorflow-and-keras-a8a3a02c1bfa) **:** 漏失技术通过随机减少神经网络内互连神经元的数量来工作。在每一个训练步骤中，每个神经元都有可能被遗漏，或者更确切地说，被排除在连接神经元的整理贡献之外。

# 4.超参数

![](img/93626a73dfee45a37c3d07367bd6525c.png)

马尔科·布拉舍维奇在 [Unsplash](https://unsplash.com/s/photos/technology?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) 上拍摄的照片

**损失函数**:一种量化机器学习模型表现*好坏的方法。量化是基于一组输入的输出(成本)，这些输入被称为参数值。参数值用于估计预测，而“损失”是预测值和实际值之间的差异。*

***优化算法**:神经网络内的优化器是一种算法实现，它通过最小化损失函数提供的损失值来促进神经网络内的梯度下降过程。为了减少损失，适当地选择网络内的权重值是至关重要的。*

***学习率**:神经网络实现细节的一个组成部分，因为它是一个因子值，决定了网络权值的更新水平。学习率是一种超参数。*

***Epoch:** 这是一个数值，表示网络暴露于训练数据集中所有数据点的次数。*

# *结论*

*显然，在你从事和完成机器学习项目的过程中，你肯定会遇到更多的术语和术语。*

*在以后的文章中，我可能会扩展机器学习中经常出现的更复杂的概念。*

*请随意保存这篇文章或与刚开始学习旅程或职业生涯的机器学习从业者分享。*

# *我希望这篇文章对你有用。*

*要联系我或找到更多类似本文的内容，请执行以下操作:*

1.  *订阅我的 [**邮箱列表**](https://richmond-alake.ck.page/c8e63294ee) 获取每周简讯*
2.  *跟我上 [**中**](https://medium.com/@richmond.alake)*
3.  *通过 [**LinkedIn**](https://www.linkedin.com/in/richmondalake/) 联系我*

*[](/5-ways-a-machine-learning-practioner-can-generate-income-in-2020-and-beyond-2f541db5f25f) [## 2020 年(及以后)机器学习从业者创收的 5 种方式

### 了解如何利用机器学习技能增加收入或创造新的收入来源

towardsdatascience.com](/5-ways-a-machine-learning-practioner-can-generate-income-in-2020-and-beyond-2f541db5f25f) [](/the-importance-of-side-projects-in-machine-learning-edf9836bc93a) [## 辅助项目在机器学习中的重要性

### 你和马克·扎克伯格有什么共同点？在这篇文章中找出，以及其他几个原因，为什么…

towardsdatascience.com](/the-importance-of-side-projects-in-machine-learning-edf9836bc93a)*