# 基于 YoloV3 卫星影像的储油罐容积占有率研究

> 原文：<https://towardsdatascience.com/oil-storage-tanks-volume-occupancy-on-satellite-imagery-using-yolov3-3cf251362d9d?source=collection_archive---------27----------------------->

## **使用 Tensorflow 2.x 从零开始使用 Yolov3 物体检测模型识别卫星图像中的储油罐，并借助*浮头罐*产生的阴影计算其所占体积。**

![](img/790742036805a2cb06b47327eab7346d.png)

[来源](https://www.kaggle.com/towardsentropy/oil-storage-tanks)

在 1957 年之前，我们的地球只有一颗天然卫星:月球。1957 年 10 月 4 日，[苏联](https://en.wikipedia.org/wiki/Soviet_Union)发射了世界上第一颗人造卫星。自那时以来，来自 40 多个国家的大约 8900 颗卫星已经发射。

![](img/3ade3114eb48e6505c0254bc6be5a695.png)

美国宇航局在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 拍摄的照片

这些卫星帮助我们进行监测、通信、导航等等。这些国家还利用卫星来监视另一个国家的土地及其动向，以评估他们的经济和实力。然而，所有的国家都互相隐瞒他们的信息。

同样，全球石油市场也不完全透明。几乎所有产油国都在努力隐瞒其总的生产、消费和储存量。各国这样做是为了间接地向外界隐瞒他们的实际经济状况，并增强他们的防御系统。这种做法可能导致对其他国家的威胁。

出于这个原因，许多初创公司，如 [Planet](https://www.planet.com/) 和 [Orbital Insight](https://orbitalinsight.com/) 都出来通过卫星图像来关注各国的此类活动。Thye 收集储油罐的卫星图像并估计储量。

但问题是，人们如何仅仅通过卫星图像来估计坦克的体积？嗯，只有当油储存在浮顶/高位槽时，这才有可能。这种特殊类型的储罐是专为储存大量石油产品，如原油或凝析油而设计的。它由直接位于油顶部的顶盖组成，顶盖随着油箱中油的体积而上升或下降，并在其周围形成两个阴影。正如你所看到的下图，阴影在北面

![](img/7c3d7296287ac47e465ac98df1669ec6.png)

[来源](https://medium.com/planet-stories/a-beginners-guide-to-calculating-oil-storage-tank-occupancy-with-help-of-satellite-imagery-e8f387200178)

储罐的(外部阴影)是指储罐的总高度，而储罐内的阴影(内部阴影)显示了浮头/浮顶的深度(即储罐有多少空)。并且体积将被估计为 1-(内部阴影面积/外部阴影面积)。

在这篇博客中，我们将使用 Tensorflow2.x 框架，在 python 语言的卫星图像的帮助下，从头开始实现完整的模型来估计坦克的占用体积。

## GitHub 回购

> 本文的所有内容和全部代码都可以在 [this](https://github.com/mdmub0587/Oil-Storage-Tank-s-Volume-Occupancy) github 资源库中找到

以下是该博客关注的内容列表。我们会一个一个地探索。

# 目录

1.  [问题陈述、数据集和评估指标](#51b9)
2.  [现有方法](#e4a8)
3.  [相关研究著作](http://e7bb)
4.  [有用的博客和研究论文](http://a969)
5.  [我们的贡献](#8e89)
6.  [探索性数据分析(EDA)](#967b)
7.  [第一次切割方法](#2eee)
8.  [数据预处理、扩充和 TFRecords](#1bd6)
9.  [使用 YoloV3 进行物体检测](#e9aa)
10.  [预留量估算](#973f)
11.  [结果](#6d3c)
12.  [结论](#cf07)
13.  [未来工作](#e8fc)
14.  [参考文献](#c93d)

# 1.问题陈述、数据集和评估指标。

## 问题陈述:

检测浮头罐并估计其中的油的储备/占用体积。随后将图像补片重新组合成增加了体积估计的全尺寸图像。

## 数据集:

**数据集链接:**[https://www.kaggle.com/towardsentropy/oil-storage-tanks](https://www.kaggle.com/towardsentropy/oil-storage-tanks)？

该数据集包含一个带注释的边界框，卫星图像取自谷歌地球上包含世界各地工业区的储罐。数据集中有 2 个文件夹和 3 个文件。让我们一个一个来看。

*   **large_images:** 这是一个文件夹/目录，包含 100 张大小为 4800x4800 的卫星原始图像。所有图片均以 id_large.jpg 格式命名。
*   **Image _ patches:**Image _ patches 目录包含从大图像生成的 512x512 个补丁。每个大图像被分割成 100，512x512 个小块，在两个轴上的小块之间有 37 个像素的重叠。图像补丁按照 id_row_column.jpg 格式命名
*   **labels.json:** 它包含了所有图片的标签。标签存储为字典列表，每个图像一个。不包含任何浮头罐的图像被标注为“跳过”。边界框标签采用边界框四个角的(x，y)坐标对的格式。
*   **labels_coco.json:** 包含了与之前文件相同的标签，转换成 coco 标签格式。这里边界框被格式化为[x_min，y_min，width，height]。
*   **large_image_data.csv:** 包含大图像文件的元数据，包括每张图像的中心坐标和高度。

## 评估指标:

对于坦克的检测，我们将对每种类型的坦克使用**平均精度(AP)** ，对所有类型的坦克使用**图(平均平均精度)**。浮头式水箱的估计容积没有度量标准。

地图是对象检测模型的标准评估度量。地图的详细解释可以在下面的 youtube 播放列表中找到

[来源](https://www.youtube.com/playlist?list=PL1GQaVhO4f_jE5pnXU_Q4MSrIQx4wpFLM)

# 2.现有方法

卡尔·基耶在他的仓库里使用 RetinaNet 来完成坦克探测任务。他从头开始制作模型，并从这个数据集应用生成的锚盒。这导致浮头罐的平均精度(AP)得分为 76.3%。然后他应用阴影增强和像素阈值来计算它的体积。

据我所知，这是互联网上唯一可用的方法。

# 3.相关研究工作

## 基于高分辨率遥感图像估算油罐容积[【2】](#bb27):

提出了基于卫星图像估计油罐容量/体积的解决方案。为了计算坦克的总体积，他们需要坦克的高度和半径。为了计算高度，他们使用了投影长度的几何关系。但是计算影子的长度并不容易。为了突出阴影，使用 HSV(即色调饱和度值)颜色空间，因为通常，阴影在 HSV 颜色空间中具有高饱和度和增加的色调。然后采用基于亚像素细分定位的中值法计算阴影长度。最后，通过霍夫变换算法得到油罐的半径。

在本文的相关工作中，提出了基于卫星图像计算建筑物高度的解决方案。

# 4.有用的博客和研究论文

## 借助卫星图像计算储油罐容积的初学者指南[【3】](#cdaa):

[](https://medium.com/planet-stories/a-beginners-guide-to-calculating-oil-storage-tank-occupancy-with-help-of-satellite-imagery-e8f387200178) [## 借助卫星图像计算储油罐容积的初学者指南

### 在 TankerTrackers.com，我们的使命是借助……展示石油现货市场的鸟瞰图

medium.com](https://medium.com/planet-stories/a-beginners-guide-to-calculating-oil-storage-tank-occupancy-with-help-of-satellite-imagery-e8f387200178) 

这个博客是 TankerTracker.com 自己写的。其中一项服务是利用卫星图像追踪几个地理和地缘政治景点的原油储存情况。在这篇博客中，他们详细描述了油罐的外部和内部阴影如何帮助我们估计油罐中的油量。还比较了卫星在特定时间和一个月后拍摄的图像，显示了一个月内储油罐的变化。这个博客给了我们一个直观的知识，那就是体积的估算是如何进行的。

## 用深度学习温柔介绍物体识别[【4】](#254d):

本文涵盖了对象检测初学者头脑中出现的最令人困惑的概念。首先，描述物体分类、物体定位、物体识别和物体检测之间的区别。然后讨论了一些主要的先进的深度学习算法，以开展对象识别任务。

对象分类是指将标签分配给包含单个对象的图像。而对象定位意味着围绕图像中的一个或多个对象绘制边界框。目标检测任务结合了目标分类和定位。这意味着这是一项更具挑战性/更复杂的任务，首先通过定位技术在感兴趣对象(OI)周围绘制一个边界框，然后在分类的帮助下为每个 OI 分配一个标签。目标识别不过是上述所有任务的集合(即分类、定位和检测)。

![](img/3fe4902869eba6eb49ff814a0a745369.png)

[来源](https://machinelearningmastery.com/object-recognition-with-deep-learning/)

最后，讨论了两大类目标检测算法/模型，它们是基于区域的卷积神经网络(R-CNN)和你只看一次(YOLO)。

## 对象识别的选择性搜索【T6【5】:

在目标检测任务中，最关键的部分是目标定位，因为目标分类在此之后。分类取决于本地化建议的感兴趣区域(简称区域建议)。更完美的定位将导致更完美的对象检测。选择性搜索是最先进的算法之一，在一些物体识别模型中用于物体
定位，如 [R-CNN](https://arxiv.org/abs/1311.2524) 和 Fast [R-CNN](https://arxiv.org/abs/1504.08083) 。

该算法首先使用有效的基于图的图像分割生成输入图像的子片段，然后使用贪婪算法将较小的相似区域组合成较大的区域。片段相似性基于四个属性，即颜色、纹理、大小和填充。

![](img/2ce69ff62e6f805d8affe4d4afe2e4d8.png)

[信号源](https://www.geeksforgeeks.org/selective-search-for-object-detection-r-cnn/)

## 区域提案网—详细视图[【6】](#cafe):

RPN(区域建议网络)因其比传统的选择性搜索算法更快而被广泛用于目标定位。它从特征图中学习感兴趣对象的最佳位置，就像 CNN 从特征图中学习分类一样。它负责三个主要任务，首先生成锚框(从每个特征地图点生成 9 个不同形状的锚框)，其次，将每个锚框分类为前景或背景(即，它是否包含对象)，最后，学习锚框的形状偏移以使它们适合对象。

## 更快的 R-CNN:利用区域提议网络实现实时目标检测[【7】](#37d5):

更快的 R-CNN 模型解决了前两个相对模型(R-CNN 和快速 R-CNN)的所有问题，并使用 RPN 作为区域提议生成器。其架构与快速 R-CNN 完全相同，只是它使用 RPN 而不是选择性搜索，这使得它比快速 R-CNN 快 34 倍。

![](img/1105a88aceaa3e5940b5f9eef494786a.png)

[来源](https://arxiv.org/abs/1506.01497)

## 使用 YOLO、YOLOv2 和现在的 YOLOv3 进行实时对象检测[【8】](#7c56):

在介绍 Yolo(你只看一次)系列模型之前，我们先来看看它的首席研究员 Joseph Redmon 在 Ted Talks 上的演示。

该模型位于对象检测模型列表的顶部有许多原因。但是，最主要的原因还是它的牢度。它的推断时间非常少，这是它容易匹配视频的正常速度(即 25 FPS)并应用于实时数据的原因。下面是 [Yolo 网站](https://pjreddie.com/darknet/yolo/)提供的 YoloV3 在 [COCO 数据集](https://cocodataset.org/#home)上的精度和速度对比。

![](img/cbc2e2a9b1424495bc83b39fd1f48d41.png)

[来源](https://pjreddie.com/darknet/yolo/)

**与其他物体检测模型不同，Yolo 模型具有以下特征。**

*   单个神经网络模型(即分类和定位任务将从同一模型执行):将照片作为输入，并直接预测边界框和每个边界框的类别标签，这意味着它只看图像一次。
*   由于它对整个图像而不是图像的一部分执行卷积，因此它产生非常少的背景错误。
*   YOLO 学习物体的概括表示。当在自然图像上训练和在艺术品上测试时，YOLO 远远胜过 DPM 和 R-CNN 等顶级检测方法。由于 YOLO 是高度概括的，当应用于新的领域或意想不到的输入时，它不太可能崩溃。

**是什么让 YoloV3 比 Yolov2 优秀。**

*   如果你仔细看了 yolov2 论文的标题，那就是“YOLO9000:更好、更快、更强”。是 yolov3 比 yolov2 好很多吗？好吧，答案是肯定的，它更好但不是更快更强，因为暗网架构的复杂性增加了。
*   Yolov2 使用 19 层暗网架构，没有任何残留块、跳过连接和上采样，因此它很难检测到小对象。但是，在 Yolov3 中添加了这些功能，并使用了在 Imagenet 上训练的 53 层 DarkNet 网络。除此之外，还堆叠了 53 层卷积层，形成了 106 层完全卷积层架构。

![](img/8d97c128a38583cf763d29ff92fbf459.png)

[来源](/dive-really-deep-into-yolo-v3-a-beginners-guide-9e3d2666280e)

*   Yolov3 在三个不同的尺度上进行预测，首先是在 13X13 网格中预测大型物体，其次是在 26X26 网格中预测中型物体，最后是在 52X52 网格中预测小型物体。
*   YoloV3 总共使用 9 个锚盒，每个音阶 3 个。使用 K-均值聚类来选择最佳锚盒。
*   Yolov3 现在对图像中检测到的对象执行多标签分类。通过逻辑回归预测对象置信度和类别预测。

# 5.我们的贡献

我们的问题陈述包括两个任务，第一个是浮头罐的检测，另一个是阴影的提取和被识别的罐的体积的估计。第一项任务基于目标检测，第二项任务基于计算机视觉技术。让我们描述一下解决每项任务的方法。

## 储罐检测:

我们的目标是估计浮头罐的容积。我们可以为单个类建立目标检测模型，但是，为了减少模型与另一种坦克(即坦克/固定头坦克和坦克群)的混淆，并且为了使其稳健，我们提出了三个类目标检测模型。 *YoloV3 具有用于物体检测的转移学习*，因为它易于在不太特定的机器上训练。此外，为了增加度量值，还应用了*数据增强*。

## 阴影提取和体积估计；

阴影提取涉及许多计算机视觉技术。由于 RGB 颜色方案对阴影不敏感，我们必须先将其转换为 HSV 和 LAB 颜色空间。我们已经使用-(l1+l3)/(V+1)(其中 l1 是 LAB 颜色空间的第一个通道值)比率图像来增强阴影部分。之后，通过阈值化 0.5*t1 + 0.4*t2(其中 t1 是最小像素值，t2 是平均值)对增强的图像进行滤波。然后用形态学操作(即，清晰的噪声、清晰的轮廓等)处理阈值图像。最后，提取两个坦克阴影轮廓，然后通过上述公式估计所占体积。这些想法摘自下面的笔记本。

[](https://www.kaggle.com/towardsentropy/oil-tank-volume-estimation) [## 油罐容积估算

### 使用 Kaggle 笔记本探索和运行机器学习代码|使用储油罐中的数据

www.kaggle.com](https://www.kaggle.com/towardsentropy/oil-tank-volume-estimation) 

遵循整个管道来解决此案例研究，如下所示。

![](img/e7e7a0b9a9bb4c854f5fe562012f6fd3.png)

让我们从数据集的探索性数据分析开始吧！！

# 6.探索性数据分析

## 探索 Labels.json 文件:

作者代码

![](img/75554929179bb9c3db933b7db2d6668b.png)

作者图片

> 所有的标签都存储在字典列表中。总共有 10K 的图像。不包含任何坦克的图像标记为**跳过**，包含坦克的图像标记为**坦克**、**坦克群**或**浮头坦克**。每个坦克对象都有字典格式的四个角点的边界框坐标。

## 计数对象:

![](img/25b315c15f52369d321d11d0666d1c29.png)

作者图片

> 在 10K 图像中，8187 幅图像没有标签(即它们不包含任何坦克物体)。此外，至少包含一个**坦克群**对象的图像有 81 个，至少包含一个**浮头坦克**的图像有 1595 个。
> 
> 在条形图中，可以观察到包含图像的 1595 个浮头罐中的 26.45%的图像仅包含一个浮罐对象。单个图像中浮动头坦克对象的最大数量是 34。

## 探索 labels_coco.json 文件:

作者代码

![](img/37ef4dc0e5d87dd4797049a37c5d34bb.png)

作者图片

> 该文件仅包含浮头罐的边界框以及字典格式列表中的图像 id

## 绘制边界框:

![](img/dba795f49c6bbb9d2c3cb887436b0d45.png)

作者图片

> 有三种坦克:
> 
> 1.坦克(吨)
> 
> 2.坦克集群，
> 
> 3.浮头罐(FHT)

# 7.首次切割方法

在 EDA 中，已经观察到 10000 个图像中的 8171 个是无用的，因为它们不包含任何对象。此外，1595 个图像包含至少一个浮头坦克对象。我们知道，所有的深度学习模型都渴望数据，如果没有足够的数据，性能就会很差。

因此，我们的第一种方法是数据扩充，然后将获得的扩充数据拟合到 Yolov3 对象检测模型中。

# 8.数据预处理、扩充和 TFRecords

## 数据预处理:

观察到对象的注释以具有 4 个角点的 Jason 格式给出。首先，从这些角点中提取左上和右下点。接下来，属于单个图像的所有注释及其相应的标签都保存在 CSV 文件的单行列表中。

从角点提取左上角和右下角点的代码

作者代码

CSV 文件将如下所示

![](img/55475dcc3b6c66e4540055f564c4814d.png)

作者图片

为了评估该模型，我们将保留 10%的图像作为测试集。

作者代码

## 数据扩充:

正如我们所知，物体检测需要大量的数据，但我们只有 1645 张图像用于训练，这非常少。为了增加数据，我们必须进行数据扩充。在这个过程中，通过翻转和旋转原始图像来生成新图像。所有的功劳都归入下面的 GitHub 库，代码就是从这里提取的。

[](https://blog.paperspace.com/data-augmentation-for-bounding-boxes/) [## 边界框的数据扩充:翻转

### 当谈到从深度学习任务中获得良好的表现时，数据越多越好。然而，我们可能只会…

blog.paperspace.com](https://blog.paperspace.com/data-augmentation-for-bounding-boxes/) 

通过执行以下动作，从单个原始图像生成 7 个新图像:

1.  水平翻转
2.  旋转 90 度
3.  旋转 180 度
4.  旋转 270 度
5.  水平翻转和 90 度旋转
6.  水平翻转和 180 度旋转
7.  水平翻转和 270 度旋转

下面显示了一个示例

![](img/004ea314fe65548260a04e539a893179.png)

作者图片

## TFRecords 文件:

TFRecords 是 TensorFlow 自己的二进制存储格式。当数据集太大时，这通常是有用的。它以二进制格式存储数据，会对定型模型的性能产生重大影响。复制二进制数据需要的时间更少，而且占用的空间也更少，因为在训练时只加载一批数据。你可以在下面的博客中找到它的详细描述。

[](https://medium.com/mostly-ai/tensorflow-records-what-they-are-and-how-to-use-them-c46bc4bbb564) [## Tensorflow 记录？它们是什么以及如何使用它们

### 自 2015 年 11 月推出以来，对 Tensorflow 的兴趣稳步增长。一个鲜为人知的组成部分…

medium.com](https://medium.com/mostly-ai/tensorflow-records-what-they-are-and-how-to-use-them-c46bc4bbb564) 

你也可以查看下面的 Tensorflow 文档。

[](https://www.tensorflow.org/tutorials/load_data/tfrecord) [## TFRecord 和 tf.train.Example | TensorFlow 核心

### 为了有效地读取数据，将数据序列化并存储在一组文件(每个文件 100-200MB)中会很有帮助，这些文件…

www.tensorflow.org](https://www.tensorflow.org/tutorials/load_data/tfrecord) 

我们的数据集已经被转换成 RFRecords 格式。这个任务没有必要，因为我们的数据集并不庞大。然而，这样做是为了获取知识。如果感兴趣，您可以在我的 GitHub 资源库中找到代码。

# 9.使用 YoloV3 的对象检测

## 培训:

为了训练 yolov3 模型，使用了迁移学习。第一步包括加载暗网网络的权重，并冻结它以在训练期间保持权重不变。

![](img/dd61affa7907e1cdad3366298e3b3cb2.png)

我们已经使用 adam optimizer(初始学习率=0.001)来训练我们的模型，并应用余弦衰减来降低学习率与历元数的关系。训练过程中使用模型检查点保存最佳权重，训练完成后保存最后一个权重。

损失图:

![](img/8e796a33bea9260d5b378a306d50ede4.png)

作者图片

## Yolo 损失函数:

用于 Yolov3 模型训练的损失函数相当复杂。Yolo 在三个不同的尺度上计算了三个不同的损失，并求和用于反向传播(正如您在上面的代码单元格中所看到的，最终损失是三个不同损失的列表)。每个损失借助 4 个子函数计算定位和分类损失。

1.  中心(x，y)的均方差(MSE)。
2.  边界框的宽度和高度的均方误差(MSE)。
3.  包围盒的二元交叉熵客观分数和无客观分数
4.  包围盒的多类预测的二元交叉熵或稀疏分类交叉熵。

让我们看看 Yolov2 中使用的损失公式，并检查不同的来源

![](img/5e03d918a5d2811fe1a5a320d90b2611.png)

[来源](https://pjreddie.com/media/files/papers/yolo_1.pdf)

Yolov2 中的最后三项是平方误差，而在 Yolov3 中，它们被交叉熵误差项所取代。换句话说，Yolov3 中的对象置信度和类预测现在是通过逻辑回归来预测的。

看看 Yolov3 损失函数的实现

## 分数:

为了评估我们的模型，我们使用了测试和训练数据的 AP 和 mAP

**测试分数**

作者代码

![](img/b3600510fc453d525500cfcf9f7a7ba0.png)

作者图片

**训练成绩**

作者代码

![](img/1d6c028cfc13ad14597060bb3fcaf2a7.png)

作者图片

## 推论:

让我们看看模型是如何表演的！！

![](img/f327e0935453512ec5b0d8c382fba917.png)

作者图片

![](img/e700d44562005c996f472cacb7fbec52.png)

作者图片

# 10.保留体积估算

体积的估算是本案例研究的最终结果。没有度量标准来评估储罐的估计容积。然而，我们已经尝试提出图像的最佳阈值像素值，以便可以在很大程度上检测阴影区域(通过计算像素数量)。

我们将使用由卫星捕获的形状为 4800X4800 的大图像，并将其分成 100 个 512x512 的小块，在两个轴上的小块之间有 37 个像素的重叠。图像补丁按照 id_row_column.jpg 格式命名。

每个生成的预测补丁将被存储在一个 CSV 文件中。我们只储存了一个浮头水箱的包围盒。接下来，估计每个浮头罐的体积(代码和解释可以在我的 GitHub 存储库中以笔记本格式获得)。最后，所有的图像块以及边界框与标签合并，作为大图像的估计体积。你可以看看下面给出的例子:

![](img/caf812c0eba3abe9d59b259b6267ffa5.png)

作者图片

# 11.结果

浮头式水箱在试验装置上的 AP 分数为 0.874，在列车装置上的 AP 分数为 0.942。

# 12.结论

*   仅用有限数量的图像就获得了相当好的结果。
*   数据扩充工作做得很好。
*   在这种情况下，与 RetinaNet 模型的现有方法相比，yolov3 表现良好。

# 13.未来的工作

*   对于浮头罐，获得了 87.4%的 AP，这是很好的分数。然而，我们可以尝试在某种程度上增加分数。
*   我们将尝试用增强生成的更多数据来训练这个模型。
*   我们将尝试训练另一个更准确的模型，如 yolov4、yolov5(非官方)。

# 14.参考

[1] [油罐容积估算](https://github.com/kheyer/Oil-Tank-Volume-Estimation)，作者:Karl Heyer，2019 年 11 月。

[2] [根据高分辨率遥感图像估算油罐容积](https://www.researchgate.net/publication/332193936_Estimating_the_Volume_of_Oil_Tanks_Based_on_High-Resolution_Remote_Sensing_Images)王童，，俞胜涛，，2019 年 4 月。

[3][TankerTrackers.com2017 年 9 月](https://medium.com/planet-stories/a-beginners-guide-to-calculating-oil-storage-tank-occupancy-with-help-of-satellite-imagery-e8f387200178)借助卫星图像计算储油罐容积的初学者指南。

[4] [深度学习物体识别](https://machinelearningmastery.com/object-recognition-with-deep-learning/)温柔介绍作者[https://machinelearningmastery.com/](https://machinelearningmastery.com/)，2019 年 5 月。

[5] [由 el 的 J.R.R. Uijlings 进行的对象识别的选择性搜索](http://www.huppelen.nl/publications/selectiveSearchDraft.pdf)。2012

[6] [地区提案网 Sambasivarao 的详细观点](/region-proposal-network-a-detailed-view-1305c7875853)。k，2019 年 12 月

[7] [更快的 R-CNN:通过区域提议网络实现实时对象检测](https://arxiv.org/abs/1506.01497) Ross Girshick 等人，2016 年 1 月。

[8]Joseph Redmon 于 2015 年至 2018 年利用 [YOLO](https://arxiv.org/abs/1506.02640) 、[约洛夫 2](https://arxiv.org/abs/1612.08242) 和现在的[约洛夫 3](https://arxiv.org/abs/1804.02767) 进行实时物体检测

申请课程:[https://www.appliedaicourse.com/](https://www.appliedaicourse.com/)

感谢您的阅读！这是我的 LinkedIn 和 Kaggle 简介