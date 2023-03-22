# 如何用 Python 从 API 获取数据🐍

> 原文：<https://towardsdatascience.com/how-to-get-data-from-apis-with-python-dfb83fdc5b5b?source=collection_archive---------10----------------------->

## 数据科学家需要数据。没有数据，你就没有模型、仪表板或应用程序。在本文中，您将学习如何查询 API 来获取构建酷东西所需的数据！🚀

![](img/e82fcbc94cc770443c01fc397844222f.png)

资料来源:pixabay.com

说数据是[新石油](https://www.wired.com/insights/2014/07/data-new-oil-digital-economy/)有点老生常谈了。我更愿意把它看作像风能一样的可再生资源。💨无论你选择哪种能源作为你的隐喻，你都想利用它的一些力量！⚡️

理想情况下，您可以直接访问您控制的文件或数据库中的数据。如果情况不是这样，幸运的话，这些数据可以通过面向公众的 API 获得。☘️

在本文中，我将向您展示使用 Python 从公共 API 获取数据的步骤。🐍首先，我将向您展示如何以及在哪里寻找 Python API 包装器，并分享最大的 Python API 包装器库。🎉

然后，我将向您展示如何使用*请求*库从没有 Python 包装器的 API 中获取您想要的数据。

如果您想要的数据在一个网站上，但不能通过面向公众的 API 获得，那么有几种选择来抓取它。何时使用哪个刮包是我正在做的另一篇文章。关注[我](https://medium.com/@jeffhale)确保你不会错过！😁

我们往下钻吧！

![](img/ed6e01e838d509f6cf9d1083b823adab.png)

已经钻好了。资料来源:pixabay.com

# 蜜蜂

面向外部的应用程序编程接口(API)通常用于提供大块数据。你只需要知道如何使用 API。

一个组织创建一个面向公众的 API，目的是让您使用它。他们的动机从理想主义到唯利是图，可能包括以下内容:

1.  希望你能建造一些东西来改善这个世界。🌍
2.  希望你会使用他们的免费计划，然后需要如此多的数据或需要数据如此频繁，你会付钱给他们访问更多的数据。💵
3.  如果他们不给你一个直接的链接，你会从网站上抓取数据，所以他们可能会减少他们的服务器开销，让你的体验更好。😀

API 可以被很好地记录，很差地记录，或者介于两者之间。如果你幸运的话，它们是有据可查的。如果你真的幸运的话，有一个 Python 包装器可以运行，并且有很好的文档记录。🎉

# 查找 Python API 包装器

为您需要的 API 找到 Python 包装器可能很棘手。这是我建议的方法。

## Python API 包装器列表

我在 [GitHub](https://github.com/discdiver/list-of-python-api-wrappers) 维护我认为最大的 Python API 包装器列表。 [Real Python](https://realpython.com) 做了一个很好的列表，由 [johnwmiller](https://github.com/johnwmillr/list-of-python-api-wrappers) 分叉并更新。我稍微清理了一下列表，然后，鉴于冠状病毒隔离终止了我的孩子从足球裁判⚽️和照看猫中赚钱的能力🐈我付钱让他们帮忙改进名单。我们更新和扩充了这个过时的列表，因为我在别处找不到一个好的可用 API 包装器列表。😀

如果您发现列表中缺少一个 Python API 包装器，请编辑 [ReadMe](https://github.com/discdiver/list-of-python-api-wrappers/blob/master/readme.md) 文件并提交一个 pull 请求。如果您是新手，这里有一个在 GUI 中编辑 GitHub Markdown 文件的快速指南:

点击右上角的铅笔图标，进行修改([这里有](https://commonmark.org/help/tutorial/)一个可爱的降价教程，如果你需要的话)。然后点击页面底部的绿色*提议文件更改*按钮。然后点击绿色的*创建拉动请求*按钮，总结更改，并点击底部的绿色*创建拉动请求*按钮。谢谢大家！🎉

仅供参考，我经常在我的 [Data Awesome 邮件列表](https://dataawesome.com)中突出 Python 包。在下一期文章中，我将重点介绍由[冉阿让](https://medium.com/u/68179100a7a3?source=post_page-----dfb83fdc5b5b--------------------------------)撰写的 [yfinance](https://github.com/ranaroussi/yfinance/tree/master/yfinance) 。yfinance 包装了雅虎金融 API。你可以用一行代码将股票市场数据读入熊猫数据框。🚀

如果 Python API 包装器列表中没有您需要的东西，我建议您使用通常的方法在互联网上查找。🕸

## 谷歌一下

![](img/00fe02339c50f3fd3d272eb445b4f3f6.png)

一个好的搜索引擎是开发者或数据科学家最好的朋友😁

具体来说，我会搜索我正在寻找的 *Python 包装器的名称。GitHub 链接可能是最有成效的。如果回购协议在过去几年中没有更新或者已经存档，那么成功使用包装器的可能性就不大。那样的话，我建议你继续找。👓*

## 查看 API 网站

如果幸运的话，您正在使用的 API 的网站可能会列出各种编程语言中可用的包装器。值得一试。😃

# 直接使用 API

当没有 API 包装器时，你必须直接查询 API。我建议你使用 Python *请求*库。

## 使用请求

古老的[请求](https://requests.readthedocs.io/en/master/)库是久经考验的从 API 获取信息的方法。Requests 由 Kenneth Reitz 创建，受 Python 软件基金会保护。在撰写本文时，这是下载量最大的 Python 包。👍

使用`pip install requests`从命令行将请求安装到您的环境中

然后导入使用。使用 HTTP 动词 *get* 和 *post* 作为返回所需信息的方法。大多数情况下，您将使用 *get。*下面是如何查询 GitHub API:

```
import requestsr = requests.get**(**'https://api.github.com/events'**)**
```

您可以将参数作为字典传递给 *get* 方法。这是[快速入门指南](https://requests.readthedocs.io/en/master/user/quickstart)。

当你发出一个 *get* 请求时，你经常会得到 JSON。您可以使用请求*。json()* 快速将 json 变成字典的方法。

```
my_dict = r.json()
```

![](img/e29b89b994560604c60658c405c6d8cb.png)

用请求提炼数据查询比提炼石油要快得多😀资料来源:pixabay.com

Requests 在幕后使用了 [urllib3](https://urllib3.readthedocs.io/en/latest/) 库并对其进行了增强。您还可以使用其他库，但是 requests 非常可靠，易于使用，并且为大多数 Python 程序员所熟悉。我建议您在想要进行 API 调用并且 API 没有 Python 包装器时使用请求。

说到这里，如果你最喜欢的 API 没有 Python 包装器，我鼓励你考虑做一个。👍

## 制作 Python API 包装器🛠

制作 API 包装器是学习 Python 打包技能的好方法。[我写了一个指南](/build-your-first-open-source-python-project-53471c9942a7)让你开始制作 Python 包并在 [PyPi](https://pypi.org/) 上发布。

我创建了[py libraries](https://readthedocs.org/projects/pybraries/)，这是一个用于 [Libraries.io](https://libraries.io/) API 的 Python 包装器。制作它是一次很棒的学习经历，回馈给我受益匪浅的开源社区是一件很酷的事情。🚀

# 关于 API 你应该知道的其他事情

API 键、速率限制和 cURL 是您应该熟悉的另外三个术语。

## API 键🗝

您通常需要一个 API 键来查询 API。API 文档应该清楚地说明如何获得密钥。大多数时候，你可以通过在你想要其数据的组织的网站上注册来免费获得一个。如果你想要大量的数据或者经常需要，你可能需要支付特权。

![](img/f4e5a19e17f9372ba295a6b4f374f99b.png)

钥匙。资料来源:pixabay.com

您可以将 API 密钥存储在环境变量中。这里有一个在 Mac、Linux 和 Windows 上设置环境变量的指南。按照惯例，环境变量名都是大写的。说到所有的大写:

**在任何情况下，⚠️都不会在 GITHUB 或其他可公开访问的在线版本控制系统中存储 API 密钥。⚠️**

你的钥匙可能会被盗和滥用——尤其是当你的账户有信用卡的时候。☹️

要在 Python 中访问保存 API 键的环境变量，请导入`os`模块并从字典中获取值，该值的键与环境变量的名称相匹配。例如:

```
import osos.environ.get('MYENVIRONMENTVARIABLE')
```

这段代码返回您的 *MYENVIRONMENTVARIABLE* 环境变量的值。

如果您将在云上的应用程序中使用您的密钥，您应该研究一下秘密管理，正如这里讨论的。

说到远离麻烦，我们来讨论一下费率限制。

## 费率限制

![](img/170f5d26dd2aa8b6ff4e990e84a471fc.png)

需要限制一下。😲资料来源:pixabay.com

许多 API 限制在给定的时间内 ping 它们的次数，以避免支付大量额外的服务器。为了避免超过这些限制，您可以使用 Python *time* 库并将您的请求放入一个循环中。例如，这里有一个简短的代码片段，它在请求之间等待五秒钟。

```
import time
import requestst = 0
my_url = 'https://example.com'while t < 100:
    r = requests.get(my_url)
    time.sleep(5)            # wait five seconds
    t += 1
    print(r.json())
```

这段代码的目的是向您展示如何使用 *time.sleep()* 来避免可能使您超过速率限制的不间断 API 端点 pings。在真实的用例中，您可能希望以某种方式保存数据，并使用 *try…except* 块。

## 卷曲

这是一个使用 Python 和 API 的指南，但是你应该知道你可以用流行的 [cURL](https://curl.haxx.se/) 程序从命令行查询 API。默认情况下，运行最新 Windows 10 版本的 MAC 和机器上都包含 cURL。如果你想学习 cURL，我建议你去看看免费的电子书[征服命令行](http://conqueringthecommandline.com/book/curl)，作者是 Mark Bates。🚀

# 概述

如果你想从一个 API 中获取数据，首先尝试找到一个 Python 包装器。点击查看 Python 包装器列表[。如果失败，谷歌搜索，并检查 API 网站。如果你在 GitHub 上发现我的列表中缺少一个 Python 包装器，请添加它。😀](https://github.com/discdiver/list-of-python-api-wrappers)

如果没有 API 的 Python 包装器，就使用请求库。如果您有时间，可以考虑创建一个包装器，这样所有 Python 用户都可以受益。❤️

不要忘记保护您的 API 键的安全，避免达到速率限制。🔑

# 包装

您已经学习了使用 Python 查找和使用 API 的工作流。您正在获得做大事所需的数据！🚀

我希望这篇关于在 Python 中使用 API 的指南对你有用。如果你有，请在你最喜欢的社交媒体上分享，这样其他人也可以找到它。👍

我撰写关于 [Python](https://memorablepython.com) 、 [SQL](https://memorablesql.com) 、 [Docker](https://memorabledocker.com) 、数据科学以及其他技术主题的文章。如果你对此感兴趣，请关注我，在这里阅读更多。😀

[![](img/ba32af1aa267917812a85c401d1f7d29.png)](https://dataawesome.com)![](img/63380f7608c1d9a884d777d38c6fa190.png)

驾驭风！资料来源:pixabay.com