# 如何找到用于安全研究的黑暗网络黑客论坛

> 原文：<https://towardsdatascience.com/how-to-locate-dark-web-hacker-forums-for-security-research-e87b53f18508?source=collection_archive---------3----------------------->

## 黑暗网络安全研究的滚雪球抽样

![](img/b4fb6a143f18820dc8c487b993d8abc4.png)

资料来源:Pexels

# 警告:访问黑暗网络可能是危险的！请自担风险继续，并采取必要的安全预防措施，如禁用脚本和使用 VPN 服务。

# 介绍

对大多数用户来说，谷歌是探索互联网的门户。但是，deep web 包含不能被 Google 索引的页面。在这个空间中，隐藏着黑暗网络——匿名网站，通常被称为隐藏服务，从事从毒品到黑客到人口贩运的犯罪活动。

在黑暗网络上进行安全研究可能很困难。黑暗网络上的网站 URL 不遵循惯例，通常是由字母和数字组成的随机字符串，后跟。洋葱子域。这些网站需要 TOR 浏览器解析，无法通过 Chrome 或 Safari 等传统浏览器访问。

此外，传统的搜索引擎如谷歌并不存在于暗网上。取而代之的是，网站 URL 要么被人对人(在线或面对面)交换，要么被收集到一个简单的 Html 目录中。这使得在黑暗网络上很难找到黑客论坛——特别是严肃的个人聚集的论坛。

一些论坛可以在上面列出的黑暗网络目录中找到。这些都可以在暗网甚至表面网上找到(我们都知道的网，这篇文章也是在上面托管的)。但是，因为这些论坛很容易找到，所以它们经常吸引社区中的业余爱好者。为了定位更多安全研究的相关论坛，可以使用滚雪球抽样法对暗网进行爬取。

# 滚雪球抽样

滚雪球抽样是一种方法，可用于定位暗网上的隐藏服务，用于安全研究，包括数据收集和 CTI 流。雪球抽样是一个网络爬虫架构，它获取一个根 URL 并抓取该网站到其他网站的输出链接。然后，对于每个收集的链接，在设定的深度内继续该过程。这个爬虫将返回一个它收集到的黑暗网站的 URL 的大列表。

这种方法非常类似于早期搜索引擎网络爬虫的工作方式。谷歌的创始人谢尔盖·布林和劳伦斯·佩奇在 1998 年的论文[“大规模超文本网络搜索引擎的剖析”](http://infolab.stanford.edu/~backrub/google.html)中可以找到一个重要的例子。

当应用于黑暗网络的论坛时，滚雪球抽样效果很好。用户通常会在论坛帖子和评论中链接到其他论坛，这是前面提到的人与人之间交换 URL 的一个例子。通过从一个在目录上找到的黑客论坛(或者你已经知道的一个)开始，可以快速找到更严肃的和安全相关的论坛。

# 环境设置

为了开发黑暗网络爬虫，你需要设置你的环境。阅读我写的关于[如何刮掉黑暗之网](/how-to-scrape-the-dark-web-53145add7033)的文章可能会有助于更好地理解这个过程。这也将有助于从你所发现的黑暗网络论坛中搜集数据。本文假设您使用的是 OSX 操作系统。但是，如果你使用的是 Linex 或者 Windows，很多方面应该还是适用的。

## TOR 浏览器

TOR 浏览器是一种使用 TOR 网络的浏览器，允许我们使用. onion 子域解析网站。TOR 浏览器可以在这里下载[。](https://www.torproject.org/download/)

## 虚拟专用网络

在爬行黑暗网络时运行 VPN 可以为您提供额外的安全性。虚拟专用网络(VPN)不是必需的，但强烈建议使用。

## 计算机编程语言

对于本文，我假设您已经在自己的机器上安装了 python，并使用了自己选择的 IDE。如果没有，网上可以找到很多教程。

## 熊猫

Pandas 是一个数据操作 Python 包。Pandas 将用于存储和导出刮到 csv 文件的数据。通过在终端中键入以下命令，可以使用 pip 安装 Pandas:

```
pip install pandas
```

## 硒

Selenium 是一个浏览器自动化 Python 包。Selenium 将用于抓取网站和提取数据。Selenium 可以通过在终端中键入以下命令来使用 pip 进行安装:

```
pip install selenium
```

## 壁虎

为了让 selenium 自动化浏览器，它需要一个驱动程序。因为 TOR 浏览器在 Firefox 上运行，我们将使用 Mozilla 的 Geckodriver。你可以在这里下载驱动[。下载后，解压驱动程序，并将其移动到您的~/中。本地/bin 文件夹。](https://github.com/mozilla/geckodriver/releases)

## Firefox 二进制文件

TOR 浏览器的 Firefox 二进制文件的位置也是需要的。要找到它，右键单击应用程序文件夹中的 TOR 浏览器，然后单击显示内容。然后导航到 Firefox 二进制文件并复制完整路径。将此路径保存在某个地方以备后用。

# 履行

这个实现将让你开始创建一个深度为 1 的雪球抽样黑暗网络爬虫。因为论坛的网站结构互不相同，所以很难在深度 1 之外实现爬虫的自动化。

首先，从 selenium 导入 web 驱动程序和 FirefoxBinary。也进口熊猫作为 pd 和 re。

```
from selenium import webdriver
from selenium.webdriver.firefox.firefox_binary import FirefoxBinary
import pandas as pd
import re
```

创建一个变量“binary ”,并将其设置为您之前保存的 Firefox 二进制文件的路径。

```
binary = FirefoxBinary(*path to your firefox binary*)
```

设置 web 驱动程序使用 Firefox 并传递二进制变量。

```
driver = webdriver.Firefox(firefox_binary = binary)
```

创建一个变量“starting_node”并将其设置为起始论坛的 URL。

```
starting_node = *your url*
```

创建空列表“found_nodes”。

```
found_nodes = []
```

您现在需要自动抓取论坛帖子、评论或两者的文本。这一过程因论坛而异。我推荐阅读我的文章[“如何清理黑暗网络”](/how-to-scrape-the-dark-web-53145add7033)来获得一些指导，Selenium 文档可以在[这里](https://selenium-python.readthedocs.io/)找到。

编译一个正则表达式，用于识别字符串中的洋葱链接。

```
p = re.compile('\S+onion')
```

对于每个字符串(帖子文本或评论文本),搜索 onion URLs 并将它们添加到列表“found_nodes”中。

```
for post in posts:
  nodes = p.findall(post)
  found_nodes.append(nodes)
```

列表“found_nodes”将包含从起始节点找到的洋葱链接。然后，可以对每个找到的链接重复该算法，或者在深度 1 处停止。

# 讨论

滚雪球抽样是发现黑暗网络黑客论坛和其他隐藏服务的好方法。此外，通过这种方法找到的服务通常比在目录中找到的服务更有助于安全研究，因为黑客社区的重要成员通常不会聚集在广告宣传良好的站点上。

然而，在这个实现中有一些需要改进的地方。按照这种滚雪球抽样的实现，需要手动访问每个找到的节点并检查其内容。这不仅是一个耗时的过程，而且访问未知的洋葱链接也很危险。通过帖子上下文自动确定网站内容将极大地改善这个爬虫。

我想重申，刮暗网可能是危险的。确保你采取了必要的安全措施。请继续研究黑暗网络上的安全浏览。我对发生的任何伤害不负任何责任。