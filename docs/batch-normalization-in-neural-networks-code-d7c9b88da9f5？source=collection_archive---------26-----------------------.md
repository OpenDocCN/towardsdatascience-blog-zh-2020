# 神经网络中的批量标准化(包括代码)

> 原文：<https://towardsdatascience.com/batch-normalization-in-neural-networks-code-d7c9b88da9f5?source=collection_archive---------26----------------------->

## 通过 TensorFlow (Keras)实施

![](img/0c06e787dd13043efcbe34a7b9f6edf1.png)

克里斯托夫·高尔在 [Unsplash](https://unsplash.com/s/photos/code?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) 上拍摄的照片

# 介绍

**批量归一化(BN)** 是很多机器学习从业者遇到的一种技术。

如果你还没有，这篇文章解释了 BN 背后的基本直觉，包括它的起源以及如何使用 TensorFlow 和 Keras 在神经网络中实现它。

对于那些熟悉 BN 技术并想专注于实现的人来说，你可以跳到下面的**代码**部分。

对于那些可能对其背后的数学更感兴趣的人，请随意阅读下面的文章。

[](/batch-normalization-explained-algorithm-breakdown-23d2794511c) [## 解释了神经网络中的批量标准化(算法分解)

### 理解深度神经网络中使用的一种常见转换技术

towardsdatascience.com](/batch-normalization-explained-algorithm-breakdown-23d2794511c) 

# 定义

批次正常化是一种技术，它透过引入额外的层来减轻神经网路内不稳定梯度的影响，该层会对前一层的输入执行作业。这些操作对输入值进行标准化和规范化，然后通过缩放和移位操作转换输入值。

# 密码

第一步是导入将用于实现或支持神经网络实现的工具和库。使用的工具如下:

*   [**TensorFlow**](https://www.tensorflow.org/) :一个开源平台，用于机器学习模型的实现、训练和部署。
*   [**Keras**](https://keras.io/) :一个开源库，用于实现运行在 CPU 和 GPU 上的神经网络架构。

```
import tensorflow as tf
from tensorflow import keras
```

我们将利用的数据集是普通的[时尚-MNIST 数据集](https://github.com/zalandoresearch/fashion-mnist)。

时尚-MNIST 数据集包含 70，000 幅服装图像。更具体地说，它包括 60，000 个训练样本和 10，000 个测试样本，这些样本都是尺寸为 28×28 的灰度图像，分为十类。

数据集的准备包括通过将每个像素值除以 255.0 来归一化训练图像和测试图像。这将像素值置于范围 0 和 1 之间。

数据集的验证部分也在此阶段创建。这组数据集在训练期间被用来评估网络在各种迭代中的性能。

```
(train_images, train_labels),  (test_images, test_labels) = keras.datasets.fashion_mnist.load_data()
train_images = train_images / 255.0
test_images = test_images / 255.0
validation_images = train_images[:5000]
validation_labels = train_labels[:5000]
```

Keras 提供了实现分类模型所需的工具。Keras 提出了一种顺序 API，用于以连续的方式堆叠神经网络的层。

下面是一些层的信息，这些层将被实现以构成我们的神经网络。

*   **展平**:取一个输入形状，将输入图像数据展平成一维数组。
*   **密集**:密集层中嵌入了任意数量的单元/神经元。每个神经元都是一个感知器。
*   感知器是人工神经网络的基本组成部分，由弗兰克·罗森布拉特于 1958 年发明。感知器利用基于阈值逻辑单元的操作。
*   **批量规范化**:批量规范化层通过对传入的输入数据执行一系列操作来工作。这组操作包括进入 BN 层的输入值的标准化、规范化、重缩放和偏移。
*   **激活层**:对神经网络内的输入执行指定的操作。这一层在网络中引入了非线性。本文中实现的模型将利用激活函数:**整流线性单元(ReLU)** 和 **softmax** 。
*   由 **ReLU** 对来自神经元的值施加的变换由公式 y=max(0，x)表示。ReLU 激活函数将来自神经元的任何负值钳制为 0，而正值保持不变。该数学变换的结果被用作当前层的激活，并作为下一层的输入。

```
# Placing batch normalization layer before the activation layers
model = keras.models.Sequential([
    keras.layers.Flatten(input_shape=[28,28]),
    keras.layers.Dense(300, use_bias=False),
    keras.layers.BatchNormalization(),
    keras.layers.Activation(keras.activations.relu),
    keras.layers.Dense(200, use_bias=False),
    keras.layers.BatchNormalization(),
    keras.layers.Activation(keras.activations.relu),
    keras.layers.Dense(100, use_bias=False),
    keras.layers.BatchNormalization(),
    keras.layers.Activation(keras.activations.relu),
    keras.layers.Dense(10, activation=keras.activations.softmax)
])
```

让我们来看看 BN 层的内部组件

仅仅访问索引 2 处的层将向第一 BN 层内的变量及其内容提供信息，

```
model.layers[2].variables
```

我不会在这里进入太多的细节，但请注意变量名'伽马'和'贝塔'，这些变量中的值负责层内激活的重新缩放和偏移。

```
for variable in model.layers[2].variables:
    print(variable.name)>> batch_normalization/gamma:0
>> batch_normalization/beta:0
>> batch_normalization/moving_mean:0
>> batch_normalization/moving_variance:0
```

这篇[文章](/batch-normalization-explained-algorithm-breakdown-23d2794511c)详细介绍了 BN 层内的操作。

在密集层中，偏置分量被设置为假。偏差的省略是由于在激活的标准化过程中由于均值相减而发生的常量值的抵消。

下面是特斯拉现任人工智能总监安德烈·卡帕西(Andrej Karpathy)的一篇 twitter 帖子的片段。他的推文是基于经常犯的神经网络错误的主题，而不是在使用 BN 时将 bias 设置为 false。

在下一段代码中，我们设置并指定了用于训练已实现的神经网络的优化算法，以及损失函数和超参数，如学习速率和时期数。

```
sgd = keras.optimizers.SGD(lr=0.01, decay=1e-6, momentum=0.9, nesterov=True)
model.compile(loss="sparse_categorical_crossentropy", optimizer=sgd, metrics=["accuracy"])
```

现在，我们使用模型的顺序 API 的' *fit* 方法来训练网络。我们将跳过关于如何训练神经网络模型的一些细节。有关神经网络的训练和实现的详细解释的进一步信息，请参考下面的链接。

[](/in-depth-machine-learning-image-classification-with-tensorflow-2-0-a76526b32af8) [## 使用 TensorFlow 2.0 进行(深入)机器学习图像分类

### 理解实现用于图像分类的神经网络的过程。

towardsdatascience.com](/in-depth-machine-learning-image-classification-with-tensorflow-2-0-a76526b32af8) 

```
model.fit(train_images, train_labels, epochs=60, validation_data=(validation_images, validation_labels))
```

使用先前搁置的测试数据进行模型性能的评估。

通过评估结果，您可以在观察测试数据集评估的准确性后，决定微调网络超参数或继续生产。

```
model.evaluate(test_images, test_labels)
```

在训练阶段，您可能会注意到，与没有批量归一化图层的训练网络相比，每个历元的训练时间更长。这是因为批量标准化增加了神经网络的复杂性，以及模型在训练期间学习所需的额外参数。

尽管每个历元时间的增加与批量标准化减少了模型收敛到最优解所需的时间这一事实相平衡。

本文中实现的模型太浅，我们无法注意到在神经网络架构中使用批处理规范化的全部好处。通常情况下，批量归一化出现在更深层次的卷积神经网络中，如[异常](https://arxiv.org/abs/1610.02357)、 [ResNet50](https://www.mathworks.com/help/deeplearning/ref/resnet50.html) 和 [Inception V3](https://uk.mathworks.com/help/deeplearning/ref/inceptionv3.html) 。

# 额外的

*   上面实现的神经网络在激活层之前具有批量标准化层。但是在激活层之后添加 BN 层是完全可能的。

```
# Placing batch normalization layer after the activation layers
model = keras.models.Sequential([
    keras.layers.Flatten(input_shape=[28,28]),
    keras.layers.Dense(300, use_bias=False),
    keras.layers.Activation(keras.activations.relu),
    keras.layers.BatchNormalization(),
    keras.layers.Dense(200, use_bias=False),
    keras.layers.Activation(keras.activations.relu),
    keras.layers.BatchNormalization(),
    keras.layers.Dense(100, use_bias=False),
    keras.layers.Activation(keras.activations.relu),
    keras.layers.BatchNormalization(),
    keras.layers.Dense(10, activation=keras.activations.softmax)
])
```

*   研究人员已经对批量标准化技术做了一些广泛的工作。例如[批量重正化](https://arxiv.org/pdf/1702.03275.pdf)和[自归一化神经网络](https://arxiv.org/pdf/1706.02515.pdf)

# 结论

BN 是神经网络中常用的技术，因此理解该技术如何工作以及如何实现将是有用的知识，尤其是在分析大多数神经网络架构时。

下面是 GitHub 到一个笔记本的链接，其中包含了本文中的代码片段。

[](https://github.com/RichmondAlake/tensorflow_2_tutorials/blob/master/04_batch_normalisation.ipynb) [## Richmond alake/tensor flow _ 2 _ 教程

### permalink dissolve GitHub 是 4000 多万开发人员的家园，他们一起工作来托管和审查代码，管理…

github.com](https://github.com/RichmondAlake/tensorflow_2_tutorials/blob/master/04_batch_normalisation.ipynb) [](/should-you-take-a-masters-msc-in-machine-learning-c01336120466) [## 你应该读机器学习硕士吗？

### 包含来自理学硕士毕业生的想法和意见

towardsdatascience.com](/should-you-take-a-masters-msc-in-machine-learning-c01336120466) [](/how-does-ai-detect-objects-technical-d8d63fc12881) [## AI 如何检测物体？(技术)

### 了解如何使用机器和深度学习技术应用和实现对象检测

towardsdatascience.com](/how-does-ai-detect-objects-technical-d8d63fc12881)