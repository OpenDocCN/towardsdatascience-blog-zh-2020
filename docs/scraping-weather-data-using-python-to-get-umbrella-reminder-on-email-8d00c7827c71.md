# 使用 Python 抓取天气数据以在电子邮件中获得雨伞提醒

> 原文：<https://towardsdatascience.com/scraping-weather-data-using-python-to-get-umbrella-reminder-on-email-8d00c7827c71?source=collection_archive---------53----------------------->

## 使用 Python 获得自动“雨伞提醒”电子邮件

![](img/0c06e787dd13043efcbe34a7b9f6edf1.png)

图片由[克里斯多佛·高尔](https://unsplash.com/@cgower)——[Unsplash](https://unsplash.com/photos/m_HRfLhgABo)

Python 在脚本编写、自动化、web 抓取、数据分析等方面创造了奇迹，这样的例子不胜枚举。在本文中，我混合了 web 脚本和自动化——因此，如果城市的天气是阴雨天，Python 脚本将发送一封“雨伞提醒”电子邮件。

**库导入**

```
from bs4 import BeautifulSoup
import smtplib
import requests as rq
```

“美丽的汤”用于从网站、Html 和 XML 页面中提取数据。“Requests”库用于发送 HTTP 请求，而“smtplib”用于向任何机器发送电子邮件。

**获取天气数据**

```
url = rq.get('https://forecast.weather.gov/MapClick.php?lat=40.71455000000003&lon=-74.00713999999994')

soup = BeautifulSoup(url.text, 'html.parser')

weather = (soup.select('#current_conditions-summary p')[0]).text
temperature = (soup.select('#current_conditions-summary p')[1]).text
```

我使用过[国家气象局](https://forecast.weather.gov/)网站搜集纽约的天气数据。requests.get()函数接受要下载的 URL 字符串。BeautifulSoup()函数需要用一个包含它将解析的 HTML 的字符串来调用。

现在，BeautifulSoup 对象已经创建，您可以使用一个 *"select"* 方法从 web 页面中检索天气和温度的 web 元素。

**构建 smtplib 对象**

```
server = smtplib.SMTP('smtp.gmail.com', 587)
print(server.ehlo())
print(server.starttls())

server.login('Enter your email', 'Password')
```

我创建了一个带有参数“smtp.gmail.com”和 587 的 SMTP 对象。创建 SMTP 对象后，您可以使用您的电子邮件地址和密码登录。

> 注意:smtplib 中的参数可能会更改。SMTP()取决于域的 SMTP 服务器。如果您有 Gmail 帐户，您必须为您的电子邮件地址生成特定于应用程序的密码。否则，当您的程序试图登录时，您将得到一个特定于应用程序的需要密码的错误消息。

您可以使用本[指南](https://support.google.com/accounts/answer/185833?hl=en)创建您的 Gmail 应用程序专用密码

**发送电子邮件**

```
if weather == 'Raining' or weather == 'Overcast' :

    subject = "Umbrella Reminder"
    body = f"Take an umbrella with you. Weather condition for today is {weather} and temperature is {temperature} in New York."

    msg = f"Subject:{subject}\n\n{body}\n\nRegards,\nVishal".encode('utf-8')
    print(msg)

    server.sendmail('Sender's Email', 'Recipient Email', msg)

    print("Email Sent!")

    server.quit()
else :
    print("There is going to be {} and temperature is {} in New York".format(weather, temperature))
```

如果来自 web 元素的天气结果是下雨或阴天，发件人将向收件人发送一封带有“雨伞提醒”消息的电子邮件。

这就是你如何使用 Python 从一小步开始并自动化你生活中的小事情。开始您的脚本之旅。

请联系我，在[维沙尔·夏尔马](https://www.linkedin.com/in/vishal-sharma-239965140/)寻求反馈或讨论！