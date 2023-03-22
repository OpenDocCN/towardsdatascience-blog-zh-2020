# 行为代码分析 101:利用法庭技术

> 原文：<https://towardsdatascience.com/behavioural-code-analysis-65a226e1a601?source=collection_archive---------80----------------------->

## 技术债务不仅仅是技术上的东西。行为分析在发现技术债务的形成过程中起到了拯救作用。

行为代码分析之于静态代码分析，正如宇宙学之于天文学。它处理宏观尺度，即代码库的宇宙——从它的开始到当前状态，不要忘记幕后的组织机构。

![](img/73084fd2f3ab34d3d0a01ed0ad15d302.png)

***行为代码分析是关于宏的。关于大局。*** 照片由[格雷格·拉科齐](https://unsplash.com/@grakozy?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)在 [Unsplash](https://unsplash.com/s/photos/astronomy?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) 上拍摄

我已经使用 Sonarqube 来帮助测量 React 项目中的技术债务。作为一个静态分析工具，它返回一个技术负债最高的文件列表。主要的一个是 *renderer.js* ，有九个问题，包括重复和无用赋值。但这是技术债吗，值得重构吗？

所以我在这里的观点是，静态分析不应该成为指引我们重构之旅的北极星。它只告诉我们代码库的症状。不是疾病。重构后看起来很糟糕的代码可能会很容易地返回到以前的状态，如果你真的不知道它最初为什么会处于那种状态。

诸如贡献者的数量、变更的频率、变更耦合的度量是揭示代码是否腐烂以及为什么腐烂的基础。你不能像 Sonarqube 那样，把一个又一个文件作为单独的部分来分析，而是要通过行为分析把它们作为一个网络来分析。

***目录***

*   ***技术债不是技术债***
*   ***行为代码分析技巧***
*   ***组织机构***

# **技术债*不是技术债*** 😕

> “一塌糊涂不是技术债。它没有机会在未来支付。一塌糊涂就是**永远**亏本。”罗伯特·马丁。

技术债务就像金融债务一样，随着时间的推移会变得越来越昂贵，除非我们不断偿还。我们这样做是因为这是一种生存行为，一种赌注，以保证我们的软件满足短期的商业期望。不利的一面是差点的实施。没有正确地在将来集成其他新功能。

我们的技术债务越多，就越接近技术破产，在这种状态下，集成新功能的时间太长了，这使得它们不值得。

![](img/74da7fc9f4ab02a9bd25779a0b7b9ad1.png)

[技术债务与项目生命周期的关系。](https://efficientuser.files.wordpress.com/2019/11/1_tecdebt.jpg?w=555)

**留意** [**莱曼持续变化定律**](https://en.wikipedia.org/wiki/Lehman%27s_laws_of_software_evolution) **，没有新特性，你的软件会变得没那么有用。那么，无用的作业怎么会是技术债务呢？嗯，我们不能只看代码就知道，我们必须看版本控制工具，它会告诉我们维护这个文件有多困难。**

# ***行为代码分析技术***

[研究表明，文件更改的次数是质量下降的一个很好的指标，会导致错误。为什么不呢？如果一个类改变得太频繁，是不是因为它有太多的责任，或者对它的理解不够。此外，我们经常工作的地方也是重构最有用的地方。](https://ieeexplore.ieee.org/document/859533/)

## 热点

亚当·桑希尔称之为热点。React 有很多这样的例子，ExhaustiveDeps.js 就是其中之一。这些都是潜在的重构目标，是滋生错误代码的温床。它们是复杂代码和活动代码的结果。

![](img/4054461009ba55df20440fef82f0a746.png)

[通过 Kibana 显示 React 的热点。](https://github.com/pbmiguel/behavioural-code-analyser)

## 复杂性趋势

然后，我们可以开始确定每个潜在重构目标的**复杂度趋势**。就像火山一样，我们可以分类热点是**活跃**(即复杂性增加的趋势)**休眠**(复杂性不变的趋势)，还是**消亡**(复杂性趋势已经开始降低)。虽然文件*expletivedeps . js*处于休眠状态，但还有其他文件处于活动状态，如 *ReactFiberWorkLoop.old.js、*。如果不注意的话，将会持续增长到无法维持。

## 更换联轴器

*reactfiberworkloop . old . js*是 2759 行代码，本身有 50 多个函数。在掌握它的所有功能之前，我们可以应用另一种技术来理解所有这些功能进化背后的动力。那就是，**改变联结。**这是两个函数/文件之间的一种共享逻辑，如果你改变其中一个，你很可能必须改变另一个。注意，这不同于软件工程中通常的“耦合”，因为它是不可见的**，**隐含在代码中。我们只能通过观察随着时间的推移所做的改变来注意到它。很快，可以注意到函数 *commitRootImpl* 和 *prepareFreshStack* 之间的耦合百分比非常高，为 47%。因此，这成为我们开始重构的候选对象。

高耦合度并不一定是错误的，事实上，它在单元测试和它们各自的类中都是意料之中的，但是在其他情况下，我们必须开始问自己它是否容易被察觉。例如，通过遵循 [**接近**](https://en.wikipedia.org/wiki/Principles_of_grouping) 原则，将一起变化的函数分组，我们正在传递仅通过代码不可能表达的信息。

总之，通过热点、复杂性趋势和变化耦合等技术，我们有了一个不同于 Sonarqube 最初给我们的视角。这是因为行为分析背后的范式不同于 Sonarqube，在 Sonarqube 中，从维护的角度来看，并非所有代码都同等重要。仅仅因为一些代码是坏的，并不意味着它是一个问题。我们工作最多的代码越重要。

# 组织机构这个问题不是技术性的，而是社会性的。

技术债总会有的。重要的是我们如何处理它。大自然母亲长期以来一直教导我们要学会适应未来不可预见的常态。而这正是根源所在，把 [**的越轨行为正常化:**](https://books.google.pt/books?id=-wOfDwAAQBAJ&pg=PA325&lpg=PA325&dq=%22The+gradual+process+through+which+unacceptable+practice+or+standards+become+acceptable.+As+the+deviant+behavior+is+repeated+without+catastrophic+results,+it+becomes+the+social+norm+for+the+organization%22&source=bl&ots=4pJ401-ALr&sig=ACfU3U12YlYpdtyw3t4mUHn6y6k7LqGT9A&hl=pt-PT&sa=X&ved=2ahUKEwiOzc-1sITqAhUc8uAKHcb5AYwQ6AEwAXoECAoQAQ#v=onepage&q=%22The%20gradual%20process%20through%20which%20unacceptable%20practice%20or%20standards%20become%20acceptable.%20As%20the%20deviant%20behavior%20is%20repeated%20without%20catastrophic%20results%2C%20it%20becomes%20the%20social%20norm%20for%20the%20organization%22&f=false)

> 不可接受的实践或标准逐渐变得可接受的过程。当越轨行为被重复而没有灾难性的结果时，它就成为了组织的社会规范。

## 解决原因，而不是症状

在软件领域，这意味着当我们接受并继续使用不寻常的功能/系统/行为时，它们就变成了新常态，而其他未来的偏差就变成了另一个新常态，一次又一次。这种现象是无国界的，即使是美国宇航局，一个错误就可能导致生命的损失，也经历过不同的时间——挑战者号，哥伦比亚号。想想看，如果这种现象发生在 Nasa 的组织文化中，为什么在我们的日常项目中不会发生呢？然而，开发商是罪魁祸首。因为没有其他人应该为好看的可维护代码辩护。

> “在不召开大型员工会议的情况下，你能在多大程度上实现新功能，这是对一个架构成功与否的最终检验。”作者亚当·托恩希尔。

自 70 年代以来，软件行业的人口持续增长，[，预计未来四年全球增长率将达到 20%](https://www.future-processing.com/blog/how-many-developers-are-there-in-the-world-in-2019/)。意思是发展变得越来越团队化。

然而，社会学已经证明，一个团队拥有的元素越多，就越难以协调，也就越有可能出现沟通上的差距，即所谓的[](https://dictionary.apa.org/process-loss)****。**此外，在团队中工作时，我们更容易受到影响，因为无论你是否注意到，我们的价值观和决策都会突然受到团队行为的影响。这种现象通常被称为 [**旁观者效应**](https://dictionary.apa.org/bystander-effect) ，可以通过两个主要的社会方面来解释。**

*   **[**多元无知**](https://dictionary.apa.org/pluralistic-ignorance) 。这是一个群体状态，群体的规范被公开接受，却被私下拒绝。这是因为每个人都觉得自己是唯一这样想的人，害怕被排斥。所以所有人都没有采取行动。例如，这在人际关系中很常见，在学生课堂上——当害怕在课堂上暴露怀疑时——甚至在紧急情况下，如果有一个旁观者而不是一群人在场，受害者有更好的生存机会。**
*   **[**责任扩散**](https://dictionary.apa.org/diffusion-of-responsibility) 。一个人的状态，在一大群人当中感觉不那么负责，不那么负责。责任被稀释了，从个人到群体，但如果每个人都有这种感觉，那么就没有人会对那个责任负责。**

**![](img/71aea822f6b1d57e6067532a198adc50.png)**

**这个问题不是技术性的，而是社会性的。照片由[哈维尔·阿莱格·巴罗斯](https://unsplash.com/@soymeraki?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)在 [Unsplash](https://unsplash.com/s/photos/team?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) 上拍摄**

**除了关注重构什么，我们不应该低估组织机构。[研究表明，实践代码评审，并有一个主要的维护者对代码的质量有积极的影响。这个问题不是技术性的，而是社会性的。我们不应该只依靠自己，因为我们是主观的，有偏见的，适应性强的。但是我们可以利用从行为分析中得出的不带偏见的、客观的试探法，来了解幕后发生的事情。](https://dl.acm.org/doi/10.1145/1653662.1653717)**

***我的行为代码分析的学习之路是从亚当·托希尔的书开始的，* [*【你的代码作为犯罪现场】*](https://www.goodreads.com/book/show/23627482-your-code-as-a-crime-scene)*[*软件设计 x 光片*](https://www.goodreads.com/book/show/36517037-software-design-x-rays) *。在这段旅程中，我学到了更多关于代码的社会方面，除了跟踪代码本身，版本控制工具还有多强大。最后，让我开发一个新的开源工具，它可以很好地完成上面强调的技术。****

***[](https://github.com/pbmiguel/behavioural-code-analyser) [## 行为代码分析器

### 该工具利用 Kibana 的可视化特性，提供更加可定制的分析体验。

github.com](https://github.com/pbmiguel/behavioural-code-analyser)***