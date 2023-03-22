# 构建简单的 Web 应用程序

> 原文：<https://towardsdatascience.com/build-a-simple-web-app-with-github-pages-flask-and-heroku-bcb2dacc8331?source=collection_archive---------20----------------------->

## 使用免费的智能框架，如 Flask 和 Heroku

作为一名技术专家和数据科学家，我从逻辑学家的角度看待日常任务。无论我是在企业范围内从事复杂的项目，还是在家做琐碎的家务，我都发现自己*幻想着*能够优雅而高效地管理我的工作流程的自动化解决方案。由于新冠肺炎的激增，我们中的许多人都是 WFH，专业和个人责任的冲突使我们的日常工作变得错综复杂，没有比现在更好的时机来自动化任务并将这些解决方案部署到 web 上。先说简单的吧！

![](img/c38fd3463d777ba903dd56c093aab28c.png)

来源: [beccatapert](https://unsplash.com/@beccatapert) 经 [unsplash](https://unsplash.com/photos/QofjUnxy9LY) (CC0)

就像你们中的许多人被困在家里一样，我一直拼命想了解新冠肺炎的最新动态。大量的实证新闻文章和创造性的应对机制在我的收件箱和新闻订阅中快速掠过。一个共同的线索？耐心越来越少！和我们爱的人一起被隔离(或者更糟，被单独隔离)加剧了那些我们通常可以置之度外的小气恼和怨恨。让我家每个人都不爽的事情是每周准备晚餐。

为了每个人的理智，我决定将每周的膳食准备民主化，并简化这个过程。与其在周末花太多时间浏览互联网，从众多流行的在线资料库中选择食谱，与我可爱但挑剔的家人争论/讨价还价，比较配料/说明以最大限度地节省时间和金钱…

🤦‍

…我们现在只需将我们最喜欢的食谱汇总到一个由 GitHub Pages 托管并通过 Flask 和 Heroku 部署的**易用、响应迅速的在线应用程序中。**它超级简单，可以创造性地应用于你和你的检疫同伴争吵的任何数量的艰巨任务。看一看成品:

 [## 完美应用

garretteichhorn.github.io](https://garretteichhorn.github.io/recipe_generator/) 

# 步骤:

*   用 [BeautifulSoup](https://pypi.org/project/beautifulsoup4/) 和 [Splinter](https://pypi.org/project/splinter/) 从一些你喜欢的在线供应商那里搜集食谱。在深入研究之前，一定要温习一些网上搜集的法律理论。
*   创建一个定制的搜索引擎来返回相关的食谱。
*   用 HTML 和 Javascript (CSS 可选)搭建一个简单的前端网站，用 GitHub 页面托管。
*   使用 [Flask](https://flask.palletsprojects.com/en/1.1.x/) 通过 API 访问您的食谱，这是一个用 Python 编写的出色的微型网络框架。
*   使用 Heroku 来托管您的 API，而不是使用本地主机资源。

# 网页抓取:

**BeautifulSoup** 和 **Splinter** 让我们可以轻松地遍历网页，提取我们需要的数据。每个网站都有一个 DOM，它是一棵树，其结构由嵌套的标签定义。Beautiful Soup 浏览这棵树，然后将它转换成一个专门化的对象，该对象配备了强大的方法来遍历和搜索 HTML 中的属性、文本等。Splinter 是一个使用 Python 测试 web 应用程序的开源工具。它让你自动化浏览器的动作，比如访问 URL 和与它们的项目交互。

半生不熟的收获是我最喜欢的健康美味食谱的地方之一。看一下下面的代码，通过网站迭代提取相关的数据，如配料、说明、图像等！

通过检查 HTML，我们可以很容易地为其他食谱网站复制这一点。对于我构建的应用程序，我从其他几个菜谱存储库中收集菜谱，创建一个以 JSON 格式存储的菜谱数据的自定义数据库。

# 搜索引擎

在搜集了所有这些精彩的食谱数据之后，我们需要某种方法来只提取相关的信息。我们*可以*使用基本的比较操作符来检查文本中的搜索词，但是这有什么意思呢？另外，简单地检查食谱标题是否包含“鸡肉”有一些严重的局限性。

相反，我们可以构建一个**搜索引擎**来返回按相关性排序的食谱。使用一点数学知识，我们可以利用 [scikit-learn](https://scikit-learn.org) 来清理、标记和构建一个加权矩阵，以将输入搜索词与一个文本字符串相关联。

有很多方法可以使用这个强大的工具！我们可以发挥创意，只返回余弦相似度超过特定阈值的食谱。根据食谱数据库的大小，我们可能希望将所有相关的食谱作为一个解析的数组返回，然后使用另一个搜索词进行迭代。对于这个应用程序，我只是返回所有搜索项相似度> 0 的食谱，并让我的随机函数选择一个。

# GitHub Pages 托管的前端

尽管我们能够使用 python 来获取食谱，并基于搜索词返回相关选项，但这种功能对于我这个不懂技术的家庭来说还无法使用。我需要使用一个简单的界面，要求一个搜索词，并显示食谱图像，配料和说明。我们将转向 **HTML** 和 **JavaScript** 来完成繁重的工作。

声明:我非常尊重网页开发者！有一些非常棒的开源前端框架，可以帮助你建立快速响应的网站。我更喜欢[引导](https://getbootstrap.com)和 [D3](https://d3js.org) 来实现高效部署。

GitHub 有一个令人惊叹的网站托管服务，叫做 [GitHub Pages](https://guides.github.com/features/pages/) 。你可以看看下面我的知识库，它使用 GitHub Pages 和 JavaScript 来构建 HTML(带有嵌入式样式)和显示食谱数据。

[](https://github.com/GarrettEichhorn/recipe_generator.git) [## GarrettEichhorn/配方生成器

### 这个应用程序的创建是为了让周日的膳食计划更容易！穿越…的日子一去不复返了

github.com](https://github.com/GarrettEichhorn/recipe_generator.git) 

# 用 Flask 构建一个 API

接下来呢？我们有 JSON 格式的所有这些精彩的食谱数据，一个用 python 编写的搜索引擎来帮助我们提取相关选项，还有一个简单的网站，对我的家人来说非常容易使用。为了让这些组件相互交互，我们需要使用一个应用程序编程接口。web API 允许其他程序通过互联网操纵信息或功能。

让我们欢迎烧瓶参加派对！！！Flask 将 HTTP 请求映射到 Python 函数，允许我们基于端点运行特定的代码。当我们连接到位于 [http://127.0.0.1:5000/](http://127.0.0.1:5000/) 的 Flask 服务器时，Flask 会检查所提供的路径和定义的函数之间是否匹配。我们将使用 Flask 来处理一个输入搜索查询，作为运行我们之前定义的搜索引擎功能的给定端点。

例如，我们可以发送…

```
[http://127.0.0.1:5000/chicken](http://127.0.0.1:5000/chicken)
```

…让 flask 将“鸡肉”作为 return_relevant_recipes()函数的参数进行处理。太棒了。

# 部署到 Heroku

最后，我们需要使用 **Gunicorn** 将 API 部署到 **Heroku** 上，这样我们就不需要依赖本地主机资源让用户访问搜索引擎功能。Heroku 是一个云平台即服务，支持我们在这个项目中使用的几种编程语言。很喜欢这个[教程](https://medium.com/the-andela-way/deploying-a-python-flask-app-to-heroku-41250bda27d0)快速设置必要的需求(Procfile，requirements.txt，git connection 等。)我们需要部署到 Heroku。Gunicorn 是一个高性能的 web 服务器，可以在生产环境中运行 Flask 应用程序。

一旦您有效地部署了 API，您将更新 JavaScript 代码，将一个搜索词作为端点发送到[https://reciperfect.herokuapp.com](https://reciperfect.herokuapp.com)而不是您的本地主机。就这么简单。

# 结论

本文概述了使用几种流行的开源工具和框架来自动化艰巨任务的必要组件。发挥创造力！在我的家人转而使用我的应用程序之前，每周做饭是一件非常棘手的事情，我们没有回头看！当我们对目前的食谱感到厌倦时，我需要做的就是再刮一些。你可以用这个纲要来自动化许多平凡的杂务，让新冠肺炎的生活稍微轻松一点。

我的公开回购可以在[这里](https://github.com/GarrettEichhorn/recipe_generator)找到。感谢阅读！