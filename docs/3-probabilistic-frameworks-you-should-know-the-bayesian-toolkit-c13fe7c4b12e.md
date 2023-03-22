# 你应该知道的 3 个概率框架|贝叶斯工具包

> 原文：<https://towardsdatascience.com/3-probabilistic-frameworks-you-should-know-the-bayesian-toolkit-c13fe7c4b12e?source=collection_archive---------24----------------------->

## 用概率编程语言构建更好的数据科学工作流程，克服经典 ML 的缺点。

![](img/11c9cc6b12144c4e579e08e9ac104b54.png)

构建、训练和调整概率模型的工具。帕特里克·grądys 在 Unsplash 上拍摄的照片。

我们应该始终致力于创建更好的数据科学工作流。
但是为了实现这个目标，我们应该找出我们还缺少什么。

# 传统的 ML 工作流程缺少一些东西

经典的机器学习是流水线作业。通常的工作流程如下所示:

1.  有一个带有潜在假设的用例或研究问题，
2.  构建和管理与用例或研究问题相关的数据集，
3.  建立一个模型，
4.  训练和验证模型，
5.  甚至可能交叉验证，同时网格搜索超参数，
6.  测试拟合的模型，
7.  为用例部署模型，
8.  回答你提出的研究问题或假设。

您可能已经注意到，一个严重的缺点是要考虑模型的确定性和对输出的信心。

# 确定不确定

在经历了这个工作流程之后，假设模型结果看起来是合理的，我们认为输出是理所当然的。那么缺少什么呢？
**首先是**，我们有**没有考虑我们工作流程中出现的丢失或移位的数据**。
*你们中的一些人可能会插话说，他们对自己的数据有一些增强程序(例如图像预处理)。那很好——但是你把它正式化了吗？*
**其次是**，在看到数据之前**构建一个原型**怎么样——类似于建模健全性检查？**模拟**一些**数据**并在投入资源收集数据并拟合不充分的模型之前建立一个原型**。
Andrew gel man 在 2017 纽约 PyData 主题演讲[中已经指出了这一点。
**最后**，获得更好的**直觉和参数洞察**！对于深度学习模型，你需要依靠像](https://youtu.be/veiLCvcLIg8?t=2280) [SHAP](https://github.com/slundberg/shap) 和绘图库这样的老生常谈的工具来解释你的模型学到了什么。
对于概率方法，您可以快速了解参数。那么，我们希望在生产环境中使用什么工具呢？**

# 一.斯坦——统计学家的选择

STAN 是研究的一个完善的框架和[工具。严格地说，这个框架有自己的概率语言，Stan-code 看起来更像是你正在拟合的模型的统计公式。
一旦你用你的模型建立并完成了推理，你就可以把所有的东西保存到文件中，这带来了一个很大的好处，那就是所有的东西都是可复制的。
STAN 在 R 中通过](https://www.jstatsoft.org/article/view/v076i01) [RStan](https://mc-stan.org/users/interfaces/rstan) ，Python 带 [PyStan](https://pystan.readthedocs.io/en/latest/) ，以及其他[接口](https://mc-stan.org/users/interfaces/)得到很好的支持。
在后台，框架将模型编译成高效的 C++代码。
最后，通过 MCMC 推理(例如 NUTS sampler)来完成计算，该推理易于访问，甚至支持变分推理。
如果你想开始使用贝叶斯方法，我们推荐[案例研究](https://mc-stan.org/users/documentation/case-studies.html)。

# 二。pyro——编程方法

我个人最喜欢的深度概率模型工具是 [Pyro](https://pyro.ai/) 。这种语言由[优步工程部门](https://eng.uber.com/pyro/)开发和维护。该框架由 PyTorch 提供支持。这意味着您正在进行的建模与您可能已经完成的 PyTorch 工作无缝集成。
构建你的模型和训练例程，编写和感觉**像任何其他 Python 代码**一样，带有一些概率方法带来的特殊规则和公式。

作为概述，我们已经在以前的帖子中比较了 STAN 和 Pyro 建模的一个小问题集:

[](/single-parameter-models-pyro-vs-stan-e7e69b45d95c) [## 单参数模型| Pyro 与 STAN

### 用两种贝叶斯方法模拟美国癌症死亡率:STAN 的 MCMC 和 Pyro 的 SVI。

towardsdatascience.com](/single-parameter-models-pyro-vs-stan-e7e69b45d95c) 

当你想找到随机分布的参数，采样数据和执行有效的推断时，Pyro 表现出色。由于这种语言在不断发展，并不是你所做的每件事都会被记录下来。有很多用例以及已经存在的模型实现和[例子](https://pyro.ai/examples/)。此外，文档一天比一天好。
示例和[教程](https://pyro.ai/examples/)是一个很好的起点，尤其是当你是概率编程和统计建模领域的新手时。

# 三。张量流概率——谷歌的最爱

说起机器学习，尤其是深度学习，很多人想到的是 [TensorFlow](https://www.tensorflow.org/) 。因为 TensorFlow 是由 Google 开发者支持的，所以你可以肯定它得到了很好的维护，并且有很好的文档。
当您的工作流程中已经有 TensorFlow 或更好的 TF2 时，您就可以使用 TF Probability 了。
Josh Dillon 在 Tensorflow Dev Summit 2019 上做了一个很好的案例，说明为什么概率建模值得学习曲线，以及为什么您应该考虑 TensorFlow 概率:

张量流概率:信心学习(TF Dev Summit '19)张量流频道

这里有一个简短的笔记本，让你开始写张量流概率模型:

[](https://colab.research.google.com/github/tensorflow/probability/blob/master/tensorflow_probability/g3doc/_index.ipynb) [## 谷歌联合实验室

### 编辑描述

colab.research.google.com](https://colab.research.google.com/github/tensorflow/probability/blob/master/tensorflow_probability/g3doc/_index.ipynb) 

# 荣誉奖

PyMC3 是一个公开可用的 python 概率建模 API。它在研究中有广泛的应用，有强大的社区支持，你可以在 YouTube 上找到一些关于概率建模的演讲[来帮助你开始。](https://www.youtube.com/watch?v=TMmSESkhRtI&list=PL1Ma_1DBbE82OVW8Fz_6Ts1oOeyOAiovy)

如果你在给 Julia 编程，看看 [Gen](https://www.gen.dev/) 。这也是公开提供的，并且处于非常早期的阶段。因此仍然缺少文档，事情可能会出错。无论如何，这似乎是一个令人兴奋的框架。如果你愿意尝试，那么到目前为止的出版物和讲座都非常有前途。

# 参考

[1]保罗-克里斯蒂安·布克纳。 [**brms:使用 Stan 的贝叶斯多水平模型的 R 包**](https://www.jstatsoft.org/article/view/v080i01)
【2】b . Carpenter，A. Gelman 等人 [**STAN:一种概率编程语言**](https://www.jstatsoft.org/article/view/v076i01)
【3】e . Bingham，J. Chen 等人 [**Pyro:深度泛概率编程**](https://arxiv.org/abs/1810.09538)