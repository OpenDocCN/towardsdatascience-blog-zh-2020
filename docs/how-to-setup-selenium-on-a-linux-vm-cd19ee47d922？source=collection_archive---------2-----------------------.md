# 如何在 Linux 虚拟机上设置 Selenium

> 原文：<https://towardsdatascience.com/how-to-setup-selenium-on-a-linux-vm-cd19ee47d922?source=collection_archive---------2----------------------->

## 如何在 Linux 操作系统上安装 selenium 和 chromedriver 以运行无头抓取的分步指南

![](img/4aca72b2f274f461848d97115e17cf45.png)

尼古拉斯·皮卡德在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄的照片

Selenium 是测试 web 应用程序的流行框架。您在这里可能是因为您已经在本地机器上编写并测试了您的 web 抓取/测试脚本。现在，你想知道…

> "我如何每 X 小时/天/周自动运行我的 Selenium 脚本？"

因此，本指南将向您展示如何使用 Selenium 和 Chromedriver 设置一个 Linux 虚拟机(VM)来实现这一点。虽然你可以一直使用 Windows 操作系统，但是在本指南中，让我们把重点放在 Linux 操作系统上。

# 装置

要让 selenium 和 Chromedriver 在本地机器上运行，可以分为 3 个简单的步骤:

1.  安装依赖项
2.  安装 Chrome 二进制和 Chromedriver
3.  安装 Selenium

## 第一步

每当你得到一台新的 Linux 机器，总是首先更新软件包。然后安装必要的依赖项。

## 第二步

为了让 Chromedriver 在 Linux 上运行，你必须安装 Chrome 二进制文件。

对于 Chromedriver 版本，这将取决于你的 Chrome 二进制版本。

## 第三步

最后，安装 Selenium。

还有…你都准备好了！

# 测试您的脚本

剩下要做的就是检查 Selenium 和 Chromedriver 是否安装正确，以及您是否能够运行使用 Selenium 的 python 脚本。

```
from selenium import webdriver
from selenium.webdriver.chrome.options import OptionsCHROMEDRIVER_PATH = '/usr/local/bin/chromedriver'
WINDOW_SIZE = "1920,1080"chrome_options = Options()
chrome_options.add_argument("--headless")
chrome_options.add_argument("--window-size=%s" % WINDOW_SIZE)
chrome_options.add_argument('--no-sandbox')driver = webdriver.Chrome(executable_path=CHROMEDRIVER_PATH,
                          chrome_options=chrome_options
                         )
driver.get("[https://www.google.com](https://www.google.com)")
print(driver.title)
driver.close()
```

如果它打印“Google”，这意味着你已经在一个 Linux VM 上成功运行了带有 Chromedriver 的 Selenium！

# 奖金

如果你正在寻找一个[文档](https://gist.github.com/zhunhung/86a60dcd829ba2a5b2cf815616083dcc)，你很幸运。希望这有所帮助！