# 使用 Terraform 在 GCP 自动部署弹性搜索

> 原文：<https://towardsdatascience.com/automate-elasticsearch-deployment-in-gcp-part-1-terraform-3f51b4fcf5e6?source=collection_archive---------12----------------------->

## [GCP 的弹性研究](https://towardsdatascience.com/tagged/elasticsearch-in-gcp)

## 在一个命令中设置您的 Elasticsearch、Kibana 和 Logstash (ELK)环境

![](img/fbcddb259876840c88f15b16a3f1193e.png)

(改编)伊莱恩·卡萨普在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 拍摄的照片

我们都想利用 Elasticsearch 的强大功能，但是插件、配置和错综复杂的文档数量给入门设置了巨大的障碍。

> NB！本指南希望您对 Terraform、Bash 脚本和 Elasticsearch 有一些基本的了解。

在 GCP(谷歌云平台)，目前开始使用 Elasticsearch 只有几个选择:使用 Elastic 中的托管 SaaS 或市场中的某个产品。这两种选择都有缺点，我打算展示我的方法，告诉您如何快速开始并看到一些结果，同时保持对您的部署的完全控制。

稍后，您可以吸收文档中的详细信息，并按照自己的进度深入理解。本文绝不是对 Elasticsearch 集群的完美设置的描述，它更像是一种让您需要的所有工具以一种平稳的方式快速启动和运行的方式，然后由您根据自己的用例修改脚本。我一直在 GCP 工作，但是这些例子应该适用于任何云环境。

Elasticsearch、Kibana、Logstash 和所有相关插件都是开源的，因此唯一的成本是在 GCP 或任何其他云环境中运行的虚拟机和基础设施。

> Terraform 非常适合自动化部署，因为您只需一个命令，就可以在几分钟内拆除和启动虚拟机和基础架构。

# 1.将（行星）地球化（以适合人类居住）

## 1.1.虚拟机

首先，我们需要运行 Elasticsearch 的虚拟机:

这里发生了很多事情。通过迭代上面的代码片段，您可以选择想要部署多少台机器。最有趣的是我称之为`./startup-elastic.sh`的启动脚本。我们将在第 2 节回到这个话题。

我们还需要一个新实例来运行 Kibana:

所以，这是我们所需要的核心，实际的 GCE VM (Google 计算引擎)运行 Elasticsearch 和 Kibana 作为 Hashicorp 配置语言。

您可以在 var 文件中选择任何适合您的设置。此配置将在其自己的子网中部署一个带有 200 GB SSD 驱动器的虚拟机，该虚拟机的服务帐户具有最低要求的权限。没有外部 IP，所以我们必须想出另一种方法来访问虚拟机。

我的 [tfvar](https://www.terraform.io/docs/configuration/variables.html) 文件看起来像这样:

到目前为止一切顺利，但我们需要的不仅仅是虚拟机，对吗？
让我们设置 IAM，并给予虚拟机使用的服务帐户正确的权限。

## 1.2.许可

为了能够在 GCS (Google Cloud Storage)中创建我们的数据备份，服务帐户将需要一些细粒度的权限。

首先，我们使用虚拟机备份到 GCS 所需的权限创建一个自定义角色，并获取服务帐户的密钥，然后我们创建一个服务帐户，并将该角色应用于该帐户。最后，我们为服务帐户生成一个密钥。这个密钥我们将保存在 Elasticsearch 的 Keystore 中，并用它在 GCS 中创建一个备份存储库。

## 1.3.网络

为了安全，我们还需要一些网络基础设施。

我们首先为虚拟机和基础架构创建单独的 VPC(虚拟私有云)和子网。在这里，我们指定您可以给机器的 IP 范围。

由于虚拟机没有外部 IP，您无法访问互联网和下载任何软件。为了解决这个问题，我们需要用路由器打开一个 NAT 网关，以便能够从互联网上下载任何东西。一旦软件安装完毕，我们就可以移除 NAT 网关了。

在 GCP，如果你想通过 Appengine 或云功能访问你的 VPC 网络，你需要一个 VPC 连接器。在我的设置中，我在 GAE 上托管了一个 API，它通过 VPC 连接器调用 Elasticsearch 内部负载平衡器。

超过 1 个节点的 Elasticsearch 集群需要一个负载均衡器来分发请求。要将虚拟机置于负载平衡器之下，我们需要创建实例组。为了实现冗余，我们将虚拟机放在同一区域的不同分区中。如果一个区域出现问题，其他区域不会受到影响。在本例中，我们部署了三台虚拟机，my-elastic-instance-1 和 2 位于区域 d，而 my-elastic-instance-3 位于区域 c。

因为我们还想通过负载均衡器访问 Kibana，所以我们也将为此创建一个实例组。

现在，我们需要的是带有健康检查和转发规则的负载平衡器。

## 1.4.防火墙规则

当然，还有我经常忘记的部分，防火墙规则。允许子网中的节点之间进行内部通信，允许负载平衡器和运行状况检查与虚拟机进行通信。

# 2.Elasticsearch Bash 脚本

在这一节中，我们将看看有趣的东西:当部署机器时，在机器上运行的启动脚本。

该脚本接受几个输入参数:

*   弹性搜索集群主节点的名称
*   集群中所有节点的 IP
*   GCS 备份-存储桶名称(存储备份的位置)
*   存储证书的 GCS 存储桶名称
*   服务帐户密钥
*   Elasticsearch 的密码。

这些桶是手动创建的，但是如果您更喜欢在 Terraform 中创建，那也很好。

在我们查看脚本之前，还有另一个手动步骤——创建[证书](https://www.elastic.co/guide/en/elasticsearch/reference/current/configuring-tls.html#node-certificates)。这可能是自动化的，但我发现手动生成它们并在启动时将它们放入 GCS bucket 并复制到 Elasticsearch 更容易。

证书对于保护集群内的通信和启用 Kibana 中的某些功能非常重要。

我将把脚本分解成更小的块，并逐一解释它们。

## 2.1.启动检查

我们首先检查`credentials.json`文件是否存在，所以我们只运行一次启动脚本，而不是每次重启机器。如果文件存在，我们退出脚本。

## 2.2.安装必备组件

然后，我们下载并安装先决条件。你可以指定你喜欢哪个版本，或者只取最新的 7.x 版本如下。

## 2.3.配置 elasticsearch.yml

现在，我们需要在`elasticsearch.yml`文件中配置 Elasticsearch。在这里，我们设置节点的 IP，并决定哪个是主节点。

如果您还记得第 1 节，启动脚本接受了一些输入变量。
这些是:

## 2.4.配置堆大小

如果你以前有过使用 Elasticsearch 的经验，你就知道 RAM 有多重要。我不会深入讨论这个问题，我只是将堆大小设置为总 RAM 的 50%并且低于 32 GB。

## 2.5.安装 GCS 备份插件

在这一步，我们安装必要的插件，我们需要使用 GCS 作为我们所有弹性搜索索引的备份。这也将服务帐户密钥添加到密钥库中，并重新启动 Elasticsearch。

## 2.6.启用监控

下一个命令在群集设置中启用 X-Pack 监控。

## 2.7.扩展 Elasticsearch.yml 并复制证书

在这里，我们将放在 GCS 桶中的证书复制到 Elasticsearch 中，并且我们还使用一些安全设置扩展了`elasticsearch.yml`文件。最后，我们将密码添加到 Elasticsearch 密钥库中。在这些例子中，我们没有为密钥库设置任何密码。

现在，我们不想像发送到这个脚本中的所有其他参数一样，将我们的密码保存在 tfvar 文件中。如果你一开始就注意到了，我们的 tfvar 文件没有任何 elastic_pw。我们在部署 terraform 代码时添加了它，以使密码远离代码和存储库。

像这样:

## 2.8.注册备份存储库、创建自定义角色等。

最后一步是注册备份存储库并创建一个策略，以便每天将快照放入我们的备份存储桶。我们还创建了一些自定义角色。在这一步之后，您可以从 Elasticsearch 文档中添加您想要的任何内容。

当你安装 Elasticsearch 时，如果你也安装 Kibana(elastic search 的前端)和一些插件来监控你的集群，你的生活会轻松很多。

# 3.基巴纳巴什脚本

在这最后一部分中，我们将浏览 Kibana 启动脚本，并通过解释我们如何通过浏览器安全地访问 Kibana 来将它们联系在一起。

为了简单起见，我选择在与 Kibana 相同的实例中运行所有 beats 插件。

## 3.1.启动检查

正如我们在第 2 节中提到的，我们只想运行这个脚本一次。

## 3.2.安装必备组件

## 3.3.密钥库和密码

就像 Elasticsearch 一样，Kibana 有一个密钥库来安全地存储敏感信息。Keystores 的诀窍是 Kibana 将从`kibana.yml`和`kibana.keystore`读取设置。现在，Kibana 可以连接到 Elasticsearch，而不会以纯文本形式暴露我们的密码。Beats 和 APM 有以类似方式操作的密钥库。

## 3.4.将证书复制到 Kibana

## 3.5.追加 Kibana 配置文件

## 3.6.开始基巴纳

要向 Elasticsearch 发送数据，我们需要 Beats。它们充当 Elasticsearch 和 Logstash 的数据传送器。我将避免描述细节，因为它最好在[文档](https://www.elastic.co/guide/en/beats/libbeat/current/beats-reference.html)中找到。

## 3.7.Filebeat

## 3.8.开始 Filebeat

## 3.9.公制节拍

## 3.10.心跳

## 3.11.高级电源管理

## 3.12.Logstash

皮尤，配置真多！

表演时间到了，让我们开始吧！

现在，我们的存储库中已经有了所有的配置，并且已经使用 Terraform 部署了 Elasticsearch，而没有暴露密码或证书。此外，您可以根据需要轻松添加更多插件和配置。

请注意，一切都部署在自己的 VPC 网络中，没有外部 IP 地址。那太好了，但是我们如何通过浏览器安全地访问 Kibana web 服务器呢？

## 3.13.访问基巴纳

这部分我们将在谷歌控制台手动完成，但也可以用 Terraform 自动完成。

我们保留一个静态 IP 地址，创建一个 HTTPS 负载平衡器。我们将后端配置设置为指向我们的 Kibana 实例组，而前端配置设置为我们的外部静态 IP 地址和端口 443。然后，我们创建一个证书，并将其分配给负载平衡器。设置负载平衡器时，可以在控制台中直接创建证书。

所以现在可以通过你预定的 IP 访问 Kibana 了。但是其他人也可以…这就引出了下一点:

**输入 IAP (** [**身份感知代理**](https://cloud.google.com/iap/docs) **)**

IAP 允许我们通过设置 Oauth 客户端并授予用户 *IAP 安全的 Web 应用用户*权限来管理谁可以访问我们的负载平衡器。

现在，在 GCP 项目中，只有拥有此权限的用户才能访问您的 IP 地址。最重要的是，他们必须输入我们发送到 Kibana 的密码。

太棒了，现在我们可以在几分钟内用一个命令搭建一个全新的环境来运行 Elasticsearch、Kibana 和 Logstash！

在一个指挥部里部署一头麋鹿是什么感觉

感谢安德斯·阿克伯格的知识和支持。