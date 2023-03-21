# 如何通过电子邮件自动发送 Python 应用程序崩溃通知

> 原文：<https://towardsdatascience.com/how-to-send-python-app-crash-notifications-via-email-6fd5c156b49d?source=collection_archive---------43----------------------->

## 如果您在远程服务器上运行 Python 应用程序，您可能会不时遇到应用程序崩溃。并且您希望尽快知道发生了崩溃，以便尽快修复问题。

![](img/07e7c77abb7596c509f462e68e959976.png)

照片由 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上的[尼克·费因斯](https://unsplash.com/@jannerboy62?utm_source=medium&utm_medium=referral)拍摄

这个小小的代码片段可以帮助你实现这一点。

1.  导入所需的 Python 模块(电子邮件、日志和 io)
2.  定义电子邮件处理功能。您只需更新您的电子邮件服务器认证详情。
3.  定义 Try/Except 循环，其中“Try”部分将包含您的主代码，“Except”部分将捕获应用程序崩溃，并将详细的崩溃描述发送到您定义的电子邮件地址。

```
from email.mime.text import MIMEText
from email.mime.multipart import MIMEMultipart
from email.mime.base import MIMEBase
from email import encoders
import logging
from io import StringIO
import smtplibdef send_email_crash_notification(*crash_message*):email = 'your_email@gmail.com'
    send_to_email = 'receipent_email@gmail.com'
    subject = 'Python application CRASHED!' msg = MIMEMultipart()
    msg['From'] = email
    msg['To'] = send_to_email
    msg['Subject'] = subject
    message = *crash_message* msg.attach(MIMEText(message, 'plain')) *# Send the message via SMTP server.* server = smtplib.SMTP('smtp.gmail.com', 587)
    server.ehlo()
    server.starttls()
    server.login('your_email@gmail.com', 'your_password')
    text = msg.as_string()
    server.sendmail(email, send_to_email, text)
    server.quit()
    *print*('email sent to ' + *str*(send_to_email)) return True try:
    *#Your main code will be placed here
    print*(1+1)except *Exception* as e:
    log_stream = StringIO()
    logging.basicConfig(*stream*=log_stream, *level*=logging.INFO)
    logging.error("Exception occurred", *exc_info*=True)
    send_email_crash_notification(log_stream.getvalue())
```

感谢您的阅读，我期待着阅读您的评论！

> 如果你觉得特别慷慨，**你可以在[**www.buymeacoffee.com/kingmichael**](http://www.buymeacoffee.com/kingmichael)给我买杯咖啡。你的支持将极大地帮助我保持动力，写出你喜欢的文章。**

![](img/ad1a67aa28b91407f5fa873fcf42f0d5.png)

由[彼得罗·德·格兰迪](https://unsplash.com/@peter_mc_greats?utm_source=medium&utm_medium=referral)在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄的照片