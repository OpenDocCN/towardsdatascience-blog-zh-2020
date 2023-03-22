# 使用 Conda (+pip)管理基于 JupyterLab 的数据科学项目

> 原文：<https://towardsdatascience.com/managing-jupyterlab-based-data-science-projects-using-conda-pip-cd2ee8521705?source=collection_archive---------43----------------------->

## 为眼光敏锐的数据科学家收集的“最佳实践”

![](img/5ce089d16affadf2b557db347b783dc3.png)

使用 Conda (+pip)和一点 Bash，自动安装 JupyterLab 很容易。(图片:NASA/JPL/比约恩·杰森/塞恩·多兰/[Flickr](https://www.flickr.com/photos/136797589@N04/35532950750/)([CC BY-NC-ND 2.0](https://creativecommons.org/licenses/by-nc-nd/2.0/)))

本文讨论了使用 Conda (+pip)管理基于 [JupyterLab](https://jupyter.org/) 的数据科学项目的两种方法:一种是“全系统”方法，其中 Conda (+pip)用于管理所有项目共享的单个 JupyterLab 安装，另一种是“基于项目”方法，其中 Conda (+pip)用于管理每个项目的单独 JupyterLab 安装。在描述了这两种方法之后，我将浏览一些例子并讨论相关的权衡。

# “系统级”JupyterLab 安装

Conda (+pip)采用“全系统”方法管理 JupyterLab，用于管理所有或您的数据科学项目共享的 JupyterLab 安装。“全系统”方法有几个好处。

*   一组通用的 JupyterLab 扩展简化了用户界面(UI)和用户体验(UX)。
*   允许更快地启动新项目，因为不需要安装(和构建！)JupyterLab 每一个项目。
*   通过用户主目录中的`~/.jupyter`目录中的文件对 JupyterLab 进行简单的低级配置。

## “系统范围”安装的典型 Conda environment.yml

下面是一个“系统级”JupyterLab 安装的存根`environment.yml`文件。

```
name: jupyterlab-base-envchannels:
 — conda-forge
 — defaults

dependencies:
 — jupyterlab
 — jupyterlab-git # provides git support
 — nodejs # required for building (some) extensions
 — pip
 — pip:
 — -r file:requirements.txt # extensions available via pip go here
 — python
```

有几件事值得注意。首先，您应该将通过 Conda(通常来自`[conda-forge](https://conda-forge.org/)`频道)获得的任何 JupyterLab 扩展作为依赖项包含在这个文件中。通过 Pip 获得的 JupyterLab 扩展应该单独包含在一个`requirements.txt`文件中(下面讨论)。第二，我明确地将`nodejs`作为一个依赖项。 [Node.js](https://nodejs.org/en/) 是重新构建 JupyterLab 所必需的(这可能是必需的，取决于您的扩展集合)。最后，我安装 Pip 并使用`pip`来安装包含在一个单独的`requirements.txt`文件中的所有包和扩展。

## “系统范围”安装的典型 pip 要求. txt

关于`requirements.txt`文件没有什么特别的。您只需列出您想通过`pip`安装的包和扩展。在这里，我包括了 Jupyter 语言服务器协议扩展，它带来了完整的 IDE 功能，比如代码导航、悬停建议、linters、自动完成和 JupyterLab 的重命名。

```
jupyter-lsp
python-language-server[all]
```

## 使用 Bash 脚本自动化`jupyterlab-base-env`构建

因为 Conda (+pip)环境的环境构建过程有点复杂(因为它涉及到通过 Conda 安装包，通过 pip 安装包，然后可能安装扩展并重新构建 JupyterLab 本身),所以使用 Bash 脚本自动构建环境是一个好主意。

```
#!/bin/bash --login
set -econda env create \
 —-name jupyterlab-base-env \
 —-file environment.yml \
 —-force
conda activate jupyterlab-base-env
source postBuild # put jupyter labextension install commands here
```

注意`--login`标志的使用。这确保了脚本将在一个登录 shell 中运行，该 shell 将正确地获取必要的 Bash 概要文件，这些文件是`conda activate`命令按预期工作所必需的。还要注意对一个`postBuild`文件的引用。这是一个 Bash 脚本，包含启用这些扩展和重建 JupyterLab 所需的任何必要的`jupyter labextension install`命令。我已经在 [GitHub](https://github.com/davidrpugh/jupytercon-2020-talk/tree/jupyterlab-base-env) 上包含了上面提到的所有配置文件的工作示例。

## 保持身体的倾斜

您的`jupyterlab-base-env`环境应该*仅*包含 JupyterLab 和任何必需的扩展(+依赖项)。不要将用于数据科学项目的包安装到您的`jupyterlab-base-env`中。相反，您应该为每个项目创建单独的 Conda (+pip)环境，然后为每个特定于项目的 Conda (+pip)环境创建定制的 Jupyter 内核。

## 为 Conda 环境创建 Jupyter 内核

为您项目的每个 Conda (+pip)环境创建一个定制的 Jupyter 内核，这将允许您在一个通用的 JupyterLab 安装中从这些环境启动 Jupyter 笔记本和 IPython 控制台。您甚至可以使用`[jupyter-conda](https://github.com/fcollonval/jupyter_conda)`扩展为您机器上的所有 Conda (+pip)环境自动化内核创建过程！

然而，与其为每个 Conda (+pip)环境创建定制内核，我更喜欢为我真正关心的特定 Conda (+pip)环境手动创建定制 Jupyter 内核。

**如何手动创建自定义 Jupyter 内核**

在为 Conda (+pip)环境创建定制内核之前，您需要确保在 Conda 环境中安装了`[ipykernel](https://pypi.org/project/ipykernel/)`包，因为您将需要使用该包来创建[内核规范](https://jupyter-client.readthedocs.io/en/latest/kernels.html#kernelspecs)文件。

```
conda activate $PROJECT_DIR/env # don’t forget to activate!
python -m ipykernel install \ # requires ipykernel!
 --user \
 —-name name-for-internal-use-only \
 —-display-name “Name you will see in the JupyerLab launcher”
```

# 基于项目的 JupyterLab 安装

通过“基于项目”的方法来管理 JupyterLab，Conda (+pip)用于管理每个项目的独立 JupyterLab 安装。“基于项目”的方法有几个优点。

*   更灵活的用户界面/UX，因为 JupyterLab 版本和扩展可以为每个项目定制。
*   JupyterLab 前沿特性的更简单实验。
*   自动生成数据科学项目报告[“活页夹就绪”](https://mybinder.org/)。

## “基于项目”安装的典型 Conda environment.yml

这个`environment.yml`文件的结构类似于“全系统”方法的结构。不同之处在于，使用“基于项目”的方法，您应该将您的项目所需的所有包和扩展通过 Conda 添加到这个文件中；皮普和`requirements.txt`文件也是如此。

```
name: nullchannels:
 — conda-forge
 — defaults

dependencies:
 — jupyterlab
 — jupyterlab-git # extensions available via conda go here
 - nodejs
 — pip
 — pip:
 — -r file:requirements.txt # packages available via pip go here
 — python
```

## 使用 Bash 脚本自动构建项目环境

同样，您应该尽可能自动化 Conda (+pip)环境构建。这个脚本与“系统范围”方法中使用的脚本之间的唯一区别是，我将 Conda (+pip)环境安装在我的项目目录的一个名为`env`的子目录中。这是一个 [Conda (+pip)“最佳实践”](/managing-project-specific-environments-with-conda-406365a539ab)。

```
#!/bin/bash —-login
set -eexport ENV_PREFIX=$PROJECT_DIR/env
conda env create \
 —-prefix $ENV_PREFIX 
 —-file environment.yml \
 —-force
conda activate $ENV_PREFIX
source postBuild # put jupyter labextension install commands here
```

## “基于项目”的 JupyterLab 安装示例

正如我所承诺的，这里有几个“基于项目”方法的例子，可以作为你下一个数据科学项目的灵感。

*   [JupyterLab+Scikit Learn+Dask](https://github.com/davidrpugh/jupytercon-2020-talk/tree/scikit-learn-env):基于 CPU 的数据科学项目环境，将 JupyterLab 与 [Scikit-learn](https://scikit-learn.org/stable/index.html) 和 [Dask](https://dask.org/) (还有朋友！).包括一些常见的 JupyterLab 扩展。
*   [JupyterLab + PyTorch](https://github.com/davidrpugh/jupytercon-2020-talk/tree/pytorch-env) :使用 JupyterLab 和 [PyTorch](https://pytorch.org/) 进行 GPU 加速深度学习的标准环境。包括 GPU 和深度学习特定的 JupyterLab 扩展，如 [jupyterlab-nvdashboard](https://github.com/rapidsai/jupyterlab-nvdashboard) 和 [jupyterlab-tensorboard](https://github.com/chaoleili/jupyterlab_tensorboard) 。
*   [JupyterLab+NVIDIA RAPIDS+BlazingSQL+Dask](https://github.com/davidrpugh/jupytercon-2020-talk/tree/nvidia-rapids-env):更复杂的 GPU 加速机器学习环境有 JupyterLab、 [NVIDIA RAPIDS](https://rapids.ai/) 、 [BlazingSQL](https://www.blazingsql.com/) 、 [Dask](https://dask.org/) (还有*很多*的朋友！).包括一些常见的 JupyterLab 扩展以及一些特定于 GPU 的扩展，如 [jupyterlab-nvdashboard](https://github.com/rapidsai/jupyterlab-nvdashboard) 。

# %conda 和%pip 魔术命令

任何关于 JupyterLab、Conda 和 pip 的讨论，如果不提及内置的 IPython 魔术命令，将软件包通过 Conda ( `[%conda](https://ipython.readthedocs.io/en/stable/interactive/magics.html#magic-conda)`)或 Pip ( `[%pip](https://ipython.readthedocs.io/en/stable/interactive/magics.html#magic-pip)`)安装到*活动*环境/内核中，都是不完整的。

*   这两个命令都可以在 Jupyter 笔记本或 IPython 控制台中使用。
*   `%conda`和`%pip`对新项目的原型设计都非常有用。
*   对于“生产”，更喜欢向`environment.yml`或`requirements.txt`文件添加新的包(并重建环境)。

# 摘要

希望到此为止，您将理解用 Conda (+pip)管理 JupyterLab 安装的“系统范围”和“基于项目”方法之间的区别。您还看到了这两种方法的几个示例，包括一些可用于下一个数据科学项目的起始代码。

总的来说，我推荐“基于项目”的方法，因为它具有更大的灵活性和最小的额外开销。如果您只在部分项目中使用 GPU，那么您可能更喜欢“基于项目”的方法，因为有一些很好的用于 GPU 加速数据科学项目的 JupyterLab 扩展(您不想为纯 CPU 项目安装这些扩展)。然而，如果您的所有项目要么只有 CPU，要么几乎总是使用 GPU，并且总是使用一组通用的 JupyterLab 扩展，那么您可能更喜欢“系统范围”的方法。