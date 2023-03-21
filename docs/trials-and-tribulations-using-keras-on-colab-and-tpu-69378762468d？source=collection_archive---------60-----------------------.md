# 考验和磨难:在科拉布和 TPU 上使用 Keras

> 原文：<https://towardsdatascience.com/trials-and-tribulations-using-keras-on-colab-and-tpu-69378762468d?source=collection_archive---------60----------------------->

![](img/da011357d1110aa22e120e969e4ccc27.png)

Max 陈在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上的照片

## 延续 [**之前的帖子**](https://medium.com/@umash4/using-tpus-on-google-colab-966239d24573) 关于在 Colab 和 TPU 上试验 Keras，同时尝试一些旧的 NLP 教程。在这篇文章中，我试图帮助我的业余爱好者节省一些时间和挫折。

根据上一篇文章中概述的设置，在 160 万条推文中训练情感分类器的速度非常快(每个时期大约 500 秒，而 1.5 小时)。然而，完整的训练仍然需要一个多小时。对于这一轮来说，81%的准确度就足够了。因此，我决定现在停止花时间在训练上，这样我就可以专注于进一步的实验。

为此，我想将模型保存到磁盘(即映射的 Google Drive)，这样每当我重新打开 Colab 时，我就可以加载模型并从我离开的地方继续。这比预期的要困难得多。我不得不尝试各种组合，然后才能满足于一个折衷的解决方案(使用 h5 格式保存到 Google Drive )。

1.  首先，我尝试在映射的 Google Drive 文件夹中使用 *model.save()* 。出现以下错误:

```
*UnimplementedError: File system scheme ‘[local]’ not implemented.*
```

2.显然，TPU 训练过的 SavedModel 模型只能保存到谷歌云存储中。这让我开始了在 google.cloud.storage API 和各种 gsutil 命令中尝试 blobs 的徒劳之旅。用这些工具将模型(二进制格式)保存到 GCS 似乎是不可能的。

最后，我想到了最简单的解决方案——简单地使用*GS://<bucket name>/<save folder>"*作为调用 *model.save* 的路径。虽然我可以保存它，装载它回来原来是一个全新的分支洞穴。

3.我在运行 *load_model* 时不断遇到这个错误:

```
*NotFoundError: Unsuccessful TensorSliceReader constructor*
```

显然，它无法在*变量/变量*文件夹中找到一些匹配的文件。这是真的，因为那个文件夹是空的，我不知道为什么 tensorflow 没有填充那个文件夹。那不是我能解决的问题。

假设这里引用的变量是优化器变量，我在保存时尝试了*include _ optimizer = False*。对错误没有影响。

4.接下来，我尝试了 h5 格式，理由是它是一个单独的文件，所以不需要处理丢失的文件夹和文件。这次尝试失败得很惨，因为这个错误导致我甚至无法保存文件:

```
*OSError: Unable to create file (unable to open file: name = ‘gs:<savepath>’, errno = 2, error message = ‘No such file or directory’, flags = 13, o_flags = 242)*
```

保存路径的目录存在，正在运行*！gsutil ls* 命令*T3 确认了这一点。该文件显然不存在，因为 tensorflow 尚未保存它。*

作为一个黑客实验，我复制了一个虚拟文件到那个位置，tensorflow 可以覆盖它。没什么区别。

5.接下来尝试将 h5 保存到本地映射的 Google Drive，并加载回来，它工作得非常好。虽然我仍然在检查使用“旧的”h5 格式会丢失什么，但这将是我目前默认的保存和加载方法。

我没有尝试的一个选项是只使用 *save_weights()* 调用，因为我想保存整个模型。

再说一次，虽然训练中表现出色，使用 TPU 的愿望非常强烈，但实现这一愿望的途径却是一个绝对的雷区。讨论这些问题的一些线程已经运行了多年。我相信在某个地方会有解决办法。但我希望谷歌团队能尽快坐下来，写一本合适的剧本，帮助我们解决这些问题。