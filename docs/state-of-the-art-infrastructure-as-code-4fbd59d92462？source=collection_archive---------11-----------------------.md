# 最先进的基础设施代码

> 原文：<https://towardsdatascience.com/state-of-the-art-infrastructure-as-code-4fbd59d92462?source=collection_archive---------11----------------------->

![](img/f2f7fbb87ae11f483d45fbdab68fab82.png)

萨阿德·萨利姆在 [Unsplash](https://unsplash.com/s/photos/infrastructure?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) 上拍摄的照片

## Gruntwork 的最新抽象层将使您的生活更加轻松

# 开始

HashiCorp 在几年前推出 Terraform 时，已经彻底改变了代码基础设施。从那时起，Terraform 已经取代了特定于供应商的基础设施，成为像 AWS 的 CloudFormation 这样的代码解决方案。Terraform 抽象了云形成的复杂性，还提供了一个公共基础设施作为代码平台，以迁移到多云环境。一开始，Terraform 的规模变得非常复杂，维护基础架构的模块化方法并不容易，尤其是当您有许多不同的环境具有相似的基础架构时。

Gruntwork 的 Terragrunt 是 Terraform 的一个包装器，致力于解决您的 Terraform 状态管理和配置问题。它还解决了在不同环境中部署类似基础设施的一些问题。

 [## 用例

### Terragrunt 是 Terraform 的一个瘦包装器，它为使用多个 Terraform 模块提供了额外的工具。

davidbegin.github.io](https://davidbegin.github.io/terragrunt/use_cases/) 

古老的原则“不要重复自己”是从良好的软件开发实践中借鉴来的，在良好的软件开发实践中，你创建一个你必须重复做的事情的函数或方法。等同于地形的是一个模块。模块是一段可重复的、独立的代码，可用于部署基础设施。给 [**这个**](https://medium.com/r?url=https%3A%2F%2Fblog.gruntwork.io%2Fterragrunt-how-to-keep-your-terraform-code-dry-and-maintainable-f61ae06959d8) 一个念。随着 Terraform 被各种规模和背景的工程团队广泛采用，Terragrunt 是一个很好的升级。

来自作者本人，Yevgeniy brik man——terra grunt 的联合创始人

# [现在……](https://linktr.ee/kovid)

虽然 Terragrunt 给了我们一些真正重要的功能，并且肯定是对 Terraform 的升级，但 Gruntwork 今天[宣布了另一个升级。它只是另一个抽象层，可以防止你在设计 Terraform 模块或编写 Terraform 代码时做出错误的设计决策。](https://blog.gruntwork.io/introducing-the-gruntwork-module-service-and-architecture-catalogs-eb3a21b99f70)

这种抽象将代码库的基础结构分为三个部分

*   **模块目录** —通过组合数百个可重复使用、久经考验的模块来部署基础架构。模块可用于部署数据库或 EC2 服务器。例如，EC2 服务器的模块也可能有 IAM、安全组、DNS 记录设置。
*   **服务目录** —这是两个新抽象层中的第一层。它在底层组合了许多模块，并为您提供了部署服务的选项。服务将包括地形代码、配置管理代码、日志记录&警报、安全性、自动化测试以及应用程序在生产环境中运行所需的任何其他内容。
*   **架构目录** —这只是简单地部署了完整的堆栈，一切都已接入。类似于 AWS 的着陆区，这将建立安全基线。除此之外，它还将具备完整的网络、容器编排、存储、隐私、CI/CD、监控、警报、合规性，以及生产环境中运行的应用程序所具备的几乎一切。

Gruntwork 再次为现有的一系列产品增添了一个伟大的功能，使 SRE、DevOps 和 DataOps 的人们的生活变得轻松。唯一的问题(也是最大的问题)是，你必须有订阅计划才能访问代码。对于投资 IaC 的团队来说，订阅可能是值得的。此外，所有这些代码都完全独立于 Terragrunt，不会将您束缚在其中。它可以与 Terragrunt、Terraform Enterprise 和 Terraform Cloud 配合使用。你可以在这里查看订阅计划

[](https://gruntwork.io/pricing/?ref=service-catalog-announcement) [## 成为 Gruntwork 的订户

### 为您的团队配置合适的计划。

gruntwork.io](https://gruntwork.io/pricing/?ref=service-catalog-announcement)