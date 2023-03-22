# 如何刮暗网

> 原文：<https://towardsdatascience.com/how-to-scrape-the-dark-web-53145add7033?source=collection_archive---------4----------------------->

## 在 Mac OSX 上使用 Python、Selenium 和 TOR 清理黑暗网络

![](img/d82e48a0db2f957c6054d9f07060cf5b.png)

资料来源:Pexels.com

# 警告:访问黑暗网络可能是危险的！请自担风险继续，并采取必要的安全预防措施，如禁用脚本和使用 VPN 服务。

# 介绍

对大多数用户来说，谷歌是探索互联网的门户。但是，deep web 包含不能被 Google 索引的页面。在这个空间中，隐藏着黑暗网络——匿名网站，通常被称为隐藏服务，从事从毒品到黑客到人口贩运的犯罪活动。

黑暗网络上的网站 URL 不遵循惯例，通常是由字母和数字组成的随机字符串，后跟。洋葱子域。这些网站需要 TOR 浏览器解析，无法通过 Chrome 或 Safari 等传统浏览器访问。

# 查找隐藏服务

抓取黑暗网络的第一个障碍是找到要抓取的隐藏服务。如果你已经知道你想要抓取的网站的位置，你很幸运！这些网站的 URL 通常是不可搜索的，并且是在人与人之间传递的，无论是面对面的还是在线的。幸运的是，有几种方法可以用来找到这些隐藏的服务。

## 方法 1:目录

包含指向隐藏服务的链接的目录在暗网和明网上都存在。这些目录可以给你一个很好的方向，但往往会包含更多众所周知的服务，更容易找到的服务。

## 方法 2:滚雪球抽样

雪球抽样是一种抓取方法，它获取一个种子网站(比如您从目录中找到的网站)，然后抓取该网站以查找其他网站的链接。收集完这些链接后，爬虫将继续对这些站点进行搜索。这种方法能够找到目录中没有列出的隐藏服务。此外，这些网站更有可能吸引严重的罪犯，因为它们的存在并不透明。

虽然推荐使用滚雪球抽样方法来查找隐藏的服务，但是它的实现超出了本文的范围。我已经在这里写了第二篇关于雪球取样黑暗网络的文章。

# 环境设置

在确定了要收集的隐藏服务之后，需要设置环境。本文涵盖了 Python、Selenium、TOR 浏览器和 Mac OSX 的使用。

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

现在你已经设置好了你的环境，你可以开始写你的 scraper 了。

首先，从 selenium 导入 web 驱动程序和 FirefoxBinary。还进口熊猫当 pd。

```
from selenium import webdriver
from selenium.webdriver.firefox.firefox_binary import FirefoxBinary
import pandas as pd
```

创建一个变量“binary ”,并将其设置为您之前保存的 Firefox 二进制文件的路径。

```
binary = FirefoxBinary(*path to your firefox binary*)
```

设置 web 驱动程序使用 Firefox 并传递二进制变量。

```
driver = webdriver.Firefox(firefox_binary = binary)
```

创建一个变量“url ”,并将其设置为您希望抓取的隐藏服务的 url。

```
url = *your url*
```

打开 TOR 浏览器并获取 url。

```
driver.get(url)
```

你现在可以像抓取任何网站一样抓取隐藏的服务！

# 基本刮硒技术

无论你是初学 Selenium 还是需要刷一下，你都可以使用这些基本技巧来有效地刮网站。其他硒刮教程可以在网上找到。

## 查找元素

Selenium 抓取的一个关键部分是定位 HTML 元素来收集数据。在 Selenium 中有几种方法可以做到这一点。一种方法是使用类名。为了找到一个元素的类名，可以右键单击它，然后单击 inspect。下面是一个通过类名查找元素的例子。

```
driver.find_element_by_class_name("postMain")
```

还可以通过元素的 XPath 来查找元素。XPath 表示元素在 HTML 结构中的位置。您可以在 inspect 界面的 HTML 项目的右键菜单中找到元素的 XPath。下面是一个通过 XPath 查找元素的例子。

```
driver.find_element_by_xpath('/html/body/div/div[2]/div[2]/div/div[1]/div/a[1]')
```

如果要查找多个元素，可以用“find_elements”代替“find_element”。下面是一个例子。

```
driver.find_elements_by_class_name("postMain")
```

## 获取元素的文本

您可以使用 text 函数检索元素的文本。下面是一个例子。

```
driver.find_element_by_class_name('postContent').text
```

## 存储元素

您可以通过将元素保存在变量中，然后将该变量追加到列表中来存储元素。下面是一个例子。

```
post_content_list = []
postText = driver.find_element_by_class_name('postContent').text
post_content_list.append(postText)
```

## 在页面间爬行

一些基于页面的网站会在 URL 中包含页码。您可以在一个范围内循环并更改 url 来抓取多个页面。下面是一个例子。

```
for i in range(1, MAX_PAGE_NUM + 1):
  page_num = i
  url = '*first part of url*' + str(page_num) + '*last part of url*'
  driver.get(url)
```

## 导出到 CSV 文件

在抓取页面并将数据保存到列表中之后，您可以使用 Pandas 将这些列表导出为表格数据。下面是一个例子。

```
df['postURL'] = post_url_list
df['author'] = post_author_list
df['postTitle'] = post_title_listdf.to_csv('scrape.csv')
```

# 防爬行措施

许多隐藏服务采用反爬行措施来保持信息秘密和避免 DDoS 攻击。你会遇到的最常见的措施是验证码。虽然存在一些验证码自动解算器，但隐藏服务经常会使用解算器无法传递的独特验证码类型。下面是一个在论坛上找到的验证码的例子。

![](img/8413d4f7010921bd2fc1497bd6862644.png)

来源:暗网论坛

如果在特定的时候需要验证码(比如第一次连接到服务器)，可以使用 Selenium 中的隐式等待功能。该功能将等待预定的时间，直到可以执行下一个动作。下面是一个使用中的例子，Selenium 将一直等待，直到找到类名为“postMain”的元素。

```
driver.implicitly_wait(10000)
driver.find_element_by_class_name("postMain")
```

其他时候，如果服务器识别出你是机器人，它就会停止为你服务。为了绕过这一点，将网站分块抓取，而不是一次全部抓取。您可以将数据保存在不同的 csv 文件中，然后使用 Pandas concat 函数将它们与额外的 python 脚本相结合。下面是一个例子。

```
import pandas as pddf = pd.read_csv('scrape.csv')
df2 = pd.read_csv('scrape2.csv')
df3 = pd.read_csv('scrape3.csv')
df4 = pd.read_csv('scrape4.csv')
df5 = pd.read_csv('scrape5.csv')
df6 = pd.read_csv('scrape6.csv')frames = [df, df2, df3, df4, df5, df6]result = pd.concat(frames, ignore_index = True)result.to_csv('ForumScrape.csv')
```

# 讨论

与刮擦表面网相比，刮擦暗网具有独特的挑战。然而，它相对尚未开发，可以提供出色的网络犯罪情报操作。虽然隐藏服务通常采用反爬行措施，但这些措施仍然可以被绕过，并提供有趣和有用的数据。

我想重申，刮暗网可能是危险的。确保你采取了必要的安全措施。请继续研究黑暗网络上的安全浏览。我对发生的任何伤害不负任何责任。