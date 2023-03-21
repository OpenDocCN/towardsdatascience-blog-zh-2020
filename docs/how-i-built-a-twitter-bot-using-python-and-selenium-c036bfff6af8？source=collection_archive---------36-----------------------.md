# 我如何使用 Python 和 Selenium 构建 Twitter Bot？

> 原文：<https://towardsdatascience.com/how-i-built-a-twitter-bot-using-python-and-selenium-c036bfff6af8?source=collection_archive---------36----------------------->

## 让你的 Python 脚本来做所有的推文吧！！

![](img/98701a9d147b0b73a441a2dca3f86328.png)

使用 Python 的 Twitter 机器人— [Twilio](https://twilio-cms-prod.s3.amazonaws.com/original_images/twitter-python-logos.jpg)

我们正在寻求这个星球上每一件事情的自动化。从表单自动填充到无人驾驶汽车，人类使用自动化流程已经走过了漫长的道路。Python 脚本在生成自动化工具和任务方面非常方便。另一方面，Selenium 以自动化浏览器和网络应用而闻名。当 Python 和 Selenium 的力量结合在一起时，人们可以轻松地自动化数百项任务。由于这两者的结合，制造机器人变得更加容易。

这篇文章是制作一个简单的 Twitter 机器人的指南，这个机器人会为你自己发微博。

**图书馆**

```
import time
from selenium import webdriver
from selenium.webdriver.common.keys import Keys
```

Selenium 是我们构建 Twitter bot 所需的主要库。我已经从 selenium 导入了“webdriver”子包来访问浏览器并在其上执行任务。此外，我已经导入了“Keys”子包，以便在自动化过程中使用键盘键进行输入。

**现在是肉的部分**

我已经初始化了一个类“TwitterBot ”,它有一个接受 Twitter 帐户用户名和密码的构造函数。此外，它踢了自动化的 Chrome 网络驱动程序。

```
class TwitterBot():
    def __init__(self,username,password):
        self.browser=webdriver.Chrome("/Users/vishalsharma/Downloads/chromedriver")
        self.username=username
        self.password=password
```

> 你可以从[下载 Chrome 网络驱动](https://sites.google.com/a/chromium.org/chromedriver/downloads)

我在类中定义了一个函数“SignIn ”,它通过在登录表单中输入用户名和密码来登录 twitter 句柄。我已经使用“find_element_by_name”方法来定位 web 元素并填写表单。

```
def signIn(self):
    self.browser.get("https://www.twitter.com/login")
    time.sleep(5)
    usernameInput=self.browser.find_element_by_name("session[username_or_email]")
    passwordInput=self.browser.find_element_by_name("session[password]")
    usernameInput.send_keys(self.username)
    passwordInput.send_keys(self.password)
    passwordInput.send_keys(Keys.ENTER)
    time.sleep(5)
```

对于我们的推文部分，我使用了“find_element_by_xpath”方法，并使用了“send_keys”方法。在最后一条语句中，“send_keys”有两个参数。这两个参数按 Command+Enter 键发布推文。

```
def TweetSomething(self):
    tweet = self.browser.find_element_by_xpath('''//*[@id='react-root']/div/div/div[2]/main/div/div/div/div/div
                                                  /div[2]/div/div[2]/div[1]/div/div/div/div[2]/div[1]/div/div
                                                  /div/div/div/div/div/div/div/div[1]/div/div/div/div[2]/div
                                                  /div/div/div''')
    tweet.send_keys("""Hello World!""")
    tweet.send_keys(Keys.COMMAND, Keys.ENTER)
```

> 在“find_element_by_xpath”中，你可能会发现大量的胡言乱语。但是，这只是一种定位 web 元素的方法。您也可以使用其他定位方法。找到他们[这里](https://selenium-python.readthedocs.io/locating-elements.html)！

**调用类**

现在，在控制台中给出用户名和密码。调用“TwitterBot”类并运行该类中的方法。

```
if __name__=="__main__":
    username= input("Enter your username: ")
    password= input("Enter your password: ")
    t=TwitterBot(username,password)
    t.signIn()
    t.TweetSomething()
```

就是这样！你已经建立了一个为你发微博的推特机器人。现在，你可以查看 Instagram 的关注者，比如照片，甚至可以右键点击你的 Tinder 个人资料。现在，继续尝试使用 Python 和 Selenium 自动化您的社交媒体。

如需讨论和反馈，请联系我 [Linkedin](https://www.linkedin.com/in/vishal-sharma-239965140/) ！