# 如何使用 Python 下载文件

> 原文：<https://towardsdatascience.com/how-to-download-files-using-python-ffbca63beb5c?source=collection_archive---------4----------------------->

![](img/10154e7747f3d022e1b3cb3c45229d74.png)

émile Perron 未喷涂的图像

## 权威指南

## 了解如何使用 python 下载 web 抓取项目中的文件

P ython 非常适合在互联网上抓取网页，但在从我想做的网站上抓取一些标题或链接后，我的首要任务之一就是下载文件。我需要一种方法来自动化这个过程！

这里我们将概述如何做到这一点。

# 到本文结束时，您将

1.  注意 python 中 HTTP 处理包的选择

2.详细了解请求包

2.知道如何使用请求包下载文件

3.如何用请求包处理大文件？

4.如何下载使用请求包重定向的文件。

python 中有很多处理互联网的包。你没有必要知道所有这些，但是让你知道为什么人们会选择其中一个。

下面是处理 HTTP 请求的不同包。

*   内置包:urllib 和 urllib2、urllib3
*   请求(基于 urllib3 包)
*   grequests(扩展请求以处理异步 HTTP 请求)
*   aiohttp(另一个处理异步 http 的包)

你可能会问同步请求和异步请求有什么区别？为什么这很重要？

同步请求阻塞客户端(浏览器)，直到操作完成。这意味着有时 CPU 什么也不做，会浪费计算时间。在我看来效率很高！

异步请求不会阻塞浏览器，这允许客户端同时执行其他任务。这允许轻松扩展 1000 个请求。

url-lib 和 url-lib2 包有许多样板文件，有时可能有点难以阅读。我使用 requests 包，因为它是可读的，并且能够管理您无论如何都需要发出的大多数 HTTP 请求。

当您有大量 HTTP 请求时，异步包非常有用。这是一个复杂的话题，但是可以提高 python 脚本的效率。我会在后面的文章中回到这一点！

# 请求包介绍

要使用请求包，我们必须导入请求模块。然后，我们可以使用一系列方法与互联网互动。使用请求包的最常见方式是使用 requests.get 方法。在幕后，它对选择的 URL 执行 HTTP GET 请求。

首先，我们创建一个发送到服务器的请求对象，然后服务器发回一个响应。这个对象携带关于请求的所有数据。

```
import requestsurl = 'PLEASE INSERT URL LINK'
html = requests.get(url)
```

要访问这个对象，我们可以调用 text 方法。这将允许我们看到字符串形式的响应。请求根据从服务器返回的数据进行编码。

我们收到的信息有两部分，头和主体。标题为我们提供了关于响应的信息。可以把邮件头想象成将邮件发送到计算机所需的所有信息。

请看下面一个来自媒体标题的例子！有很多信息告诉我们关于反应。

```
{'Date': 'Thu, 30 Jan 2020 17:06:12 GMT', 
'Content-Type': 'text/html; charset=utf-8', 
'Transfer-Encoding': 'chunked', 
'Connection': 'keep-alive', 
'Set-Cookie': 'uid=lo_NHI3i4bLD514; Expires=Fri, 29-Jan-21 17:06:12 GMT; Domain=.medium.com; Path=/; Secure; HttpOnly, 
optimizelyEndUserId=lo_NHI3i4bLD514; path=/; expires=Fri, 29 Jan 2021 17:06:12 GMT; domain=.medium.com; samesite=none; secure, 
sid=1:Hu5pQRgkgEyZr7Iq5hNn6Sns/FKPUZaBJBtDCMI+nmsU48zG2lXM+dtrtlefPkfv; path=/; expires=Fri, 29 Jan 2021 17:06:12 GMT; domain=.medium.com; samesite=none; secure; httponly', 
'Sepia-Upstream': 'production', 
'x-frame-options': 'allow-from medium.com', 
'cache-control': 'no-cache, 
no-store, max-age=0, must-revalidate', 
'medium-fulfilled-by': 'lite/master-20200129-184608-2156addefa, rito/master-20200129-204844-eee64d76ba, tutu/medium-39848', 'etag': 'W/"179e9-KtF+IBtxWFdtJWnZeOZBkcF8rX8"', 
'vary': 'Accept-Encoding',
 'content-encoding': 'gzip', 
'x-envoy-upstream-service-time': '162', 
'Strict-Transport-Security': 'max-age=15552000; includeSubDomains; preload', 
'CF-Cache-Status': 'DYNAMIC', 
'Expect-CT': 'max-age=604800, 
report-uri="https://report-uri.cloudflare.com/cdn-cgi/beacon/expect-ct"', 'Alt-Svc': 'h3-24=":443"; ma=86400, h3-23=":443"; ma=86400', 'X-Content-Type-Options': 'nosniff', 
'Server': 'cloudflare', 
'CF-RAY': '55d508f64e9234c2-LHR'}
```

request package get 方法下载响应的正文，而不要求许可。这将与下一节相关！

为了下载文件，我们希望以字节而不是字符串的形式获取请求对象。为此，我们调用 response.content 方法，这样可以确保我们接收的数据是字节格式的。

现在要写一个文件，我们可以使用一个 open 函数，它是 python 内置函数的样板文件。我们指定文件名，而“wb”指的是写入字节。Python 3 需要明确地知道数据是否是二进制的，这就是我们定义它的原因！

然后，我们使用 write 方法写入 get 请求的已定义二进制内容。

```
with open('filename.txt', 'wb') as r: 
    r.write(html.content)
```

with 语句打开了所谓的上下文管理器。这很有用，因为它将关闭 open 函数，而不需要额外的代码。否则，我们将不得不请求关闭 open 函数。我们不必使用 with 语句。

# 根据请求下载大文件

我们已经讨论了使用请求包下载的基本方法。get 方法参数帮助定义我们如何从服务器请求信息。我们可以用许多方法改变请求。请参阅请求文档以了解更多详细信息。

我们说过请求下载二进制文件的主体，除非另有说明。这可以通过定义流参数来覆盖。这在请求文档中的标题“正文内容工作流”下。见[此处](https://2.python-requests.org/en/master/user/advanced/#id7)了解详情。这是一种控制何时下载二进制文件主体的方法。

```
request.get(url, stream=True)
```

在脚本的这一点上，只有二进制文件的头被下载。现在，我们可以通过一个名为 request.iter_content 的方法来控制如何下载文件。这个方法将整个文件停止在内存(缓存)中。

在后台，iter_content 方法迭代响应对象。然后，您可以指定 chunk_size，这是我们定义要放入内存的大小。这意味着在所有数据传输完成之前，连接不会关闭。

详情见[此处](https://2.python-requests.org//en/master/api/#requests.Response.iter_content)。

```
r = requests.get(url, Stream=True)
with open("filename.pdf",'wb') as Pypdf:
    for chunk in r.iter_content(chunk_size=1024)
      if chunk: 
         pypdf.write(ch)
```

因此，这里我们使用一个请求 get 方法来获取内容。我们使用 with 语句作为上下文管理器，并调用 r.iter_content。我们使用 for 循环并定义变量 chunk，这个 chunk 变量将包含由 chunk_size 定义的每 1024 个字节。

我们将 chunk_size 设置为 1024 字节，如果需要，可以是任何值。

我们在将数据块放入内存的同时写入数据块。我们使用 if 语句查找是否有块要写，如果有，我们使用 write 方法来完成。这使得我们不会用尽所有的缓存，并以零碎的方式下载较大的文件。

# 下载重定向的文件

有时候你想下载一个文件，但网站重定向到检索该文件。请求包可以轻松处理这个问题。

```
import requests
url = 'insert url'
response = requests.get(url, allow_redirects=True)
with open('filename.pdf') as Pypdf:
    pypdf.write(response.content)
```

这里我们在 get 方法中使用 allow_redirects=True 参数。我们像前面一样使用 with 语句来编写文件。

本文到此为止！敬请期待下一部分。我们将看看验证下载，恢复下载和编码进度条！

在后面的文章中，我们将讨论异步技术。这些可以扩大下载更大的文件集！

**关于作者**

我是一名医学博士，对教学、python、技术和医疗保健有浓厚的兴趣。我在英国，我教在线临床教育以及运行 www.coding-medics.com[网站。](http://www.coding-medics.com./)

您可以通过 asmith53@ed.ac.uk 或 twitter [这里](https://twitter.com/AaronSm46722627)联系我，欢迎所有意见和建议！如果你想谈论任何项目或合作，这将是伟大的。

更多技术/编码相关内容，请点击这里注册我的简讯[。](https://aaronsmith.substack.com/p/coming-soon?r=6yuie&utm_campaign=post&utm_medium=web&utm_source=copy)

[](https://medium.com/analytics-vidhya/everything-you-need-to-know-about-enumerate-a49a7fd0d756) [## 关于 Enumerate()您需要知道的一切

### 使用 python 的枚举函数永远改变你的循环方式

medium.com](https://medium.com/analytics-vidhya/everything-you-need-to-know-about-enumerate-a49a7fd0d756)