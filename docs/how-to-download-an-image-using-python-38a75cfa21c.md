# 如何使用 Python 下载图像

> 原文：<https://towardsdatascience.com/how-to-download-an-image-using-python-38a75cfa21c?source=collection_archive---------1----------------------->

## 了解如何使用 Python 模块(如 request、urllib 和 wget)下载图像文件。

来源: [Giphy](https://giphy.com/gifs/food-pizza-9Wh7TNThCgWti)

最近，我在用一个[远程系统](https://docs.oracle.com/cd/E23824_01/html/821-1454/wwrsov-3.html)工作，需要下载一些我的代码最终会处理的图像。

我可以在我的终端上使用 [curl 或者 wget](https://ubuntuforums.org/showthread.php?t=1942128) 来下载文件。但是，我希望最终用户的整个过程是自动化的。

这让我想到了一个问题:

> 如何使用 Python 下载图像？

在本教程中，我将介绍几个可用于下载 Python 文件(特别是图像)的模块。涵盖的模块有: [**请求**](https://2.python-requests.org/en/master/) 、 [**wget**](https://pypi.org/project/wget/) 、 [**urllib**](https://docs.python.org/3/library/urllib.html) 。

> 免责声明:不要下载或使用任何违反其版权条款的图像。

# 设置

本教程中使用的代码是在安装了 [Python 3.6.9](https://www.python.org/downloads/release/python-369/) 的 Ubuntu 系统上测试的。

我强烈推荐建立一个[虚拟环境](https://realpython.com/python-virtual-environments-a-primer/)，其中包含所有测试所需的库。这是你可以做到的。

```
$ virtualenv image_download
$ source ./image_download/bin/activate
$ pip3 install requests wget
```

我们将在教程中使用的图像位于[这里](https://pixabay.com/photos/summer-vacation-dog-journey-4823612/)。**是** [**免费用于商业用途，不需要任何归属**](https://pixabay.com/service/license/) **。**

![](img/deea9e68dab16f15f67f0480136c93cb.png)

图片来自 [Pixabay](https://pixabay.com/?utm_source=link-attribution&utm_medium=referral&utm_campaign=image&utm_content=4823612) 的[肖恩·韦林](https://pixabay.com/users/seanwareing-8721416/?utm_source=link-attribution&utm_medium=referral&utm_campaign=image&utm_content=4823612)

我们将用于下载的**图像 URL** 将是:

```
[https://cdn.pixabay.com/photo/2020/02/06/09/39/summer-4823612_960_720.jpg](https://cdn.pixabay.com/photo/2020/02/06/09/39/summer-4823612_960_720.jpg)
```

> 我们将创建一个简短的脚本来从给定的 URL 下载图像。

脚本将下载与脚本文件相邻的图像，并且*可选地，保留原始文件名*。

# 请求模块

[**请求**](https://2.python-requests.org/en/master/) 是 Python 中一个整洁且用户友好的 **HTTP 库**。它使得发送 HTTP/1.1 请求变得非常简单。

**这似乎是使用 Python 下载任何类型文件的最稳定和推荐的方法。**

来源: [Giphy](https://giphy.com/gifs/reactionseditor-yes-awesome-3ohzdIuqJoo8QdKlnW)

这是完整的代码。

来源:作者

不要担心。让我们一行一行地分解它。

我们将从导入必要的模块开始，还将设置图像 URL。

```
import requests # to get image from the web
import shutil # to save it locallyimage_url = "[https://cdn.pixabay.com/photo/2020/02/06/09/39/summer-4823612_960_720.jpg](https://cdn.pixabay.com/photo/2020/02/06/09/39/summer-4823612_960_720.jpg)"
```

我们使用切片符号**将文件名从图像链接**中分离出来。我们使用正斜杠(`/`)分割图像 URL，然后使用`[-1]`分割最后一段。

```
filename = image_url.split("/")[-1]
```

来自请求模块的`get()`方法将用于检索图像。

```
r = requests.get(image_url, stream = True)
```

> 使用`stream = True`来保证没有中断。

现在，我们将以二进制写入模式在本地创建文件，并使用`copyfileobj()`方法将我们的映像写入文件。

```
# Set decode_content value to True, otherwise the downloaded image file's size will be zero.
r.raw.decode_content = True# Open a local file with wb ( write binary ) permission.
with open(filename,'wb') as f:
        shutil.copyfileobj(r.raw, f)
```

我们还可以使用 [**请求的状态代码**](https://www.tutorialspoint.com/python_network_programming/python_request_status_codes.htm) 添加某些条件来检查图像是否被成功检索。

我们还可以在下载大文件或者大量文件时通过 [**增加进度条**](/a-complete-guide-to-using-progress-bars-in-python-aa7f4130cda8) 来进一步改进。[这里的](https://stackoverflow.com/questions/37573483/progress-bar-while-download-file-over-http-with-requests/37573701)就是一个很好的例子。

> **Requests 是使用 Python 下载任何类型文件的最稳定和推荐的方法。**

# Wget 模块

除了 python 请求模块，我们还可以使用 [**python wget 模块**](https://pypi.org/project/wget/#description) 进行下载。

这是 python 中相当于 [**GNU wget**](https://www.gnu.org/software/wget/) 的。

使用起来相当简单。

来源:作者

# Urllib 模块

通过你的程序访问网站的**标准 Python 库**是 [**urllib**](https://docs.python.org/2/library/urllib.html) 。请求模块也使用它。

通过 urllib，我们可以做多种事情:**访问网站**，**下载数据**，**解析数据**，**发送 GET 和，POST 请求**。

我们可以使用几行代码下载我们的图像:

我们使用了 [**urlretrieve**](https://docs.python.org/2/library/urllib.html#urllib.urlretrieve) 方法将所需的 web 资源复制到本地文件中。

需要注意的是，在一些系统和很多网站上，上面的代码会导致一个错误:[**HTTP Error:HTTP Error 403:Forbidden**](https://www.howtogeek.com/357785/what-is-a-403-forbidden-error-and-how-can-i-fix-it/)。

这是因为很多网站不喜欢随机程序访问他们的数据。一些程序可以通过发送大量请求来攻击服务器。这将阻止服务器运行。

这就是为什么这些网站可以:

1.  屏蔽你你会收到 [**HTTP 错误 403**](https://www.howtogeek.com/357785/what-is-a-403-forbidden-error-and-how-can-i-fix-it/) **。**
2.  向您发送不同的数据或空数据。

我们可以通过修改**用户代理**来克服这个问题，这是一个随我们的请求一起发送的变量。默认情况下，这个变量告诉网站访问者是一个 python 程序。

通过修改这个变量，我们可以像普通用户在标准的 web 浏览器上访问网站一样。

你可以在这里阅读更多关于它的[。](https://pythonprogramming.net/urllib-tutorial-python-3/)

# 结论

[**请求**](https://2.python-requests.org/en/master/) 已经成为 Python 中事实上的下载东西的方式。

甚至 [**urllib 文档页面**](https://docs.python.org/3/library/urllib.request.html) **都推荐请求**更高级别的 HTTP 客户端接口。

如果你想在你的程序中有更少的依赖项，你应该选择 urllib。它是标准库的一部分。所以，没必要下载。

希望你觉得这个教程有用。

# 重要链接

1.  [StackOverflow —通过 Urllib 下载图像并请求](https://stackoverflow.com/questions/3042757/downloading-a-picture-via-urllib-and-python)
2.  [请求库](https://2.python-requests.org/en/master/)
3.  [wget 库](https://pypi.org/project/wget/)
4.  [urllib 库](https://docs.python.org/3/library/urllib.html)