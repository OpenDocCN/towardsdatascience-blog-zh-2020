# 使用 Amazon Macie 和 AppFlow 为 Slack 提供数据丢失防护(DLP)

> 原文：<https://towardsdatascience.com/dlp-for-slack-using-amazon-macie-appflow-50990e2269e5?source=collection_archive---------57----------------------->

## AWS 安全性

## 使用 Amazon Macie、AppFlow 的 Slack 集成、CloudWatch 事件来检测和通知 Slack 通道中的敏感数据泄漏

![](img/1d138cb47fd2bb587a2c790399d518af.png)

照片由 [Fernando Arcos](https://www.pexels.com/@ferarcosn?utm_content=attributionCopyText&utm_medium=referral&utm_source=pexels) 从 [Pexels](https://www.pexels.com/photo/white-caution-cone-on-keyboard-211151/?utm_content=attributionCopyText&utm_medium=referral&utm_source=pexels) 拍摄

AWS 最近宣布了一项名为亚马逊 AppFlow 的新服务。Amazon AppFlow 允许我们集成和自动化各种软件即服务(SaaS)应用程序和 AWS 服务之间的数据流。

这可以在许多方面改变游戏规则，并可以开辟许多新的可能性和有趣的用例。Amazon AppFlow 可以支持从 SaaS 应用程序到 AWS 以及从受支持的 AWS 服务到 SaaS 应用程序的数据流。在 SaaS 应用程序已经与 [AWS PrivateLink](https://aws.amazon.com/privatelink/) 集成的情况下，数据流也可以以安全的方式发生。

> 让我们考虑一个有趣的数据泄漏场景

如今，许多企业使用 Slack 这样的交流平台进行更好的协作，甚至使用 ChatOps。我在这个场景中选择了 Slack，因为在我写这篇文章的时候，它是 Amazon AppFlow 中唯一支持的通信平台。

## 方案

DevOps 团队表示，Slack 用户可能会错误地在 Slack 频道上发布敏感信息(PII、社会安全号码、信用卡号、访问密钥等等)。

*作为一项企业指导方针，您可能希望实施相关政策，以阻止或至少通知用户在其他渠道/团队成员可以看到的空闲渠道上共享此类信息。*

给定场景，让我们看看如何使用 AWS 服务构建解决方案。

> 解决方案看起来怎么样？

# 体系结构

![](img/1456b7f9e728ba4912aec40ce96679ca.png)

无服务器解决方案架构

高级实施步骤—

> 将 Amazon AppFlow 与一个 Slack 通道集成为源。
> 
> 将对话数据上传到亚马逊 S3。
> 
> 使用 Amazon Macie 来发现和检测 S3 内容中的敏感数据。
> 
> 使用 Amazon Macie 生成的 Amazon CloudWatch 事件采取适当的措施进行补救或通知。

# 实施细节

## 将 Amazon AppFlow 与一个 Slack 通道集成为源

作为此步骤的先决条件，您需要以下详细信息来从 AppFlow 连接到 Slack。每当您尝试将 AppFlow 流连接配置为 Slack 时，都会看到此弹出窗口。如果需要建立新的连接，请参考下面的步骤 3。

![](img/4b568776d7419c91e7de678d7c9e7cae.png)

图片:松弛连接细节

> 设置 Amazon AppFlow

> **步骤 1:为亚马逊 AppFlow 创建一个 S3 桶**

创建一个 S3 桶，Amazon AppFlow 将从 Slack 上传数据。此时不需要额外的配置。如果需要，请检查此链接—

 [## 我如何创建一个 S3 桶？

### 在将数据上传到亚马逊 S3 之前，您必须在其中一个 AWS 区域创建一个存储桶来存储您的数据。之后…

docs.aws.amazon.com](https://docs.aws.amazon.com/AmazonS3/latest/user-guide/create-bucket.html) 

> **第二步:指定流程细节**

提供流名称、描述、加密和可选标记。

![](img/1b050d72ccf24f39757fc5df3de4631f.png)

图片:流量详情

请注意，Amazon Macie 在加密数据发现方面有一定的限制。正如您将看到的，我们将为 Amazon Macie 使用`AWSServiceRoleForAmazonMacie`服务关联角色，我们将不得不使用自定义的 AWS KMS 客户管理密钥(CMK)，如下面的截图“图片:数据加密”所示。如果您还没有可以使用的 AWS KMS 键，请创建一个。

注意:我必须获得这个配置部分以及 KMS 密钥权限(在下面的后面部分讨论),并且还要经过 AWS 的验证，因为这些信息并不容易获得。

![](img/27167f0f55280a4b083cfa24e70fa62f.png)

图片:数据加密

有关亚马逊 Macie 的加密/解密相关细节，请参考此链接—

 [## 用 Amazon Macie 分析加密的 S3 对象

### 如果一个对象存储在亚马逊 S3 存储桶中并且是加密的，亚马逊 Macie 可能无法分析其中的数据…

docs.aws.amazon.com](https://docs.aws.amazon.com/macie/latest/user/discovery-supported-encryption-types.html) 

> **第三步:配置流量**

这里需要创建一个新的松弛连接。遵循此链接中仔细提供的说明——

 [## 亚马逊 AppFlow 的要求

### 在使用 Amazon AppFlow 创建流之前，请确认您拥有关于源和…的所需信息

docs.aws.amazon.com](https://docs.aws.amazon.com/appflow/latest/userguide/requirements.html#slack) 

一旦创建了连接，就配置您想要监视的 Slack 通道。目前，不可能在同一个流中选择多个通道。因此，如果您想要监视多个通道，您必须配置多个流。

![](img/13f7b3be53c68afd0fe7719f26defc6a.png)

图片:配置流量

配置 Amazon AppFlow 将在其中上传松弛渠道对话数据的目标 S3 存储桶。

![](img/1b5a55661a7ad67419666d34ed8fd53a.png)

图片:目的地详情

可以根据需要安排流触发。松弛集成不支持“事件时运行流”触发器。

![](img/e180effb4ed9b23f674c441dd496df10.png)

图片:流量触发器

> **步骤 4:创建源和目标数据字段之间的映射**

在此步骤中，您可以选择需要映射到目标字段的源字段。在本例中，我已经映射了所有字段。您可以只选择几个您需要的，它允许您做一些数据修改，以及像屏蔽字段。

![](img/145bcabf028d223afe893caaf9f1b99c.png)

图片:数据映射

我没有添加任何验证或过滤器。继续做你喜欢的实验。

最后，创造流动。

创建完成后，在 Slack 通道中提交一些消息，看看 AppFlow 是否成功地将数据上传到 S3。测试数据应该是 Amazon Macie 能够正确检测的有效格式。

![](img/39619097887418f38fd394f17b1d0658.png)

仅测试数据！😄

一旦流量开始触发，您可以看到如下运行历史。现在忽略 0 KB 的大小。当流处理非空数据时，它将显示正确的大小。

*注意,“错误”记录不提供任何故障排除选项。没有可供浏览的链接，没有 CloudWatch 日志，只是一个黑洞。在 AWS 在未来的版本中提供一种方法之前，您只能靠自己找出问题所在！我现在已经提交了一个功能请求。如果您找到了进一步排除故障的方法，请与我们分享。*

![](img/40082932c1de982b5bba6d6fc5c583c5.png)

图片:跑步记录

## 将对话数据上传到亚马逊 S3

如果流运行成功，它应该将数据上传到配置的 S3 存储桶。上传的对象存储在使用流名称创建的路径下。

![](img/c95c427bed420ee9d622be582c392748.png)

图片:S3

根据配置的数据映射，S3 对象内容可能如下所示

```
{“client_msg_id”:”51c6e23c-abb2–4dc2–82e6–12ff9216af58",”text”:”aws_access_key AKIAJVCPCOZKBNJFRAXQ”,”ts”:”1591923645.001300",”type”:”message”}
```

> S3 铲斗配置

Amazon AppFlow 自动配置了一个 S3 存储桶策略以及一个如下所示的策略声明。

![](img/2678808062e3e964bd67cecc0fe8e51a.png)

图片:AppFlow S3 桶政策

这种策略对于 Amazon AppFlow 来说很好，但是我们必须添加一个额外的策略，如下图所示，以允许 Amazon Macie 从 S3 桶中读取数据。这里的权限可能看起来过于宽松。继续尝试，使其更具限制性。

![](img/cd9f52bea68a5bba8469677e713996af.png)

图片:亚马逊 Macie 的 S3 桶政策声明

## 使用 Amazon Macie 发现和检测 S3 内容中的敏感数据

随着数据上传到 S3，现在我们可以配置 Amazon Macie 来分类 S3 数据，并在检测到敏感数据时接收生成的警报。

亚马逊 Macie 使用机器学习来自动发现、分类和保护存储在 S3 桶中的敏感数据。

## 使用 Amazon Macie 生成的 Amazon CloudWatch 事件采取适当的补救或通知措施

可以捕获由 Amazon Macie 发现生成的 CloudWatch 事件。可以创建一个事件规则来向所需的目标发送通知。现在，我们将配置一个简单的 SNS 主题作为规则的通知目标。

> 让我们在配置 Macie 之前配置通知系统

> **第一步:创建或重用一个 SNS 主题，需要订阅者**

请参考此链接创建 SNS 主题-

 [## 设置 Amazon SNS 通知

### 或者，如果您计划使用 AWS 管理控制台创建 CloudWatch 警报，您可以跳过这一步…

docs.aws.amazon.com](https://docs.aws.amazon.com/AmazonCloudWatch/latest/monitoring/US_SetupSNS.html) 

> **第二步:创建一个云观察事件规则**

使用下面的事件模式和 SNS 主题创建一个 CloudWatch 事件规则。

![](img/4423033e37971ddbaee90cced618a4d9.png)

图片:Macie 的 CloudWatch 事件规则

> 现在，让我们配置 Amazon Macie

*先决条件:*需要启用亚马逊 Macie。如果你已经在使用 Macie 经典版，建议[升级到新版本](https://docs.aws.amazon.com/macie/latest/userguide/macie-migration.html)。新版本有很多改进。同样参考[这篇文章](https://aws.amazon.com/blogs/aws/new-enhanced-amazon-macie-now-available/)。

启用 Macie 后，您会看到一个类似下图的带有摘要视图的仪表板。Macie 可能需要一些时间来发现和显示数据，具体取决于数据量。

![](img/e9d4d094ad8c59aae0a71e9c9c5a361c.png)

图片:亚马逊 Macie 仪表盘

> **第一步:配置 AWS KMS CMK 权限**

添加如下所示的关键策略语句，以允许 Amazon Macie 的服务关联角色使用 CMK 密钥解密由 Amazon AppFlow 加密的 S3 对象。

![](img/3498dbd1d1b7c7fab396284474369ac2.png)

图片:AWS KMS CMK 亚马逊 Macie 关键政策声明

> **步骤 2:创建自定义数据标识符(可选；取决于使用案例)**

我正在为这个特定的用例演示配置自定义数据标识符。但是，它对你来说可能是可选的。我将在一个 [Macie 数据分类作业](https://docs.aws.amazon.com/macie/latest/user/discovery-jobs.html)配置中使用它们。

我已经配置了 2 个基于正则表达式的标识符，分别用正则表达式`AKIA[0–9A-Z]{16}`和`[0–9a-zA-Z/+]{40}`来检测 AWS 访问密钥和 AWS 秘密密钥。

![](img/fda64eff96a8ccb4c0dc0fe1a6f9af38.png)

设置自定义数据标识符

> **步骤 3:创建数据分类作业**

转至“S3 存储桶”页面，选择要发现和分类数据的存储桶，然后单击“创建作业”。单击下一步。

![](img/cfabddb0367c7c882182bc1b66924740.png)

图片:S3 水桶精选

在“范围”屏幕中，配置作业执行频率和数据发现标准。我为此用例选择了“一次性工作”。而且，因为我想发现所有数据并进行分类，所以我没有配置任何包含和/或排除标准。

![](img/87e25e8d8830ad8e29a107e2bd72a07c.png)

图片:发现和分类范围

如果需要，选择自定义数据标识符。

![](img/43409d624dac58637624dcb24ab4a72f.png)

图片:添加自定义数据标识符

提供作业名称和描述，并创建作业。

![](img/fe3f7b9e931b04b84f8f50e8b9cdc7bb.png)

图片:工作名称和描述

根据需要发现和分类的数据量，这项工作需要一些时间才能完成。

![](img/356f3259ede55671bd04dbb74e818b4c.png)

图片:工作列表

> **第四步:查看亚马逊 Macie 调查结果**

一旦作业发现敏感数据，就可以在“调查结果”屏幕下过滤和搜索调查结果。Amazon Macie 显示了调查结果的严重程度、类型和其他细节，可以进一步深入研究。

![](img/42c6c62ba5c3356fea0094bf725def8c.png)

图片:工作发现

> 从时差中发送更多数据

现在我们已经有了预期的解决方案，启动 Slack 并在通道中发送更多带有适当敏感数据的消息。

假设 Amazon AppFlow 每隔几分钟就处理一次数据，我们应该会在 Macie 中看到警告，也会收到 SNS 通知。

下面的通知是普通的 SNS 消息格式，所以可能不是很直观。根据 CloudWatch 事件目标，可以采取适当的措施，并根据需要处理数据。但是，无论如何你得到了这个想法！

![](img/8b3d984bde28810e858c23cb6b29af89.png)

图片:SNS 通知

# 结论

谢谢你读到这里！

因此，这篇文章展示了我们如何构建一个 AWS 解决方案来检测和通知基于聊天的通信平台中的敏感数据泄漏场景。该架构完全无服务器、安全且可扩展。

关于这个解决方案，有一点需要注意

上传到 Slack 频道的文档和图像不能被 Amazon AppFlow 处理，也不能以相同的格式存储在 S3。因此，亚马逊 Macie 将无法检测通过此类对象传递的敏感信息。

*如果需要这样的能力，我们应该能够使用 Slack APIs 和 AWS Lambda 构建一个解决方案来检索数据，并将其存储回 S3 供 Amazon Macie 分析。*

根据需求，可以使用亚马逊人工智能服务(如 Rekognition、Textract 等)构建各种这样的解决方案。也是。

# 参考

[](https://aws.amazon.com/appflow/) [## 亚马逊应用流-亚马逊网络服务

### 安全集成应用程序并自动处理任意规模的数据流，无需代码。Amazon AppFlow 是一个完全托管的…

aws.amazon.com](https://aws.amazon.com/appflow/) [](https://aws.amazon.com/blogs/aws/new-announcing-amazon-appflow/) [## 新-宣布亚马逊应用流|亚马逊网络服务

### 软件即服务(SaaS)应用对我们的客户越来越重要，采用率也在不断提高…

aws.amazon.com](https://aws.amazon.com/blogs/aws/new-announcing-amazon-appflow/)  [## 什么是亚马逊 Macie？

### 亚马逊 Macie 是一个完全托管的数据安全和数据隐私服务，使用机器学习和模式匹配…

docs.aws.amazon.com](https://docs.aws.amazon.com/macie/latest/user/what-is-macie.html) [](https://aws.amazon.com/blogs/aws/new-enhanced-amazon-macie-now-available/) [## 全新增强的 Amazon Macie 现已推出，价格大幅降低| Amazon Web Services

### Amazon Macie 是一个完全托管的服务，它使用机器学习来帮助您发现和保护您的敏感数据…

aws.amazon.com](https://aws.amazon.com/blogs/aws/new-enhanced-amazon-macie-now-available/) 

*关注我上*[*LinkedIn*](https://www.linkedin.com/in/bideep/)*，*[*Twitter*](https://twitter.com/bbideep)*。*

[*在*](https://bbideep.medium.com/subscribe) *上订阅更多此类帖子。*