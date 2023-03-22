# 如何开始学习强化学习(RL)

> 原文：<https://towardsdatascience.com/where-to-start-learning-rl-e294b6879ad1?source=collection_archive---------40----------------------->

## 在这些工具成为行业标准之前，学会实现它们。

![](img/9eb1d56b8477f015909354d1916845cc.png)

去追它。凯文·Ku 从[派克斯](https://www.pexels.com/photo/coding-computer-data-depth-of-field-577585/?utm_content=attributionCopyText&utm_medium=referral&utm_source=pexels)拍摄的照片。

建立和反思。有很多强化学习的资源，但是它们并不总是容易融入到你自己的项目中。构建自己的项目。让代码成为你自己的，这就是知道 强化学习的*的人和知道*如何* 强化学习的人之间的区别。*

*这篇文章展示了学习如何在学习中胜任是易处理的，它提供了足够的资源来建立一个完整的知识。*

我会说我在机器人学习(机器人+强化学习)方面很有能力。我很荣幸在我的博士学位上被推到这一步，但是你也可以。主题是可重复和有效的。

## 实践中学习

我们学习计算机科学的新技能来制作东西。将想法编写成代码是创造价值的地方(除了少数理论教授)。这个帖子的关键在于你需要 ***找到你的问题空间*** 。

在本文的最后有一个长长的资源列表，可以学习 RL 理论，但是随着 AI 方法的广泛应用，你必须选择哪里。这归结为三个动机的叠加:

1.  你喜欢解决的问题。
2.  具有全球影响的问题。
3.  能让你获得工作和稳定的问题。

为 RL 决定一个问题空间，在那里你喜欢你正在做的事情，它会做一些事情来帮助这个世界，希望其他人会明白并给你一个更大的平台来做出改变。

![](img/07dca84d6822a35612a11876d9b3bf14.png)

一个模拟机器人手臂任务的例子——称为 Reacher3d。使用 [Mujoco](http://www.mujoco.org/) 和[健身房](https://gym.openai.com/)。

我建造了什么？我和机器人一起工作。我希望机器人在任何地方都能做许多简单的工作。他们可以帮我们搬家具、开车、送箱子等等。所有这些都将在 T2 十年内实现。十年后，这看起来像学习低级运动控制器。**学习机器人动力学和控制**的核心库在[这里](https://github.com/natolambert/dynamicslearn)找到。(*大部分的研究在发表之前还在私人进行。*)

## 通过写作或思考建立基础和深度

我已经在 Medium 上写了大约 20 篇帖子，这是对任何教育项目的惊人赞美。是时候反思你构建了什么，以及它如何融入一个更大的画面。这是确保其他人能够理解你的结果的时候。我遇到的最好的研究生都有一个共同的弱点——无法清晰地分解他们的想法。作为一名大四研究生，我专注于让我的工作持续下去，并在我完成学位后得到重用。

[](https://medium.com/@natolambert) [## 内森·兰伯特-中等

### 阅读媒体上内森·兰伯特的作品。学习者、非职业运动员、瑜伽师和机器人学习研究@加州大学伯克利分校…

medium.com](https://medium.com/@natolambert) 

研究论文、博客帖子等都是写作形式，是你头脑和自我的永久娱乐。很少有东西能让个人在职业生涯结束后继续发挥作用，但高质量的写作可能是我们目前最容易获得的工具。

迄今为止我在 RL 上写的帖子。这是一个奇妙的主题，永远有更多的东西需要探索。

1.  [深度 RL 算法要点。](/getting-just-the-gist-of-deep-rl-algorithms-dbffbfdf0dec)
2.  什么是马尔可夫决策过程？
3.  [RL 前要掌握的 3 个技巧](/3-skills-to-master-before-reinforcement-learning-rl-4176508aa324)。
4.  [强化学习的隐藏线性代数。](/the-hidden-linear-algebra-of-reinforcement-learning-406efdf066a)
5.  [强化学习的基本迭代方法。](/fundamental-iterative-methods-of-reinforcement-learning-df8ff078652a)
6.  [强化学习算法的收敛性](/convergence-of-reinforcement-learning-algorithms-3d917f66b3b7)

## 学习 PyTorch

PyTorch 正在机器学习研究领域占据主导地位，因为强化学习很年轻，所以它主要是研究。你可以在这里找到统计数据[。PyTorch 非常流畅，所以不要担心在学习它的过程中陷入困境，它可能会发生。](https://thegradient.pub/state-of-ml-frameworks-2019-pytorch-dominates-research-tensorflow-dominates-industry/)

# 概念工具

## 课程

[伯克利深度 RL 课程](http://rail.eecs.berkeley.edu/deeprlcourse/)来自[谢尔盖·莱文](https://people.eecs.berkeley.edu/~svlevine/)。

*   优势:由该地区最聪明、最有前途的人教授。非常现代，非常有技术含量，很好的项目。
*   缺点:对新人来说，这可能是一个挑战。需要很强的 python 技能。
*   其他点评:我是 2019 年上的这门课。会推荐。

 [## CS 285

### 加州大学伯克利分校的 CS 285 讲座:周一/周三上午 10-11:30，苏打厅，306 室讲座将被流式传输和录制。的…

rail.eecs.berkeley.edu](http://rail.eecs.berkeley.edu/deeprlcourse/) 

大卫·西尔弗在 UCL 的讲座:

*   优势:理论背景牛逼。
*   缺点:可能开始变得有点过时。自 2015 年以来，RL 发生了很多变化——主要是[软演员评论家](https://arxiv.org/abs/1801.01290)超级有效，而[基于模型的 RL](https://arxiv.org/pdf/1903.00374.pdf) 获得了更多关注。
*   其他评论:这些是我第一次看的讲座。我会在开始时推荐它们，因为它们非常有理论基础。没有太多的技术细节打断主题。

[](https://www.davidsilver.uk/teaching/) [## 教学-大卫·西尔弗

### 编辑描述

www.davidsilver.uk](https://www.davidsilver.uk/teaching/) 

## 书

[萨顿](https://homepages.inf.ed.ac.uk/csutton/) & [巴尔托](https://en.wikipedia.org/wiki/Andrew_Barto):强化学习

*   优势:教科书有很大的深度，例子，和上下文。这可以补充 RL 中的任何学习路径。
*   缺点:我不认识只看课本就能掌握一门学科的人。
*   其他评论:下面有一个很棒的 Python 代码伴侣，我也包括在内。

[](http://incompleteideas.net/book/the-book-2nd.html) [## 强化学习:导论

### 从亚马逊购买勘误表和注释完整的 Pdf 格式，无边距代码解决方案—发送您的解决方案一章，获得…

incompleteideas.net](http://incompleteideas.net/book/the-book-2nd.html) [](https://github.com/ShangtongZhang/reinforcement-learning-an-introduction) [## 疼痛/强化学习-介绍

### 如果你对代码有任何困惑或想报告一个错误，请打开一个问题，而不是直接给我发电子邮件…

github.com](https://github.com/ShangtongZhang/reinforcement-learning-an-introduction) 

罗素:人工智能——一种现代方法

*   优点:给出了使 RL 在更多自动化领域有用的背景。
*   缺点:RL 的尖端细节范围非常有限。
*   其他评论:它反映了我在加州大学伯克利分校教授的课程，CS 188。

[](http://aima.cs.berkeley.edu/) [## 人工智能:现代方法

### 人工智能领域的顶级教科书。在超过 125 个国家的 1400 多所大学中使用。第 22 名最…

aima.cs.berkeley.edu](http://aima.cs.berkeley.edu/) 

# 代码库

## 技术公司支持

OpenAI Spinning Up:这是我的最爱之一。它将彻底的理论阐述与编码教程结合在一起。它也是相当新的。*这是一个研究新算法以及代码与其前身有何不同的伟大工具。*

[](https://spinningup.openai.com/en/latest/) [## 欢迎来到极速旋转！-编制文件

### 编辑描述

spinningup.openai.com](https://spinningup.openai.com/en/latest/) 

Deepmind 在 Tensorflow 中的 RL 积木:我发现的新工具。这对于构建实用的系统非常有用，因为它抽象出了 RL 代理的各个部分。*它为状态空间的 q-learning 提供了可调用的 python 对象。*

[](https://github.com/deepmind/trfl/) [## deepmind/trfl

### TRFL(发音为“truffle”)是一个构建在 TensorFlow 之上的库，它为……提供了几个有用的构建块

github.com](https://github.com/deepmind/trfl/) 

## 研究报告

有些人正在为业余项目制作惊人的工具。我用过 [rlkit](https://github.com/vitchyr/rlkit) ，但是很难区分它们。现在入门的推荐 PyTorch，下面有些用 Tensorflow。这是一个巨大的区别因素，但这取决于你来决定。从部署机器学习系统的其他领域的经验来看，Tensorflow 也可能更适用于你。

[](https://github.com/vitchyr/rlkit) [## vitchyr/rlkit

### PyTorch 中实现的强化学习框架和算法。实现的算法:斜配合加固…

github.com](https://github.com/vitchyr/rlkit) [](https://github.com/dennybritz/reinforcement-learning) [## Denny britz/强化学习

### 这个库为流行的强化学习算法提供代码、练习和解决方案。这些意味着…

github.com](https://github.com/dennybritz/reinforcement-learning) [](https://github.com/MorvanZhou/Reinforcement-learning-with-tensorflow) [## MorvanZhou/张量流强化学习

### 在这些强化学习教程中，它涵盖了从基本的强化学习算法到高级算法开发…

github.com](https://github.com/MorvanZhou/Reinforcement-learning-with-tensorflow) 

## 如果你需要官方认证

有两个最大的 MOOCs 教授的课程:Coursera 和 Udacity。这些在申请工作和首轮筛选“相关经验”时会有所帮助不过，我绝对认为来自加州大学伯克利分校的最新材料会给你带来更令人兴奋的教育。

[](https://www.coursera.org/specializations/reinforcement-learning) [## 强化学习|课程

### 掌握强化学习的概念。实施完整的 RL 解决方案，并了解如何将人工智能工具应用于…

www.coursera.org](https://www.coursera.org/specializations/reinforcement-learning) [](https://www.udacity.com/course/reinforcement-learning--ud600) [## 强化学习

### 免费课程由佐治亚理工学院提供，编号为 CS 8803。关于本课程的免费课程，如果你需要，你应该选修本课程

www.udacity.com](https://www.udacity.com/course/reinforcement-learning--ud600) [](https://robotic.substack.com/) [## 自动化大众化

### 一个关于机器人和人工智能的博客，让它们对每个人都有益，以及即将到来的自动化浪潮…

robotic.substack.com](https://robotic.substack.com/) 

像这样？请在[https://robotic.substack.com/](https://robotic.substack.com/)订阅我关于机器人、自动化和人工智能的直接通讯。

*快去追！*