# 使用 Python 通过电子邮件和短信发送新年祝福

> 原文：<https://towardsdatascience.com/send-your-new-year-greetings-by-email-and-text-message-using-python-6222a2836538?source=collection_archive---------14----------------------->

![](img/56fe5cde394f0962e8abaa335dbcd218.png)

安妮·斯普拉特在 [Unsplash](https://unsplash.com/s/photos/greeting?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) 上的照片

## 有时，开发人员喜欢通过编程解决常规任务来使事情复杂化。我选择使用 Python 编写一些代码来发送我的新年祝福。不是因为容易；这是因为它很难——在某种程度上。不管怎样，编码很有趣！

新年到了。虽然我们开发者一般不会费心维护友情(到底是不是开玩笑？)，我们不介意把我们的问候发给我们的朋友，只要有一个程序化的方法来完成这件事！

在这里，我将分两部分向您展示如何使用 Python 通过电子邮件和文本消息发送问候。

# 第一部分:发送电子邮件问候

Python 有一个名为`[smtplib](https://docs.python.org/3/library/smtplib.html)`的内置库——一个可以用来创建 SMTP 客户端会话对象的模块，允许我们向任何实施 SMTP 或 ESMTP 的电子邮件服务提供商发送电子邮件。发送电子邮件的另一个有用的模块是`email`包，它提供了与电子邮件相关的功能，比如读和写。

## **首先导入相关模块:** `**smtplib**` **和** `**email**` **。**

为了当前教程的简单性，我们将只发送一封纯文本电子邮件。为此，我们只需要 MIMEText 来创建电子邮件正文。在后面的教程中，我可以向您展示如何发送更复杂的电子邮件(例如，图像附件)。

```
import smtplib
import email
from email.mime.text import MIMEText
```

## **其次，创建发送邮件所需的变量。**

在本教程中，我将使用 gmail 帐户发送电子邮件。请注意，您需要更新您的 gmail 安全设置([https://myaccount.google.com/lesssecureapps](https://myaccount.google.com/lesssecureapps))，以允许您的电子邮件使用 Python 以编程方式发送。

```
email_host = "smtp.gmail.com"
email_port = 587
email_sender = "" # change it to your own gmail account
email_password = "" # change it to your gmail password
email_receivers = ["xxx@gmail.com", "yyy@gmail.com", "zzz@gmail.com"] # the list of recipient email addresses
```

## 第三，起草信息。

接下来，您可以撰写您的邮件。您可以将发件人指定为您自己的电子邮件帐户。对于打算发送的邮件列表，可以使用以下格式:首先，最后<xxxx>，这是邮件收件人的标准格式。</xxxx>

```
message = MIMEText("I wish you a great 2020.")
message["From"] = ""  # your email account
message["To"] = "John Smith<xxxx@gmail.com>, Mike Dickson<yyyyy@hotmail.com>" # the list of email addresses
message["Subject"] = "Happy New Year"
```

## 第四，发送消息。

您使用主机和端口来创建所需的 SMTP 会话。在初始化期间，Python 将通过调用 connect 方法自动建立 SMTP 连接。然后，出于安全原因，我们启动 TLS。

您只需要使用电子邮件帐户和密码来验证您的身份。如前所述，对于 gmail，您必须先放松安全设置，然后才能使用此方法登录。

您可以简单地调用`sendmail`方法来发送消息并退出会话。

```
email_server = smtplib.SMTP(email_host, email_port)
email_server.starttls()
email_server.login(email_sender, email_password)
email_server.sendmail(email_sender, email_receivers, message.as_string())
email_server.quit()
```

# 第二部分。发送短信问候

实际上，有多种方法可以使用 Python 通过短信发送新年祝福。

## 方法一。电子邮件发送选项的调整。

你们很多人可能都知道，许多手机运营商允许你用电子邮件发送短信。以下是美国主要航空公司的简短列表。

*   cell-phone-number@txt.att.net 美国电话电报公司
*   冲刺:cell-phone-number@messaging.sprintpcs.com
*   t-Mobile:cell-phone-number@tmomail.net
*   cell-phone-number@vtext.com 威瑞森

因此，您可以简单地使用 1234567890@txt.att.net 作为电子邮件收件人之一，而不是使用电子邮件地址作为`email_receivers`变量。

但是，有一个问题，你可能必须知道与该号码相关的运营商。如果你不知道，你可以写一些代码来创造所有可能的变化(如 1234567890@messaging.springtpcs.com，1234567890@tmomail.net)，以涵盖所有的可能性。我还听说有付费的第三方服务可以让你查找某个电话号码的运营商。但是我想这对于我们想要完成的目标来说有点过头了。

## 方法二。通过 AWS 发送

您也可以使用 AWS SNS 发送短信。显然，您需要注册一个 AWS 帐户，这超出了当前教程的范围。可以参考官网([https://AWS . Amazon . com/premium support/knowledge-center/create-and-activate-AWS-account/](https://aws.amazon.com/premiumsupport/knowledge-center/create-and-activate-aws-account/))。

## 首先，安装 AWS boto3 模块。

在终端或命令行工具中运行代码(`pip install boto3`)。boto3 库是为 AWS 相关管理开发的 Python 模块。

## 其次，用 Python 创建一个 SNS 客户端。

SNS 是一款 [AWS 产品](https://aws.amazon.com/sns)，允许开发者向最终用户发送文本消息和其他通信(例如，电子邮件)。

```
import boto3
client = boto3.client(
    "sns",
    aws_access_key_id="", # your aws access key
    aws_secret_access_key="", # your aws secrete access key
    region_name="us-east-1"
)
```

## 第三，创建话题，添加订阅者。

要向多个电话号码发送问候语，您需要创建一个主题，该主题允许您向该主题添加订阅者。每个用户由一个电话号码指定，电话号码还应该包括国家代码(例如，美国为+1)，即 E.164 格式。因为我们正在发送消息(即短消息服务或 sms)，所以我们将协议指定为`sms`。

```
topic = client.create_topic(Name="new_year") # create a topic
topic_arn = topic['TopicArn']  # get its Amazon Resource Name# Add SMS Subscribers to this topic
phone_numbers = ["+1234567890"] for phone_number in phone_numbers:
    client.subscribe(
        TopicArn=topic_arn,
        Protocol='sms',
        Endpoint=phone_number
    )
```

## 第四，发布消息给题目。

将订阅者添加到主题后，就可以向主题发布消息了。您只需要提供消息，并指定要发布该消息的主题。通过这样做，所有的用户都将收到文本消息。

```
client.publish(Message="Happy New Year!", TopicArn=topic_arn)
```

## 方法三。通过 Twilio 发送

您也可以选择使用 Twilio 发送短信。当然，它需要一个 Twilio 帐户，可以免费注册，类似于 AWS 帐户。一旦您创建了 Twilio 帐户，我们就可以开始玩游戏了！

## 首先，安装 Twilio 模块。

运行命令`pip install twilio`安装 Twilio 模块，这将使您能够访问 Twilio 提供的各种功能。短信只是其中之一。

## 其次，用 Python 创建一个 Twilio 客户机

`Cilent`方法将用于通过获取您的 Twilio 帐户 SID 和 AUTH Token 来创建 Twilio 客户端，这两者都可以在您的 Twilio 帐户的仪表板上找到。

```
from twilio.rest import Clientclient = Client(
    "", # your Twilio Account SID
    "", # your Twilio AUTH token
)
```

## 第三，创建和发送消息。

您创建了一个列表，列出了您要发送新年祝福的所有电话号码。请再次注意，电话号码使用 E.164 格式，类似于 AWS SNS 的使用。

一个问题是，使用免费的 Twilio 帐户，你只能向你验证过的电话号码发送短信，与 AWS SNS 相比，你的灵活性较低。但是，您可以升级您的 Twilio 帐户，这样您就可以选择以非常低的成本(每条消息不到 1 美分)向您选择的多个电话号码发送消息。

```
phone_numbers = ["+1234567890", "+1234567891"] # the list of phone numbers to receive the messagesfor phone_number in phone_numbers:
    client.messages.create(
        to=phone_number,
        from_="", # your Twilio phone number
        body="Happy New Year!"
    )
```

# 结论

通过学习本教程，您应该已经为每个作业创建了多个 python 脚本。以后，你可以使用这些脚本非常方便地发送电子邮件和短信。

所以实际上，编程确实使某些事情变得更容易，不是吗？