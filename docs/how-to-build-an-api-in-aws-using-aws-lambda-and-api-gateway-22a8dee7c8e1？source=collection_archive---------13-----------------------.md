# 如何在 AWS 中构建 API—使用 AWS Lambda 和 API Gateway

> 原文：<https://towardsdatascience.com/how-to-build-an-api-in-aws-using-aws-lambda-and-api-gateway-22a8dee7c8e1?source=collection_archive---------13----------------------->

## 因为 API 就是未来！

![](img/e3e4dcdc9f647a0da5b2129c16583a40.png)

如何在 AWS 中构建 API—使用 Lambda 和 API Gateway。Emile Perron 在 [Unsplash](https://unsplash.com/s/photos/api-programming?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) 上的照片

API 是应用编程接口的缩写，是允许不同(部分)计算机程序交换数据的连接点。使用 API 来交付软件服务可以使您的代码组织得更好，更容易重用。

在本文中，我将介绍使用 AWS Lambda 和 API Gateway 创建 API 的基本步骤。

该示例是一个非常短的代码示例，它将根据给定的长度、一些大写字母和一些数字字符创建一个随机密码。其他字符将是小写字母。您可以让 API 背后的服务变得尽可能大和先进，从出售数据到使用先进的人工智能模型进行预测。

让我们开始吧:

# 1.创建 lambda 函数

在 AWS 管理控制台上，转到 Lambda:

![](img/d93627927df3c06d73df19361f9aabc2.png)

创建新功能:

![](img/b907c1cc5abd39f55eb2db5b448a752a.png)

选择*“作者从零开始”*，给你的函数起个名字。在这种情况下，api 将生成安全的密码。

![](img/10f3f4f50b1b9ae7f6e7ea98024260ed.png)

# 2.编写 lambda 函数

在新创建的 lambda 函数中，转到函数代码:

![](img/a462687ff55c30fc2189b5575a2dd0ff.png)

并编写您的函数:

![](img/8b577f7c699e3ab0967aba6b0e54a529.png)

我使用的代码如下:

# 3.配置测试

进入测试菜单，点击配置测试。

![](img/deb7bf43d1bd9b1d4e37184fecd9953f.png)

单击 create new test event，并在单击 create 之前在字典中为您的测试指定输入:

![](img/dc869d0c19d964ba0f5a50618b5c9de1.png)

通过点击测试来测试你的 Lambda 函数:

![](img/174350f4c5c5e7605013e7480ae1498b.png)

您会看到它在字典(或 json)的“主体”中发回了一个密码。

# 4.构建 API 网关

通过 AWS 管理控制台转到 API 网关。点击 REST API。

![](img/22f970a55bb51f70da40091d40e19251.png)

单击新建 API 并选择一个名称:

![](img/434bb3f4e0a7c8bb9852b7db70f237ef.png)

创建一个 POST 方法，并选择集成类型“Lambda Function”。然后选择 Lambda 函数的名称。

![](img/e0b35abce5c9afb68292538b5f742e91.png)

您现在应该会看到:

![](img/16dfc4bb27f6ccc2459db0bea5567f6b.png)

最后一步，部署您的 API:

![](img/b9930fcbfee1eb831760e3d0da42c237.png)

可以命名为“ *prod* ”例如:

![](img/77c18578a89221b4cd2ce143f3c7b2d3.png)

API 网关现在应该告诉您找到 API 的 URL:

![](img/d525b7c1800c46d1f9677e84b4bc6fc3.png)

# 5.真正测试你的 API

现在，您可以使用 Python 笔记本或任何您喜欢的东西来测试您的 API。

如果你把它复制粘贴到一个笔记本上，API 会给你发送一个新的密码！