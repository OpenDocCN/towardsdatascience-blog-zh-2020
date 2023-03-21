# 用 Python 发送股票数据到你的手机！

> 原文：<https://towardsdatascience.com/send-stock-data-to-your-phone-in-seconds-with-python-39759c085052?source=collection_archive---------61----------------------->

## 了解如何利用 Python 的力量在股票市场中取胜…

![](img/0a3ab236ad1fe72815781ebb9a680be6.png)

来自 [Pixabay](https://pixabay.com/?utm_source=link-attribution&amp;utm_medium=referral&amp;utm_campaign=image&amp;utm_content=2891817) 的 [3D 动画制作公司](https://pixabay.com/users/QuinceCreative-1031690/?utm_source=link-attribution&amp;utm_medium=referral&amp;utm_campaign=image&amp;utm_content=2891817)的图片

随着股市每天都在快速变化，许多初出茅庐的投资者使用 Robinhood 等移动应用进行交易，不断保持更新是必不可少的，否则就有被落在后面的风险。这正是为什么我创建了这个快速的~150 行 Python 程序，它可以将实时股票数据直接发送到我的手机上，以便于访问。既然对这样一个程序的需求已经很清楚了，那就让我们开始编写代码吧！

导入依赖项并设置参数

首先，我们必须导入将在代码中使用的依赖项，并设置程序正确执行所需的参数。如果有任何模块没有下载到您的机器上，您可以很容易地打开您的终端或命令提示符，并使用 pip 来安装软件包。stock_list 变量必须设置为股票列表，如上所示。开始和结束日期可以更改，但必须保持日期时间格式！

将消息直接发送到您的手机的功能！

为了让代码工作，我们必须用我们的电子邮件更新 email 变量，用相应电子邮件的密码更新 password 变量。对于 sms_gateway 变量，您可以直接进入这个[链接](https://freesmsgateway.info/)，输入您的电话号码，然后复制 **SMS 网关地址**响应。SMS 网关地址的一个例子是 1234567890@tmomail.net。您也可以根据需要更改文本的主题，但我将它保留为股票数据。

***免责声明:*您必须在 Gmail 帐户设置中将“允许不太安全的应用程序”设置为开！** *这允许 Python 访问您的 Gmail 帐户来发送 SMS 文本消息。如果你有困惑，请按照这个* [*教程*](https://hotter.io/docs/email-accounts/secure-app-gmail/) *！*

现在我们已经设置了 sendMessage 函数，我们可以继续实际获取数据了！

收集股票数据并发送消息的功能！

上面的代码由 getData 函数组成，它计算每只股票的不同指标，然后回调 sendMessage 函数向您的手机发送 SMS 消息。

在这个特定的示例中，度量是使用模块 TaLib、NumPy 和 nltk 计算的。它们包括股票的当前价格、当前新闻情绪、beta 值和相对强弱指数(RSI)值。当然，这可以根据您想要接收的数据根据您的喜好进行更改。

程序的最后两行调用 getData 函数，该函数又调用我们不久前创建的 sendMessage 函数。

这就是这个项目的全部内容，如果您对代码的任何部分或任何指标的计算有任何疑问，请随时联系我们。整个程序的所有代码都在下面的 GitHub gist 中！

所有的代码！

请记住在您的 Gmail 帐户设置中将“允许不太安全的应用程序”设置为“开”,这样该程序才能运行！我希望这段代码在将来对你有用，非常感谢你的阅读！

*免责声明:本文材料纯属教育性质，不应作为专业投资建议。自行决定投资。*

如果你喜欢这篇文章，可以看看下面我写的其他一些 Python for Finance 文章！

[](/parse-thousands-of-stock-recommendations-in-minutes-with-python-6e3e562f156d) [## 使用 Python 在几分钟内解析数千份股票推荐！

### 了解如何在不到 3 分钟的时间内解析顶级分析师的数千条建议！

towardsdatascience.com](/parse-thousands-of-stock-recommendations-in-minutes-with-python-6e3e562f156d) [](/making-a-stock-screener-with-python-4f591b198261) [## 用 Python 制作股票筛选程序！

### 学习如何用 Python 制作一个基于 Mark Minervini 的趋势模板的强大的股票筛选工具。

towardsdatascience.com](/making-a-stock-screener-with-python-4f591b198261) [](/creating-a-finance-web-app-in-3-minutes-8273d56a39f8) [## 在 3 分钟内创建一个财务 Web 应用程序！

### 了解如何使用 Python 中的 Streamlit 创建技术分析应用程序！

towardsdatascience.com](/creating-a-finance-web-app-in-3-minutes-8273d56a39f8)