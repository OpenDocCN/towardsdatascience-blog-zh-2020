# 如何将屏幕日志添加到 Flask 应用程序中，并将其部署在 AWS Elastic Beanstalk 上

> 原文：<https://towardsdatascience.com/how-to-add-on-screen-logging-to-your-flask-application-and-deploy-it-on-aws-elastic-beanstalk-aa55907730f?source=collection_archive---------8----------------------->

## *提示:不要忘记配置 Nginx 反向代理服务器*

2019 年底 [Deeplearning.ai](https://info.deeplearning.ai/the-batch-companies-slipping-on-ai-goals-self-training-for-better-vision-muppets-and-models-china-vs-us-only-the-best-examples-proliferating-patents) 报道称，只有 22%使用机器学习的公司实际部署了模型。大多数公司不会超越概念证明，通常是通过 Jupyter 笔记本中的模型。因此，许多公司正在雇用机器学习工程师，他们可以建立机器学习模型，并将其投入生产。

数据科学家至少应该熟悉一些生产模型的方法。为此，数据科学家工具箱中最重要的工具是 Docker。 [Docker 是一个容器服务，使您能够在本地机器之外部署模型或应用程序](https://medium.com/vantageai/taking-python-beyond-your-local-machine-with-docker-89793982865f)。例如，在亚马逊网络服务(AWS)或谷歌云平台(GCP)上运行它。有几种框架可以在这些 Docker 容器中构建应用程序并为您的模型提供服务。正如许多数据科学家已经知道 Python 一样， [Flask](https://flask.palletsprojects.com/en/1.1.x/) 很容易上手。此外，Flask 为您提供了构建(简单的)用户界面的机会，因此您的用户将能够与您的模型进行交互，而不必学习如何使用命令行界面或发出 API 请求。

在本实践教程中，我将向您展示如何在 AWS Elastic Beanstalk 上的 Docker 容器中部署一个简单的 Flask 应用程序，以及如何添加日志记录功能，以便您的用户能够看到幕后发生的事情。该应用程序不会包含任何机器学习模型，但您可以轻松地自行扩展它。当我第一次部署这个解决方案时，我遇到了一些麻烦，因为我必须在 AWS 上配置(反向)代理服务器。在本教程的最后一部分，我会告诉你如何做到这一点。

# 基础知识

![](img/7245c9288de2f876e735d2bb37b9f66b.png)

我们的 Flask 应用程序的文件夹结构

首先，我们设置了 Flask 应用程序的基础。它包括一个用于所有 Flask 代码的 **app.py** 文件，一个用于格式化索引页面结构的静态**index.html**文件和一个 css 样式表( **custom.css)**

app.py 只有 7 行代码。我们首先初始化一个 Flask 应用程序类，并定义静态和模板文件夹。然后我们定义一条路线('/')，并告诉应用程序它应该呈现**index.html。**最后一行告诉应用程序在端口 5000 上暴露自己。**主机**参数被设置为 0.0.0.0，以便稍后在 AWS Elastic Beanstalk 上部署。

HTML 和 CSS 文件也很简单，为了完整起见，在下面演示。我们用一些样式选项定义了一个 logging_window 类，稍后将包含日志记录。

我们现在可以第一次运行我们的应用程序，看看它是什么样子的。如你所见，仍然没有什么特别的。

![](img/98512ad0a3baef38a7016ae65583b986.png)

应用程序第一个版本的屏幕截图

# 添加日志记录功能

如前所述，显示 Python 进程的日志将使应用程序的工作变得更加容易。例如，用户可以看到一个进程是停滞还是仍在运行，他们应该有耐心。

将日志添加到我们的应用程序非常简单。它需要一个助手函数 **flask_logger，**,(在本例中)每秒返回一个编码字符串形式的当前日期时间。此外，我们添加了一个新的路由('/log_stream ')，它将使用我们的 **flask_logger** 函数的输出返回一个 Flask 响应类。另外，不要忘记为这个例子导入 datetime。

如果我们转到新创建的路由(将在[https://localhost:5000/log _ stream 公开)，](https://localhost:5000/log_stream),)我们现在将看到以下内容。格式不是很好，但它每秒都返回日期时间。

![](img/4735ec0e2de159ed24e29e776eb02e1f.png)

## 记录您的日志

由于我们现在能够每秒显示一次输出，我们实际上可以显示我们的日志。为此，我们必须改变我们的 **flask_logger** 函数。首先，我们必须配置我们的记录器。在这个例子中，我将使用来自 [**loguru**](https://github.com/Delgan/loguru) 的记录器，但是你可以使用任何你喜欢的记录器。记录器将被配置为将所有日志写入静态文件夹中的 job.log 文件。 **flask_logger** 将被配置为每秒读取日志文件并返回日志。此外，日志文件将在 25 次迭代后被清除。

这将导致更好的格式化日志记录。请注意，loguru 记录器(在所有 python 进程中)记录的所有信息都将显示出来，因为在配置记录器后，这些信息都将写入 **job.log** 文件。因此，如果你在训练过程中有一行代码**logger . info(' Model is training ')**，它也会显示在我们的 logger 中。

![](img/1c5f1dc425373ca6ac8da7cd8d74c5eb.png)

从 job.log 文件读取的格式化日志记录

**包括登录我们的索引页面**

部署应用程序之前的最后一步是在我们创建的 index.html 中包含日志记录。这相当简单，但是包含了一点 JavaScript。我们创建一个函数，它在页面加载后启动，向我们的 **/log_stream** 路由发出 GET 请求，并将响应写入一个 id 为‘output’的 HTML 元素。整个 HTML 文件将如下所示:

如您所见，我们现在有了一个应用程序，它记录了我们所有的 python 进程，并向我们的用户显示它们。

![](img/a42a1fdf3e1871093413cf001e05d672.png)

我们的 index.html 文件中包含日志流

# 部署到弹性豆茎

现在我们可以将我们的简单应用程序部署到 AWS Elastic Beanstalk，这样任何人都可以访问它。Elastic Beanstalk 是一个所谓的“编排服务”,它不仅负责我们应用程序的部署，还负责设置服务器实例，负责负载平衡(如果多个实例已被实例化，则在您的计算资源上分配任务，以使整个过程更有效),以及监控您应用程序的健康和状态。

对于这一步，我们需要在我们的项目的根文件夹中添加两个文件:一个 Dockerfile 来封装应用程序，另一个 requirements.txt 包含应该安装在这个容器中的所有包。将 **pip 冻结**命令的结果复制粘贴到 requirements.txt，并按如下方式设置 Dockerfile。

现在是时候让奇迹发生了。有几种方法可以将您的应用程序部署到 AWS Elastic Beanstalk(假设您已经有一个 AWS 帐户。如果您还没有，请在 aws.amazon.com 上注册 12 个月的免费层访问。最方便的方法就是安装弹性豆茎的[命令行接口](https://docs.aws.amazon.com/elasticbeanstalk/latest/dg/eb-cli3-install.html)。安装后，使用终端导航到项目的根文件夹。随后调用 **eb init** 和 **eb create** 并回答提示的问题。Elastic Beanstalk 会自动识别您的项目文件夹中有一个 Dockerfile，并开始构建环境。等待几分钟(通过 AWS 的管理控制台检查进度)，然后您可以通过在初始化过程中定义的 URL 访问您的应用程序。如果您通过 AWS 管理控制台导航到您的 Elastic Beanstalk 环境，也可以找到这个 URL。

# Nginx

但是，如果您访问应用程序的 URL，您将会看到没有日志记录出现。25 秒后，所有日志同时出现。要解决这个问题，我们必须配置 Nginx 反向代理服务器，如果创建了弹性 Beanstalk 环境，默认情况下会实例化这个服务器。配置这个服务器听起来可能很吓人，但实际上非常简单。为了理解我们正在做的事情，我来解释一下问题从何而来。

如前所述，Nginx 反向代理服务器是在创建弹性 Beanstalk 环境时启动的。该代理服务器旨在将您的应用程序映射到您环境的负载平衡器。然而，Nginx 的一个特性是，它缓冲我们的应用程序正在生成的所有响应，直到生成日志的过程完成。因为我们想立即显示所有日志，所以我们必须配置代理服务器停止缓冲它们。

配置 Nginx 可以分两步完成:1)在项目的根文件夹中创建一个. ebextensions 文件夹，2)向该文件夹添加一个配置文件(不管它的名称是什么，只要它有. config 扩展名)。该文件的内容应该是:

现在，我们可以从项目的根文件夹中调用终端中的 **eb deploy** 来更新我们的应用程序，并等待部署更改。

**注意:**如果您已经将项目的根文件夹初始化为 GitHub repo，请确保在部署应用程序的新版本之前提交您的更改。默认情况下，只有已经提交的变更才会通过 **eb deploy** 调用进行部署。您还可以运行**EB deploy—staged；**然后你的 staged changed(所以你必须 **git 也添加**它们)也将被部署。

部署完成后，访问您的应用程序的 URL，您可以看到日志工作正常！

# 结论

按照这些步骤，在 AWS Elastic Beanstalk 环境中创建和部署一个具有日志功能的简单 Flask 应用程序是相当容易的。请随意扩展应用程序以服务于您的机器学习模型，并使用这种简单的方法将它们投入生产！

# 关于作者

[Rik Kraan](https://www.linkedin.com/in/rikkraan/) 是一名医学博士，在荷兰数据科学咨询公司 **Vantage AI** 担任数据科学家。通过[rik.kraan@vantage-ai.com](mailto:rik.kraan@vantage-ai.com)取得联系