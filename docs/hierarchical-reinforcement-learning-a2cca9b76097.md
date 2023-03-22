# 通过分层强化学习打造更聪明的智能体

> 原文：<https://towardsdatascience.com/hierarchical-reinforcement-learning-a2cca9b76097?source=collection_archive---------12----------------------->

如今围绕强化学习(RL)有很多炒作。我相信这是有充分理由的:RL 在雅达利电子游戏中取得了超人的表现，甚至已经击败了世界上最好的围棋选手。目前，RL 还被应用于系统的管理和配置、自动驾驶、机器人等领域。RL 会接管世界吗？还没有，首先有许多问题要处理:

*   **样本效率**:人类需要练习才能成为一个像样的视频游戏玩家，但 RL 代理需要的练习量是疯狂的！对于像 Breakout 这样的简单视频游戏，我们谈论的是数千小时的游戏时间，当然，当你有一个强大的服务器并且正在玩视频游戏时，这不是问题，但如果你需要一个像样的性能并且事先没有访问环境的权限，该怎么办？有许多不同的方法可以解决这个问题，如少量的镜头学习或从演示中学习，但这仍然是 RL 研究中的一个热门话题。
*   **缩放**:当状态和动作空间增大时，效率变差。如果你想到表格 Q-learning，每个动作有一列，每个状态有一行，所以如果你有 10 个状态和 10 个动作，那么你有 100 个 Q 值要计算。例如，当你处理视频游戏时，状态是由可能有 100x100 像素的游戏帧定义的(只是为了四舍五入)，其中每个像素可以取 0 到 255 之间的值，所以可能的状态数是 255 ⁰⁰⁰⁰，假设我们有 10 个动作，那么你将有 255 个⁰⁰⁰⁰⁰ q 值…哎呀。更糟糕的是，想想连续的状态或动作空间；这确实是一个复杂的问题。深度 RL 可以处理这种巨大的状态空间，但以一种简单有效的方式做到这一点仍然是一个挑战。
*   **泛化和迁移学习**:此外，RL 智能体可以很好地完成一项他们已经接受过训练的任务。如果他们开始新的任务，表现又会很糟糕。

正如我以前说过的，已经开发了许多方法来管理这些问题，分层强化学习就是其中之一。但是 HRL 到底是什么呢？

# 分层强化学习的拯救

![](img/f2556f6d4e36e41cc8736aae43923c1b.png)

走路并不像看起来那么简单【图片由 [Kira auf der Heide](https://unsplash.com/@kadh?utm_source=medium&utm_medium=referral) 上 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 。]

比方说，当你正在读这篇文章的时候，门铃响了。你要怎么办？嗯，你可以考虑站起来走向门口，但实际上，你要做的事情要复杂得多:你必须控制许多肌肉才能站起来，然后你必须走向门口，为此你必须再次控制你的肌肉，也要保持平衡，直到你走到门口。然而，你只考虑高层次的行为(起床，走向门口)，因为低层次的行为是自然而然的。

HRL 生来就有这种想法。不再只有一个策略来实现任务的目标，我们现在有几个子策略在一个层次结构中协同工作。这种方法带来许多好处:

*   改进探索:得益于高层次动作的使用，动作空间减少，探索更简单。
*   样本效率:状态也可以用分层的方式管理，低级别的策略可以对其高级别的策略隐藏不相关的信息。这也减少了状态空间，正如我之前解释的那样，这提高了采样效率。
*   迁移学习:子策略或低级策略可以在不同的任务中重用。假设一个代理学会了如何制作咖啡，但现在你想让它学习如何制作卡布奇诺，如果你将制作卡布奇诺的任务分成制作咖啡，然后加热牛奶并使其起泡，那么第一个机器人已经可以使用它制作咖啡的经验，这将加速这项新任务的学习。

在实践中有不同的方法来实现 HRL。我亲自挑选了一些，并为你做了一个总结:

## 期权框架

由萨顿和 T2·辛格于 1999 年引入的选项框架提供了一种在 RL 中实现层次和宏操作的方法。这可能是 HRL 最常见的表述之一。

在期权框架中，代理处于半马尔可夫决策过程(SMDP)中，这是马尔可夫决策过程的一种扩展，其中动作之间的时间不是常数，允许它为其动作包括不同级别的抽象。

选项框架引入了**选项**的概念，这是一个动作的概括，让我们可以引入宏动作。一个选项的例子可能是拿起一个物体(涉及某些肌肉抽搐)，但也可能是一个单一的肌肉抽搐。在这两个例子中，第一个是由多个原语动作组成的宏动作，第二个是原语动作。[萨顿等人(1999)](https://pdf.sciencedirectassets.com/271585/1-s2.0-S0004370200X00549/1-s2.0-S0004370299000521/main.pdf?X-Amz-Security-Token=IQoJb3JpZ2luX2VjEIP%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLWVhc3QtMSJGMEQCIESLGtl5GrhhtZzdj5yqOgLrGDYmpVYXrKly71u0oGyFAiBv%2FCP50jyEYbf%2F%2BE2HfV6EW7K5zBrv5JIzFhq9uCH6Eyq9Awi8%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F8BEAMaDDA1OTAwMzU0Njg2NSIMNYyrwvB33DZ1Kvq2KpED2FGegHwZmRk4sG7jfW1H78WdCz2G5%2BpEUrTz7aVuPOz0Zq0uPgCvB%2FKt%2FnIs0nxyfjamVx%2BTiEg7mXd8Kx7LV1EuFqSrFAIZnICMr5wmQowQ5D1XY%2BGCRRCgAsSi2BYqPtVpR6taAMgOwvaotyUXU0l6%2BVWE%2F5WJJEYkKdSYNhFGTLGwt0q7KtzO4B8n1BydcHuoeUmAfXptKjAQ8UFMEC2UnQNcBEz3tsN1SnC7%2BGvMi79YSxGypwzqa6SjZK1AyUQMRi%2F8whfXBereqN3ambFtnW0uGU9iuZi3MXWC3oZVQkrEmvXrqFeb21V0XZhprRG9NzDdpDmCrtuWWJsDu8XJTR8978wWKmHFPfnOMdQ6DvJ12vfy%2BwxNktNdVe8Tv61CwpYaHVU1n0D9h7VnGYjrA00XwBbdSd1HWv%2BTf88TMfRfHWi1emigaIsCFO8roZA8NOtsagNpLnKUuTrh89c1SeNGHrkyp9bKiEAe4Lztujruty1HSYm%2Bel0wUKdfpL4F%2FH7BtEHgq9bEwSiYrGYw3MzP9QU67AGxunsKSQrJkq6nGSSjWdrUrWMcvcBN6YtyOo7S5Ou%2F5s4WeZltWSkYMKa%2FbrbnLbNwvk%2BmMYUEfroS%2FNN51aaY03WfncGwo6DvX6S3llWno0E5mtLiF7x3JYXkilFnXnpcleWERzSP6ubqMLFYxyvsFAdgImdAK4DXxV89kmyht0tG%2FxQeQ1BULxqwbg8RlTIL0TTusUvCDy8Wu0zaRJpwxvwYZAWukK7VbnrGEkMZkaJ7iVs9EZBgMmw72HW7sRViY2X3qdfbJCI4Twzbx1T%2FXwhJ8RH50e0LpE%2B5CXwZIc5iHqee3pw5E7mWfA%3D%3D&X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Date=20200507T112434Z&X-Amz-SignedHeaders=host&X-Amz-Expires=299&X-Amz-Credential=ASIAQ3PHCVTY2WMI6KVY%2F20200507%2Fus-east-1%2Fs3%2Faws4_request&X-Amz-Signature=fbeea06d2fffc09501375dfd3c8ccb28725cbffa5f2776ac31e8fb35278d5d0a&hash=e3cf3d3e59d1e679b1771c52c7242cfd07f5faa8a499adb416f226a1b1297e89&host=68042c943591013ac2b2430a89b270f6af2c76d8dfd086a07176afe7c76c2c61&pii=S0004370299000521&tid=spdf-d8bd98f6-5625-46bc-b78e-9f168d0a531b&sid=2d141b7d833611469889b85080d0b045cbe8gxrqb&type=client) 介绍了期权的概念，并对其进行了如下描述:

> 选项由三部分组成:策略π : S × A → [0，1],终止条件β : S+ → [0，1],以及初始集 I ⊆ S。当且仅当 st∈ I 时，选项⟨I,π,β⟩在状态 st 中可用。如果选择了选项，则根据π选择动作，直到选项根据β随机终止。

这意味着每次观察到新的状态时，检查初始集 I 以查看当前状态是否属于它，如果是，则选项开始:从现在开始，π确定在每个步骤中采取的动作(而不是全局策略),并且在每个状态中，检查终止条件β，如果它返回 True，则选项结束，并且我们回到使用全局策略。在下图中，我们可以看到如何使用选项来管理不同时间长度的动作和状态。

![](img/133d1a41ae78066b8f31279d3b433236.png)

在 MDP，状态转换由相同持续时间的离散时间步骤表示，而在 SMDP，步骤具有不同的持续时间。选项允许我们以任何方式分析轨迹[图来自 [Sutton 等人(1999)](http://papers.nips.cc/paper/1586-learning-macro-actions-in-reinforcement-learning.pdf) 。]

每个问题的选项的定义留给每个用例，增加了 RL 要处理的问题定义的复杂性。此外，Options 框架没有明确考虑任务分段。

## [封建强化学习](http://papers.nips.cc/paper/714-feudal-reinforcement-learning.pdf)

封建强化学习(FRL)定义了一个控制层级，其中一个级别的管理者可以控制子管理者，同时这个级别的管理者又被超级管理者控制。每个经理为其子经理分配目标，子经理采取行动实现该目标并获得奖励。

![](img/39e3780eda604dd571ba7c39b79c65fe.png)

FRL 中管理人员的控制层级[图片来自 Pixabay。]

封建强化学习(FRL)定义了一个控制层级，其中一个级别的管理者可以控制子管理者，同时这个级别的管理者又被超级管理者控制。每个经理为其子经理分配目标，子经理采取行动实现该目标并获得奖励。

![](img/f1cd32259b0c89626d0d3c01dd5fba6d.png)

FRL 应用于一个迷宫[图来自于[达扬&辛顿(1993](http://papers.nips.cc/paper/714-feudal-reinforcement-learning.pdf) )。]

FRL 最初是为了解决一个迷宫问题。代理从迷宫中的任意位置开始，并且必须找到到达某个位置的路。经理的结构如左图所示，蓝色区域是任务的目标,“U”代表代理无法跨越的障碍。每个经理的行动是:北、南、东、西或*，*是一个特殊的行动，意味着经理应该在其区域内探索。在顶层，经理只能执行*,而在底层，经理只能执行 NSEW。学习在开始时自下而上进行，底层的每个经理都知道应该去哪里，然后还有一些自上而下的学习，优化迷宫中的整体轨迹。下图显示了每位经理学习的操作:

![](img/cb1f81b0e2fee1eb59fdb8680854ebf6.png)

每个经理都有自己的行动和学习到的概率，圆圈(或*)代表在分配给经理的区域内探索的行动，区域上的颜色代表去 NSEW 的行动。每个行动的面积与其概率成正比[图来自 from [达扬&辛顿(1993](http://papers.nips.cc/paper/714-feudal-reinforcement-learning.pdf) )。]

FRL 基于两个主要原则:奖励隐藏和(2)信息隐藏。奖励隐藏意味着经理必须奖励执行其命令的下级经理*，不管这是否满足了上级经理的命令。所以每个经理都学会了自己去满足上层。信息隐藏是指这样一个事实，即管理者只需要在他们自己选择的任务粒度上知道系统的状态。由于这一点，更高的级别在一个简化的状态下在更宽的粒度上工作。*

FRL 代表了 HRL 的一大进步，但它在不同领域的应用并不成功，在某些情况下效率非常低。

## [封建关系网(HRL 的乐趣)](https://dl.acm.org/doi/pdf/10.5555/3305890.3306047?download=true)

2015 年，DeepMind 带回了 FRL，并应用于 Deep RL。他们在 FRL 的原始架构和一些原则中启发自己:他们创建了一个模块化的 NN，其中有一个管理器，它从环境中接收状态和奖励作为输入，并输出状态的嵌入和工人必须实现的子目标。工人选择行动，并努力实现经理设定的目标。

## [期权批评家架构](https://arxiv.org/pdf/1609.05140.pdf)

Option-Critic 架构改进了期权框架，能够学习期权的内部政策和终止条件，而无需提供任何额外的奖励或子目标。

在这种情况下，假设选项能够在任何状态下启动，并且内部策略和终止条件已被学习，因此不必预先定义选项。选项上的策略目标是决定在每一步中使用什么选项，并让该选项的子策略处于控制中，直到选项策略返回终止条件。政策和终止条件是根据来自 Sutton 等人(2000 年)的基于政策梯度定理的解决方案来学习的。

此外，选择批评家架构是基于行动批评家架构的。其中有一个尝试动作的*演员*和一个*评论家*，他们评估每个动作有多好，并将其传达给演员。选项内策略、终止函数和对选项的策略属于系统的*参与者*部分，而*评论家*由 Q_U 和 A _ω组成，Q _ U 是在状态-选项对的上下文中执行动作的值，A _ω是对选项的优势函数。您可以在下图中看到该架构:

![](img/80094cfebb95b64cb5f3a2cf1a4bf188.png)

期权批评家架构[图来自 Bacon 等人(2016 年)。]

# 你自己试试

如果您想亲自尝试一下，我在 Tensorflow 上找到了 Option-Critic 架构的实现，您可以从 [Github](https://github.com/yadrimz/option-critic) 下载。资源库使用 OpenAI Gym，但是如果你想在其他环境中尝试，可以查看[这个 RL 环境列表](https://medium.com/@mauriciofadelargerich/reinforcement-learning-environments-cff767bc241f)。

感谢您的阅读！