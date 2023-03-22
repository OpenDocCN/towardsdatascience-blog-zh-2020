# 解释了反事实政策梯度

> 原文：<https://towardsdatascience.com/counterfactual-policy-gradients-explained-40ac91cef6ae?source=collection_archive---------39----------------------->

## 多智能体强化学习中的学分分配问题

![](img/f58fc18eac39c054c0b7dcc12da73481.png)

由 [Ales Nesetril](https://unsplash.com/@alesnesetril?utm_source=medium&utm_medium=referral) 在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄的照片

在众多挑战中，多主体强化学习有一个被忽视的障碍:“学分分配”为了解释这个概念，我们先来看一个例子…

假设我们有两个机器人，机器人 A 和机器人 b。他们试图合作将一个盒子推入一个洞。此外，他们*和*都将获得 1 英镑的奖励，如果他们按下按钮，则获得 0 英镑的奖励。在理想情况下，两个机器人会同时将盒子推向洞口，最大限度地提高任务的速度和效率。

然而，假设机器人 A 做了所有的重活，也就是说机器人 A 把箱子推进洞里，而机器人 B 在一旁无所事事。即使机器人 B 只是四处游荡，*机器人 A* ***和机器人 B****都会得到 1 的奖励。*换句话说，即使机器人 B 执行了一个次优策略，同样的行为随后也会被鼓励*。*这就是“信用转让”问题的由来。在多智能体系统中，我们需要找到一种方法，将“荣誉”或奖励给予为整体目标做出贡献的智能体，而不是那些让别人做工作的智能体。

好吧，那解决办法是什么？可能我们只给对任务本身有贡献的特工奖励。

![](img/3acca653c057cf058a76dbd764b704e0.png)

照片由 [Kira auf der Heide](https://unsplash.com/@kadh?utm_source=medium&utm_medium=referral) 在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄

# 这比看起来要难

看起来这个简单的解决方案可能行得通，但是我们必须记住几件事。

首先，强化学习中的状态表征可能没有足够的表现力来恰当地定制这样的奖励。换句话说，我们不能总是很容易地量化一个代理人是否对给定的任务做出了贡献，并相应地给予奖励。

其次，我们不想手工制作这些奖励，因为这违背了设计多智能体算法的目的。告诉代理如何协作和鼓励他们学习如何协作之间有一条细微的界限。

# 一个答案

反事实的政策梯度解决了信用分配的问题，但没有明确给出代理人的答案。

这种方法背后的主要思想是什么？让我们通过将代理的操作与它可能采取的其他操作进行比较来训练代理策略。换句话说，代理会问自己:

> “如果我选择了不同的行动，我们会得到更多的奖励吗？”

通过将这一思考过程纳入数学，反事实多主体(COMA)策略梯度通过量化主体对完成任务的贡献来解决信用分配问题。

![](img/80279bd0ffe59ee6528de1d935360469.png)

由[布兰登·莫温克尔](https://unsplash.com/@bmowinkel?utm_source=medium&utm_medium=referral)在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄的照片

# 组件

COMA 是一种演员-评论家方法，使用集中学习和分散执行。这意味着我们训练两个网络:

*   一个 **actor** :给定一个状态，输出一个动作
*   评论家:给定一个状态，估计一个价值函数

此外，critic*仅在培训期间使用，在测试期间移除*。我们可以把批评家看作算法的“训练轮”我们使用评论家来指导演员在整个培训过程中，并给它如何更新和学习其政策的建议。然而，当到了执行演员所学政策的时候，我们就把批评家去掉了。

关于演员-评论家方法的更多背景知识，请看 [Chris Yoon](https://medium.com/u/b24112d01863?source=post_page-----40ac91cef6ae--------------------------------) 的深度文章:

[](/understanding-actor-critic-methods-931b97b6df3f) [## 理解演员评论方法

### 预赛

towardsdatascience.com](/understanding-actor-critic-methods-931b97b6df3f) 

让我们先来看看评论家。在这个算法中，我们训练一个网络来估计所有代理的联合 Q 值。我们将在本文后面讨论评论家的细微差别以及它是如何被特别设计的。然而，我们现在需要知道的是，我们有两个批评家网络的副本。一个是我们试图训练的网络，另一个是我们的目标网络，用于训练稳定性。目标网络的参数定期从训练网络中复制。

为了训练网络，我们使用策略训练。我们使用 TD(lambda)来确定我们的目标 Q 值，而不是使用一步或 n 步前瞻，TD(lambda)使用 n 步回报的混合。

![](img/1c7ffebed19599175039d0c32ff85572.png)

使用 TD(λ)的 n 步返回和目标值

其中，γ是折扣系数，r 表示特定时间步长的奖励，f 是我们的目标价值函数，λ是超参数。这个看似无限的地平线值是由目标网络使用自举估计来计算的。

关于 TD(lambda)的更多信息， [Andre Violante](https://medium.com/u/54f2f2975136?source=post_page-----40ac91cef6ae--------------------------------) 的文章提供了一个奇妙的解释:

[](https://medium.com/@violante.andre/simple-reinforcement-learning-temporal-difference-learning-e883ea0d65b0) [## 简单强化学习:时间差异学习

### 所以最近我读了很多关于强化学习的书，还看了大卫·西尔弗的介绍…

medium.com](https://medium.com/@violante.andre/simple-reinforcement-learning-temporal-difference-learning-e883ea0d65b0) 

最后，我们通过最小化该函数来更新评论家的参数:

![](img/8cd713f1bb49c9b6b3cdbba8b20808fb.png)

损失函数

![](img/58b70b10767385862bdfc520bf170199.png)

何塞·莫拉莱斯在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上的照片

# 问题是

现在，你可能想知道:这不是什么新鲜事！这个算法有什么特别之处？这种算法背后的美妙之处在于我们如何更新*演员网络的参数。*

在 COMA 中，我们训练概率策略，这意味着在给定状态下的每个动作都是以特定的概率选择的，该概率在整个训练过程中会发生变化。在典型的行动者-批评家场景中，我们通过使用策略梯度来更新策略，通常使用价值函数作为基线来创建优势行动者-批评家:

![](img/af3fdcedc253613a66d83a025efe52e4.png)

天真的优势演员评论家政策更新

然而，这里有一个问题。这没有解决我们试图解决的原始问题:“信用分配”我们不知道“任何一个个体对任务的贡献有多大”相反，考虑到我们的价值函数估计*联合价值函数*，所有的代理都被给予相同数量的“信用”。因此，COMA 建议使用不同的术语作为我们的基线。

为了计算每个代理的这个**反事实基线**，*我们计算代理可以采取的所有行动的期望值，同时保持所有其他代理的行动固定。*

![](img/382b30a2bd878902ca5730e49b5a39c8.png)

将反事实基线添加到优势函数估计中

让我们后退一步，剖析这个等式。第一项只是与联合状态和联合行动(所有代理)相关联的 Q 值。第二项是期望值。观察求和中的每一项，有两个值相乘。第一个是这个代理人选择特定行动的概率。第二个是采取行动的 Q 值，而所有其他代理保持他们的行动不变。

那么，这为什么会起作用呢？直觉上，通过使用这个基线，代理人知道这个行为相对于它可能采取的所有其他行为贡献了多少回报。这样，它可以更好地区分哪些行动将更有助于所有代理的*整体奖励。*

COMA 提出使用特定的网络架构有助于更有效地计算基线[1]。此外，通过使用蒙特卡罗样本估计期望值，该算法可以扩展到连续动作空间。

![](img/57edae4ce0ce26f231b4d9732b96bb16.png)

[JESHOOTS.COM](https://unsplash.com/@jeshoots?utm_source=medium&utm_medium=referral)在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍照

# 结果

COMA 在星际争霸单位微观管理上进行测试，与各种中央和独立演员批评家变量进行比较，估计 Q 值和价值函数。结果表明，该方法明显优于其他方法。对于官方报道的结果和分析，查看原始论文[1]。

# 结论

没人喜欢懒鬼。机器人也不会。

适当地允许代理认可他们对任务的个人贡献，并优化他们的策略以最好地利用这些信息，这是使机器人协作的重要部分。在未来，可能会探索更好的分散方法，有效地成倍降低学习空间。然而，这说起来容易做起来难，所有这类问题都是如此。当然，这是一个强大的里程碑，让多智能体在更高、更复杂的层次上发挥作用。

# 参考

[1] J. Foerster，G. Farquhar，T. Afouras，N. Nardelli，S. Whiteson，[反事实多主体政策梯度](https://arxiv.org/pdf/1705.08926.pdf) (2017)。

> 从经典到最新，这里有讨论多代理和单代理强化学习的相关文章:

[](/openais-multi-agent-deep-deterministic-policy-gradients-maddpg-9d2dad34c82) [## OpenAI 的 MADDPG 算法

### 多主体 RL 问题的行动者批评方法

towardsdatascience.com](/openais-multi-agent-deep-deterministic-policy-gradients-maddpg-9d2dad34c82) [](/hierarchical-reinforcement-learning-feudal-networks-44e2657526d7) [## 分层强化学习:封建网络

### 让电脑看到更大的画面

towardsdatascience.com](/hierarchical-reinforcement-learning-feudal-networks-44e2657526d7)