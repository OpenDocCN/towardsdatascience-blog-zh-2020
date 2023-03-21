# 宝宝很牛逼…人类就是 OG 神经网。

> 原文：<https://towardsdatascience.com/babies-are-awesome-humans-are-the-og-neural-net-e2dc83fe9eff?source=collection_archive---------58----------------------->

## 即使人工智能和神经科学在许多方面相似，但它们并不完全相同。

![](img/7b8a98972fea22ca163ddb4660d0c66a.png)

图片由[简·海伦布兰特](https://pixabay.com/users/Juhele-3094317/?utm_source=link-attribution&utm_medium=referral&utm_campaign=image&utm_content=4314563)从[皮克斯拜](https://pixabay.com/?utm_source=link-attribution&utm_medium=referral&utm_campaign=image&utm_content=4314563)拍摄

> “婴儿太棒了……人类就是 OG 神经网络。”—埃隆·马斯克，在最近的一次[乔·罗根采访](https://www.youtube.com/watch?v=RcYjXbSJBN8)中，他们在讨论他的新生儿 xa-12。

事实上，我们的大脑如何连接和神经网络如何工作有许多相似之处。人工智能神经网络的本质类似于人脑，模拟大脑在学习过程中的行为。即使人工智能和神经科学在许多方面相似，但它们并不完全相同。

就像，我们建造潜艇不是为了像鱼一样游泳；而是借用了流体力学的原理，应用于建造潜艇。在莱特兄弟之前，人们设计翅膀像鸟一样扇动。但是莱特兄弟解决了飞行的问题，他们不再试图制造完美的鸟类翅膀。相反，他们研究了翅膀的模式和产生升力的翅膀上下流动的空气动力学。

同样，我们可以从人脑中寻找灵感，并借用有价值的概念。人工智能研究人员致力于通过理解生物大脑来模拟人脑的内部过程，这可能在构建智能机器方面发挥重要作用。

# 情景记忆

构建智能系统的关键在于[记忆系统](https://psycnet.apa.org/doiLanding?doi=10.1037%2F0003-066X.40.4.385)记住过去的经历。在大脑中，这是[海马](https://en.wikipedia.org/wiki/Hippocampus)，它在巩固信息、学习和记忆中起着至关重要的作用。

在[强化学习](https://www.nature.com/articles/nature16961)中，这允许动作的值通过重复的经验被增量地学习并存储在记忆中，被称为情景记忆。 [Deep Q-network](https://arxiv.org/abs/1312.5602) (DQN)的一个关键要素是“经验重放”，网络通过经验存储行动的价值，然后“重放”它。DQN 存储了与每个雅达利游戏屏幕或星际争霸场景相关的行动和奖励结果等经验。它根据当前情况和存储在记忆中的先前经历之间的相似性来选择行动，采取能产生最高回报的行动。

经验重放允许强化学习从过去发生的成功或失败中学习，由此导致奖励或惩罚的行动序列在内部重新制定。存储在 DQN 重放缓冲器中的经验就像原始的海马体一样实现，允许巩固、学习和记忆发生。

# 内存储器

人类不是每秒钟都从零开始思考；相反，我们的思想具有持久性。当你读这句话的时候，你对句子的理解是基于一系列的单词。你利用现有的知识并产生新的信息。

人类智慧的定义是我们在活跃的商店中保持和操纵信息的非凡能力，这种能力被称为[工作记忆](https://www.sciencedirect.com/science/article/pii/S0079612308626886)。与回忆过去的情景记忆不同，工作记忆使认知成为可能，这是一个通过思想、经验和感官获取知识和理解的心理过程。

随着时间的推移，我们维护和处理信息的能力导致人工智能研究人员开发了[递归神经网络架构](https://github.com/kjw0612/awesome-rnn)。这些网络中有环路，允许信息持续存在。它已经在多种应用中得到应用，比如[自然语言处理](http://cs224d.stanford.edu/index.html)和[语音识别](http://www.cs.toronto.edu/~fritz/absps/RNN13.pdf)。它还被用于创建描述图像的标题，通过选择图像的一部分来查看它输出的每个单词。这些网络在各种各样的问题上工作得非常好，特别是[长短期记忆网络](https://www.mitpressjournals.org/doi/abs/10.1162/neco.1997.9.8.1735)，它在各个领域都取得了最先进的表现。

# 注意力

不像大多数 CNN 模型直接在整个图像上工作来确定猫是否存在，我们的视觉注意力战略性地转移到物体上。我们不是处理整个图像，而是集中我们的处理资源，[分离出在任何给定时刻相关的信息](https://www.jneurosci.org/content/13/11/4700.short)。

这种注意力机制一直是人工智能架构的灵感来源，旨在忽略图像中不相关的对象，专注于相关的内容。这也使得人工智能从业者可以根据输入图像的大小来调整计算成本。注意力机制已经导致在混乱的情况下在困难的[多物体识别任务](http://papers.nips.cc/paper/5542-recurrent-models-of-visual-attention)中产生令人印象深刻的表现。它还支持图像到字幕的生成。

虽然注意力最初被认为是感知的定向机制，但它已经导致了机器翻译和谷歌翻译的艺术表现。它的成功归功于它在翻译过程中通过有选择地关注句子的子部分来很好地概括长句的能力。

# 迁移学习

人类有一种与生俱来的能力，可以将从一个环境中获得的知识应用到新的环境中。我们在学习一项任务时获得的知识，我们可以用同样的方法来概括解决相关任务的经验。当我们试图学习新的东西时，我们不会从头开始学习所有的东西。相反，我们利用过去学到的知识。

例如，我们认为汽车是一种有轮子和门的物体，它们有特定的形状和大小。我们可以在试图识别卡车时使用这些知识，而无需重新学习车轮的外观。同样，在学习法语之后，我们可以有效地学习意大利语，因为我们可以概括共同的语法特征和单词相似性。

尽管是人类的第二天性，[迁移学习](https://ieeexplore.ieee.org/abstract/document/5288526/)是机器学习中的一个研究难题。人工智能研究人员专注于利用从一个应用程序中获得的存储知识，并将其应用于一个不同但相关的问题。在深度学习的背景下，迁移学习的关键动机是缺乏不同领域的注释数据。由于机器学习方法依赖于足够数量的带注释的训练数据的可用性，所以获得足够的带标签的训练数据通常是昂贵的。

在计算机视觉领域，迁移学习的一个成功应用是在 [ImageNet 挑战赛](http://image-net.org/)中，参与者被提供了包含 1000 个类别和 120 万张图像的 ImageNet 训练数据子集。

对于自然语言处理，单词嵌入，如 [Word2vec](https://papers.nips.cc/paper/5021-distributed-representations-of-words-and-phrases-and-their-compositionality.pdf) ，其中单词的语义是从维基百科这样的源数据中训练出来的，并将其应用于情感分析和文档分类。

在[语音识别](https://arxiv.org/abs/1706.00290)中，为英语开发的模型已成功用于提高其他语言的语音识别能力。

[](/the-fascinating-relationship-between-ai-and-neuroscience-89189218bb05) [## 人工智能和神经科学之间的迷人关系

### 他们如何相互激励、共同进步、相互受益

towardsdatascience.com](/the-fascinating-relationship-between-ai-and-neuroscience-89189218bb05) 

乔·罗根采访埃隆·马斯克

[![](img/e6191b77eb1b195de751fecf706289ca.png)](https://jinglescode.github.io/)[![](img/7c898af9285ccd6872db2ff2f21ce5d5.png)](https://towardsdatascience.com/@jinglesnote)[![](img/d370b96eace4b03cb3c36039b70735d4.png)](https://jingles.substack.com/subscribe)