# 通过 Boto3 使用 Amazon SNS

> 原文：<https://towardsdatascience.com/working-with-amazon-sns-with-boto3-7acb1347622d?source=collection_archive---------7----------------------->

## 完整的备忘单。

![](img/578d168031578fc5c731f4351d83297f.png)

照片由[杰米街](https://unsplash.com/@jamie452?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)上的 [Unsplash](https://unsplash.com/s/photos/notification?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)

亚马逊简单通知服务，或 SNS，允许我们自动发送消息，如电子邮件或短信。我们还可以通过 HTTP Post 向指定的 URL 发送消息，这是一种分离微服务的便捷方式。当使用 Python 时，人们可以通过 Boto3 包轻松地与 SNS 进行交互。在这篇文章中，我将把我在使用 SNS 时经常用到的 Python 命令汇总在一起。希望你会觉得有用。

![](img/4fa79d8e1efffb34c1c49ca6a5ee5eb5.png)

让我们从一些基本的社交网络概念开始。你可以把 SNS 系统想象成一个群聊。一旦您在聊天中发布消息，所有参与者都会收到一个弹出消息。在 SNS 设置中，群聊将被称为**话题**。您可以**向主题发布**消息，一旦发布，所有主题**订阅者**都会收到通知。

AWS 资源可以通过**亚马逊资源名称** **(ARNs)** 唯一标识。每个主题都有其独特的主题卡，每个主题的订阅都有其独特的订阅号。

![](img/4fa79d8e1efffb34c1c49ca6a5ee5eb5.png)

## 设置客户端

要用 Boto3 访问任何 AWS 服务，我们必须用一个客户机连接到它。在这里，我们创建一个 SNS 客户端。我们指定保存消息的区域。我们还必须传递访问密钥和密码，这可以在 AWS 控制台中生成，如这里的[所述](https://docs.aws.amazon.com/IAM/latest/UserGuide/id_credentials_access-keys.html#Using_CreateAccessKey)。

## 主题:列出、创建和删除

为了创建一个新的主题，我们使用了`create_topic()`函数，传递所需的名称。一旦主题被创建，我们可以通过从由`craete_topic()`返回的对象中提取`TopicArn`键来获得它的 ARN。
要列出 AWS 上已经存在的主题，我们可以使用`list_topics()`函数并从其输出中提取`Topics`键。要删除一个主题，我们只需将它的 ARN 传递给`delete_topic()`函数。

## 订阅:列出、创建和删除

为了创建主题的新订阅，我们调用`subscribe()`函数，传递要订阅的主题的 ARN、协议(例如 SMS 或电子邮件)和端点(例如 SMS 协议的电话号码或电子邮件协议的电子邮件地址)。一旦订阅被创建，我们可以通过从由`subscribe()`返回的对象中提取`SubscriptionArn`键来获得它的 ARN。
要列出 AWS 上现有的所有订阅，我们可以使用`list_subscriptions()`函数并从其输出中提取`Subscriptions`键。类似地，为了列出特定主题的订阅，我们调用了`list_subscriptions_by_topic()`函数。
要删除一个订阅，我们将它的 ARN 传递给`unsubscribe()`函数。为了删除多个订阅，例如具有相同协议的所有订阅，我们可以使用下面代码块底部的 for-loop 对它们进行循环。

## 发布到主题

要向主题发布消息，我们只需调用`publish()`函数，传递主题的 ARN、想要的消息和可选的主题(它将只用于电子邮件消息)。
或者，我们可以发布没有任何主题或订阅的 SMS 消息。为此，我们调用`publish()`函数，将电话号码和消息文本作为参数传递。

![](img/4fa79d8e1efffb34c1c49ca6a5ee5eb5.png)

感谢阅读！

如果你喜欢这篇文章，为什么不订阅电子邮件更新我的新文章呢？并且通过 [**成为媒介会员**](https://michaloleszak.medium.com/membership) ，可以支持我的写作，获得其他作者和我自己的所有故事的无限访问权限。

需要咨询？你可以问我任何事情，也可以在这里为我预约 1:1[](http://hiretheauthor.com/michal)**。**

**也可以试试 [**我的其他文章**](https://michaloleszak.github.io/blog/) 中的一篇。不能选择？从这些中选择一个:**

**[](/working-with-amazon-s3-buckets-with-boto3-785252ea22e0) [## 使用 Boto3 处理亚马逊 S3 桶。

### 完整的备忘单。

towardsdatascience.com](/working-with-amazon-s3-buckets-with-boto3-785252ea22e0) [](/non-linear-regression-basis-expansion-polynomials-splines-2d7adb2cc226) [## 非线性回归:基础扩展、多项式和样条

### 如何用多项式和样条捕捉非线性关系？

towardsdatascience.com](/non-linear-regression-basis-expansion-polynomials-splines-2d7adb2cc226) [](/model-selection-assessment-bb2d74229172) [## 模型选择和评估

### 超越火车价值测试分裂

towardsdatascience.com](/model-selection-assessment-bb2d74229172)**