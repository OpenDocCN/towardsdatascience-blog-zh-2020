# PyTorch 图层尺寸:让您的图层每次都能正常工作(完整指南)

> 原文：<https://towardsdatascience.com/pytorch-layer-dimensions-what-sizes-should-they-be-and-why-4265a41e01fd?source=collection_archive---------1----------------------->

## 第一次，每一次，让你的层平滑地适合。PyTorch 中的张量和层维度入门指南。

![](img/f15ead30a9820d47330619dcbf17774e.png)

用这些宝贵的知识，第一次就能让你的衣服层很好的贴合。—[Clark Van Der Beken](https://unsplash.com/@snaps_by_clark?utm_source=medium&utm_medium=referral)在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄的照片

**前言**

*本文涵盖了定义张量、在 PyTorch 中正确初始化神经网络层等等！(原标题为* [PyTorch 图层尺寸:什么尺寸，为什么？](/pytorch-layer-dimensions-what-sizes-should-they-be-and-why-4265a41e01fd?source=your_stories_page-------------------------------------))

# **简介**

你可能会问:“我如何在 PyTorch 中初始化我的层尺寸而不被吼？”这一切都只是试错吗？不，真的…它们应该是什么？首先，您是否知道第一个**和第二个*需要一个`torch.nn.Conv2d`层的参数，而一个`torch.nn.Linear`层需要完全不同方面的完全相同的张量数据？如果你不知道这些，请继续阅读。*

***例 1:相同，相同，但不同。***

> **构造卷积图层和线性图层在语法上是相似的，但是尽管能够对完全相同的输入数据进行操作(尽管数据的大小应该不同)，但参数并不期望得到相似的东西。**

```
***# The __init__ method of a nn.Module class:**...def __init__(self):
"""Initialize neural net layers.""" super(Net, self).__init__()

    # Intialize my 2 layers here: self.conv = **nn.Conv2d(1, 20, 3)** *# Give me* ***depth*** *of input.*self.dense = **nn.Linear(2048, 10)** *# Give me* ***features*** *of input.*...*
```

*在将数据集放入某些网络层之前，您需要了解 PyTorch 模型如何使用数据。*

# ***第一课:如何在 PyTorch 中读取张量大小***

*下面是 PyTorch 中遇到的一些常见张量大小以及何时使用它们的典型示例。知道你在看什么很重要，因为它们的结构并不像你希望的那样可预测(这种有些不直观的设计选择主要是为了性能利益而实现的，*这是*好的… *我猜是*)。*

*当把张量输入卷积或线性层*(虽然* *不是 RNN 的)*时，我们有一个心理锚，那就是第一维总是**批量** **大小(N)** 。然而，剩下的维度取决于月亮的相位和你所在地区月亮升起时蟋蟀的间歇频率。开玩笑，没那么简单。你必须死记硬背。所以开始*旋转(下面的例子 2)。**

*![](img/b3d9557d5b2c973f8fc95c8a86f95b29.png)*

*吃他们说的红色药丸。他们说再深入一点。这个张量的第三维度应该是什么？！？—照片由 [Tim Gouw](https://unsplash.com/@punttim?utm_source=medium&utm_medium=referral) 在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄*

*了解 PyTorch 期望它的张量如何成形是很重要的——因为你可能非常满意你的 28 x 28 像素图像显示为 Torch 的张量。大小([28，28])。而 PyTorch 则认为你希望它查看你的 28 批 28 个特征向量。我只想说，在你学会如何用她的方式看待事情之前，你们不会成为朋友——所以，不要成为那样的人。研究你的张量维度！*

***例 PyTorch 喜欢的张量维数。***

```
***"""Example tensor size outputs, how PyTorch reads them, and where you encounter them in the wild.** *Note: the values below are only examples. Focus on the rank of the tensor (how many dimensions it has).****"""*****>>> torch.Size([32])**
 *#* ***1d:*** *[batch_size] 
    # use for target labels or predictions.***>>> torch.Size([12, 256])**
    *#* ***2d:*** *[batch_size, num_features (aka: C * H * W)]
    # use for* ***nn.Linear****() input.***>>> torch.Size([10, 1, 2048])**
   *#* ***3d:*** *[batch_size, channels, num_features (aka: H * W)]
    # when used as* ***nn.Conv1d****() input.
    # (but [seq_len, batch_size, num_features]
    # if feeding an* ***RNN****).***>>> torch.Size([16, 3, 28, 28])**
    *#* ***4d:*** *[batch_size, channels, height, width]
    # use for* ***nn.Conv2d()*** *input.***>>>  torch.Size([32, 1, 5, 15, 15])**
    *#* ***5D:*** *[batch_size, channels, depth, height, width]
    # use for* ***nn.Conv3d****() input.**
```

> *注意到 **Conv2d 层想要一个 4d 张量吗？1d 或 3d 图层怎么样？***

*因此，如果您想将一幅灰度为 28 x 28 像素的图像加载到 Conv2d 网络图层中，请在上面的示例中找到图层类型。因为它需要一个 4d 张量，而你已经有一个具有高度和宽度的 2d 张量，只需添加 batch_size 和 channels *(参见下面 channels 的经验法则)*来填充额外的维度，就像这样:[1，1，28，28]。那样的话，你和 PyTorch 可以和好如初。*

# ***第二课:初始化 torch.nn.Conv2d 层***

*文档描述了这样一个 Conv2d 层:*

```
*"""
*Class*torch.nn.Conv2d(***in_channels***, ***out_channels***, *kernel_size*, *stride=1*, *padding=0*, *dilation=1*, *groups=1*, *bias=True*, *padding_mode='zeros'*)*Parameters****in_channels***(int) - Number of channels in the input image
***out_channels*** (int) - Number of channels produced by the convolution
"""*
```

*还记得你的输入通道是哪个维度吗？如果你忘了，请看第一课。只需在第一个卷积层中使用该数字作为您的`in_channels`参数。*搞定。**

> ***第一个 Conv2d 层上“in_channels”的经验法则:***
> 
> *—如果您的图像是黑白的，则为 1 通道。(您可以通过在 dataloader 的 transforms 参数中运行`transforms.Grayscale(1)`来确保这一点。)*
> 
> *—如果您的图像是彩色的，则它是 3 个通道(RGB)。*
> 
> *—如果有 alpha(透明)通道，则它有 4 个通道。*

*这意味着对于您的第一个 Conv2d 层，即使您的图像大小很大，如 1080 像素乘 1080 像素，您的`in_channels`通常会是 1 或 3。*

****注意:*** *如果你用一些随机产生的张量测试这个，它仍然对你呕吐，你现在对着你的电脑大喊，深呼吸。没关系。确保它有正确的尺寸。你有没有* `*unsqueeze()*` *这个张量？Pytorch 想要批量。*[*unsqueeze()*](https://pytorch.org/docs/stable/torch.html#torch.unsqueeze)*函数将添加一个表示批量为 1 的维度 1。**

## *但是，out_channels 呢？*

*你说的`out_channels`怎么样？你希望你的人际网络有多深，这是你的选择。基本上，Pytorch 定义的您的`out_channels`维度是:*

> ***out _ channels**([*int*](https://docs.python.org/3/library/functions.html#int))—卷积产生的通道数*

*对于您使用的每个卷积核，当通过该层时，您的输出张量变得更深一个通道。如果你想要大量的内核，把这个数字设得高一些，比如 121，如果你只想要一些，把这个数字设得低一些，比如 8 或者 12。您在这里选择的任何数字都将是下一个卷积层*的`channels_in`值，以此类推。**

> **注意:`kernel_size`的值是自定义的，虽然很重要，但不会导致令人头疼的错误，因此在本教程中省略。取一个奇数即可，通常在 3-11 之间，但大小可能因应用程序而异。**

**通常，网络前半部分的卷积层越来越深，而网络末端的全连接(又名:线性或密集)层越来越小。这里有一个来自 [60 分钟初学者闪电战](https://pytorch.org/tutorials/beginner/blitz/neural_networks_tutorial.html)的有效例子(注意`self.conv1`的*出频道*变成了`self.conv2`的*入频道*):**

```
****class** **Net**(nn**.**Module):

    **def** __init__(self):
        super(Net, self)**.**__init__()
        *# 1 input image channel, 6 output channels, 3x3 square convolution*
        *# kernel*
        self**.**conv1 **=** nn**.**Conv2d(1, **6**, 3)
        self**.**conv2 **=** nn**.**Conv2d(**6**, 16, 3) *# an affine operation: y = Wx + b*
        self**.**fc1 **=** nn**.**Linear(16 ***** 6 ***** 6, 120)  *# 6*6 from image dimension*
        self**.**fc2 **=** nn**.**Linear(120, 84)
        self**.**fc3 **=** nn**.**Linear(84, 10)**
```

**现在来说说全连接层。**

# ****第三课:完全连接(torch.nn.Linear)图层****

**[线性层的文档](https://pytorch.org/docs/stable/nn.html#linear)告诉我们以下内容:**

```
****"""** *Class*  
torch.nn.Linear(***in_features***, ***out_features***, *bias=True*)*Parameters***in_features** – size of each input sample
**out_features** – size of each output sample"""**
```

**我知道这些看起来很相似，但是不要混淆:“`in_features`”和“`in_channels`”是完全不同的，初学者经常混淆它们，认为它们是相同的属性。**

```
****# Asks for in_channels, out_channels,** kernel_size, etc
self.conv1 = nn.Conv2d(1, 20, 3)**# Asks for in_features, out_features**
self.fc1 = nn.Linear(2048, 10)**
```

## ****计算尺寸。****

**对于所有的`nn.Linear`层网络，有两个特别重要的论点，无论你的网络有多深，你都应该知道。第**个参数**和第**个参数**。你有多少个完全连接的层在中间并不重要，这些维度很容易，你很快就会看到。**

**如果你想把你的 28 x 28 的图像传入一个线性层，你必须知道两件事:**

1.  ****您的 28 x 28 像素图像不能作为[28，28]张量输入。**这是因为`nn.Linear` 会将其读取为 28 批 28 个特征长度的向量。既然它期望一个`[batch_size, num_features]`的输入，你必须以某种方式转置它(见下面的*视图()**)。***
2.  ****您的批次大小不变地通过您的所有层。**不管你的数据在网络中如何变化，你的第一维最终将是你的`batch_size`，即使你从未在网络模块的定义中看到这个数字。**

> ****使用** [**视图**](https://pytorch.org/docs/stable/tensors.html#torch.Tensor.view) **()来改变你的张量的维度。****
> 
> **`image = image.view(**batch_size**, -1)`**
> 
> **您提供您的 batch_size 作为第一个数字，然后“-1”基本上告诉 Pytorch，“您为我算出另一个数字…请。”你的张量现在将正确地馈入任何线性层。现在我们正在谈话！**

**因此，要初始化线性图层的第一个参数**,请向其传递输入数据的要素数量。对于 28×28，我们的新视图张量大小为[1，784] (1 * 28 * 28):****

****示例 3:使用视图调整大小()以适合线性图层****

```
**batch_size = 1# Simulate a 28 x 28 pixel, grayscale "image"
input = torch.randn(1, 28, 28)# Use view() to get [batch_size, num_features].
# -1 calculates the missing value given the other dim.
input = input.view(batch_size, -1) # torch.Size([1, **784**])# Intialize the linear layer.
fc = torch.nn.Linear(**784**, 10)# Pass in the simulated image to the layer.
output = fc(input)print(output.shape)
>>> torch.Size([1, 10])**
```

> **R **记住这个**——如果你曾经从卷积层输出转换到线性层输入，你必须使用 view 将其从 4d 调整到 2d，如上面的图像示例所述。**
> 
> **所以，[32，21，50，50]的 conv 输出应该被“展平”成为[32，21 ***** 50 ***** 50]张量。并且线性图层的 in_features 也要设置为[21 ***** 50 ***** 50]。**

**线性层的第二个参数**如果你把它传递给更多的层，被称为隐藏层的 **H** 。你只需要和 H 打定位乒乓球，让它成为上一个的最后一个和下一个的第一个，就像这样:****

```
****"""The in-between dimensions are the hidden layer dimensions, you just pass in the last of the previous as the first of the next."""**fc1 = torch.nn.Linear(784, 100) *# 100 is last.*
fc2 = torch.nn.Linear(100, 50) *# 100 is first, 50 is last.*
fc3 = torch.nn.Linear(50, 20) *# 50 is first, 20 is last.*
fc4 = torch.nn.Linear(20, 10) *# 20 is first.* *"""This is the same pattern for convolutional layers as well, only it's channels, and not features that get passed along."""***
```

****最后的输出**，也就是你的**输出层**取决于你的模型和你的损失函数。如果你有 10 个类，像在 MNIST，你正在做一个分类问题，你希望你的所有网络架构最终合并成最后的 10 个单元，这样你就可以确定你的输入预测的是这 10 个类中的哪一个。**

**最后一层取决于您想从数据中推断出什么。为了得到您需要的答案，您可以做的操作是另一篇文章的主题，因为有很多内容需要讨论。但是现在你应该已经掌握了所有的基础知识。**

****就是这样！****

**你现在应该能够建立一个网络，而不用挠头或者被翻译吼了。请记住，您的 batch_size 或 dim 0 始终是相同的。卷积图层关注深度(通道)，线性图层关注要素数量。并学习如何阅读那些张量！**

**请留下评论，或者分享这篇文章，如果你喜欢它，并发现它很有帮助！**