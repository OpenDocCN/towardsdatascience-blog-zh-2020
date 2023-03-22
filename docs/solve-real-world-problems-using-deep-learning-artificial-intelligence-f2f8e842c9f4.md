# 使用深度学习和人工智能解决现实世界的问题

> 原文：<https://towardsdatascience.com/solve-real-world-problems-using-deep-learning-artificial-intelligence-f2f8e842c9f4?source=collection_archive---------35----------------------->

![](img/8afff64232852a9b7388d679cbbcea94.png)

由 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上的[Carlos“Grury”Santos](https://unsplash.com/@grury?utm_source=medium&utm_medium=referral)拍摄的照片

## [变更数据](https://towardsdatascience.com/tagged/data-for-change)

## 不是你通常的物体检测博客

每一项新技术的引入都有一个目的(几乎总是如此)。通常，它是由它的创建者在头脑风暴时确定的某个问题的解决方案。但当我们谈论人工智能时，这是一个很大程度上未经探索但不断发展的计算领域，我们经常被打扮得漂漂亮亮的对象检测项目分散注意力。

我们都知道，正确分类手写数字或区分动物并不是 Geoffrey Hinton 和 Alexey Ivakhnenko 等研究人员在将他们的生命奉献给该领域时所想的应用。人工智能有可能改变我们的生活方式。证据就在你的口袋里。从你最喜欢的视频流媒体平台到你的在线出租车服务，人工智能的基本原则正被到处使用。

但是对于那些口袋里没有 20 世纪的超级计算机或者口袋里没有任何东西的人来说呢？那些每天晚上饿着肚子睡觉，醒来发现生活中的怪物的人呢？

我们需要明白，人工智能及其子集不仅仅是数学或算法，它们可以神奇地预测一些事情。数据是真实的交易。这些没有数据的算法就像你的手机没有互联网一样。绝对没用。(除非你安装了离线游戏)

但问题就出在这里。如果我们想使用人工智能来诊断/治疗疾病，防止气候变化的剧烈影响或跟踪即将到来的流行病，我们需要真实的私人数据。除非有办法在工程师无法检索或查看的数据上训练深度学习模型。

近年来，已经有相当多的研究来保护用户的隐私，以解决需要私人用户数据的现实世界的问题。谷歌和苹果等公司一直在大力投资去中心化的隐私保护工具和方法，以提取新的“石油”，而不是从所有者那里“提取”。我们将在下面探讨其中的一些方法。

# 联合学习

从本质上来说，联合学习是一种利用驻留在世界各地(不同设备上)的用户的数据来回答问题的方法。安全多方计算(SMPC)是一种加密协议，不同的工作人员(或数据生产者)使用它在分散的数据集上进行训练，同时保护用户的隐私。该协议用于联合学习方法中，以确保驻留在用户处的私有数据不会被试图获得洞察力和解决全球问题的深度学习实践者看到。

然而，这种技术容易发生逆向工程，正如一个名为 [**OpenMined**](https://www.openmined.org/) 的组织成员在这篇论文 中所述。他们一直试图通过在研究人员和从业者日常使用的常用工具上开发这样的框架来普及人工智能中的隐私概念。

联合学习最好的例子是你的 Gboard(如果你用的是 android 设备)。简单来说，它接受你键入句子的方式，试图分析你使用的是什么样的单词，然后试图预测你可能键入的下一个单词。有趣的事实是，训练发生在设备本身(边缘设备)上，而不会向设计算法的工程师透露数据。复杂又时髦，是吧？

# 差异隐私

想象一下，你有一个包含某些条目的数据集，可能是像一个人在患有新冠肺炎时是否有遗传劣势这样的东西。现在，不用实际查看数据携带了关于每个个体的什么信息，您可以尝试理解单个条目对整个数据集的输出结果有什么意义。一旦我们做到了这一点，我们就能对这个结果是如何形成的有所了解。

正如《差分隐私的算法基础》一书中所述，差分隐私解决了这样一个悖论:在学习关于一个群体的有用信息的同时，却对一个个体一无所知。引用 Cyntia Dwork 对差分隐私的准确定义，

> “差异隐私”描述了数据持有人或管理人对数据主体做出的承诺，该承诺如下:

> “您不会因为允许您的数据用于任何研究或分析而受到不利或其他方面的影响，无论其他研究、数据集或信息来源是否可用”

然而，差分隐私有很多缺点，因为它高度依赖于数据集的性质。如果单个条目过于多样化(这是很有可能的)，整体结果将偏向某些条目，这可能导致关于这些多样化条目的信息泄漏。如果数据中有太多的噪音，就有可能无助于获得有用的见解。

从头开始实现差分隐私方案是非常困难的。就像密码学一样，按照预期的方式构建这些方案既复杂又困难。因此，在训练你的深度学习模型时，最好使用一些流行的隐私保护库，如 [TF Privacy](https://github.com/tensorflow/privacy) 和 [Google 的 Differential Privacy](https://github.com/google/differential-privacy) 。

差分隐私可以用在很多情况下，我们希望保护用户的隐私，数据集不是太多样化。一个示例用例可以是基因组学，其中机器学习可以用于根据患者的遗传特征为其确定独特的治疗计划。有许多建议的解决方案来实现这个应用与[k-匿名](https://en.wikipedia.org/wiki/K-anonymity#:~:text=A%20release%20of%20data%20is,also%20appear%20in%20the%20release.)，但数据的匿名化并不是保护用户隐私的最佳方式，因为它容易受到链接攻击。差分隐私在训练数据的“过程”中增加了一定程度的[不确定性，这使得攻击者的日子不好过，从而确保了隐私。](https://desfontain.es/privacy/differential-privacy-awesomeness.html)

# 同态加密

数据加密是当今组织的基本安全标准，如果没有数据加密，他们甚至会被列入黑名单，尤其是在处理私人用户数据时。但是，如果这些组织想要使用这些加密数据，可能是为了增强他们提供的产品的用户体验，我们首先必须解密它，以便能够操作它。这引起了严重的关注，因为信息可能在解密过程中泄露。

同态加密基本上是一种加密，我们可以对加密数据执行某些操作，以获得相关的见解并回答真正重要的问题，而不会损害用户的隐私。即使是量子计算机也无法破解这种类型的加密。

现在，这看起来非常方便，因为数据是加密的，不需要工程师查看数据提供的信息。但同样，它需要大量的计算能力，并且肯定需要大量的时间来执行这些计算。

同态加密有两种类型，具体取决于使用它们可以执行的计算类型，即部分同态加密和完全同态加密。前者仅支持基本运算，如加、减等。而后者理论上可以执行任意操作，但是开销很大。

同态加密的一个假设但非常重要的用例可以在不同国家的大选期间使用，这样投票系统就不会被篡改。许多重要的(事实上正确的)见解也可以从同态加密数据中导出，而不会泄露任何信息。

这些仅仅是在不损害用户隐私的情况下获得解决全球危机的有用见解的许多方法中的几个。我将探索不同的框架和工具来利用这些概念，这样我们就可以建立一些对人们的生活有实际影响的东西。我将在接下来的文章中分享我的见解，敬请关注！

# 参考

[1] OpenMined:什么是同态加密？https://www.youtube.com/watch?v=2TVqFGu1vhw&feature = emb _ titleT2

[2]隐私保护人工智能(Andrew Trask) |麻省理工学院深度学习系列【https://www.youtube.com/watch?v=4zrU54VIK6k T4

[3]谷歌联邦学习:[https://federated.withgoogle.com/](https://federated.withgoogle.com/)

[4]差分隐私的算法基础:[https://www.cis.upenn.edu/~aaroth/Papers/privacybook.pdf](https://www.cis.upenn.edu/~aaroth/Papers/privacybook.pdf)

【5】什么是同态加密？为什么它如此具有变革性？:[https://www . Forbes . com/sites/bernardmarr/2019/11/15/what-is-homo morphic-encryption-and-why-it-so-transformation/# 3f 69895 b7e 93](https://www.forbes.com/sites/bernardmarr/2019/11/15/what-is-homomorphic-encryption-and-why-is-it-so-transformative/#3f69895b7e93)

【6】为什么差分隐私很牛逼？Ted 正在写东西:[https://desfontain . es/privacy/differential-privacy-awesome ness . html](https://desfontain.es/privacy/differential-privacy-awesomeness.html)

[7]tensor flow 隐私介绍:训练数据的差分隐私学习:[https://blog . tensor flow . org/2019/03/Introducing-tensor flow-Privacy-Learning . html](https://blog.tensorflow.org/2019/03/introducing-tensorflow-privacy-learning.html)

[8] [Azencott C.-A.](https://royalsocietypublishing.org/author/Azencott%2C+C-A) 2018 机器学习与基因组学:精准医疗与患者隐私*菲尔。反式。R. Soc。* 37620170350