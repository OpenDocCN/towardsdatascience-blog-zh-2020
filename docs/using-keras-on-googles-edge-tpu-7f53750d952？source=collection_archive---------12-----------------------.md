# 在谷歌的边缘 TPU 使用 Keras

> 原文：<https://towardsdatascience.com/using-keras-on-googles-edge-tpu-7f53750d952?source=collection_archive---------12----------------------->

## 在边缘快速部署准确的模型

![](img/aa06d02d10bebf95185124e5c2dd494b.png)

内森·希普斯在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 拍摄的照片

使用 [Keras](https://keras.io/) 是快速理解和高效使用深度神经网络模型的好方法。目前，Keras API 不正式支持[量化感知训练](https://github.com/tensorflow/tensorflow/tree/r1.13/tensorflow/contrib/quantize)，因此无法使用该 API 直接生成一个用于 edge 硬件优化(例如，仅使用整数数学)推理的模型。与云相比，边缘上的推断可以提供几个优势，包括减少延迟、降低云成本和改善客户数据隐私。

本文涵盖了以下步骤，因此您可以利用快速高效的 [Google Coral Edge TPU](https://coral.ai/) 轻松使用 Keras 模型。

*   选择 Edge TensorFlow 处理单元(TPU)兼容的 Keras 模型。
*   在自定义数据集上微调模型。
*   使用训练后量化将微调的 Keras 模型转换为张量流(TF) Lite 模型。
*   编译用于 Edge TPU 的量化 TF Lite。
*   在边缘 TPU 上评估量化模型精度。
*   在边缘 TPU 上部署量化模型。

本文不会深入研究训练深度神经网络、TF Lite 或 Keras APIs，也不会深入研究模型量化背后的理论。您可以通过提供的各种链接来了解这些主题的更多信息。另外，参见[皮特·沃顿的优秀博客](https://petewarden.com/2015/05/23/why-are-eight-bits-enough-for-deep-neural-networks/)。这篇文章反映出我主要是一名工程师，比起花大量时间在理论上，我更关心让事情顺利进行。

请注意，我为 [smart-zoneminder](https://github.com/goruck/smart-zoneminder) 项目开发了大量材料，帮助我更好地理解深度学习，因为我在家里解决了一个实际问题——识别一个人是我的家人还是陌生人。我将使用项目中的例子来阐明这些材料，如下所示。

# 型号选择

根据 Google Coral [文档](https://coral.ai/docs/edgetpu/models-intro/)显示，只有少数 Keras 机型在 Edge TPU 上得到验证——[Mobilenet _ v1](https://www.tensorflow.org/api_docs/python/tf/keras/applications/MobileNet)、 [Mobilenet_v2](https://www.tensorflow.org/api_docs/python/tf/keras/applications/MobileNetV2) 、 [Inception_v3](https://www.tensorflow.org/api_docs/python/tf/keras/applications/InceptionV3) 和 [ResNet50](https://www.tensorflow.org/api_docs/python/tf/keras/applications/ResNet50) 。我决定使用 Mobilenet_v2、Inception_v3 和 ResNet50 来确认它们在 Edge TPU 上确实工作得很好。我还[尝试了 VGG16](https://www.tensorflow.org/api_docs/python/tf/keras/applications/VGG16) 和 [InceptionResNetV2](https://www.tensorflow.org/api_docs/python/tf/keras/applications/InceptionResNetV2) ，看看它们在对比中表现如何。这些模型对于我的 [smart-zoneminder](https://github.com/goruck/smart-zoneminder) 应用程序来说是完美的，因为它需要一种方法来分类一个人是我的家庭成员还是陌生人——这是这些卷积神经网络(CNN)模型的理想用途。选择这些模型可以很好解决的问题来使用 edge 硬件，否则当您尝试将另一种类型的网络(可能使用 TF Lite 无法量化的运算符)映射到硬件时，您将处于未知领域。

# 模型微调

有许多很好的资料向您展示如何在您自己的定制数据集上微调 Keras 模型，例如 [Keras 文档](https://keras.io/applications/)和 [smart-zoneminder](https://github.com/goruck/smart-zoneminder/blob/master/person-class/train.py) 。但是，我发现在 Keras 模型的输出中使用 max pooling 将不会针对边 TPU 进行编译，因为它有一个当前无法由 TF Lite 量化的运算符(REDUCE_MAX)。因此，您将需要使用平均池(或无)，如下例所示。

使用平均池实例化 Keras 模型

# 训练后量化

由于 Keras 模型不支持[量化感知训练](https://github.com/tensorflow/tensorflow/tree/r1.13/tensorflow/contrib/quantize)，您需要使用训练后量化，如 [TF Lite 文档](https://www.tensorflow.org/lite/performance/post_training_quantization)中所述。在 [smart-zoneminder](https://github.com/goruck/smart-zoneminder) 项目中使用的 [Python 训练脚本](https://github.com/goruck/smart-zoneminder/blob/master/person-class/train.py)将在 Keras 模型的最终训练步骤之后运行训练后量化，如下所示。

从 Keras .h5 文件生成量化的 TFLite 模型

[keras_to_tflite_quant](https://github.com/goruck/smart-zoneminder/blob/master/person-class/keras_to_tflite_quant.py) 模块将 keras 模型(以. h5 格式)量化为 TF Lite 8 位模型。该脚本还可以评估量化模型的准确性，但 TF Lite 模型的推理在英特尔 CPU(我与 Nvidia GPU 一起用于培训)上非常慢，因为当前的 TF Lite 运算符内核针对 ARM 处理器进行了优化(使用 NEON 指令集)。参见此[线程](https://stackoverflow.com/questions/54093424/why-is-tensorflow-lite-slower-than-tensorflow-on-desktop)了解更多细节，并在下面了解评估边缘 TPU 量化模型的可行方法。

# 为边缘 TPU 编译

在下一步中，您需要使用 [Google Coral 编译器](https://coral.ai/docs/edgetpu/compiler/)为 Edge TPU 编译量化 TensorFlow Lite 模型。在 [smart-zoneminder](https://github.com/goruck/smart-zoneminder) 项目中使用的 [Python train 脚本](https://github.com/goruck/smart-zoneminder/blob/master/person-class/train.py)将运行如下所示的编译器。

为边缘 TPU 编译

# 评估量化模型的准确性

你的模型会受到量化的影响，所以你应该检查一下它的精度是否在应用程序所能容忍的范围之内。一种快速的方法是使用 TPU 的量化模型对用于开发模型的训练和验证数据集进行推理。为此，我使用了 Python 脚本 [evaluate_model.py](https://github.com/goruck/smart-zoneminder/blob/master/tpu-servers/evaluate_model.py) ，它是在 TPU 边缘运行的[智能区域管理器](https://github.com/goruck/smart-zoneminder)项目的一部分。该脚本的实际评估函数如下所示。参见 Google Coral [文档](https://coral.ai/docs/edgetpu/tflite-python/)了解更多关于如何使用 TF Lite 在 edge TPU 上运行推理的信息。

函数在边 TPU 上运行推理

Resnet50 的 evaluate_model.py 的输出如下所示。您可以看到精度为 0.966，非常接近训练和验证数据集精度 0.973 的加权平均值。这个模型的量化损失对我的应用来说是可以接受的。

TPU 评估结果的量化结果

总体而言，我测试的所有模型都具有可接受的量化性能，除了 InceptionResNetV2，其量化性能仅为 0.855，而基线为 0.980。目前，尚不清楚这种量化模型表现不佳的原因。

# 部署量化模型

在您验证了足够的模型准确性之后，您可以部署它以在您的应用程序中运行推理。smart-zoneminder 为此使用了一个基于 zerorpc 的推理服务器。

# 结论

通过仔细选择和训练 Edge TPU 兼容的 Keras 模型，您可以快速将推理模型部署到 Edge。这些模型必须首先量化为 8 位，然后针对边缘 TPU 进行编译。量化过程会在模型中引入精度误差，在生产环境中使用之前，应评估其在应用程序中的适用性。边缘推理提供了几个优势，包括减少延迟、降低云成本和改善客户数据隐私。