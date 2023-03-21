# 人工智能与真理仲裁

> 原文：<https://towardsdatascience.com/ai-arbitration-of-truth-808b57a93a97?source=collection_archive---------43----------------------->

## 我们能制造一个人工智能事实检查器吗？语言、知识和观点都是移动的目标。

鉴于推特在事实监管方面的作用，是时候弄清楚一个 ***人工智能事实审查员*** 会是什么样子了。技术上是否可行，它的偏差是什么，等等。

![](img/8788b132b57657a9511e14930b0686b3.png)

许多被标记的推文之一。

# 社交媒体公司和诉讼真相:简介

第 230 条是 1996 年《通信规范法案》的一项条款，它通过不要求互联网公司对其用户的内容承担法律责任来保护互联网公司。特朗普正在法庭上与之斗争。

让我们看看这两家声音最大的科技公司在事实核查领域都做了些什么。

> 推特的观点:我们有责任。

几个月来，杰克·多西一直在划清界限，看着川普挑战它。现在，Twitter 开始在其平台上处理假新闻。许多许多用户在短期内对此感到高兴(老实说，我是这样认为的，但是这将会带来长期的后果。如何以一种无偏见、最新的方式做出这个决定极具挑战性。我希望他们能成功地走过这条路，但肯定会有争议(可能在 11 月)。

> 脸书的观点是:让公司成为真理的仲裁者是危险的。

举个反例 [Twitter 禁止政治广告](https://www.nytimes.com/2019/10/30/technology/twitter-political-ads-ban.html) : **政治广告的界限是什么？我会把政治广告定义为一个花钱影响公众舆论的实体(而不仅仅是销售产品)。参议员购买选票显然是政治行为，但一家石油公司在拙劣的漏油应对措施后购买公众意见是政治广告吗？这里的具体答案对我来说并不重要——重要的是个人(或有偏见的计算机系统)必须决定政治和非政治广告之间的界限。**

问题是，这两种仲裁事实的方法，都是有效的论点，如果做得对，都有巨大的好处。植入是关键。

> 监管的需要是有保证的，也是强烈的(脸书)，但当监管缺失时，我们的人性就会受到强大的拉力，去减轻对用户造成的伤害(推特)。

# 技术—变压器和自然语言处理

自然语言处理(NLP)是机器学习的子领域，涉及从文本中处理和提取信息。[用于智能助手、翻译器、搜索引擎、在线商店等。](https://en.wikipedia.org/wiki/Natural_language_processing) NLP(以及计算机视觉)是少数货币化的最先进的机器学习发展之一。*它是被用来诠释真理的候选者。*

迄今为止最好的 NLP 工具是名为**变压器**的神经网络架构。长话短说，变形金刚使用一种[编码器和解码器结构](/understanding-encoder-decoder-sequence-to-sequence-model-679e04af4346)，将单词编码到潜在空间，并解码到翻译、打字错误修复或分类(*你可以认为编码器-解码器通过神经网络将复杂的特征空间压缩到更简单的空间——非线性函数逼近)*。NLP 领域的一个关键工具是一种叫做 [Attention](https://medium.com/@joealato/attention-in-nlp-734c6fa9d983) 的东西，它学习关注哪些单词以及关注多长时间(而不是硬编码到一个工程系统中)。

一个[转换器](https://arxiv.org/pdf/1810.04805.pd)结合了这些工具，以及一些其他的改进，允许模型被高效地并行训练。下图显示了数据如何流经转换器。

![](img/aa0cac8df54c19d7d8bd7f3551d40761.png)

我从一个很棒的教程中找到了一个可视化的 https://jalammar.github.io/illustrated-transformer/。

## 在线事实核查

transformer 结构的关键技术点是它们是**可并行化的**(可以通过一个模型轻松运行多个单词和 tweets)。在线事实核查将意味着每一篇基于文本的帖子都将通过一个模型，要么 a)在公开发布之前，要么 b)在公开发布之后不久。这里的计算规模是前所未有的。

作为参考，每秒大约有 [6000 条推文](https://www.internetlivestats.com/twitter-statistics/)和[脸书的数字更令人震惊](https://www.brandwatch.com/blog/facebook-statistics/)。最先进的 NLP 工具 [BERT 拥有超过 1 亿个参数](https://yashuseth.blog/2019/06/12/bert-explained-faqs-understand-bert-working/)。处理新内容所需的计算顺序在这些值中是线性的。这需要大量的 GPU(谷歌和脸书经常花费数百万美元来训练这些模型)。对这些公司来说，处理所有这些数据很可能会大幅增加成本(我可能会重新考虑这个问题，进行计算)。

[脸书使用人工智能来减少虚假账户的创建](https://phys.org/news/2019-05-fake-facebook-accounts-never-ending-bots.html)，同时这些账户被用来消除下游的基础设施负担。也许这些公司可以使用分类器来确定潜在的基于事实的内容，并且只检查那些内容？事实证明，这种事实核查对正面的公众印象很有价值。

好了，我们有了模型和服务器群，接下来是什么。终极问题是**什么是真**的问题？基于学习的事实核查方法的问题是:移动的目标、有偏见的数据和不明确的定义。这是文章的关键，也是我一直在琢磨的，也是我认为自动化**不可能实现的目标**。

# 给在线话语添加结构

监管网络言论的过程肯定会带来一些阻力。所有公司都会遵守吗？参赛或弃权的代价是什么？这里讨论的主题将在未来几个月通过 2020 年美国总统选举的扩音器播放出来。

## 什么是事实？什么是真理？

这是一个根本性的问题，即不是所有的人都认为相同的信息是真实的。理想情况下，事实是一组不可质疑的项目。真理是个人持有并用来挑战他人观点的价值——真理也可以包括信仰。

> 真理是个人的移动目标。事实是科学和社会的移动目标。

一个人无时无刻不在一系列本地和全球的真理中工作。对我和许多我最亲密的朋友来说，一个当地的事实是*划船是一项特殊的运动，由于它的合作和高极限*而无人能及，但它肯定不是对每个人都是如此。一个全球真理是*地球是圆的*。在那些更难分类的真相之间有很多真相。

我认为互联网事实检查器的问题是，用户也希望他们当地的事实得到检查。我们如何收集真实陈述和不真实陈述的数据集？

## 谁管理数据库？

不可能将每一个有待检验的事实都归结为科学的首要原则(这只是当前的世界模型，所以可能会改变)。我们能为人类制作一个知识图表吗？这意味着将会有一些数据标记过程(让用户这样做似乎是一个巨大的挑战，因为本地的事实)。数据驱动的方法总是在偏差和方差之间有一个[权衡](/bias-variance-tradeoff-fundamentals-of-machine-learning-64467896ae67)，这是假设数据本身是有效的。我已经谈到了在进行物理自动化时数据偏差的挑战，但它在数字领域也会同样普遍。

[](/democratizing-automation-744bacdc5e97) [## 自动化大众化

### 让每个人都从人工智能热潮中受益可能比一些人预期的更具挑战性。

towardsdatascience.com](/democratizing-automation-744bacdc5e97) 

让我们更深入地探讨一下这个问题。事实真相数据库没有明确的部署场景。

*   如果科学家决定事实，我会担心白人男性信息的不成比例的代表性。
*   如果社交媒体内容决定事实，我会担心俄罗斯的虚假信息决定事实数据库的真相。
*   如果书面媒体(报纸、书籍)决定了事实，那么这是在重现少数作者的世界观。

如果你把这三者结合起来，听起来还不错，但是总会有一些极端情况、遗漏的信息和令人困惑的偏见。

## 开源事实:

我[读过的最佳解决方案](https://twitter.com/balajis/status/1266180931435417600) : **开源事实核查**，有资金支持(可能来自政府和大型社交媒体公司)。这将意味着一个独立的组织定期更新和维护一个真相目录。这将是一项代价高昂的任务，但为了保持我们开放的互联网资本主义社会的稳定，这可能是必要的。

当有一个开源的事实检查组织时，剩下的是许多许多的角落案例。当一条推文没有被分类为真实或不真实时会发生什么——我们必须将其标记为*新*吗？采用数据库需要多大规模的公司？一个新的区域 [deepfakes](https://en.wikipedia.org/wiki/Deepfake) 来伪造真相探测仪怎么样？我认为这些都是我们能够并且应该解决的问题，但是讨论需要频繁和尽早。

[出处。](https://twitter.com/balajis/status/1266180931435417600)

*你如何根据基本原则进行事实核查？我认为这应该研究图中的表示理论。有一套核心原则，然后新的知识形成基于已证实的联系的优势。将会有不确定的断言区域(与真实图断开)，但是通过建立新的证明和新的边，我们可以向事实数据库添加更多的条目。

![](img/120eaf59b7d8ef0daf0f1a7ed4b74b4e.png)

照片由[丫蛋古坦](https://www.pexels.com/@danya-gutan-1529084?utm_content=attributionCopyText&utm_medium=referral&utm_source=pexels)从[佩克斯](https://www.pexels.com/photo/man-reading-burning-newspaper-3278364/?utm_content=attributionCopyText&utm_medium=referral&utm_source=pexels)拍摄

我已经包括了大量关于这个主题的阅读材料。我也发现自己在发关于这件事的推文，如果你愿意 [*关注我*](https://twitter.com/natolambert) *，或者* [*本汤普森*](https://twitter.com/benthompson) *(作者*[*strate Chery*](https://stratechery.com/)*)可能会做得更好。*

新闻报道:

*   杜克大学的一个实验室研究报道机制(包括自动事实核查)。
*   去年来自《大西洋月刊》的两篇文章研究了 a) [事实核查和特朗普](https://www.theatlantic.com/magazine/archive/2019/06/fact-checking-donald-trump-ai/588028/)以及 b)[事实核查对脸书](https://www.theatlantic.com/technology/archive/2019/02/how-much-factchecking-worth-facebook/581899/)的价值。
*   麻省理工科技评论[在新冠肺炎时代](https://www.technologyreview.com/2020/05/29/1002349/ai-coronavirus-scientific-fact-checking/)用人工智能进行事实核查。
*   [假新闻检测和政治立场](https://bdtechtalks.com/2020/02/24/deep-learning-fake-news-stance-detection/)调查。
*   [通过 Twitter 进行开源事实核查的理由](https://twitter.com/balajis/status/1266180931435417600)。

相关阅读:

*   [SOTA 自然语言处理模型](https://pytorch.org/hub/pytorch_fairseq_roberta/)的开源代码和[描述结构的维基百科文章](https://en.wikipedia.org/wiki/Transformer_(machine_learning_model))。
*   [新闻媒体中的虚假信息评估](https://arxiv.org/pdf/1911.11951.pdf)NLP(论文)。
*   [用 NLP(论文)验证科学主张](https://arxiv.org/pdf/2004.14974.pdf)
*   [图形上的表征学习](https://www-cs.stanford.edu/people/jure/pubs/graphrepresentation-ieee17.pdf) —一个我认为应该从(论文)中解读事实和真理的镜头。

[](https://robotic.substack.com/) [## 自动化大众化

### 一个关于机器人和人工智能的博客，让它们对每个人都有益，以及即将到来的自动化浪潮…

robotic.substack.com](https://robotic.substack.com/) 

像这样？请订阅我关于机器人、自动化和人工智能的直接时事通讯。*感谢阅读！*