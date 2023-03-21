# 在 Binder 和 Google Colab 上渲染 OpenAI 健身房环境

> 原文：<https://towardsdatascience.com/rendering-openai-gym-envs-on-binder-and-google-colab-536f99391cc7?source=collection_archive---------18----------------------->

## 关于解决一个乏味(但重要)问题的笔记

![](img/d4e906611d72c0c274fa6660f2dbb66f.png)

在 Google Colab 或 Binder 中远程渲染 OpenAI envs 很容易(一旦你知道了配方！).

我目前正在使用我的新冠肺炎强制隔离来扩展我的深度学习技能，完成了来自 [Udacity](https://www.udacity.com/) 的 [*深度强化学习纳米学位*](https://www.udacity.com/course/deep-reinforcement-learning-nanodegree--nd893) 。当在远程服务器上训练时，我几乎立刻就遇到了让我的模拟正确渲染的乏味问题。

特别是，让 [OpenAI](https://openai.com/) [Gym](https://gym.openai.com/docs/) 环境在远程服务器上正确渲染，比如那些支持流行的免费计算设施的服务器，如 [Google Colab](https://colab.research.google.com/notebooks/intro.ipynb) 和 [Binder](https://mybinder.org/) 比我预期的更具挑战性。在这篇文章中，我列出了我的解决方案，希望我可以节省其他人的时间和精力来独立解决这个问题。

# Google Colab 序言

如果你希望使用 Google Colab，那么这个部分就是为你准备的！否则，您可以跳到活页夹前言的下一节。

## 安装 X11 系统依赖项

首先，您需要安装必要的 [X11](https://en.wikipedia.org/wiki/X_Window_System) 依赖项，特别是 [Xvfb](https://www.x.org/releases/X11R7.7/doc/man/man1/Xvfb.1.xhtml) ，它是一个可以在没有显示硬件和物理输入设备的机器上运行的 X 服务器。您可以在 Colab 笔记本中安装系统依赖项，方法是在 install 命令前面加上一个感叹号(`!`)，它将在自己的 Bash shell 中运行该命令。

```
**!**apt-get install -y xvfb x11-utils
```

## 安装其他 Python 依赖项

现在您已经安装了 Xvfb，您需要安装一个 Python 包装器`pyvirtualdisplay`,以便在 Python 中与 Xvfb 虚拟显示交互。你还需要为 [OpenGL](https://www.opengl.org/) : [PyOpenGL](http://pyopengl.sourceforge.net/) 和 [PyOpenGL-accelerate](https://pypi.org/project/PyOpenGL-accelerate/) 安装 Python 绑定。前者是实际的 Python 绑定，后者是一组可选的 C (Cython)扩展，为 PyOpenGL 3.x 中的常见操作提供加速。

```
**!**pip install pyvirtualdisplay**==**0.2.* \
             PyOpenGL**==**3.1.* \
             PyOpenGL-accelerate**==**3.1.*
```

## 安装开放式健身房

接下来，你需要安装 OpenAI 健身房包。请注意，根据您感兴趣的健身房环境，您可能需要添加额外的依赖项。因为我要在下面的演示中模拟 LunarLander-v2 环境，所以我需要安装`box2d` extra 来启用依赖于 [Box2D](https://box2d.org/) 物理模拟器的健身房环境。

```
**!**pip install gym**[**box2d**]==**0.17.*
```

为了简单起见，我将所有的软件安装步骤收集到一个代码块中，您可以将其剪切并粘贴到您的笔记本中。

```
%%bash*# install required system dependencies*
apt-get install -y xvfb x11-utils*# install required python dependencies*
pip install gym**[**box2d**]==**0.17.* \
            *pyvirtualdisplay***==**0.2.* \
            *PyOpenGL***==**3.1.* \
            PyOpenGL-accelerate**==**3.1.*
```

## 在背景中创建虚拟显示

现在已经安装了所有需要的软件，您可以创建一个虚拟显示器(即在后台运行的显示器)，OpenAI Gym Envs 可以连接到该显示器进行渲染。您可以通过确认`DISPLAY`环境变量的值还没有被设置来检查当前是否没有显示。

```
**!***echo* *$DISPLAY*
```

下面单元格中的代码在背景中创建了一个虚拟显示，您的健身房 env 可以连接到该显示进行渲染。您可以随意调整虚拟缓冲区的`size`，但在使用 Xvfb 时必须设置`visible=False`。

这个代码只需要在你的笔记本上运行一次就可以开始显示。

使用 pyvirtualdisplay 启动 Xvfb 虚拟显示

在笔记本上运行上述代码后，您可以再次回显环境变量`DISPLAY`的值，以确认您现在有一个正在运行的显示。

```
**!***echo* *$DISPLAY # should now be set to some value*
```

为了方便起见，我将上述步骤收集到两个单元格中，您可以复制并粘贴到您的 Google Colab 笔记本的顶部。

# 活页夹序言

如果您希望使用[活页夹](https://mybinder.org/)，那么这一部分就是为您准备的！

## 不需要额外安装！

不像 Google Colab，使用 Binder 你可以烘焙所有需要的依赖项(包括 X11 系统依赖项！)放入绑定器实例所基于的 Docker 映像中。这些配置文件可以位于 Git repo 的根目录中，也可以位于一个`binder`子目录中(这是我的首选)。

## binder/apt.txt

需要定义的第一个配置文件是用于安装系统依赖项的`apt.txt`文件。您可以创建一个适当命名的文件，然后列出您想要安装的依赖项(每行一个)。经过一段时间的反复试验，我发现了下面的成功组合。

```
freeglut3-dev
xvfb
x11-utils
```

## binder/environment.yml

第二个配置文件是用于定义 Conda 环境的标准`environment.yml`文件。如果你对 Conda 不熟悉，那么我建议你看看我最近写的关于[*Conda*](/managing-project-specific-environments-with-conda-b8b50aa8be0e)*和 [*使用 Conda*](/managing-project-specific-environments-with-conda-406365a539ab) *管理特定项目环境的文章。**

```
*name: null channels:
  - conda-forge
  - defaults dependencies:
  - gym-box2d=0.17
  - jupyterlab=2.0
  - matplotlib=3.2
  - pip=20.0
  - pip:
    - -r file:requirements.txt
  - python=3.7
  - pyvirtualdisplay=0.2*
```

## *binder/requirements.txt*

*最后需要的配置文件是 Conda 使用的`requirements.txt`文件，用于安装任何额外的 Python 依赖项，这些依赖项不能通过使用`pip`的 Conda 通道获得。*

```
*PyOpenGL==3.1.*
PyOpenGL-accelerate==3.1.**
```

*如果你有兴趣学习更多关于 Binder 的知识，那就去看看 [BinderHub](https://binderhub.readthedocs.io/en/latest/) 的文档，这是 Binder 项目背后的底层技术。*

## *在背景中创建虚拟显示*

*接下来，您需要在背景中创建一个虚拟显示器，健身房 env 可以连接到该显示器进行渲染。您可以通过确认`DISPLAY`环境变量的值尚未设置来检查当前是否没有显示。*

```
***!***echo* *$DISPLAY**
```

*下面单元格中的代码在背景中创建了一个虚拟显示，您的健身房 env 可以连接到该显示进行渲染。您可以随意调整虚拟缓冲区的`size`,但在使用 Xvfb 时必须设置`visible=False`。*

*这段代码只需要在每个会话中运行一次就可以开始显示。*

*启动 Xvfb 虚拟显示与使用 Google Colab 完全一样！*

*运行上面的单元格后，您可以再次回显`DISPLAY`环境变量的值，以确认您现在有一个正在运行的显示。*

```
***!***echo* *$DISPLAY**
```

# *演示*

*只是为了证明上面的设置像宣传的那样有效，我将运行一个简短的模拟。首先我定义了一个`Agent`，它从一组可能的动作中随机选择一个动作，然后定义一个函数，它可以用来创建这样的代理。然后我将代码打包，模拟一个开放的人工智能健身房环境中的一集。注意，实现假设所提供的环境支持`rgb_array`渲染(不是所有的健身房环境都支持！).*

*模拟“代理”与支持 RGB 数组渲染的 OpenAI Gym 环境交互*

*目前，在模拟过程中似乎有相当数量的闪烁。不完全确定是什么导致了这种不良行为。如果你有任何改进的想法，请在下面留下评论。如果我找到一个好的解决方法，我一定会相应地更新这篇文章。*