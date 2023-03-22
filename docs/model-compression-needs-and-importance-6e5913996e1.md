# 模型压缩:需求和重要性

> 原文：<https://towardsdatascience.com/model-compression-needs-and-importance-6e5913996e1?source=collection_archive---------21----------------------->

## 了解深度学习的不同模型压缩技术的需求和优势

![](img/b615eaaa470690c7687e379f71dbb187.png)

物联网连接的智慧城市(图片来自 [Pixabay](https://pixabay.com/?utm_source=link-attribution&utm_medium=referral&utm_campaign=image&utm_content=4317139) 的 [Tumisu](https://pixabay.com/users/Tumisu-148124/?utm_source=link-attribution&utm_medium=referral&utm_campaign=image&utm_content=4317139)

无论你是计算机视觉新手还是专家，你都可能听说过 AlexNet 在 2012 年赢得了 ImageNet 挑战赛。那是计算机视觉历史上的转折点，因为它表明深度学习模型可以以前所未有的准确度执行被认为对计算机来说非常困难的任务。

> 但是你知道 AlexNet 有 6200 万个可训练参数吗？

有趣的权利。

2014 年推出的另一个流行模型 VGGNet 甚至有更多，1.38 亿个可训练参数。

> 那是 AlexNet 的 2 倍多。

你可能会想…我知道模型越深入，它的表现就越好。那么为什么要突出参数的数量呢？网络越深，显然参数会越多。

![](img/f621270d0fadc4ac47eefe9714698442.png)

已知卷积神经网络(CNN)网络的复杂性和准确性。**#参数和#MACCs 在上表中按百万顺序排列** [8]

当然，这些深度模型已经成为计算机视觉行业的基准。但是当你想创建一个真实世界的应用时，你会选择这些模型吗？

我想我们在这里应该问的真正问题是:**你能在你的应用程序中使用*这些*模型吗？**

暂时不要想这个问题！

在得到答案之前，让我在这里转移一下话题。*(不过可以随意跳到最后。)*

到 2030 年，物联网设备的数量预计将达到 1250-5000 亿台，假设其中 20%将拥有摄像头，带摄像头的物联网设备将是 130-1000 亿台的市场。[9,10,11]

物联网摄像头设备包括家庭安全摄像头(如 Amazon Ring 和 Google Nest ),当你到家时会打开门，或者如果它看到陌生人会通知你，智能车辆上的摄像头会帮助你驾驶，或者停车场的摄像头会在你进出时打开大门，等等！其中一些物联网设备已经在某种程度上使用人工智能，其他设备正在慢慢赶上。

![](img/7834b9a44b474a42a5e0652dbfe08e6d.png)

与物联网设备连接的智能家居系统(图片来自 [Pixabay](https://pixabay.com/?utm_source=link-attribution&utm_medium=referral&utm_campaign=image&utm_content=3872063) 的 [Gerd Altmann](https://pixabay.com/users/geralt-9301/?utm_source=link-attribution&utm_medium=referral&utm_campaign=image&utm_content=3872063)

许多现实世界的应用需要实时的设备处理能力。自动驾驶汽车就是一个很好的例子。为了让汽车在任何道路上安全行驶，它们必须实时观察道路，如果有人走在汽车前面，它们就必须停下来。在这种情况下，需要在设备上实时处理视觉信息并做出决策。

那么，回到之前的问题:**你能在你的应用中使用*这些*模型吗？**

如果您正在使用计算机视觉，那么您的应用很有可能需要物联网设备，从物联网设备的预测来看，您的公司很不错。

主要挑战是物联网设备资源受限；它们的内存有限，计算能力较低。模型中可训练的参数越多，它的规模就越大。深度学习模型的推理时间随着可训练参数数量的增加而增加。此外，与具有较少参数的较小网络相比，具有较高参数的模型需要更多的能量和空间。最终结果是，当模型规模很大时，很难在资源受限的设备上部署。虽然这些模型在实验室中取得了巨大的成功，但它们在许多现实应用中并不可用。

在实验室中，您拥有昂贵的高速 GPU 来获得这种级别的性能[1]，但当您在现实世界中部署时，成本、功耗、散热和其他问题会阻碍**“加大投入”**策略。

在云上部署深度学习模型是一种选择，因为它可以提供高计算和存储可用性。然而，由于网络延迟，它的响应时间很短，这在许多实时应用程序中是不可接受的(不要让我开始谈论网络连接对整体可靠性或隐私的影响！).

简而言之，AI 需要靠近数据源进行处理，最好是在物联网设备本身上！

这给我们留下了一个选择:**减小模型的尺寸。**

制造能够在边缘设备的限制下运行的较小模型是一个关键的挑战。这也不会影响精度。仅仅有一个可以在资源受限的设备上运行的小模型是不够的。它应该表现良好，无论是在准确性还是推理速度方面。

那么，如何在有限的设备上安装这些模型呢？如何让它们在现实应用中可用？

这里有一些技术可以用来减少模型的大小，以便您可以将它们部署在您的物联网设备上。

## **修剪**

修剪通过删除对性能不敏感的冗余、不重要的连接来减少参数的数量。这不仅有助于减少整个模型的大小，而且节省了计算时间和能量。

![](img/58ccb2e22bd0788f26fed66723cb515f.png)

修剪([来源](https://software.intel.com/content/www/us/en/develop/articles/compression-and-acceleration-of-high-dimensional-neural-networks.html)

**优点:**

*   可以在训练期间或训练后使用
*   可以改善给定架构的推理时间/模型大小与准确性的权衡[12]
*   可以应用于卷积层和全连接层

**缺点:**

*   一般来说，没有切换到更好的架构更有帮助[12]
*   受益于延迟的实现很少，因为 TensorFlow 只带来模型大小的好处

![](img/070096cb5517fb89446218b48f8b1e76.png)

原始模型和修剪模型的速度和大小权衡[13]

## **量化**

在 DNN，权重存储为 32 位浮点数。量化是通过减少位数来表示这些权重的思想。权重可以量化为 16 位、8 位、4 位甚至 1 位。通过减少使用的比特数，深度神经网络的规模可以显著减小。

![](img/72802bc16c01bd8c57c0d074f3b19f5c.png)

二进制量化([源](https://software.intel.com/content/www/us/en/develop/articles/compression-and-acceleration-of-high-dimensional-neural-networks.html))

**优点:**

*   量化可以在训练期间和之后应用
*   可以应用于卷积层和全连接层

**缺点:**

*   量化的权重使得神经网络更难收敛。需要较小的学习速率来确保网络具有良好的性能。[13]
*   量化的权重使得反向传播不可行，因为梯度不能通过离散的神经元反向传播。需要近似方法来估计损失函数相对于离散神经元输入的梯度[13]
*   TensorFlow 的量化感知训练本身在训练过程中不会进行任何量化。仅在训练期间收集统计数据，这些数据用于量化训练后的数据。所以我不确定以上几点是否应该被列为缺点

## **知识升华**

在知识提炼中，大型复杂模型在大型数据集上进行训练。当这个大型模型可以对看不见的数据进行归纳并表现良好时，它就被转移到一个较小的网络中。较大的模型也称为教师模型，较小的网络也称为学生网络。

![](img/338a1a2ef0579b20a61deef9c5d19b3d.png)

知识蒸馏([来源](/knowledge-distillation-simplified-dd4973dbc764))

**优点:**

*   如果你有一个预先训练的教师网络，训练较小的(学生)网络所需的训练数据较少。
*   如果你有一个预先培训过的教师网络，较小的(学生)网络的培训会更快。
*   可以缩小网络规模，而不管教师和学生网络之间的结构差异。

**缺点:**

*   如果您没有经过预先培训的教师网络，可能需要更大的数据集，并且需要更多的时间来训练它。

## **选择性注意**

选择性注意是指关注感兴趣的物体或元素，而忽略其他物体(通常是背景或其他与任务无关的物体)。它的灵感来自于人眼的生物学。我们看东西的时候，一次只关注一个或者几个物体，其他区域就模糊掉了。

![](img/c63ceb46d50e9135ef4e4541875deaa4.png)

选择性注意([来源](http://www.xailient.com))

这需要在你现有的人工智能系统的上游添加一个选择性注意力网络，或者如果它服务于你的目的，单独使用它。这取决于你试图解决的问题。

**优点:**

*   更快的推理
*   较小的型号(例如只有 44 KB 的人脸检测器和裁剪器！)
*   精度增益(通过将下游 AI 仅聚焦在感兴趣的区域/对象上)

**缺点:**

*   仅支持从头开始培训

## **低秩因子分解**

使用矩阵/张量分解来估计信息参数。具有 m×n 维且秩为 r 的权重矩阵 A 被更小维的矩阵代替。这项技术有助于将一个大矩阵分解成更小的矩阵。

![](img/f7cff3a67cca0564b75fa335d54aabc1.png)

低秩因子分解([来源](https://www.researchgate.net/figure/Diagram-of-the-low-rank-factorized-DNN_fig1_269295146))

**优点:**

*   可以在训练期间或训练后使用
*   可以应用于卷积层和全连接层
*   当在训练期间应用时，可以减少训练时间

最棒的是，以上所有技术都是相辅相成的。它们可以原样应用，或者与一种或多种技术结合使用。使用三级流水线；修剪、量化和霍夫曼编码为了减小预训练模型的大小，在 ImageNet 数据集上训练的 VGG16 模型从 550 MB 减小到 11.3 MB。

上面讨论的大多数技术都可以应用于预先训练的模型，作为减少模型大小和提高推理速度的后处理步骤。但是它们也可以在训练期间使用。量化越来越受欢迎，现在已经被纳入机器学习框架。我们可以期待修剪很快会成为流行的框架。

在本文中，我们研究了将基于深度学习的模型部署到资源受限设备(如物联网设备)的动机，以及减少模型大小的需求，以便它们在不牺牲准确性的情况下适合。我们还讨论了一些压缩深度学习模型的现代技术的利弊。最后，我们谈到了每种技术既可以单独使用，也可以结合使用。

确保在培训后和培训期间探索适合你的模特的所有技巧，并找出最适合你的方法。

*哪种模型压缩技术对你最有效？* ***下面留下评论。***

*想训练自己的* ***选择性注意网络*** *？* [*点击这里*](http://www.console.xailient.com/) *。*

原载于*[*www.xailient.com/blog*](https://www.xailient.com/post/model-compression-needs-and-importance)*。**

****关于作者****

**Sabina Pokhrel 在*[*xai lient*](http://www.xailient.com)*工作，这是一家计算机视觉初创公司，已经建造了世界上最快的边缘优化物体探测器。**

***参考文献:***

1.  *[https://towards data science . com/machine-learning-models-compression-and-quantization-simplified-a 302 ddf 326 f 2](/machine-learning-models-compression-and-quantization-simplified-a302ddf326f2)*
2.  *c .布西卢、r .卡鲁阿纳和 a .尼古列斯库-米齐尔(2006 年 8 月)。模型压缩。第 12 届 ACM SIGKDD 知识发现和数据挖掘国际会议论文集*(第 535–541 页)。**
3.  *程，王，周平，张，等(2017)。深度神经网络的模型压缩和加速综述。 *arXiv 预印本 arXiv:1710.09282* 。*
4.  *[http://mitch Gordon . me/machine/learning/2020/01/13/do-we-really-need-model-compression . html](http://mitchgordon.me/machine/learning/2020/01/13/do-we-really-need-model-compression.html)*
5.  *[https://software . Intel . com/content/www/us/en/develop/articles/compression-and-acceleration-of-high-dimensional-neural-networks . html](https://software.intel.com/content/www/us/en/develop/articles/compression-and-acceleration-of-high-dimensional-neural-networks.html)*
6.  *[https://towards data science . com/the-w3h-of-Alex net-vggnet-resnet-and-inception-7 baaaecccc 96](/the-w3h-of-alexnet-vggnet-resnet-and-inception-7baaaecccc96)*
7.  *[https://www . learnopencv . com/number-of-parameters-and-tensor-sizes-in-convolutionary-neural-network/](https://www.learnopencv.com/number-of-parameters-and-tensor-sizes-in-convolutional-neural-network/)*
8.  *Véstias 议员(2019 年)。基于可重构计算的边缘卷积神经网络综述。*算法*， *12* (8)，154。*
9.  *[https://technology . informa . com/596542/IHS-markit 称，到 2030 年，物联网设备的数量将激增至 1250 亿台](https://technology.informa.com/596542/number-of-connected-iot-devices-will-surge-to-125-billion-by-2030-ihs-markit-says)*
10.  *[https://www . Cisco . com/c/dam/en/us/products/parallels/se/internet-of-things/at-a-glance-c45-731471 . pdf](https://www.cisco.com/c/dam/en/us/products/collateral/se/internet-of-things/at-a-glance-c45-731471.pdf)*
11.  *莫汉，a .，高恩，k .，卢耀辉，李伟伟，陈，X. (2017 年 5 月)。2030 年的视频物联网:一个有很多摄像头的世界。在 *2017 IEEE 国际电路与系统研讨会(ISCAS)* (第 1–4 页)。IEEE。*
12.  *Blalock，d .，Ortiz，J. J. G .，Frankle，j .，& Guttag，J. (2020)。神经网络剪枝是什么状态？。arXiv 预印本 arXiv:2003.03033*
13.  *郭，于(2018)。量化神经网络方法和理论综述。 *arXiv 预印本 arXiv:1808.04752* 。*