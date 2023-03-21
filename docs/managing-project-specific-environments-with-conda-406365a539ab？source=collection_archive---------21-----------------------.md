# 使用 Conda 管理您的数据科学项目环境

> 原文：<https://towardsdatascience.com/managing-project-specific-environments-with-conda-406365a539ab?source=collection_archive---------21----------------------->

## 为眼光敏锐的数据科学家提供的一组最小的“最佳实践”。

![](img/2ec9d1cef865dcfa5e82035396d50bd2.png)

有眼光的数据科学家的环境和包管理器。来源:[https://docs.conda.io/en/latest/](https://docs.conda.io/en/latest/)

# Conda“最佳实践”

在这篇文章中，我详细介绍了使用 [Conda](https://docs.conda.io/en/latest/) 管理我在自己的数据科学工作中使用的特定于项目的环境的“最佳实践”的最小集合。本文假设您对 Conda 有基本的了解，并重点介绍了使用 Conda 管理数据科学项目环境的一小组“最佳实践”。如果你没听说过康达或者刚开始使用康达，那么我推荐你看一下 [*康达*](/managing-project-specific-environments-with-conda-b8b50aa8be0e) *入门。*

# TLDR；

下面是使用 Conda 管理特定项目软件堆栈的基本方法。

```
(base) $ mkdir project-dir
(base) $ cd project-dir
(base) $ nano environment.yml # create the environment file
(base) $ conda env create --prefix ./env --file environment.yml
(base) $ conda activate ./env # activate the environment
(/path/to/env) $ nano environment.yml # forgot to add some deps
(/path/to/env) $ conda env update --prefix ./env --file environment.yml --prune # update the environment
(/path/to/env) $ conda deactivate # done working on project (for now!)
```

# 新项目，新目录

每一个新项目(不管多小！)应该存在于自己的目录中。开始组织你的项目目录的一个好的参考是[科学计算的足够好的实践](https://journals.plos.org/ploscompbiol/article?id=10.1371/journal.pcbi.1005510)。

```
mkdir project-dir
cd project-dir
```

# 新项目，新环境

现在您已经有了一个新的项目目录，您可以为您的项目创建一个新的环境了。我们将分两步进行。

1.  创建一个描述软件依赖关系的环境文件(包括具体的版本号！)对于项目来说。
2.  使用新创建的环境文件来构建软件环境。

这里是一个典型环境文件的示例，可用于运行使用 [PyTorch](https://www.pytorch.org/) 开发的深度学习模型的 GPU 加速、分布式训练。

```
name: nullchannels:
  - pytorch
  - conda-forge
  - defaultsdependencies:
  - cudatoolkit=10.1
  - jupyterlab=1.2
  - pip=20.0
  - python=3.7
  - pytorch=1.5
  - tensorboard=2.1
  - torchvision=0.6
  - torchtext=0.6
```

一旦在项目目录中创建了一个`environment.yml`文件，就可以使用下面的命令在项目目录中创建一个名为`env`的子目录。

```
conda env create --prefix ./env --file environment.yml
```

# 激活环境

激活环境对于让环境中的软件工作良好(或者有时根本不工作)是必不可少的！).环境的激活做两件事。

1.  为环境向`PATH`添加条目。
2.  运行环境可能包含的任何激活脚本。

第 2 步尤其重要，因为激活脚本是包如何设置它们运行所需的任意环境变量的。

```
conda activate ./env # activate the environment
(/path/to/env) $ # prompt indicates which environment is active!
```

# 更新环境

您不太可能提前知道哪些包(和版本号！)你将需要用于你的研究项目。例如，情况可能是…

*   您的一个核心依赖项刚刚发布了一个新版本(依赖项版本号更新)。
*   您需要一个用于数据分析的附加包(添加一个新的依赖项)。
*   您已经找到了一个更好的可视化包，不再需要旧的可视化包(添加新的依赖项并删除旧的依赖项)。

如果在您的研究项目过程中出现这些情况，您需要做的就是相应地更新您的`environment.yml`文件的内容，然后运行下面的命令。

```
conda env update --prefix ./env --file environment.yml --prune
```

或者，您可以使用以下命令从头开始简单地重建环境。

```
conda env create --prefix ./env --file environment.yml --force
```

除非从头开始构建环境需要大量的时间(这应该非常罕见！)当我添加(或删除)依赖项时，我几乎总是从头开始重建我的环境。

# 停用环境

当您完成项目时，最好停用当前环境。要停用当前激活的环境，使用如下的`deactivate`命令。

```
conda deactivate # done working on project (for now!)
(base) $ # now you are back to the base environment
```

# 有兴趣了解更多信息吗？

有关使用 Conda 为您的数据科学项目管理软件堆栈的更多详细信息，请查看我为[Carpentries 孵化器](https://carpentries.org/involved-lessons/)撰写的[Conda(数据)科学家简介](https://carpentries-incubator.github.io/introduction-to-conda-for-data-scientists/)培训材料。这些课程材料正在积极开发中，因此请随时提出问题或提交 PRs！