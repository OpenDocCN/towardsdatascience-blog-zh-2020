# Scrapy 中我最喜欢的三个新特性

> 原文：<https://towardsdatascience.com/my-favourite-three-new-features-in-scrapy-8470cba87d9f?source=collection_archive---------47----------------------->

![](img/9282011d3141b03e34d195fd570a4dc0.png)

[https://unsplash.com/@kmuza](https://unsplash.com/@kmuza)

## 找出 scrapy 在 2.2 版本中提供的新的网页抓取功能

Scrapy 的新版本上周刚刚发布！在这篇文章中，我将深入探讨 2.2 版本中框架的变化，以及 Scrapy 为您的网络抓取需求提供了哪些很酷的新功能！

有关可用内容的详细信息以及对这些已实现功能的讨论，请参见发布说明[此处](https://docs.scrapy.org/en/latest/news.html)。

## 在本文中，您将了解到

1.  如何直观地用 scrapy 处理 json
2.  你能从一个文件管道中得到什么信息关于你下载的文件。
3.  类型提示和 Scrapy

# 新的零碎功能

## 去序列化 JSON 响应

Scrapy 现在有能力直接从提供 json 数据的网站上解码 json。在此之前，我们必须使用 python 内置的 json 包。这个新特性受到了请求包方法`requests.json()`的影响。

Scrapy 现在可以直接从服务器响应中解码 json 作为 python 对象。在幕后，Scrapy 导入标准库 json 包并调用`json.loads()`方法。该方法将获取一个 json 字符串，并将其转换成 python 字典。有关 json 库的概述，请参见这里的。

让我们看一个 scrapy shell 中的例子

```
>>> fetch('[https://api.github.com/events'](https://api.github.com/events'))
2020-06-27 07:16:38 [scrapy.core.engine] INFO: Spider opened
2020-06-27 07:16:38 [scrapy.core.engine] DEBUG: Crawled (200) <GET [https://api.github.com/events](https://api.github.com/events)> (referer: None)
>>> data = response.json()
>>> print(data[0])
{'id': '12750675925', 'type': 'CreateEvent', 'actor': {'id': 51508147, 'login': 'LLGLSS', 'display_login': 'LLGLSS', 'gravatar_id': '', 'url': '[https://api.github.com/users/LLGLSS'](https://api.github.com/users/LLGLSS'), 'avatar_url': '[https://avatars.githubusercontent.com/u/51508147?'](https://avatars.githubusercontent.com/u/51508147?')}, 'repo': {'id': 275306349, 'name': 'LLGLSS/hsshoping', 'url': '[https://api.github.com/repos/LLGLSS/hsshoping'](https://api.github.com/repos/LLGLSS/hsshoping')}, 'payload': {'ref': 'llg', 'ref_type': 'branch', 'master_branch': 'master', 'description': None, 'pusher_type': 'user'}, 'public': True, 'created_at': '2020-06-27T06:11:39Z'}
```

1.  在 scrapy shell 中，fetch()类似于 HTTP GET 请求，它获取经过解析的 html。
2.  现在，如果这个响应是一个 json 字符串，那么 response.json()方法会将 json 解析到一个字典中

这使得处理 json 比以前更加流畅，太棒了！继续前进，在你的新的网络抓取项目中使用它。

![](img/e4d2ded473b1e55a438759ddcf4c4993.png)

[https://unsplash.com/@ev](https://unsplash.com/@ev)

## 下载文件的状态

当使用 Scrapy 下载一个文件时，你会使用所谓的文件管道。管道只是 http 请求将经历的一系列操作，这些操作要么被处理，要么不被处理。

尝试下载文件或完成下载文件后，文件管道填充包含文件路径、校验和等信息的字典。当文件管道完成时，关于下载的信息存储在一个名为`results`的变量中，该变量包含一个 2 元素元组列表`(success, file_info_or_error)`。

`success`变量要么取真值，要么取假值，而`file_info_or_error`变量是关于这个文件的信息字典。

如果`success`变量为真。一个名为`status`的`file_info_or_error`字典的新键现在已经被添加，它告诉我们我们的文件是否已经被下载。该状态键可以取三个值之一

1.  下载
2.  已更新—文件未被下载，因为它最近被下载过
3.  缓存—文件已经计划下载

如果`success`变量为假，这意味着文件还没有被下载，并且`file_info_or_error`字典会忽略这个错误。

文件管道创建的`results`变量的一个例子是这样的。

```
[
   (True, {'checksum': '2b00042f7481c7b056c4b410d28f33cf',             'path': 'full/0a79c461a4062ac383dc4fade7bc09f1384a3910.jpg',             'url': 'http://www.example.com/files/product1.pdf'}),             'url': 'http://www.example.com/files/product1.pdf',             'status': 'downloaded'}),           
   (False,            Failure(...))
]
```

1.  第一个元组显示在这种情况下文件被成功下载`success = true`
2.  第一个元组的第二项给了我们`file_info_or_error` 字典，告诉我们这个下载的文件。
3.  第二个元组显示了`success = false`，并开始在`file_info_or_error`变量中提供解释。

当您创建自己的下载管道，并希望检查文件的状态或记录有多少文件没有下载以及具体原因时，这将非常有用。

为了在您想要下载的文件上使用这些信息，有必要创建您自己的管道。为此，我们创建了一个覆盖 pipeline 类 scrapy 提供的文件的函数，这些函数写在 scrapy 项目的 pipeline.py 中。有关如何解决这一问题的更多信息，请参见此处的。

然后你可以覆盖在`filepipeline`类中找到的名为`items_complete()`的方法，用 scrapy 下载文件。该方法将`results`变量作为参数。然后可以访问这些文件，每个文件中的信息可以用于您自己的管道需求。

![](img/8b73ac743984b8631e43a887c05015a5.png)

[https://unsplash.com/@kellysikkema](https://unsplash.com/@kellysikkema)

# 杂乱和类型提示

Scrapy 的新版本要求安装 Python 3.5.2 及以上版本，框架才能运行。你可能会问为什么会这样？Python 3.5 版本引入了一种叫做类型提示的东西，它的意思就是你所认为的意思！类型提示是一种将类型对象信息添加到语言中用于注释目的的方法。

为了理解为什么这很重要，请看这个函数。

```
def send_request(request_data,'#]
                 headers,
                 user_id,
                 as_json):
   DOING SOMETHING
   RETURNING SOMETHING
```

与…相比

```
def send_request(request_data : Any,
                 headers: Optional[Dict[str, str]],
                 user_id: Optional[UserId] = None,
                 as_json: bool = True):
```

看看知道输入是什么以及函数中返回内容的可能性是多么容易。为什么发音变得清晰，我们确切地知道输入是什么，输出会是什么。Python 作为一种语言的起源是一种叫做动态类型语言的东西。也就是说，Python 直到运行时才知道对象类型，因此不适合在代码库中注释类型。

类型提示是一种使代码库更容易阅读和维护的方式，有人建议 Scrapy 应该为框架的新贡献者提供这种方式。Scrapy 中的类型提示是为了使代码可维护，因此 Python 3.5.2 版的要求是允许 Scrapy 代码库被类型提示。

请点击此处查看我的博客和其他帖子中关于项目的更多细节。更多技术/编码相关内容，请点击这里[订阅我的时事通讯](https://aaronsmith.substack.com/p/coming-soon?r=6yuie&utm_campaign=post&utm_medium=web&utm_source=copy)

我将非常感谢任何评论，或者如果你想与 python 合作或需要帮助，请联系我。如果你想和我联系，请在这里或者在[推特](http://www.twitter.com/@aaronsmithdev)上联系我。

# 相关文章

[](/approach-to-learning-python-f1c9a02024f8) [## 学习 Python 的方法

### 今天如何最大限度地学习 python

towardsdatascience.com](/approach-to-learning-python-f1c9a02024f8) [](/how-to-run-scrapy-from-a-script-ff07fd6b792b) [## 如何从脚本运行 Scrapy

### 忘记 scrapy 的框架，全部用使用 scrapy 的 python 脚本编写

towardsdatascience.com](/how-to-run-scrapy-from-a-script-ff07fd6b792b) [](/efficient-web-scraping-with-scrapy-571694d52a6) [## 使用 Scrapy 进行有效的网页抓取

### Scrapy 的新功能使您的刮削效率

towardsdatascience.com](/efficient-web-scraping-with-scrapy-571694d52a6)