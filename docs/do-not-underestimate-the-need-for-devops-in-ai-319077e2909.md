# 不要低估人工智能对 DevOps 的需求。进入深度学习 DevOps — DL 基础设施工程。

> 原文：<https://towardsdatascience.com/do-not-underestimate-the-need-for-devops-in-ai-319077e2909?source=collection_archive---------20----------------------->

![](img/763a8a66de034cf81a8dd1f61d887310.png)

[https://unsplash.com/photos/Zeu57mprpaI](https://unsplash.com/photos/Zeu57mprpaI)

# 你为什么要在乎？

随着机器学习越来越成熟，构建支持运行这些工作流的基础设施的需求就更大了。在一个大型企业中，平均至少有 200 多名数据科学家/DL/ ML 工程师在进行他们的模型训练和推理工作。确保这些用户能够方便地使用硬件/软件来训练他们的模型是非常必要的。这听起来像是一个简单的任务，我在这里告诉你它不是。

# **有多重挑战。例如:**

*   对于数据科学家来说，运行作业的抽象和硬件访问的简易性非常重要。
*   不同的用户有不同的硬件和软件要求。一些用户使用 Tensorflow 训练他们的模型，而一些用户使用 Pytorch，其他用户使用他们自己内部构建的框架。
*   一个团队使用大规模 **BERT 甚至** [**图灵-NLG**](https://www.microsoft.com/en-us/research/blog/turing-nlg-a-17-billion-parameter-language-model-by-microsoft/) 据我们所知，分别使用 16 个或 256 个 V100s 来训练他们，而另一个团队使用预先训练好的 **EfficientNet** 和两个 T4 GPU。所有这些都是假设，但你得到了要点。不同的团队需要不同数量的 GPU、FPGAs、CPU、TPU 甚至[IPU](https://www.graphcore.ai/products/ipu)。
*   如何管理和维护这些资源？
*   谁实际上保留了对这些硬件资源的控制权？是数据科学家还是 DevOps 团队？
*   谁设置作业的优先级(作业是您想要在硬件上运行的任何东西，例如，培训、推理等。)?
*   如何保持资源分配的合理性？我们都知道每个人都希望他们的模型先被训练。
*   如何支持不知道充分利用分配给他们的资源的数据科学家——例如，他们跨 32 个 GPU 的 GPU 利用率低于 25%？
*   如何处理安全问题—例如，确保只有需要的用户才能访问？
*   如何确保所有加速器和节点都得到有效利用，同时又不会显著降低性能？
*   谁来帮助数据科学家分析他们缓慢的应用程序？这有时是一个问题，因为数据科学家不一定建立最有效的模型训练管道。换句话说，数据科学家不是软件工程师。
*   一些作业是一个依赖链:也许他们使用并行服务器-工作器架构，其中一些工作器必须在其他工作器之前被启动，如何处理它们来维护队列？
*   谁安装和维护新工具-例如， [MLFlow](https://github.com/mlflow/mlflow) ， [KubeFlow](https://www.kubeflow.org/) ， [Polyaxon](https://github.com/polyaxon/polyaxon) ， [Seldon](https://docs.seldon.io/en/latest/) ， [Pachyderm](https://www.pachyderm.com/platform/) ， [Domino Data Lab](https://www.dominodatalab.com/product/) ， [Argo](https://argoproj.github.io/argo/) 等。最快下个月就会到来。
*   如果这些问题还不足以引起麻烦，考虑安装和维护硬件级驱动程序，例如 CUDA 驱动程序、新的软件包等。
*   数据科学家遇到的一些错误是 ML 库特有的，例如， [NCCL](https://developer.nvidia.com/nccl) 环形拓扑问题。典型的数据科学家可能不会接触到这些问题。
*   处理部署和支持推理作业是另一个需要自己的基础设施团队的任务。

这篇文章没有完全提到将模型从开发转移到推理的问题。正如你所想象的，这本身就是一个问题，这些问题是大局的一部分。我们暂时把它放在一边。

一个典型的 DevOps 工程师不一定具有支持一些特定于 ML 库的问题的专业知识。另一方面，数据科学家本身并不是管理大规模集群的专家，将大型集群交给数据科学家也不是一个好主意。那么以上是谁做的工作呢？此时，你可能会想，“嗯…这听起来很像一个系统管理员的角色”，在某种程度上，的确如此！然而，由于这需要了解 ML 概念，因此需要有人是 SysAdmin+ML Engineer = Enter**ML/DL 基础架构工程师**并且随着大型本地集群一起添加的通常是 [**HPC**](/why-distributed-deep-learning-is-going-to-gain-paramount-importance-bd2d83517483) 。

# 结论

DL 基础设施工程师负责管理和维护集群。当您从云迁移到本地时，情况更是如此。在未来(不完全是，我们已经看到它正在发生)，我们将能够看到一个基础设施分支，它迎合了解 ML 和 DevOps 概念的数据科学家。这样，让科学家做他们的科学，让基础设施工程师做他们的 DL 基础设施；)