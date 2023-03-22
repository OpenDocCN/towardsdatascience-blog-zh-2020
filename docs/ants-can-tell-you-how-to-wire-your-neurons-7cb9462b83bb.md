# 蚂蚁可以告诉你如何连接你的神经元

> 原文：<https://towardsdatascience.com/ants-can-tell-you-how-to-wire-your-neurons-7cb9462b83bb?source=collection_archive---------48----------------------->

## 群体智能在神经结构搜索中的应用

蚂蚁启发的算法可以用来确定你的大脑启发神经网络模型的最佳架构。自然无疑是技术的缪斯。

![](img/c9a0ca64249ae0a6a710500d8ed6e519.png)

图片由 [Tworkowsky](https://pixabay.com/pl/users/Tworkowsky-3251663/?utm_source=link-attribution&utm_medium=referral&utm_campaign=image&utm_content=2028276) 来自 [Pixabay](https://pixabay.com/pl/?utm_source=link-attribution&utm_medium=referral&utm_campaign=image&utm_content=2028276)

这里我们将讨论以下主题:

*   蚂蚁如何找到寻找食物来源的最佳路线，
*   一个蚂蚁启发的优化算法的例子，
*   如何应用此算法为人工神经网络模型找到最佳架构。

# 蚂蚁的觅食行为

蚂蚁如何找到到达食物源的最短路线？它当然不会从蚁穴里看到它，因为它的目标通常在几十米之外，而蚂蚁本身还不到一厘米长。它必须完全根据*有限公司和当地的导航信息* [](https://www.researchgate.net/publication/215444190_Self-organized_shortcuts_in_the_Argentine_Ant_Naturwissenschaften_76_579-581)来选择它的长途行程。

这个秘密在 1989 年被揭开，并在阿根廷蚂蚁 *的一篇简洁而激动人心的论文 [*中呈现。*作者进行了一个优雅的实验，证明蚂蚁依赖于蚁群成员在可用路线上留下的**信息素水平**中表达的**集体经验**。](https://www.researchgate.net/publication/215444190_Self-organized_shortcuts_in_the_Argentine_Ant_Naturwissenschaften_76_579-581)*

一个蚁巢通过一个特殊的桥与一个有食物的竞技场相连，桥由两个相同的模块组成:

![](img/7482f926dd6498db2a663392aaf0d0d3.png)

从上面看，放置在蚁群和食物竞技场之间的双模块桥的单模块；[来源](https://www.researchgate.net/publication/215444190_Self-organized_shortcuts_in_the_Argentine_Ant_Naturwissenschaften_76_579-581/link/0c96051813ff79ee9a000000/download)

正如你所看到的，一个模块有两个长度明显不同的分支，但是在分支点上没有任何区别:与主桥轴的角度是相同的。因此，蚂蚁无法预先知道哪个选项更好。

为了消除朝向左侧或右侧的潜在偏置(即，以防由于某些未知原因蚂蚁更喜欢一侧)，第二模块以其较长分支在另一侧的方式连接到第一模块:

![](img/06f39cfa5465cdc51c60633b749883c3.png)

从上面看，整个蚁群和食物竞技场之间的桥梁；改编自[来源](https://www.researchgate.net/publication/215444190_Self-organized_shortcuts_in_the_Argentine_Ant_Naturwissenschaften_76_579-581/link/0c96051813ff79ee9a000000/download)

面对这样的设置，蚂蚁最初会随机选择路线。20 秒钟后，第一批通过较短路径过桥的蚂蚁开始返回。那些再次选择短路线的将会是第一个返回巢穴的人，也是第一个沿着整个巢穴——食物——巢穴路径放置信息素的人。这将激励其他蚂蚁选择较短的路径(标记有较高水平的信息素)。

neat 实验的结果最好通过下面的照片来捕捉:

![](img/b23159ab1cab204428633b838054f0c0.png)

放置蚁巢和食物之间的桥后 4 分钟和 8 分钟拍摄的照片；[来源](https://www.researchgate.net/publication/215444190_Self-organized_shortcuts_in_the_Argentine_Ant_Naturwissenschaften_76_579-581/link/0c96051813ff79ee9a000000/download)

你必须仔细观察才能注意到这些小黑点——蚂蚁们正忙着给蚁群提供食物。左边的照片是在大桥开通后不久拍摄的——你可以看到蚂蚁随机分布在两条道路之间。在稍后拍摄的第二张照片中，蚂蚁已经集中在较短的路线上。

# 蚁群系统

蚂蚁的这种行为启发了整个算法家族，统称为蚁群优化算法，属于更广泛的称为群体智能的方法。所有这些技术的共同特点是，它们依赖于没有独立控制单元的**分散**系统。系统中个体代理之间的局部交互导致全局智能行为。

![](img/d7ada63d74f8e857a2345bbf73e4490d.png)

图片来自 [Pixabay](https://pixabay.com/pl/?utm_source=link-attribution&utm_medium=referral&utm_campaign=image&utm_content=2838690) 的 [Ratfink1973](https://pixabay.com/pl/users/Ratfink1973-5627178/?utm_source=link-attribution&utm_medium=referral&utm_campaign=image&utm_content=2838690)

在这里，我将介绍一种叫做蚁群系统(ACS)的算法，它是由 Marco Dorigo(T1)在 1997 年提出的，作为旅行推销员问题的解决方案:在一个由不同长度的道路连接的城市网络中寻找连接所有城市的最短路线。

## ACS 算法概述

简而言之，该算法执行以下操作:

*   一些蚂蚁被随机放置在图的一些节点上。
*   蚂蚁开始在图中穿行，根据随机的**状态转移规则**选择节点(它们喜欢信息素水平高的短边)。
*   在蚂蚁行走的过程中，信息素会根据一个**局部更新规则**沿着它的路径蒸发(这是为了鼓励其他蚂蚁尝试其他路径)。
*   在所有蚂蚁完成它们的行走后，信息素的量根据**全局更新规则**再次更新(整个路径越短，属于该路径的每条边上的信息素沉积越大)。

## 详细的状态转换规则

首先让我们介绍下面的符号:

![](img/2536f1a4bfc1cffe8337b2155acaa93a.png)

放置在节点 *r* 中的蚂蚁根据以下**状态转移规则**选择下一个节点 *s* :

![](img/8349554c02f23d44935490acaf8cf634.png)

状态转换规则

其中:

*   *q* 是均匀分布在[0，1]中的随机变量，
*   *q₀* 为参数(0 ≤ *q₀* ≤1)，
*   S 是从下面定义的概率分布中抽取的随机变量:

![](img/0415fd08f16eb076895bf1f37ce8f1f8.png)

让我们仔细看看这些方程。如前所述，在选择下一个节点时，蚂蚁更喜欢具有较高信息素水平 *τ(r，s)* 和较低长度 *δ(r，s)* 的边 *(r，s)* (因此更高的 *η(r，s)= 1 / δ(r，s)* 。因此，选择给定边 *p(r，s)* 的概率应该与 *τ(r，s)* 和 *η(r，s)*成比例

![](img/19c2fdc8b4f69726e5760d8e10806d29.png)

为了控制两个因素(信息素水平 *τ* 和边长 *δ* 的相对重要性，引入了参数 *β > 0* :

![](img/233a87f4ed6c89ed61a062e39220ca52.png)

*β* 越大，距离比信息素水平越重要，即长距离将比信息素的吸引力具有更高的排斥力。

为了得到一个可以解释为概率的变量，我们必须确保它的值在 0 和 1 之间。这可以通过将
*τ(r，s) η(r，s)ᵝ* 除以从节点 *r* 可访问且尚未被蚂蚁 *k* 访问的所有节点 *u* 的相似表达式之和来完成。如果我们用 *Jₖ(r)* 来表示这些节点的集合，那么蚂蚁 *k* 从节点 *r* 中选择节点 *s* 作为其下一个目的地的概率 *pₖ(r，s)* 的完整公式由前面引用的公式给出:

![](img/0415fd08f16eb076895bf1f37ce8f1f8.png)

只有在所谓的*有偏探测*阶段，节点之间的转换才根据该概率发生。在*开发*阶段，蚂蚁的行为是确定的:它选择最佳边，即具有最高 *τ(r，s) η(r，s)ᵝ.)的边*

## 信息素更新规则详细

在其行走期间，蚂蚁根据局部更新规则改变每个被访问边上的信息素水平:

![](img/d8dd0fae0fe159645c89f476dcb68b57.png)

局部信息素更新规则

其中 *ρ* 表示信息素衰减因子 *τ₀* 是初始信息素值。信息素的蒸发诱使其他蚂蚁尝试其他路径。

在所有蚂蚁完成它们的路线后，找到最佳解的蚂蚁根据全局更新规则沿着它的路径存放信息素:

![](img/6ad14dc1521b9dc5a5dd1a3fb938d725.png)

全局信息素更新规则

其中描述了:

*   由参数 *0 < α < 1* 调节的信息素蒸发
*   信息素沉积与从试验开始的全局最佳旅程的长度成反比 *L_bg。*

# 神经结构搜索

![](img/5b7737802a15d5f771c8ebd5978f4e39.png)

图片来自 [Pixabay](https://pixabay.com/pl/?utm_source=link-attribution&utm_medium=referral&utm_campaign=image&utm_content=3979490) 的 [Borko Manigoda](https://pixabay.com/pl/users/borevina-9505414/?utm_source=link-attribution&utm_medium=referral&utm_campaign=image&utm_content=3979490)

每当你想出一个神经网络架构，你必须执行超参数搜索:找到学习率，批量大小，学习率调度等。您的网络在哪些方面表现最佳。为什么不扩大搜索范围来帮助您找到最佳架构呢？这种方法被称为神经架构搜索(NAS)，可以通过各种技术实现，如强化学习和进化算法。当然，你必须在搜索空间中引入一些约束条件，例如，定义算法应该考虑哪种层以及网络应该有多大。

# 如何将蚁群系统应用于神经架构搜索？

![](img/a1dc95c6799453e01341155df339ad20.png)

塞巴斯蒂安·斯塔姆在 [Unsplash](https://unsplash.com/?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) 上的照片

总体思路是这样的:你决定你对哪种层感兴趣(例如，你只想要你的模型中的卷积、汇集和全连接层)以及你的模型应该有多深。使用一些“蚂蚁”(独立的代理)，你逐渐构建一个图，其中每个节点对应一个神经网络层，每条路径对应一个架构。“蚂蚁”会留下“信息素”，这样“更好”的路径会在接下来的步骤中吸引更多的“蚂蚁”。[哪个型号胜出]

让我们看看在 [DeepSwarm 的论文](https://arxiv.org/pdf/1905.07350.pdf)中是如何实现的，这篇论文为 CNN 将 ACS 应用于 NAS。论文中的下图给出了一个很好的概述:

![](img/baa2aa498526eef37eaee11b101eb4ef.png)

[来源](https://arxiv.org/pdf/1905.07350.pdf)

让我们深入细节。DeepSwarm 算法从一个仅包含输入节点(输入层)的图开始，并在该节点中放置许多“蚂蚁”。每个“蚂蚁”根据前面讨论的 ACS 状态转换规则选择它下一步要去的节点。然后，所选节点被添加到图中，并且“蚂蚁”选择其属性(例如，在节点对应于 CNN 层的情况下，过滤器大小和步幅)。属性的选择也基于 ACS 状态转换规则。

一旦蚂蚁的路径达到当前最大长度，对应于该路径的神经网络结构被评估，并且蚂蚁执行 ACS 局部信息素更新，该更新衰减信息素，从而鼓励其他蚂蚁探索其他路径。

一旦所有的蚂蚁完成了它们的行走，所有架构的精确度被比较，并且架构产生最高精确度的蚂蚁执行 ACS 全局信息素更新，从而增加其路径上的信息素水平。

当前最大路径长度增加，新的蚂蚁群体从输入节点释放。这将持续到当前最大路径长度达到用户定义的模型最大允许深度。

# 结论

蚂蚁或蜜蜂等群居昆虫的生活不仅本身令人着迷，还为许多属于群体智能技术的算法提供了灵感。在这里，我们讨论了这样一种方法的细节:蚁群系统，我们描述了如何将它应用于寻找神经网络的最佳结构的问题。

# 参考

[1]戈斯，s &阿隆，塞尔日&德涅堡，让-路易&帕斯特尔斯，雅克。(1989).*阿根廷蚂蚁*中的自组织捷径。自然科学杂志 76:579–581。自然科学。76.579–581.10.1007/BF00462870。；[链接](https://www.researchgate.net/publication/215444190_Self-organized_shortcuts_in_the_Argentine_Ant_Naturwissenschaften_76_579-581)

[2]多利戈，马尔科&甘巴尔代拉，卢卡玛丽亚。(1997).Gambardella，L.M.: *蚁群系统:旅行商问题的合作学习方法*。IEEE Tr。伊沃。比较。1, 53–66.进化计算。1.53–66.10.1109/4235.585892.；[链接](http://people.idsia.ch/~luca/acs-ec97.pdf)

[3] Byla，Edvinas & Pang，Wei .(2019).使用群体智能优化卷积神经网络。；[链接](https://arxiv.org/pdf/1905.07350.pdf)