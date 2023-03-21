# 数据科学家的 Docker 最佳实践

> 原文：<https://towardsdatascience.com/docker-best-practices-for-data-scientists-2ed7f6876dff?source=collection_archive---------12----------------------->

![](img/acbab94ce762c42bb8e93510e382edad.png)

码头工人…鲸鱼…你懂的。

作为一名数据科学家，我每天都与 Docker 打交道。对我来说，创建图像、旋转容器已经变得和编写 Python 脚本一样普通。这个旅程有它的成就也有它的时刻，“我希望我以前就知道”。

本文讨论了在数据科学项目中使用 Docker 时的一些最佳实践。这绝不是一份详尽的清单。但这涵盖了我作为数据科学家遇到的大多数事情。

本文假设读者对 Docker 有基本到中等程度的了解。例如，你应该知道 Docker 是用来做什么的，应该能够轻松地编写 Docker 文件，并理解 Docker 命令，如`RUN`、`CMD`等。如果没有，通读这篇来自 Docker 官方网站的文章。你也可以浏览那里收集的文章。

# 为什么是 Docker？

自从《Docker》发行以来，它风靡了全世界。在 Docker 时代之前，虚拟机曾经填补了这一空白。但是 Docker 提供的不仅仅是虚拟机。

# docker 的优势

*   隔离—隔离的环境，无论底层操作系统/基础架构、安装的软件、更新
*   轻量级—共享操作系统内核，避免每个容器都有操作系统内核
*   性能——轻量级允许许多容器在同一个操作系统上同时运行

# 码头工人入门

Docker 有三个重要的概念。

这是一组可运行的库和二进制文件，代表了一个开发/生产/测试环境。您可以通过以下方式下载/创建映像。

*   从图像注册表中提取:例如`docker pull alpine`。这里发生的是，Docker 将在本地计算机中查找名为`alpine`的图像，如果没有找到，它将在 [Dockerhub](https://hub.docker.com/) 中查找
*   使用 Dockerfile 文件在本地构建映像:例如`docker build . -t <image_name>:<image_version>`。在这里，您不是在尝试下载/拉取图像，而是在构建您自己的图像。但这并不完全正确，因为一个`Dockerfile`包含一个以`FROM <base-image>`开始的行，它寻找一个基础图像作为开始，这个基础图像可能是从 Dockerhub 中提取的。

**容器**——这是一个图像的运行实例。你可以使用语法``docker container run <arguments> <image> <command>` 建立一个容器，例如从`alpine`图像使用，`docker container run -it alpine /bin/bash`命令创建一个容器。

**卷** —卷用于永久/临时存储数据(如日志、下载的数据)供容器使用。此外，卷可以在多个容器之间共享。您可以通过多种方式使用卷。

*   创建卷:您可以使用`docker volume create <volume_name>`命令创建卷。*请注意，如果该卷被删除，存储在此处的信息/更改将会丢失。*
*   绑定挂载一个卷:您还可以使用`-v <source>:<target>`语法将现有的卷从主机绑定挂载到您的容器。例如，如果您需要将`/my_data`卷作为`/data`卷安装到容器中，您可以执行`docker container run -it -v /my_data:/data alpine /bin/bash`命令。*您在装载点所做的更改将反映在主机上。*

# 1.创建图像

## 1.保持图像较小，避免缓存

构建图像时，您必须做两件常见的事情，

*   安装 Linux 软件包
*   安装 Python 库

当安装这些包和库时，包管理器将缓存数据，这样如果你想再次安装它们，将使用本地数据。但是这不必要地增加了图像尺寸。docker 图像应该是尽可能轻量级的。

当安装 Linux 包时，记得通过添加最后一行到您的`apt-get install`命令来删除任何缓存的数据。

```
RUN apt-get update && apt-get install tini && \
 rm -rf /var/lib/apt/lists/*
```

安装 Python 包时，为了避免缓存，请执行以下操作。

```
RUN pip3 install <library-1> <library-2> --no-cache-dir`
```

## 2.将 Python 库分离到 requirements.txt 中

您看到的最后一个命令将我们带到了下一点。最好将 Python 库分离到一个`requirements.txt`文件中，并使用下面的语法使用该文件安装库。

`RUN pip3 install -r requirements.txt --no-cache-dir`

这很好地区分了 Dockerfile 做“Docker 的事情”和不(明确地)担心“Python 的事情”。此外，如果您有多个 docker 文件(例如用于生产/开发/测试),并且它们都希望安装相同的库，您可以轻松地重用该命令。`requirements.txt`文件只是一堆库名。

```
numpy==1.18.0
scikit-learn==0.20.2
pandas==0.25.0
```

## 3.修复库版本

注意在`requirements.txt`中我是如何冻结我想要安装的版本的。这一点非常重要。因为否则，每次构建 Docker 映像时，您可能会安装不同东西的不同版本。[属地地狱](https://en.wikipedia.org/wiki/Dependency_hell)是真实的。

# 运行容器

## 1.拥抱非根用户

当您运行容器时，如果您没有指定一个用户来运行，它将假定为`root`用户。我不想撒谎。我天真的自己曾经喜欢使用`sudo` 或者`root`来按照我的方式做事(尤其是绕过许可)。但如果我学到了一件事，那就是拥有不必要的特权是一种恶化的催化剂，会导致更多的问题。

要以非根用户的身份运行容器，只需

*   `docker run -it -u <user-id>:<group-id> <image-name> <command>`

或者，如果你想跳到一个已存在的容器中去，

*   `docker exec -it -u <user-id>:<group-id> <container-id> <command>`

例如，您可以通过将`<user-id>` 指定为`$(id -u)`并将`<group-id>`指定为`$(id -g)`来匹配主机的用户 id 和组 id。

> 注意不同的操作系统如何分配用户 id 和组 id。例如，您在 MacOS 上的用户 ID /组 ID 可能是 Ubuntu 容器中预先分配/保留的**用户 ID/组 ID。**

## 2.创建非特权用户

我们可以以非 root 用户的身份登录我们的主机——远离主机——这太棒了。但是如果你像这样登录，你就是一个没有用户名的用户。因为，显然容器不知道用户 id 来自哪里。每次你想把一个容器或`exec`变成一个容器时，你需要记住并输入这些用户 id 和组 id。因此，您可以将这个用户/组创建作为`Dockerfile`的一部分。

```
ARG UID=1000
ARG GID=1000
```

*   首先在`Dockerfile`上增加`ARG UID=1000`和`ARG GID=1000`。`UID` 和`GID`是容器中的环境变量，您将在`docker build`阶段向其传递值(默认为 1000)。
*   然后使用`RUN groupadd -g $GID john-group`在映像中添加一个组 ID 为`GID`的 Linux 组。
*   接下来，使用`useradd -N -l -u $UID -g john-group -G sudo john`在映像中添加一个用户 ID 为`UID`的 Linux 用户。您可以看到，我们正在将`john`添加到`sudo`组中。但这是可有可无的事情。如果你 100%确定你不需要`sudo`的许可，你可以省去它。

然后，在映像构建期间，您可以传递这些参数的值，例如，

*   `docker build <build_dir> -t <image>:<image_tag> --build-arg UID=<uid-value> --build-arg GID=<gid-value>`

举个例子，

*   `docker build . -t docker-tut:latest --build-arg UID=$(id -u) --build-arg GID=$(id -g)`

拥有非特权用户有助于您运行不应该拥有 root 权限的进程。例如，当 Python 脚本所做的只是从一个目录(例如数据)读取和向一个目录(例如模型)写入时，为什么要以 root 用户身份运行它呢？还有一个额外的好处，如果您在容器中匹配主机的用户 ID 和组 ID，那么您创建的所有文件都将拥有您的主机用户的所有权。因此，如果您绑定装载这些文件(或创建新文件)，它们看起来仍像是您在主机上创建的。

# 创建卷

## 1.使用卷分离工件

作为一名数据科学家，很明显你将与各种工件(例如数据、模型和代码)打交道。您可以将代码放在一个卷中(如`/app`)，将数据放在另一个卷中(如`/data`)。这将为您的 Docker 映像提供一个良好的结构，并消除任何主机级的工件依赖性。

我说的工件依赖是什么意思？假设你在`/home/<user>/code/src`有代码，在`/home/<user>/code/data`有数据。如果将`/home/<user>/code/src`复制/挂载到卷`/app`并将`/home/<user>/code/data`复制/挂载到卷`/data`。如果代码和数据的位置在主机上发生变化，也没有关系。只要您挂载这些工件，它们将总是在 Docker 容器中的相同位置可用。因此，您可以在 Python 脚本中很好地修复这些路径，如下所示。

```
data_dir = "/data"
model_dir = "/models"
src_dir = "/app"
```

您可以使用`COPY`将必要的代码和数据放入图像

```
COPY test-data /data
COPY test-code /app
```

注意`test-data`和`test-code`是主机上的目录。

## 2.开发期间绑定安装目录

关于绑定挂载的一个伟大之处在于，无论你在容器中做什么，都会被反射到主机上。当您正在进行开发并且想要调试您的项目时，这非常有用。让我们通过一个例子来看看这一点。

假设您通过运行以下命令创建了 docker 映像:

`docker build <build-dir> <image-name>:<image-version>`

现在，您可以使用以下方式从该图像中建立一个容器:

`docker run -it <image-name>:<image-version> -v /home/<user>/my_code:/code`

现在，您可以在容器中运行代码，同时进行调试，对代码的更改将反映在主机上。这又回到了在容器中使用相同的主机用户 ID 和组 ID 的好处。您所做的所有更改，看起来都来自主机上的用户。

## 3.切勿绑定装载主机的关键目录

有趣的故事！我曾经将我的机器的主目录挂载到一个 Docker 容器中，并设法更改了主目录的权限。不用说，我后来无法登录到系统，花了好几个小时来解决这个问题。因此，只装载需要的东西。

例如，假设您有三个想要在开发期间挂载的目录:

*   `/home/<user>/my_data`
*   `/home/<user>/my_code`
*   `/home/<user>/my_model`

您可能很想用一行代码安装`/home/<user>`。但是写三行代码来分别挂载这些单独的子目录绝对是值得的，因为这将为您节省几个小时(如果不是几天)的时间。

# 其他提示

## 1.知道添加和复制的区别

你可能知道有两个 Docker 命令叫做`ADD`和`COPY`。有什么区别？

*   `ADD`可用于从网址下载文件，使用时如，`ADD <url>`
*   `ADD`当给定一个压缩文件(如`tar.gz`)时，会将文件提取到所提供的位置。
*   `COPY`将给定的文件/文件夹复制到容器中的指定位置。

## 2.入口点和 CMD 的区别

我想到的一个很好的类比是，把`ENTRYPOINT`想象成一辆汽车，把`CMD`想象成汽车中的控制装置(例如加速器、刹车、方向盘)。它本身什么都不做，它只是一个容器，你可以在里面做你想做的事情。它只是等待您将任何传入的命令推送到容器中。

命令`CMD`是容器中实际执行的内容。例如,`bash`会在你的容器中创建一个 shell，这样你就可以像在 Ubuntu 的普通终端上工作一样在容器中工作。

## 3.将文件复制到现有容器

又来了！我已经创建了这个容器，但忘了将这个文件添加到图像中。建立图像需要很长时间。有没有什么方法可以欺骗我并把它添加到现有的容器中？

是的，有，你可以使用`docker cp`命令。简单地做，

`docker cp <src> <container>:<dest>`

下一次你跳进容器时，你会在`<dest>`看到复制的文件。但是请记住，在构建时实际更改`Dockerfile`来复制必要的文件。

# 3.结论

太好了！这是所有的乡亲。我们讨论过，

*   什么是 Docker 映像/容器/卷？
*   如何写好 Dockerfile 文件
*   如何以非根用户的身份启动容器
*   如何在 Docker 中进行正确的卷安装
*   以及使用`docker cp`拯救世界等额外提示

现在你应该有信心看着 Docker 的眼睛说“*你吓不倒我*”。玩笑归玩笑，知道你在 Docker 上做什么总是值得的。因为如果你不小心的话，你可能会使整个服务器瘫痪，并扰乱在同一台机器上工作的其他人的工作。

如果你喜欢我分享的关于数据科学和机器学习的故事，考虑成为会员吧！

[](https://thushv89.medium.com/membership) [## 通过我的推荐链接加入媒体

### 作为一个媒体会员，你的会员费的一部分会给你阅读的作家，你可以完全接触到每一个故事…

thushv89.medium.com](https://thushv89.medium.com/membership)