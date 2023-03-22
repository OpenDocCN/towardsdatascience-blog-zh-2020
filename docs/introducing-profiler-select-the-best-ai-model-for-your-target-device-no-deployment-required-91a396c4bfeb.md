# 简介 Profiler，作者:Auptimizer:为您的目标设备选择最佳的人工智能模型——无需部署

> 原文：<https://towardsdatascience.com/introducing-profiler-select-the-best-ai-model-for-your-target-device-no-deployment-required-91a396c4bfeb?source=collection_archive---------54----------------------->

> [*剖析器*](https://github.com/LGE-ARC-AdvancedAI/auptimizer/tree/master/src/aup/profiler) *是一个用于剖析机器学习(ML)模型脚本性能的模拟器。探查器可以在开发管道的训练和推断阶段使用。它对于评估部署到边缘设备的模型和脚本的脚本性能和资源需求特别有用。Profiler 是*[*au optimizer*](https://github.com/LGE-ARC-AdvancedAI/auptimizer)*的一部分。您可以从*[*auoptimizer GitHub 页面*](https://github.com/LGE-ARC-AdvancedAI/auptimizer) *获取 Profiler，或者通过 pip 安装 auoptimizer。*

## 我们为什么要建立 Profiler

在过去几年里，云中训练机器学习模型的成本大幅下降。虽然这种下降将模型开发推到了云上，但是仍然有重要的原因来训练、适应和部署模型到设备上。性能和安全性是两大因素，但成本节约也是一个重要的考虑因素，因为传输和存储数据的成本以及为数百万台设备构建模型的成本往往会增加。毫不奇怪，尽管云计算变得更加便宜，但针对边缘设备或边缘人工智能的机器学习继续成为主流。

为边缘开发模型为从业者带来了有趣的问题。

1.  型号选择现在包括考虑这些型号的资源需求。
2.  由于在回路中有一个设备，训练-测试周期变得更长，因为现在需要在设备上部署模型来测试其性能。只有当有多个目标设备时，这个问题才会被放大。

目前，有三种方法可以缩短型号选择/部署周期:

*   使用在开发机器上运行的特定于设备的模拟器，不需要部署到设备上。*警告:*模拟器通常不能跨设备通用。
*   使用目标设备自带的分析器。*警告:*他们需要将模型部署到目标设备上进行测量。
*   使用 FLOPS 或乘加(MAC)运算等方法来给出资源使用的近似度量。*警告:*模型本身只是整个管道(还包括数据加载、增强、特征工程等)的一部分(有时是无关紧要的)。)

在实践中，如果您想要选择一个能够在目标设备上高效运行的模型，但是无法访问专用的模拟器，那么您必须通过在所有目标设备上部署来测试每个模型。

*Profiler 有助于缓解这些问题。* Profiler 允许您在开发机器上模拟您的训练或推理脚本将如何在目标设备上执行。使用 Profiler，您可以了解 CPU 和内存的使用情况，以及目标设备上模型脚本的运行时间。

## 探查器的工作原理

Profiler 将模型脚本、它的需求和相应的数据封装到 Docker 容器中。它使用用户输入的计算、内存和框架约束来构建相应的 Docker 映像，因此脚本可以独立运行，不依赖外部。然后，可以轻松地对该映像进行缩放和移植，以简化未来的开发和部署。当模型脚本在容器中执行时，分析器跟踪并记录各种资源利用率统计信息，包括*平均 CPU 利用率*、*内存利用率*、*网络 I/O* 和*块 I/O* 。记录器还支持设置采样时间，以控制 Profiler 从 Docker 容器中采样利用率统计信息的频率。

> 获取分析器:[单击此处](https://github.com/LGE-ARC-AdvancedAI/auptimizer/tree/master/src/aup/profiler)

## 探查器如何帮助

我们的结果表明，Profiler 可以帮助用户为许多流行的图像/视频识别模型建立良好的模型运行时间和内存使用估计。我们在三种不同的设备上进行了 300 多次实验，这些设备包括各种型号(InceptionV3、SqueezeNet、Resnet18、mobilenet v2–0.25 x、-0.5x、-0.75x、-1.0x、3D-SqueezeNet、3D-shuffle net v2–0.25 x、-1.0x、-1.5x、-2.0x、3D-mobilenet v2–0.25 x、-0.5x、-0.75x、-1.0x、-2.0x)您可以在这里找到全套实验结果以及如何在您的设备上进行类似实验的更多信息[。](https://github.com/LGE-ARC-AdvancedAI/auptimizer/tree/master/Examples/profiler_examples/experiments)

Profiler 的加入使 Auptimizer 更接近一种工具的愿景，这种工具可以帮助机器学习科学家和工程师为边缘设备建立模型。au optimizer 的[超参数优化(HPO)功能有助于加快模型发现。Profiler 有助于选择正确的部署模型。它在以下两种情况下特别有用:](https://github.com/LGE-ARC-AdvancedAI/auptimizer)

1.  在模型之间做出决定——使用开发机器上的 Profiler 测量的模型脚本的运行时间和内存使用情况的排名表明了它们在目标设备上的排名。例如，如果在开发机器上使用 Profiler 测量时，Model1 比 Model2 快，那么在设备上 Model1 将比 Model2 快。只有当 CPU 满负荷运行时，这个排名才有效。
2.  预测设备上的模型脚本性能—一个简单的线性关系将使用开发机器上的探查器测量的运行时间和内存使用情况与使用目标设备上的本机分析工具测量的使用情况联系起来。换句话说，如果一个模型在使用 Profiler 测量时运行时间为 x，那么它在目标设备上的运行时间大约为(a*x+b)(其中 a 和 b 可以通过使用本机分析工具分析设备上的几个模型来发现)。这种关系的强度取决于模型之间的架构相似性，但是，通常，为相同任务设计的模型在架构上是相似的，因为它们由相同的一组层组成。这使得 Profiler 成为选择最适合的模型的有用工具。

## 展望未来

分析器在不断发展。到目前为止，我们已经在选定的移动和边缘平台上测试了它的功效，用于运行流行的图像和视频识别模型进行推理，但还有更多需要探索。探查器可能对某些型号或设备有限制，可能会导致探查器输出和设备上的测量值不一致。我们的[实验页面](https://github.com/LGE-ARC-AdvancedAI/auptimizer/tree/master/Examples/profiler_examples/experiments)提供了更多关于如何使用 Profiler 设置您的实验以及如何解释结果中潜在的不一致的信息。确切的用例因用户而异，但我们相信 Profiler 适用于任何在设备上部署模型的人。我们希望 Profiler 的评估功能能够为资源受限的设备提供更精简、更快速的模型开发。我们很乐意听到(通过 github)您是否在部署期间使用 Profiler。

**作者:** Samarth Tripathi、Junyao Guo、Vera Serdiukova、Unmesh Kurup 和 Mohak Shah——LG 电子美国公司高级人工智能