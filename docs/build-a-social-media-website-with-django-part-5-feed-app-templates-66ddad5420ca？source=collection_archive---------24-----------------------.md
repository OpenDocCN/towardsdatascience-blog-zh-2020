# 用 Django 构建一个社交媒体网站——第 5 部分(Feed 应用程序模板)

> 原文：<https://towardsdatascience.com/build-a-social-media-website-with-django-part-5-feed-app-templates-66ddad5420ca?source=collection_archive---------24----------------------->

## 在第五部分中，我们将着重于为我们在上一个教程中定义的 Feed 应用程序构建模板。

![](img/1bce17cc8050dc0970cdb10a421ff9ac.png)

[图片由弗里皮克](https://www.freepik.com/vectors/background)

因此，在第四部分的[中，我们定义了 Feed 应用程序的所有后端部分，如所有的 URL、视图、模型和表单，并在管理面板中注册了我们的模型。](/build-a-social-media-website-with-django-feed-app-backend-part-4-d82facfa7b3)

因此，在第五部分，我们将关注所有用于显示我们在 Feed 应用程序中编写的逻辑的模板。正如你已经知道的，Feed 应用程序处理帖子、赞、评论和搜索帖子部分。

因此，在这一部分，会有很多 HTML，因为我们正在处理模板的一部分。它将接收 Feed 应用程序的 views.py 文件传递的数据，并将帮助向用户显示数据，并通过表单接收用户输入。

因此，我们将所有模板放在以下位置:feed 文件夹中的 templates/feed。Django 的命名约定是将数据放在那里，因为 Django 在那个位置寻找模板。

我们将逐个处理模板。他们有六个人。我们不会讲太多细节，因为它主要是 HTML 和 CSS 类，我们将只讨论我们在模板中使用的逻辑部分，以定义谁有权访问什么以及哪个部分做什么工作。那么，让我们开始吧:

## **create_post.html**

让我们从 create_post 部分开始。这一部分涉及新职位的设立。该模板将允许用户通过添加相关的详细信息，如图像(必需的)，关于图像的详细信息和标签(可选)，轻松地创建新帖子。

该模板的功能是显示所需的表单，该表单将接受用户输入。有一个提交按钮来保存和提交文章，并将其添加到数据库中。

此外，在导航栏中显示了一个搜索栏，允许您搜索带有相关标签的特定帖子。

该模板扩展了 Feed 的所有其他模板和用户模板正在使用的布局模板。我们很快就会看到`layout.html`文件。

## home.html

它还扩展了 layout.html 文件。这是我们应用程序的主页。任何登录的用户都将被重定向到此页面。该页面也无需登录即可访问。

它也有同样的搜索栏来搜索文章。这个主页的主要目的是以相关的方式显示文章。

它还检查用户是否喜欢某个帖子，并根据需要显示“喜欢”或“不喜欢”按钮。

它还有分页功能，每页只显示 10 篇文章。它也有下一页和上一页的链接。

它还有 AJAX 脚本，无需刷新页面就能实时处理赞。一旦用户喜欢它，它就把喜欢按钮变成不喜欢按钮，反之亦然。它改变按钮的颜色来表示相同的颜色。

## layout.html

这是布局文件。它导入我们在网站上使用的所有需要的 CSS 和 JS 文件。我们使用 Bootstrap4 和一些自定义的 CSS 文件。

接下来，我们有一个 Navbar，它显示登录用户和未登录用户的相关链接。这样做是为了只显示所需的链接。

例如，我们将向已经登录的用户显示注销链接，并向未经认证的用户显示登录和注册链接。

我们还有一个显示名称和版权信息的小页脚。

你会注意到我们有一个*{ % block content % } { % end block content % }，*，它保存了我们在其他页面上的所有内容信息。

同样，我们也有一个*搜索模块*，用于搜索帖子或人物部分。

## post_detail.html

这个模板详细处理了文章的显示。它显示了完整的帖子详细信息，以及喜欢和评论数。它还有一个发布评论的部分(显示评论表单)，下面是显示所有评论的部分。

此外，还有更新和删除帖子的链接。只有当此人与帖子的所有者相同时，它才会显示这些链接。否则，它不会向任何其他人显示这些按钮。

它也有相同的 AJAX 脚本来处理喜欢和不喜欢，无需刷新。

## search_posts.html

这部分显示与搜索词匹配的文章。它显示所有相关的文章(搜索结果)。

这类似于主页，因为我们需要显示帖子，但不同于主页，主页显示所有帖子，这里我们显示匹配搜索词的选择性帖子。

所有其他组件都与主页相同，如 AJAX 脚本来处理喜欢和显示风格。

## user_posts.html

此页面显示特定用户的帖子。它几乎与带有分页的主页相同，使用相同的 AJAX 脚本来处理赞，以及相同的帖子显示风格。

唯一的区别是它只显示特定用户的帖子。

这就是 Feed 应用程序模板的全部内容。有很多 HTML 内容。希望你不会觉得无聊。

希望你喜欢这个教程。这是 Django 社交媒体教程五部分系列的最后一部分。

我希望你们都喜欢这个完整的系列。我希望你们都能够用 Django 制作自己的社交媒体项目，并可以添加更多方面，如标记、仅通过朋友圈显示帖子、增强的推荐系统等等。

完整系列的 [Github 回购](https://github.com/shubham1710/ByteWalk)在这里是。请随意对该存储库发表评论或提出任何拉取请求，这将有助于以任何方式增强它。

一个基于 Django Rest 框架的新系列，您应该看看它以增强您的 Django 技能:

[](/build-a-blog-website-using-django-rest-framework-overview-part-1-1f847d53753f) [## 使用 Django Rest 框架构建博客网站——概述(第 1 部分)

### 让我们使用 Django Rest 框架构建一个简单的博客网站，以了解 DRF 和 REST APIs 是如何工作的，以及我们如何添加…

towardsdatascience.com](/build-a-blog-website-using-django-rest-framework-overview-part-1-1f847d53753f) 

一个类似的以 Django 为中心的系列(建立一个求职门户)将教你一些惊人的新概念，它是:

[](https://shubhamstudent5.medium.com/build-a-job-search-portal-with-django-overview-part-1-bec74d3b6f4e) [## 用 Django 构建求职门户——概述(第 1 部分)

### 让我们使用 Django 建立一个工作搜索门户，它允许招聘人员发布工作并接受候选人，同时…

shubhamstudent5.medium.com](https://shubhamstudent5.medium.com/build-a-job-search-portal-with-django-overview-part-1-bec74d3b6f4e) 

前四部分可以在这里找到:-

[](/build-a-social-media-website-using-django-setup-the-project-part-1-6e1932c9f221) [## 使用 Django 构建一个社交媒体网站——设置项目(第 1 部分)

### 在第一部分中，我们通过设置密码来集中设置我们的项目和安装所需的组件…

towardsdatascience.com](/build-a-social-media-website-using-django-setup-the-project-part-1-6e1932c9f221) [](/build-a-social-media-website-with-django-users-app-part-2-7f0c0431ccdc) [## 用 Django-Users App 建立一个社交媒体网站(第二部分)

### 在这一部分中，我们将重点创建用户应用程序以及与用户配置文件、身份验证等相关的所有模型

towardsdatascience.com](/build-a-social-media-website-with-django-users-app-part-2-7f0c0431ccdc) [](/build-a-social-media-website-with-django-part-3-users-app-templates-c87ad46682be) [## 用 Django 构建一个社交媒体网站——第 3 部分(用户应用模板)

### 在第三部分中，我们将着重于为我们在上一个教程中定义的用户应用程序构建模板。

towardsdatascience.com](/build-a-social-media-website-with-django-part-3-users-app-templates-c87ad46682be) [](/build-a-social-media-website-with-django-feed-app-backend-part-4-d82facfa7b3) [## 用 Django 构建一个社交媒体网站——Feed 应用后端(第 4 部分)

### 在这一部分中，我们将重点放在构建 Feed 应用程序及其所有与喜欢、评论、帖子和所有相关的模型上

towardsdatascience.com](/build-a-social-media-website-with-django-feed-app-backend-part-4-d82facfa7b3) 

您可能喜欢的其他文章有:-

[](https://medium.com/javascript-in-plain-english/build-a-rest-api-with-node-express-and-mongodb-937ff95f23a5) [## 用 Node，Express 和 MongoDB 构建一个 REST API

### 让我们使用 Node、Express 和 MongoDB 构建一个遵循 CRUD 原则的 REST API，并使用 Postman 测试它。

medium.com](https://medium.com/javascript-in-plain-english/build-a-rest-api-with-node-express-and-mongodb-937ff95f23a5) [](https://medium.com/javascript-in-plain-english/build-a-simple-todo-app-using-react-a492adc9c8a4) [## 使用 React 构建一个简单的 Todo 应用程序

### 让我们用 React 构建一个简单的 Todo 应用程序，它教你 CRUD 的基本原理(创建、读取、更新和…

medium.com](https://medium.com/javascript-in-plain-english/build-a-simple-todo-app-using-react-a492adc9c8a4)