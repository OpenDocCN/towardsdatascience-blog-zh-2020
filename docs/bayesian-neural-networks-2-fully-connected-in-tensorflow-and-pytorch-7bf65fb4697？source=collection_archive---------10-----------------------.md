# 贝叶斯神经网络:TensorFlow 和 Pytorch 中的 2 个完全连接

> 原文：<https://towardsdatascience.com/bayesian-neural-networks-2-fully-connected-in-tensorflow-and-pytorch-7bf65fb4697?source=collection_archive---------10----------------------->

## [贝叶斯神经网络](https://towardsdatascience.com/tagged/adam-bayesian-nn)

![](img/a9cbff0feafd497e0e7d9c9d7c3dcc08.png)

Photo by [青 晨](https://unsplash.com/@jiangxulei1990?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) on [Unsplash](https://unsplash.com/s/photos/eggs?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)

**本章继续贝叶斯深度学习系列。可用上一章** [**此处**](/bayesian-neural-networks-1-why-bother-b585375b38ec) **和下一章** [**此处**](/bayesian-neural-networks-3-bayesian-cnn-6ecd842eeff3) **。**本章我们将探索传统密集神经网络的替代解决方案。这些备选方案将调用神经网络中每个权重的概率分布，从而产生单个模型，该模型有效地包含基于相同数据训练的神经网络的无限集合。我们将用这些知识来解决我们这个时代的一个重要问题:煮一个鸡蛋需要多长时间。

## 章节目标:

*   熟悉密集贝叶斯模型的变分推理
*   了解如何将正常的全连接(密集)神经网络转换为贝叶斯神经网络
*   了解当前实施的优点和缺点

这些数据来自煮鸡蛋的实验。煮鸡蛋的时间和鸡蛋的重量(以克为单位)以及切开鸡蛋后的发现一起提供。调查结果分为三类:欠熟、半熟和全熟。我们想从鸡蛋的重量和煮的时间得到鸡蛋的结果。这个问题非常简单，以至于数据几乎是线性可分的⁠.但也不尽然，因为没有提供鸡蛋的预煮寿命(冰箱温度或室温下的橱柜储存),你会看到这改变了烹饪时间。没有缺失的数据，我们无法确定打开鸡蛋时会发现什么。知道了我们有多确定我们可以影响这里的结果，就像我们可以影响大多数问题一样。在这种情况下，如果相对有把握鸡蛋没有煮熟，我们会在敲开之前多煮一会儿。

![](img/6d05094396fa97e2fe8811808095bbcf.png)

让我们先看看数据，看看我们在处理什么。如果你想亲自感受这种不同，你可以在[github . com/doctor loop/Bayesian deep learning/blob/master/egg _ times . CSV](http://github.com/DoctorLoop/BayesianDeepLearning/blob/master/egg_times.csv)获取数据。您将需要 Pandas 和 Matplotlib 来探索数据。(pip install —升级 pandas matplotlib)将数据集下载到您正在工作的目录中。如果不确定，可以从 Jupyter 笔记本的单元格中键入 pwd 来查找目录的位置。

![](img/aedbbcaf2dd2e24a8ca9bc9523ffd02a.png)

[https://gist . github . com/doctor loop/5a 8633691 f 912d 403 e 04 a 663 Fe 02 E6 aa](https://gist.github.com/DoctorLoop/5a8633691f912d403e04a663fe02e6aa)

![](img/59b469192107eefb27b49a59407935dc.png)![](img/686c456b4c48bf082eb7646c20507657.png)

[https://gist . github . com/doctor loop/21e 30 BD F16 D1 f 88830666793 f 0080 c 63](https://gist.github.com/DoctorLoop/21e30bdf16d1f88830666793f0080c63)

![](img/b41e46a97d71b00085fb060736b779f7.png)

**图 2.01** 鸡蛋结果散点图

让我们看一下柱状图。

![](img/02a1fda316fc86906059630be6dc8a56.png)

[https://gist . github . com/doctor loop/2 a5e 95 a68a 29315 f 167 E0 e 875 E7 FAE 16](https://gist.github.com/DoctorLoop/2a5e95a68a29315f167e0e875e7fae16)

![](img/df6a6f939c5386d2272bc582064b2c9e.png)

**图 2.02** 按结果划分的排卵次数直方图

我似乎不太擅长做我喜欢的半熟鸡蛋，所以我们看到了一个相当大的阶级不平衡，相对于半熟的可爱鸡蛋，半熟鸡蛋的数量是半熟鸡蛋的两倍，熟鸡蛋的数量是半熟鸡蛋的三倍。这种类别不平衡可能会给传统的神经网络带来麻烦，导致它们表现不佳，并且不平衡的类别大小是一个常见的发现。

注意，我们没有将 density 设置为 True (False 是默认值，因此不需要指定)，因为我们对比较实际数字感兴趣。而如果我们要比较从三个随机变量中的一个中采样的概率，我们会希望设置 density=True，以将数据总和的直方图归一化为 1.0。

直方图对于数据的离散化可能很麻烦，因为我们需要显式指定(或使用算法为我们指定)收集值的箱数。箱的大小极大地影响了数据的显示方式。作为一种替代方法，我们可以选择使用核密度估计，但在比较组时通常会更好，因为我们在这里使用的是小提琴图。violin 图是一种混合图，带有显示平滑分布的箱形图，便于比较。在 python 中，用 seaborn 库来绘制 violin 图既容易又漂亮(pip install —升级 Seaborn)。但是为了保持一致，我将在 matplotlib 中显示它。

![](img/6862ee86e25b90ac3fc104469850c936.png)

[https://gist . github . com/doctor loop/C5 BAC 12d 7 b 7 EBD 7758 cc 83 f 0 Fe 931 fc 0](https://gist.github.com/DoctorLoop/c5bac12d7b7ebd7758cc83f0fe931fc0)

![](img/273cf408b747ae854710d6a81790bfbc.png)

**图 2.03** 按结局划分的鸡蛋次数小提琴图。中间的条形表示平均值。

好极了。现在让我们强调一下我们实际上要学习的神经网络之间的架构差异。我们将首先实现一个经典的密集架构，然后将其转换成一个强大的 Bayesianesque 式的对等物。我们使用基本库(TensorFlow 或 Pytorch)实现密集模型，然后使用附加库(TensorFlow-Probability 或 Pyro)创建贝叶斯版本。不幸的是，TensorFlow 实现密集神经网络的代码与 Pytorch 的代码非常不同，所以请转到您想要使用的库部分。

# 张量流**/张量流-概率**

源代码可从以下网址获得

【github.com/DoctorLoop/BayesianDeepLearning/ 号

![](img/844ffff4b90d3335ac50bade2d3bc36a.png)

[https://gist . github . com/doctor loop/3a 67 FD 1197 a 355 f 68 f 45076 be 0074844](https://gist.github.com/DoctorLoop/3a67fd1197a355f68f45076be0074844)

我们使用 Keras 来实现，因为这是我们后面的 TFP 模型所需要的。我们通过调用编译后的模型开始训练，如下所示。

![](img/e405fde457a115011c04716959139230.png)

[https://gist . github . com/doctor loop/f6c 74509046068 AE 7 AC 37 EFE 00d 08545](https://gist.github.com/DoctorLoop/f6c74509046068ae7ac37efe00d08545)

你会看到 TensorBoard 是用来记录我们的训练的——利用 TensorBoard 作为其强大的模型调试手段和 TensorFlow 的关键功能之一是很好的。如果我们想在训练期间在单元格下输出训练进度，我们可以使用 Keras 输出，将 model.fit 的 verbose 参数设置为 1。Keras 输出的问题是它会打印每个时期的损失。这里我们训练了 800 个纪元，所以我们会得到一个又长又乱的输出。因此，最好创建一个定制的日志记录器来控制打印损失的频率。自定义记录器的源代码非常简单，可以在 Github 的笔记本中找到。TensorBoard 回调(和任何自定义记录器)都被传递给 callbacks 参数，但如果您不想只记录笔记本中的每个纪元，可以忽略这一点。Keras 的伟大之处在于，它会为我们拆分数据进行验证，从而节省时间。这里 10%的数据用于验证。有些人使用 25%的数据进行验证，但由于数据是神经网络最重要的考虑因素，数据集往往相当大，因此 10%的数据很好，并使模型更有可能达到我们的训练目标。Github 笔记本还展示了如何使用类权重来解决我们之前讨论的类不平衡问题。最后，如果培训时间很重要，请确保验证不频繁，因为验证比培训本身慢得多(特别是使用自定义记录器时，大约慢 10 倍)，从而使培训时间更长。

最终损耗在 0.15 左右，精度 0.85。如果你在 1200 个历元之后训练超过 800 个历元，你会发现验证的准确性在下降，尽管损失仍在下降。这是因为我们已经开始过度适应训练数据。当我们过度拟合时，我们使我们的模型依赖于特定的训练实例，这意味着它不能很好地概括我们看不见的验证数据或模型⁠.的预期真实世界应用过度拟合是传统神经网络的祸根，传统神经网络通常需要大型数据集和早期停止方法来缓解。

但我们是来学习解决方案而不是问题的！进入我们的贝叶斯神经网络阶段，它可以抵抗过度拟合，不会特别担心类别不平衡，最重要的是 ***在很少数据的情况下表现得非常好*** 。

让我们看一下模型的贝叶斯实现。

![](img/c7e1de2c4fe29bdc399533a1a37fa6b6.png)

[https://gist . github . com/doctor loop/f 00 c 18 f 591553685 e 06 a 79 fdb 4 b 68 e 0](https://gist.github.com/DoctorLoop/f00c18f591553685e06a79fdbe4b68e0)

它与我们的传统模型非常相似，只是我们使用了 TensorFlow-Probability 的 flipout 层。我们指定一个核散度函数，它是前面提到的 Kullback-Leibler 散度。大概就是这样。

# **Pytorch/Pyro**

当比较传统的密集模型和贝叶斯等价模型时，Pyro 做的事情不同。对于 Pyro，我们总是先创建一个传统的模型，然后通过添加两个新的函数来进行转换。传统模型需要提供一种从权重分布中自动采样值的方法。采样值被插入到传统模型上的相应位置，以产生训练进度的估计。从上一章你会记得训练*如何调节*(训练)体重分布。在清晰的语言中，这意味着训练改变了正态分布，并改变了它们的比例来代表每个权重。然而，为了进行预测，我们仍然插入单一重量的固体值。虽然我们不能使用整个分布来进行预测，但是我们可以进行许多预测，每个预测使用不同的采样权重值来近似分布。这就是我们首先构建的密集模型非常适合的地方。

我们将从一个类开始来封装我们的密集模型。完整的源代码可以在 https://github.com/DoctorLoop/BayesianDeepLearning 的在线笔记本上找到。

![](img/980178784d15082e840e257451d41085.png)

[https://gist . github . com/doctor loop/33b 192030388 c 46 d0a 2591459 b2f 6623](https://gist.github.com/DoctorLoop/33b192030388c46d0a2591459b2f6623)

我们设置了三层，只有前两层使用激活功能。然后，我们指定了损失函数和优化器。请注意，对于这个模型，我们使用的是 torch.optim.Adam (Pytorch 优化器),而不是 Pyro 优化器。如果你试图在这里使用 Pyro，它会抛出错误，因为参数是不同的。如果你以前用过 Pytorch，我们的训练循环没有什么特别的。800 个纪元完全是多余的，但那又怎样。

![](img/21a32c8e1cfb6e8515120334902db574.png)

[https://gist . github . com/doctor loop/75854 f 26 bee 106 d9 b 596 B2 ee 0544 C5 D5](https://gist.github.com/DoctorLoop/75854f26bee106d9b596b2ee0544c5d5)

```
[Out]:
…
Test accuracy: 0.88
Final loss:0.173 at epoch: 799 and learning rate: 0.001
```

这里的表现非常好，速度也很快。如果你是 Pytorch 的新手，并且在其他地方有经验，你可能会对 softmax 的缺乏感到惊讶。在 Pytorch 中，需要注意的是 softmax 内置于 CrossEntropyLoss 中。

让我们继续升级它。这是我提到的两个新功能，模型和向导。

![](img/4304a0a23992b28f03ddff4127e24681.png)![](img/17b7dbb0f68e5ed13f967c7036f51202.png)

[https://gist . github . com/doctor loop/a 4d cf 6 b 0 f 7 f1 ECE 43087534 ebdfcaa 08](https://gist.github.com/DoctorLoop/a4dcf6b0f7f1ece43087534ebdfcaa08)

我们将在本系列的另一篇文章中详细讨论这些函数的作用。简而言之，该模型明确声明了用于每个图层的分布，以替换点值。而指南声明了用于调节(训练)这些分布的变量。您会注意到这些函数看起来很相似，但仔细观察后，模型列出了权重和偏差的单独分布，然后指南列出了模型 中每个权重和偏差分布的平均值和标准差 ***的分布。这听起来有点玄乎——因为确实如此。把思考留到以后，现在就感受一下训练。***

![](img/5dddb3575b4fe09b896358a49a2129b9.png)

[https://gist . github . com/doctor loop/5a 3837 c 384181 b 391 df 5927 BD D5 e 2 ab 5](https://gist.github.com/DoctorLoop/5a3837c384181b391df5927bdd5e2ab5)

训练这个美丽的怪物很快。我们得到的精度略高于密集模型 0.88–0.90，损失为 0.18。我在这里实现了一些额外的函数来使训练更容易(trainloader 和 predict 函数),并在 GitHub 笔记本中提供了完整的源代码。

# 贝叶斯深度学习的损失函数

我们不能通过随机变量反向传播(因为根据定义它们是随机的)。因此，我们欺骗和重新参数化这些分布，并让训练更新分布参数。随着这一根本性的变化，我们需要一种不同的方法来计算培训损失。

你可能已经知道传统模型中使用的负对数似然。它反映了模型生成数据的概率。现在不要担心这意味着什么。相反，我们通过取大量样本的平均值来估算这一损失。但是，随着我们的负对数可能性，我们结合了一个新的损失，利用计算中的分布。新的损失是 kull back-lei bler 散度(KL 散度),并提供了两个分布如何彼此不同的度量。我们会经常提到 KL 散度，并发现它在其他领域也很有用，包括 uncertainty⁴.指标负对数似然被加到 KL 散度上以得到 ELBO 损失(边际似然的预期下限，也称为变分自由能)。ELBO 损失允许我们在训练期间近似分布，并且在可训练性和训练时间方面受益巨大。

最后，作为总结，让我们来看看当我们做预测时会发生什么，毕竟这是我们在这里的目的。

![](img/f5b8c80e1a5a6d0cedee09ef824364bd.png)

[https://gist . github . com/doctor loop/41 c 31 BF 30339 DC 1 B4 B4 c 7 e 69 a 001 b 9 BF](https://gist.github.com/DoctorLoop/41c31bf30339dc1b4b4c7e69a001b9bf)

我们制作 5000 个样本(这可能需要几分钟，取决于您的计算机)。使用传统的神经网络，所有这些预测都将是完全相同的，因此这将是一个毫无意义的努力。但是我们不再使用传统的神经网络。

![](img/846c6fa986da6529abc510203526e9e7.png)

[https://gist . github . com/doctor loop/1 ae0d 213d 3875 DD 569d 582 c 810769 fc 7](https://gist.github.com/DoctorLoop/1ae0d213d3875dd569d582c810769fc7)

![](img/4090058dcc30f0d256878680c79110b3.png)

**图 2.04:**3 个测试输入的 5000 个样本的直方图

预测函数从我们用 TFP 或 Pyro 训练的主模型中采样多个不同的模型版本。我们输入的三个测试实例都产生了定义明确的峰值。第一个测试实例(红色直方图)的重量为 61.2 克，沸腾时间为 4.8 分钟。大多数时候，我们可以看到我们的模型预测它将是一个半熟的鸡蛋，但有 5%的时间它预测一个半熟的鸡蛋，有 2%的时间它认为它将是一个半熟的鸡蛋。该模型对此预测相当有信心，但不如第二个测试示例(绿色直方图)有信心，第二个测试示例几乎总是预测煮了 6 分钟的 53g 鸡蛋的煮熟结果。如果每次预测都不一样，我们如何从模型中获得一致性？我们只是取平均值。如果预测太易变，即每类中有三分之一，我们就不想做预测，除非我们可以根据信息采取行动，告诉用户模型对结果不确定(或者问问我们自己是否需要对模型/数据做更多的工作！)在这两种情况下，我们都获得了传统模型无法提供的非常有力的信息。因此，我们可以认为传统模型总是傲慢的，而我们的新模型是恰当的。很谨慎，但是自信正确的时候很自信。我们不需要成为计算机科学心理学家来欣赏更好的模型——人格。

# **总结**

在这一章中，我们已经探索了基本贝叶斯模型的变化，并看到了伴随它的一些主要优势。我们已经在 TensorFlow-Probability 和 Pyro 中看到了如何做到这一点。虽然该模型功能齐全，但在现阶段它并不完美，也不是真正的贝叶斯模型。“更真实”的模型是否重要取决于你的环境。在随后的文章中，我们将讨论 softmax 的使用等缺陷，以及如何解决这些问题。在下一篇文章中，我们的主要关注点将是使用贝叶斯卷积深度学习进行图像预测。如果我的写作看起来更正式一点，我可能会更有信心在那里看到你！

1 因此，当更简单的模型可以做得很好的时候，用神经网络来解决问题有点滥用。

2 不确定这里的实际应用是什么，早餐咖啡馆？

3 如果你的想法是:哇，张量流概率代码看起来简单多了，因为它现在为你做了运算。也就是说，我们可以通过使用 Pyro autoguide 来简化 Pyro 代码。正如它所暗示的那样，这对你起着向导的作用。但我们在这里是为了学习贝叶斯深度学习的神奇之处，所以我们需要在某些时候曝光！

4 有趣的是，我们可以通过直接使用 KL 散度来制作具有合理性能的简单分类器，即，可以使用局部二进制模式算法和 KL 来构建图像模式分类器。在这种伙伴关系中，KL 比较输入图像和具有已知图案的图像之间的角和边缘的分布。