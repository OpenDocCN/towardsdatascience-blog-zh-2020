# 用 Python 构建一个 YouTube 下载器

> 原文：<https://towardsdatascience.com/build-a-youtube-downloader-with-python-8ef2e6915d97?source=collection_archive---------1----------------------->

## 在 PyTube3 的帮助下，学习使用 Python 构建一个简单的 YouTube 下载器

![](img/114f0e4a65419670b1ba34a03202b55e.png)

Kon Karampelas 在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄的照片

读者你好！今天，我们将使用 PyTube3 库在 Python3 中构建一个 YouTube 下载器。原来的 pytube 库不再工作，所以我们需要使用 pytube3 库，它只适用于 Python3，不适用于 Python2。

我们将看到我们可以用 Youtube 下载器做的各种事情，以及它为我们提供的各种功能。所以，我们一步一步来。

## 下载和导入库

首先，在做任何事情之前，您需要在您的系统中下载 pytube3 库。为此，我们将使用 python3。

在 CLI 中键入以下命令，在您的系统中下载并安装 pytube3。

```
pip install pytube3
```

该命令将在您的系统中下载并安装 pytube3。

现在我们可以开始构建我们的 YouTube 下载器了。我们需要将这个库导入到我们的程序中来使用它的功能。

因此，我们用下面的命令开始我们的程序:

```
from pytube import YouTube
```

你会注意到，虽然我们在系统中下载并安装了 pytube3，但是我们在代码中导入了 pytube。

为了消除混淆，pytube3 也是通过只编写 pytube 来导入的。我们不会通过将它写成 pytube3 来导入它。

## 接受用户的输入

我们的下一步是要求用户提供我们需要下载的 youtube 视频的链接。然后，用户将向我们提供他想要下载的视频的链接。

```
link = input("Enter the link: ")
yt = YouTube(link)
```

因此，我们接受了用户的输入，并将链接传递给我们的 YouTube 类。它将帮助我们揭示视频的所有信息，并让我们下载它。

## 揭示关于视频的各种信息

现在我们有了链接，并且我们已经将它传递给了 YouTube 类。现在，我们可以玩链接，并显示视频的各种信息，如标题，观看次数，收视率，描述，视频长度和其他各种事情。

```
#Title of video
print("Title: ",yt.title)
#Number of views of video
print("Number of views: ",yt.views)
#Length of the video
print("Length of video: ",yt.length,"seconds")
#Description of video
print("Description: ",yt.description)
#Rating
print("Ratings: ",yt.rating)
```

当我们运行这段代码时，我们将看到关于视频的各种细节，我们已经将视频的链接输入到程序中。此外，还有更多这样的操作可以执行，您可以在 pytube3 的官方文档中找到。

因此，出于输出的目的，我们不打印描述(它很大)，所以我们打印其余的四样东西。

我们在这里使用了《黑暗》第三季官方预告片的链接。您可以使用自己选择的任何链接。

显示视频各种细节的输出

所以，正如你在上面看到的，我们已经打印了关于这个节目的各种细节。

## 查看可用的流

你一定已经看到 youtube 上有各种各样的质量可供观看。因此，在使用 pytube 下载的同时，我们还可以获得所有可用流的选项。

pytube 提供了一种非常简单的方法来查看用户提供的链接的所有可用流。因此，让我们运行代码来查看该特定视频的所有可用流。

```
#printing all the available streams
print(yt.streams)
```

在运行上面的代码时，我们获得了该视频的所有可用流。下面是生成的输出:

视频的可用流

现在，你可以看到视频和音频流。所以，你也可以只过滤掉音频或视频流。您还可以根据文件格式过滤掉流。我们还可以过滤出渐进流和破折号流(稍后会谈到它们)。

所以，让我们过滤掉只有音频的流。为此，我们需要编写如下代码:

```
print(yt.streams.filter(only_audio=True))
```

我们将得到的输出如下:

纯音频流

现在，让我们过滤掉只有视频的流。它将只显示包含视频但没有音频的流。所以，它也会过滤掉所有的渐进流。为此，我们将这样写:

```
print(yt.streams.filter(only_video=True))
```

我们将得到的输出如下:

仅视频流

现在，让我们来谈谈渐进的 v/s 破折号流。YouTube 使用 Dash 流进行更高质量的渲染。

渐进流被限制为 720p，并包含音频和视频编解码器文件，而 Dash 流具有更高的质量，但只有视频编解码器。

因此，如果我们想下载一个渐进流，我们将得到一个内置音频的现成视频。

但是，为了更高的质量，我们应该为视频使用 Dash 流，还应该下载一个音频流，然后使用任何混合工具合并它们。

因此，对于本文，我们将使用渐进式流下载来准备播放视频。你可以自由选择你的下载质量和选择流。

所以，我们先过滤掉渐进流。下面的代码将为我们做这件事:

```
print(yt.streams.filter(progressive=True))
```

它将列出可供我们下载的渐进流。它将有有限的选择，但它为我们做了工作。输出将如下所示:

可用的累进流

为了获得最高分辨率的渐进流，我们可以编写下面的代码:

```
ys = yt.streams.get_highest_resolution()
```

这将在 ys 变量中创建并存储最高分辨率的流。

我们也可以在 itag 的帮助下选择任何流。

```
ys = yt.streams.get_by_itag('22')
```

因此，现在我们已经将首选流存储在一个变量中。现在，让我们把它下载到我们的系统。

```
ys.download()
```

上面的代码将下载我们的首选流，并将其保存在当前的工作目录中。

我们还可以在系统中指定下载 youtube 视频的位置。我们可以通过在下载的大括号之间指定绝对路径来做到这一点。

下面的代码帮助您将它下载到您喜欢的位置。

```
ys.download('location')
```

仅此而已！恭喜你，你已经使用 Python 构建了你的第一个简单的 YouTube 下载器。仅用于测试和教育目的。不要滥用这些知识。

以下是使用最高质量的渐进流下载 youtube 视频的完整代码:

YouTube 下载器的完整代码

请访问我的 [GitHub 库](https://github.com/shubham1710/Youtube-downloader)了解更多细节和更新。我鼓励你们尝试一些新的东西，然后请在评论中分享你们的想法和经验。我很想听听你学到了什么，还做了些什么。祝大家一切顺利！

看，我们在这里建立的是一个非常简单的版本。我们还可以尝试将相同的概念转换成一个应用程序或网站，以用户友好的方式执行相同的功能。您还可以通过使用不同网站各自的 API 来添加对不同网站的支持。有许多这样的功能丰富的视频下载软件，它们使用类似的概念，并给用户一个轻松下载视频的选项。

我最喜欢的一个是 [YTSaver](https://ytsaver.net/?affid=898166e1-0ae0-45ca-bbe8-6cf9b436673d) ，这是一个功能丰富的应用程序，可以一键从大量网站下载不同分辨率和格式的视频或播放列表。它还允许你将视频从一种格式转换成另一种格式，并且比其他下载者快得多。

> 注意:以上段落包含一个附属链接。

实际上，你也可以使用 Python 库[，比如这个](https://github.com/senko/python-video-converter)，将视频从一种格式转换成另一种格式。如果你有兴趣，也可以尝试这样做。非常感谢您的阅读！

读完这篇文章后，还有更多的故事值得一读

[](/build-a-blog-website-using-django-rest-framework-overview-part-1-1f847d53753f) [## 使用 Django Rest 框架构建博客网站——概述(第 1 部分)

### 让我们使用 Django Rest 框架构建一个简单的博客网站，以了解 DRF 和 REST APIs 是如何工作的，以及我们如何添加…

towardsdatascience.com](/build-a-blog-website-using-django-rest-framework-overview-part-1-1f847d53753f) [](https://shubhamstudent5.medium.com/build-an-e-commerce-website-with-mern-stack-part-1-setting-up-the-project-eecd710e2696) [## 用 MERN 堆栈构建一个电子商务网站——第 1 部分(设置项目)

### 让我们使用 MERN 堆栈(MongoDB，Express，React 和 Node)建立一个简单的电子商务网站，用户可以在其中添加项目…

shubhamstudent5.medium.com](https://shubhamstudent5.medium.com/build-an-e-commerce-website-with-mern-stack-part-1-setting-up-the-project-eecd710e2696) [](https://shubhamstudent5.medium.com/build-a-job-search-portal-with-django-overview-part-1-bec74d3b6f4e) [## 用 Django 构建求职门户——概述(第 1 部分)

### 让我们使用 Django 建立一个工作搜索门户，它允许招聘人员发布工作并接受候选人，同时…

shubhamstudent5.medium.com](https://shubhamstudent5.medium.com/build-a-job-search-portal-with-django-overview-part-1-bec74d3b6f4e) [](/build-a-social-media-website-using-django-setup-the-project-part-1-6e1932c9f221) [## 使用 Django 构建一个社交媒体网站——设置项目(第 1 部分)

### 在第一部分中，我们通过设置密码来集中设置我们的项目和安装所需的组件…

towardsdatascience.com](/build-a-social-media-website-using-django-setup-the-project-part-1-6e1932c9f221)