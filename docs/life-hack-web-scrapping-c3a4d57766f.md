# 生活帮网络报废

> 原文：<https://towardsdatascience.com/life-hack-web-scrapping-c3a4d57766f?source=collection_archive---------35----------------------->

## 网络报废让我的生活变得简单多了。然而，从大多数网站中提取内容的过程从来没有真正被提及过。这使得处理信息几乎不可能

# 为什么？

网络报废让我的生活变得简单多了。然而，从使用专有系统锁定内容的网站中实际提取内容的过程从未被真正提及。这使得将信息重新格式化成期望的格式即使不是不可能，也是极其困难的。几年来，我发现了几种(几乎)防失败的技术来帮助我，现在我想把它们传递下去。

我将带您了解将纯网络书籍转换为 PDF 的过程。这里的想法是强调如何根据自己的情况复制/修改它！

如果你有任何其他的技巧(或者有用的脚本)来完成这样的任务，一定要让我知道，因为创建这些生活黑客脚本是一个有趣的爱好！

![](img/f4ac2fe049480d6b5e2765615cfb38cc.png)

封面图片来源于[此处](https://www.pxfuel.com/en/free-photo-oinlw)

# 再现性/适用性？

我列举的例子来自一个只提供学习指南的网站(为了保护他们的安全，我排除了特定的网址)。我概述了几个经常出现在网络废弃时的缺陷/问题！

# 要犯的错误？

我在试图搜索受限访问信息时犯了几个错误。每一个错误都消耗**大量**的**时间**和**能量**，所以它们是:

*   使用 AutoHotKey 或类似的来直接影响鼠标/键盘(这种**会产生躲闪** **不一致的行为**)
*   加载所有页面，然后导出一个 HAR 文件(HAR 文件没有实际数据，需要很长时间才能加载)
*   尝试使用 GET/HEAD 请求(大多数页面使用**授权**方法，这实际上是不可逆的)

# 进展缓慢

为这些网站写一个 300 行的简短脚本似乎很容易/很快，但它们总是比这更困难。以下是解决方案的潜在障碍:

*   Selenium changing 使用的浏览器配置文件
*   以编程方式查找配置文件
*   不知道要等多久才能加载链接
*   当链接**不等于**当前链接**时进行检测**
*   或者使用浏览器 JavaScript(可能的话，将在下面详细描述)
*   需要找到关于当前网页内容的信息
*   看看潜在的 JavaScript 函数和 URL
*   失败时重新启动长脚本
*   **减少**文件的**查找**次数
*   将文件复制到**可预测的位置**
*   在开始做任何复杂的事情之前，检查这些文件
*   不知道一个长的脚本是什么
*   打印任何必要的输出(仅用于那些花费大量时间且没有其他度量的输出)

# 密码

## 准备工作

```
from selenium import webdriver
from selenium.webdriver.common.by import By
from selenium.webdriver.support.ui import WebDriverWait
from PIL import Image
from natsort import natsorted

import time
import re
import os
import shutil
import img2pdf
import hashlibdriver = webdriver.Firefox()
cacheLocation = driver.capabilities['moz:profile'] + '/cache2/entries/'
originalPath =  os.getcwd()
baseURL = 'https://edunlimited.com'
```

## 装载书

```
driver.get(loginURL)
driver.get(bookURL)

wait.until(lambda driver: driver.current_url != loginURL)
```

## 获取元数据

经常可以找到用于提供有用信息的 JavaScript 函数。有几种方法可以做到这一点:

*   查看页面的 HTML 源代码(右键单击“查看页面源代码”)
*   使用 web 控制台

```
bookTitle = driver.execute_script('return app.book')
bookPages = driver.execute_script('return app.pageTotal')
bookID = driver.execute_script('return app.book_id')
```

## 组织文件

脚本通常不会按预期执行，有时需要很长时间才能完成。因此，在脚本的迭代过程中保持进度是相当自由的。实现这一点的一个好方法是保持有条理！

```
if not os.path.exists(bookTitle):
        os.mkdir(bookTitle)
    if len(os.listdir(bookTitle)) == 0:
        start = 0
    else:
        start = int(natsorted(os.listdir(bookTitle), reverse=True)[0].replace('.jpg', ''))
        driver.execute_script('app.gotoPage(' + str(start) + ')')

os.chdir(bookTitle)
```

## 浏览这本书

图像总是存储在缓存中，所以当其他方法都失败时，就利用这一点吧！

不过这并不容易，首先我们需要加载页面，然后我们需要以某种方式恢复它！

为了确保我们总是加载整个页面，有两种安全措施:

*   在移动到下一页之前，等待当前页面加载
*   如果*无法加载*，则重新加载页面

让这两者工作需要保证完成(JavaScript 或浏览器响应)的函数，以及故障安全等待时间跨度。安全时间跨度是反复试验的，但是通常在 0.5 到 5 秒之间效果最好。

直接从硬盘缓存中恢复特定数据是一个相对模糊的话题。关键是首先找到一个下载链接(通常很容易，因为**不需要工作**)。然后在 **URL** 上运行 **SHA1** 、**十六进制摘要**和一个**大写函数**，产生最终的文件名(它**不仅仅是上述安全算法中的一个**，因为旧的来源让你相信，而是两个都有)。

最后一点，确保现在而不是以后清理数据(从 PNG 图像中删除 alpha 通道),因为它减少了代码中使用的循环次数！

```
for currentPage in range(start, bookPages - 1):

        while driver.execute_script('return app.loading') == True:
            time.sleep(0.5)

        while (driver.execute_script('return app.pageImg') == '/pagetemp.jpg'):
            driver.execute_script('app.loadPage()')
            time.sleep(4)

        location = driver.execute_script('return app.pageImg')

        pageURL = baseURL + location
        fileName = hashlib.sha1((":" + pageURL).encode('utf-8')).hexdigest().upper()
        Image.open(cacheLocation + fileName).convert('RGB').save(str(currentPage) + '.jpg')

        driver.execute_script('app.nextPage()')
```

## 转换为 PDF

我们终于可以得到一个方便的 PDF 文件

```
finalPath =  originalPath + '/' + bookTitle + '.pdf'

with open(finalPath, 'wb') as f:
    f.write(img2pdf.convert([i for i in natsorted(os.listdir('.')) if i.endswith(".jpg")]))
```

## 移除多余的图像

```
os.chdir(originalPath)
shutil.rmtree(bookTitle)
```

封面图片来源于[此处](https://www.pxfuel.com/en/free-photo-oinlw)

# 感谢阅读！

这基本上是我在博客上发表的第一篇以代码为中心的文章，所以我希望它有用！

———下次见，我要退出了