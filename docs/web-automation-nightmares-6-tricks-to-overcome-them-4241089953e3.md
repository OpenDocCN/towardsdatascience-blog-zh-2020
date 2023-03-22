# Web 自动化噩梦:克服它们的 6 个技巧

> 原文：<https://towardsdatascience.com/web-automation-nightmares-6-tricks-to-overcome-them-4241089953e3?source=collection_archive---------27----------------------->

![](img/052a91fdfddb9bdad43512bb15649db8.png)

由[塞巴斯蒂安·赫尔曼](https://unsplash.com/@officestock?utm_source=medium&utm_medium=referral)在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 拍摄的照片

## 当硒和美丽的食物不够的时候。

当我第一次发现 Python 的一些网络抓取库时，真是梦想成真。想想一个人能做的所有事情！可能性是无限的。不用说，当我想刮的那几页纸直接从地狱出来时，我的希望破灭了。

> 当你开始怀疑如果你自己动手，这个过程会不会更快时，你知道你遇到了一个真正的障碍。

经过几个小时的搜索 StackOverflow，以下是我在开始自动化我的 web 流程时克服一些挑战的 6 个简单方法:

# 1.从 Selenium 导入例外

**问题:**有时候你想定位的按钮可能会被隐藏，可能是因为讨厌的弹出窗口。当您无法确定这些弹出窗口何时会弹出时，这就变得更加令人恼火了。而在其他时候，您的问题可能是由于互联网连接速度慢造成的…

**解决方案:**顺利抓取网页的道路布满了例外——并学习如何处理它们！每当发生这些异常时，知道自己有一个备份计划总是很方便的。首先导入它们:

```
from selenium.common.exceptions import NoSuchElementExceptionfrom selenium.common.exceptions import StaleElementReferenceExceptionfrom selenium.common.exceptions import ElementClickInterceptedExceptionfrom selenium.common.exceptions import ElementNotInteractableException
```

然后用 try/except 块解决可能出现的问题！

# 2.让您的浏览器等待

回到上一个问题:假设一个缓慢的互联网连接导致了 NoSuchElementException。上面提到的一种方法是使用 try/except 块让您的脚本等待 10 秒钟:

```
try: 
    button = driver.find_element_by_id('button-id')
    button.click()except NoSuchElementException: 
    sleep(10)
    button = driver.find_element_by_id('button-id')
    button.click()
```

这很棒，而且很可能解决了问题(如果没有，那么你真的需要考虑修理那个路由器)，但是这意味着每次你想点击那个按钮的时候都必须等待整整 10 秒钟。

**解决方法:**换个更优雅的方式怎么样？使用 WebDriverWait，并将按钮的可见性指定为条件！一旦找到按钮，你的浏览器就会停止等待，允许你点击它。每当你想找到一个按钮时，这可以节省你宝贵的时间:

```
from selenium.webdriver.support.ui import WebDriverWait
from selenium.webdriver.support import expected_conditions as ECbutton = WebDriverWait(driver, 10).until(EC.presence_of_element_located((By.XPATH, 'insert_button_xpath_here')))
```

# 3.用 Javascript 输入文本

**问题:**现在你正在填写一张表格，然后你意识到 Selenium 的`send_keys()`选项是有帮助的，但是帮助*不够快*。当然，它能以最快的速度打字，但是你很快就会怀疑自己复制粘贴是否真的会更快。当你看到你的一大段文字在页面上一行一行慢慢串起来时，自我怀疑就开始了。

**解决方案:**通过执行脚本输入文本！这要容易得多，也不需要导入。

```
text = text.replace('\n', '\\n')script='''document.getElementById("source").value='{}';'''.format(text)driver.execute_script(script)
```

现在，您的文本值立即被设置，文本几乎立即出现在表单中！请注意，您应该首先在新行前面添加一个额外的反斜杠' \ '，否则它将不起作用。如果您使用大量文本，这尤其有用。

*另外，我唯一一次遇到这个问题是在输入外语时(在这种情况下，它不起作用)。如果有人找到了更好的解决方法，请告诉我！*

# 4.使用动作链点击

**问题:**一些网站的格式很糟糕，或者找到一些带有 id(或任何其他属性)的按钮似乎无法工作。

解决方法:在这种令人沮丧的情况下，不要惊慌！有其他方法可以解决这个问题。*首先问问自己:* *什么是工作？也许我们可以利用这一点。*

在之前类似这样的体验中，我首先找到了我知道我肯定能识别的按钮，然后弄清楚了我想点击的按钮的相对位置，但却不能这样做。

因为有了 ActionChains，你可以先找到一个工作按钮，然后通过选择的坐标偏移点击来点击屏幕上的任何地方！这在大多数情况下工作得很漂亮，但是找到坐标是关键的一步。

```
from selenium.webdriver.common.action_chains import ActionChainsac = ActionChains(driver)ac.move_to_element(driver.find_element_by_id('insert_id_here').move_by_offset(x,y).click().perform()
```

*现在*你可以点击屏幕上的任何地方！

# 5.做人，随机应变

**问题:**最后，事情进展顺利，你收集了所有你需要的信息…直到你找到验证码。

**解决方法:**负责任的刮！在操作之间引入随机等待时间，以防止服务器过载。更重要的是，它可以防止你的脚本的重复模式被发现。

```
from time import sleep
import randomsleepTimes = [2.1, 2.8, 3.2]
sleep(random.choice(sleepTimes))
```

# 6.最后，确保您的数据得到备份

**问题:**我一开始犯的一个新手错误就是没有引入备份选项，以防我的抓取在中途随机失败(有我的互联网中途断网的可能)。我只是在脚本的*结尾*将我收集的数据保存到一个文件中。这是个大错误。

**解决方案:**备份数据有很多种方法。一些对我有用的方法:

1.  将我的数据的每个“条目”保存到一个 JSON 文件(或任何其他方便的格式)。这让我放心，因为我知道我的脚本运行的每个循环/页面都保存在我的计算机上的某个地方，如果有任何失败，我仍然有我的数据。这也是节省计算机内存的好方法。
2.  将循环放在一个 try/except 块中，如果出现任何问题，它会将我的所有数据保存到一个文件中。

布伦特兰博的 Gif

# 成功了！

这些只是除了简单的 BeautifulSoup 和 Selenium 库之外，您可以使用的许多技巧中的一部分。

我希望这有所帮助，感谢您的阅读！