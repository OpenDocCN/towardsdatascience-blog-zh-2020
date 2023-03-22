# Python 中的 RESTful APIs

> 原文：<https://towardsdatascience.com/restful-apis-in-python-121d3763a0e4?source=collection_archive---------5----------------------->

## 什么是 RESTful APIs 和在 Python 中实现 GET

![](img/21ec862151871b8b407f3b40f3881c86.png)

[真诚媒体](https://unsplash.com/@sincerelymedia?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)在 [Unsplash](https://unsplash.com/s/photos/rest?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) 上拍摄的照片

API 是两个软件之间的假想契约。Web APIs 使得跨语言应用程序能够很好地工作。

应用程序编程接口通常用于从远程网站检索数据。为了向远程 web 服务器发出请求并检索数据，我们使用了提供 API 的 URL 端点。每个 URL 被称为一个**请求**，被发回的数据被称为一个**响应**。

RESTful API 是一个应用程序接口，它使用 *HTTP 请求*来获取、上传、发布和删除数据。基于 REST 的交互使用熟悉 HTTP 的任何人都熟悉的约束。并且交互使用标准的 HTTP 状态代码来传达它们的状态。

# 为什么使用 API 而不是可以下载的静态数据集？

1.  数据是不断变化的
2.  您想要更大的数据池中的一小部分

# API 请求

API 托管在 web 服务器上。

当我们在浏览器的地址栏中键入*www.google.com*时，计算机实际上是在向*www.google.com*服务器请求一个网页，然后它将这个网页返回给浏览器。

API 的工作方式非常相似，除了不是网络浏览器请求网页，而是程序请求数据。

这些数据以 JSON 格式返回。

为了获得数据，我们向 web 服务器发出请求。然后，服务器用数据进行回复。在 Python 中，我们利用`requests`库来做这件事。

# “请求”

最常用的请求是 GET 请求，用于检索数据。GET 请求将是本文的重点。

## 端点/路由

根端点是我们请求的 API 的起点。它完成了确定被请求的资源的路径。例如-[https://www.google.com/maps](https://www.google.com/maps)。
***/地图*** 是终点，*是起始网址。*

## *方法*

*   *获取—从服务器获取资源。*
*   *POST —在服务器上创建新资源。*
*   *上传、修补—更新服务器上的请求。*
*   *删除—从服务器中删除资源。*

## *状态代码*

*   ***1xx:信息:**传达传输协议级信息。*
*   ***2xx:成功:**客户的请求被成功接受。*
*   ***3xx:重定向:**客户端必须采取一些额外的动作来完成它们的请求。*
*   ***4xx:客户端错误:**客户端错误。*
*   ***5xx:服务器错误:**服务器端错误。*

## ***JSON 数据***

*JSON 是数据在 API 之间来回传递的主要格式，大多数 API 服务器将以 JSON 格式发送响应——包含一个 JSON 对象的单个字符串。*

*JSON 是一种将列表和字典等数据结构编码成字符串的方法，以确保它们易于被机器读取。*

*因此，列表和字典可以转换成 JSON，字符串可以转换成列表和字典。*

*JSON 看起来很像 Python 中的字典，存储了键值对。*

# *进入 Python*

*我们将实现一个单一案例场景，它弥补了大多数 GET 请求的场景。*

## *进口货*

```
*import json
import requests
import urllib.parse*
```

*一些 GET 请求由端点组成，这些端点具有不可选的路径参数，并且是端点的一部分，例如-***/volunteer/{ volunteerID }/badge/{ badgeID }****

**需要将 volunteerID* 和 *badgeID* 传入完整的 URL，以完成 GET 请求。*

*这就是我们利用`urllib.parse`的地方。*

*假设我们在一个 JSON 文件中有一个 volunteerIDs 和 badgeIDs 的列表，我们希望将它输入到端点中，调用 API 并获得结果信息。*

```
***#reading in the JSON file**
with open(‘volunteer_data.json’, ‘r’) as text_file_input:
    data=text_file_input.read()**#loading that file as a JSON object**
obj = json.loads(data)*
```

*下一步是设置 URL。*

```
*API_ENDPOINT = ‘https://registeredvolunteers.xyz.com/volunteer/{}/badge/{}'*
```

*现在，让我们运行一个`for`循环来调用 API。*

```
***#we pass on the arguments volunteerID and badgeID** 
for i in obj:
     r=requests.get(API_ENDPOINT.format(urllib.parse.quote(
i[‘volunteerID’]),
i[‘badgeID’])
)
     print(r.text)**#outside for loop**
text_file_input.close()*
```

*注意，如果你期待一个 JSON 输出，`r.text`会给你一个由 JSON 对象组成的字符串。为了从中提取 JSON 对象，我们:*

```
*output_json = json.loads(r.text)**#or directly use** r.json()*
```

*要获得请求的状态代码和状态代码的原因，*

```
*r.status_code
r.reason*
```

# *最终意见*

*上面的场景涵盖了 GET 请求的不同情况——是否需要传递 URL 参数，以及传入的这些参数是否存储在文件中；以及 GET 请求的不同属性，您可以研究这些属性来理解接收到的数据。*

*获得 REST APIs 的领先优势！*