# Google Colab 中 ResNet 的端到端适配—第 2 部分:硬件和数据集设置

> 原文：<https://towardsdatascience.com/end-to-end-adaptation-of-resnet-in-google-colab-part-2-hardware-dataset-setup-f23dd4e0004d?source=collection_archive---------69----------------------->

## 端到端 Resnet

## CPU、GPU、数据集设置

![](img/e15eff2a5640c76c2b468e5bc688d4ad.png)

来源:Unsplash.com

我希望你们都有机会预览本系列的[第 1 部分](/end-to-end-adaptation-of-resnet-in-google-colab-part-1-5e56fce934a6)，它不需要任何 python 知识，只需要一个 Google 和 Kaggle 帐户(后者只是为了验证你自己，所以你可以下载数据集。)

作为快速概述，我们能够在 ResNet-18 架构上训练狗与猫数据集(总共 25，000 张图像)。一旦完成训练，你就可以上传你最喜欢的猫或狗的照片，并观察输出结果。

我将把这个系列分成 5 个部分:

1.  介绍
2.  硬件和数据集设置
3.  图像预处理
4.  建筑培训
5.  真实世界测试

# 旁白

一旦你完成了训练，当你输入一张猫头鹰的照片时会发生什么？

![](img/b33d15fa9b072855ab509875822eb39d.png)

来源:Unsplash.com

输出是‘狗’！虽然只有 78.41%的把握。(如果最后一个单元格有问题，只需使用左侧的箭头重新运行它。)

我已经插入了所有东西，从物品到我朋友的照片(他们看起来有点像猫)，结果到处都是。有时，概率在 90%以上。

如果还不清楚的话，我要强调这一点——**你必须知道使用**训练网络的基础数据，否则你的预测将是不可靠的，并且取决于你使用它的目的，是危险的。你对这些数据了解得越多，你对输出的理解就越丰富。

不要用“人工智能”这个时髦词，但如果我要描述我们所建立的东西，我会称之为[“狭义人工智能”](https://en.wikipedia.org/wiki/Weak_AI)。这不是[人工通用智能(AGI)](https://en.wikipedia.org/wiki/Artificial_general_intelligence) 。在这里我们只能辨别狗和猫。

# 库导入

这些是运行整个程序所需的库。‘torch’是 PyTorch(类似于机器学习库 Tensorflow)。“torchvision”包含最流行的模型和数据集以及图像转换(稍后讨论)。“matplotlib”让我们可以创建图像的图形和表格。

当您看到“从 ABC 进口 XYZ”时，我们尽量不输入 ABC。XYZ 每次都可以用 XYZ。例如:

现在，我们可以只使用“listdir”命令，而不是键入“os.listdir”。这是一个简单的例子，但是浓缩类似下面这样的代码有助于简化我们的代码:

你会看到在库导入单元格的末尾是:

想想那个“！”将命令传递到命令行，而不是 python 内核，这样你就可以安装你需要的库，比如‘ka ggle’(让我们下载数据集)。

# CPU、内存和 GPU

我提到过 Google Colab 中的高端 CPU 和 GPU。

这将向命令行传递一个命令，以获取有关 CPU 的信息，而“grep”(全局正则表达式打印)搜索纯文本数据。如果你跑了！你会得到一个很长的输出列表，但我们只是想知道有多少个 cpu，型号和 GHz。

与上面类似，‘啊！' cat /proc/meminfo '给出了一个很长的输出列表，但是我们只想知道总 RAM(因此 grep MemTotal)

CUDA(计算统一设备架构)是由 Nvidia 设计的，旨在让我们这样的程序员访问支持 CUDA 的 GPU 来加速我们的数学运算(在这种情况下，是图像分析)。有很多帖子在不同的细节层次上讨论了这是如何工作的。

注意我们通过' torch.cuda.is_available()'使用 pytorch 库，它输出一个布尔值(True vs False)。如果为真，我们还可以使用 pytorch 库来获取设备的名称。

这个设备名称会根据 Google Colab 提供的内容不时更改(有时你根本无法获得 GPU，因为你已经用完了当天的配额)。今天早上是特斯拉 P100。有时我会得到一个超快的特斯拉 T4(免费的！).

# 获取数据集

卡格尔号。在此步骤中，您下载的“JSON”文件将通过以下方式导入:

如果您参考库导入部分，您会看到这一行:

“files.upload()”允许您上传 kaggle.JSON。

上面的命令都传递给机器命令行，而不是 python 内核。创建一个名为“kaggle”的目录，并将您上传的 kaggle.json 文件复制到新创建的目录中。

“chmod 600”允许用户读写 kaggle.json 文件。这很重要，因为 kaggle 库使用 JSON 文件来验证您下载名为“dogs-vs-cats”的数据集。

最后，解压缩(悄悄地，用-q ),但也覆盖(-o)以防你已经下载并解压缩了它。

请在下面留下任何问题，我会尽力回答。

# 参考

[1] [深度学习与 Pytorch](https://pytorch.org/tutorials/beginner/deep_learning_60min_blitz.html) ，2020 年 10 月访问

[【2】神经网络与 Pytorch。【2020 年 10 月接入](https://pytorch.org/tutorials/beginner/blitz/neural_networks_tutorial.html)

[3] [为计算机视觉转移学习。【2020 年 10 月接入](https://pytorch.org/tutorials/beginner/transfer_learning_tutorial.html)

[4] [Kaggle API。【2020 年 10 月访问](https://github.com/Kaggle/kaggle-api)