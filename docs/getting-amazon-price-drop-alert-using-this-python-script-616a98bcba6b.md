# 使用这个 Python 脚本获取 Amazon 价格下降警报

> 原文：<https://towardsdatascience.com/getting-amazon-price-drop-alert-using-this-python-script-616a98bcba6b?source=collection_archive---------39----------------------->

## 刮亚马逊获得降价预警！

![](img/06e67a3fab9110df26382e73b4525265.png)

格伦·卡斯滕斯·彼得斯——[Unsplash](http://www.unsplash.com)的照片

Python 做了很多奇妙的事情。而且，这种美丽的语言几乎没有做不到的事情。当涉及到数据分析、web 抓取或任务自动化时，大量的库使得这种语言一枝独秀。

> 试想一下，你想买一部新 iPhone。你厌倦了一遍又一遍地查看亚马逊网站。但是，它的价格没有下降，你总是失望。

如果，有一个自动价格检查器，会给你一个电子邮件提醒，如果有价格下降。令人兴奋，不是吗？让我们通过编写一个 Python 脚本来做同样的事情。

**库配料**

```
import smtplib
import time
import requests as rq
from bs4 import BeautifulSoup
```

主要有三个库可以为我们完成这项工作。Requests 库将与 HTTP 服务器通信，Beautiful Soup 将与网页通信并提取网页元素，smtplib 将向您发送电子邮件警报。

**肉** **码**

```
site = "https://www.amazon.in/Apple-iPhone-11-64GB-Green/dp/B07XVKBY68/ref=sr_1_7?keywords=iphone+11&qid=1573668357&sr=8-7"header = {'User-Agent': 'Mozilla/5.0 (Macintosh; Intel Mac OS X 10_15_0) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/78.0.3904.97 Safari/537.36'}
```

首先，我已经请求了亚马逊 iPhone 网页的网络服务器。然后，我创建了一个 soup 对象来解析网页。我们只想要产品的价格。所以，使用“选择”方法，我们可以得到产品的价格。

```
def get_price():
    html = rq.get(site, headers=header).text
    soup = BeautifulSoup(html, 'html.parser')
    price = [i.get_text() for i in
             soup.find_all('span', {'class': 'a-size-medium a-color-price priceBlockBuyingPriceString'})]

    final_price = ''.join(price)[2:8]
    final_price = int(final_price.replace(',', ''))

    if final_price < 64900:
        send_email()
```

现在，每当“新价”变得小于“刮价”时。必须向用户发送一封关于价格下降的电子邮件。

我们将使用 smtplib 库向用户发送电子邮件。首先，我们将创建一个 SMTP 对象来与域的 SMTP 服务器通信。

一旦一切都完成并设置好，我们就可以登录我们的电子邮件帐户了。然后，将电子邮件发送给收件人。

```
def send_email():
    server = smtplib.SMTP('smtp.gmail.com', 587)
    server.ehlo()
    server.starttls()
    server.ehlo()

    server.login('Your email address', 'Application-specific password')

    subject = "Price fell down"
    body = "Check this link: https://www.amazon.in/Apple-iPhone-11-64GB-Green/dp/B07XVKBY68/ref=sr_1_7?keywords=iphone+11&qid=1573668357&sr=8-7"
    msg = f"Subject:{subject}\n\n{body}"

    server.sendmail('Sender's email', 'Recipient email', msg)

    print("Hey, email has been sent!")
    server.quit()
```

> 注意:smtplib 中的参数可能会更改。SMTP()取决于域的 SMTP 服务器。如果您有 Gmail 帐户，您必须为您的电子邮件地址生成特定于应用程序的密码。否则，当您的程序试图登录时，您将得到一个特定于应用程序的需要密码的错误消息。

```
while True:
    get_price()
    time.sleep(60)
```

这 40-50 行代码将为您完成这项工作。下一次，你不必一次又一次地查看网站上的价格下降。为其他电子商务网站制作这样的程序，节省您的时间和精力。

有关讨论和疑问，请通过 [Linkedin](http://linkedin.com/in/vishal-sharma-239965140) 联系我！