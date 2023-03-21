# Conda + Google Colab

> 原文：<https://towardsdatascience.com/conda-google-colab-75f7c867a522?source=collection_archive---------1----------------------->

## 使用 Google Colab 时安装 Conda 的指南

![](img/b9a38ae25d73cb1f6bf1c74138c614a1.png)

让 Conda 在 Google Colab 上工作有点乏味，但如果你不能与 pip 相处，这是必要的。[来源](https://pixabay.com/users/geralt-9301/)

[Conda](https://docs.conda.io/en/latest/index.html) 是许多流行的数据科学工具的推荐环境和包管理解决方案，包括 [Pandas](https://pandas.pydata.org/pandas-docs/stable/getting_started/install.html) 、 [Scikit-Learn](https://scikit-learn.org/stable/install.html) 、 [PyTorch](https://pytorch.org/) 、 [NVIDIA Rapids](https://rapids.ai/) 和许多其他工具。Conda 还大大简化了安装流行的深度学习工具的过程，如 [TensorFlow](/installing-tensorflow-bcbe6ef21213) 。

[Google Colab](https://colab.research.google.com/) 是一项免费服务，通过与 Jupyter 笔记本*非常*相似的用户界面提供交互式计算资源，运行在谷歌云平台(GCP)上，并提供对 GPU 和 TPU 的免费访问。Google Colab 是一个很棒的教学平台，也可能是唯一一个与你的同行共享 GPU 或 TPU 加速代码的免费解决方案。不幸的是，Conda 在 Google Colab 上默认是不可用的，在 Google Colab 的默认 Python 环境中安装并正常工作 Conda 有点麻烦。

在这篇文章中，我将向您介绍我在 Google Colab 中工作时需要使用 Conda 安装包时所经历的过程。

# 预赛

首先，您需要确认 Google Colab 中默认使用的是哪种 Python。运行以下命令将返回默认 Python 可执行文件的绝对路径。

```
!which python # should return /usr/local/bin/python
```

现在检查这个默认 Python 的版本号。

```
!python --version
```

在编写时，上述命令返回`Python 3.6.9`。这意味着，为了使用所有预装的 Google Colab 包，您需要安装一个默认情况下与 Python 3.6 兼容的 Miniconda 版本。默认情况下，Miniconda 的最新版本(即 4.5.12+)面向 Python 3.7 或 Python 3.8。针对 Python 3.6 的 Miniconda 的最新版本是 Miniconda 4.5.4，因此这是您应该安装的版本。

最后，检查是否已经设置了`PYTHONPATH`变量。

```
!echo $PYTHONPATH
```

在编写这个命令时，它只返回`/env/python`(奇怪的是，这个目录似乎并不存在于 Google Colab 文件系统中)。

通常情况下，在安装 Miniconda 之前取消设置`PYTHONPATH`变量是一个好主意，因为如果通过`PYTHONPATH`中包含的目录安装和访问的软件包与 Miniconda 中包含的 Python 版本不兼容，这可能会导致问题。

您可以使用以下命令取消设置`PYTHONPATH`变量。这一步是可选的，但是如果你不取消设置这个变量，那么你会在安装 Miniconda 后看到一个警告消息。

```
%env PYTHONPATH=
```

# 安装 Miniconda

当在 Google Colab 单元中执行时，下面的代码将下载 Miniconda 适当版本的安装程序脚本，并将其安装到`/usr/local`中。直接安装到`/usr/local`，而不是默认位置`~/miniconda3`，确保 Conda 及其所有必需的依赖项在 Google Colab 中自动可用。

```
%%bashMINICONDA_INSTALLER_SCRIPT=[Miniconda3-4.5.4-Linux-x86_64.sh](https://repo.continuum.io/miniconda/Miniconda3-$MINICONDA_VERSION-Linux-x86_64.sh)
MINICONDA_PREFIX=/usr/local
wget [https://repo.continuum.io/miniconda/](https://repo.continuum.io/miniconda/Miniconda3-$MINICONDA_VERSION-Linux-x86_64.sh)$MINICONDA_INSTALLER_SCRIPT
chmod +x $MINICONDA_INSTALLER_SCRIPT
./$MINICONDA_INSTALLER_SCRIPT -b -f -p $MINICONDA_PREFIX
```

一旦你安装了 Miniconda，你应该能够看到 conda 可执行文件是可用的…

```
!which conda # should return /usr/local/bin/conda
```

…并且版本号是正确的。

```
!conda --version # should return 4.5.4
```

请注意，虽然安装 Miniconda 似乎不会影响 Python 可执行文件…

```
!which python # still returns /usr/local/bin/python
```

…然而，Miniconda 实际上安装了一个略有不同的 Python 版本。

```
!python --version # now returns Python 3.6.5 :: Anaconda, Inc.
```

# 更新 Conda

既然您已经安装了 Conda，那么您需要将 Conda 及其所有依赖项更新到最新版本*，而不需要*将 Python 更新到 3.7(或 3.8)。下面的`conda install`命令实际上是*将* Conda 更新到最新版本，同时保持 Python 版本固定在 3.6。然后,`conda update`命令将 Conda 的所有依赖项更新到最新版本。

```
%%bashconda install --channel defaults conda python=3.6 --yes
conda update --channel defaults --all --yes
```

现在，您可以通过检查 Conda 的版本号来确认更新。

```
!conda --version # now returns 4.8.3
```

另外，请注意 Python 版本再次发生了变化。

```
!python --version # now returns Python 3.6.10 :: Anaconda, Inc.
```

# 追加到`sys.path`

现在您已经安装了 Miniconda，您需要将 conda 将安装包的目录添加到 Python 在查找要导入的模块时将搜索的目录列表中。通过检查`[sys.path](https://docs.python.org/3/library/sys.html)`，您可以看到 Python 在查找要导入的模块时将搜索的当前目录列表。

```
import syssys.path
```

在撰写本文时，Google Colab 上的`sys.path`如下所示。

```
['',  
 '/env/python',
 '/usr/lib/python36.zip',
 '/usr/lib/python3.6',
 '/usr/lib/python3.6/lib-dynload',
 '/usr/local/lib/python3.6/dist-packages', # pre-installed packages
 '/usr/lib/python3/dist-packages',
 '/usr/local/lib/python3.6/dist-packages/IPython/extensions',
 '/root/.ipython']
```

请注意，Google Colab 附带的预安装软件包安装在`/usr/local/lib/python3.6/dist-packages`目录中。您可以通过简单地列出这个目录的内容来了解哪些包是可用的。

```
!ls /usr/local/lib/python3.6/dist-packages
```

你用 Conda 安装的任何包都将被安装到目录`/usr/local/lib/python3.6/site-packages`中，所以你需要把这个目录添加到`sys.path`中，以便这些包可以被导入。

```
import sys_ = (sys.path
        .append("/usr/local/lib/python3.6/site-packages"))
```

请注意，因为包含预装 Google Colab 软件包的`/usr/local/lib/python3.6/dist-packages`目录出现在 Conda 安装包的`/usr/local/lib/python3.6/site-packages`目录之前，所以通过 Google Colab 获得的软件包版本将优先于通过 Conda 安装的同一软件包的任何版本。

# 安装软件包

现在你需要做的就是安装你喜欢的软件包。只要记住在安装软件包时包含`--yes`标志，以避免提示确认软件包计划。

```
!conda install --channel conda-forge featuretools --yes
```

# 摘要

在本文中，当我需要使用 conda 来管理 Google Colab 上的包时，我介绍了安装和配置 Miniconda 的过程。希望这能在你下次需要在 Google Colab 上分享 Conda 管理的数据科学项目时有所帮助。