# 在 Java 中从胸部 x 射线图像中检测肺炎

> 原文：<https://towardsdatascience.com/detecting-pneumonia-from-chest-x-ray-images-e02bcf705dd6?source=collection_archive---------52----------------------->

## 基于 Keras 和 Deep Java 库的图像分类

![](img/a3f4a82aa04404d35d24209b4bcb02da.png)

照片来自 [Unsplash](https://unsplash.com/photos/NFvdKIhxYlU) 上的[国家癌症研究所](https://unsplash.com/@nci)

*免责声明:这篇博文仅用于教育目的。该应用程序是使用实验代码开发的。该结果不应用于肺炎的任何医学诊断。此内容尚未经过任何科学家或医学专业人士的审核或批准。*

# 介绍

在这篇博客文章中，我们展示了深度学习(DL)如何用于从胸部 x 光图像中检测肺炎。这项工作的灵感来自 Kaggle 上的[胸部 x 光图像挑战](https://www.kaggle.com/paultimothymooney/chest-xray-pneumonia)和[一篇相关论文](https://www.cell.com/cell/fulltext/S0092-8674(18)30154-5)。在这篇文章中，我们通过关注企业部署来说明人工智能如何帮助临床决策。这项工作利用了使用 Keras 和 TensorFlow [与 Kaggle 内核](https://www.kaggle.com/aakashnain/beating-everything-with-depthwise-convolution)训练的模型。在这篇博文中，我们将重点关注使用 [Deep Java Library](https://djl.ai/) (DJL)，一个用 Java 构建和部署 DL 的开源库，用这个模型生成预测。

# 软件设置

我们选择了 Keras 和 [DJL](https://djl.ai/) ，两个用户友好的 DL 工具来实现我们的图像分类器。Keras 是一个易于使用的高级深度学习 API，支持快速原型制作。与此同时，DJL 为 Keras 用户提供了简单的 API 来在 Java 中部署 DL 模型。现在，让我们深入教程。

# 使用 Keras 训练并保存您的模型

第一步是训练图像分类模型。你可以按照[这个内核](https://www.kaggle.com/aakashnain/beating-everything-with-depthwise-convolution)中引用的指令来获得一步一步的指导。该模型试图通过目视检查胸部 x 光图像中的特征来识别肺炎。作为一个参考点，下面的图像比较了三个具有正常肺(左)、细菌性肺炎(中)和病毒性肺炎(右)的候选人之间的差异。[1]

![](img/7003516da60a8b2f9cd40fc487141b90.png)

*图 1 据* [*论文图 S6*](https://www.cell.com/cell/fulltext/S0092-8674(18)30154-5)*，正常胸片(左)示双肺清晰，细菌性肺炎(中)示局灶性肺叶实变，病毒性肺炎(右)示双肺更弥漫性“间质”样改变。*

训练过程包括 3 个步骤:准备数据、构建模型和训练模型。您可以使用此[链接](https://www.kaggle.com/paultimothymooney/chest-xray-pneumonia/download)下载用于训练该模型的数据集。该模型由深度方向可分离的卷积层组成，具有 ImageNet 上的部分预训练权重。深度方向可分离的卷积层具有更少的参数，并且比可比较的 DL 模型更有效。我们还使用了[迁移学习](https://cs231n.github.io/transfer-learning/)，这是一种流行的 DL 技术，它将针对一个问题训练的模型应用于另一个相关的问题。迁移学习利用在类似问题上已经学到的特征，而不是从零开始开发模型，并且快速产生更健壮的模型。对于我们模型中的前 2 层，我们使用了一个 [VGG 网络](https://arxiv.org/abs/1409.1556)的权重，该网络是在 [ImageNet](http://www.image-net.org/) 上预先训练的，这是一个大得多的数据集。

你可以直接下载内核笔记本，在本地运行来生成模型。注意，我们需要将模型保存为 TensorFlow 保存的模型格式。你可以在笔记本的最后加上下面一行。有关在 DJL 使用 Keras 模型的更多信息，请参见[如何在 DJL 导入 Keras 模型](https://github.com/awslabs/djl/blob/master/docs/tensorflow/how_to_import_keras_models_in_DJL.md)。

```
model.save("best_model")
```

如果你想用预先训练好的模型直接运行预测，从下载这个[模型](https://djl-tensorflow-javacpp.s3.amazonaws.com/tensorflow-models/chest_x_ray/saved_model.zip)开始。

# 使用深度 Java 库加载和运行预测

一旦有了训练好的模型，就可以使用 DJL 生成预测。完整代码见[肺炎检测](https://github.com/aws-samples/djl-demo/tree/master/pneumonia-detection)。您可以使用以下命令从命令行运行预测。使用`-Dai.djl.repository.zoo.location`指定模型的位置。

以下是输出示例:

下面几节将带您详细了解代码。

# 导入 DJL 库和 TensorFlow 引擎

要在 Keras 模型上运行预测，您需要 DJL 高级 API 库和底层 TensorFlow 引擎。它们可以使用 Gradle 或 Maven 导入。更多详情，请参见[肺炎检测自述文件](https://github.com/aws-samples/djl-demo/tree/master/pneumonia-detection)。以下示例使用 Gradle 来设置依赖关系:

# 负荷模型和运行预测

接下来，我们需要加载我们训练好的模型。DJL 提供了简单易用的 API 来加载模型。你可以使用我们的[模型动物园](https://github.com/awslabs/djl/blob/master/model-zoo/README.md)来加载模型，或者从你的本地驱动器使用你自己的模型。以下示例使用本地模型动物园来加载模型并运行预测。这只用几行代码就可以完成。

在这段代码中，我们首先使用一个 Criteria builder 来告诉 model zoo 我们想要加载什么样的模型。我们在这里指定希望加载一个模型，将一个`[BufferedImage](https://javadoc.io/doc/ai.djl/api/latest/ai/djl/modality/cv/util/BufferedImageUtils.html)`作为输入，并预测一个`[Classifications](https://javadoc.io/doc/ai.djl/api/latest/ai/djl/modality/Classifications.html)`作为结果。然后我们可以使用`ModelZoo.loadModel`在模型库中找到匹配的模型。默认情况下，DJL 将在我们的内置库中寻找模型。我们需要告诉 DJL 查看一个自定义路径，该路径包含我们在培训部分获得的 TensorFlow SavedModel 格式。我们可以通过指定`【T3]来做到这一点。之后，我们创建一个新的预测器来运行预测并打印出分类结果。这非常简单明了。

# 定义你的翻译

当我们加载模型时，我们还想定义如何预处理模型的输入数据和后处理模型的输出数据。DJL 为这个函数使用了`[Translator](https://javadoc.io/doc/ai.djl/api/latest/ai/djl/modality/cv/translator/package-summary.html)`类。下面是实现过程:

转换器将输入数据格式从`[BufferedImage](https://javadoc.io/doc/ai.djl/api/latest/ai/djl/modality/cv/util/BufferedImageUtils.html)`转换为`[NDArray](https://javadoc.io/doc/ai.djl/api/latest/ai/djl/ndarray/NDArray.html)`，以符合模型的要求。它还将图像的大小调整为 224x224，并在将图像输入模型之前，通过除以 255 使图像正常化。运行推理时，您需要遵循在训练期间使用的相同预处理过程。在这种情况下，我们需要匹配 Keras 训练代码。运行预测后，模型将每个类别的概率输出为一个[数组](https://javadoc.io/doc/ai.djl/api/latest/ai/djl/ndarray/NDArray.html)。然后，我们将这些预测转换回我们想要的类别，即“肺炎”或“正常”。

# 下一步是什么？

就是这样！我们已经完成了对 x 光图像的预测。现在，您可以尝试构建更复杂的模型，并尝试使用更大的数据集进行学习。关注我们的 [GitHub](https://github.com/awslabs/djl/tree/master/docs) 、[演示库](https://github.com/aws-samples/djl-demo)和 [twitter](https://twitter.com/deepjavalibrary) 获取更多关于 DJL 的文档和示例！

# 关于 DJL

![](img/4b38a743d1a619d58607e4a67a06c036.png)

DJL 是一个用 Java 编写的深度学习框架，支持训练和推理。用户可以轻松地使用 DJL 部署他们喜欢的模型，而无需为各种引擎进行额外的转换。它包含 ModelZoo 概念，允许用户在 1 行中加载深度学习模型。DJL 模型动物园现在支持 70 多个预先训练好的模型，如 GluonCV、HuggingFace、TorchHub 和 Keras。

NDArray 的加入使其成为 Java 中运行深度学习应用的最佳工具包。它可以自动识别您运行的平台，并判断是否利用 GPU 来运行您的应用程序。

从最新发布的版本来看，DJL 0.5.0 正式支持 MXNet 1.7.0、PyTorch 1.5.0、TensorFlow 2.1.0。它还有一个 PyTorch Android 的实验引擎。

# 参考资料:

[1] [通过基于图像的深度学习识别医疗诊断和可治疗疾病](https://www.cell.com/cell/fulltext/S0092-8674(18)30154-5)
【2】[企业最常用的 10 种编程语言:2019 版](https://www.codeauthority.com/Blog/Entry/top-10-most-used-programming-languages-for-enterprise)
【3】[胸部 x 光图像(肺炎)](https://www.kaggle.com/paultimothymooney/chest-xray-pneumonia)
【4】[胸部 x 光图像的内核](https://www.kaggle.com/aakashnain/beating-everything-with-depthwise-convolution)