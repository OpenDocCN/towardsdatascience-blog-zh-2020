# Anvil 简介——只有 Python 的全栈 Web 应用

> 原文：<https://towardsdatascience.com/an-introduction-to-anvil-full-stack-web-apps-with-nothing-by-python-cbab06392d13?source=collection_archive---------15----------------------->

![](img/2206aa83f2e72c36973d717248cb8f15.png)

我和 Anvil 一起做的一个网络应用，用来监测蒙古乌兰巴托的空气污染。

## 在几分钟内构建一个数据科学应用程序，并通过一次点击将其部署到 web 上。

> 声明:我不隶属于 Anvil，我只是喜欢他们的产品。

随着越来越多的数据科学家进入世界各地的组织，大多数人会发现一个与他们在网飞、脸书或谷歌梦想的非常不同的工作环境。在这些公司，数据科学家由数据工程师、机器学习工程师、应用程序开发人员和开发-运营专家提供支持。相反，他们可能会发现自己在一个小团队中工作，甚至是独自工作。当数据科学家想要将他们的见解、模型甚至产品从 Jupyter 中取出并投入生产时，这就带来了重大问题。

Anvil 填补了这些空白，它允许你只使用 Python 来构建一个全栈的 web 应用。可以用简单的拖拽 UI 构建用户界面(如果你坚持的话也可以用代码构建)，用你喜欢的 Python 绘图库(Plotly，Matplotlib 等)绘图。)，然后一键部署到 web。没有服务器或容器需要处理。

我们先来看看 Anvil 的基本特性，看看它能有多强大。

# 拖放用户界面

使用拖放界面轻松构建您的用户界面。像下拉菜单、滑块、图像和文本这样的元素很容易定位，不需要 HTML 或 CSS。

![](img/de5ecab37d207bee5ddcf2ddb50f7e9e.png)

视觉定位元素以获得您想要的外观。

您不会被限制在特定的用户界面上。您可以为您的用例定位元素。当你有了你想要的外观，你可以用 Python 把它们全部连接在一起。

![](img/291b1576a055c4cc6c398deb6af3a8d3.png)

下拉菜单的选项可以预先定义，也可以用代码创建。不仅仅是下拉菜单。Python 中的每个元素都是可访问的。

# 测绘

通常，应用程序的很大一部分将通过数据可视化来分享见解。Anvil 支持几乎所有流行的 Python 绘图库的绘图。以下是 Anvil 为其制作指南的库:

*   Matplotlib
*   Plotly
*   海生的
*   散景
*   阿尔泰尔
*   臀的

由于它的前端 Javascript 库，Plotly 得到了最直接的支持，这也是我选择使用的库。对于其他人来说，可以通过嵌入 HTML 甚至图像文件来显示绘图。查看[砧座标绘指南](https://anvil.works/blog/plotting-in-python)。

![](img/6ada5c55b87965307feda5e0e4c77df0.png)

只需将图表元素拖到页面上，然后用 Python 连接它。可以用下面的代码制作一个简单的条形图:

![](img/787693cd376b31b9c0233f4000449ab6.png)

# 数据库ˌ资料库

通常你会想为你的应用程序存储一些数据。如果您或您的组织有一个数据库，那么没有问题。Anvil 通过流行的 Python 库如 *pymysql* 和 *psycopg2* 支持[外部数据库](https://anvil.works/docs/data-tables/external-database)。

但是我发现一个可以加速我开发的特性，那就是 Anvil apps 拥有的集成数据库。称为数据表，它们是基于 PostgreSQL 的数据库系统，你可以在 Python 中直接访问。

# 部署到 Web

当你准备好部署你的应用程序时，没有必要担心如何从你的本地机器上下载应用程序。已经在云端了！只需一键发布应用程序并共享链接。如果你想保护应用程序，你可以通过添加[用户](https://anvil.works/docs/users)来轻松认证。

![](img/4d862e5f8428e1eed90fd7eb74e166ef.png)

部署只需三次点击，你有一个公共链接与世界分享！

实际上，有几种方法可以部署您的应用程序:

*   **在 Anvils cloud 中构建和部署**——这可能是最简单、最快的方法，但这可能并不适合所有人。
*   **在您的私有云上构建和部署** — Anvil 有一个企业版，允许您在自己的服务器上部署完整的 Anvil 堆栈。
*   **使用 Anvils IDE 构建，托管在您自己的服务器上** —这为您提供了强大的拖放功能，但允许您自由托管在您想要的任何地方。见下面的开源应用服务器。
*   **用文本编辑器从头构建**——你会错过几乎所有让 Anvil 更快的东西，但这是可能的。

# 开源应用服务器

也许你是那种希望从上到下控制应用程序的人，而不是依赖某个特定的供应商来托管你的应用程序。恩，Anvil 把你也包括在内了。Anvil 已经[开源了它的应用服务器](https://anvil.works/open-source)，它允许你在几乎任何地方托管 Anvil 构建的应用。

就我个人而言，我仍然更喜欢使用 Anvil 主机，因为这是迄今为止最简单、最方便的应用程序托管方式。

# 速度快 7 倍

使用 Anvil 开发 web 应用程序比使用当前工具快 7 倍。这听起来像是一句营销台词，的确如此，但这绝对是真的。我尝试用 Dash 开发一个应用程序，不到一周，我就想把我的电脑扔出窗外。以下或许是您应该如何看待 Anvil 的更好描述:

> 使用 Anvil，一个或两个团队可以比四个或五个团队更快地完成同样的工作。

# 加入社区

Anvil 最大的好处之一就是它有一个很棒的社区。在 [Anvil forums](https://anvil.works/forum) 上，你会发现一群热情的开发者帮助你入门并回答问题。还有一个很棒的展示和讲述部分，在那里你可以获得自己项目的灵感。以下是一些用 Anvil 制作的优秀应用程序:

*   [店内 iPads 产品目录](https://anvil.works/blog/community-apps#product-catalogues-for-in-store-ipads)
*   [面向学校的数据探索工具](https://anvil.works/blog/community-apps#data-exploration-tools-for-schools)
*   [实时雪车比赛跟踪](https://anvil.works/blog/community-apps#real-time-snowmobile-race-tracking)
*   [机器学习竞赛的主办和评分平台](https://anvil.works/blog/community-apps#a-platform-for-hosting-and-scoring-machine-learning-competitions)

我使用 Anvil 已经快一年了，我无法想象使用其他任何东西向世界展示我的数据科学想法。它在不断改进，开发人员社区非常优秀并且支持我们，和它一起工作简直是一种享受。在 [https://anvil.works 查看](https://anvil.works.)

> 感谢阅读。你可以在这里找到更多关于我的信息。考虑订阅以便在我发布时收到通知。如果你想直接支持我的文章，你可以使用我的推荐链接注册成为一名媒体会员。