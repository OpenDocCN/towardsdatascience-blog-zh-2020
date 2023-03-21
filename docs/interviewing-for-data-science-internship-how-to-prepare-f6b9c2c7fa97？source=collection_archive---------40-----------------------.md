# 数据科学实习面试。如何准备。

> 原文：<https://towardsdatascience.com/interviewing-for-data-science-internship-how-to-prepare-f6b9c2c7fa97?source=collection_archive---------40----------------------->

## 第一步:做好基础工作

![](img/44d9edccef19367e253399d77a569edd.png)

在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上 [Keagan Henman](https://unsplash.com/@henmankk?utm_source=medium&utm_medium=referral) 拍摄的照片

> 不幸的是，这一次，你的申请没有成功，我们已经任命了一名申请人…

听起来很熟悉，对吧？在我花了这么多时间准备面试后，拒绝接踵而至。虽然我通过了最初的几个面试阶段，但在面对面的阶段，我并不顺利。“我是多么失败啊”，我想。

我开始寻找改进的方法。我已经确定了一些通常被忽视但可能对面试结果产生巨大*影响的领域。这反过来又帮助我提高并得到了一份我想要的工作！*

# 掌握基本知识

![](img/ace3165bbc4c9cff368882d21dd6e638.png)

照片由[粘土堤](https://unsplash.com/@claybanks?utm_source=medium&utm_medium=referral)在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄

DS 实习通常竞争激烈，招聘人员的任何危险信号都可能决定你是否会被直接拒绝。其中一个危险信号是你的基础是否足够好。数据科学是一个需要你有很好的数学和编程知识的领域。

怎么才能提高？对于数据科学理论，我建议对最常见的算法有很好的数学理解。我平时推荐的书有两本: [*模式识别与机器学习*](https://www.goodreads.com/book/show/55881.Pattern_Recognition_and_Machine_Learning) 和机器学习 [*第一课*](https://www.goodreads.com/book/show/32758159-a-first-course-in-machine-learning) 。它们都包含对机器学习算法的深入数学解释，这些解释将帮助您将 DS 面试问题粉碎！

根据公司的不同，你可能还会被问到编程方面的问题。它们通常没有那么难，但是考虑到压力和时间限制，你真的需要掌握它们。你应该预料到从排序、递归到数据结构的所有问题。尽早开始练习这些题是有好处的。为了更好地理解如何处理编码问题，我推荐阅读《破解编码面试 的书 [*。要获得更多实践经验，请访问*](https://www.goodreads.com/en/book/show/12544648-cracking-the-coding-interview) *[*黑客排行榜*](https://www.hackerrank.com/) ，或 [*LeetCode*](https://leetcode.com/) 。*

# 玻璃门是你最好的朋友

你也可以从 Glassdoor 的评论中感受到这家公司的文化和氛围。这可以给你一个很好的提示，说明这家公司是否适合你。举例来说，如果一家公司的氛围看起来真的很糟糕，那么撤回申请，花更多时间准备其他公司的面试可能会更好？去那些你并不真正想去的公司面试有什么意义呢？

你也可以找到一些关于面试结构或者他们问的问题类型的有用信息。有些公司每次都在问*相同的*系列问题！我不知道他们为什么这样做，但是在这种情况下，你应该注意到这些问题在 Glassdoor 评论中被重复了。你可以利用这一点，把它们背下来。

# 简单的面试问题并不容易

![](img/837a18bd381e4fdd880bfc1b4e989472.png)

Jules Bss 在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄的照片

想象一种情况，当面试官问:线性回归是什么？

你可以回答:

> 它是一种线性方法，对因变量和自变量之间的数据关系进行建模。

或者:

> 它是一种线性方法，对因变量和自变量之间的数据关系进行建模。该模型的参数可以使用普通的最小二乘法导出，并且通用方程适用于多维数据。这是一种简单、快速且可解释的算法。但是，它有某些警告，例如…

你明白我的意思吗？通过问一个看起来简单的问题，面试官可以测试两件事。首先，如果你有一个基本的知识(显而易见)。其次，它测试你的理解深度和你在研究某个话题时的好奇心。这种能力在数据科学家的技能组合中至关重要，因为您将经常需要使用新工具和阅读研究论文。如果你没有彻底分析这个主题，没有理解它的局限性和能力，这是一条通向失败项目的直路。

# 展示项目。质量还是数量？

*TLDR；质量！*

![](img/47fae0f19169bda22fa0da8887eab9f5.png)

好的高质量的展示项目能给你的面试官留下深刻印象。[【来源】](https://octodex.github.com/pythocat/)

令人痛苦的事实是，没有人关心你为 100 多个迷你项目创建的无尽的 Jupyter 笔记本。不要误解我的意思:这仍然是试验新模型和数据的好方法。但是，最有可能的是，它不会给面试官留下深刻印象。

数据科学不仅仅是在一个文件中创建几十个*未经测试的*机器学习模型。在现实场景中，代码需要使用内部服务器或云服务进行测试、打包、记录和部署。

我的建议？追求*质量*并致力于创造 3 个更大的项目，给面试官留下深刻印象*。这里有一些你可以遵循的建议:*

*   找到一个需要大量预处理和 EDA 的真实数据集
*   使您的代码模块化:为模型、数据预处理和端到端管道创建单独的类
*   在开发打包代码时，使用[测试驱动开发(TDD)](https://en.wikipedia.org/wiki/Test-driven_development)
*   使用 Git 和持续集成服务，如 [CircleCI](https://circleci.com/)
*   向用户公开模型的 API，例如 Python 的 [Flask](https://flask.palletsprojects.com/en/1.1.x/)
*   使用 [Sphinx](https://www.sphinx-doc.org/en/master/) 记录代码，并遵循代码样式指南(例如 [PEP-8](https://www.python.org/dev/peps/pep-0008/) 用于 Python)

Udemy 的 *Babylon Health* 和 *Train In Data* 的数据科学家创建了一个非常好的 ML 模型部署课程。你可以在这里找到它。

## 奖励:简历模板

我非常喜欢数据科学实习的单页简历。它帮助我保持简单明了，没有多余的信息。我以前有一个 Word 模板，但是我花了很多时间去修改它。当我删除或添加一些信息的时候，格式立刻被吹走了，让我的简历看起来像一个谜😆

无论如何，我找到了一个好看的背页简历模板，我一直在用。它简单、清晰，最重要的是，它是用模块化的 Latex 代码呈现的，这使得格式化成为一项轻松的任务。[简历模板的链接在这里](https://www.overleaf.com/latex/templates/awesome-cv/dfnvtnhzhhbm)。

# 关于我

我是阿姆斯特丹大学的人工智能硕士学生。在我的业余时间，你可以发现我摆弄数据或者调试我的深度学习模型(我发誓这很有效！).我也喜欢徒步旅行:)

如果你想了解我的最新文章和其他有用的内容，以下是我的社交媒体资料:

*   [领英](https://www.linkedin.com/in/kacperkubara/)
*   [Github](https://github.com/KacperKubara)
*   [个人网站](https://kacperkubara.com/)