# 设置您自己的深度学习沙盒:快速指南

> 原文：<https://towardsdatascience.com/setup-your-own-deep-learning-sandbox-quick-guide-9f09e140baef?source=collection_archive---------42----------------------->

## 如何使用 Google Cloud、Jupyter 和 Visual Studio 代码轻松设置一个强大的深度学习沙盒，并将成本控制在每月几美元(第一笔 300 美元在 Google 上)。

![](img/fd54948f0980d96b0ce460529bf4e215.png)

摄影:[卡斯帕·卡米尔](https://unsplash.com/@casparrubin)鲁宾在 [Unsplash](https://unsplash.com)

像许多数据科学爱好者一样，我投身于这一领域，利用每一分钟的空闲时间学习 DS 生态系统——即 Python、Jupyter、NumPy、Pandas、Scikit-learn、Matplotlib 和少量 Seaborn。以及所有强大的 Unix 工具——VI、less、sort、cut、grep、cat、tail、head 等。–您拥有一个极其高效的框架来获取、清理和分析数据集、设计功能、运行回归模型以及构建专业视觉效果。然而，一旦你的好奇心(或工作)将你拉向“机器学习”世界的“深度学习”一端，你会很快发现——就像我一样——即使是高端的 Macbook Pro 也没有足够的肌肉来运行哪怕是最基本的卷积神经网络模型(简称 CNN)。通过学习杰瑞米·霍华德和瑞秋·托马斯的 fast.ai 课程，我很快就明白了这一点。如果您像我一样，并且您的数据科学培训资金来自您自己的口袋，那么我有一个便宜、简单但功能强大的解决方案供您使用。

首先，我承认我喜欢在我的笔记本电脑上运行所有我能运行的东西。然而，对于一些事情，我的笔记本电脑就是不行，例如，对于我的音乐创作爱好，我在我的地下室有一个增强的英特尔 i7 盒子，它运行一个音乐库服务器。但是对于训练 DL 模型来说，i7 和大量内存对你来说没什么用。你需要 GPU。而 GPU 又很贵。但是租起来不贵。有了现在市面上可以买到的云选项自助表，我的新 MO 就变成了， ***不买，租*** 。

通过谷歌搜索和反复试验，我建立了一个基于 Jupyter 的客户端-服务器沙箱，使用我的 Macbook Pro 和 Visual Studio 代码作为客户端，Jupyter 服务器运行在谷歌云上的高端 Linux 实例上。结果呢？我的 DL 模型训练现在可以在几秒钟内完成。在我的 Macbook Pro 上？嗯，运行 30 分钟后，风扇不停地运转，我结束了这个过程。你明白了吗？

***注:*** *这种方式在 Google cloud 中使用完整的 Linux 服务器。还有其他成本更低的途径，例如 Google Colab 和 Kaggle 内核。然而，通过这种方法，你获得了配置的灵活性，300 美元的信用(如果节省使用将持续一段时间)，以及一些使用 Google 云平台和命令行界面的经验。*

# 安装服务器

## 创建 GCP 帐户并获得 300 美元

第一步是在谷歌云平台(GCP)上创建一个账户。我将假设你已经明白了这一点，并登录到 GCP 控制台。接下来，检查以确保您获得了 300 美元的信用。在左上角点击菜单，选择*计费***→T5*总览*。在概览的右下角，您应该会看到促销积分余额。应该是 300 美元。**

![](img/1254a8da39d059f05f2d2299cae8aeb7.png)

## 创建您的虚拟机服务器

GCP 现在有一项专门为运行 Jupyter 笔记本电脑配置虚拟机服务器的服务。点击 GCP 菜单(左上角)并向下滚动到人工智能部分。然后选择 *AI 平台→笔记本*。

![](img/63e9770381260e3f37acb817e7ec7bf8.png)

从笔记本实例屏幕选择顶部的*新实例*。这个菜单有许多选项，包括一些测试和实验选择。我会选择 PyTorch 1.4 和一个 NVIDIA Tesla T4 GPU。

![](img/dcaf05ecb8e4002f31f67a2299aac25c.png)

下一个屏幕显示选项。在这个练习中，我将使用默认值。*自定义*按钮允许您向引导驱动器添加更多磁盘空间，添加 GPUs RAM 等。默认费用为每小时 0.382 美元。Google E2 实例在使用一分钟后按秒计费。这个默认配置每小时运行 38.2 美分。那是 785 小时的免费使用。

![](img/6adf43646e016fddc1ebdacdc956526b.png)

一旦你点击 *Create，* Google 将花费几分钟来配置你的虚拟机实例。完成后，您将看到如下屏幕。

![](img/11888e7ab0ceb674a5bcaa62f21c4899.png)

现在，您拥有了一个功能齐全的 Jupyter 笔记本，它具有强大的 GPU 来支持 DL 模型构建。点击实例名称旁边的*打开 JUPYTERLAB* 。您的浏览器中将会打开一个新标签页，并打开一个 Jupyter 笔记本。界面非常简单。您可以浏览文件系统、创建 Git 存储库或启动终端会话。

![](img/681c17d7abb5052def37b4b189ee29c4.png)

在这个阶段，你已经有了一个功能齐全、功能强大的 DL 沙箱。然而，这不是一种经济有效的工作方式。您的新虚拟机实例每秒钟都在消耗您运行该实例的 300 美元。我更喜欢在我的 macbook 上本地完成我所有的数据收集、特征工程和初始回归工作。然后，当我清理和准备好我的数据集，并且我正处于尝试和适应 DL 模型的阶段时，我启动我的虚拟机实例，连接，运行我的模型，获得我的结果，然后关闭实例。为了有效地执行这个工作流，我现在将介绍几个步骤。

## 停止并启动您的虚拟机实例(在继续之前执行此步骤)

返回笔记本控制台并关闭您的实例。

![](img/faac19ea4ae867b3ba989fb67c944b2d.png)

> T **ip:** 转到账单部分，在 ***预算&提醒*** 下创建一个预算，当你达到 1 美元的使用量时，它会向你发送电子邮件。

## 为远程 HTTP/HTTPS 访问配置虚拟机实例

要打开对 VM 实例的远程访问，您需要做两件事。

**第一步**。打开对虚拟机实例的 HTTP 访问。从主控制台菜单中的*计算*部分，选择*计算引擎→虚拟机实例*。

![](img/f108448e8596569e61f3e2551ef0d645.png)

然后从列表中单击您的实例。将显示您的实例的详细信息。我们需要在这里做两个改变。点击顶部的*编辑*，向下滚动并选中防火墙部分的两个 HTTP 框，然后滚动到底部并点击*保存*。

![](img/2e676aa9d9c8cb6a73efbcf78b61213a.png)

**第二步**。遵循下面的菜单，然后单击防火墙控制台顶部的*创建防火墙规则*。

![](img/123f25ecc2ddfde9a6f050264fae55bf.png)

在以下两个屏幕中输入注释字段。

![](img/057eebb0c25614822036bb75a5cbe52c.png)![](img/f1e13e1286c7f4d893a74a5159edae66.png)

您的防火墙规则现在应该处于活动状态。如果不是，您将在接下来的步骤中知道。

**第三步。**停止您的虚拟机实例。返回笔记本控制台，点击实例旁边的*复选框*，并点击*停止*。

![](img/faac19ea4ae867b3ba989fb67c944b2d.png)

## 使用 Google Cloud SDK 管理虚拟机实例

**步骤一。**第一步是在你的本地计算机上安装 SDK。下面是谷歌为 MacOS、Windows 和 Linux 下载和安装 SDK 的说明。确保您遵循所有步骤，包括将 SDK 放在您的路径中。

**第二步。从终端运行以下命令，初始化 SDK。**

```
> gcloud init
...> You must log in to continue. Would you like to log in (Y/n)?
```

系统将提示您登录。当您按下 ***Y*** 和 ***回车键*** 时，您将被重定向到一个谷歌网站以登录您的帐户。成功登录后，您应该会返回到终端会话，系统会提示您选择一个计算区域。

```
> Do you want to configure a default Compute Region and Zone? (Y/n)?
```

选择 ***Y*** 。在前面的一个屏幕截图中，我建议记下您的实例名和创建它的地区。在列表中找到显示的区域，并输入其索引号。完成后，运行以下命令。

```
> gcloud config list
```

您应该会在终端窗口中看到如下内容。

![](img/9fd477bf1ce991e5b5f20ed730f025a1.png)

**第三步**。在我们离开虚拟机实例之前，我们将其关闭。现在让我们使用 SDK 重新启动它。下面是命令。用实例名替换**实例名**。

```
> gcloud compute instances start **inst_name**
```

![](img/45f6e5736a4169d490551a4b869308e5.png)![](img/63f13dc2cbc6652b4ac1d8ab2d3895ff.png)

现在让我们练习停止实例。下面是命令。

```
> gcloud compute instances stop **inst_name**
```

这可能需要几分钟，所以让它运行吧。在小旋转完成后，你应该得到这个结果。

![](img/8d1133833d026d5938ce5a286bdb5c3d.png)

一旦你看到 ***…done*** 然后再次启动你的实例，让我们继续。

**第四步。**连接到实例并完成服务器端设置的时间。输入以下命令并用您的实例替换 **inst_name** 。

```
> gcloud compute ssh **inst_name**
```

系统将提示您输入密码。这是您最初创建的帐户密码。接下来，您将看到如下屏幕。

![](img/02076f25b932dffd461eb165c096439d.png)

好了，我们快到了。只需对 Jupyter 进行一些配置更改，然后我们将能够连接 Visual Studio 代码并运行模型。**耶！😎**

## 配置 Jupyter 笔记本服务器

**第一步。**为笔记本创建文件夹。这是 Jupyter 存储笔记本文件的地方。我还建议创建两个子文件夹，当我们运行下面的示例模型时，它们的用途就变得很清楚了。

```
> mkdir jnotebooks
> mkdir jnotebooks/data
> mkdir jnotebooks/pics
```

**第二步。**创建 Jupyter 配置。这就产生了一个**。jupyter** 文件夹下你的根目录。

```
> jupyter notebook --generate-config
```

现在是一个有点棘手的部分。我们需要在文件**中配置一些 Jupyter 参数。jupyter/jupyter _ notebook _ config . py**。我推荐使用 **vim** 。在编辑文件之前，通过执行 **pwd** 记下你的根路径。这是参数。所有这些参数都已经在文件中。但是，它们被注释掉了。它们需要取消注释和修改。

```
c.NotebookApp.ip = '*'
c.NotebookApp.port = 8888
c.NotebookApp.notebook_dir = '/home/username/jnotebooks'
c.NotebookApp.open_browser = False
```

记得我们创建了 **jnotebooks** 文件夹。输入 **notebook_dir** 参数的完整路径。

这里有一个简短的你需要的命令清单。

![](img/d4ccd0733c40ab8f36688c207b2e5878.png)

**第三步。**设置一个 Jupyter 密码。这是可选的，但是强烈推荐，因为我们刚刚打开了 HTTP 访问。从命令行输入以下内容，然后输入您的密码。你必须在每次从 VSC 访问这个服务器时输入这个，所以我建议不要在这里走极端。

```
> jupyter notebook password
```

**第四步。这里是最后一步。让我们启动 Jupyter 笔记本服务器。下面是命令。**

```
> jupyter-notebook
```

## 还有**哒哒**！您应该会看到如下所示的内容。

![](img/982593985ed05edebcd926e2f253b528.png)

如果你的屏幕与此不太相似，那么按下 **Control-c** 终止这个过程，并返回检查 Jupyter 配置文件中的四个参数。

# 安装客户端

我是 Visual Studio 代码的忠实粉丝。微软在一段时间前开源了它，从那以后它迅速发展成为一个非常用户友好和功能强大的编辑器。我觉得很酷的是，它现在完全支持 Jupyter 笔记本。我个人不喜欢在工作模式下用浏览器运行笔记本。我更喜欢我用于 Python 和 Java 的真正的代码编辑器**(希望 Swift 很快会得到支持)**。在 VSC，我可以在笔记本上建立我的模型，然后轻松地导出到 Python 文件中，开始将我的项目生产化，所有这些都在同一个编辑器中完成。闲话少说。让我们连接到我们的服务器并训练一个模型。

**第一步**。安装和设置 Visual Studio 代码。如果您使用的是 Anaconda 发行版，那么您就万事俱备了。蟒蛇和 VSC 合作得很好。否则，单击此处的[获取您的操作系统的最新版本。VSC 本机支持 Jupyter，所以没有什么额外的需要安装。](https://code.visualstudio.com/download)

**第二步。**打开 VSC，创建一个新的 Jupyter 笔记本。最简单的方法是使用命令浏览器。在 MacOS 上命令是 **shift+command+p** 。然后输入`Python: Create New Blank Jupyter Notebook`

**第三步**。连接到您的服务器。当你启动你的服务器时，你记下你的外部 IP 地址了吗？你在这里会需要它。再次通过 **shift+command+p.** 进入命令浏览器，然后输入命令`Python: Specify local or remote Jupyter server for connections`

![](img/824835c5e7a328746ce8bf7f9a44d5f4.png)

你接下来会看到这个。选择现有的。

![](img/91ea64ad1bcbdd3ddd6bab08b1ffe852.png)

这里是您需要虚拟机实例的外部 IP 地址的地方。

![](img/e0cef0bbe9ea5c7523239725b84f9a32.png)

在上面输入你的 http 地址并按下回车键后，VSC 有可能需要重启。如果发生这种情况，则关闭 VSC，再次启动它，然后按照此**步骤 3** 返回至此。键入一些测试 Python 代码并执行单元。VSC 将代码发送到服务器，并显示返回的结果。然后你会在右上角看到你的 Jupyter 服务器地址，如下图所示。

![](img/afb85ac83657cac19430e8247e1b359f.png)

成功了！现在是时候享受真正的乐趣了！

# 有趣的时间——训练一个 DL 模型

我相信每个数据科学黑客、程序员和专业人士都熟悉 Kaggle 的[庞大数据集。这就像数据科学的“你好世界”。我也摆弄过这个数据集。我只是运用了基本的推理，比如“假设所有的人都会死”。只用几行代码就能得到一个普通的分数。为了好玩，我做了很多特性工程，然后应用了所有的 Scikit-learn 模型和 Xgboost。然后，为了真正的乐趣，我想我会应用一个 DL 模型。](https://www.kaggle.com/c/titanic)[我的型号](https://github.com/tmovai/Titanic)在 Github 上有。我建议跟随查看工作流程。

**第一步**。从 Github [这里](https://github.com/tmovai/Titanic)获取模型。

**第二步。**将 VSC 切换到本地 Python 实例。 **shift+command+p** 后跟`Python: Specify local or remote Jupyter server for connections`然后从菜单中选择**默认**。这将强制重新加载 VSC **(希望这种重新加载的需求在未来会消失)**。

![](img/da88924a71f3a28214dfb2c17dc812c3.png)

**第三步。**在 VSC 打开文件**titanic _ feature _ creation . ipynb**，执行每个单元格。最后一个单元格将把文件**titanic _ train _ wrangled . CSV**和**titanic _ test _ wrangled . CSV**写出到名为 **data** 的子文件夹中。这两个文件必须复制到您的虚拟机实例中。

**步骤四。**将数据文件复制到虚拟机实例。在从 Github 获取的 titanic 文件夹中，执行以下命令。用实例名替换 **inst_name** 。

```
> gcloud compute scp ./data/*.csv **inst_name**:~/jnotebooks/data/
```

**第五步**。将 VSC 连接到您的服务器。s**shift+command+p**后跟`Python: Specify local or remote Jupyter server for connections`然后从菜单中选择您的服务器。这将迫使 VSC 重装上阵。

![](img/14624432a74dd88bef88eba4e0c2bab2.png)

**第六步。**打开文件**titanic _ model _ evaluation _ CNN . ipynb**。请注意，在第一个单元格中，读取的数据文件。您在本地创建了它们，然后将它们转移到您的 VM 实例。

![](img/e945c7989ab8b2b94daf0fe65c0524d7.png)

**第七步。**执行所有单元格，生成一个 **submission.csv** 文件。

## 注意事项

*免责声明:*标题为**的单元格将特征转换为方形 PNG，**只是将两个数据文件的每一行(或记录)转换为图像的一种简单方法。有许多更好的方法来翻译 DL 模型的数据。在这次演示中，我想保持最基本的简单。所有创建的图像都存储在服务器上名为 **images_titanic** 的子文件夹中。

![](img/45960ffeb87f958b844e726f2e2cff42.png)

# 看看我们的排名吧！

**步骤 1。**现在让我们检索在下面显示的最后一个单元格中创建的文件 **submissions.csv** 。

![](img/af8dd0838d5dc4ce1631a45448dc9bbf.png)

下面是命令。再次用实例名替换 inst_name。

```
> gcloud compute scp **inst_name**:~/jnotebooks/data/submission.csv .
```

**第二步。**访问 Kaggle 的泰坦尼克号网站[在这里](https://www.kaggle.com/c/titanic)提交你的作品。

# 我们表现如何？

78%没问题。还不错。然而，如果你是一个真正的 DS 实践者，你会对此一笑置之。这是过度工程化的一个极端例子。但这不是最佳深度学习模型工程中的一项练习。这都是关于如何建立一个沙箱，这样你*然后*就可以专注于构建最佳模型。

最后一件事。

![](img/3e403f04e33646ccd1d9bad082bf37b9.png)