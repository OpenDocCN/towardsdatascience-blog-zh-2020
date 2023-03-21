# 安装 TensorFlow

> 原文：<https://towardsdatascience.com/installing-tensorflow-bcbe6ef21213?source=collection_archive---------34----------------------->

## 这并不像你可能听说的那样困难

![](img/a992782760db456f3f88fbb525cb91ac.png)

朋友不让朋友从源头建 TF。[来源](https://www.tensorflow.org/)

我看到越来越多的文章认为安装 [TensorFlow](https://www.tensorflow.org/) 很困难。从源代码构建 TensorFlow 肯定不适合胆小的人，但是安装 TensorFlow 用于您的下一个 GPU 加速的数据科学项目可以在一行中完成(如果您使用正确的工具)。

# 一条线的解决方案

使用 Conda (+pip)，您可以在一行中安装支持 GPU 的最新版本 TensorFlow。

```
conda create --name tensorflow-22 \
    tensorflow-gpu=2.2 \
    cudatoolkit=10.1 \
    cudnn=7.6 \
    python=3.8 \
    pip=20.0
```

如果你以前从未使用过 Conda (+pip)，我推荐你看看《Conda *入门》。*

# 我的首选解决方案

如果您只想快速启动并运行 TensorFlow 2.2 的一些新功能，上面的一行解决方案非常有用，如果您正在使用 TensorFlow 2.2 启动一个新项目，那么我建议您为您的项目创建一个`environment.yml`文件，然后将 Conda 环境作为子目录安装在项目目录中。

## 在项目目录中创建一个 environment.yml 文件

```
name: nullchannels:
  - conda-forge
  - defaultsdependencies:
  - cudatoolkit=10.1
  - cudnn=7.6
  - nccl=2.4
  - pip=20.0
  - python=3.8
  - tensorflow-gpu=2.2
```

## 在您的项目目录中安装 Conda 环境

```
conda env create --prefix ./env --file environment.yml --force
```

要了解更多 Conda“最佳实践”，我建议查看使用 Conda *管理您的数据科学项目环境的 [*。*](/managing-project-specific-environments-with-conda-406365a539ab?source=friends_link&sk=95468007215ddd7b6d47bb9d8522964e)*

# 使用 Conda (+pip)的好处

使用 Conda (+pip)安装 TensorFlow 有很多好处。最重要的是，不需要手动安装英伟达 CUDA 工具包，cuDNN，或英伟达集体通信库(NCCL)。将 TensorFlow 安装到虚拟 Conda 环境中，而不是在系统范围内安装，可以避免接触系统 Python，并允许您在机器上安装多个版本的 TensorFlow(如果需要)。最后，如果你选择使用我的首选方法，你也将拥有一个`environment.yml`,你的同事和同行可以使用它在他们的机器上重建你的 Conda 环境，或者你可以使用它在公共云上重建你的 Conda 环境，比如 AWS、GCP、微软 Azure。