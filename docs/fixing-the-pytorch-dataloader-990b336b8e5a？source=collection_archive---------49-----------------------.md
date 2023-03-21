# PyTorch 数据加载器需要翻新

> 原文：<https://towardsdatascience.com/fixing-the-pytorch-dataloader-990b336b8e5a?source=collection_archive---------49----------------------->

## 入侵数据科学工作流程

## 向 PyTorch 数据加载器类添加高度可定制性

我最近遇到了一个有趣的问题。我和一个队友正在进行一系列深度学习实验，这些实验涉及一个跨越数百千兆字节的图像数据集。作为一个优柔寡断的傻瓜，我想知道这些数据是否适合过多的分类任务，所有这些任务都跨越数据集的不同配置。

这让我们在一个 [PyTorch DataLoader](https://pytorch.org/docs/1.1.0/_modules/torch/utils/data/dataloader.html) 形状的兔子洞里走了几个小时，之后我们几乎因为沮丧而放弃。谢天谢地，唯一比编写脚手架代码更令人沮丧的事情是等待虚拟机完成跨任意目录复制大量文件。

![](img/1f10df184de3abefe5d6e89a6e8341e5.png)

您是否曾经为了在不同的标签集上训练 PyTorch 模型而不得不复制/symlink 文件？(图片由 [Unsplash](https://unsplash.com/photos/-2vD8lIhdnw) 上的[jeshouts](https://unsplash.com/@jeshoots)拍摄)

幸运的是，我们很快发现了一个解决方案，并决定是时候对 DataLoader 类进行一次翻新了。我们将杂乱的脚手架代码清理干净，不仅添加了动态标记训练数据的功能，还添加了指定子集、执行自定义预处理操作以输入数据加载器等功能。几个小时的咖啡因诱导代码后， [BetterLoader](https://binitai.github.io/BetterLoader/) 诞生了。

![](img/f44f1736bccba312ac64fa94ad5c6570.png)

如果我说我对自己设计的商标一点都不自豪，那我是在撒谎

[BetterLoader](https://binitai.github.io/BetterLoader/) 完全去掉了默认的 PyTorch DataLoader 结构。现在，您可以将文件存储在一个单一的平面目录中，并使用 [JSON](https://www.json.org/json-en.html) 配置文件的强大功能以不同的方式加载数据。例如，下面几行代码允许您加载带有动态标签的数据集子集:

```
from betterloader import BetterLoaderindex_json = './examples/index.json'
basepath = "./examples/sample_dataset/"loader = BetterLoader(basepath=basepath, index_json_path=index_json)
loaders, sizes = loader.fetch_segmented_dataloaders(
batch_size=2, transform=None)
```

酷的部分？我们的`index.json`只包含一个键-值对的列表，其中键是类标签，值是相关文件名的列表。然而，读取索引文件的 [BetterLoader](https://binitai.github.io/BetterLoader/) 函数可以被[定制](https://binitai.github.io/BetterLoader/docs/#dataset-metadata)，我已经能够通过正则表达式、布尔条件甚至 MongoDB 调用来使用这个库。

我最新的 [BetterLoader](https://binitai.github.io/BetterLoader/) 工作流包括检查是否需要加载图像，从 MongoDB 实例获取裁剪中心，创建一堆裁剪，然后将这些**裁剪馈送给加载器。我只是使用了 [BetterLoader](https://binitai.github.io/BetterLoader/) ，而不是每次都创建不同的搭建代码。**

所以，是的。那是[更好的加载器](https://binitai.github.io/BetterLoader/) — **类固醇 PyTorch 数据加载器**。现在还为时尚早，但我们对让我们的生活，希望也是其他数据科学家的生活变得更容易的前景感到非常兴奋。如果你认为 [BetterLoader](https://binitai.github.io/BetterLoader/) 听起来有用，并且你已经避开了我在本文中散布的所有文档的链接，你可以在 Github [这里](https://github.com/binitai/betterloader)找到源代码，在 PyPi 页面[这里](https://pypi.org/project/BetterLoader/)找到源代码。

我们还将开放大量票证，以增加对无监督深度学习方法的支持，并修复最终会出现的过多问题，因为咖啡因引发的狂欢很少会产生完美稳定的库。我们希望[能为我们带来一位明星](https://github.com/binitai/betterloader)、[参与其中](https://github.com/binitai/betterloader/issues)，或者敬请关注！