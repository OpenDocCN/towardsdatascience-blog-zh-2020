# 构建深度学习的人物分类器

> 原文：<https://towardsdatascience.com/building-a-deep-learning-person-classifier-ecc55bd01048?source=collection_archive---------8----------------------->

## 准确识别人脸和非人脸的图像

![](img/e0d6ccd91a3b7bb36fc9ea5e2aa3d85d.png)

照片由[岩田良治](https://unsplash.com/@ryoji__iwata?utm_source=medium&utm_medium=referral)在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄

# 介绍

在[机器学习](https://en.wikipedia.org/wiki/Machine_learning)和[统计](https://en.wikipedia.org/wiki/Statistics)，**分类**是识别一个新的[观察值](https://en.wikipedia.org/wiki/Observation)属于一组[类别](https://en.wikipedia.org/wiki/Categorical_data)(子群体)中的哪一个的问题，其基于包含其类别成员已知的观察值(或实例)的数据的[训练集](https://en.wikipedia.org/wiki/Training_set)[1]。

“浅层”学习技术，如[支持向量机](https://en.wikipedia.org/w/index.php?title=Support-vector_machine&oldid=940974287) (SVM)可以从中等大小的数据集产生有效的分类器，最近[深度学习](https://en.wikipedia.org/w/index.php?title=Deep_learning&oldid=940556767)分类器已经在识别任务中与人类相匹敌，但需要更大的数据集和大量计算资源来实现这一点。特别是，深度学习技术已经产生了[卷积神经网络](https://en.wikipedia.org/w/index.php?title=Convolutional_neural_network&oldid=940484524) (CNN)，这是最先进的图像分类器。

本文将向您展示如何使用 [TensorFlow](https://www.tensorflow.org/) 开发一个基于 CNN 的个人分类器，它在某些情况下可以胜过标准的人脸识别技术。标准方法通常包括使用像 SVM 这样的浅层学习器来对面部嵌入生成的模式进行分类(例如，参见高超的[面部识别](https://github.com/ageitgey/face_recognition)程序)。)标准技术是 ***极度*** 擅长在大部分人脸可见的情况下识别。然而，我的应用程序需要一种方法来准确地对只有部分面部视图的人进行分类，最好是从后面进行分类。这为使用深度学习开发一个人分类器提供了动力，我的目标是在看他们认识的人的图像时，无论有没有脸，都能达到和人类一样的识别精度。

需要以下步骤来构建准确的基于 CNN 的人分类器，该分类器识别新观察结果属于一组已知和未知人中的哪一个。这些步骤也适用于训练其他类型的基于 CNN 的分类器。

1.  **收集你想要分类的每个人以及陌生人的图像，以创建你的训练集并对其进行预处理**。您应该使用数据扩充方法，从收集的数据中自动生成相关的新图像，以增加数据集的大小。你的数据集很可能是不平衡的，也就是说，每一类的观察值并不相同。您将需要对此进行补偿，否则您的模型将过分强调具有更多观察值的类，其准确性将受到影响。最后，您的数据集需要转换成适合训练过程的数据格式。你应该为每堂课计划一个至少有 1000 个观察值的训练集。请注意，观察和样本这两个术语在本文中可以互换使用。
2.  **选择已经在标准数据集上训练过的 CNN，并将其用作将成为您的分类器的新模型中的一个层。**标准数据集应包含类似于您想要分类的类别。在这种情况下， [ImageNet](http://www.image-net.org/) 是一个很好的选择，因为“人”(在一般意义上)已经是它被训练识别的一个类。TensorFlow 提供了许多 CNN 模型，具有不同的复杂性和精确度。您应该选择能够满足您的应用程序的推理准确性要求的最简单的模型。当然，您可以对任何类似于 ImageNet 类的类进行微调，而不仅仅是人。
3.  **使用您的图像数据**在您的 CNN 上应用 [**转移学习**](https://www.tensorflow.org/tutorials/images/transfer_learning) **。深度神经网络容易[过度拟合](https://www.tensorflow.org/tutorials/keras/overfit_and_underfit)，你将学习几种缓解技术来对抗它。请注意，迁移学习也称为微调，训练模型也称为将模型拟合到数据集。这些术语在本文中可以互换使用。**
4.  **评估微调后的型号**。您应该检查微调模型的准确性，看看它是否符合您的应用要求，如果不符合，则使用更多数据或更优的训练参数(“超参数”)重新调整它。
5.  **保存您的模型并为推理做准备**。 [SavedModel](https://www.tensorflow.org/guide/saved_model) 是保存和提供 TensorFlow 程序的标准方式，但是您可能还希望生成一个 [TensorFlow Lite](https://www.tensorflow.org/lite) 表示，以便部署在移动或边缘设备上。两者都包括在下面。

请注意，我使用了一台 64GB 主内存的英特尔 i5 + Nvidia 1080Ti 机器来训练我的模型。你至少需要一台类似的机器来可行地训练一个深度学习模型。

另外，请注意，这些工作大部分是为[**smart-zone minder**](https://github.com/goruck/smart-zoneminder)项目完成的，您可以利用这些工作。如果您只想快速微调 TensorFlow CNN 而不深入细节，[在合适的机器上安装机器学习平台](https://github.com/goruck/smart-zoneminder#machine-learning-platform-installation-on-linux-server)，按如下所述准备您的数据集，并运行 Python 程序 [**train.py**](https://github.com/goruck/smart-zoneminder/blob/master/person-class/train.py) ，其中包含适合您配置的选项。本文的其余部分将一节一节地对 train.py 进行分解，以帮助您理解它是如何工作的，从而更好地使用它，并可能根据您自己的需要对它进行修改。

# 数据集准备

在一个名为“数据集”的目录中，为你想要识别的每个人的图像创建一个子目录，并以该人的名字命名(我用我的 Google photos 作为这个目录的种子)。另外，创建一个名为“未知”的子目录，保存随机陌生人的面孔。在每个子目录中，您可以选择放置另一个子目录，该子目录可以保存没有完整或部分面部视图的人的图像。您可以决定将此类图像作为其类的成员或未知类。数据集目录将如下所示。

```
dataset
    |-name_of_person_1
        |-sample_image_1
        |-sample_image_2
        |-sample_image_n
        |-images_with_no_face
            |-sample_image_1
            |-sample_image_2
            |-sample_image_n
    |-name_of_person_2
    |-name_of_person_n
    |-Unknown
```

下面的函数根据数据集的内容创建 Python 数据帧。

从数据集创建数据帧

dataframe 是 TF . keras . preprocessing . image . image data generator 类的 [flow_from_dataframe](https://www.tensorflow.org/api_docs/python/tf/keras/preprocessing/image/ImageDataGenerator#flow_from_dataframe) 方法的输入。ImageDataGenerator 通过实时数据扩充生成批量张量图像数据，并创建训练集和验证集。flow_from_dataframe 方法为每个集合创建一个 Python 生成器。该代码如下所示。

创建培训和验证生成器

虽然可以直接使用 train_generator 和 validation_generator 来拟合 CNN 模型，但是使用来自 [tf.data.Dataset](https://www.tensorflow.org/api_docs/python/tf/data/Dataset) 的数据集对象将会给[更好的拟合性能](https://www.tensorflow.org/guide/data_performance)。在 train.py 中，这是按如下方式完成的。

创建一个 tf 数据集

由于某些类别中的样本图像可能比其他类别中的多，因此您需要将类别权重传递给模型拟合过程。下面显示了实现这一点的代码，以及拟合过程用于训练和验证的步骤数。

确定类别权重和拟合步骤

这些数据现在可以用来拟合模型了。

# 模型准备

下面的函数从一个 [ImageNet](http://www.image-net.org/) 预训练的 [tf.keras.applications](https://www.tensorflow.org/api_docs/python/tf/keras/applications) 模型创建一个 CNN 模型。虽然该函数将基于 VGG16、InceptionResNetV2、MobileNetV2 和 ResNet50 创建模型，但只显示了 InceptionResNetV2。从基本模型中移除最后的 softmax 层，添加新的密集分类器和 softmax 层，然后编译它。请特别注意超参数常数，因为它们可能需要调整以适应您的数据集，但默认值给我很好的结果。该函数包括各种过拟合缓解技术，包括 [L2 正则化](/l1-and-l2-regularization-methods-ce25e7fc831c)、[标签平滑](/what-is-label-smoothing-108debd7ef06)和 D [ropout](http://jmlr.org/papers/volume15/srivastava14a/srivastava14a.pdf) 。它还会选择一个适当的 tf.keras 预处理器来正确格式化数据样本。

Python 函数来创建 CNN 模型

该模型现在可以进行微调了。

# 模型微调

微调分两步完成。第一遍使用高学习率，并且只训练新添加的密集层和软最大层，因为它们是用随机数据初始化的。第二遍使用小得多的学习速率，并训练最后几层以及基本模型的大部分。两遍方案以及正则化被设计成尽可能多地保留来自基础模型的原始权重，同时仍然从新的数据集中学习模式。两次通过都使用[提前停止](https://www.tensorflow.org/api_docs/python/tf/keras/callbacks/EarlyStopping)基于验证损失，这是减轻过度拟合的另一种方法。

第一遍微调代码如下所示。请再次注意，只有新的层是适合的。

微调的第一步

微调的第二步如下所示。请注意，底部基础模型的一些层是冻结的，不会在此步骤中进行训练，但所有其他层都将进行训练。冻结的层数是一个超参数，它在保留根据 ImageNet 的高级功能训练的层与允许顶层学习数据集中的新功能之间保持平衡。尽管采取了所有缓解措施，解冻的图层越多，防止过度拟合所需的数据就越多。

微调的第二步

该模型现在已经过微调，将评估其准确性，并保存以供下一步进行推断。

# 最终模型评估

应该对您的模型进行评估，以确定其精度是否满足您的应用要求。从它的预测中生成一个 [sklearn 分类报告](https://scikit-learn.org/stable/modules/generated/sklearn.metrics.classification_report.html)就是这样做的一种方式，如下面的代码所示。

生成分类报告

下面显示了我的一次运行的分类报告示例。虽然我很高兴这个模型偏向于它的创造者；)，需要更多的工作来通过数据集中的额外观察和超参数优化来提高其他类的准确性。

分类报告示例

目前，我最好的结果是使用 InceptionResNetV2 基本模型，它实现了大约 92%的整体准确性。我的目标是大于 95%，这是我对正常人在有脸和无脸图像上所能实现的主观估计。作为比较，使用上述标准方法对于具有完整或大部分完整人脸视图的那些图像产生大约 90%的准确度，而对于具有完整、部分人脸和没有人脸的相同数据集，准确度小于 70%。

# 保存用于推理的模型

train.py 包括以各种方式保存模型的选项-冻结的 TensorFlow 程序(从 TensorFlow 2 开始不推荐使用)，针对物联网设备上的推理进行优化的八位量化 TensorFlow Lite 程序，针对谷歌的 Edge TPU 和 TensorFlow SavedModel 格式(从 TensorFlow 2 开始的规范方法)编译的 TensorFlow Lite 程序。下面的代码显示了 train.py 如何保存为 SavedModel 格式。

将最终模型保存为 SavedModel 格式

最终的模型现在已经准备好通过它的 SavedModel 或其他表示进行推理了。有关保存和使用边缘模型的更多细节，请参见[在谷歌边缘 TPU](/using-keras-on-googles-edge-tpu-7f53750d952) 上使用 Keras。

# 结论

与用于训练基本模型的原始数据集相比，您可以利用 TensorFlow 通过中等大小的数据集快速微调 CNN 分类器，如上面的 person 分类器示例所示。来自[**smart-zone minder**](https://github.com/goruck/smart-zoneminder)项目的 Python 程序 [**train.py**](https://github.com/goruck/smart-zoneminder/blob/master/person-class/train.py) 可以用来构建自己的分类器，或者作为开发自己程序的参考。

将深度学习模型拟合到有限的数据可能会很棘手，因为它们可以快速记住数据集，但不会很好地推广到新的观察结果，这就是所谓的过度拟合。为了减轻这种情况，您可以使用以下技术。

*   收集更多的观察数据。
*   数据增强。
*   选择满足应用精度要求的最简单模型。
*   L2 正规化。
*   辍学。
*   标签平滑。
*   阻止许多基础模型层被定型(“冻结”)。
*   根据验证损失提前停止培训过程。

您还很可能不得不处理数据集中每个类的不同数量的样本(例如，不平衡的集合)，这将影响模型的准确性。为了进行补偿，您需要在模型拟合过程中使用类别权重。

构建 CNN 分类器的步骤如下。

1.  收集和预处理您的数据集。
2.  选择一个合适的 CNN 基本模型。
3.  微调模型。
4.  评估您的微调模型的准确性。
5.  准备您的最终模型进行推理，并保存它。

# 参考

[1] [统计分类](https://en.wikipedia.org/w/index.php?title=Statistical_classification&oldid=921296045)，来自*维基百科，免费百科。*