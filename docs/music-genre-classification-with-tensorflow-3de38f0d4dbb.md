# 基于张量流的音乐流派分类

> 原文：<https://towardsdatascience.com/music-genre-classification-with-tensorflow-3de38f0d4dbb?source=collection_archive---------18----------------------->

## 使用 TensorFlow 比较标准机器学习与深度学习，以找到音频样本的流派

音乐流媒体服务的兴起让音乐无处不在。我们在上下班途中听音乐，在我们锻炼、工作或者只是为了放松的时候。我们日常生活中的持续干扰并没有削弱音乐在引发情感和处理我们思想中的作用，正如“变焦音乐会”的出现所证明的那样。

这些服务的一个关键特征是播放列表，通常按流派分组。这些数据可能来自发布歌曲的人的人工标记。但这种方法的规模并不大，可能会被那些想利用某一特定流派的受欢迎程度的艺术家所利用。更好的选择是依靠[自动音乐流派分类](https://artists.spotify.com/blog/how-spotify-discovers-the-genres-of-tomorrow)。与我的两位合作者 [Wilson Cheung](http://wilsoncheung.me/) 和 [Joy Gu](https://github.com/joygu1370) ，我们试图比较将音乐样本分类成流派的不同方法。特别是，我们评估了标准机器学习与深度学习方法的性能。我们发现特征工程是至关重要的，领域知识确实可以提高性能。

在描述了所使用的数据源之后，我简要概述了我们使用的方法及其结果。在本文的最后一部分，我将花更多的时间解释 Google Colab 中的 TensorFlow 框架如何通过 TFRecord 格式在 GPU 或 TPU 运行时有效地执行这些任务。所有的代码都在这里[可用](https://github.com/celestinhermez/music-genre-classification)，我们很高兴与任何感兴趣的人分享我们更详细的报告。

# 数据源

预测音频样本的流派是一个监督学习问题(对于监督与非监督学习的好入门，我推荐 [Devin 关于主题](/supervised-vs-unsupervised-learning-14f68e32ea8d)的文章)。换句话说，我们需要包含标记示例的数据。 [FreeMusicArchive](https://github.com/mdeff/fma%7DFreeMusicArchive) 是一个带有相关标签和元数据的音频片段库，最初是为 2017 年国际音乐信息检索大会(ISMIR)上的 [a 论文](https://arxiv.org/abs/1612.01840)收集的。

我们将分析的重点放在所提供的一小部分数据上。它包含 8，000 个音频片段，每个长度为 30 秒，分为八种不同的类型:

*   嘻哈音乐
*   流行音乐
*   民间的
*   实验的
*   岩石
*   国际的
*   电子的
*   仪器的

每个流派都有 1000 个代表性的音频片段。由于采样率为 44，100 Hz，这意味着每个音频样本有超过 100 万个数据点，或超过 10⁹数据点总数。在分类器中使用所有这些数据是一个挑战，我们将在接下来的章节中对此进行更多的讨论。

有关如何下载数据的说明，请参考存储库中包含的自述文件。我们非常感谢[Michal Defferrard](https://deff.ch/)、 [Kirell Benzi](https://kirellbenzi.com/) 、 [Pierre Vandergheynst](https://people.epfl.ch/pierre.vandergheynst) 、 [Xavier Bresson](https://www.ntu.edu.sg/home/xbresson) 将这些数据放在一起并免费提供，但我们只能想象 Spotify 或 Pandora Radio 拥有的数据规模所能提供的见解。有了这些数据，我们可以描述各种模型来执行手头的任务。

# 模型描述

我将尽量减少理论上的细节，但会尽可能地链接相关资源。此外，我们的报告包含了比我在这里所能包括的更多的信息，特别是关于特性工程，所以如果你想让我与你分享，请在评论中告诉我。

## 标准机器学习

我们使用了逻辑回归、k-最近邻(kNN)、高斯朴素贝叶斯和支持向量机(SVM):

*   [SVM](/support-vector-machine-introduction-to-machine-learning-algorithms-934a444fca47) 试图通过最大化训练数据的余量来找到最佳决策边界。内核技巧通过将数据投影到高维空间来定义非线性边界
*   kNN 基于 k 个最接近的训练样本的多数投票来分配标签
*   [天真的巴爷](/naive-bayes-classifier-81d512f50a7c) s 根据特征预测不同类别的概率。条件独立性假设大大简化了计算
*   [逻辑回归](/logistic-regression-detailed-overview-46c4da4303bc)也通过利用逻辑函数直接模拟概率来预测不同类别的概率

## 深度学习

对于深度学习，我们利用 TensorFlow 框架(详见本文第二部分)。我们根据输入的类型建立了不同的模型。

对于原始音频，每个示例都是 30 秒的音频样本，即大约 130 万个数据点。这些浮点值(正的或负的)表示在某个时间点的波浪位移。为了管理计算资源，可以使用不到 1%的数据。有了这些特征和相关的标签(一位热编码)，我们可以建立一个卷积神经网络。总体架构如下:

*   [1 维卷积层](https://www.tensorflow.org/api_docs/python/tf/keras/layers/Conv1D)，其中的过滤器组合来自偶然数据的信息
*   [最大池层](https://www.tensorflow.org/api_docs/python/tf/keras/layers/MaxPool1D)，结合来自卷积层的信息
*   [密集层](https://www.tensorflow.org/api_docs/python/tf/keras/layers/Dense)，完全连接以创建提取的卷积特征的线性组合，并执行最终分类
*   [删除层](https://www.tensorflow.org/api_docs/python/tf/keras/layers/Dropout)，帮助模型归纳出看不见的数据

![](img/5ee67a3dbfc689c125cd59b9f0f1f349.png)

原始音频示例

另一方面，频谱图充当音频样本的视觉表示。这启发了将训练数据视为图像，并通过[迁移学习](/a-comprehensive-hands-on-guide-to-transfer-learning-with-real-world-applications-in-deep-learning-212bf3b2f27a)利用预训练模型。对于每个例子，我们可以形成 Mel 谱图，它是一个矩阵。如果我们计算的维度正确，这个矩阵可以表示为 224x224x3 的图像。这些都是利用 [MobileNetV2](https://arxiv.org/abs/1801.04381%7DMobileNetV2) 的正确维度，MobileNetV2 在图像分类任务上有着出色的表现。迁移学习的思想是使用预训练模型的基础层来提取特征，并用定制的分类器(在我们的例子中是密集层)替换最后一层。这是可行的，因为基础层通常可以很好地推广到所有图像，甚至是他们没有训练过的图像。

![](img/4b3aa0dbc684d63303295c12d1ff87af.png)

Mel 光谱图示例

# 模型结果

我们使用 20%的测试集来评估模型的性能。我们可以将结果总结在下表中:

![](img/e61792baac0cd8863085175c2be4c1b0.png)

测试集上的模型结果

将迁移学习应用于频谱图的卷积神经网络是最好的表现者，尽管 SVM 和高斯朴素贝叶斯在性能上是相似的(考虑到后者的简化假设，这本身是有趣的)。我们在报告中描述了最好的超参数和模型架构，但是我不会在这里包括它们。

我们对训练和验证曲线的分析突出了[过度拟合](/what-are-overfitting-and-underfitting-in-machine-learning-a96b30864690)的问题，如下图所示(我们的大多数模型都有类似的图表)。呈现的特征模式有助于我们识别这个问题。我们为此设计了一些解决方案，可以在这个项目的未来迭代中实现:

*   减少数据的维度:可以使用 PCA 等技术将提取的特征组合在一起，并限制每个示例的特征向量的大小
*   增加训练数据的大小:数据源提供了更大的数据子集。我们将探索限制在整个数据集的 10%以下。如果有更多的计算资源可用，或者数据的维数被成功降低，我们可以考虑使用完整的数据集。这很可能使我们的方法能够隔离更多的模式，并极大地提高性能
*   在寻找功能时要更加小心:FreeMusicArchive 包含了一系列功能。当使用这些特性而不是我们自己的特性时，我们确实看到了性能的提高，这使我们相信我们可以希望通过领域知识和增强的特性集获得更好的结果

![](img/3487b247e379711c1b32888eeb7b06bc.png)

使用原始音频输入构建的深度学习模型的训练(橙色)和验证(蓝色)准确性

# TensorFlow 实现

TensorFlow 是一个非常强大的大规模构建神经网络的工具，尤其是在与 Google Colab 的免费 GPU/TPU 运行时结合使用时。这个项目的主要启示是识别瓶颈:我最初的实现非常慢，即使使用 GPU 也是如此。我发现问题在于 I/O 过程(来自驱动器，速度非常慢),而不是训练过程。使用 TFrecord 格式可以加快并行化的速度，这使得模型训练和开发更快。

在开始之前，有一点很重要:虽然数据集中的所有歌曲都是 MP3 格式，但我将它们转换为 wav 文件，因为 TensorFlow 对它们有更好的内置支持。请参考 GitHub 上的[资源库，查看与这个项目相关的所有代码。代码还假设你有一个](https://github.com/celestinhermez/music-genre-classification)[谷歌云存储桶](https://cloud.google.com/storage/docs/creating-buckets)，里面有所有的 wav 文件，一个上传了元数据的谷歌硬盘，并且你正在使用[谷歌 Colab](https://colab.research.google.com/notebooks/intro.ipynb#recent=true) 。尽管如此，让所有代码适应另一个系统(基于云的或本地的)应该相对简单。

谷歌的这个 [codelab](https://codelabs.developers.google.com/codelabs/keras-flowers-data/#4) 以及 TensorFlow 官方文档对这个项目非常有帮助，其中一些代码仅仅是从这些来源改编而来。

## 初始设置

这个项目需要大量的库。存储库中的`requirements.txt`文件为您处理安装，但是您也可以在下面找到详细的列表。

必要的库

第一步是挂载驱动器(元数据已经上传到这里)，并使用存储音频文件的 GCS bucket 进行身份验证。从技术上讲，元数据也可以上传到 GCS，这样就不需要安装驱动器了，但这就是我自己的项目的结构。你可以在这里找到更多关于如何导入文件到 Colab [的细节。](https://colab.sandbox.google.com/notebooks/io.ipynb#scrollTo=S7c8WYyQdh5i)

安装驱动器并使用 GCS 进行身份验证:[https://colab . sandbox . Google . com/notebooks/io . ipynb # scroll to = S7 c8 wyyqdh 5 I](https://colab.sandbox.google.com/notebooks/io.ipynb#scrollTo=S7c8WYyQdh5i)

我们还存储了一些变量以备将来使用，比如数据大小

## 创建张量流数据集

下一组函数读入必要的元数据信息。我没有写这段代码，只是改编自[免费音乐档案](https://github.com/mdeff/fma)。这一部分很可能会在您自己的项目中发生变化，这取决于您正在使用的数据集。

然后我们需要函数来创建一个[张量流数据集](https://www.tensorflow.org/api_docs/python/tf/data/Dataset)。这个想法是在文件名列表上循环，在一个[管道中应用一系列操作，返回一个 BatchedDataset](https://www.tensorflow.org/guide/data) ，包含一个特征张量和一个标签张量。我们使用 TensorFlow 内置函数和 Python 函数(使用 [tf.py_function](https://www.tensorflow.org/api_docs/python/tf/py_function) ，在数据管道中使用 Python 函数非常有用)。在这里，我只包括从原始音频数据创建数据集的函数，但是这个过程非常类似于以声谱图作为特征来创建数据集(更多细节参见[库](https://github.com/celestinhermez/music-genre-classification))。您将需要在自己的项目中对这条管道进行轻微的编辑，例如改变数据的形状，但是整体结构将保持不变。

创建张量流数据集的管道

## 在 GCS 上使用 TFRecord 格式

现在我们有了数据集，我们使用 [TFRecord 格式](https://www.tensorflow.org/tutorials/load_data/tfrecord)将它存储在 GCS 上。这是 GPU 和 TPU、[使用的推荐格式，因为并行化实现了快速 I/O](/how-to-build-efficient-audio-data-pipelines-with-tensorflow-2-0-b3133474c3c1)。主要的想法是一本 tf 词典。[特性](https://www.tensorflow.org/api_docs/python/tf/train/Feature)，存储在 [tf 中。例](https://www.tensorflow.org/tutorials/load_data/tfrecord)。我们将数据集写入这些示例，存储在 GCS 上。除了更改要素类型的潜在例外情况，这部分代码应该只需要对其他项目进行最少的编辑。如果数据已经上传到记录格式一次，这一部分可以跳过。本节大部分代码改编自 [TensorFlow 官方文档](https://www.tensorflow.org/tutorials/load_data/tfrecord)以及本[音频管道教程](/how-to-build-efficient-audio-data-pipelines-with-tensorflow-2-0-b3133474c3c1)。

写入 TFRecord 格式

一旦这些记录被存储，我们需要其他函数来读取它们。每个例子都被依次处理，从 TFRecord 中提取相关信息并重建一个 tf.Dataset。数据集→作为 tfRecord 上传到 GCS 将 TFRecord 读入 TF。数据集)，但这实际上通过简化 I/O 过程提供了巨大的速度效率。如果 I/O 是瓶颈，使用 GPU 或 TPU 没有帮助，这种方法允许我们通过优化数据加载来充分利用他们在训练期间的速度增益。如果使用另一个数据集，主要变化将围绕数据维度。

将 TFRecord 数据读入数据集

## 准备培训、验证和测试集

重要的是将数据适当地分成训练-验证-测试集(64%-16%-20%)，前两个用于调整模型架构，后一个用于评估模型性能。拆分发生在文件名级别，读取 TFRecord 示例的管道应用于适当的名称集。

创建培训、验证和测试集

## 模型构建和培训

最后，我们可以使用 Keras API 来构建和测试模型。网上有大量关于如何使用 Keras 构建模型的信息，所以我不会深入研究细节，但这里重要的是使用 [1D 卷积层](/understanding-1d-and-3d-convolution-neural-network-keras-9d8f76e29610)结合[池层](https://www.tensorflow.org/api_docs/python/tf/keras/layers/MaxPool1D)从原始音频中提取特征。

![](img/37e47f778bf8e9fcf38d766fa02232da.png)

插图 1D 卷积和池层。杨、黄、、王、向、李、薛琼。(2019).一种基于深度学习的盲频谱感知方法。传感器。19.2270.10.3390/s19102270。

训练和测试模型

最后一点相关信息是关于使用 TensorBoard 绘制训练和验证曲线。这些强调了过度拟合的问题，并为结论中指出的未来工作开辟了领域。它也可以用于模型开发期间的[迭代](https://www.tensorflow.org/tensorboard/scalars_and_keras)。

显示此训练过程的张量板

总之，对同一机器学习任务的不同机器学习方法进行基准测试是有启发性的。这个项目强调了领域知识和特征工程的重要性，以及标准、相对简单的机器学习技术(如朴素贝叶斯)的力量。过度拟合是一个问题，因为与示例数量相比，特征的尺寸很大，但我相信未来的努力可以帮助缓解这个问题。

我很高兴地惊讶于迁移学习在 spectrograms 上的强劲表现，并认为我们可以通过使用音乐理论提供的更多功能来做得更好。然而，如果有更多的数据来提取模式，对原始音频的深度学习技术确实显示出了前景。我们可以设想一种应用，其中分类可以直接在音频样本上发生，而不需要特征工程。我期待着看到您在这篇文章中介绍的工作的基础上所做的一切！