# 深度学习-为数据集准备图像

> 原文：<https://towardsdatascience.com/deep-learning-prepare-image-for-dataset-d20d0c4e30de?source=collection_archive---------22----------------------->

## 获取数据集图像的简单方法

![](img/874a15db1f3e8cb772ee8b753182eb13.png)

每当我们开始一个机器学习项目时，我们首先需要的是一个数据集。数据集将是您培训模型的支柱。您可以自动或手动构建数据集。在这里，我将分享关于手动过程。

# 什么是数据集？

数据集是满足您的 ML 项目需求的特定数据的集合。数据的类型取决于你需要训练的人工智能的类型。基本上，你有两个数据集:

*   **训练**
*   **测试**

> 分别为 90%和 10%的成分

每当你训练一个定制模型时，重要的是**图像**。是的，当然图像在深度学习中起着主要作用。你的模型的准确性将基于训练图像。所以，在你训练一个定制模型之前，你需要计划**如何获取图像？**在这里，我将分享我关于获取数据集图像的简单方法的想法。

# 从谷歌获取图片

是的，我们可以从谷歌上获取图片。使用 [**下载所有图片**](https://chrome.google.com/webstore/detail/download-all-images/ifipmflagepipjokmbdecpmjbibjnakm) 浏览器扩展我们可以在几分钟内轻松获得图片。你可以在这里查看关于这个扩展的更多细节！各大浏览器都有。遗憾的是，Safari 浏览器不支持该扩展。

*   [铬合金](https://chrome.google.com/webstore/detail/download-all-images/nnffbdeachhbpfapjklmpnmjcgamcdmm?hl=en)
*   [火狐](https://addons.mozilla.org/en-US/firefox/addon/save-all-images-webextension/)
*   [歌剧](https://addons.opera.com/en/extensions/details/save-all-images/)

一旦你使用这个扩展下载图像，你会看到下载的图像在一个文件夹中，文件名是随机的。我们可以重命名文件或删除。png 文件，使用下面的 Python 脚本。

[](https://chrome.google.com/webstore/detail/download-all-images/ifipmflagepipjokmbdecpmjbibjnakm) [## 下载所有图像

### 将活动选项卡中的所有图像保存为. zip 文件。轻松保存来自 Instagram、谷歌图片等的照片。

chrome.google.com](https://chrome.google.com/webstore/detail/download-all-images/ifipmflagepipjokmbdecpmjbibjnakm) 

**从下载的图像文件夹中删除 png。**

**重命名您的图像文件**

# 从视频中获取图像

这里我们有另一种方法来为数据集准备图像。我们可以很容易地从视频文件中提取图像。 **Detecto** 给出了从视频中获取图像的简单解决方案。更多信息参见 [**探测器**](https://github.com/alankbi/detecto) 。

```
pip3 install detecto
```

使用下面的代码，我们可以从视频文件中提取图像。

# 使用你的一组图像

您可以拍摄将用于训练模型的对象的照片。重要的是要确保你的图像不超过 800x600。这将有助于您的数据集训练更快。

我准备了一个视频，并解释了上述过程。请查看下面的视频博客。

**为数据集准备图像的视频博客**

# 结论

作为一名 ML noob，我需要找出为训练模型准备数据集的最佳方法。希望这能有用。我的最终想法是为这个过程创建一个 Python 包。:)

是的，我会想出我的下一篇文章！

***原载于***[**www.spritle.com**](https://www.spritle.com/blogs/2020/05/25/preparing-the-dataset-for-deep-learning/)