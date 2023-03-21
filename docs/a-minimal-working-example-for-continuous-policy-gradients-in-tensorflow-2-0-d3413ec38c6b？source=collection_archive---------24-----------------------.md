# TensorFlow 2.0 中连续策略梯度的最小工作示例

> 原文：<https://towardsdatascience.com/a-minimal-working-example-for-continuous-policy-gradients-in-tensorflow-2-0-d3413ec38c6b?source=collection_archive---------24----------------------->

## 一个简单的训练高斯演员网络的例子。定义自定义损失函数并应用 GradientTape 功能，只需几行代码就可以训练演员网络。

![](img/3b769c4692ec204a195b93598e93ffe6.png)

列宁·艾斯特拉达在 [Unsplash](https://unsplash.com/@lenin33?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) 上的照片

目前设计和应用的所有复杂的行动者-批评家算法的基础是普通策略梯度算法，它本质上是一个只针对行动者的算法。现在，学习决策政策的参与者通常由神经网络来表示。在连续控制问题中，该网络输出相关的分布参数以采样适当的动作。

有这么多深度强化学习算法在流通，你可能会认为很容易找到大量即插即用的 TensorFlow 实现，用于连续控制的基本参与者网络，但事实并非如此。这可能有各种原因。首先，TensorFlow 2.0 于 2019 年 9 月才发布，与其前身有很大不同。第二，大多数实现关注离散的动作空间，而不是连续的动作空间。第三，流通中有许多不同的实现，但有些是定制的，因此它们只在特定的问题环境中工作。仔细阅读数百行充满占位符和类成员的代码，却发现这种方法根本不适合您的问题，这可能有点令人沮丧。本文基于我们的 ResearchGate 笔记[1]，提供了一个在 TensorFlow 2.0 中运行的最小工作示例。我们将向您展示真正的神奇只发生在三行代码中！

## 一些数学背景

在这篇文章中，我们提出了一个简单和通用的执行机构网络的背景下的香草政策梯度算法加强[2]。在连续变量中，我们通常从高斯分布中提取动作；目标是学习适当的均值 *μ* 和标准差 *σ* 。演员网络学习并输出这些参数。

让我们更正式地描述一下这个演员网络。这里，输入是状态 *s* 或特征数组 *ϕ(s)* ，后面是一个或多个变换输入的隐藏层，输出是 *μ* 和 *σ* 。一旦获得该输出，从相应的高斯分布中随机抽取动作 *a* 。由此，我们得到 *a=μ(s)+σ(s)ξ，*其中*ξ∾*𝒩*(0，1)。*

在采取行动 *a* 后，我们观察到相应的奖励信号 *v* 。与一些学习率α一起，我们可以将权重更新为提高我们策略的预期回报的方向。基于梯度上升的相应更新规则[2]由下式给出:

![](img/f60d3b73fedf8fe43957bb85c9905620.png)

如果我们使用线性近似方案*μ_θ(s)=θ^⊤ϕ(s】*，我们可以直接对每个特征权重应用这些更新规则。对于神经网络来说，我们应该如何执行这种更新可能不那么简单。

通过最小化损失函数来训练神经网络。我们通常通过计算均方误差(预测值和观测值之差的平方)来计算损失。例如，在一个批评网络中，损失可以定义为*(r*ₜ*+q*ₜ₊₁*-q*ₜ*)*，其中 *Q* ₜ是预测值，而 *r* ₜ *+ Q* ₜ₊₁是观测值。计算损失后，我们通过网络反向传播它，计算更新网络权重所需的部分损失和梯度。

乍一看，更新方程与这种损失函数几乎没有共同之处。我们只是试图通过向某个方向前进来改进我们的政策，但头脑中没有明确的“目标”或“真正的价值”。事实上，我们需要定义一个“伪损失函数”来帮助我们更新网络[3]。当将更新规则表达成其通用形式时，传统更新规则和该损失函数之间的联系变得更加清楚:

![](img/5d3b7dcf3367d09b96ca22d8336c459b.png)

转换成损失函数相当简单。由于损失仅为反向传播过程的*输入*，我们首先丢弃学习速率 *α* 和梯度 *∇_θ* 。此外，使用梯度*下降*而不是梯度*上升*来更新神经网络，因此我们必须添加减号。这些步骤产生以下损失函数:

![](img/5df7d5958f8104ff9b9a73f0069c3717.png)

跟更新规则挺像的吧？提供一些直觉:提醒对数转换对所有小于 1 的值产生一个负数。如果我们有一个低概率和高回报的行动，我们会希望观察到一个大的损失，即一个强烈的信号，将我们的政策更新到高回报的方向。损失函数正是这样做的。

为了应用高斯策略的更新，我们可以简单地用高斯概率密度函数(pdf)代替*π_θ*——注意，在连续域中，我们使用 pdf 值而不是实际概率——以获得所谓的*加权高斯对数似然损失函数*:

![](img/e89582aa5f64c77b3b8856b13517e004.png)

## TensorFlow 2.0 实施

现在数学已经讲得够多了，是实现的时候了。

我们刚刚定义了损失函数，可惜在 Tensorflow 2.0 中不能直接应用。在训练一个神经网络的时候，你可能习惯了类似于`model.compile(loss='mse',optimizer=opt)`，其次是`model.fit`或者`model.train_on_batch`的东西，但是这个不行。首先，高斯对数似然损失函数不是 TensorFlow 2.0 中的默认函数，它在 Theano 库中，例如[4]，这意味着我们必须创建一个自定义的损失函数。但是更具限制性:TensorFlow 2.0 要求损失函数正好有两个参数，`y_true`和`y_predicted`。正如我们刚刚看到的，我们有三个论点，因为与奖励相乘。让我们稍后再考虑这个问题，首先介绍我们的定制高斯损失函数:

所以我们现在有正确的损失函数，但是我们不能应用它！？我们当然可以——否则所有这些都是毫无意义的——只是与你可能习惯的略有不同。

这就是`GradientTape`功能的用武之地，它是 TensorFlow 2.0 [5]的新增功能。它本质上是在一个'*磁带'*上记录你前进的步伐，这样它就可以应用自动微分。更新方法包括三个步骤[6]。首先，在我们的自定义损失函数中，我们向前传递参与者网络(已记忆)并计算损失。第二，通过函数`.trainable_variables`，我们回忆在我们向前传递时发现的重量。随后，`tape.gradient`通过简单地插入损失值和可训练变量，为您计算所有梯度。第三，使用`optimizer.apply_gradients`我们更新网络权重，其中优化器是您选择的一个(例如，SGD、Adam、RMSprop)。在 Python 中，更新步骤如下所示:

所以最后，我们只需要几行代码来执行更新！

## 数例

我们给出了一个连续控制问题的最小工作示例，完整代码可以在我的 GitHub 上找到。我们考虑一个极其简单的问题，即只有一个状态和一个微不足道的最优策略的一次性博弈。我们越接近(固定但未知的)目标，我们的奖励就越高。奖励函数形式上表示为 *R =ζ β /* max *(ζ，|τ - a|)* ，其中 *β* 为最大奖励， *τ* 为目标， *ζ* 为目标范围。

为了表示演员，我们定义了一个密集的神经网络(使用 Keras ),该网络将固定状态(值为 1 的张量)作为输入，使用 ReLUs 作为激活函数(每层五个)在两个隐藏层中执行变换，并返回 *μ* 和 *σ* 作为输出。我们初始化偏置权重，使得我们从 *μ* =0 和 *σ* =1 开始。对于我们的优化器，我们使用 Adam，其默认学习率为 0.001。

下图显示了一些运行示例。请注意，收敛模式符合我们的预期。起初，损失相对较高，导致 *μ* 向更高回报的方向移动，σ增加，允许更多的探索。一旦达到目标，观察到的损失减少，导致 *μ* 稳定， *σ* 下降到接近 0。

![](img/98c20adb53f8fd6679937a996850798a.png)

收敛到目标 *μ* (作者自己的工作[1])

## 要点

*   政策梯度法不适用于传统的损失函数；我们必须定义一个伪损失来更新行动者网络。对于连续控制，伪损失函数只是 pdf 值乘以回报信号的负对数。
*   几个 TensorFlow 2.0 更新函数只接受正好有两个参数的自定义损失函数。`GradientTape`功能没有这种限制。
*   使用三个步骤来更新演员网络:(I)定义定制损失函数，(ii)计算可训练变量的梯度，以及(iii)应用梯度来更新演员网络的权重。

*本文部分基于我在 ResearchGate 上发表的论文:* [*在 TensorFlow 2.0*](https://www.researchgate.net/publication/343714359_Implementing_Gaussian_Actor_Networks_for_Continuous_Control_in_TensorFlow_20) *中实现用于连续控制的高斯行动者网络。*

*GitHub 代码(使用 Python 3.8 和 TensorFlow 2.3 实现)可以在我的* [*GitHub 资源库*](http://www.github.com/woutervanheeswijk/example_continuous_control) *找到。*

*希望实施离散变量或深度 Q 学习？退房:*

[](/a-minimal-working-example-for-discrete-policy-gradients-in-tensorflow-2-0-d6a0d6b1a6d7) [## TensorFlow 2.0 中离散策略梯度的最小工作示例

### 一个训练离散演员网络的多兵种土匪例子。在梯度胶带功能的帮助下…

towardsdatascience.com](/a-minimal-working-example-for-discrete-policy-gradients-in-tensorflow-2-0-d6a0d6b1a6d7) [](/a-minimal-working-example-for-deep-q-learning-in-tensorflow-2-0-e0ca8a944d5e) [## TensorFlow 2.0 中深度 Q 学习的最小工作示例

### 一个多臂土匪的例子来训练一个 Q 网络。使用 TensorFlow，更新过程只需要几行代码

towardsdatascience.com](/a-minimal-working-example-for-deep-q-learning-in-tensorflow-2-0-e0ca8a944d5e) 

## **参考文献**

[1] Van Heeswijk，W.J.A. (2020)在 TensorFlow 2.0 中实现连续控制的高斯行动者网络。*[https://www . research gate . net/publication/343714359 _ Implementing _ Gaussian _ Actor _ Networks _ for _ Continuous _ Control _ in _ tensor flow _ 20](https://www.researchgate.net/publication/343714359_Implementing_Gaussian_Actor_Networks_for_Continuous_Control_in_TensorFlow_20)*

*[2] Williams，R. J. (1992)连接主义强化学习的简单统计梯度跟踪算法。机器学习，8(3–4):229-256。*

*[3] Levine，S. (2019)加州大学伯克利分校 CS 285 深度强化学习:政策梯度。[http://rail . eecs . Berkeley . edu/deeprlcourse/static/slides/LEC-5 . pdf](http://rail.eecs.berkeley.edu/deeprlcourse/static/slides/lec-5.pdf)*

*[4]the nets 0 . 7 . 3 文档。高斯对数似然函数。[https://the nets . readthe docs . io/en/stable/API/generated/the nets . losses . Gaussian loglikelihood . html # the nets . losses . Gaussian loglikelihood](https://theanets.readthedocs.io/en/stable/api/generated/theanets.losses.GaussianLogLikelihood.html#theanets.losses.GaussianLogLikelihood)*

*[5] Rosebrock，A. (2020)使用 TensorFlow 和 GradientTape 来训练 Keras 模型。[https://www.tensorflow.org/api_docs/python/tf/GradientTape](https://www.pyimagesearch.com/2020/03/23/using-tensorflow-and-gradienttape-to-train-a-keras-model/)*

*[6]南丹，A. (2020)演员评论家法。[https://keras.io/examples/rl/actor_critic_cartpole/](https://keras.io/examples/rl/actor_critic_cartpole/)*