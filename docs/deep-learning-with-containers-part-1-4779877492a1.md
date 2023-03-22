# 用容器进行深度学习。第一部分

> 原文：<https://towardsdatascience.com/deep-learning-with-containers-part-1-4779877492a1?source=collection_archive---------15----------------------->

![](img/f9bc577a9847deedf487272e50372229.png)

照片由 Antoine Petitteville 在 Unsplash 上拍摄

# TL；速度三角形定位法(dead reckoning)

容器是在主机计算环境中独立运行的轻量级软件包。使用集装箱化的环境进行研究是有帮助的，因为它们易于使用，可重复，并保持您的主要系统干净。您可以使用 Docker Compose 轻松设置一个功能完整的多容器应用程序。所有需要的脚本都可以在[这里](https://github.com/visheratin/dl-containers/tree/master/part-1)找到。

**更新 02/2021:** 该系列的第二部分可在[这里](/deep-learning-with-containers-part-2-6495da296f79)。

# 为什么是集装箱？

当进行深入研究时，有时很难维持你周围正在发生的事情。大量包含数据的文件，几十个你不用但又不确定能清除它们的库，保存在你主目录中某处的临时文件，应有尽有。虚拟环境有所帮助，但作用不大。您仍然必须管理它们，并且可能会以同样混乱的位置结束，但是环境的数量会成倍增加。在设置我的笔记本电脑以处理深度 NLP 模型时，我想到了创建易于使用且完全一次性的实验环境的方法——容器。它们让我只需一个命令就可以设置带有 Tensorboard 跟踪功能的 Jupyter 笔记本。用另一个命令，我可以把这些都拉下来，而不用担心留在内存或硬盘上的东西。在这篇文章中，我将描述如何在您的系统中设置和部署深度学习的容器，在下一篇文章中，我将展示如何扩展特定任务的基本图像——使用拥抱面部变形器的连体学习。

我使用的堆栈如下:

*   [Docker](https://www.docker.com/) 。创建容器的平台和软件。该领域事实上的标准工具。
*   [PyTorch](https://pytorch.org/) 带 GPU 支持。我选择的框架，简直太棒了。
*   [Jupyter 笔记本](https://jupyter.org/)。支持在浏览器中进行交互式开发的软件。
*   [张量板](https://www.tensorflow.org/tensorboard)。实验跟踪的良好起点。从那你可以切换到[快板 AI](https://allegro.ai/) 或[重量&偏差](https://www.wandb.com/)。
*   [抱脸](https://huggingface.co/)。简单地说，你可以得到基于[变压器](https://en.wikipedia.org/wiki/Transformer_(machine_learning_model))和其他架构的最先进模型的最佳方式。

# 容器

如果你不熟悉容器的概念，阅读一些介绍性的文章(例如来自 [Docker](https://www.docker.com/resources/what-container) 或 [Google](https://cloud.google.com/containers) )并浏览覆盖基础知识的[教程](https://docker-curriculum.com/)会是一个好主意。

我们现在将创建两个容器—一个用于 Jupyter Notebook，另一个用于 Tensorboard。我们从 Tensorboard 容器开始，因为它非常小，容易理解。以下是它的完整版本:

```
*# Use small image with Python pre-installed.*
**FROM** python:3.8-slim-buster
*# Install tensorboard*
**RUN** pip3 install tensorboard
```

为了运行和操作 Tensorboard，我们不需要任何东西，除了它的包。您现在可以将这个文件保存为`tensorboard.Dockerfile`，构建容器(`docker build -t dl-tensorboard:latest -f tensorboard.Dockerfile .`)并使用命令`docker run --rm -it dl-tensorboard:latest /bin/bash`交互运行它，命令`tensorboard`将可供运行。但是它不会自己运行，它需要一个存储实验运行数据的目录。我们将在稍后的服务设置阶段处理它。

下一部分是用 Jupyter 笔记本建立一个图像。这里有相当多的细微差别。首先，我们将与深度学习合作，因此我们的容器中需要一个 GPU 支持。幸运的是，NVIDIA 用他们的[容器工具包](https://github.com/NVIDIA/nvidia-docker)覆盖了我们。经过几个简单的步骤，您就可以随意使用支持 GPU 的 Docker 容器了。NVIDIA 的另一个非常有用的项目是一组安装了 CUDA 的[预建图像](https://hub.docker.com/r/nvidia/cuda)。几个版本的 Ubuntu 之前，安装 NVIDIA 驱动程序和 CUDA 是一个巨大的痛苦。现在容易多了，但是有了这些图像，你不需要做任何与 CUDA 设置相关的事情，这很酷。

到写这篇文章的时候，PyTorch 已经支持 CUDA 10.2 版本了，所以我们将使用 NVIDIA base image 搭配 CUDA 10.2。此外，NVIDIA 提供了三种风格的图像— `base`、`runtime`和`devel`。我们的选项是`runtime`，因为包含了 cuDNN。

下一个有趣的点是，我们将执行我们的映像的多阶段构建。这意味着在构建过程中，我们将有一个封装了所有与设置相关的逻辑的基础映像，以及一个根据手头的任务添加收尾工作的最终映像，例如安装额外的包或复制所需的文件。这个逻辑将允许我们在 Docker 缓存中保存一个基础映像，这样就不需要每次添加新的包时都下载 PyTorch。毫无疑问，我们可以通过在 Dockerfile 中现有的行之后添加新的行，并且不打乱已经添加的行的顺序(因为[Docker 缓存的工作方式](https://docs.docker.com/develop/develop-images/dockerfile_best-practices/#leverage-build-cache))来实现相同的目的，而不需要多阶段构建，但是这种方法给出了责任分工，易于操作，并且为我们将在下一篇文章中讨论的更多高级场景奠定了基础。

在我们进入 Jupyter 图像的 docker 文件之前，我想澄清几个问题。首先，由于我们使用 NVIDIA CUDA 映像作为基础，我们将需要安装 Python 和一些必需的工具。第二，由于[已知问题](https://jupyter-notebook.readthedocs.io/en/stable/public_server.html#docker-cmd)，不可能在容器中运行笔记本二进制程序。前一段时间，需要在 docker 文件中设置 Tini，如链接中所述。但是现在 Tini 已经[内置到 Docker](https://github.com/krallin/tini/#using-tini) 中，所以不需要任何额外的设置，只需在运行容器时添加`--init`标志。稍后我们将看到如何将它合并到 Docker compose 中。

最后，我们可以看看 Jupyter 笔记本图像的实际 docker 文件:

```
*# NVIDIA CUDA image as a base*
*# We also mark this image as "jupyter-base" so we could use it by name*
**FROM** nvidia/cuda:10.2-runtime AS jupyter-base
**WORKDIR** /
*# Install Python and its tools*
**RUN** apt update && apt install -y --no-install-recommends **\
**    git **\
**    build-essential **\
**    python3-dev **\
**    python3-pip **\
**    python3-setuptools
**RUN** pip3 -q install pip --upgrade
*# Install all basic packages*
**RUN** pip3 install **\
**    *# Jupyter itself*
    jupyter **\
**    *# Numpy and Pandas are required a-priori*
    numpy pandas **\
**    *# PyTorch with CUDA 10.2 support and Torchvision*
    torch torchvision **\
**    *# Upgraded version of Tensorboard with more features*
    tensorboardX

*# Here we use a base image by its name - "jupyter-base"*
**FROM** jupyter-base
*# Install additional packages*
**RUN** pip3 install **\
**    *# Hugging Face Transformers*
    transformers **\
**    *# Progress bar to track experiments*
    barbar
```

记住之前描述的一切，脚本非常简单——获取 NVIDIA CUDA 映像，安装 Python，安装基本包，使用所有这些作为最终映像的基础，并根据具体问题添加一些额外的包。

# 服务

创建环境的最后一步是在 [Docker Compose](https://docs.docker.com/compose/) 的帮助下，将两个图像组合成完整的多容器应用程序。该工具允许创建一个 YAML 文件，该文件包含构成应用程序的服务的描述。这是一个很好的工具，主要关注单主机部署，这正是我们的用例。撰写本文时 Compose 的一个主要缺点是缺乏 GPU 支持，尽管 Docker 和它的 [API](https://docs.docker.com/engine/api/v1.40/) 已经实现了这个特性。但是有一个[解决方法](https://github.com/docker/compose/pull/7124)支持在组合服务中使用 GPU。我[已经将](https://github.com/beehiveai/compose)集成到最新发布的 Compose 中，并计划维护它，直到 GPU 和其他设备请求得到支持。在这方面，您需要安装 Compose 的这个分支，以便能够运行服务。

让我们看看`docker-compose.yml`文件:

```
**version**: "3.8"
**services**:
    **tensorboard**:
        **image**: dl-tensorboard
        **build**:
            **context**: ./
            **dockerfile**: tensorboard.Dockerfile
        **ports**:
            - ${TENSORBOARD_PORT}:${TENSORBOARD_PORT}
        **volumes**:
            - ${ROOT_DIR}:/jupyter
        **command**:
            [
                "tensorboard",
                "--logdir=${TENSORBOARD_DIR}",
                "--port=${TENSORBOARD_PORT}",
                "--bind_all",
            ]

    **jupyter-server**:
        **image**: dl-jupyter
        **init**: **true**
        **build**:
            **context**: ./
            **dockerfile**: jupyter.Dockerfile
        **device_requests**:
            - **capabilities**:
                - "gpu"
        **env_file**: ./.env
        **ports**:
            - ${JUPYTER_PORT}:${JUPYTER_PORT}
        **volumes**:
            - ${ROOT_DIR}:/jupyter
        **command**:
            [
                "jupyter",
                "notebook",
                "--ip=*",
                "--port=${JUPYTER_PORT}",
                "--allow-root",
                "--notebook-dir=${JUPYTER_DIR}",
                '--NotebookApp.token=${JUPYTER_TOKEN}'
            ]
```

这里有很多东西需要解开。首先，定义了两个服务— `tensorboard`和`jupyter-server`。`image`部分定义了将为服务构建的映像的名称。`build`部分定义了一个路径，指向将用于构建映像的目录或带有 Dockerfile 的存储库。在`ports`部分，我们指定主机的哪些端口将暴露给容器。这里事情变得有趣了——我们看到的不是数值，而是环境变量的名称。Docker compose 从一个名为`.env`的特殊文件中获取这些变量，用户在该文件中列出了在合成过程中必须使用的所有变量。这允许有一个单独的位置来存储不同环境中不同的合成脚本的所有部分。例如，一台机器上用于 Tensorboard 的端口可以被另一台机器占用，这个问题可以通过更改`.env`文件中`TENSORBOARD_PORT`变量的值来轻松解决。

对于我们的场景，`.env`文件如下所示:

```
ROOT_DIR=/path/to/application/root/directory *# path on the host*
JUPYTER_PORT=42065
JUPYTER_TOKEN=egmd5hashofsomepassword
JUPYTER_DIR=/jupyter/notebooks               *# path in the container*
TENSORBOARD_PORT=45175
TENSORBOARD_DIR=/jupyter/runs                *# path in the container*
```

`ROOT_DIR`是一个目录，将存储我们的应用程序所需的所有信息(笔记本、数据、运行历史)。从`docker-compose.yml`中的`volumes`部分可以看到，这个目录作为`/jupyter`安装到两个容器中，这解释了为什么`JUPYTER_DIR`和`TENSORBOARD_DIR`变量都有这个前缀。文件中的其他变量定义了要公开的端口和用于身份验证的 Jupyter Notebook 的令牌。

服务定义的`command`部分包含一组参数，这些参数组合成一个命令，将在容器启动时执行。为了解决笔记本二进制文件的上述问题，我们为 Jupyter 服务添加了`init: true`。我们还使用部分`env_file`将环境变量从`.env`文件传递到 Jupyter 笔记本的容器中。这是因为我们将在笔记本中使用这些变量，细节将在下一篇文章中介绍。服务定义的最后但并非最不重要的特性是一个`device_requests`部分。这正是使 GPU 在容器内部可用的解决方法。没有它，你将无法在多容器应用程序中享受顶级 NVIDIA 卡的强大功能。

最后，我们得到工作目录的如下结构:

```
├── .env                     *# File with environment variables.*
├── docker-compose.yml       *# Compose file with services.*
├── jupyter.Dockerfile       *# Dockerfile for the Jupyter image.*
├── tensorboard.Dockerfile   *# Dockerfile for the Tensorboard image.*
```

完成所有设置后，您终于可以在我们的工作目录中运行`docker-compose up -d`了。在下载了所有基本映像并安装了所有必需的包之后，两个容器都应该启动并运行了。第一次启动需要相当长的时间，但是接下来的会非常快——即使你在最终映像中安装了新的包，所有下载和安装基础包的繁重工作都会被跳过。当你完成工作后，只要运行`docker-compose down`所有的容器就会消失，让你的系统保持干净，为其他有趣的事情做好准备，比如在 Go 中开发分布式系统或者在 COMSOL 中运行复杂的物理模拟。

# 结论

在这篇文章中，我们看了一个例子，如何用 Jupyter Notebook、PyTorch 和 Tensorboard 建立一个可用于深度学习研究的容器化环境。在下一篇文章中，我们将深入探讨如何将这种环境用于一个真实的用例——新闻的几个镜头分类的暹罗学习。这篇文章的主要摘录:

1.  使用多阶段构建，其中基础映像完成一次性的重要工作，最终映像根据您的需求定制环境。你知道，就像微调。
2.  Docker Compose 不支持将 GPU 绑定到容器。但如果你真的想，它会的。