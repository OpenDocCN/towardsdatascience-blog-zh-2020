# 我如何使用 Python 开发数据可视化的 Web 应用程序

> 原文：<https://towardsdatascience.com/how-i-developed-my-web-app-for-data-visualization-with-python-93555ad83c2d?source=collection_archive---------27----------------------->

## 关于如何开发数据仪表板并将其上传到网上的一些建议

![](img/400bf6882d5a6146059be8375a05d185.png)

Luke Chesser 在 Unsplash 上的照片

**简介**

我不是 web 开发人员，但作为一名数据分析师，我认为拥有一个用于数据可视化的 web 应用程序非常有用，您可以在其中部署您的数据并向世界展示您的结果。因此，我决定开发我自己的数据仪表板，我将在其中上传我未来的结果。你可以在这里看到它。为了更好地可视化图表，我建议你用你的电脑而不是智能手机来查看我的网络应用程序。

在这篇文章中，我想给你一些建议来创建你自己的数据可视化 web 应用程序。

关于开发它的技术，唯一的要求是了解标记语言 HTML 和编程语言 Python。

你不需要知道 CSS 或者 JavaScript。事实上，为了开发我的 web 应用程序，我使用 Flask 作为 web 框架，Bootstrap 作为前端框架。

如果你需要一些关于构建你的知识库的建议，你可以以我在 [GitHub](https://github.com/moryba/Web-App-Development-for-Data-Visualization) 上的知识库为例。

在本文中，我将重点介绍部署 Web 应用程序所需的所有步骤。

## 第 1 部分:在您的操作系统上安装 Anaconda

对于部署，我使用了一个 Linux 终端，我认为它非常适合开发。如果你的操作系统是 windows，你可以点击这个[链接](https://tldp.org/LDP/intro-linux/html/app2.html)查看对应的命令。

当您在终端中时，您必须做的第一件事是更新您的 Anaconda 版本。在这个[链接](https://repo.anaconda.com/archive/)中，你可以找到许多版本的列表。

使用 curl 命令下载 Anaconda 的最新版本。

```
curl -O [https://repo.anaconda.com/archive/Anaconda3-2020.02-Linux-x86_64.sh](https://repo.anaconda.com/archive/Anaconda3-2019.03-Linux-x86_64.sh)
```

按回车键直到结束。然后，对于许可协议，你得说 **yes** 。

当您到达您可以在下面看到的点时，按照第一个指示并按 Enter，或者您可以按照第三个指示并选择一个不同于默认位置的位置。

出局:

```
Anaconda3 will now be installed into this location:
/home/your_name/anacond - Press ENTER to confirm the location
  - Press CTRL-C to abort the installation
  - Or specify a different location below[/home/yor_name/anaconda3]>>>
```

当这个过程完成时，键入 **yes** 来确认 Anaconda 将被安装的路径。之后，您可以通过以下方式激活您的安装。

```
source ~/.bashrc
```

## 第二部分:创建您的虚拟环境

在这一部分，你必须创建一个虚拟环境。开始之前，请确保您已进入包含您的 web 应用程序文件的文件夹。所以使用这个命令来创建您的环境。

```
python3 -m venv my_environment
```

然后，您可以使用以下命令激活它:

```
source my_environment/bin/activate
```

现在你在你的环境里。在我个人的例子中，为了实现我的 web 应用程序，我用以下命令安装了 flask、pandas、Plotly 和 gunicorn:

```
pip install flask pandas plotly gunicorn
```

## 第三部分:上传到 Heroku

现在，你需要在这个[环节](https://www.heroku.com/)的云平台 Heroku 上注册。

然后，按照以下方式安装 Heroku 的所有命令行工具:

```
**curl** https://cli-assets.heroku.com/install-ubuntu.sh | sh
```

使用这个简单的命令登录并应用:

```
heroku login
```

你必须输入你的 Heroku 档案的用户名和密码。

下一步是创建一个文件，目的是告诉 Heroku 当你启动你的 web 应用程序时该做什么。在我的文件中，我写下了以下状态:

```
web gunigorn app:app
```

然后，指定将要需要的库。因此，您必须将所有信息放在一个名为 requirements.txt 的文件中，该文件允许 Heroku 知道在哪里查找。命令是:

```
pip freeze > requirements.txt
```

现在我们必须初始化存储库:

```
git init .git add .git commit -m 'first commit'
```

您可以通过以下方式指定您的电子邮件地址和姓名来进行配置:

```
git config user.email “username@something.com”git config user.name “Your Name”
```

最后一步是使用以下命令创建您的 web 应用程序。

```
heroku create name_of_your_web_app
```

使用以下命令推送文件:

```
git push heroku master
```

每次您想要更新 web 应用程序上的数据时，请记住遵循以下简单指示。

使用以下命令进入您的环境:

```
source my_environment/bin/activate
```

然后，使用这些命令更新您的数据仪表板

```
pip freeze > requirements.txtgit add .git commit -m “make some changes”git push heroku master
```

## 结论

这是一种简单的方式，允许你拥有自己的数据仪表板，并向世界上的每个人展示你的结果，我认为这是惊人的。

如果你在创建你的 web 应用程序时遇到了一些问题，请随时私信我或在评论中写给我。

我们也可以在我的电报群 [**初学数据科学**中取得联系。](https://t.me/DataScienceForBeginners)