# AWS IAM 简介

> 原文：<https://towardsdatascience.com/aws-iam-introduction-20c1f017c43?source=collection_archive---------22----------------------->

## AWS 身份和访问管理服务概述

![](img/489dc98a0455422891141925bff34ba6.png)

IAM 的关键元素是用户、角色和策略。除了任何相关的背景，我们将在本帖中逐一讨论。用 [Draw.io](https://desk.draw.io/support/solutions/articles/16000042494-usage-terms-for-diagrams-created-in-diagrams-net) 创建

让许多迁移到基于云的环境的人感到震惊(和沮丧)的第一件事是权限可以有多复杂。通常，在多年习惯于成为您之前编写的任何代码的神之后，您会被引入一个几乎所有东西都被默认锁定的环境。

这篇文章将通过教你 IAM 最重要的部分 Amazon Web Services 的身份和访问管理服务——以及介绍一些众所周知的最佳实践来减轻你的痛苦。在以后的文章中，我计划以这个为基础。

> 如果你有兴趣看到我在这篇文章中讨论的所有 IAM 元素，请务必阅读这篇后续文章。

# 帐户和资源

AWS 的世界始于帐户和这些帐户中的资源。IAM 的存在主要是为了保护您帐户中的资源，防止出现以下问题:

*   恶意行为者试图对你的 AWS 帐户做不必要的事情(例如，从你的 S3 桶中窃取你的数据)。
*   您公司内的用户/应用程序意外删除了资源，或者执行了他们原本不应该执行的操作。

## **账户:**

AWS 用户之间的逻辑障碍。非常简单:

*   与 12 位*账户 id* 一一对应，如`123456789012`。
*   作为最佳实践，一个组织将拥有多个 AWS 帐户[1]。IAM 通常用于处理组织内 AWS 帐户之间的权限。
*   与电子邮件地址、信用卡绑定；账户就是你的账单。

## **资源:**

资源是您的帐户中您想要使用的持久对象，如负载平衡器或 EC2 实例。

*   资源总是有一个[ARN](https://docs.aws.amazon.com/general/latest/gr/aws-arns-and-namespaces.html)——亚马逊资源名称——来唯一地标识它们。例如，对于 IAM 用户，ARN 可能如下所示:

```
arn:aws:iam::123456789012:user/Development/product_1234/*
```

注意帐户 id `123456789012`是如何出现在 ARN 中的，以及资源的类型(这个资源是一个 IAM `user`)。

# 身份

IAM 的核心特性是身份，这是一种 AWS 资源。AWS 服务总是公开可以使用某种身份调用的 APIs 这告诉 AWS 你是谁(认证)以及你是否被允许做你正在做的事情(授权)。

IAM 中有两种主要的身份形式:用户和角色。

## 用户

IAM 中用户的意图类似于脸书等其他网站中的用户。当你第一次创建一个 AWS 账户时，你会得到一个[根用户](https://docs.aws.amazon.com/IAM/latest/UserGuide/id_root-user.html)，可以完全访问你的账户【2】。然后，您可以选择——强烈建议——创建更多的用户和角色。

关于用户的一些说明:

*   与角色不同，用户有用户名和密码。这些是*长期有效的*凭证，可以保存很长一段时间，用于登录 AWS 控制台。
*   通常旨在为个人提供对 AWS 控制台或 API 的访问。**然而，作为一个** [**最佳实践**](https://docs.aws.amazon.com/general/latest/gr/aws-access-keys-best-practices.html#use-roles) **，你应该尽可能使用角色而不是用户。**这是为了限制长期凭据被放错位置并让攻击者访问您的 AWS 帐户的风险。
*   用户被给予访问密钥*。*访问密钥可用于通过 CLI 或 SDK 调用 AWS 服务。像用户名/密码一样，访问密钥是长期有效的。它们看起来更加随机，并且可以通过创建一个名为`~/.aws/credentials`的文件与 CLI 一起使用，该文件的内容如下所示:

```
[personal]
aws_access_key_id     = AKIAIOSFODNN7EXAMPLE
aws_secret_access_key = wJalrXUtnFEMI/K7MDENG/bPxRfiCYEXAMPLEKEY
```

您可以通过 CLI 上的`--profile`选项在 CLI 中使用凭据配置文件:

```
> aws s3 ls --profile personal2017-11-30 16:20:55 some-s3-bucket
2017-10-31 20:05:17 some-other-s3-bucket
...
```

## 角色

像用户一样，角色是用于访问 AWS APIs 和资源的身份。然而，角色通常用于向 AWS 帐户授予临时凭证。此外，可以将这些临时凭证配置为信任第三方或其他 AWS 服务。

角色在 AWS 中无处不在:

*   需要访问 AWS 资源的 EC2 实例将使用实例角色根据您的应用程序逻辑来命令其他 AWS 服务/资源。
*   需要访问您帐户中的资源的其他 AWS 帐户有时会*在您的帐户中扮演*角色，以通过 [STS 扮演角色 API](https://docs.aws.amazon.com/STS/latest/APIReference/API_AssumeRole.html) 获得访问权限。为了允许他们这样做，您可以通过信任策略为另一个帐户中的某个身份授予角色权限(稍后将详细介绍)。
*   AWS 的一个重要特性是它能够代表您采取行动。然而，AWS 服务通常被实现为 AWS 帐户；这意味着默认情况下，他们无权访问您帐户中的资源。因此，他们通常会要求您在您的帐户中创建并授予他们访问“服务角色”的权限，以便他们可以代表您采取行动*。*例如，EC2 上的自动伸缩[将需要权限](https://docs.aws.amazon.com/autoscaling/ec2/userguide/autoscaling-service-linked-role.html)来为您上下旋转 EC2 实例。
*   像 JumpCloud 这样的第三方将使用角色来授予在 AWS 之外管理的用户对 AWS 资源的访问权。这个过程称为联合。

# 政策

最后，当您(以某种身份运行)调用 AWS API 时，IAM 将通过评估一个或多个策略来确定调用是否有效。有两种主要的策略:身份策略和资源策略。

## 身份策略

身份策略确定允许给定身份(角色/用户)做什么。身份策略可以由 AWS 管理(这些受管策略将在您的帐户中预先创建)，也可以由您管理。

下面是一个更简单的身份策略示例:

*   可以被调用的 API 在 IAM 中被称为`Actions`。多个 API 可以命中一个动作，但是，通常情况下，动作只对应于一个 API。
*   该策略是白名单；这意味着，默认情况下，不允许*动作。对于两个动作:`CreateRole`和`CreateUser`，给出了明确的`Allow`。有关如何在 AWS 帐户中评估策略逻辑的完整分类，请参见本文档。*
*   AWS 中的大多数 API 调用都可以缩小范围，只允许在特定的资源上调用，如上所述。您可以看出发生了这种情况，因为`Resources`部分不是`*`。这意味着使用该标识的请求只能针对具有这些特定 arn 的资源成功。例如，您只能使用此策略创建 ARN `arn:aws:iam::123456789012:role/some-role`的角色。**将范围缩小到单个资源是防止许多安全漏洞的最佳实践。**以 2019 年的 Capital One hack 为例。Capital One [给了他们的防火墙过多的 S3 权限](https://krebsonsecurity.com/2019/08/what-we-can-learn-from-the-capital-one-hack/)，一旦它被 [SSRF](https://www.hackerone.com/blog-How-To-Server-Side-Request-Forgery-SSRF) 欺骗，就允许攻击者窃取超过 1 亿个人的数据。
*   没有资源策略的身份策略只能在同一个 AWS 帐户中工作。
*   支持通配符。该策略中的第二个声明与 Cloudwatch 日志有关，并给予*对 Cloudwatch 日志的完全访问权*(他们开放的任何 API，以及任何资源)。

任何给定的身份都可以附加多个策略；在这种情况下，身份获得应用于它的策略的逻辑或，其中显式的`Deny`将覆盖显式的`Allow`。

我认为语法很简单，但是，要更深入地了解策略语法，请阅读[文档。](https://docs.aws.amazon.com/IAM/latest/UserGuide/reference_policies.html)

这里还有一个 EMR 策略生成器，我推荐你用它来更快地构建这些:[https://awspolicygen.s3.amazonaws.com/policygen.html](https://awspolicygen.s3.amazonaws.com/policygen.html)

## 资源政策

当某个主体*对某个资源*执行* *操作*时，无论该资源是从其获取对象的 S3 桶，还是某人试图承担的 IAM 角色，其资源策略都将生效。这些乍一看似乎是多余的，因为您可以将身份策略的范围扩大到特定的资源，但通常在您的资源而不是调用它的身份上设置策略是有用的。*

*这些资源策略在支持跨帐户访问资源方面也起着关键作用。单独的身份策略无法实现跨帐户访问，因为 AWS 帐户之间的权限界限比单个帐户内部的权限界限要严格得多。*

*让我们检查一个角色的资源策略。请注意，角色上的资源策略也称为信任策略，因为它们使其他人能够承担角色。*

*   *[**委托人**](https://docs.aws.amazon.com/IAM/latest/UserGuide/reference_policies_elements_principal.html) **是试图访问资源**的调用者的名字。该策略将服务 EC2 以及帐户`123456789012`中的两个其他角色列入白名单，以承担该角色。主体是普通身份的超级集合，可以包括 AWS 服务或来自其他应用程序的用户。*
*   *您会注意到，当您为某个规则指定一个主体时，您需要指定它是哪种主体。委托人有几种类型，但最常见的两种是`Service`和`AWS`。前者允许 AWS 服务(如 EC2)访问该角色，后者允许特定 AWS 帐户的身份访问该角色。*
*   *根据服务的不同，资源策略会有很大的不同。有些资源策略比其他资源策略更严格，或者启用不同的功能。例如，在密钥管理服务中，您必须[在密钥的资源策略中授予您帐户的根用户权限](https://docs.aws.amazon.com/kms/latest/developerguide/key-policies.html#key-policy-default)，然后身份策略才能授予对该密钥的访问权限。S3 桶有额外的安全控制，像[访问控制列表](https://docs.aws.amazon.com/AmazonS3/latest/dev/acl-overview.html)。**始终致力于理解您正在使用的资源如何单独管理权限:不要假设两种类型的资源以相同的方式管理权限**。*

*在大多数资源上，资源策略是可选的。如果它们不存在，它们将被忽略，如下所示。*

# *简化的策略评估逻辑*

*最后，我想提供一个简单的图表来形象化地说明我们讨论的策略是按照什么顺序进行评估的。*

*为了简单起见，我省略了完整的评估逻辑，只包括下面的通用策略。我建议您阅读[公共文档](https://docs.aws.amazon.com/IAM/latest/UserGuide/reference_policies_evaluation-logic.html#policy-eval-denyallow)以获得更详细的策略评估视图，包括更深奥的策略，如会话策略和权限边界。*

*![](img/6670af8fdb9412208189f9b00ae57fff.png)*

*委托人提出请求时评估的典型策略的流程图。这里的允许/拒绝对应于您在上面看到的 JSON 策略文档中指定的允许/拒绝。用 [Draw.io](https://desk.draw.io/support/solutions/articles/16000042494-usage-terms-for-diagrams-created-in-diagrams-net) 创建*

*IAM 是 AWS 的主要构件之一。我希望这已经向您介绍了使用它时会遇到的主要问题。*

*如果你想更深入地了解我，一定要看看[我的后续文章](/how-to-copy-between-encrypted-s3-buckets-cross-account-e4e3096d1a8a)！*

*如果您还有任何问题或者只是想聊天，您可以通过我的 evankozliner@gmail.com 联系我，或者在 [Twitter](https://twitter.com/evankozliner) 上给我发消息。*

# *笔记*

*[1]构建多账户组织超出了本文的范围，但是，如果您感兴趣，我推荐下面的视频和[简介](https://aws.amazon.com/organizations/getting-started/best-practices/)。*

*[2]根用户实际上不同于 IAM 用户。[有些任务只有根用户才能执行](https://docs.aws.amazon.com/general/latest/gr/root-vs-iam.html#aws_tasks-that-require-root)。*