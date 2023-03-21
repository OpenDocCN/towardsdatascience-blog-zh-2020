# 你信任机器为你做决定吗？

> 原文：<https://towardsdatascience.com/do-you-trust-machines-to-make-your-decisions-3dcab11c392c?source=collection_archive---------34----------------------->

![](img/07c0628d690beeb2509ead41511d7313.png)

卢克·切瑟在 [Unsplash](https://unsplash.com/s/photos/technology?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) 上的照片

约翰有了一个主意。他经常有相当好的想法，但这是一个非常好的想法，他对自己非常满意。他发现，由于大多数同事使用的考勤卡系统出现异常，他的公司每个月都浪费了数千个工时。这些过程已经很多年没人看了，也没人记得为什么会有现在的协议，或者它有什么用，但是每个人都同意这是巨大的时间浪费。他决定开发一个简单的系统，在保留其核心功能的同时，将大部分程序自动化。

这是一个非常非常好的主意，正如我所说，他对自己非常满意。他在一个周一自豪地向管理层提交了他的提案，而在接下来的周五，他被召集参加一个会议，他认为这将是一个关于他的计划实施的讨论。相反，他面对的是一个冷漠无情的主管，他直截了当地告诉他这个提议不合适，然后一言不发地离开了房间。

![](img/533282596751f7503d897e4489977150.png)

约翰的会面并没有像这样进行(照片由[奥斯汀·迪斯特尔](https://unsplash.com/@austindistel?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)在 [Unsplash](https://unsplash.com/s/photos/bad-meeting?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) 上拍摄)

约翰相当恼火。他不明白为什么他的提议被拒绝，而且对主任没有花时间解释原因感到特别恼火。我们大多数人可能想知道为什么我们的推销没有成功，或者我们的应用程序有什么问题。被蒙在鼓里令人沮丧，会破坏我们对决策过程的信任。这对于算法来说更是如此——因为[我们倾向于将技术拟人化](https://www.wired.com/story/book-excerpt-algorithm-transparency/)当一个人工智能似乎将我们排除在外时，人们会变得非常不安。这就是为什么人工智能中的可解释性如此重要的原因之一，因为它进一步嵌入了我们的日常交易，信任变得至关重要。

![](img/0c46fedfb4f2bb09906a1a291ac53c89.png)

图片由[穆罕默德·哈桑](https://pixabay.com/users/mohamed_hassan-5229782/?utm_source=link-attribution&utm_medium=referral&utm_campaign=image&utm_content=3174223)来自 [Pixabay](https://pixabay.com/?utm_source=link-attribution&utm_medium=referral&utm_campaign=image&utm_content=3174223)

在任何一天，你或我都可能收集到成千上万个关于我们的数据点。这可能包括网络浏览行为、身体活动或消费模式。在用户的默许下，我们的个人数据被公开编辑和记录，并出售给相关方。对于谁可以购买用于预测我们未来行为和评估我们服务适用性的材料没有限制。

主要的科技公司继续成长，它们对日常生活的影响正在扩大。对于生活在发达国家的人来说，每天都接触不到谷歌、苹果、脸书、亚马逊或微软是极其困难的。软件几乎影响到人类活动的每个方面，因此收集点的范围很广。

企业或组织拥有的关于一个人的数据点越多，他们就能做出越多的决定。例如，银行会使用信用报告来评估申请人是否适合贷款。有完善的法规来管理信用报告中的可用信息、保留时间以及谁可以访问这些信息。但是直到最近，还很少有立法规定如何处理个人的个人数据。GDPR 和加利福尼亚州的 CCPA 隐私法于 2020 年 1 月生效，已经开始制定规则，规定如何以适当的方式对待这种已经变得异常珍贵的商品。

![](img/6f3e03fd17f1661efc95fd01980c9c43.png)

马库斯·斯皮斯克·temporausch.com 摄于[派克斯](https://www.pexels.com/photo/technology-computer-display-text-330771/?utm_content=attributionCopyText&utm_medium=referral&utm_source=pexels)的

我曾在其他地方写过关于组织一旦获得这些数据后如何使用的问题。《GDPR 法案》第 22 条规定，任何个人都不能受制于完全自动化的决策过程，该过程不会以任何有意义的方式*影响他们*。这意味着，例如，乘客分数低的优步乘客不能被自动排除在进一步的乘坐之外。必须有一个人在循环中，以确保优步软件中的算法运行正常，乘客的分数不会因故障、错误或偏见而受到影响。

许多人担心的是这些组织对我们的信任程度。人们担心，当个人试图租车或办理电话合同时，他们可能会受到无法解释的算法的莫名其妙的影响。查理·布洛克的科技讽刺作品*黑镜*在“急转弯”这一集中很好地说明了这一点，在这一集中，一个角色在一系列不幸的互动后，她的算法计算社交得分大幅下降。因此，她无法在取消的航班上重新预订座位参加婚礼，在经历了几次巨大的压力后，她最终被迫接受一名卡车司机的搭车，在那里她得知司机的丈夫因社会分数低而被拒绝接受救命的医疗服务。

随着私营公司提供的服务进一步融入我们的日常生活，人工智能决策中必须有合法的人类监督。另一种选择可能会把我们引向一个怪诞的卡夫卡式的“计算机说不”的现实，在这个现实中，没有人可以挑战任何决定，没有人可以解释系统是如何工作的，也没有人承担责任。

欧盟立法者的立场是，问责制是消费者信任自动化决策的先决条件。不可否认的是，这个行为的后果是相关的——当谷歌地图选择了一条路线而不是另一条路线让你在晚上回家时，不需要解释。传统人工智能非常适合简单或琐碎的任务宋立科在 Spotify 或语音助手软件上的推荐。

![](img/34d3bfc3c0f70472098ab31d761c5843.png)

无人驾驶汽车做出的决定将需要在发生碰撞时得到解释

然而，配备面部识别技术的警方人工智能系统可能会导致错误逮捕(或更糟)。像无人机这样的现代军事系统使用人工智能过程在战场上做出瞬间决定。医疗保健甚至股票交易应用中的诊断人工智能工具，当它们以意想不到的方式运行时，都有可能造成真正的损害。如果人工智能做出的决定不透明或不可解释，那么使用这些决定就很难被证明是正当的。我们如何确定我们可以信任这些系统？我们能确定它们是在没有有偏见的训练数据的情况下开发的，或者它们没有被恶意操纵吗？如果一个封闭的 AI 系统犯了误判、股灾或战争罪，谁能被追究责任？

诸如此类的考虑促使 IBM 和谷歌等大公司开始认真对待可解释的人工智能，尽管该技术的高级应用仍处于起步阶段。任何 XAI 应用中的两个关键因素是**可检查性** & **可追溯性**:事后调查者必须能够精确地检查在算法中的什么地方做出了决策，以及为什么。

![](img/7ddfd42ce12ba81a2ff907be7a2a37c6.png)

图片由 [Gerd Altmann](https://pixabay.com/users/geralt-9301/?utm_source=link-attribution&utm_medium=referral&utm_campaign=image&utm_content=2713357) 从 [Pixabay](https://pixabay.com/?utm_source=link-attribution&utm_medium=referral&utm_campaign=image&utm_content=2713357) 拍摄

DARPA(开发供美国军方使用的新兴技术的机构)已经确定，XAI 算法应该产生两种输出:决策和得出决策的模型。分解到基本级别，一个完整的算法由训练数据(输入层)、深度学习过程(隐藏层)和最终产生解决方案的解释界面以及到达那里的步骤(输出层)组成。这将意味着许多现代先进的人工智能技术，就其本质而言，永远无法解释。复杂人工神经网络的“黑匣子”永远无法产生一个如何做出决策的模型。相反，开发人员一直在研究其他可能有助于解释的技术。其中包括:

[LRP](/indepth-layer-wise-relevance-propagation-340f95deb1ea)——逐层相关性传播——可以解释一些机器学习系统如何得出结论的更简单的技术之一。LRP 通过神经网络逆向工作，查看哪些输入与输出最相关。

[LIME](https://www.oreilly.com/learning/introduction-to-local-interpretable-model-agnostic-explanations-lime)——本地可解释的模型不可知解释——是一个临时模型，它稍微改变(或干扰)输入，以查看输出如何变化，从而提供关于决策如何做出的见解。

[保留](https://arxiv.org/abs/1608.05745) —反向时间注意力模型可用于医疗诊断，并利用[注意力机制](http://www.wildml.com/2016/01/attention-and-memory-in-deep-learning-and-nlp/)观察两个可能的神经网络中哪一个对决策影响最大。

最近来自 [Venture Radar](https://blog.ventureradar.com/2019/08/19/explainable-ai-and-the-companies-leading-the-way/) 的这篇文章更详细地总结了在 XAI 技术领域处于领先地位的公司，包括[**【DarwinAI】**](https://www.ventureradar.com/organisation/Darwin%20Ai/edb64140-9404-47f3-9c8d-26acda8e191a)**，****使用了一种被称为“生成合成”的过程，以使开发人员能够理解他们的模型； [**Flowcast**](https://www.ventureradar.com/organisation/Flowcast/277d0366-ae93-488c-b59a-fd58d6fa39ab) 使用机器学习技术为贷方创建预测性信用风险模型，以及[**Factmata**](https://www.ventureradar.com/organisation/Factmata/6d540fa6-c6a0-4e9e-9cf3-f1fc254afe73)**旨在打击网上假新闻。****

*****所有观点均为我个人观点，不与甲骨文共享。* [请随时在 LinkedIn 上联系我](https://www.linkedin.com/in/mark-ryan101/)****