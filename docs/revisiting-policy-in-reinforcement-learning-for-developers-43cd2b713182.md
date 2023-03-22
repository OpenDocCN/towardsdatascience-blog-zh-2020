# 开发人员的强化学习策略

> 原文：<https://towardsdatascience.com/revisiting-policy-in-reinforcement-learning-for-developers-43cd2b713182?source=collection_archive---------15----------------------->

## 理解强化学习策略的实用方法

![](img/4a0da404970965d2cb771c9d6017c1e4.png)

菲利普·怀尔斯在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上的照片

**更新**:学习和练习强化学习的最好方式是去[http://rl-lab.com](http://rl-lab.com/)

策略在某种程度上是一个棘手的概念，主要针对强化学习的初学者。这篇文章将试图用简单明了的英语澄清这个话题，远离数学概念。它是在考虑到**开发者**的情况下编写的。

# 政策

如果您听说过最佳实践或指导原则，那么您应该听说过政策。例如，考虑住在高楼里的人们的消防安全指南。也许最重要的指导方针是在火灾中不要使用电梯。人们应该关上门，储备水，使用湿床单，并让消防员知道他们的位置。
这一系列行动是根据指导方针发布的，这些指导方针是对长期以来积累的最佳实践和经验进行汇编的结果。
所以**政策告诉你在面对某种情况时如何行动**。它不会告诉你行动的结果或价值。然而，当遵循这个策略时，你隐含地期望有最好的可能结果，例如安然无恙地逃离火灾。

# 动作值

**行动价值是指你采取行动并评估其结果或价值的时候。**
例如，一个被困在着火建筑的高层的人考虑他/她的选择(可用的行动)，然后尝试每一个来评估其结果。当然，在现实中，这是不可能的，因为一旦受伤就没有回头路了。但是在模拟、体育、游戏中，有可能玩许多场景并找出给出最佳结果的动作。

# 政策与行动价值的关系

不言而喻，策略是基于得分(或价值)最高的操作的最佳实践的集合。

我们这样写道

![](img/8c91922469f43b3a786844aefb23748a.png)

这意味着在状态 **s** 时，我们采取行动 **a** 使得 **a** 给出最佳结果。
其中 q(s，a)是在状态 **s** 采取的动作 **a** 的值。

但是有一个问题。你需要真正理解这是一个随机的环境，充满了不确定性。这意味着同一状态下的同一动作，每次执行都可能导致不同的结果。

例如，一个篮球运动员练习投三分球，即使他用同一只手从同一个点(或角度)投篮，有时会得分，有时会失手。他/她必须计算一个平均值(得分/投掷次数)来找出点(或角度)和手牌的组合效率。

![](img/144a3a61f8ddf0c743840bd4b6736e9a.png)

[克里斯·摩尔](https://unsplash.com/@chrismoore_?utm_source=medium&utm_medium=referral)在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍照

为了找到比赛中使用的最佳角度和手，他/她必须从不同的角度和不同的手练习投篮，然后平均结果，以推断出哪种组合最适合他/她。

当然，解决这个问题的一个方法是执行大量的迭代。在每次迭代中，执行该状态下所有可用的动作，然后对每个动作取 q(s，a)的平均值，并更新策略𝛑(s).我们说我们正在**更新每次迭代的策略**。因此，在每次迭代之后，我们都有一个新的策略𝛑来反映计算出的 q 值。

在足够多的迭代之后，当 q(s，a)的平均值开始缓慢变化到足以认为它是稳定的时候，我们说我们已经到达了一个稳定的阶段，并且策略已经变得最优(意味着这是可以达到的最好的)，我们称之为**最优策略**，并且我们写它为𝛑*

处理政策问题有不同的方式，改进政策也有不同的方式(即更新)

# 最优策略

最佳策略是当当前被训练的策略已经达到一个阶段，在这个阶段之后它不能被实质性地改进。
最优策略的重要性在于，它允许跟随它的代理实现“最佳可能”的结果。

# 动态规划方法中的策略

这是最简单的场景，也是最不现实的场景。所有的环境动态都是已知的，我们只需迭代计算 q 值，并在每次迭代后将具有最高 q(s，a)值的动作分配给策略𝛑(s).
在动态编程中，我们知道所有的状态和所有的转移概率，所以不需要发现任何东西，只需要计算值。

在本文中有更多关于[动态编程的内容。](https://medium.com/@zsalloum/dynamic-programming-in-reinforcement-learning-the-easy-way-359c7791d0ac)

# 蒙特卡罗方法中的策略

在蒙特卡洛，我们不太了解环境的内部运作，所以需要通过播放剧集来发现。我们播放每一集直到结束，直到我们到达一个终端状态，然后我们计算每个访问的状态和执行的每个动作的 q 值 q(s，a)。

然而，与动态编程不同，我们无法计算所有状态的 q 值，因为我们事先根本不知道它们。我们必须在播放剧集时发现它们。现在的问题是，我们如何以一种有意义的方式播放这些片段，以便我们能够提取最佳策略？
当然，还有随机漫步策略，即在每个状态选择一个要执行的随机动作。这在开始时会起作用，但后来，当我们开始发现一些行为比其他行为更好时，保持随机行走很快就会变得低效，因为它不会学到任何东西。这种低效率的原因是因为策略没有用 q 的新值来更新。

因此，最好使用在每个州都能发挥最佳作用的政策。但这种策略的问题是，我们每次都将执行相同的操作，我们将无法发现新的状态，此外，我们也不确定所选择的操作是否真的是最好的。请记住，环境充满了不确定性，没有任何东西可以保证，如果一个行动在时间 t 产生了好的结果，在时间 t+1 也会有同样的好结果。因此，为了最好地利用这两种方法，我们采用了一种叫做𝜀-greedy.的策略
在𝜀-greedy 策略中，我们部分时间遵循贪婪策略(选择目前已知的最佳行动)1- 𝜀，部分时间选择随机行动𝜀。这将允许在某些时候通过随机漫步发现新的状态，并在大多数时候利用最佳动作。

点击这些链接，了解更多关于[蒙特卡洛](https://medium.com/@zsalloum/monte-carlo-in-reinforcement-learning-the-easy-way-564c53010511)和[时差](/td-in-reinforcement-learning-the-easy-way-f92ecfa9f3ce)方法的信息。

# 随机政策

并不是所有的事情都是关于选择产生最佳 q 值的行为。要真正掌握这个想法，做以下实验:
和某人玩石头剪子布，注意连续玩同一个动作是个坏主意(即使前几次奏效了)，因为你的对手会对此采取对策。例如，如果你经常玩石头，你的对手会玩纸并赢。
所以政策不再是关于某个行动在某个状态下的最佳结果，而是关于让你赢的行动的概率分布。换句话说，这是关于你的行为应该有多不可预测。

请注意，随机政策并不意味着它在所有状态下都是随机的。对他们中的一些人来说已经足够了。在这些状态中，策略确定性地动作，其动作概率分布(在这些状态上)对于一个动作是 100%,对于所有其他动作是 0%。

[基于策略的强化学习](/policy-based-reinforcement-learning-the-easy-way-8de9a3356083)和[逐步策略梯度](/policy-gradient-step-by-step-ac34b629fd55)更详细地解释了随机策略。

# 政策评价

评估一项政策意味着什么？
嗯，就像测试其他东西一样。你试着去发现它是否实现了它的承诺。
同样，策略评估包括要求策略提供给定状态的操作，然后执行该操作并计算结果(例如 q 值)
最终，并非所有操作都会产生预期结果，因此策略需要一些微调。
这是**策略控制的工作**也叫**策略改进**

# 政策控制/改进

策略控制或改进是关于在某个状态下执行一个动作后给策略反馈。动作的结果被输入到策略中，因此它更新其内部行为，在某种程度上使该动作在下一次达到相同状态时更有可能被使用(或不被使用)。这种反馈被称为更新。

再次考虑篮球运动员在三分线投掷的例子。从左手开始，他/她得分几次，因此策略被更新为使用左手。随着练习的进行，他/她在使用左手时有 30%的机会成功，但在使用右手时有 70%的机会成功。然后应该更新策略，在这个点(或角度)上使用右手而不是左手。

# 概念实现

为了从概念上实现一个策略，让我们考虑下面这个名为 IPolicy 的接口。它包含两种方法:

*   getAction 返回在状态 **s** 时要执行的操作。
*   update(s，a，some_value)，以某种方式更新当前策略，以调整其在状态 **s** 时的行为。这通常通过告诉策略关于在状态 **s** 执行的动作 **a** 的结果(q 值或其他)来完成。

```
interface IPolicy{ // get action given state s as indicated by the current policy
   getAction(s);

   // update policy at state s and action a
   update(s, a, some_value);
}
```

每个特定的策略都以反映其行为的方式实现这个接口。
例如，RandomPolicy 实现 getAction(s ),以便它在状态 **s** 返回随机动作。它不实现 update 方法，因为当行为总是随机的时，不需要任何更新。

```
class RandomPolicy : IPolicy { // get random action at state s
   getAction(s){
      return random action at state s
   }

   // no need for implementation since the policy is random
   update(s, a, some_value){
   }
}
```

GreedyPolicy 返回在状态 **s** 给出最佳 q 值的动作。因此，它必须将这些值存储在适当的结构中。
另一方面，更新方法更新在状态 **s** 和动作**a**计算的 q 值。这通常通过对先前计算的值求平均值来完成。

```
class GreedyPolicy : IPolicy {
   // store q values by state and action
   QValueByStateAndAction store // get action that has the highest q for the given state s
   getAction(s){
      return store.getActionOfMaxQ(s)
   }

   // update policy by storing the average of the q computed at
   // state s and action a
   update(s, a, q){
      prev_q = store.getQForStateAndAction(s, a)
      store.updateQ(s, a, average(prev_q, q))
 }
}
```

EpsilonGreedyPolicy 返回在状态 **s** 的随机动作，𝜀部分时间，以及具有最佳 q 值(1- 𝜀)部分时间的动作。
至于更新方法，和 GreedyPolicy 的逻辑是一样的。

```
class EpsilonGreedyPolicy : IPolicy {
   // store q values by state and action
   QValueByStateAndAction store
   epsilon = 0.1 // get random action epsilon time, highest q 
   // (1-epsilon) of the time
   getAction(s){
     rnd = computeRandomValue()
     if(rnd < epsilon) return random action at state s
     else return store.getActionOfMaxQ(s)
   }

   // update policy by storing the average of the q computed at
   // state s and action a
   update(s, a, q){
      prev_q = store.getQForStateAndAction(s, a)
      store.updateQ(s, a, average(prev_q, q))
 }
}
```

StochasticPolicy 不**直接**依赖于 q 值(它可能会间接依赖于 q 值)，但是它会根据成功的概率分布返回动作。例如，如果动作 A 在 60%的时间里成功，动作 B 在 30%的时间里成功，动作 C 在 10%的时间里成功。则该方法返回的操作遵循 60% A、30% B、10% C 的分布。

至于更新方法，它更新动作的潜在概率分布。

```
class StochasticPolicy : IPolicy {
   // store q values by state and action
   ActionByFrequency store// get action following the frequency of success
   getAction(s){
     return store.getActionByProbabilityOfSuccess()
   }

   // update policy by storing the occurrence of success 
   // result : success = 1; failure = 0
   update(s, a, result){
      success_count = store.getSuccessCount()
      store.setSuccessCount(s, a, success_count + result)
 }
}
```

# 高级算法

为了综合考虑所有因素并得出最佳策略，我们考虑一种高级算法，它可以执行以下操作:

*   根据需要循环以下步骤
*   从状态 s 的策略中获取一个动作:a = action.getAction(s)
    这是策略评估
*   在环境中执行动作，观察奖励 **r** 和新状态**s’**
*   计算评估所采取措施的有效性所需的任何值 **v** 。这可能是 q 值或其他，它可能涉及神经网络或其他
*   向策略提供反馈，以便它可以自我改进:policy.update(s，a，v)
    这是策略改进

它大概是这样的。

![](img/3fbb0014be8f317d99f7ec216a8eec7b.png)

E =评估，I =改进

或者这个

![](img/f78af1fed24335d8a0820fe2f996f7a5.png)

向上的箭头是评估阶段，向下的箭头是改进阶段。

# 结论

这篇文章以一种实用的方式，通过描述性的解释而不是数学性的解释，来关注强化学习的策略。它是整个 RL 结构中的一个构建模块，对开发者来说收益最大。

# 相关文章

*   [理解强化学习数学，面向开发者](/understanding-reinforcement-learning-math-for-developers-b538b6ef921a)
*   [Q vs V 在强化学习中，最简单的方法](https://medium.com/p/9350e1523031)
*   [数学背后的强化学习，最简单的方法](https://medium.com/p/1b7ed0c030f4)
*   [动态编程中强化学习的简便方法](https://medium.com/@zsalloum/dynamic-programming-in-reinforcement-learning-the-easy-way-359c7791d0ac)
*   [蒙特卡洛强化学习，简单易行](https://medium.com/@zsalloum/monte-carlo-in-reinforcement-learning-the-easy-way-564c53010511)
*   [TD 在强化学习中，最简单的方法](/td-in-reinforcement-learning-the-easy-way-f92ecfa9f3ce)