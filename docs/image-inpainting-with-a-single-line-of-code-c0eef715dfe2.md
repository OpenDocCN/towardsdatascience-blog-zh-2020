# 用一行代码修复图像

> 原文：<https://towardsdatascience.com/image-inpainting-with-a-single-line-of-code-c0eef715dfe2?source=collection_archive---------25----------------------->

## 使用 Python 中的 OpenCV，使用四种不同的技术和一行代码，使用图像修复来恢复图像。

![](img/ecfaa08b40bf31a63eacb23e65d5a5db.png)

杰德·斯蒂芬斯在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上的照片

修复是一个过程，其中艺术品的损坏或缺失部分被填充以完成它。使用 GANs 来执行图像修复的现有技术方法，其中复杂模型在大量数据上被训练。如果呈现的数据完全不同，那么模型也可能受到影响。与此相反，图像修复也可以通过图像处理操作来完成，这也只需要使用 OpenCV 的一行代码。在本文中，我们将实现四种不同类型的图像修复方法，并基于 PSNR 和 SSIM 比较它们的结果。此外，我将在本文中遵循一种不同的方法，即首先呈现代码和结果，然后描述方法。

我们将使用三种算法，即基于 Navier-Stokes 的算法、Alexandru Telea 基于快速行进法的方法以及快速频率选择性重建算法的快速和最佳迭代。

# 目录

*   要求
*   密码
*   结果
*   方法概述

# 要求

其中两种方法需要 OpenCV Contrib，以便可以使用 pip 安装:

```
pip install opencv-contrib-python==4.3.0.36
```

# 密码

我们需要扭曲的图像和需要修补的蒙版。**对于 Navier-Stokes 和 Telea 方法，蒙版上的白色部分代表要修复的区域，而对于 FSR 方法，黑色像素则是在**修复的像素。

图像修复所需的单行代码是:

```
import cv2#distorted_img: The image on which inpainting has to be done.
#mask: Black mask with white pixels to be inpainted*res_NS = cv2.inpaint(distort_img, mask, 3, cv2.INPAINT_NS)
res_TELEA = cv2.inpaint(distort_img, mask, 3, cv2.INPAINT_TELEA)*res_FSRFAST = distorted_img.copy()
res_FSRBEST = distorted_img.copy()
mask1 = cv2.bitwise_not(mask)*cv2.xphoto.inpaint(distort, mask1, res_FSRFAST,        cv2.xphoto.INPAINT_FSR_FAST)
cv2.xphoto.inpaint(distort, mask1, res_FSTBEST, cv2.xphoto.INPAINT_FSR_BEST)*
```

# 结果

我用了三张大小为 756x1008 的图片。所有四种方法的结果如下所示。

![](img/9c9d0992e54051545f7dcd1ed49157c1.png)

结果 1

![](img/517f4bbcc61ae07f6287c0194ae5de64.png)

结果 2

![](img/8aaf5e714c4beb06959da28efbec6cfd.png)

结果 3

每幅图像的每种方法所用的时间(秒)为:

![](img/3ee9e4cfe4bf008a426c2730b438f2fb.png)

花费的时间

PSNR 和 SSIM 的值也是使用以下代码根据重建图像与原始图像进行计算的:

```
import cv2
from skimage.metrics import structural_similarity as SSIMpsnr = cv2.PSNR(img, recon, 255)
ssim = SSIM(img, recon, multichannel=True)
```

结果是:

![](img/ffd9a969dd9246071d03d5acee877334.png)

PSNR 价值观

![](img/2cbb046daaf84f923906b0548f6386bd.png)

SSIM 价值观

此外，Genser 等人发表了一篇[论文](https://github.com/opencv/opencv_contrib/files/3730212/inpainting_comparison.pdf)，对这些方法进行了比较，其中他们使用了 [Kodak](http://www.cs.albany.edu/~xypan/research/snr/Kodak.html) 和 [Tecnick](https://testimages.org/) 图像集以及五种不同的错误屏蔽，其结果如下所示:

![](img/54ea30ed0ed42448967fcf615633b749.png)

结果

下面给出了使用的完整代码，其中显示了一个样本遮罩，其中白色区域必须在原始图像上进行修复，并叠加在原始图像上。

![](img/08b517e3be09ed07a1092489b005d6f8.png)

面具

# 方法概述

## 泰莱亚

Alexandru Telea 在 2004 年的论文“基于快速行进法的图像修复技术”中介绍了这种技术。我在这里引用的 OpenCV 文档对此做了很好的概述。

> 考虑图像中要修补的区域。算法从该区域的边界开始，进入该区域，首先逐渐填充边界中的所有内容。需要在要修复的邻域的像素周围有一个小的邻域。该像素被邻域中所有已知像素的归一化加权和所代替。砝码的选择是一件重要的事情。对那些靠近该点、靠近边界法线的像素和那些位于边界轮廓上的像素给予更大的权重。一旦像素被修复，它就使用快速行进方法移动到下一个最近的像素。FMM 确保已知像素附近的像素首先被修复，因此它就像一个手动启发式操作。

## 纳维尔-斯托克斯

由 Bertalmio 等人在他们的论文中介绍，“[纳维尔-斯托克斯，流体动力学，以及图像和视频修复](https://www.math.ucla.edu/~bertozzi/papers/cvpr01.pdf)”早在 2001 年就介绍过了。再次引用 OpenCV 文档:

> 该算法基于流体动力学并利用偏微分方程。基本原理是启发式的。它首先沿着边缘从已知区域行进到未知区域(因为边缘应该是连续的)。它继续等照度线(连接具有相同强度的点的线，就像轮廓连接具有相同海拔的点)，同时在修复区域的边界匹配梯度向量。为此，使用了流体动力学的一些方法。一旦它们被获得，颜色被填充以减少该区域的最小变化。

## 快速频率选择性重建算法

使用已知的样本和已经重构的像素作为支持来外推失真块的信号。该算法迭代地产生信号的一般复值模型，该模型被外推为傅立叶基函数的加权叠加。FSR 的一个重要特征是计算是在傅立叶域中进行的，这导致了快速实现。

总之，FSR 的快速实施提供了速度和精度之间的巨大平衡，即使在高度失真的情况下，这些方法也能提供良好的结果，因此它们可以为数据密集型 GAN 提供合理的替代方案，后者在不可预见的数据上可能表现不佳。

# 参考

*   [https://docs . opencv . org/master/df/d3d/tutorial _ py _ inpainting . html](https://docs.opencv.org/master/df/d3d/tutorial_py_inpainting.html)
*   [https://docs . opencv . org/4 . 2 . 0/DC/d2f/tutorial _ x photo _ inpainting . html](https://docs.opencv.org/4.2.0/dc/d2f/tutorial_xphoto_inpainting.html)
*   [https://github . com/opencv/opencv _ contrib/files/3730212/inpainting _ comparison . pdf](https://github.com/opencv/opencv_contrib/files/3730212/inpainting_comparison.pdf)

如上所述，我在本文中使用了不同的策略。下面给出的一篇文章采用了传统的方法。如果能知道你更喜欢哪种策略就太好了。

[](/face-detection-models-which-to-use-and-why-d263e82c302c) [## 人脸检测模型:使用哪种模型，为什么？

### 一个关于用 Python 实现不同人脸检测模型的完整教程，通过比较，找出最好的…

towardsdatascience.com](/face-detection-models-which-to-use-and-why-d263e82c302c)