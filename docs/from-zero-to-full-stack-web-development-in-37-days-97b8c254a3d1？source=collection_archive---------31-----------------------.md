# 37 天内从零到全栈 web 开发

> 原文：<https://towardsdatascience.com/from-zero-to-full-stack-web-development-in-37-days-97b8c254a3d1?source=collection_archive---------31----------------------->

## 以及如何更快地完成它

![](img/24569d2dce29fdb5c147604639f3b718.png)

乔恩·泰森在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上的照片

## 我是

西蒙。整整一年前，我完成了我的第一个 LinkedIn 学习 Python 课程。我这么做是因为它有市场，而不是因为我喜欢它。我不打算成为一名全栈式的 web 开发人员，或者任何类型的程序员。如果你问我，我宁愿做一个口袋妖怪训练师。

## 这是

我是如何在 37 天内学会所有技能，构建一个 web 应用程序并将其部署到现场的。我不能回答生活中所有的重大问题，也不能告诉你复活节彩蛋在哪里。然而，如果你是第一次、第一次或第二次尝试，这是一个可以帮助你做得更好的指南。

在线教程告诉你的太少了:他们给你代码，却没有告诉你任何东西的意思，或者警告你有错误。当出现问题时，StackOverflow 会告诉你太多:你必须仔细阅读那些告诉你所有事情的帖子，从如何与你的操作系统进行对话到 Linux 早餐吃了什么。

这就是我来这里的原因。我不会给你所有的代码或者解释如何把你的 Mac 变成汽车人。相反，我会带你了解你的项目应该如何进展，给你指出相关的资源，解释绝对的基础知识，并强调要避免的重要错误。以此作为时间表和补充资源，你应该比我用更少的时间就能拥有制作一个像我这样的 web 应用程序所需要的一切。

注意:我所做的最适合于动态网站(例如，与用户交互，执行功能，产生不同的输出)。如果你想制作静态网站(即拥有不变的内容，如文本和图像)，**这将是不值得的麻烦**——选择 Wordpress 或 Squarespace 这样的网站建设者，以获得最佳投资回报。

![](img/0ad10b13d439127c7614c5d06f47114f.png)

Crackshort 的主页——作者图片

## 我建造了

[**Crackshort.co**](https://www.crackshort.co/)，一个使用技术指标回溯测试可定制交易策略的应用。外行:你选择一些股票，一些根据价格变动产生买卖信号的指标，一些管理交易的规则，然后用历史数据测试这个策略。

你可能不是为了营销信息而来的，所以我就不多说了，但是绝对欢迎你来看看这个 37 天项目的成果，也许还可以进行一两次回溯测试。现在，让我们进入你来的目的。

![](img/8f969e8627f8aae5dc8746a01a13649f.png)

5 月 19 日，我完成了我的第一门 Django 课程——图片来自作者

![](img/9ae5e1810e1643b2e278ce03f32ad092.png)

接下来的一周，我创建了我的本地 Django 项目文件夹——作者图片

![](img/d9349c0aa631b3c38534c606e6d24979.png)

我在 6 月 23 日注册了数字海洋，并在两天后(第 37 天)发布了我的网络应用——图片由作者提供

# 内容

> 如果链接没有按预期工作，请在新标签中打开它们

1.  [堆栈](#99e9)
2.  [视力](#634a)
3.  [流程](#c456)
4.  本地构建:让它在你的计算机上工作
5.  [骨架站点:用 Django 创建基本结构](#214e)
6.  [App-ify:把静态网站变成一个带 I/O 的 App](#cc41)
7.  [美化:做设计和前端工作](#28cc)
8.  [部署:将其连接到互联网](#4df9)
9.  [更新:维护一个可靠的更新过程](#ce35)

# 1.堆

在这 37 天的开始，我对 Python 的基础**很熟悉**，也就是说，我可以编写函数，使用字典，进行文件输入/输出，使用导入的库，等等。我对 Mac 终端也有一点**的接触，即我可以使用 *cd* 、 *ls* 、 *pip* 命令。这在以后变得非常重要。**

在那之前，我一直专注于在 Jupyter 中做数据分析，所以对 pandas 和 numpy 很熟悉。后一点并不是 web 开发的先决条件，但对于我的项目来说是非常必要的。

我也曾在大约 4 年前的一个在线课程中尝试过 HTML/CSS。这几乎没用，但我相信它确实给了我一点点提升前端学习曲线的动力。不然我就有了**零前端经验**。

![](img/ddf4bdb508c3089629db18850951e130.png)

我用的堆栈。徽标属于各自的创作者。

我学会使用的栈如下，从后端到前端(左上右下):

*   后端 IaaS: **数字海洋**
*   后端操作系统: **Ubuntu Linux**
*   后端数据库: **PostgreSQL**
*   后端语言: **Python + Django**
*   后端服务器: **NGINX + Gunicorn**
*   前端:**Bootstrap+HTML/CSS/JavaScript**
*   其他资源/工具:**终端**(用于部署期间的编码和 Git)**name cheap**(用于域名)**LinkedIn Learning+stack overflow**(用于字面上的一切) **Git + Github** (SCM，或源代码管理)

提醒一下——这是一个非常实用的演练，所以我不会详细解释每个工具的概念和基本原理。实际上我对他们中的大部分一无所知，仅仅足以让一个网站工作。

# 2.视力

该应用的愿景是:创建一个**模块化回溯测试引擎**，允许用户:1)选择一个回溯测试领域(股票和日期范围)，2)选择最多 2 个技术指标，3)定义这些指标和止损规则之间的关系，4)选择如何确定每笔交易的规模，5)运行回溯测试，并生成策略结果和一些指标。

![](img/ea155d1982ca88bf6f88d0d434de3384.png)

应用程序工作流程-按作者分类的图片

该网站的愿景是:制作一个网站，接受**用户输入**，通过**执行计算**的 **Python 应用**和**输出一些输出**，然后**在**相同的网页**上传递这些输出**。

这一愿景告诉我们，我们需要一个独立的 Python 应用程序，它使用单个命令运行并返回单个结果，还需要一个交互式动态网站，它允许用户通过表单输入值。

# 3.过程

下面是我的过程概述。我犯的主要错误，其中一些花费了我很多时间，用斜体表示。

*   在本地用 Python 构建应用程序
*   用 Django 创建静态网站
*   实现应用程序的输入和输出系统
*   *开始使用 Git 进行版本控制*
*   美化前端
*   *通过 AWS 部署(失败)*
*   迁移到 PostgreSQL
*   通过数字海洋部署
*   *创建应用程序更新流程并运行多次更新*

**如果我再做一次，我会遵循以下工作流程:**

*   **本地构建**:从一开始就使用 Git/Github 在本地用 Python 构建应用程序
*   **骨架站点**:用 Django 创建静态网站，迁移到 PostgreSQL
*   **App-ify** :实现 App 的输入输出系统
*   **美化**:学习 HTML/CSS/JS，美化前端
*   **部署/更新**:使用数字海洋，仅在必要时更新

现在，我将更详细地解释**理想的**工作流程以及如何避免我所犯的错误。

# 4.本地构建:让它在您的计算机上工作

## 你需要:

Python，Git/Github，终端(基本)

## 请参考:

[Git 文档](https://git-scm.com/docs/gittutorial)

## 结果:

可以在文本编辑器中运行的应用程序

## 时间线:

第 0 天

这是您运用基本 Python 技能的地方。在开始创建网站之前，我已经花了很多周的时间来规划、构建和构建本地应用程序，因为就我的技能水平而言，这是一个大项目。

回溯测试平台的功能类似于交易实践的**射击场**——当用户走进来时，他们可以定义他们的目标(股票)，定制他们武器的部分(策略)并选择载入什么(指标)。因此，该应用程序需要有一个预定义的策略，指标和规模方法目录，用户可以从中选择。Backtrader 库提供了应用程序的主要基础设施，交互式经纪人 API (IBPy)最初提供了数据。后来，我改用了 IEX 云 API，因为 IBPy 在本地连接到我的 IB Trader 工作站软件，不能在线工作。

![](img/e9f8b68dfb5748697721c11b12c16602.png)

成功运行的应用程序的后期版本——图片由作者提供

我在 **Jupyter 笔记本**中开发了整个项目，这有点不方便，因为所有的脚本后来都必须转换成。py 文件，然后才能作为 web 应用程序的一部分运行。话虽如此，我还是推荐。对于那些没有尝试过 Jupyter 的人来说，它允许您运行和调试组织到单元中的单独代码，而不是一次运行整个脚本。这对于像我这样经常出错的低技能编码人员以及必须处理大量数据的脚本来说非常有帮助。

# 饭桶

我很晚才知道，但是**使用 GIT/GITHUB。如果你像我一样自学成才，很有可能没人把 Git 塞到你脸上，所以我来做这个荣誉。作为初学者，以下是您需要知道的所有内容:**

*   SCM 是版本控制软件，Git 是 SCM。
*   简单地说，Git 存储库是一种存储设备，在做了一些更改后，您可以偶尔发送(“推送”)代码文件。
*   SCM 跟踪每一次上传(“提交”)，所以如果你想的话，你可以回到以前的版本。
*   如果你和其他人一起工作，你也可以从 repo 下载(“克隆”)整个项目，或者从 repo 获得(“拉”)一个更新的版本来覆盖你的本地文件。
*   如果你正在构建一个大的新特性，在一个“分支”上进行编码，这样就不会影响到项目其余部分所在的“主”上，然后在特性准备好的时候将两者“合并”。

Github 是一个在线 Git 回购平台。使用 Git 或 Github 很重要，因为它让你和你的项目有条理——当我开始使用它时，我已经进入了 web 开发阶段，但它让我安心地知道我的更改正在被跟踪，并帮助我从里程碑的角度思考我的进展。此外，**一旦你的应用上线，更新它是至关重要的**——稍后将详细介绍。如果你还没有，开始使用 Git/Github ( [阅读这里的教程文档](https://git-scm.com/docs/gittutorial)，或者阅读罗杰·杜德勒的[这份入门指南)。](https://rogerdudler.github.io/git-guide/)

*   **如果你有一个现有的项目，你想推送到一个新的 Github repo:不要添加许可证和自述文件到它上面**；在您第一次推进项目后添加它们。它们一开始并不在你的本地项目中，所以如果你试图把你的文件推上来，你会遇到一个错误([见 StackOverflow](https://stackoverflow.com/questions/18328800/github-updates-were-rejected-because-the-remote-contains-work-that-you-do-not-h) )。

最终，我把这个项目分成几个部分。py 文件，创建了一堆自定义类，并使应用程序工作。该应用程序的最终结构如下:

![](img/7f04864137f3f98f5f6ef2cf81ecea58.png)

粗略的项目结构—作者提供的图片

此时，我可以将输入定义为 FiringRange.py 中的变量，并调用 fire()来运行整个回溯测试，这将在 print()中返回结果。

# 5.骨架网站:用 Django 创建基本结构

## 你需要:

Django (basic)，PostgreSQL (basic)

## 请参考:

[Django 文档](https://docs.djangoproject.com/en/3.0/)、[、LinkedIn 上的“学习 Django”](https://www.linkedin.com/learning/learning-django-2/)、[、LinkedIn 上的“建立个人投资组合”](https://www.linkedin.com/learning/building-a-personal-portfolio-with-django/)

## 结果:

一个“网站”,你可以从你的本地主机运行，有一个 PostgreSQL 数据库

## 时间线:

第 1 天—第 4 天

Django 是一个用 Python 编写的全栈库，它使得创建一个网站结构变得非常快速和容易，**如果你完全按照这个过程来做的话**。我强烈推荐它，并强烈推荐[这个 LinkedIn 学习路径](https://www.linkedin.com/learning/paths/become-a-django-developer)，它包含了整个构建过程中我需要的所有基础知识和工具。Django 的课程无处不在，大多数都能达到目的，但是无论你选择哪一个，都要密切关注，因为有一些小的、容易被忽略的细节会使整个项目停顿下来。

同样，我不会教你 Django，因为这些课程做得更好，但当你参加这些课程时，请确保你特别注意**URLs . py、views.py 和模板如何相互关联**、**如何配置 settings.py、**和**如何使用 manage.py** 访问数据库、运行本地主机和组织静态文件。

因为 Django 的组件是如此错综复杂地联系在一起并且相互依赖，所以您需要有这个基本的图片来避免可能破坏整个链的小错误(例如，视图名称中的输入错误)(例如，URL 找不到要调用的视图)。Settings.py 在部署期间变得特别重要。这里有一个使用我的网站的两个页面的快速概述，动态回溯测试页面(链接到应用程序)和静态关于页面。

![](img/f8a78f754dec529f307d5fc00f362f1b.png)

我的 Django 项目的基本流程。在这个阶段，你还不需要担心应用程序(firing range . py)——图片由作者提供

有了这个基础，开始你的项目。

1.  在您的项目中创建应用程序
2.  创建基本的网址:如主页，回测页面，关于页面。
3.  创建调用相应模板的基本视图
4.  创建仅包含一些占位符 html 文本的模板
5.  运行服务器并在浏览器中转到 localhost

准系统代码

![](img/548b1fe0a99752e174d496d01ed09ced.png)

您将获得什么—按作者分类的图像

# 数据库

现在考虑数据库是很重要的，因为它以后会节省你的时间和脑细胞。这很重要，我为这部分做了个标题。

Django 默认使用 SQLite。然而，在生产中(当你的网站上线时)，你最终会使用 MySQL 或 PostgreSQL。我使用 PostgreSQL 的原因是:

1.  它被用在 LinkedIn 学习课程中
2.  它成功了(实际上并不容易——继续读下去)

为了更好地解释数字海洋[的数据库选择，请点击这里](https://www.digitalocean.com/community/tutorials/sqlite-vs-mysql-vs-postgresql-a-comparison-of-relational-database-management-systems)。更重要的是，当我尝试用 AWS 部署时，**我遇到了很多数据库问题，首先是 SQLite，然后是 PostgreSQL** ，这最终让我彻底放弃了 AWS。PostgreSQL 和 DigitalOcean 一起工作，所以我坚持使用它。

知道了这一点，**我会在这个阶段**将我的 Django 项目迁移到 PostgreSQL，而不是在我已经创建了实例和模型之后，我需要额外的步骤来移动。

*   SQLite-AWS 问题:AWS Elastic Beanstalk 虚拟机无法找到 Django 所需的 SQLite 版本，我对此无能为力([参见 StackOverflow](https://stackoverflow.com/questions/55674176/django-cant-find-new-sqlite-version-sqlite-3-8-3-or-later-is-required-found) )
*   PostgreSQL-AWS 问题:AWS Elastic Beanstalk 虚拟机首先无法安装 psycopg2，因为它找不到 pg_config ( [参见 StackOverflow](https://stackoverflow.com/questions/33747799/psycopg2-on-elastic-beanstalk-cant-deploy-app/34957820#34957820) )，然后当我安装它时，它找不到 psycopg2
*   对于 Mac 用户:PostgreSQL 需要 psycopg2。psycopg2 需要 pg_config。如果您试图用 pip 安装 psycopg2，pg_config 会阻止您。这个 StackOverflow 页面有所有的答案，但是对我来说最快的解决方法是自制(在答案列表中向下滚动一点)
*   如果你已经有了 SQLite 数据库，现在想转移到 PostgreSQL(像我一样):[这个 Github 要点是你需要的神奇修复](https://gist.github.com/sirodoht/f598d14e9644e2d3909629a41e3522ad)。

相信我的话，如果你认为 PostgreSQL 是对的，**现在就迁移你的项目**。为此，[遵循 DjangoGirls 教程](https://tutorial-extensions.djangogirls.org/en/optional_postgresql_installation/)或 LinkedIn 学习 Django 路径中的[建立个人投资组合课程。LinkedIn 课程更详细地介绍了 PostgreSQL。](https://www.linkedin.com/learning/building-a-personal-portfolio-with-django/)

# 6.App-ify:把静态网站变成有 I/O 的 App

## 你需要:

Django(中级；模型、表单、POST/GET 请求)

## 请参考:

[Django 模型文档](https://docs.djangoproject.com/en/3.0/topics/db/models/)， [Django 表单文档](https://docs.djangoproject.com/en/3.0/topics/forms/)， [Django 模板文档](https://docs.djangoproject.com/en/3.0/ref/templates/language/)，[LinkedIn 上的“Django:表单”](https://www.linkedin.com/learning/django-forms/)

## 结果:

本地“网站”的形式，传递输入到一个应用程序，并显示输出

## 时间线:

第 5 天—第 14 天

静态网站结构完成后，我们将插入 Python 应用程序并为网站提供 I/O。这将使您深入 Django 领域，所以请准备好花大量时间学习 Django 文档和课程。

在我们现有的 Django 生态系统中，我们需要实现**模型和表单**:

1.  我们输入变量的模型(策略、指标、尺寸)
2.  一个输入模型，它定义了每个回溯测试的所有输入——它将有与上述模型链接的策略、指标和大小字段
3.  我们的用户将填写的模型表单，其中表单字段对应于输入模型
4.  保存回溯测试结果的输出模型

模型是保存在数据库中的数据表；模型中的每一列都是一个数据字段，每一行都是模型的一个实例。然后，这些模型可以与外键相链接。同样，在线课程将很好地解释基础知识，但这里是我的模型如何相互关联。

![](img/3201e392ce5a00d8952c2531f6849867.png)

关系数据库—作者图片

上面，输入模型将策略、指标和 Sizer 模型实例作为其字段的输入。回溯测试 ID 是分配给每组输入的任意数字。在应用程序返回一组输出后，输出以相同的 ID 保存在数据库中，这样每组输出都可以追溯到用户的输入。

下面是这些模型如何与表单和我们现有的 Django 工作流的其余部分相关联。

![](img/abf9638ebd28432ed49f6bf99c137d15.png)

来回传递表单—作者的图像

视图-模板关系看起来很复杂，但这仅仅是因为我特别想在表单本身所在的页面上显示回溯测试的结果。更详细地说:

1.  回溯测试视图调用网页模板首先为用户呈现一个空表单
2.  当用户点击“提交”时，网页通过 POST 请求将输入发送到 Backtest 视图(一种处理数据的 HTTP 方法，[更多信息请点击](https://www.w3schools.com/tags/ref_httpmethods.asp)
3.  Backtest 视图检查它是否是有效的 POST 请求，将其保存到输入模型，然后将其发送到 FiringRange.py 应用程序
4.  回溯测试视图接收输出，将其保存到输出模型，并将其发送回同一个网页模板
5.  如果收到结果，网页模板被告知不要重置表单，并在指定的空间显示输出

如果您希望输出显示在不同的页面上，POST 请求将被发送到不同的视图，这将呈现不同的“结果”模板，但过程是相同的。这是我做的:

![](img/1faba6b8eddf683b0df06a5d8c6a42a5.png)

用户第一次进入时的新表单—按作者排序的图像

![](img/4ced333d85693f0d4a74f193848ea9c5.png)

用户提交表单后出现输出—作者提供的图像

当然，这是已经 CSSed 的版本。我在这个阶段混合了很多前端工作，因为我没有耐心，**但是我会建议不要这样做**，因为必须同时学习 HTML/CSS 和 Django 会大大降低我的速度。

![](img/af3859cb98a75a22336224cf0f49e489.png)

你的网站应该是这样的。[资料来源:熊伟·弗雷塔斯](https://simpleisbetterthancomplex.com/tutorial/2018/11/28/advanced-form-rendering-with-django-crispy-forms.html)

此外，我还实现了一个**结果页面**和一个**编辑特性**。如果用户点击“详细结果”按钮，他们将被定向到一个单独的页面，其中包含使用散景为他们刚刚运行的回溯测试生成的图表。在这个结果页面的底部有一个“编辑回溯测试”按钮，如果用户想要返回并对这个特定的运行进行更改。

![](img/d7fe12cd5ad2fa654a907797941e63a4.png)

特定回溯测试运行的结果页面-作者图片

如果用户点击“编辑”，回溯测试视图再次被调用*，并给出用户想要编辑的回溯测试的 ID。回溯测试视图在输入数据库中找到该特定 ID 的输入，并再次呈现回溯测试模板*，但是这一次，将这些输入输入到表单中，而不是显示一个新的表单。**

**![](img/37e39d1552e3b6068a09f8d9e61db23f.png)**

**编辑功能-按作者分类的图像**

****这是我的回溯测试视图最终的样子:****

# ****7。**美化:做设计和前端工作**

## **你需要:**

**Django，引导程序，HTML/CSS/JS**

## **请参考:**

**[LinkedIn 上的引导文档(示例)](https://getbootstrap.com/docs/4.5/examples/)、[“建立个人投资组合”](https://www.linkedin.com/learning/building-a-personal-portfolio-with-django/)**

## **结果:**

**一个好看的，带互动 JavaScript 的本地品牌网站**

## **时间线:**

**第 15 天—第 24 天**

**取决于你喜欢什么，这可能是最痛苦或最不痛苦的阶段。我不会在这里阐述太多，因为不会有太多问题，当涉及到美学时，我们都有不同的优先考虑。**

**我建议看一下**引导演示**(上面的链接)，选择一两个，打开页面源代码，将整个 HTML 文档复制到你的模板中。这是让你的网站看起来像样并给它一些结构的最快方法，这个过程在“建立个人作品集”课程中有很好的解释。**

**我用 Bootstrap 的封面作为我的主页，Navbar 为我的其余页面进行了修复。使用 Django 的“extends”标签，我制作了一个包含 Navbar、通用 CSS 属性和一个空“block”的“base”模板，然后简单地将它应用到我的每个模板中。这是我非常简单的关于页面。**

****Bootstrap 的卡片**(见上例)和**网格**也很有帮助。卡片使得对内容进行分组和格式化变得非常容易，网格系统使得整个页面布局变得非常快速。**

**我花了大部分时间在回溯测试输入表单的前端工作。我使用 Django 脆皮形式来快速使它看起来更好——[见熊伟·弗雷塔斯的教程。然后，我开始使用 Javascript 和 JQuery 使表单具有响应性。](https://simpleisbetterthancomplex.com/tutorial/2018/11/28/advanced-form-rendering-with-django-crispy-forms.html)**

**![](img/e4e1ec34208c1d3ff58469dd93631b1f.png)**

**输入示例 1:使用止损，只有一个指标—作者图片**

**首先，我需要表单根据用户的策略选择来显示/隐藏止损和指标字段。其次，我需要指标的**参数字段来显示/隐藏和更改它们的标题**，以告诉用户他们选择的指标需要什么参数。参考上面和下面的例子来看看这些变化。**

**![](img/fd325dd2ccaf1f0ca00e84e5ae69663d.png)**

**输入示例 2:无止损，使用两个指标—作者图片**

**对于这些特性，我基本上必须学习基本的 Javascript 和 JQuery 语法，我并不期望深入其中，但我会指出来，以便您做好心理准备。让策略和使用止损字段分别显示/隐藏指标和止损字段相对容易。**

**上面的代码将事件处理程序附加到文档中的“usestops”和“strategy”元素，这些事件处理程序检查它们的输入是否已被更改为某些值，并相应地显示/隐藏其他元素。使用这个 on()处理程序([在这里了解更多信息](https://api.jquery.com/on/))证明**比另一个常用的 JQuery 方法**[**document ready()**](https://learn.jquery.com/using-jquery-core/document-ready/)**)好得多。**见下文。**

****Ready()仅在文档对象模型(DOM)准备好用于 JavaScript 时运行一次**，而 **on()随时跟踪事件**。Ready()对大多数页面都适用，但对我的页面不适用，因为我的表单可以被访问:**

*   **当用户第一次加载回测页面时**
*   **用户点击“提交”后，结果出现在旁边**
*   **在用户点击“编辑”并返回回测页面后**

**在第二种情况下，文档不会重新加载并再次准备好。**

**参数字段的更改实现起来要繁琐得多，我就不赘述了，因为您可能不需要这样做，但要点是:**

1.  ****在我的指标模型**中增加了字段，为每个指标指定:多少个参数，调用哪些参数，参数的默认值**
2.  **在我的回溯测试视图中增加了一个功能，即**获取指标模型中的所有对象**,**使用 json.dumps()和 DjangoJSONEncoder 将其打包到 JSON** 中，然后**将其发送到回溯测试模板****
3.  **将 JS/JQuery 添加到我的回溯测试模板中，**解析 JSON** 以获取指标信息，并使用 **more on()方法**，检查用户对指标的选择，以相应地显示、隐藏和覆盖表单的参数字段**

# **品牌宣传**

****最后，烙印**。在项目的这一点上，这可能不是你的首要任务，因为你的网站还没有上线，但我还是建议你这么做，因为最终确定网站名称和标志意味着你**不必在以后对 CSS 做大的改动**，你**锁定一个域名**。**

**我对域名搜索所花费的时间以及它对我品牌选择的限制程度感到惊讶，但由于我有一些品牌设计和开发的经验，这并没有造成太大的问题。如果你不知道，这里有一个 2 分钟的品牌速成班:**

****头脑风暴网站名称**。Crackshort 以“射击场”开始，因为我用这个比喻来弄清楚如何使这个回溯测试引擎完全模块化，所以我想用这个想法。我列出了 40 多个名字(当然大部分都是垃圾)。**

**品牌名称的其他想法:你自己的名字(哈利剃刀，著名的阿莫斯饼干，JP 摩根)，主要功能(Suitsupply，脸书)，一个形容词(Gamefreak)，一个地方(每家报纸，大学和熟食店)，真实单词(Twitter，Medium，Crunchyroll)，杜撰单词(Zapier，Trello)，双关语(Crackshort)**

****检查域**。由于域名的可用性，我的列表缩小到了‘marks mind’，‘Boolsai’(因为 bulls.ai 可能很酷，但已经有人用了)，‘Tryfire’，‘Stracer’，‘Tincker’，‘triffy’和‘crack short’。你知道我选了什么。如果你是游戏，现在购买域名给你额外的踢完成这个项目。**

**![](img/35d82a8b7d30dd8d0ffcbf5bf5e373e3.png)**

**超级配色方案—作者图片**

****做一个配色**。这有助于标识，你可以马上将它们插入到你的基本 CSS 中。良好的做法是有一个良好的明暗范围，并侧重于两个主要的对比色-我的是橙色(中心)和海军蓝(右)。在某处记下这些颜色的 RGB/hex 代码，以便参考。**

**![](img/0af8c9a50713314eb891b35ab60f25d7.png)**

**比较金融/交易/教育领域的品牌(Crackshort 在中间)。各自创作者的徽标**

****为徽标进行头脑风暴/情绪板**。徽标可以大致分为文字型和图片型。组合也可以。文字徽标更快(只需选择一种字体并键入名称)，但表达性和记忆性较差。如果你选择文字，至少要做一个“完整”版本和一个“缩略图”版本(就像上面 Quantopian 的 Q)。**

**我选择制作一个带有矢量插图风格的图形标志，以便在金融/交易/教育领域脱颖而出，我觉得在这个领域，品牌一直是文本化的和“安全的”，经常出现图表或烛台之类的图像(Quantopian 除外)。虽然我的网站的*事物*是整个射击场的隐喻，但我不想将这个品牌与突击步枪和子弹联系起来，所以我选择了左轮手枪枪膛的图像，而噱头将是一个切除的部分，使它看起来像一个 c。**

**![](img/af72f12db6b2a361276acd3da7e4c546.png)**

**Photoshopping 作者图片**

**![](img/a8a22c152db2d10c24abf26c8e88d684.png)**

**最终—作者提供的图像**

**![](img/f1e9037fe3d1381ca3088b5ebada58da.png)**

**活动中的收藏夹图标—作者提供的图片**

****最后，加上你的配色方案**，把它放到你的网站上，你就可以开始了。如果你做了一个 favicon，这是一个出现在浏览器标签中的小东西，你会得到加分。最简单的方法是将你的标志图像[放入一个 favicon 生成器以获得一个 ICO 文件](https://favicon.io)，然后将一个<链接>标签放入你的 HTML <头>中。如果您复制了一个 Bootstrap 模板，那么应该已经有一个 favicon 部分，您可以在其中替换代码，如下所示。不要忘记运行 collectstatic 来获取您的。静态文件夹中的 ico 文件。**

**![](img/7d919e9446a8ec022ecf0f780d1c1dcb.png)**

**Bootstrap 的默认中的 Favicon 代码—用 wherever your。ico 文件是—作者提供的图像**

# **8.部署:将其连接到互联网**

## **你需要:**

**数字海洋，终端(基本)**

## **请参考:**

**[在 LinkedIn 上部署 Django 应用](https://www.linkedin.com/learning/deploying-django-apps-make-your-site-go-live/)**

## **结果:**

**一个实时网站(在一个定制的 IP 地址上，或者更好，在你的域名上)**

## **时间线:**

**第 25 天—第 33 天**

**部署阶段很可能是最痛苦的一步，因为可能会出现无数的问题。如果你有一个非常简单的单页应用程序，如果你仔细遵循每一步，你应该没问题。如果你的应用程序很复杂，有很多依赖，祈祷星星排成一行。**

**从我对选项的一点点了解来看，如果你想要最少的步骤，就选 Heroku ( [姜戈女孩教程](https://tutorial-extensions.djangogirls.org/en/heroku/))。如果你想要最便宜的**，就用 AWS 弹性豆茎** ( [AWS EB 教程](https://docs.aws.amazon.com/elasticbeanstalk/latest/dg/create-deploy-python-django.html))。如果你想要最实际的**，就用数字海洋** ( [数字海洋教程](https://www.digitalocean.com/community/tutorials/how-to-set-up-django-with-postgres-nginx-and-gunicorn-on-ubuntu-18-04))。为了更深入的比较，[阅读这个顺时针方向的软件帖子](https://clockwise.software/blog/amazon-web-services-introduction-largest-cloud-services-provider/)。请注意，这里的动手并不意味着乏味，它意味着当出现问题时，很可能是你的错，因此可以由你来解决。**

**我第一次尝试使用 AWS EB first，由于数据库遇到了障碍。如果您之前错过了数据库部分，以下是主要部分:**

*   **SQLite-AWS 问题:AWS Elastic Beanstalk 虚拟机无法找到 Django 所需的 SQLite 版本，我对此无能为力([参见 StackOverflow](https://stackoverflow.com/questions/55674176/django-cant-find-new-sqlite-version-sqlite-3-8-3-or-later-is-required-found) )**
*   **PostgreSQL-AWS 问题:AWS Elastic Beanstalk 虚拟机首先无法安装 psycopg2，因为它找不到 pg_config ( [参见 StackOverflow](https://stackoverflow.com/questions/33747799/psycopg2-on-elastic-beanstalk-cant-deploy-app/34957820#34957820) )，然后当我安装它时，它找不到 psycopg2**
*   **对于 Mac 用户:PostgreSQL 需要 psycopg2。psycopg2 需要 pg_config。如果您试图用 pip 安装 psycopg2，pg_config 会阻止您。[这个 StackOverflow 页面有所有的答案](https://stackoverflow.com/questions/5420789/how-to-install-psycopg2-with-pip-on-python)，但是对我来说最快的解决方法是自制(在答案列表中向下滚动一点)**
*   **如果你已经有了 SQLite 数据库，现在想转移到 PostgreSQL(像我一样):[这个 Github 要点是你需要的神奇修复](https://gist.github.com/sirodoht/f598d14e9644e2d3909629a41e3522ad)。**

**另外，即使我在 EB 设置中选择了 Python 3.7，它也一直回到 3.6。AWS 有一套强大的服务，但在初始化、破坏和终止无数环境和应用后，我决定减少损失。然而，脱离生态系统比进入生态系统**要困难得多——如果你想在尝试过 AWS 之后断开它，请确保你查看了你的帐户的账单部分，并手动终止你在每个 AWS 服务中运行的每个对象/实例/进程。****

**去数字海洋。我按照 LinkedIn 的教程(在‘参考’中)，几乎没有问题。在 DigitalOcean 上，你**在他们的服务器上创建你自己的 Linux 虚拟机，就像一台远程控制的计算机**——你上传你的项目文件，然后使用终端命令，配置和部署。如果你是一个像我一样的 Mac 用户，你会感觉像是在做你已经在这个项目中做过的事情，但是是通过终端。对我来说，这比学习亚马逊的 EB 语法和阅读数百行日志来找出代码出错的地方要直观得多。**

****当然没有完美的过程**。在 DigitalOcean 中也出现了 psycopg2 问题，但这次因为我拥有了对虚拟机的**完全控制，一个简单的 *sudo 安装* ( [参考这篇 StackOverflow 帖子](https://stackoverflow.com/questions/5420789/how-to-install-psycopg2-with-pip-on-python)中的顶部答案)让我获得了继续 Postgres 所需的软件包。另请注意 **DigitalOcean 根据未知标准要求一些新用户**进行身份验证。我不得不上传 ID 和一张自己的照片，这是一个烦恼，但批准在分钟内给予。****

**DigitalOcean 需要你自己配置 Gunicorn 和 NGINX，但是步骤很简单，在上面链接的 DigitalOcean Django 教程中有解释。除了知道 Gunicorn 是一个 WSGI 服务器，NGINX 是一个 web 服务器之外，你不需要知道任何关于它们的信息([阅读这个可以快速了解一下](https://vsupalov.com/gunicorn-and-nginx/))，但是如果你感兴趣的话，你可以阅读他们的文档。**

****关于自定义域名**:在这一点上，我还没有购买域名，因为我很便宜，但如果你这样做了，你就不用在应用程序上线后更新新的域名了。我在廉价网上买的，因为 Crackshort.co 和。我查过的其他网站(Google domains，GoDaddy，Name.com)都比这里便宜。**

**此外，如果你想保护你的品牌，或确保用户不会拼错你的网址，并最终在其他地方，购买多个域名，并重定向到另一个。我买了 Crackshort.co 和 Crackshort.com，因为我喜欢的声音。co 但是。com 仍然是最受欢迎的。**

**![](img/2953f82c7f51bc6df166b4be59efcbf8.png)**

**廉价主机记录设置(我的 IP 编辑)——来自 Namecheap.com 的截图**

**一旦你购买了你的域名，你必须进入 DNS 设置，如上所述，并**创建主机记录**，将“@”和“www”主机(即 https://crackshort.co 的[和 https://www.crackshort.co](https://www.crackshort.co/))指向你的网站(如果是 DigitalOcean，是 Droplet 的)IP 地址。**

**最后，**一定要设置 SSL 认证**(外行:把你的‘http’改成‘https’)，因为这在当今互联网基本是必须的。对于 DigitalOcean/Ubuntu/NGINX 堆栈，您拥有 shell 访问和超级用户(sudo)权限，因此[使用 Certbot 获得 SSL 非常简单](https://certbot.eff.org)。否则，大多数主机提供商提供 SSL 证书(对于 Namecheap 来说，这是额外的费用)。**

**![](img/ae109b0aa24a61ed300eddfdece6dde3.png)**

**看到左上角那个胜利的 https 了吗？—作者图片**

# **9.更新:维护一个可靠的更新过程**

## **你需要:**

**数字海洋，Django，Git，谷歌**

## **请参考:**

**[这是 StackOverflow 关于更新应用的帖子](https://stackoverflow.com/questions/31532211/updating-django-app-on-server)**

## **结果:**

**一个实时网站，您可以随时更新**

## **时间线:**

**第 34 天—第 37 天**

**Crackshort.co 正在直播。现在你会认为一切都结束了，但事实并非如此。有几件事你需要考虑。**

****首先也是最重要的，版权/copyleft** 。如果你在你的应用程序中使用了开源代码(如果你照着做的话，你肯定会这么做)，在那些库上搜索许可证。在 Github repos 中，这在 About 部分和许可证文件中非常明显。大多数开源代码都在 GPL 许可下，但我不会在这里提供法律建议，所以看看你所使用的是否需要你提供信用或公开你的源代码。这里有一个 GNU 的快速解释。**

# **更新过程**

****二、如何更新你的 app。我在网上找到的关于这方面的资源很少，但是这个过程的重要性不可低估**。这是 DigitalOcean 用户感到困惑的地方，但对 AWS EB 来说并非如此。后者，后端工作打理；在对项目做了一些更改并提交给 Git 之后，您所要做的就是从终端运行一个“eb deploy”命令。**

**如果你在 DigitalOcean 上，后端工作是你的问题，**这是你有纪律地推进 Github 得到回报的地方**。参考“参考”中链接的 StackOverflow 帖子。基本流程是:**

1.  **为本地调试配置 settings.py**
2.  **对您的项目进行更改**
3.  **进行迁移:运行*python manage . py make migrations***
4.  **为实时部署重新配置您的设置**
5.  **将项目提交给本地 Git，并将*推送到远程 Github repo***
6.  **使用 *ssh* 命令登录您的数字海洋虚拟机**
7.  **激活您的项目所使用的虚拟环境**
8.  ***cd* 到您的项目目录中，并*从 Github repo 中拉出***
9.  **迁移:运行 *python manage.py 迁移***
10.  **静态:运行*python manage . py collect static***
11.  **重启 Gunicorn 服务器:运行 *sudo 服务 gunicorn 重启***

****补遗:****

*   **你可以用 Fabric 或 Ansible 来自动化这整个过程，但这不是我的专长，如果你不经常更新，你也不需要这样做**
*   **如果你在设置中设置一个自定义变量，你的生活会变得更轻松。像这样在本地和实时配置之间切换:**

*   **建议**在本地进行迁移，但是在服务器**上迁移，因为迁移是源代码的一部分，不应该在服务器上被篡改；确保本地数据库和服务器数据库相同**
*   **即使在这次更新中没有创建新的静态文件，也只需运行 collectstatic，因为您可能会忘记**
*   **一旦完成更新，您不需要重启 NGINX，因为没有对 web 服务器做任何更改，只是对应用程序做了更改(由 WSGI 服务器负责)**

**因为有这么多的移动部件，而我对每件事都知道得很少，所以我保留了一个. txt，在那里我为自己列出了这些说明。**

# **货币铸造**

**你没有白白地花 37 天，但是如果你想要除了你妈妈之外的任何流量，你需要让你的网站在谷歌上被索引。在谷歌搜索框中输入“site:yoursite.com ”,看看它是否被索引了。在[谷歌搜索控制台](https://search.google.com/search-console/about)上创建一个账户，并按照设置说明进行操作。在这里，您将获得关于您的网站是否被索引和在搜索中排名的基本信息。**

**要“请求”索引，你应该给谷歌你的网站地图。Django 让创建一个网站地图变得非常简单——阅读[网站地图文档](https://docs.djangoproject.com/en/3.0/ref/contrib/sitemaps/)或[跟随本教程](https://django.cowhite.com/blog/creating-a-sitemap-index-in-django/)。请注意，您需要在 Django 项目中设置站点地图和站点应用程序— **如果没有站点，您生成的站点地图将不会链接到您的域**(您将得到一个包含“example.com”的 XML 文件，其中应该包含您的域)。**

**![](img/5ca2eeaedeacda4219251b1e779c7f85.png)**

**你的站点地图 XML 应该是什么样子——作者图片**

**完成后，将您的 sitemap.xml URL 上传到 Sitemaps 下的搜索控制台，然后等待。之后，谷歌至少需要几天时间来发现和索引你的网站，所以你需要耐心。一旦你被编入索引，有很多方法可以让你的网站在搜索中排名更高，但我还是不会深入讨论，因为相关的资源非常丰富。**

**在等待的时候，建立一个[谷歌分析](https://marketingplatform.google.com/about/analytics/)账户。你需要将谷歌的标签代码添加到你的页面中，但这是值得的，因为你将获得访问者的详细信息，以及他们是如何访问你的网站的。**

**除了在谷歌搜索中上市，不要忘记其他营销平台，如社交媒体广告和[产品搜索](https://www.producthunt.com)。**如果你在社交媒体上做广告，一定要小心，不要让你的网站在我的手机浏览器上发疯**。**

**![](img/17948ab914c2b3fe3f14955bfa4854c9.png)**

**谁会在手机上回测交易策略呢？—作者图片**

**有几种方法可以开始用你的网站赚钱。最直接的是 [Google AdSense](https://www.google.com/adsense/start/) ，Google 读取你网站上的内容并自动附上相关广告。然而，这最适合博客；如果你的网站是我这样的 app，上面不会有太多内容，Google AdSense 会拒绝你。**

**您可以研究的其他方法有:**

*   **创建付费墙**
*   **让它免费增值**
*   **销售其他产品(电子书或附属产品)**
*   **直接在市场上销售广告空间(但大多数时候你需要经过验证的流量来赚钱)**
*   **靠快乐用户的捐赠过活**
*   **或者写一篇 6000 字的博客，希望你能从纽约的早餐车上赚到足够买一小杯咖啡的钱**

**![](img/b2b5752d576fd26646173b2926f3838b.png)**

**关于页面的准确性—作者提供的图像**

# **结论**

**如果你在这里成功了，你现在要么是一个全新网站的所有者，要么是我的女朋友。感谢你参加我的 37 天快速跑，我希望你能学到一些东西，让你从零筹码到满筹码的旅程比我的旅程更轻松、更愉快。**

**也就是说，很少有什么事情比看到你的应用被人们使用更令人满足的了。找个时间到 Crackshort.co 来喝杯咖啡吧(你请客)。**

# **主要参考文献**

*   **课程/教程:[成为 Django 开发者](https://www.linkedin.com/learning/paths/become-a-django-developer)(LinkedIn Learning)[在数字海洋上部署 Django](https://www.digitalocean.com/community/tutorials/how-to-set-up-django-with-postgres-nginx-and-gunicorn-on-ubuntu-18-04)**
*   **Docs: [Git](https://git-scm.com/docs/gittutorial) ， [Django](https://www.djangoproject.com/) ， [Bootstrap](https://getbootstrap.com/)**
*   **stack overflow:[SQLite-AWS 版本控制](https://stackoverflow.com/questions/55674176/django-cant-find-new-sqlite-version-sqlite-3-8-3-or-later-is-required-found)， [PostgreSQL-AWS psycopg2](https://stackoverflow.com/questions/33747799/psycopg2-on-elastic-beanstalk-cant-deploy-app/34957820#34957820) ，[用 pip 安装 psycopg 2](https://stackoverflow.com/questions/5420789/how-to-install-psycopg2-with-pip-on-python)，[移动 Django 数据库](https://gist.github.com/sirodoht/f598d14e9644e2d3909629a41e3522ad)，[更新应用](https://stackoverflow.com/questions/31532211/updating-django-app-on-server)**