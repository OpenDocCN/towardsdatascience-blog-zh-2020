# 自动从 Microsoft Exchange Web 服务下载电子邮件附件

> 原文：<https://towardsdatascience.com/download-email-attachment-from-microsoft-exchange-web-services-automatically-9e20770f90ea?source=collection_archive---------10----------------------->

## 用 Python 自动化枯燥的程序

## 学习使用 Python 库 Exchangelib 处理电子邮件附件

![](img/42f8d4935ffc1291b603c75496d8977f.png)

[Webaroo.com.au](https://unsplash.com/@webaroo?utm_source=medium&utm_medium=referral)在 [Unsplash](https://unsplash.com/?utm_source=medium&utm_medium=referral) 上拍照

# 介绍

您需要定期下载电子邮件附件吗？你想让这个无聊的过程自动化吗？我知道那种感觉，兄弟。刚来工作的时候，我被分配了一个日常任务:每天从发给我们团队的邮件里下载附呈的报告。这不是一个困难的任务，但它非常无聊，我经常忘记这样做。

> 我怎样才能摆脱这个虚拟任务:定期下载电子邮件附件。

在研究了互联网之后，我发现一个小小的 Python 脚本可以接管我的工作。让我们来探索一下 Python 能帮到我们什么。

# Exchangelib

Exchangelib 是一个 Python 库，它提供了一个简单的接口，允许 Python 脚本与 Microsoft Exchange 或 Exchange Web 服务(EWS)进行交互

[](https://github.com/nylas/exchangelib) [## nylas/exchangelib

### 该模块提供了一个性能良好、行为良好、独立于平台的简单接口，用于通信…

github.com](https://github.com/nylas/exchangelib) 

您可以从 PyPI 安装这个包:

```
pip install exchangelib
```

然后按照官方网站的说明导入包。你可能不会全部用到，

```
#import pytzfrom exchangelib import DELEGATE, IMPERSONATION, Account, Credentials, ServiceAccount, EWSDateTime, EWSTimeZone, Configuration, NTLM, GSSAPI, CalendarItem, Message, Mailbox, Attendee, Q, ExtendedProperty, FileAttachment, ItemAttachment, HTMLBody, Build, Version, FolderCollection
```

下一步是指定您的凭证，即登录用户名和密码

```
credentials = Credentials([username='john@example.com](mailto:username='john@example.com)', password='topsecret')
```

# 邮件服务器配置

当然，您必须配置邮件服务器。Exchangelib 应该能够识别您的电子邮件服务器使用的身份验证类型，但是在我的例子中，它失败了，我将身份验证类型指定为 NTLM。

```
ews_url = 'mail.example.com'
ews_auth_type = 'NTLM'
primary_smtp_address = 'john@example.com'config = Configuration(service_endpoint=ews_url, credentials=credentials, auth_type=ews_auth_type)# An Account is the account on the Exchange server that you want to connect to.account = Account(
primary_smtp_address=primary_smtp_address,
config=config, autodiscover=False,
access_type=DELEGATE)
```

# 从文件夹下载附件

现在您有了一个 Account 对象，您可以在其中导航。它只是遵循你的电子邮件帐户文件夹结构。比如你想对收件箱文件夹进行操作，就用这个简单的语法:`account.inbox`。要操作收件箱里面的一个文件夹，只需要像这样放一个反斜杠和单引号:`account.inbox / 'some_folder'`。

如果您只想下载一个文件夹中的所有附件，这个短代码会有所帮助。您可能需要`os`包来处理本地目录。

```
some_folder = account.inbox / 'some_folder'for item in some_folder.all():
    for attachment in item.attachments:
        if isinstance(attachment, FileAttachment):
            local_path = os.path.join(local_path, attachment.name)
            with open(local_path, 'wb') as f:
                f.write(attachment.content)
```

> 就是这样！

# 奖金部分

如果我只想下载我以前没有下载过的文件，我可以进一步做什么？我需要一种机制来检查附件是否存在于本地。

我们的团队使用 FTP 服务器来存储所有附加的报告。因此，在我的 Python 脚本中，首先，我登录到 FTP 服务器并列出所有带有指定前缀的文件。在脚本中添加了一项检查，以确定该文件是否存在于当前文件列表中。

为此，我使用了 ftplib，这是一个 FTP 协议客户端库。

 [## ftplib — FTP 协议客户端— Python 3.8.1 文档

### 源代码:Lib/ftplib.py 这个模块定义了类和一些相关的项。该类实现了客户端…

docs.python.org](https://docs.python.org/3/library/ftplib.html) 

以下代码登录到 FTP 服务器(`ftp.login()`)，导航到目标文件夹(`ftp.cwd()`)并列出所有匹配指定前缀(`file.startswith()`)的文件(`ftp.nlst()`)

```
from ftplib import FTPftp_server_ip = FTP_SERVER_IP
username = 'username'
password = 'password'
remote_path = 'remote_path'
local_path = 'local_path'with FTP(ftp_server_ip) as ftp:
    ftp.login(user=username, passwd=password)
    ftp.cwd(remote_path + '/copied data')
    filelist = [file for file in ftp.nlst() if file.startswith('YOUR_FILE_PREFIX')]
```

在本地下载附件后，我将文件上传到 FTP 服务器。我用`storbinary`向 STOR 发送上传附件的命令。

```
if attachment.name not in filelist:
# Check if the attachment downloaded before
    local_path = os.path.join(local_path, attachment.name)
    with open(local_path, 'wb') as f:
        f.write(attachment.content)with FTP(ftp_server_ip) as ftp:
    ftp.login(user=username, passwd=password)
    ftp.cwd(remote_path)
    file = open(local_path, 'rb')
    ftp.storbinary('STOR {}'.format(attachment.name), file)
    file.close()
```

# 结论

我使用一个简单的 Python 脚本自动完成了这个日常工作，该脚本可以定期运行。该脚本自动化了从邮件服务器下载丢失的附件并将它们上传到 FTP 服务器的工作流。