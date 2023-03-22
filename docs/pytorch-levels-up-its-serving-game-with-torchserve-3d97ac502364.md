# PyTorch 用 TorchServe 升级了它的发球游戏

> 原文：<https://towardsdatascience.com/pytorch-levels-up-its-serving-game-with-torchserve-3d97ac502364?source=collection_archive---------42----------------------->

## 通过 TorchServe，脸书和 AWS 继续缩小机器学习研究和生产之间的差距。

![](img/be2cc614dc2ae2910a6557d2f8724db7.png)

[PyTorch](https://pytorch.org/) 标志

近年来，PyTorch 已经在很大程度上超过 Tensorflow，成为研究型数据科学家的[首选的机器学习模型训练框架。这有几个原因，但主要是 Pytorch 是为 Python 构建的，作为其首选语言，而 Tensorflow 的架构更接近其 C/C++核心。尽管这两个框架都有 C/C++内核，Pytorch 确实*加载了更多的*来使其接口“](https://pureai.com/articles/2019/10/10/machine-learning-framework-popularity.aspx)[python 化](https://docs.python-guide.org/writing/style/)”。对于那些不熟悉这个术语的人来说，它基本上意味着代码很容易理解，不会让你感觉像是一个敌对的老师为一道考试题而写的。

Pytorch 更简洁的界面已经被那些主要优先考虑将他们的计划分析快速转化为可操作结果的人们广泛采用。我们没有时间玩静态计算图 API，它让我觉得自己像个罪犯，因为我想设置一个断点来调试张量的值。我们需要做一些原型，然后进入下一个实验。

# 不全是阳光和彩虹

然而，这种优势是有代价的。使用 Pytorch，我们可以轻松地完成一个又一个实验，将结果以类似的速度投入生产。可悲的是，由于缺乏封装 API 复杂性的生产就绪框架，使这些模型服务于生产比实验吞吐量要慢。至少，Pytorch 缺少这些框架。

Tensorflow 早就有了一个真正令人印象深刻的模型服务框架， [TFX](https://www.tensorflow.org/tfx) 。事实上，如果你对 Tensorflow 生态系统了如指掌，它的框架并没有太多缺失。如果你有一个 TF 模型，并且正在使用谷歌云，那就使用 TFX 直到它咽下最后一口气。如果你不在那个阵营，结合 TFX 和 PyTorch 已经不是即插即用。

# 新的希望

不再害怕！PyTorch 的 [1.5 版本](https://pytorch.org/blog/pytorch-library-updates-new-model-serving-library/)带来了 [TorchServe](https://pytorch.org/serve) 的初始版本以及 [TorchElastic](https://github.com/pytorch/elastic) 与 Kubernetes 的实验支持，用于大规模模型训练。软件巨头脸书和 AWS 继续增强 PyTorch 的能力，并为谷歌基于 Tensorflow 的软件管道提供了一个有竞争力的替代方案。

> *TorchServe 是一个灵活易用的库，用于在大规模生产中高效地服务 PyTorch 模型。它不受云和环境的限制，支持多模型服务、日志记录、指标以及为应用集成创建 RESTful 端点等特性。*
> 
> - [*PyTorch 文档*](https://pytorch.org/blog/pytorch-library-updates-new-model-serving-library/)

让我们来解析其中的一些品质，因为它们值得重点重复:

*   在大规模生产中高效服务 PyTorch 模型
*   与云和环境无关
*   多模式服务
*   记录
*   韵律学
*   RESTful 端点

任何生产中的 ML 模型具有这些特征，一定会让你的工程师同事非常高兴。

# 让我们点燃火炬吧

TorchServe 提供了您期望从现成的服务 API 中得到的大部分东西。除此之外，它还为基于图像和基于文本的模型提供了[默认推理处理器](http://pytorch.org/serve/default_handlers.html)。由于我的音频背景，我不能不抱怨基于音频的模型再次被排除在一流的支持之外。目前，音频仍然是机器学习领域的害群之马！唉，我跑题了……

在图像分类任务中产生良好结果的 [DenseNet](/densenet-2810936aeebb) 架构在 TorchServe 中作为默认模型提供。PyTorch 工程师提供了一个[预建的 docker 图像](http://pytorch.org/serve/install.html#running-with-docker)，但是我[为下面的演示添加了一些东西。如果你想为自己重建，在这里运行四个](https://github.com/aagnone3/pytorch-serve-overview/blob/master/Dockerfile)[命令](https://github.com/aagnone3/pytorch-serve-overview#steps)。

# 运行正常的

通过向`torchserve`命令传递一些命令行参数，我们就有了一个准备好提供模型预测的生产服务。在不使文章陷入细节的情况下，您可以使用一个快速的驱动程序脚本从 URL 或本地路径通过服务器抽取图像。

陈词滥调时间！猫和狗，开始了。

```
./[query.sh](https://github.com/aagnone3/pytorch-serve-overview/blob/master/query.sh) images/puppy.jpg
```

![](img/38a62442501740d1279370212b04c189.png)

GDragon612 在 [Fanpop](https://www.fanpop.com/clubs/laura1233214/images/43117566/title/cute-photo) [ [来源](http://images6.fanpop.com/image/photos/43100000/cute-puppy-laura1233214-43117566-736-736.jpg)上的照片

```
Blenheim_spaniel: 97.00%

Papillon: 1.40%

Japanese_spaniel: 0.94%

Brittany_spaniel: 0.41%
```

完全正确！前四名中的三个是西班牙狗的亚种，大约 1%的巴比隆狗没什么可大惊小怪的。

一只小小的小猫怎么样，*但是* *多了一顶毛皮帽子？我猜他天生的皮毛和耳朵不适合这张照片？不管怎样，就网络的预测而言，这肯定不会改变太多…*

```
./[query.sh](https://github.com/aagnone3/pytorch-serve-overview/blob/master/query.sh) images/kitten-2.jpg
```

![](img/747caa3141a09446838f460523dd100a.png)

照片由 sillymonkeyart 在[易贝](http://www.ebay.com)[来源](https://www.ebay.com/c/1972575297)

```
Egyptian_cat: 24.90%

Marmoset: 20.75%

Tabby: 11.22%

Patas: 10.17%
```

好吧，那么…

当然，大多数人投票支持猫进入前 4 名，但是这些预测几乎没有超过狨猴和狨猴的预测，这两者都是对…猴子的分类。

![](img/655f117a16276e2762b1b09bd9c1ca18.png)

Andrew Sit 在 [Unsplash](https://unsplash.com/s/photos/patas-monkey?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) 上拍摄的照片

很可爱，但没那么可爱。

我猜是为了努力。出于好奇，现在有一个很好的机会来应用[注意力技术](https://lilianweng.github.io/lil-log/2018/06/24/attention-attention.html#soft-vs-hard-attention)来调试图像的哪些部分在影响分类中被大量使用。

# 火炬服务在行动

再往下一步，这是一些原始 cURL 命令的演示。请记住，所有这些 API 行为都是现成的。我提供的只是模型和输入图像的选择。

![](img/acbb0be99c3108f0cbd37c9b2f9689a0.png)

驾驶我的本地火炬服务器实例

# 结论

我几乎没有接触过 TorchServe 的表面，这是一件好事。如果我可以在一篇简单的博客文章中演示它的所有功能，它还远远不能投入生产。也许在把它放在任何任务关键的东西后面之前，允许更多的版本翻转。但是脸书和 AWS 为润滑 PyTorch 研究和生产之间的滑道建立了一个令人兴奋的基础。

# 保持最新状态

在学术界和工业界，事情发展得很快！用一般的 [LifeWithData](https://lifewithdata.org/blog/) 博客和 [ML UTD](https://lifewithdata.org/tag/ml-utd/) 时事通讯让自己保持最新状态。

如果你不是时事通讯的粉丝，但仍然想留在圈子里，考虑将 lifewithdata.org/blog 的[和 lifewithdata.org/tag/ml-utd 的](https://lifewithdata.org/blog)[添加到](https://https//lifewithdata.org/tag/ml-utd) [Feedly](http://that%27s%20all%20for%20ml%20utd%202.%20however%2C%20things%20are%20happening%20very%20quickly%20in%20academics%20and%20industry%21%20aside%20from%20ml%20utd%2C%20keep%20yourself%20updated%20with%20the%20blog%20at%20lifewithdata./) 聚合设置中。

*原载于 2020 年 5 月 24 日 https://lifewithdata.org*[](https://lifewithdata.org/torch-serve/)**。**