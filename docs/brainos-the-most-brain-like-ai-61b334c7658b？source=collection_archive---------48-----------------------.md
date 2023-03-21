# BrainOS——最像大脑的人工智能

> 原文：<https://towardsdatascience.com/brainos-the-most-brain-like-ai-61b334c7658b?source=collection_archive---------48----------------------->

## 应用神经科学实现更高效、更智能的人工智能。

![](img/b53bddfb0794e9bb8926cf6f0caa69e8.png)

娜塔莎·康奈尔在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上的照片

我们的大脑是生物神经网络。通过大数据、大计算和机器学习算法，我们可以创建非常接近真实交易的人工神经网络。

例如，革命性的 [GPT-3 模型](https://arxiv.org/abs/2005.14165)可以写文章欺骗 88%的读者认为它是人类，它可以写代码、诗歌、歌词、模因，等等。

然而，我们仍然没有达到开启超级智能的“光开关时刻”——也看不到它。

![](img/532026718095f01e715a7e39267ddedf.png)

照片由[伊瓦洛·乔尔杰夫](https://www.freeimages.com/photographer/idesign-er-35962)拍摄，来自[自由影像](https://freeimages.com/)

# 神经科学可以提高人工智能

这是 BrainOS(一种新颖的类脑 AutoML 框架)所基于的前提。原因很简单:我们的大脑是已知最强大的处理器，因此我们至少应该测试我们的大脑(看起来)为 AI 的运行而运行的原理。

作为效率的题外话，让我们比较一下大脑和人工智能。毕竟，大脑是我们衡量人工智能准确性的基准，所以让我们粗略地看一下效率。

大脑每天消耗 [300 卡路里](https://www.nytimes.com/2008/09/02/science/02qna.html#:~:text=Studies%20show%20that%20it%20is,brain%20uses%20roughly%20300%20calories.)。在特斯拉 V100 上训练 GPT-3 需要花费 355 年(310 万小时)，特斯拉 V100 单独消耗 300 瓦。乘以瓦特/小时乘以训练小时总数得到 933 兆瓦。这等于 8020 亿卡路里。

NVIDIA [估计](https://www.forbes.com/sites/moorinsights/2019/05/09/google-cloud-doubles-down-on-nvidia-gpus-for-inference/#7dd810a46792)人工智能 80–90%的能源成本在于推理——在训练后使用模型。仅计算 GPU 成本，GPT-3 的成本高达 7.2 万亿卡路里。

哎哟。

BrainOS 目前还没有用于生产(我们稍后会提出建议)，但目前有一些提高效率的尝试。

首先，脉冲神经网络是一种类似大脑的提高效率的方式，但它们不能提供 BrainOS 将带来的与 AutoML 相同的好处。

[](https://medium.com/bitgrit-data-science-publication/spiking-neural-networks-a-more-brain-like-ai-6f7ad86b7e7e) [## 脉冲神经网络——一种更像大脑的人工智能

### 神经网络是用来识别模式的大脑的简化模型，但是它们浪费了大量的计算…

medium.com](https://medium.com/bitgrit-data-science-publication/spiking-neural-networks-a-more-brain-like-ai-6f7ad86b7e7e) 

虽然 BrainOS 仍然是一个研究项目，但有许多功能性的 AutoML 工具，如 [Apteo](http://apteo.co) 。有关 AutoML 的更多信息，请查看本指南:

[](/will-automl-be-the-end-of-data-scientists-9af3e63990e0) [## AutoML 会是数据科学家的末日吗？

### AutoML 越来越受欢迎。这就是事情的变化。

towardsdatascience.com](/will-automl-be-the-end-of-data-scientists-9af3e63990e0) 

# BrainOS 如何工作

BrainOS 通过以下属性自动选择合适的 ML 模型:

*   它给出的数据
*   先前的经验
*   世界知识

BrainOS 不仅仅与我们对大脑如何工作的想法有着松散的联系，它还试图对神经元行为进行建模。

> "该系统的结构和操作受到神经元细胞行为的启发."

## 体系结构

高层架构非常简单:输入数据(来自任何来源)与问题上下文和目标相结合。鉴于此，要么创建一个新模型，要么选择一个现有模型进行训练，然后部署该模型。

![](img/6543080becf62a334cae762bf2e302d6.png)

高级 BrainOS 架构。作者可视化。

## 详细组件

问题形式化是系统的切入点，包括上面的“输入数据”框。接下来，critic(或 qualifier)组件通过添加早日期数据集来增强输入数据，并通过应用资格来实现中间数据。

受大脑自适应学习特性的启发，历史数据库结合了历史(或遇到的数据集的经验)和世界知识(或存储的知识以及抽象研究)。

规划器组件简单地规划算法的执行顺序，或者系统的流程。并行执行器是任务调度器，它决定如何高效地执行线程。

模块调度器接收由上述并行执行器发送的线程，计划执行时间表。

选择器是 BrainOS 的关键组件，它通过并行执行许多步骤来挑选出正确的模型:搜索 BrainOS 的历史，搜索研究数据集，从头开始构建工具，并通过组合几个模型来执行集成学习。然后选择最适合的模型。

# 深度认知神经网络(DCNN) —实现 BrainOS

DCNNs 是在现实世界中实现 BrainOS 的一种方案。

与典型的神经网络不同，DCNNs 表现出感知和推理能力，并能够在小型设备(如智能手机)上进行近实时的大数据分析。此外，它们的能效极高，比类似的深度神经网络高出 300 倍。

市场上还没有任何基于 DCNN 的 AutoML，但鉴于传统深度网络对计算、数据和能源的极高要求，我相信我们很快就会看到它们。