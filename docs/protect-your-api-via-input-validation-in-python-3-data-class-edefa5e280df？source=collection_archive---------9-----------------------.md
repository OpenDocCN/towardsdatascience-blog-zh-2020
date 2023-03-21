# 通过 Python 3 数据类中的输入验证来保护您的 API

> 原文：<https://towardsdatascience.com/protect-your-api-via-input-validation-in-python-3-data-class-edefa5e280df?source=collection_archive---------9----------------------->

## 将 json 输入反序列化为 Python 3 数据类，并验证输入值以保护您的 API

![](img/13f9d28081cc9554ca84e60e959cbe6f.png)

照片由 [Masaaki Komori](https://unsplash.com/@gaspanik?utm_source=medium&utm_medium=referral) 在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄

# 介绍

在构建 web API 时，我们不可避免地需要保护它，否则它可能会被恶意用户利用。一种方法是对 API 接收的请求体进行输入验证。如果这是一个错误的输入，例如，我们的 API 期望一个电子邮件地址，但是却给了一个地址，那么，我们的 API 将(或者应该)拒绝该数据并返回一个状态代码`400 — BAD REQUEST`。

服务器端的输入验证必须在客户端的输入验证之上完成。原因是来自客户端的数据不可信，因为在数据到达服务器之前，任何事情都可能发生，例如，中间人(MITM)攻击。此外，请记住，请求可能来自任何地方，例如，Postman 或 curl，而不仅仅是来自您的 web 和/或移动应用程序。

# 你会学到什么

您将学习如何使用 Python 3 的`dataclass`对 API 请求体建模，并使用`marshmallow-dataclass`包对其进行输入验证。

我们将在本教程中介绍的用例是一个 API，它接受一个电子邮件地址作为输入，如果用户数据库中不存在该电子邮件地址，它将基于该电子邮件地址创建一个新的用户帐户。

本教程的系统详细信息:

*   Python: `3.8.1`
*   皮普:`19.3.1`
*   虚拟人:`16.7.9`
*   马科斯·卡特琳娜:`10.15.2`

# 安装依赖项

需要安装两个依赖包:

*   棉花糖-数据类:`7.2.1`
*   dataclass-json: `0.3.6`

让我们继续将它们安装在您的 Python 项目的虚拟环境中。

使用 pip 安装 marshmallow-dataclass

# 编写请求体模型

如前所述，我们将使用 Python 3 的`dataclass`来建模我们的 API 请求体。输入是一个电子邮件地址。继续创建基本的`dataclass`，不需要任何输入验证。

没有输入验证的初始数据类

接下来，我们还将添加`@dataclass_json` decorator，以便能够将 json 请求体反序列化到`CreateUser`数据类。

创建用 dataclass_json 修饰的用户

现在，如果我们试图向`CreateUser`传递任何类型的输入，它都会接受。

示例显示 CreateUser 接受任何类型的输入

这不太好，是吗？😰

让我们在下一节看看如何改进它。

# 添加输入字段验证

我们将为`CreateUser`数据类的`emailAddress`字段添加一个验证器。

使用电子邮件地址验证创建用户

现在，继续尝试通过传递不同的输入值来创建`CreateUser`的对象。

CreateUser 对象仍然接受任何类型的输入

不幸的是，我们的`CreateUser`数据类仍然接受任何类型的输入。😦

# 使用模式去序列化和验证

输入验证的工作方式是通过一个代表我们感兴趣的`dataclass`的**模式**。在前面的部分中，我们还没有创建模式，这就是验证没有工作的原因。

为了创建`CreateUser`的模式，我们需要使用`marshmallow_dataclass`。

添加 CreateUserSchema

同样，我们初始化一个`CreateUser`对象的方式需要调整，以利用我们刚刚生成的`CreateUserSchema`模式。

创建 CreateUser 对象的模式示例

我们的输入验证与`CreateUserSchema`一起工作。这样，我们的 API 将受到保护，不会收到不是有效电子邮件地址的垃圾输入。🙂

# 使用模式进行输入验证的更简单方法

最后，我想向您展示我们如何从`dataclass`中自动创建模式。

为此，我们需要使用由`marshmallow_dataclass`包提供的`@dataclass`装饰器，它的行为就像 Python 3.x 的标准`dataclasses`库中的`@dataclass`装饰器一样。

让我们修改我们的`CreateUser`数据类并删除`CreateUserSchema`，因为它的创建将是自动的。

使用 marshmallow_dataclass 自动创建模式

让我们看看如何使用修改后的`CreateUser`数据类创建一个`CreateUser`对象。

使用自动化模式创建 CreateUser 对象的示例

我们的代码现在简单多了，输入验证也像预期的那样工作。我们成功了，伙计们！💪

![](img/bdcc00a78a86bb1b4b8f9ef4c291323f.png)

照片由 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上的[雅弗桅杆](https://unsplash.com/@japhethmast?utm_source=medium&utm_medium=referral)拍摄