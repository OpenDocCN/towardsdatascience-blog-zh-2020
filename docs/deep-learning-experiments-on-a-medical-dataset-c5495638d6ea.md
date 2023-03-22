# 使用深度学习的颅内出血检测

> 原文：<https://towardsdatascience.com/deep-learning-experiments-on-a-medical-dataset-c5495638d6ea?source=collection_archive---------25----------------------->

## 了解我们为脑出血分类而进行的一系列实验。

![](img/6b4ab494221aa015c4a63281f1c7a95a.png)

来源:https://swdic.com/radiology-services/mri-brain.php

本文由 [Prajakta Phadke](https://medium.com/u/2f12cb08e70?source=post_page-----c5495638d6ea--------------------------------) 合著

在过去十年中，深度学习在医疗应用中的使用增加了很多。无论是[使用视网膜病变](https://github.com/dipam7/deep_learning_for_healthcare/tree/master/Blindness_detection)识别糖尿病，从胸部 x 光片中预测
[肺炎](https://github.com/dipam7/deep_learning_for_healthcare/tree/master/Pneumonia_detection)还是[使用图像分割计数细胞和测量器官](/image-segmentation-with-fastai-9f8883cc5b53)，深度学习都在无处不在。从业者可以免费获得数据集来建立模型。

在本文中，您将了解我们在使用脑部核磁共振成像时进行的一系列实验。这些实验可以在 Github 上作为一系列笔记本获得。

![](img/1d67c9753f2cac1d1846531ed27d3045.png)

以这种方式给笔记本编号的原因是为了让其他人能够系统地浏览它们，看看我们采取了哪些步骤，我们得到了哪些中间结果，以及我们在此过程中做出了哪些决定。我们不是提供一个最终的抛光笔记本，而是想展示一个项目中的所有工作，因为这才是真正的学习所在。

该库还包括有用的链接，链接到领域知识的博客、研究论文和所有我们训练过的模型。我建议每个人都使用这个库，自己运行每一个笔记本，理解每一点代码是做什么的，为什么要写，怎么写。

让我们来看看每一个笔记本。所有这些笔记本都是在 Kaggle 或谷歌联合实验室上运行的。

## [00_setup.ipynb](https://github.com/dipam7/RSNA_Intracranial-hemorrhage/blob/master/nbs/00_setup.ipynb)

本笔记本包含在 Google colaboratory 上下载和解压缩数据的步骤。在 Kaggle 上，你可以直接访问数据。

## [01_data_cleaning.ipynb](https://github.com/dipam7/RSNA_Intracranial-hemorrhage/blob/master/nbs/01_data_cleaning.ipynb)

对于这个项目，我们使用了[杰瑞米·霍华德的干净数据集](https://www.kaggle.com/jhoward/rsna-hemorrhage-jpg)。他有一个[笔记本](https://www.kaggle.com/jhoward/cleaning-the-data-for-rapid-prototyping-fastai)，上面记录了他清理数据的步骤。我们在笔记本上复制了其中的一些步骤。一般来说，在深度学习中，快速清理数据以进行快速原型制作是一个好主意。

数据以 dicom 文件的形式提供，这是医疗数据的标准扩展。我们来看看其中一个的内容。

![](img/55e9d31d36fff9f296bf77f539da7e0f.png)

除了图像，它还包含一堆机器记录的元数据。我们可以使用这些元数据来深入了解我们的扫描。因此，我们将它们保存在数据帧中(因为 dicoms 访问和使用太慢)。

![](img/e7b4a29580aff2aa7c4e9f29ac724848.png)

查看包含许多列的数据帧的头部的一个有用的技巧是转置它。

现在让我们来看一些图片。

![](img/4491d98287ccfb7d54bac1c15ee1f766.png)

我们看到我们有一个人从头顶到牙齿的大脑不同切片的图像。这就是大脑扫描是如何从上到下进行的。有时顶部或底部切片可以是完全黑色的(面部之前或之后的切片)。这样的切片对我们的模型没有用，应该被删除。

元数据中有一个称为`img_pct_window`的有用栏，它告诉我们大脑窗口中像素的百分比。如果我们绘制该列，我们会得到下图:

![](img/fb3e137de653a11901ef1339134e92b9.png)

我们看到很多图像在大脑窗口几乎没有任何像素。因此，我们丢弃这样的图像。如果我们看一些我们丢弃的图像

![](img/d27cea2fc8f9968692fd6c9339feb06e.png)

我们看到它们是牙齿开始出现的较早或较晚的。

数据清理过程的第二步是修复`RescaleIntercept`列。关于这方面的更多信息，请参考杰里米的笔记本。最后，我们中心作物(以消除背景)和调整图像的大小为(256，256)。虽然高质量的图像会给出更好的精度，但这个尺寸对于原型来说已经足够了。

## [02 _ data _ exploration . ipynb](https://github.com/dipam7/RSNA_Intracranial-hemorrhage/blob/master/nbs/02_data_exploration.ipynb)

既然我们已经清理了数据，我们可以稍微研究一下。我们将分两个阶段进行建模。在第一阶段，我们只预测一个人是否有出血。

我们从检查空值开始。标签数据框没有空值，而元数据数据框有一些几乎完全为空的列。我们可以安全地删除这些列。

然后我们继续检查目标变量。

![](img/a3225c19df01329f9077f4787645e066.png)

我们有一个非常平衡的数据集。医学数据集通常不是这种情况。它们通常是不平衡的，正类样本的数量远少于负类样本的数量。

接下来，我们检查每个子类别的数量。

![](img/6761ac0a3a6055a653d3fa0146909be6.png)

硬膜下出血似乎是最常见的出血类型。元数据中一个有趣的列是“存储的位数”列，它表示用于存储数据的位数。它有两个不同的值:12 和 16。

![](img/a86347802716e388c16847df8a1ece9e.png)

这可能表明来自两个不同的组织。在深度学习中，通常建议使用来自相同分布的数据。最后，我们可以在不同的窗口中查看我们的图像。

![](img/17a838e0ea6f7a59de1645a4c29962a8.png)

然而，这只是针对人类的感知。神经网络可以处理浮点数据，并且不需要任何窗口。

## [03 _ data _ augmentation . ipynb](https://github.com/dipam7/RSNA_Intracranial-hemorrhage/blob/master/nbs/03_data_augmentation.ipynb)

训练一个好的深度学习模型的第一步是获取足够的数据。然而，这并不总是可能的。然而有可能的是，[对数据](/data-augmentations-in-fastai-84979bbcefaa)进行一些转换。

我们所做的是，不是每次都给模型提供相同的图片，而是做一些小的随机变换(一点旋转、缩放、平移等)，这些变换不会改变图像内部的内容(对于人眼而言)，但会改变其像素值。用数据扩充训练的模型将更好地概括。

在本笔记本中，我们在数据集上尝试了一些转换，看看它们是否有意义。还要注意，这些转换只应用于训练集。我们在验证集上应用非常简单的变换。

有点像空翻

![](img/6f3d83119602cccdce0202e3f16086b5.png)

或者轻微旋转

![](img/e924e0dc15c960a02115b90f7c4b209a.png)

有意义，但是其他一些转换可能一点用都没有，因为它们不像实际的数据。

![](img/2390723be7d1f1126edf9545c7ca3c83.png)

另请注意，我们已经裁剪和调整大小，所以我们不会这样做。

## [05_metadata.ipynb](https://github.com/dipam7/RSNA_Intracranial-hemorrhage/blob/master/nbs/05_metadata.ipynb)

最近的研究表明，元数据可以证明是有用的图像分类。我们可以通过平均预测来实现，或者[将两个数据输入神经网络](https://www.pyimagesearch.com/2019/02/04/keras-multiple-inputs-and-mixed-data/)。

为了做到这一点，我们首先尝试仅使用元数据来对出血进行分类，以衡量其有用性。我们发现，即使使用像随机森林这样的健壮模型，我们也只能得到 50%的准确率。因此，我们决定放弃元数据，专注于图像本身。

![](img/800ba68b4fc6a2f0155d4a7e040fc700.png)

## [04_baseline_model.ipynb](https://github.com/dipam7/RSNA_Intracranial-hemorrhage/blob/master/nbs/04_baseline_model.ipynb)

对于我们的基线模型，我们从一个 resnet18 作为主干开始，并在其上附加一个自定义头。[迁移学习在图像分类方面表现出了良好的效果](/how-do-pretrained-models-work-11fe2f64eaa2)。它提供了更好的结果，更快的培训，并大大减少了资源消耗。

然而，对于像医学图像这样的东西，[没有可用的预训练模型](https://www.fast.ai/2020/01/13/self_supervised/)。因此我们用 resnet 来凑合。我们在这个笔记本中训练两个模型，一个有和没有预训练。在这两个模型中，我们能够在仅仅 5 个时期内达到大约 89%的准确度。

![](img/84fff738a99c683f0c2918d680bccf3f.png)

`batch_size`被设置为 256。我们使用 Leslie Smith 的学习率查找器，在训练中找到一个好的学习率。

![](img/1cebbac1695f6e29fb894626cd1b792d.png)

简单地说，我们可以对我们的数据进行模拟训练，将我们的学习率从非常低的值变化到高达 10 的值。然后，我们计算损失，并将其与学习率进行对比。然后，我们选择具有最大下降斜率的学习速率。

在这种情况下，我们可以选择类似于`3e-3`的东西，因为`1e-1`和`1e-2`处于边缘，损耗很容易激增。我们也可以用一个切片来表示我们的学习率，例如`1e-5 to 1e-3`。在这种情况下，不同的学习率将适用于我们网络的不同群体。

对于训练，我们首先冻结模型的预训练部分，只训练新的头部。但是，我们确实更新了 resnet 的 batchnorm 层，以保持每层的平均值为 0，标准偏差为 1。

损失图如下所示。

![](img/1c343fb76050ff37015f0645000ad7c3.png)

然后我们可以展示一些结果。

![](img/35f046023479ec2a9f0ef0e1cab5cc95.png)

红色的是我们的模型分类错误的。

暂时就这样了。在我结束这篇文章之前，我想给出一个非常重要的提示:“在训练模型时不要设置随机种子，每次都让它在不同的数据集上训练。这将有助于您了解您的模型有多稳健。”

这篇文章和项目是一项正在进行中的工作，在下一阶段，你会看到以下笔记本。在那之前，祝你快乐。

# 接下来…

## 06 _ multi label _ classification . ipynb

可视化您的模型在分类时关注的图像部分。继续对出血的子类进行分类。一幅图像可以有一种或多种出血类型。

## 07 _ 不同 _archs.ipynb

因为我们已经认识到元数据对我们的应用程序并没有真正的用处，所以我们尝试了各种架构，比如 resnet34、resnet50、alexnet、densenet 等等。

## 08 _ 混合 _ 精确 _ 训练. ipynb

我们也使用混合精度训练来降低训练时间。

## 09 _ 锦囊妙计. ipynb

我们使用了魔术袋研究论文中的一些技巧。像渐进的图像大小调整。我们还试验了批量大小，我们试图尽可能地增加它。批量越大，更新效果越好。

## 10 _ 测试 _ 和 _ 部署. ipynb

测试时间增加，用于测试的查询数据集和用于部署的简单网站。