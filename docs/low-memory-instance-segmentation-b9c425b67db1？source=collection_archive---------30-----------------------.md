# 低内存实例分段

> 原文：<https://towardsdatascience.com/low-memory-instance-segmentation-b9c425b67db1?source=collection_archive---------30----------------------->

## 将实例分段 web 应用程序部署到 google cloud

![](img/d89ef3737af7720a605826e0d1f1261d.png)

机器学习模型是内存密集型的。我当前的 web 应用程序至少消耗 1GB 的内存。这使得它很难部署到云上。

想到的直接解决方案是增加 VM 实例的内存。但是我不想花更多的钱。毕竟，这是一个附带项目。

所以我想出了一个办法。它实际上是受杰森·安蒂奇的 [deoldify](https://github.com/jantic/DeOldify/tree/master/deoldify) 的启发。在运行他的机器学习模型之前，Jason 使用渲染因子来缩小图像并将其转换为正方形。

我认为在这里应用同样的思想可以减少内存需求。

# 解决方案

简而言之，这就是解决方案。很大程度上取自这里的。

我们缩放到大小为`targ`的正方形。`targ`本质上就是 Jason 在 deoldify 中提到的渲染因子。

如果渲染因子太高，可能会导致 OOM 错误[。太低的话，结果的分辨率会很差。](https://github.com/jantic/DeOldify/blob/edac73edf1d3557f95a71f860cffd6c4c91f66f0/deoldify/filters.py#L58)

实际上这看起来很简单。首先，我们将图像缩放到一个大小为`targ`的正方形。然后我们对缩放图像进行推理。推理的结果通过`_unsquare`函数传递。这将我们的正方形大小的图像转换为原来的大小。

此后，我们终于可以在大多数云平台上部署，而不会出现任何面向对象的问题。如果你得到一个 OOM 错误，只要减少渲染因子。

我将使用这里描述的 web 应用程序[作为起点。](https://spiyer99.github.io/Detectron2-Web-App/)

我们需要修改`app.py`脚本来实现上面描述的`run_inference_transform`函数。修改后的版本在 [Github](https://github.com/spiyer99/detectron2_web_app/blob/master/app.py) 上。

有许多方法可以获得谷歌云的推广积分。所以我将在这个项目中使用他们的服务。我的计划是，我所做的一切都将由这些免费学分支付:)。

我在 google cloud 上创建了一个[项目](https://cloud.google.com/resource-manager/docs/creating-managing-projects),并最终部署工作。谷歌云[文档](https://cloud.google.com/run/docs/quickstarts/build-and-deploy)写得很好——但我还是会出错。我发现[这个回购](https://github.com/npatta01/web-deep-learning-classifier/)非常有用。

像往常一样，我创建了一个 shell 脚本来实现我们在终端中需要做的事情。

一旦我们在 google cloud 上部署了这个，detectron2 模型终于可以工作了。输出的分辨率低于输入，但一切都符合 2G 内存的限制。

在用`docker stats`构建之后，我可以确认这种方法使用的内存要少得多。在我的本地计算机上，它在峰值时使用 1.16 GB 的内存。而之前的方法使用超过 2GB(大约。2.23 GB)。

你可以点击访问[谷歌云实例分割 web 应用。](https://neelsmlapp-lfoa57ljxa-uc.a.run.app/)

*原载于 2020 年 7 月 26 日*[*https://spiyer 99 . github . io*](https://spiyer99.github.io/Detectron2-Web-App-gcloud/)*。*