# 为神经结构搜索的权重共享辩护:一个优化的观点

> 原文：<https://towardsdatascience.com/in-defense-of-weight-sharing-for-neural-architecture-search-an-optimization-perspective-72af458a735e?source=collection_archive---------43----------------------->

## 如何优化正确的几何形状，以实现更好、更快的神经结构搜索。

CMU 和决心艾的合作作品由米沙·克达克和利亚姆·李共同创作。

![](img/757630c1a7def0c89b034042addac203.png)

**图 1:** 动画演示飞镖和其他权重分配方法如何用其输出的 *θ* 加权组合来替换四个运算 *o ∈ O* 之一到边 *e* 的离散分配。在网络的每个边 *e* 处，输入节点 *eᵢₙ* 处的值传递给 *O* 中的每个操作={Conv 3x3，Conv 5x5，池 3x3，跳过连接}；输出节点 eₒᵤₜ处的值将是由满足σ*ₒθₑₒ*= 1 的参数*θₑₒ*∈【0，1】加权的运算输出的总和。

神经架构搜索(NAS)——选择哪种神经模型用于你的学习问题——是一个有前途但计算昂贵的机器学习自动化和民主化的方向。[权重分配方法](http://proceedings.mlr.press/v80/pham18a.html)最初在大幅加速 NAS 方面的成功让该领域的许多人感到惊讶，但由于其作为完整模型训练的替代品的糟糕表现(一种被称为*等级混乱*的错误关联问题)以及最近基准测试的不一致结果，它已经受到了审查。在本帖中，我们简要介绍了重量共享，并支持在 NAS 中继续使用它。为此，我们将考虑一个简单的优化公式，它揭示了以下关键要点:

1.  对于一个支持过参数化模型的非凸优化的社区来说，权重共享工作的事实不应该太令人惊讶。
2.  对于大多数权重分配方法来说，等级无序并不重要，因为我们关心的是获得高质量的架构，而不是它们的等级。
3.  重量共享的性能有时很差，这是优化问题的结果，可以在仍然使用重量共享的情况下修复这些问题。我们提出了这样一种修复方法——几何感知指数算法(GAEA ),它适用于许多流行的 NAS 方法，并在多种设置下实现了最先进的结果。

# 具有重量共享的 NAS 简史

NAS 通常被公式化为一个双层优化问题，其中我们在某个体系结构域 *A* 中搜索实现最低验证损失的体系结构*a∈a**ℓⱽ(wₐ，a)* 在训练最小化训练损失的权重 *wₐ* 之后:

![](img/487c8c901029774f580bfc3c507556b1.png)

第一代 NAS 方法非常昂贵，因为搜索空间非常大，需要训练数千个神经网络才能完成。然后，在他们的 2018 ENAS(针对*高效* NAS) [论文](http://proceedings.mlr.press/v80/pham18a.html)中，Pham 等人引入了权重共享的思想，其中只有一个*共享*组模型参数 *w ∈ ℝᵈ* 被训练用于所有架构。使用这些共享权重计算的不同架构的验证损失 *ℓⱽ(w，a)**a∈a*然后被用作使用独立权重 *wₐ* (即通过解决上述内部优化问题为每个架构单独训练的权重)的验证损失 *ℓⱽ(wₐ，a)* 的*估计值。因为只需要训练一组参数，重量共享导致了比早期方法大幅度的加速，将 CIFAR-10 上的搜索时间从[2000-20000](https://arxiv.org/abs/1707.07012)GPU 小时减少到仅 16 小时。令许多人惊讶的是，使用共享权重计算的验证精度 *ℓⱽ(w，a)*w*足以替代 *ℓⱽ(wₐ，a)* ，这意味着 ENAS 能够廉价地找到好的模型。我们将会看到，这种相关性实际上是重量共享做得好的一个充分条件，但不是必要条件，重量共享的整体成功并不像最初想象的那么令人惊讶。

继 ENAS 突破之后，提出了几个更简单的方法，例如[飞镖](https://openreview.net/forum?id=S1eYHoC5FX)和 [GDAS](http://openaccess.thecvf.com/content_CVPR_2019/papers/Dong_Searching_for_a_Robust_Neural_Architecture_in_Four_GPU_Hours_CVPR_2019_paper) ，其中分类架构决策(例如，对于每个网络边 *e ∈ E* ，在 *e* 处使用的某个固定操作集合 *O* 中的哪些操作被放宽到连续参数*θ∈θ*(例如，因此*θ*是|的乘积如图 1 中的动画所示，这些架构参数控制用于使用训练损失的梯度来更新共享权重的架构；例如，在飞镖游戏中， *θₑₒ* 决定了网络中边 *e* 输出的操作的加权和中的权重。总的来说，这种参数化导致了早期双层问题的连续松弛:

![](img/6994191b942a396e8ba3ef2f5a4fc043.png)

由于*θ*是 *ℝᴱᴼ* 的约束子集，DARTS 和 gda 通过重新参数化“logit”参数 *α ∈ ℝᴱᴼ* 来避免单纯形投影，其中 *θ =* Softmax *(α)* 定义为

![](img/342c450fa7d8c412baebe75dcb584860.png)

然后，可以通过下面的交替梯度更新方案(这里η > 0 是步长)来近似松弛的优化问题:

![](img/ebd1edd70375672f0b83a444ea4196d1.png)

注意，在搜索的最后，我们需要从架构权重 *θ* 中恢复出一个离散架构*A∈A*；在 DARTS 中，这是在修剪步骤中完成的，该步骤简单地选择最高权重的操作。这种事后修剪是我们的方法 GAEA 可以改善的误差源，我们将在后面讨论。

进一步的简化，也许是使用共享权重作为独立权重替代的最显著的例子，是[权重共享随机搜索](https://arxiv.org/abs/1902.07638) (RS-WS)方法，其中共享参数通过使用从搜索空间均匀随机采样的架构采取梯度步骤进行优化。尽管没有更新架构参数，RS-WS 还是取得了有竞争力的，在某些情况下，是最先进的结果。

# 我们应该使用重量共享吗？

最近，体重分担受到了越来越多的关注:在模特之间分担体重真的能加速 NAS 吗？它会阻碍最佳架构的恢复吗？特别是，[几篇](https://openreview.net/forum?id=SJx9ngStPH) [论文](https://arxiv.org/abs/2001.01431)已经[观察到](https://arxiv.org/abs/2002.04289)出现了[排名混乱](https://openreview.net/forum?id=H1loF2NFwr)的问题，即当架构的共享权重性能与其独立性能没有很好的关联时；下图说明了这个问题。对于诸如 RS-WS 之类的依赖于共享权重性能来对架构进行排名以进行评估的方法来说，排名无序是一个问题，因为当其参数在没有共享的情况下被训练时，这将导致它们忽略实现高准确度的网络。

![](img/1c871978772531ef3f4bb024213f0b3f.png)

**图 2:**Yu et al .[2020](https://openreview.net/forum?id=H1loF2NFwr)排名混乱问题的图示当使用共享权重评估的架构的性能排序(右)与从零开始训练单个权重时的真实架构性能(左)不匹配时，就会出现这种情况。

最近一些著名的减肥方法表现不佳的案例强化了这种怀疑；特别是，在最近的[鲁棒性评估](https://openreview.net/forum?id=H1gDNyrKDS)中，发现 DARTS 过度适合上层验证集，并且在 [NAS-Bench-201](https://openreview.net/forum?id=HJxyZkBKDr) 上，给定类似的时间预算，GDAS 和 DARTS 都优于标准超参数优化方法。这里 DARTS 的性能特别差，收敛到只包含身份映射(跳过连接)的架构。

考虑到体重共享引发的问题和最近糟糕的结果，是时候重新考虑它的使用了吗？在下一节中，我们给出了否定的答案，表明(a)权重共享“超网”的性能与完全训练的模型的性能之间的相关性是权重共享起作用的充分但非必要条件，即*我们不需要害怕排序无序*；以及(b)使用权重共享获得高质量的架构解决方案*主要归结为正则化和优化*，这是机器学习的两个众所周知的方面。在最后一节中，我们快速强调了 NAS 中最近在正则化方面的一些工作，然后介绍了我们自己在优化方面的工作。

# 普通的重量分担只是经验风险最小化

权重共享之所以成为可能，是因为与正则化和步长等超参数不同，架构参数直接影响损失函数，因为从一种架构 *a ∈ A* 改变到不同的架构*a’∈a*导致损失从 *ℓ(w，a)* 改变到 *ℓ(w,a')* ，因为在后一种情况下，不同的函数被用于预测。另一方面，改变步长设置不会改变损失，除非通过使用新的步长训练 *w* 来改变权重；这意味着不同的超参数设置不再共享权重。

事实上，我们可以利用架构参数可以被归入超网的参数这一事实，在由权重 *w ∈ ℝᵈ* 和架构参数*θ∈θ*编码的一类扩展函数上，将具有权重共享的 NAS 作为经验风险最小化(ERM):

![](img/feccebff89de0f58641a121551c11271.png)

大多数(但不是所有的)关于 NAS 的经验工作都使用前面描述的两层公式，而不是解决这个单层 ERM 问题。然而，我们认为 ERM 应该是 NAS 研究的基线对象，因为它在统计和算法上更容易理解；更常见的[双层优化可以被视为一种(可能是关键的)通过分割数据进行正则化的方法](https://www.automl.org/wp-content/uploads/2020/07/AutoML_2020_paper_46.pdf)。

单级公式清楚地表明，NAS 可能会因为 ERM 失败的两个常见原因之一而失败，而不是等级无序:优化失败或泛化能力差。例如，这些各自的故障可以在很大程度上解释前面讨论的 NAS-Bench-201 和 NAS-Bench-1Shot1 上的 DARTS 所面临的问题。当然，超网可能面临优化和泛化问题并不奇怪，因为它们是非凸和过度参数化的模型；但是 NAS 从业者通常很舒服的训练正则深网，它也是非凸的，参数几乎一样多。一个主要的区别是，在后一种情况下，我们已经进行了多年的密集努力来设计[正则化](https://papers.nips.cc/paper/563-a-simple-weight-decay-can-improve-generalization) [方案](http://jmlr.org/papers/v15/srivastava14a.html)和[优化](http://proceedings.mlr.press/v28/sutskever13.html) [算法](https://arxiv.org/abs/1412.6980)；如果我们在这两个问题上投入类似数量的 NAS 研究，那么我们可能不会比训练标准深网更害怕重量共享。

此讨论的一个注意事项是，NAS 面临着从不断放松的体系结构权重中恢复离散体系结构的额外挑战。我们接下来提出的优化方案通过首先提升稀疏架构参数来改善这个问题。

# 通过权重共享固定可区分 NAS:一种几何感知方法

我们上面的讨论将设计 NAS 方法的问题简化为为超网设计良好的正则化和优化方案的问题。关于超网的更好的正则化方案，最近有大量的工作，包括[部分信道连接](https://openreview.net/forum?id=H1gDNyrKDS)，[惩罚确认损失的海森](https://openreview.net/forum?id=H1gDNyrKDS)，以及双层公式本身；因此，我们转而专注于改进优化方案，这是通过我们的几何感知指数算法(GAEA)来实现的。

像往常一样，我们希望有一个优化方法，能够更快地收敛到更好的解决方案。在权重共享 NAS 的情况下，高质量的解决方案不仅具有良好的泛化能力，还会导致稀疏的架构权重*θ∈θ*，其召回与通过 softmax 优化的参数 *α ∈ ℝᴱᴼ* 有关。我们期望稀疏架构参数更好，因为如前所述，架构参数在后处理步骤中被舍入，以导出最终的分立架构。

![](img/8b24bc91a02c6fecfc0a6b46804147a5.png)

**图 3:** 我们希望最终的架构权重 *θ* 是稀疏的，以便在舍入时，得到的离散架构 *a ∈ A* 接近由 *θ* 编码的超网。否则，分立架构的确认损失与超网的确认损失之间的差异可能非常不同。由于*θ*的元素位于单形的乘积中，稀疏性意味着，在每个单形中，单个元素为 1，其余元素为 0。

为了实现这一点，我们使用[指数梯度下降](https://pdf.sciencedirectassets.com/272575/1-s2.0-S0890540100X00648/1-s2.0-S0890540196926127/main.pdf?X-Amz-Security-Token=IQoJb3JpZ2luX2VjEKD%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLWVhc3QtMSJHMEUCIQD7%2BtHFNUL7RXYPJ0EDCm3b1MAsXlCnZB3Af0knFN7XfwIgX%2B71daM8A8Xs74oA1F9BN7L2MdX30ynAuwa2vi9JubYqtAMISRADGgwwNTkwMDM1NDY4NjUiDJrntPy%2Btpy9pllYliqRA%2FnQ1fCJZf1myhJd2TQzWswcCWAv%2FQntgZdm2QqNtxnhSv0nAByRpO3xi22KKJwg%2BCLPnYrZUqxa4P4SHmqvulAI%2Fxf3l6uZ7wspesGp%2FMDBv9V3kCK6dcK3w4mpNks6S8yWqzpaS0BxPXLKxA9KarZR2uoY%2B4ocQLNOe6XRWgI847lEb9P%2BTQMc01RszzqexHl9B2YiCkV9Tc7BYSWgn0F4k0gifjkESbKB1rQ%2BISmegkffzK1X%2FUgoO75yALnzAMu9qudwQzsfS%2Ftp%2BHwZxnvPnTh3JSguAzD4mA9S7UYSocZIAYVyajcCM6jZAqfMd9H4xCsvtvqnj6BvMft%2B1MzYOrSVNSJofR01U9oyCAapyLVg1QWyoMVjUk9hN0XpZjldXoX1MFSCS%2BBn6ZPJwXSpl6w4PSDG7u%2BBk6bMiZzsVIggEPd5rCELVeQFdxJrkqUAdoiK8H0zvHjtlFonXoVkr4%2Fe4h7%2BCLBMwdFAHFUvhbqEGQfJsS%2BNjyBzvKUsEmbISKWPjhzStsTMGtGztpvPMJKyp%2FgFOusBAe3cKtpSi6K%2Bl%2BD2rY16%2Fs80QxiEAFjOW4KHhWd3JFv8oUcum7KgOqmDtF3nfxBVRdkfQKZyg1fEIPMExldSSzxoJbseZBG8IvbZ7e41a0lORahGPSgHD69Anphbyb3XzmEX%2B7KdCGYbMDRWm6mO5%2FH8u1P9IplyiHZI4JdyK5Ofim9%2BYsdAmaF0RIxMncLkhpZ1MION6mtkn4yuzqpwW%2FCXui2EMWJY5hWUJj5sN%2B0QSkHPT%2BcLJ%2FalcKHEXFBbuS0X3XoVOopsvTZZUD5uNFQU7qOzgKOkL711LbBma0Lu%2Fcpx8fLHvRHhfQ%3D%3D&X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Date=20200711T162208Z&X-Amz-SignedHeaders=host&X-Amz-Expires=300&X-Amz-Credential=ASIAQ3PHCVTYWZDVZUM7%2F20200711%2Fus-east-1%2Fs3%2Faws4_request&X-Amz-Signature=40b87202e72cb5f0d6ce7e70cd60faf656a7510b700cbd87a7ca95230ea5eb20&hash=60545d1176fe9465628c11f98e59d807c0541a838531a443e9b9311d1fe87e06&host=68042c943591013ac2b2430a89b270f6af2c76d8dfd086a07176afe7c76c2c61&pii=S0890540196926127&tid=spdf-a878840f-3818-4937-b5e7-c08bf521cf0a&sid=749b22f7830723476d9a089-8d75fac41b81gxrqa&type=client)来直接优化元素*θ∈θ*而不是无约束值 *α ∈ ℝᴱᴼ* 。更新方案用逐元素乘以负指数梯度 w.r.t. *θ* (4.a)来代替减去梯度 w.r.t. *α* (伪代码中的第 4 行)，然后投影到包含 *θ* (4.b)的单纯形:

![](img/9d748fd0da59b1a9269494cdd09cf455.png)

请注意，每次迭代的开销大致与 SGD 中的一样。

对于凸问题，已知指数梯度非常适合单纯形几何，其中[迭代复杂度](https://web.iem.technion.ac.il/images/user-files/becka/papers/3.pdf)仅对数地取决于单纯形的大小 *k* ，而不是梯度下降的 O *(k)* 依赖性。在线性预测结果的镜像下降视图([视频链接](https://www.youtube.com/watch?v=uylS0FiuCK4))下，这种改进源于隐式正则化器在远离稀疏目标解时鼓励更大的更新。对于我们的非凸问题，我们通过扩展[张&何](https://arxiv.org/abs/1806.04781)最近的镜像下降结果来获得类似的保证，以表明将对架构参数的指数更新与对共享权重的 SGD 更新交替进行会在 O(log *k* / *ε* 次迭代中产生*ε*-驻点。我们还在图 4 中实验性地展示了这种方法比 DARTS 和 PC-DARTS 更鼓励稀疏的解决方案。

![](img/3b9fcb875dd7e6b3312007470c18f0df.png)![](img/579b880972fef230ea277250b8ce3644.png)

**图 4:**GAEA 获得的架构权重的稀疏性与它在两个不同的搜索空间上修改的方法相比较。稀疏性是使用跨架构决策简化的平均熵来度量的。

我们的 GAEA 方法适用于前面描述的使用 softmax 公式的任何方法(包括 DARTS、GDAS、PC-DARTS 等)，可以总结为两个简单的步骤:

1.  将传递给 softmax 的架构权重 *α* 替换为直接位于架构决策简化上的权重 *θ* 。
2.  使用指数梯度方案(4.a & 4.b)来更新这些架构权重 *θ* 。

# GAEA 的实证评估

![](img/b477fdf02b46602644c2c7ffa70cd5ac.png)![](img/d3dd610ff0c130605a65153f8295ff27.png)![](img/eaec38fb3b16f1f361967a1e5398c0cd.png)

**图 5:** 在 NAS-Bench-201 上对 GAEA 的评估。标准超参数优化方法用蓝色表示，而权重分配方案用紫色表示。搜索空间中的最优是黑色的。GAEA 是第一个在该搜索空间上优于标准超参数优化的权重共享方案，也是唯一一个在三个数据集(CIFAR-10 和 CIFAR-100)中的两个数据集上获得最优值的标准差以内的方案。

那么，从经验上看，GAEA 的稀疏性和更快的收敛速度会带来更好的性能吗？为了测试这一点，我们简单地应用上面的两个步骤来修改现有的最先进的 NAS 方法。首先，我们评估了 Dong 等人在 [NAS-Bench-201](https://openreview.net/forum?id=HJxyZkBKDr) 搜索空间上应用于 DARTS 的 GAEA。在 Dong 等人评估的方法中，非权重共享方法在所有三个数据集上都优于权重共享方法。但是，应用于单级 ERM 目标的 GAEA DARTS 在所有三个数据集上实现了最佳准确性，在其中两个数据集上达到了接近 oracle 的最佳性能。值得注意的是，它修复了飞镖在这个空间的灾难性的糟糕表现，并且是唯一击败标准超参数优化的重量共享方法。

![](img/f6e3abf8f2f4376171f04a1870213758.png)![](img/f5b5e97cb3f9b4d65417bbd355fca822.png)

**图 6:**GAEA 对飞镖搜索空间的评价。重量分担方法用紫色表示，而非重量分担方法用蓝色表示。请注意，非权重共享方法搜索所需的 GPU 时数是 GAEA 的 10，000 多倍。

我们还在原来的 DARTS CNN 搜索空间上评估了应用于 PC-DARTS 的 GAEA。通过改进权重共享优化问题的正则化，PC-DARTS 最近能够在 CIFAR-10 和 ImageNet 上匹配计算昂贵的非权重共享方法的性能。我们能够通过 GAEA 进一步提高 PC-DARTS 的性能，并在这个搜索空间上实现最先进的性能，这证明了在正确的几何结构中高效优化的重要性。

# 有兴趣了解更多信息吗？

要了解更多有关我们的结果、体重分担和 NAS 的信息，您可以

*   更多细节见我们的[论文](https://arxiv.org/abs/2004.07802)，与尼娜·巴尔坎和阿梅特·塔尔沃卡联合。
*   下载我们的代码以获得 GAEA 的完整实现。
*   阅读最初的重量分配方法 [ENAS](http://proceedings.mlr.press/v80/pham18a.html) ，以及基线重量分配方法 [RS-WS](https://arxiv.org/abs/1902.07638) 。
*   阅读该领域的[调查](http://jmlr.org/papers/v20/18-598.html)(埃尔斯肯、梅岑、&赫特、JMLR，2019)。

*原载于 2020 年 7 月 17 日*[*https://determined . ai*](https://determined.ai/blog/ws-optimization-for-nas/)*。*