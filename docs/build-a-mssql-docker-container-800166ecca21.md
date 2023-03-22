# 构建一个 MSSQL Docker 容器

> 原文：<https://towardsdatascience.com/build-a-mssql-docker-container-800166ecca21?source=collection_archive---------7----------------------->

使用容器数据库提升开发水平

![](img/13027fa71db8a7df6399de89ee04e216.png)

照片由[克里斯·帕甘](https://unsplash.com/@chris_pagan?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)在 [Unsplash](https://unsplash.com/s/photos/shipping?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) 拍摄

# 轻巧、快速、方便

您可以不再等待数据库管理员允许您访问开发服务器，或者在您的计算机的锁定工作环境中筛选关于设置 MS SQL 数据库的无尽文档，并获得您真正需要的基本信息。可以接受请求并返回数据的 sql server。就是这样。你的 API 需要测试，你需要一个数据库。

在开发的开始阶段，您不希望花费数小时来配置 sql server 或处理原型项目的托管数据库。这是一个本地码头集装箱可以改变你的生活。

# 在这篇文章中

我们将基于***mcr.microsoft.com/mssql/server***图像构建一个 Docker 容器，以创建一个定制的 Docker 文件，该文件启动一个 SQL Server、创建一个数据库、创建一个表并用数据填充该表。本文是在 [**为角斗士之旅 app 构建. NET 核心 API**](https://medium.com/@richardpeterson320/net-core-api-for-the-angular-tour-of-heroes-app-5895a36d2129) 的背景下撰写的。

**对于完成的 MSSQL Dockerfile 文件和脚本，请** [**访问我的 GitHub**](https://github.com/rchardptrsn/MSSQL-Docker-Container) **。**

**非常感谢** : twright-msft 给他的[明星码头工人形象](https://github.com/twright-msft/mssql-node-docker-demo-app)。我的很多图片都是直接从他的回购中复制的。

# 测试图像

在我们疯狂地构建自定义 docker 文件之前，让我们下载并运行映像以确保它能够工作。确保您的系统上安装了 [Docker](https://www.docker.com/products/docker-desktop) 。

从命令行运行:

```
docker pull mcr.microsoft.com/mssql/server:latest
```

通过运行下面的代码运行图像并构建容器。指定环境变量，如密码和 SQL Server Express 的版本。

```
docker run \
-e 'ACCEPT_EULA=Y' \
-e 'SA_PASSWORD=Password1!' \
-e 'MSSQL_PID=Express' \
--name sqlserver \
-p 1433:1433 -d mcr.microsoft.com/mssql/server:latest
```

如果您运行`docker container ls`,您应该看到 sqlserver 容器在端口 1433 上运行。

# 进入容器

![](img/e1443f7f6db703f44e0c3ce3ba5d16e9.png)

由[约书亚·隆多](https://unsplash.com/@rondeau?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)在 [Unsplash](https://unsplash.com/s/photos/portal?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) 上拍摄的照片

运行以下命令进入您的容器，并显示一个 bash 终端:

```
docker exec -it sqlserver "bash"
```

从容器命令行启动 sqlcmd。请注意，我们使用的密码与用`docker run`创建容器时使用的密码相同。

```
/opt/mssql-tools/bin/sqlcmd -S localhost -U SA -P "Password1!"
```

您应该会看到一个`1>`,通知您现在在 sqlcmd 命令行上，并且可以与您的服务器进行交互！让我们编写一些 SQL 来创建一个数据库和一个表。

```
CREATE DATABASE heroes
GO
USE heroes
CREATE TABLE HeroValue (id INT, name VARCHAR(50))
INSERT into HeroValue VALUES (1, "Wonder Woman")
GO
```

现在，查询数据库以确保数据被正确写入:

```
SELECT * FROM HeroValue;
GO
```

成功！

# 写文档

![](img/b7288ff508b2e4abbbcbdb48eabfb76b.png)

照片由[第十一波](https://unsplash.com/@11th_wave?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)在 [Unsplash](https://unsplash.com/s/photos/plan-network?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) 上拍摄

我们可以从 bash 终端创建数据库并插入条目，这很好，但感觉还是有点“手动”让我们通过编写 3 个脚本来实现自动化，Dockerfile 将在创建容器时运行这些脚本。这些脚本是:

1.  **entrypoint.sh** —在启动时运行的脚本，它只是运行 *import-data.sh* 并启动 SQL Server。
2.  **import-data.sh** —运行一个调用 *setup.sql、*的 ***sqlcmd*** 和一个导入. csv 文件的 *bcp 命令*。
3.  **setup.sql** —创建数据库和表的 sql 脚本。

下面是引用这三个脚本的最终 Dockerfile 文件。不要担心，我们将更详细地介绍这一切是如何工作的！

## Dockerfile 文件

```
FROM microsoft/mssql-server-linux:latest# Create work directory
RUN mkdir -p /usr/work
WORKDIR /usr/work# Copy all scripts into working directory
COPY . /usr/work/# Grant permissions for the import-data script to be executable
RUN chmod +x /usr/work/import-data.shEXPOSE 1433CMD /bin/bash ./entrypoint.sh
```

根据 [MSSQL docker 文档](https://docs.microsoft.com/en-us/sql/linux/sql-server-linux-configure-docker?view=sql-server-ver15)，您必须在启动 SQL Server 的最终命令之前运行数据库定制。在我们的 entrypoint.sh 文件中，我们在启动服务器之前调用 import-data.sh。

## entrypoint.sh

```
/usr/work/import-data.sh & /opt/mssql/bin/sqlservr
```

在创建容器时，entrypoint 将运行 import-data.sh 并启动服务器。Import-data.sh 为我们提供了两个 SQL Server 命令，以及一个非常必要的命令，用于在数据库启动时休眠 90 秒。在你等待或冥想的时候，喝杯茶。

## 导入-数据. sh

```
# wait for the SQL Server to come up
sleep 90s#run the setup script to create the DB and the schema in the DB
/opt/mssql-tools/bin/sqlcmd -S localhost -U SA -P "Password1!" -i setup.sql# import the data from the csv file
/opt/mssql-tools/bin/bcp heroes.dbo.HeroValue in "/usr/work/heroes.csv" -c -t',' -S localhost -U SA -P "Password1!" -d heroes
```

第二个命令 ***sqlcmd*** ，运行 setup.sql，搭建我们的数据库。创建数据库后，它使用 ***bcp*** 从. csv 文件导入数据。

## setup.sql

```
CREATE DATABASE heroes;
GO
USE heroes;
GO
CREATE TABLE HeroValue (id INT, name VARCHAR(50));
GO
```

# 使用您的 docker 文件构建图像

![](img/e4f2ceb1b793e4d66962364799002a5b.png)

Randy Fath 在 [Unsplash](https://unsplash.com/s/photos/build?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) 上拍摄的照片

通过运行以下命令，标记您的映像 mssql:dev 并进行构建:

```
docker build -t mssql:dev .
```

完成后，使用`docker run`运行映像。这几乎与我们的第一张`docker run`完全相同，除了我们已经将图像更改为我们新创建并标记的`mssql:dev`图像。

```
docker run \
-e 'ACCEPT_EULA=Y' \
-e 'SA_PASSWORD=Password1!' \
-e 'MSSQL_PID=Express' \
--name sqlserver \
-p 1433:1433 \
-d mssql:dev
```

当创建了映像并且我们收到确认映像 ID 时，访问容器以确认它正在工作。

```
docker exec -it sqlserver "bash"
```

从容器命令行，访问英雄数据库。

```
/opt/mssql-tools/bin/sqlcmd -S localhost -U SA -P "Password1!" -d heroes
```

查询数据以确保其插入正确。出于某种原因，当我运行它时，它返回一个空白值块，以及最后一个值。但是我已经通过从 C#运行查询确认了所有的值都在那里。如果有人知道为什么没有显示所有的值，我很想知道。

```
SELECT * FROM HeroValue;
GO
```

就是这样！现在，您已经拥有了一个 SQL Server 数据库，可以随时轻松配置和启动！

您可以通过运行以下命令来终止容器:

```
docker kill sqlserver
```

用您的`docker run`命令重启您的容器，您可以很容易地将它存储在一个脚本中，比如***start-docker-SQL . sh***或类似的文件。

```
docker run \
-e 'ACCEPT_EULA=Y' \
-e 'SA_PASSWORD=Password1!' \
-e 'MSSQL_PID=Express' \
--name sqlserver \
-p 1433:1433 \
-d mssql:dev
```

# 推送至 Docker Hub 存储库

现在您已经有了一个工作映像，您肯定希望将它保存在一个托管的存储库中，这样您就可以根据需要将映像拉到不同的机器上。

在 https://hub.docker.com/的[创建一个 Docker Hub 账户。](https://hub.docker.com/)

然后创建一个新的存储库，并给它一个名称和描述。

从命令行运行`docker login`。系统会提示您输入创建帐户时设置的凭据。

现在，标记您的图像:

```
docker tag local-image:tagname username/new-repo:tagname
```

接下来，将您的映像推送到您的新存储库:

```
 docker push username/new-repo:tagname
```

完成后，您将能够通过运行以下命令提取您的映像:

```
docker pull username/new-repo:tagname
```

希望这能让你的生活轻松一点！您可以阅读本系列的下一篇文章，通过. NET API 使用这个数据库[。](/net-core-api-dive-into-c-27dcd4170066)

# 额外收获:C#连接字符串

```
"ConnectionStrings": {
    "DockerDB": "Server=localhost,1433;Database=heroes;User ID=SA;Password=Password1!"
  }
```

感谢您的阅读！