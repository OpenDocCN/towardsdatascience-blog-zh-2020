# 你的创业公司在负责任地使用 AI 吗？

> 原文：<https://towardsdatascience.com/is-your-startup-using-ai-responsibly-4663ec43f880?source=collection_archive---------39----------------------->

![](img/252f14b6b59485e1c64a55439597c68c.png)

## **为什么模型**的公平性和**的可解释性不能满足它**

*更新:你现在可以用* [*日文*](https://ainow.ai/2020/07/15/225156/) *阅读这篇文章！(感谢 Koki Yoshimoto)。*

自从科技公司开始利用这项技术以来，他们已经收到了无数关于不道德使用人工智能的指控。

一个例子来自 Alphabet 的谷歌，它创造了一种仇恨言论检测算法，给非裔美国人的言论分配了比白人更高的“毒性分数”。

华盛顿大学的研究人员分析了数千条被算法视为“冒犯”或“仇恨”的推文的数据库，发现黑人对齐的英语更有可能被贴上仇恨言论的标签。

![](img/91ec38919f86e31a9e433de2e43e27a9.png)

照片由[米卡·鲍梅斯特](https://unsplash.com/@mbaumi?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)在 [Unsplash](https://unsplash.com/s/photos/hate-speech?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) 上拍摄

这是人工智能算法出现偏见的无数例子之一。可以理解的是，这些问题引起了很多关注。关于伦理和偏见的对话是最近人工智能领域的热门话题之一。

各行各业的组织和行动者都在从事[研究](https://ainowinstitute.org/research.html)通过公平、问责、透明和道德(FATE)来消除偏见。然而，仅仅专注于模型架构和工程的研究必然会产生有限的结果。那么，如何解决这个问题呢？

# **消除对抗人工智能偏见的误解**

![](img/5ab73969e96e60a2977af46777532f48.png)

马库斯·斯皮斯克在 [Unsplash](https://unsplash.com/?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) 上拍摄的照片

修复模型是不够的，因为这不是根本原因所在。为了找出哪些措施可以产生更好的效果，我们必须首先了解真正的原因。然后，我们可以通过研究我们在现实世界中如何应对这种偏见来寻找潜在的解决方案。

人工智能模型通过研究模式和从历史数据中识别洞察力来学习。但人类历史(以及我们的现在)远非完美。因此，毫不奇怪，这些模型最终模仿并放大了用于训练它们的数据中的偏差。

这是我们大家都相当清楚的。但是，我们如何处理我们世界中这种固有的偏见呢？

> 在现实世界中，我们必须注入偏见以对抗偏见。

当我们觉得一个社区或一部分人口可能处于不利地位时，我们会避免仅仅根据过去的情况得出结论。有时，我们会更进一步，进行包容，为这些细分市场提供机会。这是扭转趋势的一小步。

这正是我们在教授模型时必须采取的步骤。那么，我们如何注入人类偏见来对抗模型固有的“习得性”偏见呢？以下是实现这一目标的一些步骤。

# **1。为您的数据科学团队添加不同的角色**

![](img/13245765424e12c2ed23e0c88e1e2a7b.png)

照片由[丹尼斯·莱昂](https://unsplash.com/@denisseleon?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)在 [Unsplash](https://unsplash.com/?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) 上拍摄

在构建负责任的人工智能模型时，你需要你的团队超越技术。虽然对数据科学专业人员进行数据隐私教育很重要，但这种做法的好处有限。通过[引入来自社会科学和人文学科的](https://techhq.com/2019/12/a-complete-data-science-team-requires-more-than-just-data-scientists/)人，你可以获得技能和专业知识，这将帮助你减轻人工智能模型中的潜在偏见。

来自这些背景的人将会更好地理解用户和伦理考虑，并从人的角度提供见解。人类学家和社会学家可以发现模型中可能被创建模型的数据科学家忽略的刻板印象，并可以纠正数据中潜在的偏见。

数据科学团队中的行为心理学家可以帮助弥合用户和技术之间的差距，并确保模型结果的公平性。

> 在构建负责任的人工智能模型时，你必须超越技术。

除了重视不同的技能组合，数据科学团队引入更多不同性别、种族或国籍的成员也至关重要。多元化的团队提供新的视角，质疑可能过时的古老规范，并防止团队陷入[群体思维](https://www.psychologytoday.com/us/basics/groupthink)的陷阱。

例如，开发 iOS YouTube 应用程序的[谷歌团队](https://www.engadget.com/2014/09/25/unconscious-bias-is-why-we-dont-have-a-diverse-workplace-says/)在添加移动上传时没有考虑左撇子用户，因为团队中的所有人都是右撇子。

这意味着在左撇子视角下录制的视频会上下颠倒。只需要几个左撇子就能让这款应用对占全球人口 10%的人更加友好。

# **2。将人类融入循环**

![](img/45828560b2fe7e9503ecf88b9874d4eb.png)

在 [Unsplash](https://unsplash.com/?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) 上[科普高清](https://unsplash.com/@scienceinhd?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)照片

无论你的模型有多复杂，都需要人工干预。人类可以是故障安全机制的一部分，或者更好的是，数据科学团队可以[在持续的基础上结合人类的判断](https://hai.stanford.edu/news/humans-loop-design-interactive-ai-systems)并逐步丰富模型。

随着人工智能被用于涉及人类健康或生命的关键领域，它应该被视为零容忍的任务关键型——不仅仅是宕机或错误，还包括偏差。实现这一点的唯一方法是让人类负责，以确保避免人工智能偏见。

> 计划模型的零容忍，不仅仅是停机时间或错误，还有偏差。

佛罗里达州朱庇特一家医院的医生举例说明了永远不要盲目信任人工智能算法的必要性:他们拒绝了 IBM Watson 关于癌症治疗的建议，这些建议可能会产生致命的后果。数据科学团队不断面临构建更好的模型和万无一失的系统的挑战。

然而，这并不意味着人类可以被排除在系统之外。事情确实会出错，即使是在少数情况下。我们必须将人设计到决策过程中，以便在每一个这样的情况下进行控制。

# **3。让您的数据科学团队负起责任**

![](img/271f807309caaa35f80143c17ba2987e.png)

谢恩·艾弗里在 [Unsplash](https://unsplash.com/?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) 上拍摄的照片

大多数业务数据科学团队的主要目标都围绕着创造更多收入、设计最精确的模型和自动化流程以实现最高效率。然而，这些目标忽略了某人(或多人)必须确保“正确的”事情正在被做。

数据科学团队必须对结果负责，必须确保他们解决的业务问题不会以牺牲道德准则为代价。扪心自问:您的数据科学团队是否仅仅受到收入和时间表的激励？或者他们是否认为负责任的产品使用和公平的结果是项目成功的标准？

如果是前者，你需要重新思考驱动你的团队的目标。

> 为了确保负责任地使用人工智能，你必须提升你的数据科学团队的道德品质。

为了确保负责任地使用人工智能，你必须提升你的数据科学团队的道德品质。这需要积极的保护和持续的教育。此外，你还必须为高级职位做好准备，比如首席道德官或 T2 道德委员会，他们将成为你产品的道德监督人。

然而，这并没有消除将责任推给其他相关人员的需要:你需要在所有层面上承担责任。例如，[宝拉·戈德曼](https://www.salesforce.com/company/news-press/stories/2018/12/121018-i/)成为 Salesforce 的首位首席道德和人道使用官。

通过持续讨论人工智能解决方案的质量及其对整个社会的影响，你可以从高层灌输一种责任感，并确保对团队其他成员产生涓滴效应。也有可用的最佳实践和指南，比如谷歌的这些。

虽然大型科技公司普遍存在人工智能道德失误，但我们也看到了一些正确的举措。微软和 IBM 都公开承诺解决他们自己和第三方程序中的偏见。

# **4。让你的用户知道人工智能并不完美**

![](img/f40b52bfe35af9ef1a8f471a8b032f80.png)

[摇滚猴子](https://unsplash.com/@rocknrollmonkey?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)在 [Unsplash](https://unsplash.com/?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) 上的照片

公司内部和更广泛的消费者群体中的许多个人过于信任人工智能，而不了解它的能力和缺陷。虽然商业领袖经常忽视这一点，但我们不能指望所有消费者都理解今天人工智能的前景。

人们认为人工智能可以创造奇迹，但如果没有数据、人才或正确的流程，它将注定失败。教育团队成员和消费者人工智能仍处于初始阶段——并且应该被如此对待——对于避免导致灾难性结果和更大失望的盲目信任至关重要。

> 用户必须明白，人工智能解决方案是用来提供信息，而不是发号施令。

对人工智能算法能力的期望必须保持现实。就像你可能不愿意让你的汽车在拥挤的街道上以自动驾驶模式行驶一样，我们今天必须了解它的局限性。人们需要理解，人工智能解决方案应该以同样的方式来看待——它们是为了提供信息，而不是发号施令。

# **5。建立一种促进好奇心、质疑信念的意愿和改变的灵活性的文化**

![](img/c2d3b69c4d639fc573b2b7ff17886be8.png)

约瑟夫·罗萨莱斯在 [Unsplash](https://unsplash.com/?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) 上的照片

最终，为了实现负责任地使用人工智能，你需要在你组织的核心中嵌入某些属性。文化是经年累月形成的，极难改变。渴望成为数据驱动的组织必须在其文化核心中拥有某些属性。当你的创业处于早期阶段，而不是几年后，就播下这些种子。

那么，对于负责任和合乎道德地使用人工智能来说，这些至关重要的属性是什么呢？

**好奇心。**问自己:所有团队成员都愿意尝试必要的步骤来找到他们需要的答案吗？或者，他们是否乐于执行设计好的步骤和流程？一个好奇的团队将找到让人工智能工作以满足结果的方法。

接下来是**质疑信念的意愿**:你的公司是否有一个健康的环境让团队质疑既定的实践？上级会听取并鼓励具有挑战性的反馈吗？团队必须有一种开放的文化，当他们看到 AI 的实现方式不符合组织的理想时，他们可以大胆地说出来。

最后，公司文化必须促进**改变**的灵活性。与技术和数据科学打交道自然会涉及许多变化——无论是在创建解决方案还是采用解决方案方面。团队是否愿意根据他们通过好奇和质疑设定的流程所发现的东西来进行调整？

![](img/e912689c4f80259258bb9c7d303956bb.png)

照片由[肖恩·斯特拉顿](https://unsplash.com/@seanstratton?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)在 [Unsplash](https://unsplash.com/s/photos/harmony?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) 上拍摄

拥有正确的公司文化为合乎道德的人工智能使用奠定了基础。众所周知，脸书提倡“快速行动，打破常规”的文化考虑到这个科技巨头面临的无数关于用户隐私和滥用数据的丑闻，这个咒语显然没有导致合理的人工智能使用。

> 负责任地使用人工智能不会仅仅通过对模型进行一些调整就发生。

负责任地使用 AI 不是一蹴而就的事情，也不是简单地对模型进行一次调整就能确保并期待奇迹般的结果的事情。

正如我们在当前席卷科技行业的“[道德洗涤](https://www.technologyreview.com/s/614992/ai-ethics-washing-time-to-act/)的指责浪潮中所看到的，简单地声明你将打击人工智能偏见，而没有什么行动来支持这一声明是不会成功的。

避免人工智能偏差只能通过在不同阶段以多种方式增加人类输入来实现。通过使数据科学团队多样化，将人融入流程，让团队负起责任，对人工智能的能力设定现实的预期，以及最后——也许是最重要的——建立正确的公司文化，你可以为人工智能在你的组织中的道德使用铺平道路。

*这篇文章最初是在 TechCrunch 的会员版 ExtraCrunch 上发表的*[](https://techcrunch.com/2020/02/06/is-your-startup-using-ai-responsibly/)**。增加了插图。封面图片由*[](https://unsplash.com/@matthewhenry?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)**[](https://unsplash.com/?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)*组成****