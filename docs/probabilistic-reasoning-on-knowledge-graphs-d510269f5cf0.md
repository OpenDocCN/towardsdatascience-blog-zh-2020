# 知识图上的概率推理

> 原文：<https://towardsdatascience.com/probabilistic-reasoning-on-knowledge-graphs-d510269f5cf0?source=collection_archive---------36----------------------->

## 知识图表和推理

## 希望，感谢*‘神话般的五人组’，*会有事情发生！

![](img/c22dd5aa436a04bdc5af5a032f8bc7c9.png)

[戴恩·托普金](https://unsplash.com/@dtopkin1?utm_source=medium&utm_medium=referral)在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上的照片

不加推理地使用知识图表就像吃了一块诱人的蛋糕，然后把它留在那里欣赏:美学上很吸引人，但浪费了美味的配料，从长远来看，毫无意义！

推理使知识图的“知识部分”成为可能，如果没有它，可以更像是一个数据库而不是知识模型。

简而言之，区别就在这里。

虽然**关系数据库**的中心点是用关系对现实建模、存储到表中以及用 SQL 查询感兴趣的领域，但是知识图更多的是关于类似图形的数据(而不仅仅是图形数据库中的数据)。

有了**知识图**，你就可以为你拥有图状数据的感兴趣的现实建模(所谓的**地面扩展组件**)。你可以抽象地描述这样的现实是如何运作的，即:底层业务的规则是什么，人类知道的东西是什么，但程序和数据库都不知道。最后，通过推理过程，你可以为你的图生成新的节点和边形式的新知识，即派生的外延组件，又名**推理组件**【1】。

关于知识图表推理的简单例子，你可以在 [Medium](https://medium.com/the-innovation/thats-why-google-is-so-reluctant-to-answer-even-if-it-knows-the-answer-166eb2938379) 上查看。

到目前为止，一切顺利。

但是现实是不确定的，数据是不确定的，领域知识是不确定的，甚至我们的**人脑推理过程**也是被设计成不确定的。所以推理需要包含和处理各种不确定性。

因此，如果你想在知识图上进行尖端的推理，如果你想对现实进行更合理和可靠的描述，而不是一种机器人/木偶版的人类推理，你需要**考虑不确定性，并将其纳入你的推理**。

# 如何在 KGs 上进行概率推理

![](img/2f7ade321b9c6b5b547ed71dbddfee1e.png)

照片由 [Calum Lewis](https://unsplash.com/@calumlewis?utm_source=medium&utm_medium=referral) 在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄

在使用 KGs 的概率推理中，我们当然需要执行“概率部分”。然而，坏消息是，如果我们想明智地实现它，我们**也**需要同时满足**标准对 KGs [2]推理的要求**。

例如，我们不希望能够处理空值，但不能遍历图形，对吗？因此，我们需要**的四种关键成分**作为基本配方:

1.  **全递归**

如果你想**探索图形结构，至少**你会期望遍历这些图形！当然不仅仅是遍历:您希望用多种算法探索图形数据，其中路径的范围和形式事先并不知道。

对于图形数据库，您可能习惯于像 Neo4J 中那样对语言进行模式匹配。这里的世界是不同的:知识图通常不做模式匹配，而是依赖于一种更强大的工具:递归。

> 所以，我很抱歉地告诉你，因为你想在 kg 上推理，你需要递归。

而且它必须是完全的，也就是说，我们不能摆脱许多现有形式的部分递归，因为我们想要一个我们可以编写的图形算法的完整选择。

让我们以一个基本的图问题为例，如[ST-连通性](https://en.wikipedia.org/wiki/St-connectivity)(在一个有向图中询问 t 是否从 s 可达的决策问题)。它可以通过左递归来表达。但是有许多基本但重要的问题是不可能陈述的，即使用线性递归也不行。这就是为什么完全递归是必须的。

我认为仅仅了解递归是远远不够的，所以如果你同意我的观点，也读读[这个](https://medium.com/code-zen/recursion-demystified-24867f045c62)或者[这个](https://medium.com/swlh/tree-through-the-eyes-of-recursion-e5c8e19b8e02)，或者回到计算机科学的第一年，重读一本像[这个](https://www.springer.com/gp/book/9783642839542)一样精彩的书！

**2。归纳(不，这不是递归了…也读成分 2！)**

一种语言，或者如果你想更非正式和轻松，一个旨在推理(在知识图上)并准备进行概率推理的系统必须**能够表达非基础归纳定义**【3，4】。

归纳定义是就其本身给出的定义，它必须是合理的。必须有一个或多个非递归基础案例+一个或多个递归案例，最终导致回到基础案例。

非常著名的归纳定义的例子是斐波那契数列和阶乘。归纳定义最简单的例子可能是**传递闭包**。如果一个系统连一个二元关系的非地(仅用基格)传递闭包都做不到。这是推理的本质，因为:

> 如果一个系统甚至不能计算出传递闭包，你可以肯定它不会帮助你进行推理！

**3。语义**

推理过程必须追溯到特定的语义，即赋予内涵成分和查询答案意义的成分。

对于语义，您有许多不同的选择，在这种情况下，语言语义:稳定模型语义、有根据的语义、最少模型语义以及它们的所有变体，等等。就性能而言，它们提供了或多或少有效的推理过程。

> 在这种情况下，合理的选择是使用**有理有据的语义**。

让我们来回答一个连接查询。使用稳定模型语义，解决方案的构造需要考虑满足您的内涵组件和查询的所有可能解释中出现的事实。相反，在有充分根据的语义学中，对查询的正确回答包含了任何真实解释的所有事实。就性能而言，节省是不言而喻的。

这个和其他理论基础[5]是这样的，你可以使用良好的语义开发更快的推理机，因此它是一个更好的选择！

**4。本体推理**

我们正在谈论推理 ok，但是它有点模糊，哪种推理？我希望有一个强大的推理版本，当然，它允许我查询我的知识图表，以获得意想不到的、新的和重要的见解！

所以，至少我们希望有本体推理，用哲学术语来说，可以被看作是“对现存事物”进行推理的能力。而在数学术语中则意味着特定运算符的存在，如包含、泛量化和存在量化等。

> 在计算机科学中，本体推理通常对应于系统在推理规则下**支持 SPARQL** (查询语言)的能力，这种推理规则被称为‘蕴涵机制’， [**的**OWL 2 QL 概要****](https://www.w3.org/TR/owl2-profiles/) (语义 web 社区中一种众所周知的语言中的概念和推理规则的复合体)。

这个“轮廓”是一种工具箱，包含了推理的所有要素:只要想想它包括对称的、自反的和非自反的属性，不相交的属性，或者简单地定义类和子类。

这种模式下的本体推理也提供了出色的性能，在表达能力和计算复杂性之间有一个很好的平衡；例如，当假设查询是固定的时，在 [AC0](https://en.wikipedia.org/wiki/AC0) 复杂性类中。

# 准备工作:

![](img/65e0b76fa59eb1ca233a2cfe3f85d74f.png)

由[aaron Blanco Tejedor](https://unsplash.com/@healing_photographer?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)在 [Unsplash](https://unsplash.com/s/photos/preparation?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) 上拍摄的照片

我们刚刚看到的四种成分是基础配方:推理。但是对于**高级配方**，概率推理，你将需要所有'*神话般的五'*的共同努力。你至少需要另一个关键因素，第五个因素:现在，你必须决定你希望如何管理不确定性。

科学家们想出了最不同的方法来处理涉及不完善或未知信息的各种情况:概率论、模糊逻辑、主观逻辑、证据理论等。

> 如果把它保持在地面上，参考不确定性的概率论，**我们可以用什么样的模型/系统**对 KGs 进行概率推理？

很多人都在研究概率推理。涉及的人如此之多，以至于**至少存在** **三个主要的相关研究领域:**概率逻辑编程、概率编程语言、统计关系学习。

为了这个目标，我花了一段时间去钻研不同的科学分支。毫无疑问，**很多时间**去真正理解在知识图上概率推理的最新技术水平。现在我可以向你报告我的发现了。

概率逻辑编程是一组非常好的语言，允许你定义非常紧凑和优雅简单的逻辑程序。此外，他们使用 Sato 语义，这是一种定义语义的简单而紧凑的方法。

概率编程语言是优雅的，因为它们让你可以自然地处理所有的统计分布，这对统计学家来说是如此的有吸引力和舒适。

**统计关系学习**允许我们使用像马尔可夫逻辑网络这样的概率图形模型，因此人们在机器学习环境中拥有的所有能力都可以立即投入使用(例如 PGM 中的概率推理)。您可以创建非常接近您感兴趣的现实的 PGM，并以最小的努力对实体和关系进行建模。

所有愉快和积极的点，没有缺点！那么，我应该追求什么呢？

所有这些技术都只关注配方的第五个成分:如何管理不确定性。但是他们几乎完全避开其他四个。

换句话说，它们提供了一种处理不确定性的特定方法，但没有知识图上推理的所有其他基本特征。

> 它们不适用于知识图上的概率推理。

这就好像他们研究并开发了最好的咖喱粉，有时忘记了蔬菜、鸡肉甚至火。那你怎么做咖喱呢？

![](img/c42a2568d4054291d8cf2925b870ace2.png)

图片:照片由[恩里科·曼泰加扎](https://unsplash.com/@limpido?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)在 [Unsplash](https://unsplash.com/s/photos/empty?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) 上拍摄

注意到这一点后，我试图为绘制概率知识图做出自己的贡献，付出额外的努力，研究什么是必需的，并去杂货店购买所有必需的原料。这是我在知识图上进行概率推理的秘方。

我渴望听你的！

## 结论

没有推理，知识图是半生不熟的(在这里[了解更多信息](https://medium.com/@eleonora.laurenza/thats-why-google-is-so-reluctant-to-answer-even-if-it-knows-the-answer-166eb2938379))，处理不确定性对于原则性推理是至关重要的。然而，要做到这一点，有五个基本原则。分别是全递归、归纳、语义、本体推理、概率论。更多的现有方法忽略了这五个令人难以置信的因素中的大部分，只关注其中的一个“成分”。然而有人尝试过:[这里用了所有的'*神话般的五'*](https://link.springer.com/chapter/10.1007/978-3-030-57977-7_9) *。*

如果您想通过金融情报行业的真实案例了解更多关于知识图上的概率推理:

[](/reasoning-on-financial-intelligence-3e40db6c2b5) [## 金融情报推理

### 如何使用人工智能帮助打击有组织犯罪。

towardsdatascience.com](/reasoning-on-financial-intelligence-3e40db6c2b5) 

关注我的 [Medium](https://eleonora-laurenza.medium.com/) 了解更多信息。

让我们也在 [Linkedin](https://www.linkedin.com/in/eleonora-laurenza-199132175/) 上保持联系吧！

# 参考

[1] L. Bellomarini 等人，[知识图和企业人工智能:使能技术的承诺](https://ieeexplore.ieee.org/document/8731350) (2019)，ICDE

[2] L .贝洛玛里尼等，[知识图中不确定性下的推理](https://2020.declarativeai.net/events/ruleml-rr/ruleml-rr-ap) (2020)，RuleML+RR 2020

[3] D. Fierens 等，[概率逻辑程序设计中的推理与学习加权布尔公式](https://www.cambridge.org/core/journals/theory-and-practice-of-logic-programming/article/inference-and-learning-in-probabilistic-logic-programs-using-weighted-boolean-formulas/9455B07774BA31AA4F6AB81FB0A6B013) (2015)，《逻辑程序设计理论与实践》

[4] J. Lee 和 Y. Wang，[稳定模型语义下的加权规则](http://peace.eas.asu.edu/joolee/papers/lpmln-kr.pdf) (2016)，KR。第 145-154 页。AAAI 出版社

[5] M. Denecker 等著，[逻辑程序再探:作为归纳定义的逻辑程序](https://dl.acm.org/doi/10.1145/383779.383789) (2001)，美国计算机学会译。计算机。日志。