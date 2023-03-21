# 这些机器学习术语你能回忆起多少？(在媒体上学习游戏化)

> 原文：<https://towardsdatascience.com/how-much-of-these-machine-learning-terms-can-you-recall-learning-on-medium-gamified-a280a53a0737?source=collection_archive---------52----------------------->

## 挑战你自己，而我试着制造中等的乐趣

# 介绍

机器学习是一个广泛的领域，包含各种其他子领域，如计算机视觉、自然语言处理、语音识别等等。

尽管机器学习有许多分支子领域，但一些关键术语在所有子领域中都是通用的。

本文介绍了一些常见的机器学习术语，并以游戏化的方式对这些术语进行了描述和解释。

## 通过阅读这篇文章，你可以获得以前看不到的机器学习词汇的知识，或者重温你在机器学习实践中可能遇到的术语

# **游戏规则**

1.  每一节都以单词开头，供您定义
2.  花一两分钟时间，确保你能够回忆起作品的定义
3.  请随意写下您的描述，以便与我的进行比较
4.  当你写下你对这些术语的定义时，向下滚动页面查看我对这些术语的定义和解释
5.  尽可能地学习，甚至在评论区随意添加一些你的定义。
6.  玩得开心
7.  欣赏来自 [Unsplash](https://unsplash.com/) 的图片

# 1.激活功能

*在向下滚动之前，思考或写出你对激活功能的描述。*

![](img/5ae1dcd92f58f42c9d0cf6aae63195be.png)

照片由 [Tachina Lee](https://unsplash.com/@chne_?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) 在 [Unsplash](/s/photos/thinking?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) 上拍摄

我们开始吧……

***3***

***2***

***1***

**…**

**激活函数**是将神经元的结果或信号转换成标准化输出的数学运算。

作为神经网络组件的激活函数的目的是在网络中引入非线性。包含激活函数使神经网络具有更大的表示能力和解决复杂的函数。

激活功能也被称为“*挤压功能*”。

常见激活功能的示例如下:

*   Sigmoid 函数
*   热卢
*   Softmax 函数

记住你的解释不一定要和我写的一样，只要你包括或记住了一些要点。

现在，你已经知道如何理解这篇文章了……从某种意义上来说，它就像抽认卡。

*下一个。*

# 2.整流线性单位

*这是你思考或写作的时间，不要太长……*

![](img/bd404efff74c61b0672d26f873cd0e13.png)

由[蒂姆·高](https://unsplash.com/@punttim?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)在 [Unsplash](/s/photos/thinking?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) 上拍摄的照片

***3***

***2***

***1***

**…**

一种转换神经元值结果的激活函数。

ReLU 对来自神经元的值施加的变换由公式 ***y=max(0，x)*** 表示。ReLU 激活函数将来自神经元的任何负值钳制为 0，而正值保持不变。

这种数学变换的结果被用作当前层的输出，并被用作神经网络内的连续层的输入。

在*消失梯度*的问题上，ReLU 是限制或避免消失梯度对神经网络的影响的标准解决方案。

如果你得到了前一个任期，那么这应该是在公园散步。

# 3.Softmax

如果不是，也没关系，花一两分钟考虑一下。

![](img/8e933ec624609b933617a3b338138423.png)

[Icons8 团队](https://unsplash.com/@icons8?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)在 [Unsplash](/s/photos/time?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) 上的照片

你现在知道该怎么做了。

**3**

***2***

***1***

**…**

一种激活函数，用于导出输入向量中一组数的概率分布。

softmax 激活函数的输出是一个向量，其中它的一组值表示一个类或事件发生的概率。向量中的值加起来都是 1。

如果你喜欢这篇文章的格式，请留下你的评论。

# 4.Glorot 统一初始化器

*这个有点难，放心踏出*

![](img/c0d9759472497c6bb58c99ec775ff992.png)

本杰明·戴维斯在 [Unsplash](/s/photos/thinking?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) 上拍摄的照片

*这么快就回来了…好了，我们走吧*

***3***

***2***

***1***

…

Glorot 统一初始化器是一种神经网络的权重初始化方法，用作解决神经网络内不稳定梯度的解决方案。

它根据某个**范围**内的值分布初始化网络的权重，平均值评估为零，方差恒定。分布的最大值是范围的正值，最小值是范围的负值。

***范围=【值，-值】***

用于确定分布范围的值来自以下公式:

***值= sqrt(6/fan _ in+fan _ out)***

*fan_in* 是输入到层的数目， *fan_out* 是层内神经元的数目。

更多关于纸张 [**这里**](http://proceedings.mlr.press/v9/glorot10a/glorot10a.pdf) **。**

# 4.损失函数

既然我们在中途，这里有一个简单的术语。

![](img/3e1101dde1e5405096a9bf30c1bbdbea.png)

照片由[布鲁斯·马尔斯](https://unsplash.com/@brucemars?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)在 [Unsplash](/s/photos/happy?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) 上拍摄

*告诉你这很容易*

***3***

***2***

***1***

…

损失函数是一种量化机器学习模型表现得有多好的方法。量化是基于一组输入的输出(成本)，这些输入被称为参数值。参数值用于估计预测，而“损失”是预测值和实际值之间的差异。

# 5.优化(神经网络)

![](img/9104df8a26cbc138a65bff396afb7d04.png)

马修·施瓦茨在 [Unsplash](/s/photos/thinking?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) 上的照片

***3***

***2***

***1***

…

神经网络中的**优化器**是一种算法实现，通过最小化经由损失函数提供的损失值来促进神经网络中的梯度下降过程。为了减少损失，适当地选择网络内的权重值是至关重要的。

优化算法的示例:

*   随机梯度下降
*   小批量梯度下降
*   内斯特罗夫加速梯度

# 6.多层感知器(MLP)

*这是一个老的学期，让我们看看你是怎么做的……*

![](img/ddc21febb6a582188c0f1721145f9b42.png)

马修·班尼特在 [Unsplash](/s/photos/old?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) 上的照片

多层感知器(MLP)是几层感知器一个接一个地连续堆叠。MLP 由一个输入层、一个或多个称为隐藏层的 TLU 层以及一个称为输出层的最终层组成。

***3***

***2***

***1***

…

# 7.三重威胁

*   **学习率**
*   **学习率计划表**
*   **学习率衰减**

*保持冷静，你能行的*

![](img/78f91fe1af7d8314c84e2e467f072765.png)

由[麦迪逊·拉弗恩](https://unsplash.com/@yogagenapp?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)在 [Unsplash](/s/photos/thinking?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) 上拍摄的照片

*记住这都是关于学习的，所以如果你想不起来几个也没关系。*

***3***

***2***

***1***

…

**学习率**是神经网络不可或缺的组成部分，因为它是一个决定网络权值更新水平的因子值。

**学习率时间表**:在神经网络的训练过程中可以使用恒定的学习率，但这会增加达到最佳神经网络性能所需的训练量。通过利用学习速率表，我们在训练期间引入学习速率的适时减少或增加，以达到神经网络的最佳训练结果。

**学习率衰减:**学习率衰减减少了梯度下降过程中向局部最小值移动的步长的振荡。通过将学习率降低到与训练开始时使用的学习率值相比更小的值，我们可以将网络导向在最小值附近的更小范围内振荡的解。

# 8.神经类型转移

*这是最后一个*

![](img/c6ea30db76df0f3ff131ae2f4e221674.png)

弗雷德里克·图比尔蒙特在 [Unsplash](/s/photos/end?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) 上拍摄的照片

***3***

***2***

***1***

…

**神经风格转移(NST)** 是一种涉及利用深度卷积神经网络和算法从一幅图像中提取内容信息并从另一幅参考图像中提取风格信息的技术。在提取样式和内容之后，生成组合图像，其中所得图像的内容和样式源自不同的图像。

NST 是一种图像风格化的方法，这是一种涉及使用输入参考图像来提供具有从输入图像导出的风格差异的输出图像的过程。

# 我希望这篇文章的内容对你有用。

要联系我或找到更多类似本文的内容，请执行以下操作:

1.  订阅我的 [**YouTube 频道**](https://www.youtube.com/channel/UCNNYpuGCrihz_YsEpZjo8TA) 即将上线的视频内容 [**这里**](https://www.youtube.com/channel/UCNNYpuGCrihz_YsEpZjo8TA)
2.  跟我上 [**中**](https://medium.com/@richmond.alake)
3.  通过 [**LinkedIn**](https://www.linkedin.com/in/richmondalake/) 联系我