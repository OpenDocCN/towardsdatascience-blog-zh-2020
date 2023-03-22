# TigerGraph 和 Docker 的有效使用

> 原文：<https://towardsdatascience.com/efficient-use-of-tigergraph-and-docker-5e7f9918bf53?source=collection_archive---------39----------------------->

## TigerGraph 可以与 Docker 结合，在几乎任何操作系统上运行。在本文中，我们解决了大型 TigerGraph 图像及其未使用潜力的问题。

![](img/09f1318335e43c06d91ea749ba2d07f4.png)

图片作者。Logos 分别来自 [TigerGraph](https://www.tigergraph.com/) 和 [Docker 的](https://www.docker.com/)网站。

TigerGraph 是我首选的图形数据库和图形分析平台，因为它速度快、可扩展，并且有一个活跃的开源社区。由于附近没有 [TigerGraph Cloud](https://tgcloud.io/) 服务器，我经常在本地使用 TigerGraph。

在撰写本文时， [TigerGraph 软件要求](https://docs-beta.tigergraph.com/admin/admin-guide/hw-and-sw-requirements)规定支持以下操作系统:

*   Redhat 和 Centos 版本 6.5–6.9、7.0–7.4 和 8.0–8.2
*   Ubuntu 14.04、16.04 和 18.04
*   Debian 8

对于使用这个列表之外的操作系统的人来说，一个合理的解决方案是使用容器化:Docker，在本文的例子中。

在本文中，我们将涵盖:

1.  如何使用官方 TigerGraph 和里面的内容
2.  剥离不必要的臃肿的官方码头形象
3.  修改入口点以添加:

*   启动时运行`gadmin`
*   运行绑定在某个目录下的 GSQL 脚本
*   将日志文件的输出发送到`STDOUT`

4.使用 Docker Compose 运行 TigerGraph 图像

# 官方 TigerGraph 图像

运行开发人员版的官方 TigerGraph 映像可以通过以下命令获得:

```
docker pull docker.tigergraph.com/tigergraph-dev:latest
```

运行方式:

```
docker run -d -p 14022:22 -p 9000:9000 -p 14240:14240 --name tigergraph_dev --ulimit nofile=1000000:1000000 -v ~/data:/home/tigergraph/mydata -t docker.tigergraph.com/tigergraph-dev:latest
```

该[源](https://github.com/tigergraph/ecosys/tree/master/demos/guru_scripts/docker)给出了关于如何构建图像的更深入的说明，但概括来说:

*   使用 Ubuntu 16.04 的基本映像
*   所有必需的软件，如 tar、curl 等。已安装
*   可选软件，如 emacs、vim、wget 等。已安装
*   GSQL 101 和 102 教程以及 GSQL 算法库已下载
*   SSH 服务器、REST++ API 和 GraphStudio 是 3 个值得注意的端口，它们可以暴露出来并用于与服务器通信

整个映像的下载量接近 1.8-2.0 GB(取决于版本)，这给带宽带来了相当大的压力，尤其是对于 CI/CD 这样的资源敏感型使用情形。另一个值得注意的点是，使用 TigerGraph 所需要的只是一个 GSQL 套接字连接，它可以通过诸如 [Giraffle](https://github.com/Optum/giraffle) 和 [pyTigerGraph](https://pypi.org/project/pyTigerGraph/) 之类的工具进行接口。

我已经确定了膨胀的两个主要来源:

*   可选和不必要的软件，如 vim 和 GSQL 教程 101
*   TigerGraph Developer Edition 的最小操作不需要 GraphStudio 和二进制文件

# 剥离老虎图像

我用 Bitnami 的 [MiniDeb 映像](https://github.com/bitnami/minideb)替换了基本映像`ubuntu:16.04`，以便去掉几兆字节的不必要空间。这是黛比·杰西的作品。

下一步是删除在官方映像的`apt-get`阶段安装的不必要的二进制文件。我将 Vim 作为唯一的命令行文本编辑器，但保留了诸如`wget`、`git`、`unzip`、`emacs`等二进制文件。不再安装。

在 TigerGraph 安装期间，严格执行[硬件要求](https://docs-beta.tigergraph.com/admin/admin-guide/hw-and-sw-requirements)，如果不满足，安装将失败。因为我想让 DockerHub runners 自动构建和推送我的映像，所以我破解了这个检查，以便资源不足的 runners 可以继续构建映像。

这是通过用我的版本替换`os_utils`二进制来实现的，这使得`check_cpu_number()`和`check_memory_capacity()`函数更加宽松。这个二进制文件可以在下面找到:

```
/home/tigergraph/tigergraph-${DEV_VERSION}-developer/utils/os_utils
```

这已经减少了大约 400MB 的膨胀，我的 DockerHub 图像报告了 1.52GB 的压缩大小 TigerGraph 3.0.0(我注意到下载这些层表明它大约为 1.62GB)。

注意:我曾试图随意删除 GraphStudio 二进制文件，但这无法通过`gadmin start`脚本，因此必须进行更细致的调整，以便从 TigerGraph 中删除更多内容，例如编辑`gadmin` Python 脚本。

一旦我下载并解压缩了这两个图像，它们之间的最终结果可以通过调用`docker images`来查看:

我构建的源代码可以在这里找到[我鼓励任何有建议的人联系我！](https://github.com/DavidBakerEffendi/tigergraph)

**更新(2020 年 2 月 11 日):感谢 Bruno imi 在这项工作的基础上精简了 TigerGraph 企业版，并与我分享了他的代码。以下额外的剥离是他的工作，已经在这个形象实施。**

安装目录下似乎有文档和不必要的构建工件(比如许多`node_modules`)。可以在以下位置找到这些示例:

*   `${INSTALL_DIR}/app/${DEV_VERSION}/document/`
*   `${INSTALL_DIR}/app/${DEV_VERSION}/bin/gui/server/node_modules`
*   `${INSTALL_DIR}/app/${DEV_VERSION}/bin/gui/node/lib/node_modules`
*   `${INSTALL_DIR}/app/${DEV_VERSION}/gui/server/node_modules/`
*   `${INSTALL_DIR}/app/${DEV_VERSION}/.syspre/usr/share/`
*   `${INSTALL_DIR}/app/${DEV_VERSION}/.syspre/usr/lib/jvm/java-8-openjdk-amd64–1.8.0.171/`

其中`INSTALL_DIR`是 TigerGraph Developer Edition v3 的`/home/tigergraph/tigergraph`，而`DEV_VERSION`是特定版本，例如`3.0.0`。

执行 tiger graph Developer Edition v3.0.5 的新版本，并将其与我的新版本 v 3 . 0 . 5 进行比较，我们看到使用了以下数量的磁盘空间:

# 修改入口点

在我们添加功能之前，让我们先来看看最初的`ENTRYPOINT`:

```
ENTRYPOINT /usr/sbin/sshd && su — tigergraph bash -c “tail -f /dev/null”
```

这做了两件事:

1.  SSH 服务器通过运行`/usr/sbin/ssh`启动。
2.  通过以用户`tigergraph`的身份运行`tail`命令，容器保持活动状态。这样做的目的是不断地从`/dev/null`读取输出，这也是为什么容器的`STDOUT`是空的。

## 启动时启动“gadmin”

为了改善用户体验，我添加了一行代码，在 Docker 入口点启动`gadmin`服务

```
ENTRYPOINT /usr/sbin/sshd && su - tigergraph bash -c "tail -f /dev/null"
```

到

```
ENTRYPOINT /usr/sbin/sshd && su - tigergraph bash -c "/home/tigergraph/tigergraph/app/cmd/gadmin start all && tail -f /dev/null"
```

一个很简单却很有价值的改变！

## 使用卷在启动时运行 GSQL 脚本

TigerGraph Docker 映像缺少(MySQL、MariaDB 和 PostgreSQL 等其他数据库映像有)一个名为 Something 的目录，类似于`docker-entrypoint-init.d`，用户可以在其中绑定数据库脚本以在启动时运行，例如用于模式创建或数据库填充。

有多种方法可以实现这一点，但我选择了一个相当简单的方法，在`gadmin`和`tail`命令之间添加下面一行:

该命令的工作原理是:

1.  命令`if`将检查一个名为`/docker-entrypoint-initdb.d`的目录是否存在，除非存在，否则不会执行下一步。
2.  `for file in /docker-entrypoint-initdb.d/*.gsql; do`行将对入口点文件夹中以扩展名`gsql`结尾的所有文件开始 for-each 循环。
3.  `su tigergraph bash -c`行将对 for-each 循环给出的文件运行 GSQL 命令。
4.  通过添加`|| continue`，如果脚本执行失败，容器和循环都不会停止。

如果放入`entrypoint.sh`中，这看起来会更整洁，但这取决于你！最终结果看起来像这样:

## 将日志路由到标准输出

为了弄清楚日志属于哪里，可以运行`gadmin log`，它将返回如下内容

```
ADMIN  : /home/tigergraph/tigergraph/log/admin/ADMIN#1.out
ADMIN  : /home/tigergraph/tigergraph/log/admin/ADMIN.INFO
CTRL   : /home/tigergraph/tigergraph/log/controller/CTRL#1.log
CTRL   : /home/tigergraph/tigergraph/log/controller/CTRL#1.out
DICT   : /home/tigergraph/tigergraph/log/dict/DICT#1.out
DICT   : /home/tigergraph/tigergraph/log/dict/DICT.INFO
ETCD   : /home/tigergraph/tigergraph/log/etcd/ETCD#1.out
EXE    : /home/tigergraph/tigergraph/log/executor/EXE_1.log
EXE    : /home/tigergraph/tigergraph/log/executor/EXE_1.out
...etc
```

我最感兴趣的是管理日志，所以我将把`tail`命令改为从`/home/tigergraph/tigergraph/log/admin/ADMIN.INFO`而不是`/dev/null`中读取。

现在，写入管理日志的任何内容都将自动传输到容器的日志中。所有三个步骤的最终产品现在是:

# 使用 Docker 编写 TigerGraph

请注意，我添加了一个健康检查，它每 5 秒钟调用一次 REST++ echo 端点来确定容器是否健康。如果您使用官方映像，您将需要 SSH 到容器中来手动启动所有服务:

如果您希望 GSQL 脚本在启动时运行，请在`volumes`下添加以下条目:

```
- my_script.gsql:/docker-entrypoint-initdb.d/my_script.gsql
```

请注意，我添加了一个健康检查，它每 5 秒钟调用一次 REST++ echo 端点来确定容器是否健康。这对于许多应用程序都很有用，在我的用例中，用于在开始测试之前检查容器在集成测试期间是否准备好了。

如果您使用官方映像，您将需要 SSH 到容器中来手动启动所有服务:

默认密码是“tigergraph ”,在此之后您可以调用该命令

```
gadmin start all (v3.0.0 >=)
gadmin start (v3.0.0<)
```

GraphStudio 可以在您的网络浏览器的`localhost:14240`中找到，Rest++可以在`localhost:9000`中找到。

# 结论

在这篇文章中，我们有:

*   检查了官方文件，
*   识别并删除明显不必要的文件，
*   构建了一个精简版的 TigerGraph 图像，节省了大量的磁盘空间，
*   修改了`ENTRYPOINT`以向容器添加额外的自动化，以及
*   使用 Docker Compose 运行此图像。

如果你对进一步减小图片的大小有任何建议或想法，那么留下评论、问题或分叉，并在 [GitHub 库](https://github.com/DavidBakerEffendi/tigergraph)上对本文中的代码提出请求。如果你想看更多 Docker 相关的指南，为数据库如 JanusGraph 建立自定义设置，请在下面留下评论。

你可以在 [Docker Hub](https://hub.docker.com/repository/docker/dbakereffendi/tigergraph) 上找到这些图片，我会随着新版本的出现继续更新这些图片，或者直到 TigerGraph 推出正式的更瘦版本。

如果您想加入 TigerGraph 社区并做出贡献，或者启动令人敬畏的项目并深入了解 TigerGraph 的未来，请在以下平台上加入我们:

*   [不和](https://discord.gg/F2c9b9v)
*   [Reddit](https://www.reddit.com/r/tigergraph/)
*   [抽动](https://www.twitch.tv/tigergraph)

如果你有兴趣看我的其他作品，那就看看我在 https://davidbakereffendi.github.io/的个人主页。

TigerGraph 的 Jon Herke 在社区中发挥了领导作用，让我们能够以有意义的方式做出贡献，Bruno imi 分享了他在精简 TigerGraph 企业版图像方面的发现，这些发现可在[https://hub.docker.com/r/xpertmind/tigergraph](https://hub.docker.com/r/xpertmind/tigergraph)找到。