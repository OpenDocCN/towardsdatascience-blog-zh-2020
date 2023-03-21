# 在 Heroku.com 部署 Julia 项目

> 原文：<https://towardsdatascience.com/deploying-julia-projects-on-heroku-com-eb8da5248134?source=collection_archive---------31----------------------->

![](img/50a8169c3c507ddb8517dc74f6b129ac.png)

照片由[乔纳森派](https://unsplash.com/@r3dmax?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)在 [Unsplash](https://unsplash.com/s/photos/code?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) 上拍摄

嗨！今天我将向你展示如何在云应用平台 Heroku 上用 Julia 编程语言部署项目。在本例中，我将部署一个 [Dashboards.jl](https://github.com/waralex/Dashboards.jl) 文件进行演示。

这篇博客的所有内容都来自我的 GitHub 库，最终产品是这里的。请稍后查看示例:)

在我们开始之前，您将需要以下内容:您的 Dashboard 文件、一个 Heroku 帐户、一个 Julia 安装，以及安装的 [Heroku 命令行界面](https://devcenter.heroku.com/articles/heroku-cli#download-and-install) (CLI)。对于这个例子，我将在这里使用来自[的仪表板文件](https://github.com/waralex/DashboardsExamples/blob/master/dash_tutorial/5_interactive_graphing_2.jl)。

现在，您可以创建一个新文件夹来存储部署应用程序所需的所有文件。这里，我将使用文件夹`juliadash`作为示例目录。之后输入`julia`进入茱莉亚。

```
mkdir juliadash
cd juliadash
julia
```

一旦你在 Julia 中，你将需要创建一个新的项目，包含`Project.toml`和`Manifest.toml`文件，包含你需要使用的软件包和注册表的版本信息。以下代码使用这两个文件激活您的新环境，并添加您的应用程序所需的包，然后将所有信息存储在这两个文件中。

```
using Pkg; Pkg.activate(".")
Pkg.add("Dashboards")       #Do this as many times as you need to, changing the string (containing the package name) every time.
Pkg.add("HTTP")
⋮
```

完成后，您可以关闭 cmd/终端窗口。

接下来，我们将创建一个`Procfile`，这是一个空白文件，Heroku 使用它来指导您的文件的部署。您需要创建名为 Procfile 的文件，并将以下代码行复制并粘贴到该文件中。

```
web: julia --project app.jl $PORT
```

您需要用您的应用程序的启动文件的文件名替换`app.jl`。然后，您可以将文件保存到您的应用程序所在的目录中。

现在，您的目录应该有您的仪表板文件、`Project.toml`、`Manifest.toml`以及`Procfile`。

由于您通常将仪表板应用程序托管在本地主机的一个端口上，您将需要替换您的主机方法和编号，因为 Heroku 将应用程序部署在一个随机端口上。如果您使用的是 HTTP.jl，您需要用以下内容替换您的端口:

主持方式:用`"0.0.0.0"`代替`HTTP.Sockets.localhost`

主机端口:将`8080`替换为`parse(Int,ARGS[1])`

(如果你想知道，`parse(Int,ARGS[1])`对应的是`Procfile`中的`$PORT`。)

这是我的 HTTP serve 函数的样子:(更多细节，请看我在这个存储库中的代码。)

```
HTTP.serve(handler, "0.0.0.0", parse(Int,ARGS[1]))
```

完成后，您可以在 cmd 或终端上打开一个新窗口，并通过键入以下内容登录 Heroku CLI 帐户:

```
heroku login
```

它应该会打开一个浏览器窗口，让您输入凭据。完成登录并关闭浏览器窗口后，您会发现自己已经登录。现在，您可以再次进入您的目录并部署您的应用程序。

请在下面的代码中将`my-app-name`替换为您希望调用应用程序的名称！

```
cd juliadash
git init
HEROKU_APP_NAME=my-app-name
heroku create $HEROKU_APP_NAME --buildpack https://github.com/Optomatica/heroku-buildpack-julia.git
git heroku git:remote -a $HEROKU_APP_NAME
git add .
git commit -am "make it better"
git push heroku master
```

一旦该过程完成，应用程序将被部署并投入使用！

这可能需要一段时间，但一旦完成，您就可以运行以下命令在浏览器上打开您部署的应用程序！

```
heroku open -a $HEROKU_APP_NAME
```

希望这有助于你了解更多关于使用 Heroku 部署应用程序的知识。这种方法不仅限于仪表板应用程序。

如果你想看看我做的最终产品，点击[https://juliadash.herokuapp.com/](https://juliadash.herokuapp.com/)了解一下！

**引用:**该库中的`dashex.jl`文件来自这里的！