# 用于图像处理的卷积神经网络——使用 Keras

> 原文：<https://towardsdatascience.com/convolution-neural-network-for-image-processing-using-keras-dc3429056306?source=collection_archive---------1----------------------->

## Python 中使用 CNN 的综合指南

![](img/429ac717c2a2190728ef4b409e35f628.png)

美国地质勘探局在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄的照片

# 什么是图像分类？

图像分类是根据图像的特征将图像分成不同类别的过程。特征可以是图像中的边缘、像素强度、像素值的变化等等。稍后，我们将尝试理解这些组件。现在让我们看看下面的图片(参见图 1)。这三幅图像属于同一个人，但是当在诸如图像颜色、面部位置、背景颜色、衬衫颜色等特征之间进行比较时，这三幅图像会有所不同。处理图像时最大的挑战是这些特征的不确定性。对于人眼来说，看起来都是一样的，但是，当转换为数据时，您可能不容易在这些图像中找到特定的模式。

![](img/3804a17613cbeb2789f251252f8aa2af.png)![](img/c516a8db84c64e986acfc77a4dbe42f3.png)![](img/2c1fc133cccadd8976f73825362899a6.png)

图一。分别说明了作者在 2014 年和 2019 年拍摄的肖像。

图像由称为像素的最小的不可分割的片段组成，并且每个像素具有通常称为像素强度的强度。每当我们研究数字图像时，它通常带有三个颜色通道，即红绿蓝通道，俗称“RGB”值。为什么是 RGB？因为已经看到这三者的组合可以产生所有可能的调色板。每当我们处理彩色图像时，图像由多个像素组成，每个像素由 RGB 通道的三个不同值组成。让我们编码并理解我们正在谈论的内容。

```
import cv2
import seaborn as sns
import matplotlib.pyplot as plt
%matplotlib inline
sns.set(color_codes=True)# Read the image
image = cv2.imread('Portrait-Image.png') #--imread() helps in loading an image into jupyter including its pixel valuesplt.imshow(cv2.cvtColor(image, cv2.COLOR_BGR2RGB))
# as opencv loads in BGR format by default, we want to show it in RGB.
plt.show()image.shape
```

image.shape 的输出是(450，428，3)。图像的形状是 450 x 428 x 3，其中 450 代表高度，428 代表宽度，3 代表颜色通道的数量。当我们说 450 x 428 时，这意味着数据中有 192，600 个像素，每个像素都有一个 R-G-B 值，因此有 3 个颜色通道。

```
image[0][0]
```

image [0][0]为我们提供了第一个像素的 R-G-B 值，分别为 231、233 和 243。

```
# Convert image to grayscale. The second argument in the following step is cv2.COLOR_BGR2GRAY, which converts colour image to grayscale.
gray = cv2.cvtColor(image, cv2.COLOR_BGR2GRAY)
print(“Original Image:”)plt.imshow(cv2.cvtColor(gray, cv2.COLOR_BGR2RGB))
# as opencv loads in BGR format by default, we want to show it in RGB.
plt.show()gray.shape
```

gray.shape 的输出为 450 x 428。我们现在看到的是一个由 192，600 多个像素组成的图像，但只包含一个通道。

当我们尝试将灰度图像中的像素值转换成表格形式时，我们观察到的就是这样的情况。

```
import numpy as np
data = np.array(gray)
flattened = data.flatten()flattened.shape
```

输出:(192600，)

我们有一个数组形式的所有 192，600 个像素的灰度值。

```
flattened
```

输出:数组([236，238，238，...，232，231，231]，dtype=uint8)。注意灰度值可以在 0 到 255 之间，0 表示黑色，255 表示白色。

现在，如果我们拍摄多个这样的图像，并尝试将它们标记为不同的个体，我们可以通过分析像素值并在其中寻找模式来做到这一点。然而，这里的挑战是由于背景、色阶、服装等。图像之间各不相同，仅通过分析像素值很难找到模式。因此，我们可能需要一种更先进的技术，可以检测这些边缘或找到面部不同特征的潜在模式，使用这些模式可以对这些图像进行标记或分类。这就是像 CNN 这样更先进的技术发挥作用的地方。

# CNN 是什么？

CNN 或卷积神经网络(CNN)是一类**深度学习神经网络**。简而言之，将 CNN 视为一种机器学习算法，它可以接受输入图像，为图像中的各个方面/对象分配重要性(可学习的权重和偏差)，并能够区分它们。

CNN 的工作原理是从图像中提取特征。任何 CNN 都包含以下内容:

1.  作为灰度图像的输入图层
2.  输出层是一个二进制或多类标签
3.  隐藏层由卷积层、ReLU(校正线性单位)层、汇集层和完全连接的神经网络组成

理解由多个神经元组成的 ANN 或人工神经网络不能从图像中提取特征是非常重要的。这就是卷积层和池层结合的地方。类似地，卷积和池层不能执行分类，因此我们需要一个完全连接的神经网络。

在我们深入了解这些概念之前，让我们试着分别理解这些单独的部分。

![](img/a3c295ed8b0b363563c5c4c328a91260.png)

图二。说明了 CNN 从输入到输出数据的过程。图片摘自幻灯片 12《卷积神经网络简介》(斯坦福大学，2018 年)

假设我们可以访问不同车辆的多个图像，每个图像都被标记为卡车、轿车、货车、自行车等。现在的想法是利用这些预先标记/分类的图像，开发一种机器学习算法，该算法能够接受新的车辆图像，并将其分类到正确的类别或标签中。现在，在我们开始构建神经网络之前，我们需要了解大多数图像在处理之前都被转换为灰度形式。

[](/are-your-coding-skills-good-enough-for-a-data-science-job-49af101457aa) [## 对于数据科学的工作，你的编码技能够好吗？

### 5 编码嗅探如果你在数据科学行业工作，你必须知道

towardsdatascience.com](/are-your-coding-skills-good-enough-for-a-data-science-job-49af101457aa) 

## 为什么是灰度而不是 RGB/彩色图像？

![](img/22f5bef286671cba1c5d67a287a0a16b.png)

图 3。图像的 RGB 颜色通道。图像鸣谢—萨哈，S. (2018)

我们之前讨论过，任何彩色图像都有三个通道，即红色、绿色和蓝色，如图 3 所示。有几种这样的颜色空间，如灰度、CMYK、HSV，图像可以存在于其中。

具有多个颜色通道的图像面临的挑战是，我们有大量的数据要处理，这使得该过程计算量很大。在其他世界中，可以把它想象成一个复杂的过程，其中神经网络或任何机器学习算法必须处理三种不同的数据(在这种情况下是 R-G-B 值),以提取图像的特征并将它们分类到适当的类别中。

CNN 的作用是将图像简化为一种更容易处理的形式，而不会丢失对良好预测至关重要的特征。当我们需要将算法扩展到大规模数据集时，这一点非常重要。

## 什么是卷积？

图 4。说明了如何对输入图像进行卷积以提取特征。学分。GIF via [GIPHY](https://giphy.com/gifs/blog-daniel-keypoints-i4NjAwytgIRDW)

我们知道训练数据由灰度图像组成，这些图像将作为卷积层的输入来提取特征。卷积层由一个或多个具有不同权重的核组成，用于从输入图像中提取特征。假设在上面的例子中，我们正在使用大小为 3 x 3 x 1 (x 1，因为我们在输入图像中有一个颜色通道)的内核(K)，权重如下所示。

```
Kernel/Filter, K = 
1  0  1
0  1  0
1  0  1
```

当我们基于核的权重在输入图像上滑动核(假设输入图像中的值是灰度强度)时，我们最终基于不同像素的周围/相邻像素值来计算不同像素的特征。例如，如下图 5 所示，当第一次对图像应用核时，我们在如下所示的卷积特征矩阵中得到等于 4 的特征值。

![](img/1fe0d5c1b728ff55b7637f75aedf809c.png)

图 5。说明了当内核应用于输入图像时卷积特征的值。该图像是上面图 4 中使用的 GIF 的快照。

如果我们仔细观察图 4，我们会看到内核在图像上移动了 9 次。这个过程叫做**大步前进。**当我们使用步长值为 1 ( **非步长**)的操作时，我们需要 9 次迭代来覆盖整个图像。***CNN 自己学习这些核的权重。*** 该操作的结果是一个特征图，它基本上从图像中检测特征，而不是查看每一个像素值。

> I 图像特征，如边缘和兴趣点，提供了图像内容的丰富信息。它们对应于图像中的局部区域，并且在图像分析的许多应用中是基本的:识别、匹配、重建等。图像特征产生两种不同类型的问题:图像中感兴趣区域的检测，通常是轮廓，以及图像中局部区域的描述，通常用于不同图像中的匹配(图像特征。(未注明)

## 让我们更深入地了解一下我们正在谈论的内容。

从图像中提取特征类似于检测图像中的边缘。我们可以使用 openCV 包来执行同样的操作。我们将声明一些矩阵，将它们应用于灰度图像，并尝试寻找边缘。你可以在这里找到关于[功能的更多信息。](https://docs.opencv.org/master/d4/d86/group__imgproc__filter.html)

```
# 3x3 array for edge detection
mat_y = np.array([[ -1, -2, -1], 
                   [ 0, 0, 0], 
                   [ 1, 2, 1]])
mat_x = np.array([[ -1, 0, 1], 
                   [ 0, 0, 0], 
                   [ 1, 2, 1]])

filtered_image = cv2.filter2D(gray, -1, mat_y)
plt.imshow(filtered_image, cmap='gray')filtered_image = cv2.filter2D(gray, -1, mat_x)
plt.imshow(filtered_image, cmap='gray')
```

![](img/467aa6b3b92767207d6b20faf574001b.png)![](img/13496851aa41d918c99deef7df3c727b.png)

图 6。对数据应用 filter2D 变换时，显示带有边缘的图像。请注意，这两个图像明显不同。当我们谈论卷积层和内核时，我们主要是想识别图像中的边缘。使用 CNN 时，matrix_x 和 matrix_y 值由网络自动确定。图片取自作者开发的 Jupyter 笔记本。

```
import torch
import torch.nn as nn
import torch.nn.functional as fn
```

如果您使用的是 windows，请安装以下软件—# conda install pytorch torch vision cudatoolkit = 10.2-c py torch 以使用 py torch。

```
import numpy as np
filter_vals = np.array([[-1, -1, 1, 2], [-1, -1, 1, 0], [-1, -1, 1, 1], [-1, -1, 1, 1]])
print(‘Filter shape: ‘, filter_vals.shape)# Neural network with one convolutional layer and four filters
class Net(nn.Module):

 def __init__(self, weight): #Declaring a constructor to initialize the class variables
 super(Net, self).__init__()
 # Initializes the weights of the convolutional layer to be the weights of the 4 defined filters
 k_height, k_width = weight.shape[2:]
 # Assumes there are 4 grayscale filters; We declare the CNN layer here. Size of the kernel equals size of the filter
 # Usually the Kernels are smaller in size
 self.conv = nn.Conv2d(1, 4, kernel_size=(k_height, k_width), bias=False)
 self.conv.weight = torch.nn.Parameter(weight)

 def forward(self, x):
 # Calculates the output of a convolutional layer pre- and post-activation
 conv_x = self.conv(x)
 activated_x = fn.relu(conv_x)# Returns both layers
 return conv_x, activated_x# Instantiate the model and set the weights
weight = torch.from_numpy(filters).unsqueeze(1).type(torch.FloatTensor)
model = Net(weight)# Print out the layer in the network
print(model)
```

我们创建可视化层，调用类对象，并在图像上显示四个内核的卷积的输出(Bonner，2019)。

```
def visualization_layer(layer, n_filters= 4):

    fig = plt.figure(figsize=(20, 20))

    for i in range(n_filters):
        ax = fig.add_subplot(1, n_filters, i+1, xticks=[], yticks=[])
        # Grab layer outputs
        ax.imshow(np.squeeze(layer[0,i].data.numpy()), cmap='gray')
        ax.set_title('Output %s' % str(i+1))
```

卷积运算。

```
#-----------------Display the Original Image------------------- 
plt.imshow(gray, cmap='gray')#-----------------Visualize all of the filters------------------
fig = plt.figure(figsize=(12, 6))
fig.subplots_adjust(left=0, right=1.5, bottom=0.8, top=1, hspace=0.05, wspace=0.05)for i in range(4):
    ax = fig.add_subplot(1, 4, i+1, xticks=[], yticks=[])
    ax.imshow(filters[i], cmap='gray')
    ax.set_title('Filter %s' % str(i+1))

# Convert the image into an input tensor
gray_img_tensor = torch.from_numpy(gray).unsqueeze(0).unsqueeze(1)
# print(type(gray_img_tensor))# print(gray_img_tensor)# Get the convolutional layer (pre and post activation)
conv_layer, activated_layer = model.forward(gray_img_tensor.float())# Visualize the output of a convolutional layer
visualization_layer(conv_layer)
```

输出:

![](img/fd7c91b16d59f2780bbd678741be7b9f.png)![](img/5b448cd940c7b9a257d2dfcc16c3b516.png)![](img/61478895b5052b0e04b33c70f981b44f.png)

图 7。说明了原始灰度图像上不同过滤器的输出。快照是使用 Jupyter Notebook 和上述代码生成的。

**注意:**根据与过滤器相关联的权重，从图像中检测特征。请注意，当图像通过卷积层时，它会尝试通过分析相邻像素强度的变化来识别特征。例如，图像的右上角始终具有相似的像素强度，因此没有检测到边缘。只有当像素改变强度时，边缘才可见。

## 为什么是 ReLU？

ReLU 或整流线性单元是应用激活函数来增加网络的非线性而不影响卷积层的感受野的过程。ReLU 允许更快地训练数据，而 Leaky ReLU 可以用来处理消失梯度的问题。其他一些激活函数包括泄漏 ReLU、随机化泄漏 ReLU、参数化 ReLU 指数线性单位(eLU)、缩放指数线性单位 Tanh、hardtanh、softtanh、softsign、softmax 和 softplus。

图 8。说明了 ReLU 的影响，以及它如何通过将负值转换为零来消除非线性。图片来源:GIF via Gfycat。

```
visualization_layer(activated_layer)
```

![](img/0816957db1bd0dc0e432b1686d397c01.png)

图 9。说明了 ReLU 激活后卷积层的输出。图片取自作者开发的 Jupyter 笔记本。

![](img/d8e28b5f4ec58316498b3508fc0c0df6.png)

图 10。阐释了内核如何处理具有 R-G-B 通道的图像。图像鸣谢—萨哈，S. (2018)

![](img/d27870258f09c65ec673dae10ee94112.png)

图 11。在 5 x 5 图像上应用 3 x 3 内核。图片致谢(Visin，2016)

卷积运算的一般目标是从图像中提取高级特征。在构建神经网络时，我们总是可以添加多个卷积层，其中第一个卷积层负责捕获梯度，而第二个卷积层捕获边缘。图层的添加取决于图像的复杂程度，因此添加多少图层没有什么神奇的数字。注意，应用 3 x 3 滤波器会在原始图像中产生 3 x 3 卷积特征，因此为了保持原始尺寸，通常会在图像两端填充值。

## 池层的作用

池层对通常被称为**激活图**的**卷积特征**应用非线性下采样。这主要是为了降低处理与图像相关的大量数据所需的计算复杂度。合用不是强制性的，通常是避免的。通常，有两种类型的池，**最大池，**返回池内核覆盖的图像部分的最大值，以及**平均池**平均池内核覆盖的值。下面的图 12 提供了一个不同的池技术如何工作的工作示例。

![](img/45247438582acbc0654e34690104a481.png)

图 12。说明了如何在激活图上执行最大和平均池。图片摘自幻灯片 18《卷积神经网络简介》(斯坦福大学，2018 年)

## 图像展平

一旦汇集完成，输出需要被转换成表格结构，人工神经网络可以使用该表格结构来执行分类。注意密集层的数量以及神经元的数量可以根据问题陈述而变化。此外，通常还会添加一个删除层，以防止算法过拟合。退出者在训练数据时会忽略一些激活图，但是在测试阶段会使用所有的激活图。它通过减少神经元之间的相关性来防止过度拟合。

![](img/e3732060f4ea36ac17571cf6dca97ac9.png)

图 13。展示了一个完整的 CNN，由输入图像、卷积层、汇集层、展平层、带有神经元的隐藏层和二进制输出层组成。这张图片是作者使用清晰图表制作的，可以在[这里](https://app.lucidchart.com/invitations/accept/bcbe2ab9-626c-4dfc-a5cf-de8c8c32554f)找到。

## 使用 CIFAR-10 数据集

下面的链接提供了一个使用 Keras 与 CNN 合作的端到端示例。

[](https://github.com/angeleastbengal/ML-Projects/blob/master/Understanding%20Image%20Processing%20%2B%20CNN.ipynb) [## angeleastbengal/ML-项目

### permalink dissolve GitHub 是超过 5000 万开发人员的家园，他们一起工作来托管和审查代码，管理…

github.com](https://github.com/angeleastbengal/ML-Projects/blob/master/Understanding%20Image%20Processing%20%2B%20CNN.ipynb) [](/decision-tree-classifier-and-cost-computation-pruning-using-python-b93a0985ea77) [## 使用 Python 实现决策树分类器和成本计算剪枝

### 使用成本计算修剪构建、可视化和微调决策树的完整实践指南…

towardsdatascience.com](/decision-tree-classifier-and-cost-computation-pruning-using-python-b93a0985ea77) [](https://neptune.ai/blog/what-image-processing-techniques-are-actually-used-in-the-ml-industry) [## ML 行业实际使用的图像处理技术有哪些？- neptune.ai

### 处理可用于提高图像质量，或帮助您从中提取有用的信息。这是…

海王星. ai](https://neptune.ai/blog/what-image-processing-techniques-are-actually-used-in-the-ml-industry) 

# 参考

1.  萨哈，S. (2018)。*卷积神经网络综合指南 ELI5 方式*。[在线]走向数据科学。可在:[https://towardsdatascience . com/a-comprehensive-guide-to-convolutionary-neural-networks-the-Eli 5-way-3bd 2 b 1164 a 53 获取。](/a-comprehensive-guide-to-convolutional-neural-networks-the-eli5-way-3bd2b1164a53.)
2.  ‌Image 特色。(未注明)。【在线】可在:[http://morpheo . inrialpes . fr/~ Boyer/Teaching/Mosig/feature . pdf .](http://morpheo.inrialpes.fr/~Boyer/Teaching/Mosig/feature.pdf.)
3.  ‌Stanford 大学(2018 年)。*卷积神经网络介绍*。[在线]可从以下网址获取:[https://web . Stanford . edu/class/cs 231 a/lections/intro _ CNN . pdf](https://web.stanford.edu/class/cs231a/lectures/intro_cnn.pdf.)
4.  弗兰切斯科·‌visin(2016)。*英语:卷积运算的一种变体的动画。蓝色贴图是输入，青色贴图是输出。*【在线】维基共享。可从以下网址获取:[https://commons . wikimedia . org/wiki/File:Convolution _ algorithm _-_ Same _ padding _ no _ strides . gif](https://commons.wikimedia.org/wiki/File:Convolution_arithmetic_-_Same_padding_no_strides.gif)【2020 年 8 月 20 日获取】。
5.  ‌Bonner 大学(2019 年)。*深度学习完全初学者指南:卷积神经网络*。【在线】中等。可从:[https://towardsdatascience . com/wtf-is-image-class ification-8e 78 a 8235 ACB 获取。](/wtf-is-image-classification-8e78a8235acb.)

*关于作者:高级分析专家和管理顾问，帮助公司通过对组织数据的商业、技术和数学的组合找到各种问题的解决方案。一个数据科学爱好者，在这里分享、学习、贡献；你可以和我在* [*上联系*](https://www.linkedin.com/in/angel-das-9532bb12a/) *和* [*上推特*](https://twitter.com/dasangel07_andy)*；*