# 连接到 AWS 上私有子网中的 ec2 实例

> 原文：<https://towardsdatascience.com/connecting-to-an-ec2-instance-in-a-private-subnet-on-aws-38a3b86f58fb?source=collection_archive---------3----------------------->

## 这篇文章是关于如何连接到 AWS 上私有子网中的实例的“指南”。它包括一个在 Terraform 中所需的基础设施的例子，并对您的 SSH 配置文件进行修改。

![](img/511f7ed41fd7d61a267fd4b866620639.png)

弗洛里安·范·杜恩在 [Unsplash](https://unsplash.com/s/photos/tunnel?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) 上拍摄的照片

如果你想跳过，这篇文章中讨论的所有代码都可以在我的 GitHub [这里](https://github.com/HDaniels1991/AWS-Bastion-Host)找到。

亚马逊虚拟私有云(亚马逊 VPC)使您能够将 AWS 资源启动到您定义的虚拟网络中。子网是 VPC 内的一系列 IP 地址。子网可以是带有 internet 网关的公共子网，也可以是私有子网。在公共子网中启动的实例可以向互联网发送出站流量，而在私有子网中启动的实例只能通过公共子网中的网络地址转换(NAT)网关来这样做。自然，私有子网更安全，因为管理端口不会暴露在互联网上。通常，在模块化 web 应用程序中，前端 web 服务器将位于公共子网内，而后端数据库位于私有子网内。

连接到私有子网中的实例是必要的，原因有很多:

*   应用程序的后端数据库位于专用子网内，工程师需要访问该数据库来执行专门的分析。
*   私有子网被列入另一个第三方服务的白名单，并且需要与该服务进行交互。

## 连接到专用子网

同一个 VPC 中的实例可以通过它们的私有 IP 地址相互连接，因此可以从公共子网中的实例连接到私有子网中的实例；也称为堡垒主机。

Amazon 实例使用 SSH 密钥进行认证。因此，连接到私有实例将需要 bastion 主机上的私钥；同样，连接到 public 实例需要您的主机上有一个私钥，但是这是非常糟糕的做法。不要把你的私人密钥暴露给堡垒主机！

另一种解决方案是使用 SSH 代理转发，它允许用户从堡垒连接到另一个实例，而无需在堡垒上存储私钥。SSH 代理是一个跟踪用户身份密钥及其密码短语的程序，可以使用以下命令进行配置:

```
# Generate SSH keys
ssh-keygen -k mykeypair# Add the keys to your keychain
ssh-add -K mykeypair
```

一旦生成了密钥并将其添加到 keychain 中，就可以使用-A 选项通过 SSH 连接到 bastion 实例。这将启用转发，并让本地代理在从您的堡垒连接到实例时响应公钥质询。

```
# Connect to the bastion host:
ssh -A <bastion-ip-address>
```

下一步是使用 Terraform 部署所需的基础设施。

## 基础设施

以下基础设施已使用 Terraform 部署；作为代码软件的开源基础设施(也是自切片面包以来最好的东西！).

![](img/59d7778937564dc4cf05cb82eac0b916.png)

使用[https://creately.com](https://creately.com/)创建的图表

**VPC:** 亚马逊 VPC(虚拟私有云)是 AWS 云的一个独立部分，您可以在其中配置基础设施。您的组织很可能已经有了 VPC，如果是这种情况，您可以跳过这一步。

**子网:**子网本质上是 VPC 中可用地址的子集，为您环境中的资源增加了一层额外的控制和安全性。如上所述，您的组织可能已经设置了子网，但如果没有，请参见下文。

私有子网和公共子网之间的主要区别是`map_public_ip_on_launch`标志，如果这是真的，那么在这个子网中启动的实例将有一个公共 IP 地址，并且可以通过互联网网关访问。

**辅助 CIDR:** 如果您的组织在其 VPC 中的所有 IP 地址都被私有子网占用，一种解决方法是创建一个辅助 CIDR 块，并在其中启动一个公共子网。

**互联网网关:**要使子网能够访问互联网，需要 AWS 互联网网关。互联网网关允许互联网流量进出您的 VPC。

**路由表:**路由表指定哪些外部 IP 地址可以从子网或互联网网关联系到。

**Nat 网关:**Nat 网关使私有子网中的实例能够连接到互联网。Nat 网关必须部署在具有弹性 IP 的公共子网中。创建资源后，与专用子网相关联的路由表需要将互联网流量指向 NAT 网关。

**安全组:**安全组充当您的实例的虚拟防火墙，控制传入和传出流量。下面的安全组启用端口 22 (SSH)上的所有流量。私有和公共子网中的两个实例都需要该安全组。

**Ec2 实例和键:**在定义了所有必要的基础设施之后，我们可以设置我们的 Ec2 实例了。这些实例需要一个 AWS 密钥对来验证访问，该密钥对是使用 aws_key_pair 资源和前面创建的现有 ssh 密钥创建的。

现在基础架构已经完成，下一步是部署。这可以通过 Terraform 目录中的以下 terraform 命令来实现:

```
terraform initterraform apply
```

如果部署成功，您将能够在 AWS 控制台中看到两个新的 EC-2 实例。

![](img/ef851984980c702976625a6e58993091.png)

**SSH 配置文件**

SSH 配置文件是存储您所连接的远程机器的所有配置的一个很好的资源。它位于您的主目录下:`.ssh/config`。配置文件不是自动创建的，所以如果它不存在，您必须创建它。

```
Host bastion-instance
   HostName <Bastion Public IP>
   User ubuntuHost private-instance
   HostName <Private IP>
   User ubuntu
   ProxyCommand ssh -q -W %h:%p bastion-instance
```

private-instance 中的`ProxyCommand`告诉 SSH 建立到 bastion-instance 的连接，然后建立到 private-instance 的 TCP 转发。

最后，您可以使用下面的命令通过 SSH 连接到私有实例:

```
ssh private-instance
```

瞧，你成功了！

我希望你喜欢这篇文章；如果你有任何问题或建议，甚至是对未来帖子的想法，请在评论区告诉我，我会尽最大努力回复你。

请查看我的其他帖子:

*   [https://towards data science . com/Apache-air flow-automating-the-collection-of-daily-email-attachments-213 BC 7128 d3a](/apache-airflow-automating-the-collection-of-daily-email-attachments-213bc7128d3a)
*   [https://towards data science . com/selenium-on-air flow-automate-a-daily-online-task-60 AFC 05 afaae](/selenium-on-airflow-automate-a-daily-online-task-60afc05afaae)