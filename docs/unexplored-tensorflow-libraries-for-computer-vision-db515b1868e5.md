# 用于计算机视觉的未开发的张量流库

> 原文：<https://towardsdatascience.com/unexplored-tensorflow-libraries-for-computer-vision-db515b1868e5?source=collection_archive---------47----------------------->

## 探索工具包和扩展，如 TensorFlow 模型优化器、图形、联合学习、隐私等，以提升您的计算机视觉工作流程。

![](img/2afc84fd21bc648dd7ab2f96abc37b78.png)

Emil Widlund 在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上的照片

TensorFlow 是一个端到端的开源机器学习平台，能够执行一系列任务。它为初学者和研究人员提供了易用性，并可用于不同的应用，例如但不限于计算机视觉、自然语言处理和强化学习。

在计算机视觉领域，我们大多数人都熟悉核心 TensorFlow 以及 TensorFlow Lite 和 JS。前者用于在移动和边缘设备上运行模型，后者用于在网络上运行模型。然而，TensorFlow 还提供了许多神秘的库，我们将在本文中一一介绍。

# 目录

*   张量流模型优化工具包
*   张量流图形
*   张量流联邦
*   TensorFlow 隐私
*   张量流集线器

# 张量流模型优化工具包

![](img/852b58cd1bf818e4fab3c18a03f64a57.png)

照片由[托德·夸肯布什](https://unsplash.com/@toddquackenbush?utm_source=medium&utm_medium=referral)在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 拍摄

实时模型对于许多商业运作来说是必不可少的。MobileNet 的推理速度让它成为众人瞩目的焦点，即使这意味着牺牲一点准确性。优化 TensorFlow 模型首先想到的是将其转换为 TensorFlow lite 服务。然而，这在台式机上工作得不是很好，因为它针对 ARM neon 进行了优化，这在本期[中有所解释](https://github.com/tensorflow/tensorflow/issues/35380)，或者我们需要进一步优化该模型。模型优化工具包可以帮助我们完成这些任务。根据其[主页](https://www.tensorflow.org/model_optimization)，它可用于:

> 降低云和边缘设备(如移动设备、物联网)的延迟和推理成本。
> 
> 将模型部署到对处理、内存、功耗、网络使用和模型存储空间有限制的边缘设备。
> 
> 支持在现有硬件或新的专用加速器上执行和优化。

它可以应用于已经训练好的模型以及训练期间，以进一步优化解决方案。在撰写本文时，它提供了三种技术，同时还提供了其他几种技术来完善模型。

## 修剪

第一种方法是权重剪枝。它通过去除层之间的一些连接来工作，因此减少了所涉及的参数和操作的数量，从而优化了模型。它消除了权重张量中不必要的值，并在训练过程中执行。这有助于减小模型的大小，通过训练后量化可以进一步减小模型的大小。

我不会深入每个函数的细节和代码，因为那会使文章太长。你可以参考[此处](https://blog.tensorflow.org/2019/05/tf-model-optimization-toolkit-pruning-API.html)进一步了解，参考[此处](https://www.tensorflow.org/model_optimization/guide/pruning/pruning_with_keras)了解其代码。

## 量化

与仅在训练期间进行的修剪不同，量化可以在训练和测试时进行。Tensorflow Lite 模型也被量化为使用 8 位整数，而不是通常使用的 32 位浮点。这提高了性能和效率，因为整数运算比浮点运算快得多。

然而，这是有代价的。量化是一种有损技术。这意味着先前从-3e38 到 3e38 表示的信息必须从-127 到 127 表示。在加法和乘法运算期间，8 位整数放大到 32 位整数，这需要再次缩小，从而引入更多误差。为了解决这个问题，可以在训练期间应用量化。

**量化感知训练**

通过在训练期间应用量化，我们迫使模型学习它将导致的差异，并相应地采取行动。量化误差作为噪声引入，优化器试图将其最小化。以这种方式训练的模型具有与浮点模型相当的准确性。看到以这种方式创建的 Tensorflow Lite 模型与普通模型的比较将会很有趣。

要阅读更多关于它的内容，请参考[这里的](https://blog.tensorflow.org/2020/04/quantization-aware-training-with-tensorflow-model-optimization-toolkit.html)，关于它的代码，你可以看看[这里的](https://www.tensorflow.org/model_optimization/guide/quantization/training_example#see_persistence_of_accuracy_from_tf_to_tflite)。

**岗位培训量化**

尽管在训练期间应用量化是优选的，但是有时这样做是不可行的，并且我们可能已经准备好使用预训练的权重。此外，它更容易实现。

更多信息可以在这里找到[，以及](https://blog.tensorflow.org/2019/06/tensorflow-integer-quantization.html)[代码](https://www.tensorflow.org/model_optimization/guide/quantization/post_training)。

## 权重聚类

它将相似的权重组合在一起，并用单个值替换它们。可以想象成 JPEG 压缩。此外，由于相似的权重被插值到相同的数字，所以它也是有损耗的。权重矩阵存储浮点值。这些值被转换成整数，并存储一个包含聚类数的查找表。这减少了所需的空间，因为整数需要更少的空间来存储，并且留下了有限数量的浮点数。

如下例所示，十六个 float-32 值被分配给四个 float-32 质心，图层权重被转换为整数值。权重矩阵越大，节省越多。

![](img/dfaa279fdd7c5f07abfef86756a74b14.png)

权重聚类

聚类被应用于完全训练的模型以找到质心。那么任何压缩工具都可以用来减小模型的大小。要了解它的细节，请参考这里的，以及它的[实现](https://www.tensorflow.org/model_optimization/guide/clustering/clustering_example)。

不同的技术可以结合起来进一步减少延迟，更多的方法也在他们的[路线图](https://www.tensorflow.org/model_optimization/guide/roadmap)中讨论。

# 张量流图形

![](img/e46b9626c50b95ee56652c6feecbf516.png)

照片由[转换器](https://unsplash.com/@convertkit?utm_source=medium&utm_medium=referral)在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄

TensorFlow graphics 旨在结合计算机视觉和计算机图形学来解决复杂的 3D 任务。计算机图形工作流程需要 3D 对象及其在场景中的绝对位置、对它们由灯光组成的材质的描述，以及生成合成渲染的相机。另一方面，计算机视觉工作流程将从图像开始，并尝试推断其参数。

这可以被认为是一个自动编码器，其中视觉系统(编码器)将试图找到参数，而图形系统(解码器)将基于这些参数生成图像，并可以与原始图像进行比较。此外，该系统不需要标记数据，并以自我监督的方式进行训练。张量流的一些用途是:

*   **变换** —可以对对象进行旋转、平移等对象变换。这可以被神经网络学习以精确地找到物体的位置。这对于需要精确估计这些物体的位置的机械臂是有用的。
*   **建模摄像机** —可以为摄像机设置不同的内在参数，从而改变图像的感知方式。例如，改变相机的焦距会改变物体的大小。
*   **材料** —可以使用不同类型的材料，这些材料具有不同类型的光反射能力。因此，创建的场景可以准确地模拟对象在真实世界中的行为。
*   **3D 卷积和池化(点云&网格)** —它具有 3D 卷积和池化层，允许我们对 3D 数据执行语义分类和分割。
*   **TensorBoard 3D** — 3D 数据变得越来越普遍，可用于解决 2D 数据的 3D 重建、点云分割、3D 对象变形等问题。通过 TensorBoard 3D，可以可视化这些结果，从而更好地了解模型。

延伸阅读— [此处](https://blog.tensorflow.org/2019/05/introducing-tensorflow-graphics_9.html)(其中还包含 Colab 笔记本的链接)

# 张量流联邦

![](img/c4bafa5d08a8a5e9cb13504d97e18f86.png)

照片由 [Ricardo Arce](https://unsplash.com/@jrarce?utm_source=medium&utm_medium=referral) 在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄

这个库也可以用于计算机视觉以外的其他领域。随着移动设备和边缘设备的增多，产生了大量的数据。联合学习旨在对分散的数据执行机器学习，即在设备本身上！这意味着没有必要将大量(敏感)数据上传到服务器。它已经被用于谷歌键盘。

下面附上的视频解释了关于联合学习的一切，从分散数据到使用 TensorFlow Federated。

有关使用 TensorFlow Federated 进行影像分类的指南，请参考下面链接的文章。

[](https://www.tensorflow.org/federated/tutorials/federated_learning_for_image_classification) [## 用于图像分类的联合学习|张量流联合

### 这个 Colab 已经被验证可以和 Note 一起工作:tensorflow_federated pip 包的最新发布版本…

www.tensorflow.org](https://www.tensorflow.org/federated/tutorials/federated_learning_for_image_classification) 

# TensorFlow 隐私

![](img/209ea19f0337070bcf3d86d95a74c962.png)

杰森·登特在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄的照片

通过隐私攻击可以从训练好的 ML 模型中提取敏感信息。Truex 等人提交了一份关于驱动因素的[论文](https://arxiv.org/pdf/1807.09173.pdf)。这些模型甚至可以重建它被训练的信息，如本文[所示](https://www.cs.cmu.edu/~mfredrik/papers/fjr2015ccs.pdf)。

![](img/c04976cd4378705acccf798328ab2ff1.png)

左侧是仅使用人名和模型重建的图像。右边的图像是原始图像。摘自[论文](https://www.cs.cmu.edu/~mfredrik/papers/fjr2015ccs.pdf)本身。

同样，像 TensorFlow Federated 一样，这不是计算机视觉独有的。最常用的技术是差分隐私。来自[维基百科](https://en.wikipedia.org/wiki/Differential_privacy):

> 差异隐私是一种公开共享数据集信息的系统，通过描述数据集中的群体模式，同时保留数据集中的个人信息。

假设敏感信息不会在整个数据集中完全重复，并且通过使用差分隐私模型，可以确保该模型不会获知此类信息。例如，假设有一个人与人之间聊天的数据集。现在，聊天上传递的敏感信息可以是密码、银行账户详情等。因此，如果在此数据集上创建模型，差分隐私将确保模型无法了解这些细节，因为它们的数量很少。阅读[这篇](http://www.cleverhans.io/privacy/2019/03/26/machine-learning-with-differential-privacy-in-tensorflow.html)关于差分隐私的优秀文章，它也包含了执行它的代码。

# 张量流集线器

![](img/e3e79a44804347fd9132b20ee80e625c.png)

丹尼尔·萨尔西乌斯在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄的照片

你们中的大多数人一定知道这个库，所以我将非常简短地介绍它。TensorFlow Hub 是一个在 TensorFlow 中发布、发现和重用部分机器学习模块的平台。称之为 TensorFlow 模型的 GitHub 也不会错。开发人员可以分享他们预先训练好的模型，这些模型可以被其他人重用。通过重用，开发人员可以使用较小的数据集训练模型，提高泛化能力，或者只是加快训练速度。让我们快速浏览一些不同的计算机视觉模型。

*   图像分类——从 MobileNet 到 Inception 再到 EfficientNet，有 100 多种模型可用于此任务。说出你想要的任何型号，你很可能会在那里找到它。
*   对象检测和分割-同样，您需要的任何模型都可以在这里找到，尤其是在 COCO 数据集上训练的 TensorFlow 模型动物园对象检测器集合。Deeplab 架构主导了图像分割场景。还有大量的 TfLite 和 TensorFlow Js 型号可供选择。
*   图像风格化——不同的图像风格化主干可以和漫画风格一起使用。
*   生成性对抗网络-GAN 模型如 Big-GAN 和 Compare-GAN 可用，在 ImageNet 和 Celeb 数据集上训练，可用于训练 ImageNet 和人工人脸的任何类别！还有一个 unboundary-GAN，可用于生成相机捕捉的场景之外的区域。此外，他们中的大多数人都有一个 Colab 笔记本，所以在如何实现他们方面没有什么大惊小怪的。

我刚刚描述了冰山一角。有更多的模型可用于姿势估计、特征匹配、超分辨率等主题。我甚至没有讨论过视频。跳到他们的[页面](https://tfhub.dev/)了解更多信息。你也可以在这里找到关于它的教程[。](https://www.tensorflow.org/hub/tutorials)

TensorFlow 提供了更多的库，如 [TensorFlow Extended](https://www.tensorflow.org/resources/libraries-extensions) ，一个用于部署 ML 管道的库， [Magenta](https://magenta.tensorflow.org/) ，一个用于生成音乐的库，等等。点击查看完整列表[。](https://www.tensorflow.org/resources/libraries-extensions)