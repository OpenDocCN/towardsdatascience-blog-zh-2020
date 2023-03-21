# 如何从脚本运行 Scrapy

> 原文：<https://towardsdatascience.com/how-to-run-scrapy-from-a-script-ff07fd6b792b?source=collection_archive---------13----------------------->

![](img/4a5c4c956b38b2b1260a37aec28c4c66.png)

[未飞溅](https://unsplash.com/@shottrotter)

## 忘掉 Scrapy 的框架，全部用使用 Scrapy 的 python 脚本来写。

Scrapy 是一个用于抓取项目的很好的框架。但是，您知道有一种方法可以直接从脚本运行 Scrapy 吗？

查看文档，有两种方法可以运行 Scrapy。使用 Scrapy API 或框架。

## 在本文中，您将了解到

1.  为什么你会用剧本里的 scrapy
2.  理解基本脚本，每次你想从一个单独的脚本访问 scrapy
3.  了解如何指定定制的 scrapy 设置
4.  了解如何指定 HTTP 请求供 scrapy 调用
5.  了解如何在一个脚本下使用 scrapy 处理这些 HTTP 响应。

## 为什么要用剧本里的 Scrapy？

Scrapy 可以用于繁重的刮擦工作，但是，有很多项目非常小，不需要使用整个 scrapy 框架。这就是在 python 脚本中使用 scrapy 的原因。不需要使用整个框架，你可以从一个 python 脚本中完成所有工作。

在充实一些必要的设置之前，让我们看看这个的基本情况。

![](img/9d415ba8eec66fcb2a19df44a624f464.png)

https://unsplash.com/@contentpixie

# 基本脚本

在 python 脚本中运行 scrapy 的关键是 CrawlerProcess 类。这是一个爬虫类模块。它提供了在 python 脚本中运行 scrapy 的引擎。在 CrawlerProcess 类代码中，导入了 python 的 twisted 框架。

Twisted 是一个 python 框架，用于输入和输出过程，例如 HTTP 请求。现在它通过一种叫做龙卷风事件反应堆的东西来做到这一点。Scrapy 是建立在 twisted 之上的！我们在这里不会涉及太多的细节，但是不用说，CrawlerProcess 类导入了一个 twisted reactor，它监听像多个 HTTP 请求这样的事件。这是 scrapy 工作的核心。

CrawlerProcess 假设 twisted 反应器没有被其他任何东西使用，例如另一个蜘蛛。
有了这些，我们就有了下面的代码。

```
import scrapy
from scrapy.crawler import CrawlerProcessclass TestSpider(scrapy.Spider):
    name = 'test'if __name__ == "__main__":
  process = CrawlerProcess()
  process.crawl(TestSpider)
  process.start()
```

1.  现在我们要使用 scrapy 框架，我们必须创建我们的蜘蛛，这是通过创建一个继承自 scrapy.Spider. scrapy 的类来完成的。蜘蛛是所有 scrapy 项目中我们必须衍生的最基本的蜘蛛。有了这个，我们必须给这个蜘蛛一个名字让它运行/蜘蛛将需要几个函数和一个 URL 来抓取，但是对于这个例子，我们将暂时省略它。
2.  现在你看`if __name__ == “__main__”`。这在 python 中被用作最佳实践。当我们编写一个脚本时，你希望它能够运行代码，而且能够将代码导入到其他地方。关于这一点的进一步讨论，请参见[这里的](https://stackoverflow.com/questions/419163/what-does-if-name-main-do)。
3.  我们首先实例化类 CrawlerProcess 来访问我们想要的函数。CrawlerProcess 有两个我们感兴趣的函数，爬行和启动
4.  我们使用爬行来启动我们创建的蜘蛛。然后，我们使用 start 函数启动 twisted reactor，这个引擎处理并监听我们想要的 HTTP 请求。

![](img/c9e0a22d48a1a7e092797137ec29d5de.png)

[https://unsplash.com/@jplenio](https://unsplash.com/@jplenio)

# 在设置中添加

scrapy 框架提供了一个设置列表，它将自动使用，但是为了使用 Scrapy API，我们必须明确地提供设置。我们定义的设置是我们如何定制我们的蜘蛛。蜘蛛。蜘蛛类有一个变量叫做`custom_settings`。现在这个变量可以用来覆盖 scrapy 自动使用的设置。我们必须为我们的设置创建一个字典来做到这一点，因为使用 scrapy 将`custom_settings`变量设置为无。

你可能想使用 scrapy 提供的一些或大部分设置，在这种情况下，你可以从那里复制它们。或者，可以在[这里](https://docs.scrapy.org/en/latest/topics/settings.html?highlight=custom_settings#topics-settings-ref)找到内置设置列表。

```
class TestSpider(scrapy.Spider):
    name = 'test'
    custom_settings = { 'DOWNLOD_DELAY': 1 }
```

![](img/e373d86457292de0d6c8946cff020aa1.png)

[https://unsplash.com/@ericjamesward](https://unsplash.com/@ericjamesward)

# 指定要抓取的 URL

我们已经展示了如何创建一个蜘蛛并定义设置，但是我们还没有指定任何要抓取的 URL，或者我们想要如何指定对我们想要从中获取数据的网站的请求。例如，参数、头和用户代理。
当我们创建 spider 时，我们还启动了一个叫做`start_requests()`的方法。这将为我们想要的任何 URL 创建请求。现在有两种方法可以使用这个方法。

1)通过定义`start_urls`属性
2)我们实现了我们的函数`start_requests`

最短的方法是通过定义`start_urls`。我们将它定义为我们想要获取的 URL 列表，通过指定这个变量，我们自动使用`start_requests()`遍历我们的每个 URL。

```
class TestSpider(scrapy.Spider):
    name = 'test'
    custom_settings = { 'DOWNLOD_DELAY': 1 }
    start_urls = ['URL1','URL2']
```

但是请注意，如果我们这样做，我们不能指定我们的头，参数或任何其他我们想随请求？这就是实现我们的`start_requests`方法的原因。

首先，我们定义我们希望与请求一致的变量。然后我们实现我们的`start_requests`方法，这样我们就可以利用我们想要的头和参数，以及我们想要响应去哪里。

```
class TestSpider(scrapy.Spider):
    name = 'test'
    custom_settings = { 'DOWNLOD_DELAY': 1 }
    headers = {} 
    params = {} def start_requests(self):
        yield scrapy.Requests(url, headers=headers, params=params) 
```

这里我们访问 Requests 方法，当给定一个 URL 时，它将发出 HTTP 请求并返回一个定义为`response`变量的响应。

你会注意到我们没有指定回调。也就是说，我们没有指定 scrapy 应该将`response` 发送到哪里，我们只是告诉它为我们获取请求。

让我们来解决这个问题，默认情况下，scrapy 希望回调方法是解析函数，但它可以是我们想要的任何东西。

```
class TestSpider(scrapy.Spider):
    name = 'test'
    custom_settings = { 'DOWNLOD_DELAY': 1 }
    headers = {} 
    params = {} def start_requests(self):
        yield scrapy.Requests(url, headers=headers, params=params,callback = self.parse) def parse(self,response):
       print(response.body)
```

这里我们定义了接受响应变量的函数`parse` ,记住这是在我们让 scrapy 执行 HTTP 请求时创建的。然后，我们要求 scrapy 打印响应正文。

至此，我们已经有了在 python 脚本中运行 scrapy 的基础。我们可以使用所有相同的方法，但我们只需要事先做一些配置。

# 练习

1.  为什么你会使用 scrapy 框架？在 python 脚本中导入 scrapy 什么时候有用？
2.  `CrawlerProcess`班是做什么的？
3.  您能回忆起 python 脚本中用于启动 scrapy 的基本脚本吗？
4.  如何在你的 python 脚本中添加 scrapy 设置？
5.  为什么你会使用`start_requests`函数而不是`start_urls` ？

请参见[这里的](http://www.coding-medic.com/)了解我在博客和其他帖子上关于项目的更多细节。更多技术/编码相关内容，请点击这里订阅我的简讯

我将非常感谢任何评论，或者如果你想与 python 合作或需要帮助，请联系我。如果你想和我联系，请在这里联系。

# 你可能喜欢的其他文章

[](https://medium.com/swlh/5-python-tricks-you-should-know-d4a8b32e04db) [## 你应该知道的 5 个 Python 技巧

### 如何轻松增强 python 的基础知识

medium.com](https://medium.com/swlh/5-python-tricks-you-should-know-d4a8b32e04db) [](/demystifying-scrapy-item-loaders-ffbc119d592a) [## 揭开杂乱物品装载器的神秘面纱

### 自动清理和扩展你的垃圾蜘蛛

towardsdatascience.com](/demystifying-scrapy-item-loaders-ffbc119d592a)