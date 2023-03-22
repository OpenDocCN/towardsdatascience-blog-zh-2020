# 在 Docker 中运行您的本地数据库！🐳

> 原文：<https://towardsdatascience.com/run-your-local-database-in-docker-3e7ed68a50f3?source=collection_archive---------7----------------------->

## 用于本地开发的 Docker 数据库映像:

## 如何使用 docker 建立本地开发数据库？快速简单。

![](img/5ec4152bf47f53208bde8d6bb650049e.png)

照片由 [Ishant Mishra](https://unsplash.com/@ishant_mishra54?utm_source=medium&utm_medium=referral) 在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 拍摄

当建立一个新项目时，有时会有一个全面的先决条件列表，以便能够运行一个项目。在这个简短的教程中，我们将使用 docker 映像建立一个本地数据库。例如，我们将使用 PostgreSQL，但是您可以自由选择您的数据库。

**先决条件:**你首先也是唯一需要的就是 Docker。我下载了 [Docker 桌面](https://www.docker.com/products/docker-desktop)以便在我的 mac 上开始使用(你可以自由使用你喜欢的任何 Docker 安装)。

我们开始吧！🙌

# ✌️My 3 个主要原因

我使用 docker 数据库映像进行本地开发的三个主要原因，以及我认为您也应该这样做的原因:

1.  它使本地开发设置变得容易。一个简单的抽象，使用一行设置来设置数据库！
2.  它不要求您在计算机上安装任何数据库(或数据库的特定版本)。
3.  它隔离了数据库环境。它允许我同时运行多个版本和实例，而不会遇到麻烦。

我通常将它作为选项添加到我的`README`文件的项目设置中。然后人们可以决定做什么——要么用手动方式做，要么用简单的方式做😄

**您并不局限于使用 PostgreSQL 映像** — [docker 拥有所有常见数据库的数据库映像](https://hub.docker.com/search?category=database&source=verified&type=image)，如 Redis、MySQL、Oracle 等。找到你的最爱。

# 让我们装运那个集装箱吧！

我们将使用 Postgres 映像启动一个新的 docker 容器来运行数据库:

```
$ **docker run --name my_postgres -p 5432:5432 -e POSTGRES_PASSWORD=postgres postgres**
```

这就对了。我们基本上完成了——在一行中，您已经设置好了所有的东西😄 🤷‍♀(如果你没有安装任何图像，docker 会问你是否要下载)。让我们看看我们刚刚创建的内容:

*   `--name my postgres`创建一个名为`my_postgres`的 docker 容器。
*   `-p`是设置端口的标志。`5432:5432`将内部 docker 端口映射到外部端口，这样我们可以有一个从外部到 docker 映像的门。端口`5432`是一个 Postgres 特定的端口——如果使用另一个数据库映像，您可能需要更改它。
*   `-e`是环境变量的标志。我们加上了`POSTGRES_PASSWORD`。
*   最后一部分`postgres`是我们将使用的 docker 图像(如果您使用另一个数据库，您应该更改它)

现在您有了一个名为`my_postgres`的 docker 容器，它在端口 5432 上运行 PostgreSQL 数据库。数据库将`name`、`username`和`password`设置为 **postgres** 。

# 👩‍💻一些细节

好了，让我们继续看一些 docker 标志和命令:

*   `-d`如果你想在后台运行你的数据库作为守护进程。如果没有`d`标志，您需要让您的容器一直在一个单独的终端中运行，以访问您的数据库。您可以通过运行`docker start <docker_name>`或`docker stop <docker_name>`来启动和停止容器。
*   `docker postgres:latest`如果你想确保使用最新的 Postgres 镜像。
*   `docker pull postgres`拉最新的 Postgres 图片。
*   `docker images`列出您电脑上的所有图像。
*   `docker ps -a`查看所有创建的容器(具有指定端口的容器是运行中的容器)。
*   `docker rm <docker_ID>`或`docker rm <docker_name>`移除码头集装箱。
*   从 docker 容器中清除所有数据库数据。即使您有几个 docker 容器在运行，您仍然会访问相同的数据，因为它被写入相同的位置。

# 💅哪里可以欣赏我的新数据库？

您可以选择您喜欢的数据库 GUI 工具并连接到您的数据库。我喜欢用 [*DBeaver*](https://dbeaver.io/) *。*使用以下命令连接到我们在 Docker 容器中运行的新创建的数据库:

*   **数据库名称:** postgres
*   **数据库用户:** postgres
*   **数据库密码:** postgres
*   **端口:** 5432

如果您喜欢在终端中使用数据库，您可以在 docker 容器中输入以下命令来访问数据库(我们的 docker 在本教程中被命名为 **my_postgres** ):

```
$ **docker exec -it <DATABASE_NAME> bash**
```

🎉**快乐编码**
🐳你会开始使用 docker 数据库镜像来进行本地设置吗？