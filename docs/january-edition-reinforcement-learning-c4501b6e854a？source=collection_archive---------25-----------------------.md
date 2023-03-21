# 一月版:强化学习

> 原文：<https://towardsdatascience.com/january-edition-reinforcement-learning-c4501b6e854a?source=collection_archive---------25----------------------->

![](img/da1a63e67aa542a0038cde3313e7802a.png)

**图片来源:** [**OpenAI**](https://openai.com/blog/safety-gym/)

新年快乐，欢迎来到 TDS 的新十年。随着新十年的开始，有必要回顾一下不久前的过去。虽然数据科学和机器学习的大多数理论自 20 世纪 60 年代以来就已经存在，但正是从 21 世纪初开始的“大数据”的可用性推动了数据科学和机器学习在每个主要行业中无与伦比的增长和成功。

今天，这一里程碑已经过去 20 年了，直到 2010 年代中期，大多数进展都是在“经典”机器学习方面，这些机器学习能够通过 100 到 10，000 个训练样本取得良好的结果。然而，从 2010 年代中期开始，数据集规模的爆炸为深度学习模型铺平了道路，这些模型能够显著优于上一代。但从昨天开始，就连深度学习也有了 2010 年的感觉。那么接下来会发生什么呢？

显然，这肯定需要更多的数据。也许是无限数据？向强化学习问好——街区的新成员。强化学习可能是实现人工通用智能(AGI)的最热门候选人，但它对数据的渴望迄今为止一直阻碍着它。强化学习主要应用于游戏或机器人等可以产生几乎无限量数据的场景，这并非巧合。

听起来很酷——但是什么是强化学习，我为什么要关心？好吧，你来对地方了，我们收集了八篇好文章来帮助你开始强化学习之旅。第一篇文章涵盖了基础知识，然后我们提供了一些教程和实践文章，最后，对于已经(或刚刚成为)强化学习专家的人来说，还有一些高级主题。

Anton Muehlemann——determined . ai 的编辑助理/ AMLE

## [**强化学习 101**](/reinforcement-learning-101-e24b50e1d292)

通过 [Shweta Bhatt](https://medium.com/u/cd0e3acad9d6?source=post_page-----c4501b6e854a--------------------------------) — 6 分钟读取

*开始强化学习(RL)之旅的最佳地点。本文涵盖了 5 个基本的 RL 问题。*

由[耶稣罗德里格斯](https://medium.com/u/46674a2c9422?source=post_page-----c4501b6e854a--------------------------------) — 6 分钟阅读

*这篇文章评论了 MuZero，一个通过从零开始学习规则而掌握了几个策略游戏的 AI 智能体。穆泽罗是著名的 AlphaZero 引擎的继任者，该引擎击败了 AlphaGo，alpha Go 本身是第一个击败职业围棋选手的人工智能——这项任务长期以来被认为是任何人工智能都无法完成的。*

## [**现实生活规划问题的强化学习**](/reinforcement-learning-for-real-life-planning-problems-31314491e5c)

斯特林·奥斯本博士研究员——15 分钟阅读

*关于使用 RL 解决数据有限的现实世界问题的综合指南。*

## [**具有深度 Q 网络的 Cart-Pole 上的强化学习概念**](/reinforcement-learning-concept-on-cart-pole-with-dqn-799105ca670)

通过 [Vitou Phy](https://medium.com/u/81157f79faae?source=post_page-----c4501b6e854a--------------------------------) — 6 分钟读取

RL 的一个主要例子是车杆问题。在这个游戏中，你要通过左右移动来平衡一根直立的柱子。本文展示了如何使用深度 Q 网络来解决这个问题。

## [**高级 DQNs:玩吃豆人深度强化学习**](/advanced-dqns-playing-pac-man-with-deep-reinforcement-learning-3ffbd99e0814)

杰克·格雷斯比——22 分钟阅读

*本文介绍了对经典 dqn 的改进，并展示了如何应用它来越好地玩吃豆人游戏。*

## [**更聪明地交易和投资——强化学习方式**](/trade-smarter-w-reinforcement-learning-a5e91163f315)

由[亚当·金](https://medium.com/u/6e3a5234f1cc?source=post_page-----c4501b6e854a--------------------------------) — 19 分钟读完

*厌倦了游戏？赚点钱怎么样？虽然我们不能保证任何财务收益，但您一定会获得关于 TensorTrade 的知识，TensorTrade 是一个使用 RL 进行交易和投资的开源 python 框架。*

## [T3【谷歌新星球强化学习网】你需要知道的一切 ](/everything-you-need-to-know-about-googles-new-planet-reinforcement-learning-network-144c2ca3f284)

通过[测测邵](https://medium.com/u/370d0382c596?source=post_page-----c4501b6e854a--------------------------------) — 8 分钟读取

*本文回顾了 Google AI 针对 RL 的深度规划网络。PlaNet 通过潜在动力学模型、基于模型的规划和迁移学习解决了经典 RL 模型的几个缺点。*

## [**神经架构搜索—限制与扩展**](/neural-architecture-search-limitations-and-extensions-8141bec7681f)

由亚历克斯·亚当 — 11 分钟读完

*神经架构搜索是这样一种想法，它不是让专家为给定的问题找到一个好的模型，而是由 RL 算法为您找到。一个主要的问题是搜索结果的庞大，因此 NAS 算法占用了大量的计算资源。本文强调了这些限制，并展示了一些可能的扩展，以使 NAS 性能更好。*

我们也感谢最近加入我们的所有伟大的新作家，[安德鲁·郝](https://medium.com/u/f269ed708b9?source=post_page-----c4501b6e854a--------------------------------)，[路易·德·贝诺伊斯](https://medium.com/u/4c5850bd7dd8?source=post_page-----c4501b6e854a--------------------------------)，[豪斯](https://medium.com/u/378ca677f7b9?source=post_page-----c4501b6e854a--------------------------------)，[罗伯特·库伯勒博士](https://medium.com/u/6d6b5fb431bf?source=post_page-----c4501b6e854a--------------------------------)，[尼克·霍尔马克](https://medium.com/u/e6411ede297e?source=post_page-----c4501b6e854a--------------------------------)，[罗伯托·桑纳扎罗](https://medium.com/u/21007a38b704?source=post_page-----c4501b6e854a--------------------------------)，[陈卿](https://medium.com/u/baf0bb437832?source=post_page-----c4501b6e854a--------------------------------)，[丹·西格尔](https://medium.com/u/836515184818?source=post_page-----c4501b6e854a--------------------------------)，，[马丁·贝克](https://medium.com/u/a250c85fa851?source=post_page-----c4501b6e854a--------------------------------)，[我们邀请你看看他们的简介，看看他们的工作。](https://medium.com/u/4c5850bd7dd8?source=post_page-----c4501b6e854a--------------------------------)