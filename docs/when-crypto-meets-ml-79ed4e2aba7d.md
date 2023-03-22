# 当密码遇到 ML

> 原文：<https://towardsdatascience.com/when-crypto-meets-ml-79ed4e2aba7d?source=collection_archive---------41----------------------->

## 对加密图像应用均值过滤器(使用完整的 Colab 笔记本)

![](img/be3839db42e219290812814960ffdae6.png)

杰森·黑眼在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上的照片

在这篇文章中，我们将看到如何使用同态加密对加密图像进行均值滤波[【1】](https://medium.com/privacy-preserving-natural-language-processing/homomorphic-encryption-for-beginners-a-practical-guide-part-1-b8f26d03a98a)[【2】](https://homepages.inf.ed.ac.uk/rbf/HIPR2/mean.htm)。为什么会有人这么做？嗯，只要你在本地进行处理，加密就没有意义，但如果你想使用在线服务，你确定要把你的**个人**图像交给**远程网络服务器**吗？如果你的答案是否定的，请继续阅读这篇文章。

## 为什么是均值滤波器？

均值滤波器用于创建模糊效果。在某些情况下，这种效果有助于消除图像中的噪点。有许多 web 服务允许您对图像应用均值过滤器[【3】](https://www10.lunapic.com/editor/?action=blur)[【4】](https://pinetools.com/blur-image)。所有这些服务都按照以下步骤工作:

1.  你把图像上传到网络服务器上
2.  在服务器上处理图像(应用均值滤波器)
3.  你下载服务器返回的经过处理的图像

这种方法的主要问题是**隐私**:你将**你的**个人图像交给一个你一无所知的远程服务器！为了解决这个问题，我们在上述过程中增加了两个步骤:

1.  您使用您的公钥加密图像
2.  你把加密图像上传到网络服务器上
3.  服务器处理加密的图像
4.  你下载服务器返回的加密结果
5.  解密你下载的东西

图像在服务器上的所有时间都是用你的**公钥**加密的。为了让服务器能够解密它，它需要你的**私钥**，只有你知道😎

## 幕后

为了加密图像，我们将使用 Paillier 密码系统[【5】](https://medium.com/coinmonks/paillier-shines-a-light-on-a-new-world-of-computing-15c5aceed3ab)。该方案关于加法和常数乘法是同态的。这对派利尔计划意味着什么？
关于加法的同态是指，如果 *c1* 是 *m1* 的密文 *c2* 是 *m2* 的密文，那么 *c1*c2* 是 *m1+m2 的密文。* 关于常数乘法的同态是指，如果 *c* 是 *m* 的密文， *k* 是一个大家都知道的常数，那么 *c^k* 就是 *c*k.* 的加密由于除法就是乘法我们也可以用一个常数做*同态*除法。如果你想了解更多关于 Paillier 加密方案的信息，请查看这个漂亮的解释。
对于本帖，我们不需要该方案的所有细节，这已经在 phe 库中实施[【6】](https://pypi.org/project/phe/)。

## 主要思想

为了应用维度为 *kxk* 的均值滤波器，我们简单地用当前像素位于中心的 *kxk* 正方形中所有像素的均值替换每个当前像素。所以，均值滤波器的核心由一个和后面跟着一个常数值的除法组成( *k* ，大家都知道)。这听起来像是 Paillier 加密的完美应用😁
如果我们使用 Paillier 方案加密图像，我们可以直接计算加密像素的总和。然后，如果我们将加密的总和除以一个常数(这是滤波器的大小)，我们将得到像素的加密平均值。

服务器将永远不需要知道原始图像来应用均值滤波器。

如果你正在寻找一个完整的实现和进一步的解释， [**这里**](https://colab.research.google.com/drive/1zQnW2RJb8afhvEK3UV_TlkEygvlMNysP?usp=sharing) ，我已经为你准备了一个 Colab 笔记本，可以在你的浏览器上运行😉

## 参考

[1][https://medium . com/privacy-preserving-natural-language-processing/同态-encryption-for-初学者-a-practical-guide-part-1-b 8 f 26d 03 a 98 a](https://medium.com/privacy-preserving-natural-language-processing/homomorphic-encryption-for-beginners-a-practical-guide-part-1-b8f26d03a98a)

[2][https://homepages.inf.ed.ac.uk/rbf/HIPR2/mean.htm](https://homepages.inf.ed.ac.uk/rbf/HIPR2/mean.htm)

[3]https://www10.lunapic.com/editor/?action=blur

[4]https://pinetools.com/blur-image

[5][https://medium . com/coin monks/paillier-shines-a-light-on-a-new-world-of-computing-15c aceed 3 ab](https://medium.com/coinmonks/paillier-shines-a-light-on-a-new-world-of-computing-15c5aceed3ab)

[https://pypi.org/project/phe/](https://pypi.org/project/phe/)