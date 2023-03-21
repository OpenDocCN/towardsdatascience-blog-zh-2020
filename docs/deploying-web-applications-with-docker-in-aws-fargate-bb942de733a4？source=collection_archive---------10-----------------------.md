# 在 AWS Fargate 中部署 Docker 容器

> 原文：<https://towardsdatascience.com/deploying-web-applications-with-docker-in-aws-fargate-bb942de733a4?source=collection_archive---------10----------------------->

## 使用 Fargate 和 Cloud Formation 在 AWS 中部署容器化 web 应用程序的分步指南

![](img/9e2e6f705a70a7db65a8b8cb2720e072.png)

照片由[维达尔·诺德里-马西森](https://unsplash.com/@vidarnm?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)在 [Unsplash](https://unsplash.com/photos/y8TMoCzw87E) 拍摄

# 动机

Docker 是一个非常棒的工具，可以用一种简单且可扩展的方式封装和部署应用程序。事实上，我发现自己经常做的事情是将 Python 库封装到 Docker 映像中，以便以后可以用作项目的样板。

在本文中，我将说明如何在容器注册表中注册 Docker 映像，以及如何使用 Fargate 在 AWS 中部署容器，Fargate 是一个无服务器计算引擎，旨在运行容器化的应用程序。此外，我们将使用云形成以编程方式部署我们的堆栈。我们开始吧！

# 在容器注册表中注册您的图像

容器注册对于 Docker 图像就像代码库对于编码一样。在 registry 中，您可以创建映像存储库来推送和注册您的本地映像，您可以存储同一映像的不同版本，如果其他用户有权访问存储库，他们可以推送和更新映像。

现在，假设您已经在本地开发了一个映像，并希望将其部署到云中。为此，我们需要将本地映像存储在一个容器注册表中，可以从该注册表中提取和部署映像。在本文中，我将使用一个相当简单的图像，在端口 80 上启动一个 web [Python-Dash](https://github.com/vioquedu/docker-dash) 应用程序。

我们将使用 [ECR](https://aws.amazon.com/ecr/) (弹性容器注册表)来注册我们的图像。ECR 是一个 AWS 服务，非常类似于 DockerHub，用于存储 Docker 图像。我们要做的第一件事是在 ECR 中创建一个存储库，我们可以按如下方式使用 AWS CLI:

```
aws ecr create-repository \
--repository-name dash-app\
--image-scanning-configuration scanOnPush=true \     
--region eu-central-1
```

您应该能够在 AWS 管理控制台中看到存储库

![](img/beca5a6d00bef972f9253b173920c57c.png)

要将本地图像推送到我们的 ECR 存储库，我们需要在 AWS 中验证我们的本地 Docker CLI:

```
aws ecr get-login-password --region **region** | docker login --username AWS --password-stdin **acccount_id**.dkr.ecr.**region**.amazonaws.com
```

适当更换`aws_account_id`和`region`即可。您应该会在终端中看到消息`Login Succeeded`，这意味着我们的本地 Docker CLI 已经过身份验证，可以与 ECR 交互。

现在，让我们将本地图像推送到我们全新的存储库中。为此，我们必须标记我们的映像以指向 ECR 存储库:

```
docker tag **image_id** **account_id**.dkr.ecr.eu-central-1.amazonaws.com/dash-app:latest
```

现在我们只需要把它推到集控室:

```
docker push **account_id**.dkr.ecr.eu-central-1.amazonaws.com/dash-app:latest
```

您应该会在 AWS 控制台中看到推送的图像:

![](img/dce19ba5905fdd63b6fee413be782c0b.png)

现在我们到了本节的结尾，让我们总结一下:(I)我们已经在 ECR 中创建了一个名为 dash-app 的映像存储库，(ii)我们已经授权我们的本地 Docker CLI 连接到 AWS，以及(iii)我们已经将一个映像推送到该存储库。在下一节中，我们将介绍如何在 AWS 中部署这个映像。

# 在 AWS 中部署 Docker 容器

撇开 Kubernetes 不谈，AWS 提供了几种部署容器化应用程序的选项:

1.  在 EC2 上部署容器，通常是在一组自动扩展的实例中。在这个场景中，我们负责修补、保护、监控和扩展 EC2 实例。
2.  在 AWS Fargate 上部署容器。因为 Fargate 是无服务器的，所以不需要管理或提供 EC2 实例。Fargate 管理我们的*任务*的执行，提供适当的计算能力(在这个上下文中，任务指的是作为应用程序一起工作的一组容器)。

在本节中，我们将关注第二个选项，说明如何在 AWS Fargate 上部署我们的 web 应用程序。此外，我们将为 AWS 云的形成分配所有必要的资源。

## 云的形成

[云形成](https://docs.aws.amazon.com/cloudformation/index.html)是一种以编程方式提供和部署资源的 AWS 服务，这种技术通常被称为代码或 IaC 基础设施。在 IaC 中，我们不是通过管理控制台手动分配资源，而是在 JSON 或 YAML 文件中定义堆栈。然后，该文件被提交给 Cloud Formation，它会自动部署文件中指定的所有资源。这有两个主要优势:(I)它使自动化资源供应和部署变得容易，以及(ii)这些文件有助于作为我们的云基础架构的文档。

尽管在 JSON/YAML 文件中定义我们的堆栈需要经历一个学习过程，并且忘记 AWS 管理控制台及其真正易于使用的向导，但从长远来看，这绝对是值得的。随着基础设施的增长，将所有的栈保持为代码将对有效地扩展非常有帮助。

## 堆栈

现在，让我们列出运行应用程序所需的资源:

1.  [**任务**](https://docs.aws.amazon.com/AmazonECS/latest/developerguide/task_definitions.html) **:** 它描述了塑造我们的应用程序的一组 Docker 容器。在我们的例子中，我们只有一个图像。
2.  [**ECS 服务**](https://docs.aws.amazon.com/AmazonECS/latest/developerguide/ecs_services.html) **:** 它负责运行和维护所需数量的任务实例。例如，如果任务失败或停止，ECS 服务可以自动启动一个新实例，以保持所需数量的任务处于服务状态。
3.  [**Fargate**](https://aws.amazon.com/fargate/)**:**一个用于容器的无服务器计算引擎。使用 Fargate，无需预订和管理服务器。它会自动提供计算能力来运行我们的任务。
4.  **网络资源**:我们还需要一个 VPC，一个通过路由表连接到互联网网关的公共子网(别忘了我们正在部署一个应该可以从互联网到达的 web 应用程序)，以及一个安全组来安全地运行我们的容器。如果已经有了网络配置，就不需要设置这些资源，尽管我在云形成脚本中包含了它们。

现在，事不宜迟，让我们进入堆栈。下面的要点包含所有需要的资源。我们来详细解释一下:

1.  这是 VPC。启用 DNS 支持和 DNS 主机名以访问 ECR 并提取映像非常重要。
2.  `INT-GATEWAY`:这是一个互联网网关，需要将子网暴露给互联网。
3.  `ATTACH-IG`:连接互联网网关到 VPC。
4.  `ROUTE-TABLE`:这是一个路由表，我们将在其中添加规则，以将子网暴露给互联网网关。
5.  `ROUTE`:这为之前描述的路由表添加了一条规则。它将流量转发到互联网网关。
6.  `SUBNETFARGATE`:这是托管我们服务的子网。我们定义了可用性区域及其所属的 VPC。
7.  `SUBNETROUTE`:将路由表关联到子网。
8.  `SGFARGATE`:这是应用于我们服务的安全组。它允许端口 443 和 80 上的流量通过 HTTPS 和 HTTP 协议。
9.  `FARGATECLUSTER`:它定义了 Fargate 集群来托管我们的服务。
10.  `ECSTASK`:决定要执行的任务。它包括构成任务的 Docker 图像列表。对于每个容器，它注册端口映射、启动命令和日志选项。图像是从之前设置的 ECR 存储库中提取的。
11.  `SERVICE`:定义将启动和维护我们的任务的 ECS 服务。如果您已经有了网络配置，并且不需要创建新的子网和安全组，只需参考 ECS 服务的`NetworkConfiguration`部分中的现有资源。

一旦您的文件准备就绪，将其上传到 Cloud Formation 以创建您的堆栈:

![](img/d240e76453974991194a7669b14e151e.png)

按照管理控制台中的步骤启动堆栈。一旦完成，云形成将自动开始提供服务。您可以在“事件”选项卡中跟踪其进度:

![](img/9896926c6c676e381df93b4c6c3c7e14.png)

更重要的是，准备就绪后，您可以在分配给正在运行的任务的公共 IP 地址访问您的 web 应用程序！

## 关于用户策略的说明

如果您遵循最佳实践，您就不会使用 AWS root 帐户创建资源。相反，您应该使用非根用户。但是，您应该注意，要将角色传递给服务，AWS 要求创建服务的用户拥有“传递角色”权限。在我们的例子中，我们需要我们的用户将角色`ecsTaskExecutionRole`传递给`TaskDefinition`服务，因此我们必须授予用户这样做的权限。这可以通过 IAM 中的 root 帐户或任何具有 IAM 权限的帐户来完成。只需添加下面的策略，并将其附加到将分配所有资源的用户。

```
{
    "Version": "2012-10-17",
    "Statement": [
        {
            "Effect": "Allow",
            "Action": "iam:PassRole",
            "Resource": "arn:aws:iam::account_id:role/ecsTaskExecutionRole"
        }
    ]
}
```

# 结论

我们在这篇文章中已经讨论了很多。我们已经看到了如何创建 ECR 存储库以及如何将 Docker 映像推送到其中。我们还简要介绍了云形成和 IaC。我想重申将您的基础设施和堆栈指定为代码的重要性。随着您的基础设施的增长，在 JSON 或 YAML 文件中定义堆栈将使自动化部署更容易，以高效的方式扩展，并将提供关于您的基础设施的某些文档。最后，我们使用 AWS Fargate 以一种无服务器的方式部署 docker 容器，这让我们免去了供应和管理服务器的负担。

希望这篇文章对您有所帮助，感谢您的阅读。

# 参考

1.  [使用 AWS CLI 开始使用亚马逊 ECR](https://docs.aws.amazon.com/AmazonECR/latest/userguide/getting-started-cli.html)