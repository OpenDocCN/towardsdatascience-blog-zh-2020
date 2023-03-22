# 你不用再用 Docker 了

> 原文：<https://towardsdatascience.com/its-time-to-say-goodbye-to-docker-5cfec8eff833?source=collection_archive---------0----------------------->

## [意见](https://towardsdatascience.com/tagged/opinion)

## Docker 不是唯一的集装箱工具，可能会有更好的替代工具…

在古代的集装箱时代(真的更像 4 年前)*码头工人*是集装箱游戏中唯一的玩家。但现在情况不同了，Docker 不是唯一的 T4，而是另一个容器引擎。Docker 允许我们构建、运行、拉取、推送或检查容器图像，但对于这些任务中的每一项，都有其他替代工具，它们可能比 Docker 做得更好。所以，让我们探索一下前景，并且(仅仅是*也许是*)卸载并完全忘记 Docker

![](img/0053a9212f47b73bff7935dde3073e7c.png)

照片由[妮可陈](https://unsplash.com/@917sunny?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)在 [Unsplash](https://unsplash.com/?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) 上拍摄

# 为什么不使用 Docker 呢？

如果你已经是 docker 用户很长时间了，我认为你需要一些说服来考虑转换到不同的工具。所以，现在开始:

首先，Docker 是一个单体工具。它是一个尝试做所有事情的工具，这通常不是最好的方法。大多数情况下，最好选择一个专门的工具，它只能做一件事，而且做得很好。

如果您害怕切换到不同的工具集，因为您必须学习使用不同的 CLI、不同的 API 或不同的概念，那么这不是问题。选择本文中显示的任何工具都可以是完全无缝的，因为它们都(包括 Docker)遵循 OCI 下的相同规范，这是[开放容器倡议](https://opencontainers.org/)的缩写。该计划包含[容器运行时](https://github.com/opencontainers/runtime-spec)、[容器分发](https://github.com/opencontainers/distribution-spec)和[容器映像](https://github.com/opencontainers/image-spec)的规范，涵盖了使用容器所需的所有特性。

有了 OCI，您可以选择一套最适合您需求的工具，同时您还可以享受使用与 Docker 相同的 API 和 CLI 命令的乐趣。

所以，如果你愿意尝试新的工具，那么让我们比较一下 Docker 和它的竞争对手的优缺点和特性，看看是否真的有必要考虑放弃 Docker 而使用一些新的闪亮的工具。

# 集装箱发动机

当将 Docker 与其他工具进行比较时，我们需要按组件对其进行分解，首先我们应该谈论的是*容器引擎*。Container engine 是一个工具，它提供了处理图像和容器的用户界面，这样您就不必去弄乱诸如`SECCOMP`规则或 SELinux 策略之类的东西。它的工作也是从远程存储库中提取图像，并将它们扩展到您的磁盘上。它看似运行容器，但实际上它的工作是创建容器清单和带有图像层的目录。然后它将它们传递给*容器运行时*，比如`runc`或`crun`(我们稍后会谈到)。

有许多容器引擎可用，但 Docker 最突出的竞争对手是由红帽开发的。与 Docker 不同，Podman 不需要守护进程来运行，也不需要 root 权限，这是 Docker 长期关注的问题。根据名称，Podman 不仅可以运行容器，还可以运行*pod*。如果您不熟悉 pod 的概念，那么 pod 是 Kubernetes 的最小计算单元。它由一个或多个容器组成——主容器和所谓的*侧容器*——执行支持任务。这使得 Podman 用户以后更容易将他们的工作负载迁移到 Kubernetes。因此，作为一个简单的演示，这就是如何在一个单元中运行两个容器:

最后，Podman 提供了与 Docker 完全相同的 CLI 命令，因此您可以只做`alias docker=podman`并假装什么都没有改变。

除了 Docker 和 Podman，还有其他的容器引擎，但是我认为它们都是没有前途的技术，或者不适合本地开发和使用。但是为了有一个完整的画面，让我们至少提一下那里有什么:

*   [LXD](https://linuxcontainers.org/lxd/introduction/)——LXD 是 LXC (Linux 容器)的容器管理器(守护程序)。这个工具提供了运行*系统*容器的能力，这些容器提供了更类似于虚拟机的容器环境。它位于非常狭窄的空间，没有很多用户，所以除非你有非常具体的用例，那么你可能最好使用 Docker 或 Podman。
*   CRI-O —当你在谷歌上搜索什么是 CRI-O 时，你可能会发现它被描述为容器引擎。不过，它确实是容器运行时。除了它实际上不是一台发动机之外，它也不适合*的“正常”使用。我的意思是，它是专门为用作 Kubernetes 运行时(CRI)而构建的，而不是供最终用户使用的。*
*   *[rkt](https://coreos.com/rkt/) — rkt ( *“火箭”*)是由 *CoreOS* 研发的集装箱发动机。这里提到这个项目实际上只是为了完整性，因为项目结束了，它的开发也停止了——因此它不应该被使用。*

# *建筑图像*

*对于容器引擎，除了 Docker，真的只有一种选择。当涉及到构建图像时，我们有更多的选择。*

*首先，我来介绍一下 [Buildah](https://buildah.io/) 。Buildah 是由 Red Hat 开发的另一个工具，它与 Podman 配合得非常好。如果您已经安装了 Podman，您甚至可能已经注意到了`podman build`子命令，它实际上只是 Buildah 的伪装，因为它的二进制文件包含在 Podman 中。*

*至于它的功能，它遵循与 Podman 相同的路线——它是无后台和无根的，并生成 OCI 兼容的图像，因此它保证您的图像将与用 Docker 构建的图像以相同的方式运行。它还可以从`Dockerfile`或者(更恰当的名称)`Containerfile`构建图像，这是不同名称的同一事物。除此之外，Buildah 还提供了对图像层的更好的控制，允许您将许多更改提交到单个层中。与 Docker 一个意想不到但(在我看来)很好的区别是，Buildah 构建的图像是特定于用户的，因此您将只能列出您自己构建的图像。*

*现在，考虑到 Buildah 已经包含在 Podman CLI 中，您可能会问为什么还要使用单独的`buildah` CLI？嗯，`buildah` CLI 是`podman build`中包含的命令的超集，所以你可能不需要接触`buildah` CLI，但是通过使用它，你可能还会发现一些额外的有用特性(关于`podman build`和`buildah`之间的具体区别，请参见下面的[文章](https://podman.io/blogs/2018/10/31/podman-buildah-relationship.html))。*

*说到这里，让我们来看一个小演示:*

*从上面的脚本中可以看到，您可以简单地使用`buildah bud`构建映像，其中`bud`代表*使用 Dockerfile* 构建，但是您也可以使用更多的脚本化方法，使用 Buildahs `from`、`run`和`copy`，它们是 Dockerfile 中的命令(`FROM image`、`RUN ...`、`COPY ...`)的等价命令。*

*接下来是谷歌的 [Kaniko](https://github.com/GoogleContainerTools/kaniko) 。Kaniko 也从 Dockerfile 构建容器映像，与 Buildah 类似，它也不需要守护进程。与 Buildah 的主要区别是 Kaniko 更关注 Kubernetes 中的建筑图像。*

*Kaniko 应该作为一个映像运行，使用`gcr.io/kaniko-project/executor`，这对于 Kubernetes 来说是有意义的，但是对于本地构建来说不太方便，而且有点违背了初衷，因为您需要使用 Docker 来运行 Kaniko image 来构建您的映像。也就是说，如果您正在寻找在 Kubernetes 集群中构建映像的工具(例如在 CI/CD 管道中)，那么 Kaniko 可能是一个不错的选择，因为它是无后台的，而且([也许](https://github.com/GoogleContainerTools/kaniko#security))更加安全。*

*从我的个人经验来看，我使用 Kaniko 和 Buildah 在 Kubernetes/OpenShift 集群中构建图像，我认为两者都能很好地完成工作，但是在 Kaniko 中，当将图像推送到注册表时，我看到了一些随机构建崩溃和失败。*

*这里的第三个竞争者是 [buildkit](https://github.com/moby/buildkit) ，也可以称为*下一代* `docker build`。它是*莫比*项目的一部分(Docker 也是如此),可以使用`DOCKER_BUILDKIT=1 docker build ...`通过 Docker 作为实验特性来启用。好吧，但这到底能给你带来什么？它引入了许多改进和很酷的特性，包括并行构建步骤、跳过未使用的阶段、更好的增量构建和无根构建。然而另一方面，它仍然需要守护进程来运行(`buildkitd`)。所以，如果你不想摆脱 Docker，而是想要一些新的特性和不错的改进，那么使用 buildkit 可能是一个不错的选择。*

*与上一节一样，这里我们也有一些*“荣誉奖”*，它们填补了一些非常具体的用例，但不是我的首选:*

*   *[Source-To-Image (S2I)](https://github.com/openshift/source-to-image) 是一个不用 Dockerfile 直接从源代码构建图像的工具包。这个工具对于简单的、预期的场景和工作流工作得很好，但是如果你需要太多的定制或者如果你的项目没有预期的布局，它很快就会变得令人讨厌和笨拙。如果您对 Docker 还不是很有信心，或者如果您在 OpenShift 集群上构建您的映像，您可以考虑使用 S2I，因为使用 S2I 构建是一个内置特性。*
*   *[Jib](https://github.com/GoogleContainerTools/jib) 是谷歌的另一个工具，专门用于构建 Java 映像。它包括 [Maven](https://github.com/GoogleContainerTools/jib/tree/master/jib-maven-plugin#quickstart) 和 [Gradle](https://github.com/GoogleContainerTools/jib/tree/master/jib-gradle-plugin#quickstart) 插件，可以让你轻松构建图像，而不需要弄乱 docker 文件。*
*   *最后但同样重要的是 [Bazel](https://github.com/bazelbuild/bazel) ，这是谷歌的另一个工具。这不仅仅是为了构建容器映像，而是一个完整的构建系统。如果你*只是*想要建立一个形象，那么深入 Bazel 可能有点矫枉过正，但绝对是一个很好的学习经历，所以如果你准备好了，那么 [rules_docker](https://github.com/bazelbuild/rules_docker) 部分对你来说是一个很好的起点。*

# *容器运行时*

*最后一大难题是*容器运行时*，它负责运行容器。容器运行时是整个容器生命周期/堆栈的一部分，除非您对速度、安全性等有非常具体的要求，否则您很可能不会去弄乱它。所以，如果你已经厌倦了我，那么你可能想跳过这一节。另一方面，如果你只是想知道有哪些选择，那就这样:*

*runc 是基于 OCI 容器运行时规范创建的最流行的容器运行时。它被 Docker(通过 *containerd* )、Podman 和 CRI-O 使用，所以除了 LXD(使用 LXC)之外，它几乎什么都用。我没有什么可以补充的了。它是(几乎)所有东西的默认设置，所以即使你在读完这篇文章后放弃了 Docker，你也很可能仍然会使用 runc。*

*runc 的一个替代物被类似地(并且令人困惑地)命名为 [crun](https://github.com/containers/crun) 。这是 Red Hat 开发的工具，完全用 C 编写(runc 用 Go 编写)。这使得它比 runc 更快，内存效率更高。考虑到它也是 OCI 兼容的运行时，如果你想自己检查的话，你应该可以很容易地切换到它。尽管它现在不是很受欢迎，但它将作为 RHEL 8.3 版本的替代 OCI 运行时出现在技术预览版中，并考虑到它是我们最终可能会看到的 Podman 或 CRI-O 的默认红帽产品*

*说到 CRI-O，我之前说过 CRI-O 不是真正的容器引擎，而是容器运行时。这是因为 CRI-O 不包括像推送图像这样的功能，而这正是你对容器引擎的期望。CRI-O 作为一个运行时在内部使用 runc 来运行容器。这个运行时不是您应该尝试在您的机器上使用的运行时，因为它被构建为在 Kubernetes 节点上用作运行时，您可以看到它被描述为*“Kubernetes 需要的所有运行时，仅此而已”*。因此，除非您正在设置 Kubernetes 集群(或者 OpenShift 集群——CRI-O 已经是默认的),否则您可能不应该接触这个集群。*

*这部分的最后一个是 [containerd](https://containerd.io/) ，是 CNCF 的毕业设计。它是一个守护进程，充当各种容器运行时和操作系统的 API 门面。在后台，它依赖于 runc，并且是 Docker 引擎的默认运行时。它也被 Google Kubernetes 引擎(GKE)和 IBM Kubernetes 服务(IKS)使用。它是 Kubernetes 容器运行时接口的一个实现(与 CRI-O 相同)，因此它是 Kubernetes 集群运行时的一个很好的候选。*

# *图像检查和分发*

*集装箱堆栈的最后一部分是图像检查和分发。这有效地取代了`docker inspect`，并且(可选地)增加了在远程注册中心之间复制/镜像映像的能力。*

*我在这里提到的唯一可以完成这些任务的工具是 Skopeo。它是由 Red Hat 制作的，是 Buildah、Podman 和 CRI-O 的配套工具。除了我们都从 Docker 了解的基本`skopeo inspect`之外，Skopeo 还能够使用`skopeo copy`复制图像，这允许您在远程注册表之间镜像图像，而无需首先将它们拖到本地注册表。如果您使用本地注册表，此功能也可以作为拉/推。*

*作为一个小奖励，我还想提一下 [Dive](https://github.com/wagoodman/dive) ，这是一个检查、探索和分析图像的工具。它对用户更友好，提供更可读的输出，并且可以更深入地挖掘(或者我猜是*潜入*)你的图像，分析和衡量它的效率。它也适用于 CI 管道，可以衡量你的图像是否*【足够高效】*或者换句话说——是否浪费了太多空间。*

# *结论*

*本文并不是要说服您完全放弃 Docker，而是向您展示构建、运行、管理和分发容器及其映像的整体情况和所有选项。包括 Docker 在内的每一种工具都有其优点和缺点，评估哪一套工具最适合您的工作流和用例非常重要，我希望这篇文章能帮助您。*

## *资源*

*   *让我们尝试一下 Kubernetes 可用的每一个 CRI 运行时。不，真的！*
*   *[在 Kubernetes 中，容器运行时有多重要？](https://events19.linuxfoundation.org/wp-content/uploads/2017/11/How-Container-Runtime-Matters-in-Kubernetes_-OSS-Kunal-Kushwaha.pdf)*
*   *[集装箱术语实用介绍](https://developers.redhat.com/blog/2018/02/22/container-terminology-practical-introduction)*
*   *[比较下一代容器映像构建工具](https://events19.linuxfoundation.org/wp-content/uploads/2017/11/Comparing-Next-Generation-Container-Image-Building-Tools-OSS-Akihiro-Suda.pdf)*
*   *[全面的容器运行时比较](https://www.capitalone.com/tech/cloud/container-runtime/)*
*   *[建造没有码头工人的集装箱](https://blog.alexellis.io/building-containers-without-docker/)*

**本文最初发布于*[*martinheinz . dev*](https://martinheinz.dev/blog/35?utm_source=tds&utm_medium=referral&utm_campaign=blog_post_35)*

*[](/deploy-any-python-project-to-kubernetes-2c6ad4d41f14) [## 将任何 Python 项目部署到 Kubernetes

### 是时候深入 Kubernetes，使用这个成熟的项目模板将您的 Python 项目带到云中了！

towardsdatascience.com](/deploy-any-python-project-to-kubernetes-2c6ad4d41f14) [](/analyzing-docker-image-security-ed5cf7e93751) [## 分析 Docker 图像安全性

### 码头集装箱远没有你想象的那么安全…

towardsdatascience.com](/analyzing-docker-image-security-ed5cf7e93751) [](/all-the-things-you-can-do-with-github-api-and-python-f01790fca131) [## 你可以用 GitHub API 和 Python 做的所有事情

### GitHub REST API 允许您管理问题、分支、回购、提交等等，所以让我们看看您如何使用…

towardsdatascience.com](/all-the-things-you-can-do-with-github-api-and-python-f01790fca131)*