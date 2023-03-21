# 在自动气象站上为数据科学建立 Kubernetes 的实际问题

> 原文：<https://towardsdatascience.com/practical-issues-setting-up-kubernetes-for-data-science-on-aws-6e009f045f65?source=collection_archive---------51----------------------->

## Kubernetes 提供了大量有用的原语来建立自己的基础设施。然而，供应 Kubernetes 的标准方式并不适合数据科学工作流。这篇文章描述了这些问题，以及我们是如何看待它们的。

![](img/66e7c8f8e73df66f970235a9107218f9.png)

艾米·埃尔廷在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄的照片

声明:我是[土星云](https://www.saturncloud.io/s/)的首席技术官——我们构建了一个企业数据科学平台，专注于利用 Dask 和 RAPIDS 获得巨大的性能收益。

# 多个 AZ(可用性区域)与 EBS(弹性块存储)和 Jupyter 交互不佳

不管你喜不喜欢，Jupyter 是当今最常见的数据科学工具 IDE。如果您为数据科学家提供 Kubernetes 集群，他们肯定会希望运行 Jupyter 笔记本电脑。如果他们运行的是 Jupyter 笔记本，他们将需要持久存储——在 IDE 中工作，无论何时关机，文件系统都会被删除，这是一种非常糟糕的体验！。这与传统的 Kubernetes 工作负载非常不同，后者通常是无状态的。

这也意味着，如果您支持多个 AZ，您需要确保 Jupyter 实例总是在同一个 AZ 中启动，因为您的持久性存储(EBS)不能自动迁移到其他区域。一种选择是使用 EFS，而不是 EBS。我们在土星没有这样做，因为大多数数据科学家也想在他们的驱动器上存储数据，一旦你开始进入大规模并行集群计算，EFS 是不可靠的。在 Saturn，我们将所有 Jupyter 实例路由到同一个 AZ，并设置特定于该 AZ 的 ASG(自动缩放组)。您牺牲了高可用性，但无论如何，您从未真正拥有过高可用性，除非您将快照备份到多个 AZ，并准备在主服务器宕机时将 EBS 快照恢复到另一个 AZ。

# 标准 VPC 遇到 EIP(弹性 IP)限制

标准的 Kubernetes 设置将工作人员保持在私有子网中。大多数人最终会使用 CIDR 块创建私有子网，分配大约 256 个 IP 地址。这似乎是合理的，因为您可能不需要超过 256 台机器，对吗？不对！通过 EKS，实例类型决定了每个实例分配的默认(和最大)数量的 [IP 地址。默认情况下，一些较大的实例(例如 512 GB ram 的 r5.16xlarge)消耗 50 个 IP 地址。这意味着一个由 5 名数据科学家组成的团队，每个人使用 1 个 r5.16xlarge，可以轻松耗尽您的私有子网中的 IP 地址数量。我们建议分配非常大的子网(我们的默认配置在 CIDR 块中有 8000 个可用子网)](https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/using-eni.html#AvailableIpPerENI)

# Calico 的标准安全网络配置随着节点的扩展而变得不稳定。

许多数据科学工具没有提供太多内置安全性。这对于 OSS 项目来说是完全合理的，但是对于企业部署来说却是一个大问题。例如，默认情况下，Dask 集群不保护调度程序，这意味着任何可以访问集群的人都可以向任何 Dask 集群提交作业(Dask 有其他方法来减轻这种情况，但是对我们来说，在网络级别处理这种情况是最灵活的)。即使您正在保护对 Kubernetes 集群的访问，通过使用 Kubernetes 网络策略来控制锁定资源来保护数据科学工具也是一个好主意。

EKS 目前的安全网络标准是 Calico。标准 Calico 设置将根据节点数量水平自动扩展元数据存储，这对于大多数数据科学工作负载来说过于激进，因为我们通常每台机器运行 1 个用户单元(稍后将详细介绍)。这种水平自动缩放行为导致 Calico 不稳定，从而导致节点无法通信。我们通过手动控制元数据存储中副本的数量来解决这一问题，这对于我们的数据科学工作负载来说非常稳定。

# ASG(自动缩放组)和集群自动缩放器

数据科学工作负载与普通的 Kubernetes 工作负载非常不同，因为它们几乎总是受内存限制。EC2 定价主要由内存决定，数据科学工作负载的多租户不会带来太多价值。数据科学工作负载通常是长期运行的(许多数据科学家会提供一个 jupyter 实例，并让它运行一整个月)。您最好为每个 data science pod 分配一个完整的节点。如果您不这样做，很可能最终会在一个每月花费 3000 美元的节点上安排一个每月只花费 30 美元的 pod。
在 Saturn，我们创建了许多 ASG(4GB/16GB/64GB/128 GB/256 GB/512 GB ),以尽可能少地浪费内存。

数据科学工作负载也非常高。有一天，一名数据科学家可能需要在一个 4GB 的实例上编辑代码，每月花费 30 美元。第二天，他们可能想要一个 512 GB 的 jupyter 实例(3000 美元/月)，该实例连接到一个 10 节点 Dask 集群(总共 5TB /ram)。能够自动缩放 Kubernetes 非常重要。

# Kubernetes 星团-自动定标器

Kubernetes cluster-autoscaler 是一个复杂的天才，它可以计算出最佳的旋转节点以容纳新的 pod，以及应该调整哪些 pod 以释放可以旋转的节点。然而，如果您已经接受了每个实例一个 pod 的配置，这种复杂性会增加很多复杂性，几乎总是会导致问题。

## 问题— ASG 选择

Kubernetes 集群自动缩放器配置有扩展器，扩展器决定如何分配新的 EC2 实例。成本意识用户的标准是`least-waste`，然而`least-waste`扩展器以百分比计算浪费。这意味着，如果您创建了一个 pod，但忘记指定任何 CPU 或 RAM 请求(您请求 0)，集群自动缩放器将确定每种节点类型都会导致 100%的浪费。当一个扩展器判断它有一个平局时，一切都被路由到`random`扩展器。恭喜你！您每月 30 美元的 pod 现在每月花费 3000 美元。

## 问题—浪费的容量

Kubernetes pods 是基于在退回到集群自动缩放之前当前有哪些节点的容量来调度的。一旦 pod 腾出一个节点，10 分钟后该节点就会被拆除。对于活动集群，我经常看到小型 pods (4GB)被意外地安排在大型节点(512 GB)上。数据科学家认为他们正在消耗 4GB 的内存，所以他们很乐意让这个 pod 永远运行下去。恭喜你！您每月 30 美元的 pod 现在每月花费 3000 美元。

## 解决方案——具体到助理秘书长。

数据科学工作负载几乎总是受内存限制，应该独占整个实例。数据科学工作负载也非常高，因此 Kubernetes 中的自动伸缩非常重要。在 Saturn，我们通过明确标记每个 pod 的节点选择器来解决这个问题，这样它只能被安排在正确的 ASG 上。

# 实例配置

本节假设您正在使用 terraform 配置 EKS。

用户应该能够建立 docker 图像吗？如果你想运行 [binderhub](https://binderhub.readthedocs.io/en/latest/) ，这可能是必要的。在 Saturn，我们允许某些 pod 构建 docker 映像，因为我们希望从标准数据科学配置文件中启用我们的 docker 映像构建服务。如果是这样的话，您将需要使用以下内容来配置您的助理秘书长

```
bootstrap_extra_args = "--enable-docker-bridge true"
```

# 但不是针对 GPU 机器！

docker 桥不能在 GPU 节点上工作，所以您应该设置

```
bootstrap_extra_args = "--enable-docker-bridge false"
```

`terraform-aws-eks`默认 ASG 使用 CPU AMI。如果您正在配置 GPU 节点，您必须找到您所在地区的 GPU ami ID，并将其传递给 terraform。下面是我们用来做这件事的 Python 代码

```
def get_gpu_ami(region):
    eks_owner = "602401143452"
    client = boto3.client("ec2", region_name=region_name)
    images = client.describe_images(Owners=eks_owner)["Images"]
    ami_prefix = f"amazon-eks-gpu-node-{settings.kubernetes_version}"
    images = [x for x in images if x["Name"].startswith(ami_prefix)]
    images = sorted(images, key=lambda x: x["CreationDate"])
    image = images[-1]
    return image["ImageId"]
```

如果您配置了错误的 AMI，您的节点将会正常运行，但是您昂贵的 GPU 将不会在 Kubernetes pods 中可用。

# 不要忘记集群-自动缩放。

你需要标记 ASG 来表明有多少 GPU 可用，否则 autoscaler 不会知道你需要 4 个 V100s 的深度学习 pod 可以安排在这个盒子上。

```
{
          key = "k8s.io/cluster-autoscaler/node-template/resources/nvidia.com/gpu"
          value = "4"
          propagate_at_launch = true
        }
```

# 现在怎么办？

如果您已经读到这里，那么您一定在考虑为您的数据科学团队建立自己的 Kubernetes 集群。我只有一个问题要问你。

这真的是对你时间的最好利用吗？

您的公司可能会在 Kubernetes 部署中偷工减料，设置一堆 ASG，部署 jupyterhub，然后找出如何部署 dask 集群，然后确保将您需要的依赖项构建到 docker 映像中，以便 jupyter 和 dask 都能很好地工作，然后确保您正确配置 EBS 卷，以便数据科学家不会丢失他们的工作，并确保您配置 Calico、nginx-ingress 和负载平衡器，以便您的应用程序流量是安全的。您是否计划部署仪表板？还是模特？那里有什么敏感信息吗？您是否需要在它们周围提供访问控制和安全性？不要忘记设置 jupyter-server-proxy，这样在 Jupyter/Lab 工作的数据科学家就需要使用 dask dashboard，并使用 bokeh/plotly/voila 进行实验，对吗？你记得为每个 GPU 绑定一个 dask worker 吗？您正在配置 RMM 吗？您是否配置了适当的 GPU 到主机内存溢出？

这真的是对你时间的最佳利用吗？

[我们的定价](https://www.saturncloud.io/s/pricing/)是按需付费，通过 AWS marketplace 计费。我建议租用我们的软件，而不是自己开发。