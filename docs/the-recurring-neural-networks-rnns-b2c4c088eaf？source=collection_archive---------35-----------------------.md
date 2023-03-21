# 递归神经网络

> 原文：<https://towardsdatascience.com/the-recurring-neural-networks-rnns-b2c4c088eaf?source=collection_archive---------35----------------------->

## 递归神经网络(RNN)是一个输入节点(隐藏层),用于激活 sigmoid。

RNN 实现这一点的方式是获取一个神经元的输出，并将其作为输入返回给另一个神经元，或者将当前时间步长的输入馈送给较早时间步长的输出。在这里，您将以前时间的输入一步一步地输入到当前时间的输入中，反之亦然。

![](img/bf2a53e7cee14c97a4b56ebc91ff3b3f.png)

这可以以多种方式使用，例如通过具有已知变化的学习门或 sigmoid 激活和许多其他类型的神经网络的组合。

RNNs 的一些应用包括预测能源需求、预测股票价格和预测人类行为。rnn 是根据基于时间和基于序列的数据建模的，但它们在各种其他应用中也是有用的。

递归神经网络是一种用于深度学习、机器学习和其他形式的人工智能(AI)的人工神经网络。它们有许多属性，这些属性使它们对于需要顺序处理数据的任务非常有用。

说得更专业一点，递归神经网络被设计成通过从序列的一个步骤到下一个步骤遍历隐藏状态，结合输入，并在输入之间来回路由来学习数据序列。RNN 是为有效处理顺序数据而设计的神经网络，但也适用于非顺序数据。

这些类型的数据包括可被视为一系列单词的文本文档或音频文件，其中您可以看到一系列声音频率和时间。输出图层的可用信息越多，读取和排序的速度就越快，其性能也就越好。

rnn 旨在识别具有序列特征的数据，并预测下一个可能的场景。它们被用于模拟人脑中神经元活动的模型中，如深度学习和机器学习。

这种类型的 RNN 有一种记忆力，使它能够记住过去发生过多次的重要事件(步骤)。rnn 是可以分解成一系列小块并作为序列处理的图像。通过使用学习的输入数据的时间相关性，我们能够将我们学习的序列与其他回归和分类任务区分开来。

![](img/6d48d239341af0f543c382f4564c56ad.png)

处理顺序数据(文本、语音、视频等)。)，我们可以将数据向量输入常规的神经网络。RNNs 可用于各种应用，例如语音识别、图像分类和图像识别。

在前馈神经网络中，决策基于当前输入，并且独立于先前输入(例如，文本、视频等)。).rnn 可以通过接受先前接收的输入并线性处理它来处理顺序数据。神经网络中的前馈使得信息能够从一个隐藏层流向下一个隐藏层，而不需要单独的处理层。基于这种学习序列，我们能够通过其对输入数据的时间依赖性将其与其他回归和分类任务区分开来。

本质上，RNN 是一个上下文循环，允许在上下文中处理数据——换句话说，它应该允许递归神经网络有意义地处理数据。神经网络的循环连接与特定上下文中的输入和输出数据形成受控循环。

由于理解上下文对于任何类型的信息的感知都是至关重要的，这使得递归神经网络能够基于放置在特定上下文中的模式来识别和生成数据。与其他类型的直接处理数据并且独立处理每个元素的神经网络不同，递归神经网络关注输入和输出数据的上下文。

由于它们的内部循环，rnn 具有动态组合经验的能力。像记忆细胞一样，这些网络能够有效地关联遥远时间的记忆输入，并随着时间的推移以高度可预测性动态捕获数据结构。已经证明，RNNs 能够比传统的神经网络(例如，以线性回归模型的形式)更快地处理序列数据。

![](img/95f101edaf0bef4b48898e7dc0dbf928.png)

LSTM(长短期记忆)引入了一个隐藏层网络，其中传统的人工神经元被计算单元取代。

与其他传统的 rnn 不同，LSTM 可以处理梯度和消失问题，特别是在处理长期时间序列数据时，每个存储单元(LSTM 单元)保留关于给定上下文(即输入和输出)的相同信息。

研究表明，与其他传统的 rnn 相比，神经 LSTM 网络在处理长期时间序列数据时表现更好。由于理解上下文对于任何种类的信息的感知都是至关重要的，这允许递归神经网络基于放置在特定上下文中的模式来识别和生成数据。

## **引用来源**

*   [https://blog . use journal . com/stock-market-prediction-by-recurrent-neural-network-on-lstm-model-56de 700 BFF 68](https://blog.usejournal.com/stock-market-prediction-by-recurrent-neural-network-on-lstm-model-56de700bff68)
*   [https://pathmind.com/wiki/lstm](https://pathmind.com/wiki/lstm)
*   [https://developer . NVIDIA . com/discover/recurrent-neural-network](https://developer.nvidia.com/discover/recurrent-neural-network)
*   [https://bmcbioinformatics . biomed central . com/articles/10.1186/s 12859-019-3131-8](https://bmcbioinformatics.biomedcentral.com/articles/10.1186/s12859-019-3131-8)
*   [https://theapp solutions . com/blog/development/recurrent-neural-networks/](https://theappsolutions.com/blog/development/recurrent-neural-networks/)
*   [https://www . mlq . ai/guide-to-recurrent-neural-networks-lst ms/](https://www.mlq.ai/guide-to-recurrent-neural-networks-lstms/)
*   [https://blog . statsbot . co/time-series-prediction-using-recurrent-neural-networks-lst ms-807 fa 6 ca 7 f](https://blog.statsbot.co/time-series-prediction-using-recurrent-neural-networks-lstms-807fa6ca7f)
*   [https://www . simpli learn . com/recurrent-neural-network-tutorial-article](https://www.simplilearn.com/recurrent-neural-network-tutorial-article)
*   [https://mc.ai/rnn-recurrent-neural-networks-lstm/](https://mc.ai/rnn-recurrent-neural-networks-lstm/)
*   [https://victorzhou.com/blog/intro-to-rnns/](https://victorzhou.com/blog/intro-to-rnns/)