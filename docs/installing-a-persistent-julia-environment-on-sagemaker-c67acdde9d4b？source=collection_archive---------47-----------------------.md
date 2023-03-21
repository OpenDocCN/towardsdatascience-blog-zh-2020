# 在 SageMaker 上安装持久化的 Julia 环境

> 原文：<https://towardsdatascience.com/installing-a-persistent-julia-environment-on-sagemaker-c67acdde9d4b?source=collection_archive---------47----------------------->

## 通过跨重启持久化 Anaconda 环境，充分利用 SageMaker

![](img/a8d8621bbff6b80afc0d36184e7edc3c.png)

照片由[爱丽丝·多诺万·劳斯](https://unsplash.com/@alicekat?utm_source=medium&utm_medium=referral)在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

SageMaker 是一个很好的环境，让数据科学家探索新的语言和方法，而不必太担心底层基础设施。只要您不需要定制现有的环境，一切看起来都很棒，而且运行正常。然而，一旦你想冒险离开人迹罕至的道路，你就会遇到一些意想不到的挑战。

我们的数据科学家通常有成本意识，当他们不使用 SageMaker 实例时，会很高兴地停止它们。不幸的是，SageMaker 不会持久保存任何写在`~/SageMaker`目录之外的数据。这意味着对 Anaconda 环境的更改不会在笔记本重启后持续。这非常令人沮丧。为了解决这个问题，我们需要在`~/SageMaker`文件夹中创建一个持久的 Anaconda 环境，并告诉 Julia 把它的包也放在那里。我们开始吧！

登录到您的 SageMaker 环境并打开一个新的终端会话。让我们创建一个新的空的 Anaconda 环境，位于`~/SageMaker`目录中。

```
conda create --yes --prefix ~/SageMaker/envs/julia
```

下载并解压 Julia 的最新版本

```
curl --silent [https://julialang-s3.julialang.org/bin/linux/x64/1.5/julia-1.5.0-linux-x86_64.tar.gz](https://julialang-s3.julialang.org/bin/linux/x64/1.4/julia-1.4.2-linux-x86_64.tar.gz) | tar xzf -
cp -R julia-1.5.0/* ~/SageMaker/envs/julia/
```

在我们开始为 Julia 安装软件包之前，我们需要确保 Julia 正在从正确的目录加载它的软件包。这也确保了您通过笔记本或终端安装的包也将存储在 SageMaker 实例的持久空间中。

```
mkdir -p ~/SageMaker/envs/julia/etc/conda/activate.decho 'export JULIA_DEPOT_PATH=~/SageMaker/envs/julia/depot' >> ~/SageMaker/envs/julia/etc/conda/activate.d/env.shecho -e 'empty!(DEPOT_PATH)\npush!(DEPOT_PATH,raw"/home/ec2-user/SageMaker/envs/julia/depot")' >> ~/SageMaker/envs/julia/etc/julia/startup.jl
```

我们现在可以激活环境并开始安装我们的依赖项，包括 IJulia。启动茱莉亚·REPL，安装并激活伊茱莉亚。

```
juliausing Pkg
Pkg.add("IJulia")
using IJulia
```

退出 REPL (Ctrl + D)并打开 Jupyter 或 JupyterLabs。新的 Julia 1.5.0 内核现在应该是可见的，您已经准备好了。

重启 SageMaker 实例后，您会注意到 Julia 内核已经消失了。要恢复内核，只需执行

```
conda run --prefix ~/SageMaker/envs/julia/ julia --eval 'using IJulia; IJulia.installkernel("Julia")'
```

您可以随心所欲地将这个脚本注册为笔记本生命周期配置，每当 SageMaker 实例启动时就会自动执行。你可以在 https://docs . AWS . Amazon . com/sage maker/latest/DG/notebook-life cycle-config . html 找到更多关于如何设置的详细信息。