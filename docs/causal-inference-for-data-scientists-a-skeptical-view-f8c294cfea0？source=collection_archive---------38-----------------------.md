# 数据科学家的因果推理:一种怀疑的观点

> 原文：<https://towardsdatascience.com/causal-inference-for-data-scientists-a-skeptical-view-f8c294cfea0?source=collection_archive---------38----------------------->

## 因果推理如何以及为什么让我们失败

![](img/3802cc78a2aa82a6600dea1812b84af1.png)

在 [Unsplash](https://unsplash.com/s/photos/wrong?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) 上 [NeONBRAND](https://unsplash.com/@neonbrand?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) 拍摄的照片

# 介绍

这篇文章的目的是展示为什么因果推理是困难的，它如何让我们失败，以及为什么 DAGs 没有帮助。

机器学习从业者关心的是预测，很少关心解释。这是一种奢侈。我们甚至研究那些没有数据生成过程可以记录下来的问题。没有人相信一个根据莎士比亚戏剧训练出来的 LSTM 模式会奏效，因为它接近莎士比亚。然而它是有效的。

近年来，因果推理的新工具已经为更广泛的受众所用。这些工具承诺帮助非专家不仅预测，而且解释数据中的模式。有向无环图和 do-calculus 是其中最有影响力的。

人们喜欢闪亮的工具，这里面有危险。新工具的出现伴随着采用它们的热潮，伴随着一种处于先锋地位的感觉，伴随着新的机遇。这往往使它们变得不可靠:它们被误用，被更好的理论、设计或数据所替代。

在这篇文章中，我将重点放在有向无环图和 Python 库上，因为 Dag 在机器学习社区中非常流行。我提出的观点同样适用于潜在结果框架，或者任何其他表达因果关系的正式语言。

# 为什么理论上因果推理很难

因果推理依赖于因果假设。假设是允许从统计关联到因果关系的信念。

随机实验是因果推断的黄金标准，因为治疗任务是随机的，并且是物理操纵的:一组得到治疗，一组没有。这里的假设是简单明了的，通过设计是安全的，并且可以方便地进行辩护。

当无法控制治疗分配时，比如说观察数据，研究人员试图对其建模。这里的建模相当于说“我们假设在对年龄、性别、社会地位和吸烟进行调整后，跑步者和非跑步者彼此如此相似，就好像他们被随机分配到跑步中一样。”然后一个人可以在跑步上回归预期寿命，宣称‘跑步增加了 n %的预期寿命’，然后收工。

这种方法背后的逻辑是笨拙的。它隐含地假设我们确切地知道为什么人们开始跑步或长寿，唯一缺少的是我们试图估计的东西。一个不太可信、有点绕口的故事。此外，一个令人高兴的巧合是，我们模型的所有部分都有可用的经验代理，测量无误。最后，由于没有原则方法来检验选择模型在多大程度上接近真实的分配机制，所以它的所有假设都可以永远争论下去。

Jaas Sekhon [1]很好地总结了这种情况:

> *“如果没有实验、自然实验、不连续性或其他强有力的设计，再多的计量经济学或统计模型也无法让从相关性到因果关系的转变具有说服力”*

# 为什么因果推理在实践中很难

实际例子更好地说明了上面提出的问题。虽然有很多，但我坚持三个，其中一个来自经济学、流行病学和政治学。

1986 年，罗伯特·拉隆德展示了计量经济学程序如何不能复制实验结果。他利用了一个实验，在这个实验中，个人被随机挑选到工作项目中。随机化让他能够估计一个项目对收入的无偏影响。他接着问:如果没有随机化，我们会得到同样的估计吗？为了模拟观察数据，拉隆德建立了几个非实验对照组。在比较估计值后，他得出结论，计量经济学方法无法复制实验结果[2]。

流行病学也有同样的问题。想想高密度脂蛋白胆固醇和心脏病的故事。人们认为“有益的胆固醇”可以预防冠心病。研究人员甚至宣称观察性研究对协变量调整是稳健的。然而几年后，随机实验证明高密度脂蛋白并不能保护你的心脏。对于流行病学来说，这种情况并不独特，许多流行病学的发现后来被随机对照研究推翻[3]。

民主增长研究是当年政治学的热门话题。研究人员将人均 GDP 或类似的指标放在等式的左边，民主放在右边，为了避免过于简单，他们在左边放了一堆控制指标:预期寿命、受教育程度、人口规模、外国直接投资等等。从 2006 年之前发表的 81 篇论文中的 470 个估计值来看，其中 16%显示了民主对增长的显著和负面影响，20%是负面但不显著的，38%是正面但仍然不显著的，38%的估计值是正面和显著的[4]。

这种模式很明显:无论研究人员对他们的观察研究多么自信，也不一定能让他们更接近真相。

# 为什么 DAGs 理论上不能解决问题

狗很棒。它们具有强大的表达能力和很好的推理特性:考虑到 do-calculus 的完备性，如果一个效应不能用 do-calculus 来识别，那么它在别处也不是不可定义的，至少在没有额外假设的情况下是如此。它们也是有教育意义的:试着画一个简单的仪器变量设置自己去看。

但这不是重点。关键是，dag 提供的好处发挥得太晚了，无法将我们从观察数据中的因果推断的恐惧中解救出来。的确，给定一个特定的图形，微积分告诉我们什么可以估计，什么不可以。然而，它没有告诉我们如何构建一个有意义的 DAG。

有一句乔治·博克斯的名言[5]:

> “既然所有的模型都是错误的，科学家必须警惕什么是严重错误。国外有老虎，关心老鼠是不合适的。”

这里的可怕的老虎有太多可观察的变量来推理，天知道有多少不可观察的变量，我们用噪音测量的东西，我们甚至不能测量的东西。在这种情况下，真实的图形是未知的，当真实的图形是未知的时候，我们的推论是否正确的答案是“不知道”或者“不知道”。

考虑到这一点，当我们添加“如果正确的 DAG 为他们所知”[6]，[7]时，关于 DAG 的许多事情变得不那么令人困惑:

> “选择一组适当的协变量来控制混杂的任务已经简化为一个简单的“路障”难题，可通过简单的算法进行管理，*【如果正确的 DAG 已知】*”
> 
> “如果我们能够从我们的观察数据中生成用于这个图的相同数据，但使其成为因果关系，这不是很好吗？借助现代因果推理方法，我们可以！*【如果已知正确的 DAG】*

# 为什么 Dag 在实践中不能解决问题

道为什么是一个伟大的图书馆。作者尽一切努力提醒用户因果推断是困难的。然而，根本问题依然存在。考虑[下面的引用](https://medium.com/@jrodthoughts/microsoft-dowhy-is-an-open-source-framework-for-causal-reasoning-3ee118749213):

> 从概念上讲，DoWhy 是遵循两个指导原则创建的:明确询问因果假设，并测试估计值对违反这些假设的稳健性。

核心假设是所选择的 DAG 是众多可选 DAG 中的正确一个——这个假设没有健壮性检查。也很容易被侵犯。

让我们违反它。我将使用 [DoWhy:因果推断的不同估计方法](https://microsoft.github.io/dowhy/dowhy_estimation_methods.html)中描述的设置。有 5 个共因 *W* ，2 个仪器 *Z* ，一个二元处理 *v0* ，其他所有效应都有界在[0，0.5 × *βv0* 以内，结果 *y* 完全由可观测变量集合决定。真实治疗效果 *βv0* 为 10。

![](img/c509ddb2664afbca7e9b939742c5c3b8.png)

来自[https://Microsoft . github . io/dowhy/dowhy _ estimation _ methods . html](https://microsoft.github.io/dowhy/dowhy_estimation_methods.html)

虽然有很多方法可以错误地指定一个模型，但是我将模拟一个非常简单的方法:缺少一个变量 *U* 。即使少了一个变量，人们也能画出 511 种不同的箭头组合。我将只坚持可能情况的子集: *U* →结果， *U* →结果和治疗， *U* →结果和随机常见原因， *U* →治疗和随机常见原因 *U* →随机仪器和治疗， *U* →随机仪器和结果。

在该教程中，作者使用了六个估计值，并设法通过线性回归、倾向得分分层、倾向得分匹配、倾向得分加权和工具变量五次接近 10。在我的模拟中，我将使用所有五种方法。我将单独分析 IVs，因为它们不依赖于后门标准。

![](img/7c0cd890b5ca311424b65538f8e738d3.png)

后门估计量与未观测协变量 U

首先要注意的是，当违反后门标准时，就像在 *U* 影响治疗和结果的情况下，所有的估计都有明显的偏差。这并不奇怪——我们不能故意违反一个假设，并期望依赖它的程序能够工作。然而，给定的图离真正的图只有一个节点和两条边。这仍然足以扭曲估计。事实上，这个玩具示例是稳健的:所有其他影响都明显小于治疗影响，一次只有一个共同原因受到影响，只有一个不可观察的变量丢失，一切都测量无误。实际上，这种情况很少见。

另一件要注意的事情是，回归估计比倾向得分估计做得更好。这是因为回归更简单。例如，当治疗和共同原因受 *U* 影响时，回归是无偏的，因为在治疗和结果之间仍然没有开放路径。这不适用于倾向得分估计，因为倾向得分估计是两阶段过程，需要两组假设。为了估计倾向分数本身，一组可观察值 *W* 必须满足关于治疗的后门标准。因为在 *W* 和通过 *U* 的处理之间有一个开放路径。

现在，让我们转向工具变量。拥有一件好的乐器是很难得的。拥有两种乐器是前所未有的奢侈。在本教程的玩具示例中，有两个有效的乐器: *Z0* 和 *Z1* 。我将设置 *U* 随机影响乐器，即使只有 *Z0* 用于估算。我还将设置 *Z0* 和 *Z1* 为连续。

![](img/681c78eff5205349d0c5b1f57c443c7b.png)

IV 估计量与未观察到的协变量 U

这里，与后门相反，当 *U* 影响治疗和结果时，估计值不会有显著偏差。IV 估计器如此稳健的原因是只有一个箭头从 *Z0* 指向 *v0* 。这有助于满足两个假设:(I)在 *Z0* 和 *v0* 之间存在联系，以及(ii)从图中移除 *v0* 使得 *Z0* 和 *y* 之间没有联系。那么，如果 Cov[ *U* ， *Z0* ] = 0，Cov[ *y* ， *Z0* ] ≠ 0，那么处理效果就是简单的 Cov[ *y* ， *Z0* ]/Cov[ *v0* ， *Z0* 。

然而，如果从 *U* 到 *Z0* 的箭头存在，那么 Cov[ *U* ， *Z0* ] ≠ 0，这违反了排除限制假设(ii)——即 *Z0* 影响结果的唯一路径是通过治疗。在这个模拟中，这就是当 *U* 影响仪器 *Z0* 和结果的情况。

# 结论

有向无环图和 do-calculus 很可能是最有效的工具。它们不会帮助你从数据中获得因果结论，而这些结论并不存在。

# 参考

[1] J. S. Sekhon，[用于比赛的鸦片制剂:因果推理的匹配方法](https://www.annualreviews.org/doi/full/10.1146/annurev.polisci.11.060606.135444) (2009)，《政治学年度评论》，12，487–508。

[2] R. J. LaLonde，[《用实验数据评估培训项目的计量经济学评估》](http://business.baylor.edu/scott_cunningham/teaching/lalonde-1986.pdf) (1986)，《美国经济评论》，604–620 页。

[3] N. Krieger 和 G. Davey Smith，[回应:面对现实:我们的流行病学问题、方法和任务之间的生产性紧张](https://watermark.silverchair.com/dyw330.pdf?token=AQECAHi208BE49Ooan9kkhW_Ercy7Dm3ZL_9Cf3qfKAc485ysgAAAm0wggJpBgkqhkiG9w0BBwagggJaMIICVgIBADCCAk8GCSqGSIb3DQEHATAeBglghkgBZQMEAS4wEQQMjF1ZKJWzt4aUMoY0AgEQgIICIBKFcslqNjFSQyF--OdiEVg9KnqOAZA_cX6czenlnz3tGf9EKi4Z-lF_phN-pk7-9OPTptUQoDBzlJUJRl3izA_nQEIElKCffMryB4hv_MaWThpfdPcfBdAmp_uw5oP6XmdJRN7swyRIa6auOItTd7VSeZEfcpilqXXLzzERe0WdxLKVpVR1PC_QZs8VrnITGzOIXBKvflE3-vrt38KFnTajHkbNxbfRCHAajUrCfbVSTBODvYW7Ki0mKZa91Kd8N4k7DaIra6j7QtlDDrEJbOstGLHd8xtOlH73ecBpko7yU52Z18hzTDpPoHvHgAClFXj-l7r182Q6iifNv9tJYH2XP8KiZYcRuZAc8kQzt2CCpQyS1M5YMYxnRpp12NhtA0nQCPTdjn_tBIh5-1uWDO69yOCeRqanvCro8Eixv5GWEIoaKQwRciEcGE1YuIY6ta95E-j2WoSq0fpfEf65P_1qJKtK1aDsFd6obRUDP1x2dxQX9HIS4hNVX_49TClt3LkNV8wrZK5KyWtiiJ2lYIHT2Jx-KzKa1jlLm1e9qxH36VfwYLotVpgo9Nqifn149IZ3a0ytOnodHKnJsnpW7M--7BxLWB-X-Yd7xjwzwwSZIKK1ejmXgdVu8EpWu5MWOT6gOQ5-7CkL18EN_L-gOBwwtv-g8ufCAhGw1Ufq3r6OD_eOeTURAGGlc7np369x20qsqegWRbZlHSednnOOqPA) (2016)，《国际流行病学杂志》，45(6)，1852–1865。

[4] H. Doucouliagos 和 m . a . ulubaolu，[民主和经济增长:一项荟萃分析](https://www.deakin.edu.au/__data/assets/pdf_file/0010/404767/2006-04eco.pdf) (2008 年)，美国政治科学杂志，52(1)，61-83。

[5] G. E. P. Box，[科学与统计](http://www-sop.inria.fr/members/Ian.Jermyn/philosophy/writings/Boxonmaths.pdf) (1976)，美国统计协会杂志，71 (356)，791-799

[6] J. Pearl，[因果革命七次火花对机器学习的理论障碍](https://arxiv.org/pdf/1801.04016.pdf) (2018)，arXiv 预印本 arXiv:1801.04016。

[7] A. Kelleher 和 A. Sharma，[介绍用于因果推断的 do 采样器](https://medium.com/@akelleh/introducing-the-do-sampler-for-causal-inference-a3296ea9e78d) (2019)，中