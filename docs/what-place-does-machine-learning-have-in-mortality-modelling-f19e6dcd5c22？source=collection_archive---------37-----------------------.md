# 机器学习在死亡率建模中有什么地位？

> 原文：<https://towardsdatascience.com/what-place-does-machine-learning-have-in-mortality-modelling-f19e6dcd5c22?source=collection_archive---------37----------------------->

## 见习精算师的视角

![](img/8e933ec624609b933617a3b338138423.png)

[Icons8 团队](https://unsplash.com/@icons8?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)在 [Unsplash](https://unsplash.com/s/photos/time?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) 上的照片

人类的死亡不是一个确定的过程(除非你碰巧[生活在科幻反乌托邦](https://www.youtube.com/watch?v=1x1GFOvTFOU)中，剩余寿命被用作货币)。因此，对未来死亡率的估计是精算师为客户提供的许多建议中的核心假设之一。我们投入了巨大的努力，将所有最新的数据整合到人类预期寿命的模型中，并预测随着时间的推移，我们对未来的预测将如何演变。

虽然我们对导致人们寿命延长或缩短的因素有一个大致的了解，但似乎总有随机因素。伴随这种随机因素而来的是寿命风险——这是那些现金流依赖于人们寿命的组织特别关注的事情。例如，养老金计划需要知道其成员能活多久，这样才能有效地管理其负债，并确保做出正确的投资决策。同样，人寿保险公司需要合理准确地预测投保人死亡率，以便能够设定适当的保费水平——足够高以获得一些利润，但又足够低以向客户提供价值并在人寿保险市场保持竞争力。

目前，设定死亡率假设的过程可能如下所示:

1.  选择一个基础表——一个公认的相当令人沮丧的个人死亡概率列表；
2.  选择死亡率预测——这将决定每个年龄的死亡率如何随时间演变；最后
3.  执行额外的分析来定制所选的表和投影，以匹配感兴趣人群的特定人口统计特征。

这些基本表格和预测经常更新(例如，[连续死亡率调查](https://www.actuaries.org.uk/learn-and-develop/continuous-mortality-investigation/about-cmi)每年更新其预测)。此外，新的参数正在被添加到模型中，以使其对假设设置者更加灵活(例如，查看 2019 年 3 月发布的 *CMI_2018、*上的[简报](https://www.google.com/url?sa=t&rct=j&q=&esrc=s&source=web&cd=&cad=rja&uact=8&ved=2ahUKEwiU8r_ItpDrAhWUtHEKHckyDF0QFjABegQICxAD&url=https%3A%2F%2Fwww.actuaries.org.uk%2Fdocuments%2Fcmi-working-paper-119-briefing-note&usg=AOvVaw3kdHr2YUqAynchvS4-DX3e)，其中介绍了新的“死亡率改善的初始增加”参数)。事实上，我们正在不断修正我们的死亡率模型和估计，这足以证明根据历史数据预测人们的寿命实际上是相当困难的。似乎有数不清的变量与生活方式、社会经济状况甚至基因有关。我们应该做些什么来最好地捕捉驱动个人预期寿命的最重要因素——以及它随时间的演变——从而使我们能够推广到更大的人群？

# 机器学习开始发挥作用了

数据驱动的机器学习方法似乎很适合这类问题。预期寿命是一个经过充分研究的现象，已经收集了大量数据来帮助我们理解人类死亡率的“规则”。机器学习已经产生了许多无监督的算法，这些算法专门设计用于识别相似的数据点(*聚类*)和发现数据中的模式和相关性(可以说属于*关联规则学习*的领域)。如果有人认为人口学家会全面掌握这一新兴工具，并以各种创造性的方式使用它来加深我们对决定预期寿命的复杂关系的理解，这是可以理解的。

然而在现实中，到目前为止，机器学习在人口结构变化研究中的应用还是有限的。在他们的 [2019 年论文](https://www.mdpi.com/2227-9091/7/1/26)中，Levantesi 和 Pizzorusso 提出，这种缺乏受欢迎程度是因为机器学习模型通常被视为“黑盒”，其结果很难解释和解读。诚然，这是一个合理的担忧——在机器学习社区中有大量正在进行的工作和讨论，涉及解释*如何*和*为什么*一个人工智能算法得出它所确定的结论的重要性，以及研究向利益相关者证明一个模型正在做出明智和合理的决定的实用方法。

尽管如此，研究人员继续总结了迄今为止利用机器学习方法对死亡率建模领域做出的贡献:

*   评估和改进标准随机死亡率模型产生的估计值的拟合优度( [Deprez 等人，2017](https://link.springer.com/article/10.1007/s13385-017-0152-4))；和
*   应用神经网络来识别预测死亡率的重要因素，并扩展标准死亡率模型( [Hainaut，2018](https://www.cambridge.org/core/journals/astin-bulletin-journal-of-the-iaa/article/neuralnetwork-analyzer-for-mortality-forecast/9045C2A616EF9E9B063560704DC399AD) 和 [Richman 和 Wüthrich，2018](https://www.cambridge.org/core/journals/annals-of-actuarial-science/article/neural-network-extension-of-the-leecarter-model-to-multiple-populations/19651C62C3976DCD73C79E57CF4A071C) )。

Levantesi 和 Pizzorusso 在同一篇论文中继续证明，他们能够通过引入一个“ML 估计量”参数来捕捉标准死亡率模型中无法识别的模式，该参数被他们的算法识别为具有预测能力。通过这样做，当他们的机器学习模型的输出用于支持标准死亡率模型时，他们能够提高预测质量。

你会注意到，上述论文中的一个关键主题是使用机器学习来*支持*，而不一定是*取代*传统的死亡率建模方法。在我看来，这是一种明智的前进方式。如果我们想进一步了解预期寿命的驱动因素，来自机器学习和人口学的领域专家需要能够交流和合作。

传统方法和机器学习方法在自然语言处理领域一直存在着著名的分歧，在这个领域，理论驱动和基于规则的方法最初几乎被抛弃，而支持数据驱动的方法——以至于它导致了 IBM 研究员弗雷德里克·耶利内克的名言:“每当我解雇一名语言学家，语音识别器的性能就会提高。”但是，即使在 NLP 社区内，也有迹象表明钟摆可能会摆回另一边，并且有人问，如果更多的语言学家参与 NLP 研究，我们是否会取得更大的进展([相关 TWiML talk](https://www.youtube.com/watch?v=K-PCrc81wAk) )。

不需要太多的想象力就能看出 NLP 研究和人口学/死亡率建模之间的相似之处:每个领域都研究某种系统，在该系统中，基于理论和基于规则的方法产生(显然)合理且有用的结果——尽管也有大量的数据，我们能够从这些数据中建立完全适当的模型，而无需具备如此深入的领域专业知识。

我建议最好采取一种平衡的观点——如果手工设计的基于规则的方法不断被替代方法(如机器学习提供的方法)超越，那么对这些方法过于感情用事是没有用的，但是我们*应该*对这样一个事实保持开放的态度，即我们总是需要领域专家来告知我们设计和操作模型的方式。

# 结论

![](img/06c85780eff7953b52be09396ad52b27.png)

照片由 [Aron 视觉效果](https://unsplash.com/@aronvisuals?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)在 [Unsplash](https://unsplash.com/s/photos/death?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) 上拍摄

机器学习和人工智能在识别和预测死亡率趋势方面有其一席之地。毋庸置疑，这些技术是检测数据点中及数据点之间的模式和关联的强大工具，将这种分析与现有的更经典的死亡率建模方法相结合是有价值的。不可避免的是，只有在更经典的、基于规则的方法和更现代的、数据驱动的机器学习方法之间找到*平衡*时，最佳结果才会出现。

死亡率建模将永远是相关的——除非 *a)* 你碰巧发现了青春之泉， *b)* 你已经知道如何将你的意识永远上传到云中，或者 *c)* 你生活在前面提到的科幻反乌托邦中(尽管这可能是一个公平的交易，如果这也意味着你可以成为贾斯汀·汀布莱克)。现在，我们的世界不是一个反乌托邦，但我们一直面临着甚至连专家都没有预见到的预期寿命的新趋势，我们还面临着一系列新的挑战，因为我们要应对人口老龄化的后果。

我们很可能需要死亡率建模方面的进步，以成功管理我们肯定会因人口结构变化而面临的问题——只有我们的专家能够抛开他们的学术忠诚，为了每个人的利益而共同努力，我们才能做到这一点。所以*是的，*我们必须勇敢——我们必须*适应*和*探索*新技术——但是如果我们不记得我们从哪里来，我们会很快发现自己迷失了。

# 学分和更多信息

**Andrew Hetherington** 是英国伦敦的一名见习精算师和数据爱好者。

*   查看我的[网站](https://www.andrewhetherington.com/)。
*   在 [LinkedIn](https://www.linkedin.com/in/andrewmhetherington/) 上与我联系。
*   看我在 [GitHub](https://github.com/andrewhetherington/python-projects) 上摆弄什么。

讨论的论文:机器学习在死亡率建模和预测中的应用。*风险* **2019** 、 *7* 、26。

沙漏照片由[阿伦视觉](https://unsplash.com/@aronvisuals?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)拍摄。闹钟照片由 [Icons8 团队](https://unsplash.com/@icons8?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)拍摄。两者都在 [Unsplash](https://unsplash.com/) 上。