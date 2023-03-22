# 理解强化学习实践:简介

> 原文：<https://towardsdatascience.com/understanding-reinforcement-learning-hands-on-part-1-introduction-44e3b011cf6?source=collection_archive---------60----------------------->

![](img/60087a33517eef7050fffe0cd40772f3.png)

哈桑·帕夏在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上的照片

# “系列”链接:

1.  **简介**
2.  [多臂土匪](https://medium.com/@alejandro.aristizabal24/understanding-reinforcement-learning-hands-on-part-2-multi-armed-bandits-526592072bdc) | [笔记本](https://github.com/aristizabal95/Understanding-Reinforcement-Learning-Hands-On/blob/master/Multi-Armed%20Bandits.ipynb)
3.  [非平稳性](/understanding-reinforcement-learning-hands-on-part-3-non-stationarity-544ed094b55) | [笔记本](https://github.com/aristizabal95/Understanding-Reinforcement-Learning-Hands-On/blob/master/Non-Stationarity.ipynb)
4.  [马尔可夫决策过程](https://medium.com/@alejandro.aristizabal24/understanding-reinforcement-learning-hands-on-markov-decision-processes-7d8469a8a782) | [笔记本](https://github.com/aristizabal95/Understanding-Reinforcement-Learning-Hands-On/blob/master/Markov%20Decision%20Processes.ipynb)
5.  [贝尔曼方程 pt。1](https://medium.com/@alejandro.aristizabal24)

欢迎光临！这是我将要做的关于强化学习的一系列文章中的第一篇，在这一系列文章中，我们将开发这一人工智能分支背后的核心概念，并涵盖多种技术，以在不同的任务中制定、开发和应用 RL(强化学习的缩写)。我这个系列的目标是记录我在这个主题上的学习过程，并希望为任何阅读它的人提供理解这个研究领域的直观和实用的指导。

这个系列受到 Coursera 的[强化学习专业化](https://www.coursera.org/specializations/reinforcement-learning)，以及 Richard Stutton 和 Andrew G .巴尔托的书[强化学习:导论(第二版)](https://web.stanford.edu/class/psych209/Readings/SuttonBartoIPRLBook2ndEd.pdf)的巨大影响。如果有兴趣加深您对该主题的了解，我强烈推荐您深入研究这些资源。我在理解强化学习方面失败了很多次，直到我偶然发现了它们。我希望这一系列文章能作为一个额外的资源来加强你的理解。

每篇文章都将涵盖这样的理论和应用。我已经基于专业化提供的笔记本创建了自己的编码资源，使用了更多可用的工具，如 OpenAI 的 gym，而不是 RL-glue，因为我找不到有用的文档。解决了这个问题，让我们开始研究这个系列吧！

# 什么是强化学习？我们为什么需要它？

随着目前在该领域中发现的所有不同的机器学习领域，问我们自己这些问题是很重要的。看到像监督和非监督学习这样的范例在过去(和现在)工作得多么好，一些人可能会认为强化学习只是一种徒劳的学术追求。其他人可能认为 RL 只能用来炫耀[我们玩游戏](https://deepmind.com/research/case-studies/alphago-the-story-so-far)有多烂。但是，RL 超越了这一点，显然值得与其他研究领域区分开来。

理解强化学习的一个好方法是将其与机器学习的其他框架进行比较。RL 认为学习是一种非常特殊的方式。与监督和非监督学习一样，强化学习依赖于优化数值来实现期望的结果/行为。区别在于算法如何获得这些值。

在监督学习任务中，我们得到了一个包含采样数据的数据集以及它的含义。例如，手写数字及其字母数字表示的图像。该算法的目标是将视觉数据与期望的字符相关联。以这种方式，监督学习可以理解为由该领域的专家教授。问题的答案已经给出，我们使用这些信息将数据映射到结果。正如我们将通过查看分数(我们总共获得了多少个好答案)来衡量我们在测验中的表现一样，监督学习模型将通过查看算法预测的结果与其应该预测的结果之间的误差或差异来衡量其表现。使误差最小化是让我们的模型学习的原因。

![](img/40a3a3affbef1d485d1fea68e5363e83.png)

图一。监督任务的示例。MNIST 数据集。使用 torchvision 和 matplotlib 生成的图像

在无监督学习任务中，我们得到一个包含采样数据的数据集，我们的目标是找到数据中的模式或隐藏结构。与监督学习不同，我们没有得到任何答案，也不知道从这样的分析中会得到什么样的答案。该模型的设计目的是区分数据中的不同关系，以便我们以后可以在其他任务中使用这些关系。这类模型试图优化的价值很大程度上取决于我们试图从数据中获得什么样的信息。例如，自动编码器会通过算法传递数据，希望这种模型的输出与输入非常相似。在这种情况下，我们测量输入和输出之间的差异，并试图将其最小化。

![](img/74c5d0f4d77d710754373159a0c2856f.png)

图二。基于 MNIST 数据训练的自动编码器的潜在空间的可视化。

与前面提到的方法相反，强化学习没有数据集可使用。！).相反，数据是通过与环境交互而生成的。从我们的互动中，我们得到了“奖励”,它指出了我们做得好坏。在一系列长时间的互动后(比如在国际象棋比赛中获胜)，我们可以获得有意义的奖励，我们的互动可以极大地改变未来的结果(国际象棋中失败的错误可以一直追踪到比赛开始)。模型的任务是找到与环境互动的正确方式，从而使其获得的回报金额最大化。

![](img/59aaa313844fb228cb90964c51fc10c8.png)

图 3。A2C 强化学习代理玩突围。

这里明确的区分是“交互”。强化学习问题通常需要您通过采取行动来生成数据，同时根据您拥有的数据采取行动。这非常重要，因为我们的目标是得到一个尽可能表现最佳的代理。让我们假设一下，我们想要尝试训练一个模型使用强化学习之外的东西来玩突围。为了让我们能够做这样的尝试，我们需要获得一个健壮的数据集，我们的代理可以从这个数据集学习行动。这已经是一项艰巨的任务了！例如，请看下图:

![](img/064b2c4a550b3b6249311cf7b56d97da.png)

图 4。越狱游戏中的一个特殊状态。

桨接下来应该采取什么动作？球可能向球拍移动，也可能远离球拍。另外，这是我们代理的最佳位置吗？或者有更好的方法来摧毁更多的砖块？为这项任务获得好的数据集的困难不仅是技术上的，也是理论上的。无论我们决定以何种方式收集数据，我们都假设数据显示了完成任务的最佳方式。如果我们用人类的表现作为参考点，那么我们的代理人将永远做得和人类竞争者一样好。在这样的限制下，我们永远不会看到人工智能在这些任务中击败人类的新闻。

强化学习本质上是一种在不断变化的世界中进行交互学习的范式。在我们的模型需要采取行动的情况下，这种行动改变了手头的问题，那么强化学习是实现目标的最佳途径(也就是说，如果要使用一种学习方法)。RL 代理从交互中学习的能力产生了获得超人性能的潜力，无论是在我们真正擅长还是真正不擅长的任务上。我相信，这使得强化学习值得努力。现在，这个领域通常被视为在视频游戏中产生巨大的结果，但在未来，我们可以看到 RL 代理优化真实世界的问题，这些问题在历史上一直困扰着我们！强化学习确实很有前途。

# 结论

这是对什么是强化学习，它与其他学习框架有何不同，以及为什么我们应该研究它的快速介绍。强化学习是一个非常有趣的领域，我希望这篇文章给您留下了这样的印象。在下一篇文章中，我们将深入探讨武装匪徒的问题，这是一个基本的场景，展示了所有强化学习方法背后的核心思想和复杂性。

# “系列”链接:

1.  **简介**
2.  [多臂土匪](https://medium.com/@alejandro.aristizabal24/understanding-reinforcement-learning-hands-on-part-2-multi-armed-bandits-526592072bdc) | [笔记本](https://github.com/aristizabal95/Understanding-Reinforcement-Learning-Hands-On/blob/master/Multi-Armed%20Bandits.ipynb)
3.  [非平稳性](/understanding-reinforcement-learning-hands-on-part-3-non-stationarity-544ed094b55) | [笔记本](https://github.com/aristizabal95/Understanding-Reinforcement-Learning-Hands-On/blob/master/Non-Stationarity.ipynb)
4.  [马氏决策流程](https://medium.com/@alejandro.aristizabal24/understanding-reinforcement-learning-hands-on-markov-decision-processes-7d8469a8a782) | [笔记本](https://github.com/aristizabal95/Understanding-Reinforcement-Learning-Hands-On/blob/master/Markov%20Decision%20Processes.ipynb)
5.  [贝尔曼方程 pt。1](https://medium.com/@alejandro.aristizabal24)

# 参考资料:

*   没有引用的图像由作者生成。
*   图二。摘自[知识共享署名 4.0 许可](https://creativecommons.org/licenses/by/4.0/)下的 [Tensorflow 教程](https://www.tensorflow.org/tutorials/generative/cvae)。
*   图 3。取自麻省理工学院许可[下的](https://opensource.org/licenses/MIT)[稳定基线文档](https://stable-baselines.readthedocs.io/en/master/_images/breakout.gif)。
*   图 4。取自[麻省理工学院许可](https://opensource.org/licenses/MIT)下的[稳定基线文件](https://stable-baselines.readthedocs.io/en/master/_images/breakout.gif)。
*   Coursera 的[强化学习专业化](https://www.coursera.org/specializations/reinforcement-learning)由阿尔伯塔大学提供。
*   萨顿和巴尔托(2018 年)。 [*强化学习:入门*](https://web.stanford.edu/class/psych209/Readings/SuttonBartoIPRLBook2ndEd.pdf) 。剑桥(麻省。):麻省理工学院出版社。取自斯坦福档案馆。