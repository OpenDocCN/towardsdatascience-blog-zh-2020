# 用 Conda 管理 CUDA 依赖性

> 原文：<https://towardsdatascience.com/managing-cuda-dependencies-with-conda-89c5d817e7e1?source=collection_archive---------1----------------------->

## 在下一个数据科学项目中管理 CUDA 依赖性的“最佳实践”

![](img/a8961a6af46358adc9116b11730ee324.png)

Conda 大大简化了启动新的 GPU 加速数据科学项目。来源:作者

将数据科学项目从 CPU 转移到 GPU 似乎是一项艰巨的任务。特别是有相当多不熟悉的附加软件，比如[英伟达 CUDA Toolkit](https://developer.nvidia.com/cuda-toolkit) 、[英伟达集体通信库(NCCL)](https://developer.nvidia.com/nccl) 、[英伟达深度神经网络库(cuDNN](https://developer.nvidia.com/cudnn) )下载安装。

如果你去 NVIDIA 开发者网站，你会找到大量关于如何在系统范围内安装这些库的文档和说明。但是，如果您需要这些新库的不同版本用于不同的项目，您该怎么办呢？你*可以*在系统范围内安装一堆不同版本的 NVIDIA CUDA Toolkit、NCCL 和 cuDNN，然后使用环境变量来控制每个项目的“活动”版本，但这很麻烦且容易出错。幸运的是有更好的方法！

在这篇文章中，我将向你展示如何使用 [Conda](https://docs.conda.io/en/latest/) 管理 NVIDIA CUDA Toolkit、NCCL 和 cuDNN 的项目特定版本。我假设您对 Conda 有基本的了解，所以如果您还没有听说过 Conda 或者刚刚入门，我鼓励您阅读我的其他文章[*Conda 入门*](/managing-project-specific-environments-with-conda-b8b50aa8be0e) 和 [*使用 Conda*](/managing-project-specific-environments-with-conda-406365a539ab) *管理您的数据科学项目。*

# NVIDIA 库可以通过 Conda 获得吗？

没错。您可以使用`conda search`命令查看默认渠道中有哪些版本的 NVIDIA CUDA Toolkit 可用。

```
$ conda search cudatoolkitLoading channels: done# Name                       Version           Build  Channel
cudatoolkit                      9.0      h13b8566_0  pkgs/main
cudatoolkit                      9.2               0  pkgs/main
cudatoolkit                 10.0.130               0  pkgs/main
cudatoolkit                 10.1.168               0  pkgs/main
cudatoolkit                 10.1.243      h6bb024c_0  pkgs/main
cudatoolkit                  10.2.89      hfd86e86_0  pkgs/main
cudatoolkit                  10.2.89      hfd86e86_1  pkgs/main
```

NVIDIA 实际上维护着自己的 Conda 通道，默认通道中可用的 CUDA Toolkit 版本与 NVIDIA 通道中的版本相同。如果您有兴趣确认这一点，您可以运行命令`conda search --channel nvidia cudatoolkit`，然后将构建号与上面列出的默认通道中的构建号进行比较。

默认或 NVIDIA Conda 渠道提供的 NVIDIA CUDA 工具包版本的一个重要限制是它们[不包括 NVIDIA CUDA 编译器(NVCC)](https://stackoverflow.com/questions/56470424/nvcc-missing-when-installing-cudatoolkit) 。

## cuDNN 呢？

您还可以从默认渠道找到各种版本的 cuDNN。

```
$ conda search cudnnLoading channels: done# Name                       Version           Build  Channel
cudnn                          7.0.5       cuda8.0_0  pkgs/main
cudnn                          7.1.2       cuda9.0_0  pkgs/main
cudnn                          7.1.3       cuda8.0_0  pkgs/main
cudnn                          7.2.1       cuda9.2_0  pkgs/main
cudnn                          7.3.1      cuda10.0_0  pkgs/main
cudnn                          7.3.1       cuda9.0_0  pkgs/main
cudnn                          7.3.1       cuda9.2_0  pkgs/main
cudnn                          7.6.0      cuda10.0_0  pkgs/main
cudnn                          7.6.0      cuda10.1_0  pkgs/main
cudnn                          7.6.0       cuda9.0_0  pkgs/main
cudnn                          7.6.0       cuda9.2_0  pkgs/main
cudnn                          7.6.4      cuda10.0_0  pkgs/main
cudnn                          7.6.4      cuda10.1_0  pkgs/main
cudnn                          7.6.4       cuda9.0_0  pkgs/main
cudnn                          7.6.4       cuda9.2_0  pkgs/main
cudnn                          7.6.5      cuda10.0_0  pkgs/main
cudnn                          7.6.5      cuda10.1_0  pkgs/main
cudnn                          7.6.5      cuda10.2_0  pkgs/main
cudnn                          7.6.5       cuda9.0_0  pkgs/main
cudnn                          7.6.5       cuda9.2_0  pkgs/main
```

## NCCL 怎么样？

默认频道有一些旧版本的 NCCL，但这些版本不会有用(除非你被迫使用非常旧版本的 TensorFlow 或类似软件)。

```
$ conda search ncclLoading channels: done# Name                       Version           Build  Channel
nccl                           1.3.5      cuda10.0_0  pkgs/main
nccl                           1.3.5       cuda9.0_0  pkgs/main
nccl                           1.3.5       cuda9.2_0  pkgs/main
```

不要担心！[康达锻造](https://conda-forge.org/)来救援。Conda Forge 是一个由社区主导的食谱、构建基础设施和 Conda 包管理器发行版的集合。当我在默认频道上找不到我需要的节目时，我总是会检查频道。

```
$ conda search -c conda-forge ncclLoading channels: done# Name                       Version           Build  Channel
nccl                           1.3.5      cuda10.0_0  pkgs/main
nccl                           1.3.5       cuda9.0_0  pkgs/main
nccl                           1.3.5       cuda9.2_0  pkgs/main
nccl                         2.4.6.1      h51cf6c1_0  conda-forge
nccl                         2.4.6.1      h7cc98d6_0  conda-forge
nccl                         2.4.6.1      hc6a2c23_0  conda-forge
nccl                         2.4.6.1      hd6f8bf8_0  conda-forge
nccl                         2.4.7.1      h51cf6c1_0  conda-forge
nccl                         2.4.7.1      h7cc98d6_0  conda-forge
nccl                         2.4.7.1      hd6f8bf8_0  conda-forge
nccl                         2.4.8.1      h51cf6c1_0  conda-forge
nccl                         2.4.8.1      h51cf6c1_1  conda-forge
nccl                         2.4.8.1      h7cc98d6_0  conda-forge
nccl                         2.4.8.1      h7cc98d6_1  conda-forge
nccl                         2.4.8.1      hd6f8bf8_0  conda-forge
nccl                         2.4.8.1      hd6f8bf8_1  conda-forge
nccl                         2.5.6.1      h51cf6c1_0  conda-forge
nccl                         2.5.6.1      h7cc98d6_0  conda-forge
nccl                         2.5.6.1      hc6a2c23_0  conda-forge
nccl                         2.5.6.1      hd6f8bf8_0  conda-forge
nccl                         2.5.7.1      h51cf6c1_0  conda-forge
nccl                         2.5.7.1      h7cc98d6_0  conda-forge
nccl                         2.5.7.1      hc6a2c23_0  conda-forge
nccl                         2.5.7.1      hd6f8bf8_0  conda-forge
nccl                         2.6.4.1      h51cf6c1_0  conda-forge
nccl                         2.6.4.1      h7cc98d6_0  conda-forge
nccl                         2.6.4.1      hc6a2c23_0  conda-forge
nccl                         2.6.4.1      hd6f8bf8_0  conda-forge
```

# 一些示例 Conda 环境文件

现在你已经知道如何找出各种 NVIDIA CUDA 库的哪些版本在哪些频道上可用，你已经准备好编写你的`environment.yml`文件了。在本节中，我将提供 PyTorch、TensorFlow 和 NVIDIA RAPIDS 的一些示例 Conda 环境文件，以帮助您开始下一个 GPU 数据科学项目。

## PyTorch

[PyTorch](https://pytorch.org/) 是基于 Torch 库的开源机器学习库，用于计算机视觉、自然语言处理等应用。它主要由脸书人工智能研究实验室开发。Conda 其实就是[推荐的安装 PyTorch 的方式](https://pytorch.org/get-started/locally/)。

```
name: nullchannels:
  - pytorch
  - conda-forge
  - defaultsdependencies:
  - cudatoolkit=10.1
  - mpi4py=3.0 # installs cuda-aware openmpi
  - pip=20.0
  - python=3.7
  - pytorch=1.5
  - torchvision=0.6
```

官方的 PyTorch 二进制文件附带了 NCCL 和 cuDNN，所以没有必要在你的`environment.yml`文件中包含这些库(除非其他一些包也需要这些库！).还要注意频道优先级:官方`pytorch`频道必须优先于`conda-forge`频道，以确保官方 PyTorch 二进制文件(包括 NCCL 和 cuDNN)将被安装(否则你将在`conda-forge`获得一些非官方版本的 PyTorch)。

## 张量流示例

[TensorFlow](https://www.tensorflow.org/) 是一个免费的开源软件库，用于数据流和跨一系列任务的差异化编程。它是一个符号数学库，也用于机器学习应用，如神经网络。

通过 Conda 可以获得 TensorFlow 的很多版本和构建(`conda search tensorflow`的输出太长，无法在此分享！).如何决定哪个版本是“正确的”版本？如何确保你得到一个包含 GPU 支持的构建？

使用`tensorflow-gpu`元包为您的操作系统选择合适的 TensorFlow 版本和版本。

```
name: null

channels:
  - conda-forge
  - defaults

dependencies:
  - cudatoolkit=10.1
  - cudnn=7.6
  - cupti=10.1
  - mpi4py=3.0 # installs cuda-aware openmpi
  - nccl=2.4
  - pip=20.0
  - python=3.7
  - tensorflow-gpu=2.1 # installs tensorflow=2.1=gpu_py37h7a4bb67_0
```

## BlazingSQL + NVIDIA RAPIDS 示例

作为最后一个例子，我想告诉你如何开始与[英伟达急流](https://rapids.ai/index.html)(和朋友！).

NVIDIA RAPIDS 是一套开源软件库和 API，让您能够完全在 GPU 上执行端到端的数据科学和分析管道(想想[熊猫](https://pandas.pydata.org/) + [Scikit-learn](https://scikit-learn.org/stable/index.html) ，但针对的是 GPU 而不是 CPU)。

我还在这个示例环境文件中包含了 [BlazingSQL](https://www.blazingsql.com/) 。BlazingSQL 是一个开源的 SQL 接口，可以使用 NVIDIA RAPIDS 将海量数据集直接提取-转换-加载(ETL)到 GPU 内存中进行分析。安装 BlazingSQL 以便它能与 NVIDIA RAPIDS 一起正常工作比大多数 Conda 软件包安装要稍微复杂一些，所以我决定在这里包含一个工作示例。

最后，上面的环境还包括 [Datashader](https://datashader.org/) ，这是一个图形管道系统，用于快速灵活地创建大型数据集的有意义的表示，可以通过 GPU 加速。如果你打算在 GPU 上进行所有的数据分析，你也可以在 GPU 上进行数据可视化。

```
name: null channels:  
  - blazingsql
  - rapidsai
  - nvidia
  - conda-forge
  - defaultsdependencies:  
  - blazingsql=0.13
  - cudatoolkit=10.1
  - datashader=0.10
  - pip=20.0
  - python=3.7
  - rapids=0.13
```

# 但是如果我需要 NVIDIA CUDA 编译器呢？

在本文的开头，我提到了默认渠道提供的 NVIDIA CUDA Toolkit 版本不包括 NVIDIA CUDA 编译器(NVCC)。然而，如果您的数据科学项目有需要编译自定义 CUDA 扩展的依赖项，那么您几乎肯定会需要 NVIDIA CUDA 编译器(NVCC)。

您如何知道您的项目依赖项需要 NVCC？很有可能你会尝试上面的标准方法，而 Conda 将无法成功地创建环境，并可能抛出一堆编译器错误。一旦你知道你需要 NVCC，有几种方法可以安装 NVCC 编译器。

## 试试 cudatoolkit-dev 包

从`conda-forge`渠道获得的`cudatoolkit-dev`包包括 GPU 加速库、调试和优化工具、C/C++编译器和运行时库。这个包包含一个安装后脚本，用于下载和安装完整的 CUDA 工具包(NVCC 编译器和库，但不包括 CUDA 驱动程序)。

虽然`conda-forge`提供的`cudatoolkit-dev`包确实包括 NVCC，但我很难让这些包持续正确地安装。

*   一些可用的构建需要手动干预来接受许可协议，这使得这些构建不适合安装在远程系统上(这是关键的功能)。
*   其他一些版本似乎可以在 Ubuntu 上运行，但不能在其他版本的 Linux 上运行，比如 CentOS。
*   其他依赖 CUDA 的 Conda 包通常会安装`cudatoolkit`包，即使这个包中包含的所有东西都已经通过`cudatoolkit-dev`安装了。

也就是说，首先尝试`cudatoolkit-dev`方法总是值得的。也许对你的用例有用。

下面是一个使用通过`cudatoolkit-dev`包安装的 NVCC 来编译来自 [PyTorch 集群](https://github.com/rusty1s/pytorch_cluster)的定制扩展的例子，PyTorch 集群是一个高度优化的图形集群算法的小型扩展库，用于与 [PyTorch](http://pytorch.org/) 一起使用。

```
name: null

channels:
  - pytorch
  - conda-forge
  - defaults

dependencies:
  - cudatoolkit-dev=10.1
  - cxx-compiler=1.0
  - matplotlib=3.2
  - networkx=2.4
  - numba=0.48
  - pandas=1.0
  - pip=20.0
  - pip:
    - -r file:requirements.txt
  - python=3.7
  - pytorch=1.4
  - scikit-image=0.16
  - scikit-learn=0.22
  - tensorboard=2.1
  - torchvision=0.5
```

上面引用的`requirements.txt`文件包含 PyTorch 集群和相关包，如 [PyTorch Geometric](https://pytorch-geometric.readthedocs.io/en/latest/) 。这是该文件的样子。

```
torch-scatter==2.0.*
torch-sparse==0.6.*
torch-spline-conv==1.2.*
torch-cluster==1.5.*
torch-geometric==1.4.*# make sure the following are re-compiled if environment is re-built
--no-binary=torch-scatter
--no-binary=torch-sparse
--no-binary=torch-spline-conv
--no-binary=torch-cluster
--no-binary=torch-geometric
```

这里使用的`--no-binary`选项确保了无论何时重建环境，都将重建带有自定义扩展的包，这有助于在从工作站移植到可能具有不同操作系统的远程集群时，提高环境构建过程的可重复性。

## 一般来说，使用 nvcc_linux-64 元包

获得 NVCC 并仍然使用 Conda 管理所有其他依赖项的最可靠方法是在您的系统上安装 NVIDIA CUDA Toolkit，然后安装来自`conda-forge`的元包`[nvcc_linux-64](https://anaconda.org/nvidia/nvcc_linux-64)`，该元包配置您的 Conda 环境，以使用安装在您系统上的 NVCC 以及安装在 Conda 环境中的其他 CUDA Toolkit 组件。虽然我发现这种方法比依靠`cudatoolkit-dev`更健壮，但这种方法更复杂，因为它需要首先在您的系统上安装特定版本的 NVIDIA CUDA Toolkit。

要查看这种方法的完整示例，请看我最近的文章 [*为 Horovod*](/building-a-conda-environment-for-horovod-773bd036bf64) *构建 Conda 环境。*

# 摘要

我在这篇文章中涉及了很多内容。我向您展示了如何使用`conda search`来查看哪些版本的 NVIDIA CUDA Toolkit 和相关库(如 NCCL 和 cuDNN)可通过 Conda 获得。然后，我向您介绍了几个可以使用 GPU 的流行数据科学框架的示例 Conda 环境文件。希望这些想法能帮助你在下一个数据科学项目中从 CPU 跳到 GPU！