# 如何开始一个机器学习项目

> 原文：<https://towardsdatascience.com/how-to-start-a-machine-learning-project-5654832cb1ed?source=collection_archive---------30----------------------->

## 做机器学习项目的分步指南

![](img/7be5b9bbbea435aef938bac33859f400.png)

[斯科特·格雷厄姆](https://unsplash.com/@sctgrhm?utm_source=medium&utm_medium=referral)在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍照

在 a [之前的文章](/artificial-intelligence-machine-learning-and-deep-learning-what-the-difference-8b6367dad790)中，我们讨论了很多关于什么是机器学习，机器学习是如何工作的，以及机器学习实现的例子。如果你是机器学习的新手，你可能会问，你如何开始一个机器学习项目？

这可能很多人都经历过，包括我。我读了很多书，但仍然不知道从哪里开始。我发现，学习启动机器学习项目的最佳方式是通过设计和完成小项目。因为要做大，我们必须从小做起。但是这个过程因人而异。

在这篇文章中，我将根据我的经验分享从事机器学习项目的几个阶段。

## 理解问题

![](img/25b865e24e0955c18b22d4b80e5faf40.png)

照片由[优 X 创投](https://unsplash.com/@youxventures?utm_source=medium&utm_medium=referral)在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄

开始一个项目的最初阶段是知道我们将解决什么问题。这适用于任何项目，包括机器学习项目。当然，一切都从问题开始，因为如果没有问题，就没有什么需要解决的。

我们必须确定我们要解决的问题。例如，我们希望实时了解用户在社交媒体上对某个产品的看法。但是因为存在的很多意见，完全靠人类是不可能做到的。

然后我们也确定目标。由此，我们可以确定目标是创建一个机器学习系统来实时分类用户意见(情感分析)并预测未来意见的情感。

> 如果你不能解决一个问题，那么有一个更容易的问题你可以解决:找到它乔治·波利亚

建议你学习一下计算思维中的[分解，就是把复杂的问题分解成更小的部分的过程。通过分解，起初看起来势不可挡的问题变得更容易管理。](https://equip.learning.com/decomposition-computational-thinking/)

## 数据采集

![](img/ef57151c7638a2264c9298b54fb88910.png)

米卡·鲍梅斯特在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上的照片

如果你已经找到了一个问题和你想要解决的目标，那么下一步就是获取所需的数据。有许多方法可以收集数据。我将解释其中的一些。

*   第一种方式是在网上下载开源数据如 [Kaggle](https://www.kaggle.com) 、 [Google dataset](https://datasetsearch.research.google.com) 、 [UCI 机器学习](http://archive.ics.uci.edu/ml/index.php)等。但你也必须注意使用这些数据集的局限性，因为有些数据集只能用于个人需求或仅用于研究目的。
*   接下来的方式就是在网站上做抓取/抓取。比如你想检索社交媒体上仇恨言论评论的数据，可以在社交媒体 [Twitter](https://twitter.com/home) 、 [Instagram](https://www.instagram.com) 等上抓取。或者，如果你需要来自新闻网站的数据，你可以在新闻网站上做网络搜集。

[](/scraping-a-website-with-4-lines-using-python-200d5c858bb1) [## 用 Python 实现 4 行新闻的网络抓取

### 抓取网站的简单方法

towardsdatascience.com](/scraping-a-website-with-4-lines-using-python-200d5c858bb1) 

在某些情况下，您可能需要标记数据，特别是如果您通过 web 爬行/抓取获得数据集，并且您将使用的机器学习方法是监督学习。必须考虑的是标注数据时可能出现的偏差。这可能会影响模型的性能。

如果你想验证你标记的数据，你可以向他们领域的专家寻求帮助。例如，如果您在健康领域创建了一个数据集，那么您可以验证您为医生创建的数据。

## 数据准备

![](img/00f2a0f124f4cbd4f23a0b84b395c9c2.png)

照片由[卢克·切瑟](https://unsplash.com/@lukechesser?utm_source=medium&utm_medium=referral)在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄

我们得到需要的数据后，下一步就是在进入训练阶段前准备数据。这是为什么呢？

让我们比较一下作为烹饪材料的数据。我们在烹饪之前，肯定会先对原料进行处理，比如清洗、清洁、去除不必要的成分、切块等。，不可能把原料放进煎锅里。

数据也是如此。我们需要准备数据，以便在进入训练阶段时，数据不包含会影响所创建模型的性能的噪声。

在进一步输入之前，请注意有 2 种类型的数据，即结构化数据和非结构化数据。结构化数据是一种数据模型，它组织数据元素并标准化它们之间的关系。通常，这种类型的数据以表格、JSON 等形式出现。而非结构化数据是没有预定义的数据模型或者没有以预定义的方式组织的信息。这种类型的数据通常是自由文本、图像、信号等形式。

![](img/33db7fbe5fa327e807f686ddc228e6b6.png)

结构化数据与非结构化数据

以下是准备数据时常用的一些流程。

*   数据清理用于消除不需要的数据或特征。对于每种数据类型，处理方式是不同的。在结构化数据中，它用于清除不一致的数据、缺失值或重复值。而在非结构化数据中，例如在文本的情况下，它用于清除在形成模型的过程中实际上不太必要的符号、数字、标点符号或单词。
*   数据转换用于改变数据结构。这对于非结构化数据(例如文本)的情况尤其必要，因为基本上分类器模型不能接受文本输入，所以它必须首先被转换成另一种形式。一些常用的方法有 PCA、LDA、TF-IDF 等。

另一件同样重要的事情是探索性数据分析(EDA)。EDA 的功能是对数据进行初步调查，在统计摘要和图形表示的帮助下发现模式、发现异常、进行假设测试并检查假设。

## 系统模型化

![](img/a27a14a44af1ef915910b7dba67c85e7.png)

罗马卡夫在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄的照片

这可能是你最期待的部分。在这个阶段，我们会做一个机器学习模型。正如我们在上一篇文章中讨论的那样，机器学习有几种方法，即监督学习、非监督学习和强化学习。我们可以根据之前观察到的数据/问题来确定我们采取的方法。

[](/artificial-intelligence-machine-learning-and-deep-learning-what-the-difference-8b6367dad790) [## 人工智能、机器学习和深度学习——有什么区别？

### 人工智能、机器学习和深度学习的简单解释以及它们之间的区别

towardsdatascience.com](/artificial-intelligence-machine-learning-and-deep-learning-what-the-difference-8b6367dad790) 

我的其他建议是，在建模之前先找出每种机器学习方法的优缺点，因为这样会节省很多时间。对于某些数据特征，每种方法都有优点和缺点。例如，如果首先对输入进行规范化，有些方法可以很好地工作；如果数据太大，有些方法会导致过度拟合；还有一些方法需要非常大的数据。

不要让你花时间去尝试一个又一个的方法来产生最好的结果。因为通常，一个机器学习方法有很多我们可以修改的参数。例如，如果我们想使用神经网络建立一个模型，我们可以改变学习率参数、隐藏层的数量、使用的激活函数等。

也许很多人也建议使用深度学习，因为它在几次实验中一直被证明可以得到很好的结果。但在此之前，先想想案例是否真的需要深度学习。不要用深度学习只是为了跟风。因为需要的成本非常大。

如果有可能使用传统的机器学习方法，那么就使用这种方法。但是，如果案件非常复杂，无法用传统的机器学习方法处理，那么你可以使用深度学习。

## 评价

![](img/a792b21b3d8566bed1c7d1896766d656.png)

[万花筒](https://unsplash.com/@kaleidico?utm_source=medium&utm_medium=referral)在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄的照片

最后一个阶段是评估过程。如果你正在训练的模型的性能没有产生好的结果，你当然不希望。尤其是当你的预测模型有很多错误的时候。

![](img/0038c627ba49150e1a8fb5474d61e610.png)

来源:https://twitter.com/ipfconline1/status/1261733186087911427

确定我们正在制作的模型是否好的最快方法之一是测量它的性能。有几种方法来计算性能，包括准确性、f1 测量、轮廓得分等。评估你的模型的另一种方法是和他们领域的专家一起验证你的模型。