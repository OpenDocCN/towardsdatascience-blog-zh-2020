# 用 Django 构建一个社交媒体网站——第 3 部分(用户应用模板)

> 原文：<https://towardsdatascience.com/build-a-social-media-website-with-django-part-3-users-app-templates-c87ad46682be?source=collection_archive---------20----------------------->

## 在第三部分中，我们将着重于为我们在上一个教程中定义的用户应用程序构建模板。

![](img/1bce17cc8050dc0970cdb10a421ff9ac.png)

[图片由 Freepik 拍摄](https://www.freepik.com/vectors/background)

因此，在[的第二部分](/build-a-social-media-website-with-django-users-app-part-2-7f0c0431ccdc)，我们定义了用户应用程序的所有后端部分，就像我们定义了所有的 URL、视图、模型和表单，并向管理面板注册了我们的模型。

所以，现在在第三部分，我们将关注所有显示我们在用户应用程序中编写的逻辑的模板。正如你已经知道的，用户应用程序处理所有的认证，个人资料，朋友，搜索用户，寻找新朋友和其他事情。

因此，在这一部分，会有很多 HTML，因为我们正在处理模板的一部分。它将接收用户应用程序的 views.py 文件传递的数据，并将帮助向用户显示数据，并通过表单接收用户输入。

因此，我们将所有模板放在以下位置:用户文件夹中的模板/用户。Django 的命名约定是将数据放在那里，因为 Django 在那个位置寻找模板。

我们将逐个处理模板。他们有 12 个人。我们不会详细讨论，因为它主要是 HTML 和 CSS 类，我们将只讨论逻辑部分，我们在模板中使用它来定义谁可以访问什么。那么，让我们开始吧:

## login.html

先说登录部分。这个模板将让用户登录到我们的网站。这将显示登录表单，并且我们正在使用 crispy 表单来使用 Bootstrap4 格式化我们的表单。在 Django 中，表单非常容易呈现。

我们有一个提交按钮和忘记密码选项，此外，我们有一个注册页面的链接。

我们正在从我们的 Feeds 应用程序扩展布局模板。每个模板都扩展了包含页眉和页脚部分的布局。现在不要担心 layout.html 文件，因为我们将在本系列教程的第 5 部分中讨论它。

## register.html

接下来，我们转到网站的注册页面。这个页面将让用户注册进入我们的网站。它还显示表单，然后显示提交按钮。我们有一个现有用户登录页面的链接。

## logout.html

接下来，我们有注销页面。它只是显示一条消息，告诉用户他们已经从网站注销。他们可以选择再次登录。

## 请求 _ 密码 _ 重置. html

这是让用户请求密码重置的表单。用户应该提交他们的电子邮件地址，并点击重置按钮。然后，他们会收到一个链接到他们的电子邮件地址。

## password _ 重置 _ 完成. html

此页面显示重置链接已发送至其电子邮件地址的消息。我们还为用户提供了一个打开 Gmail 的按钮。

## 密码 _ 重置 _ 确认. html

当您单击发送给您的重置链接时，将打开此页面。您可以在此处设置新密码，然后点击按钮更改密码。

## 密码 _ 重置 _ 完成. html

该页面显示用户的密码已被重置，现在可以使用他的新凭据登录。

## profile.html

这可能是最复杂的一个，有很多东西要放进去。首先，我们可以看到 *{% block searchform %}，*它有一个搜索栏来搜索用户。它显示在网站的导航栏中，不是 profile.html 的一部分。它只是扩展了 navbar 部分，这样搜索表单就不会显示在每一页上。

现在，来到个人资料部分，我们可以搜索；首先，我们有一张卡片，显示我们的个人资料，如个人资料照片、朋友数量和分享的帖子数量。

现在，我们已经设置了一个限制，只有我们正在查看其个人资料的用户才能查看好友列表，其他任何人都不能查看。任何人都可以看到共享的帖子。因此，我们在这里做了一个检查( *{% if request.user == u%})，*，检查当前用户是否等于正在查看其配置文件的用户。

接下来，如果当前用户的个人资料正在被查看，我们可以选择*编辑个人资料*。这限制了其他任何人更新我们的个人资料。

我们也有各种选项，如*添加好友，取消请求，拒绝请求，接受请求，根据条件取消好友*。

1.  *添加好友—* 如果用户不是我们的好友，我们可以向他发送好友请求。
2.  *取消请求—* 如果我们已经向用户发送了请求，我们可以取消它。
3.  *接受请求—* 如果用户向我们发送了好友请求，我们可以接受。
4.  *拒绝请求—* 如果用户向我们发送了好友请求，我们可以拒绝。
5.  *解除好友关系—* 如果用户已经是我们的好友，并希望将其从我们的好友中移除，我们可以解除他的好友关系。

如果当前用户是其个人资料正在被查看的用户，我们将得到下面两个列表。

1.  *已发送的好友请求—* 这些包含我们已向其发送请求的所有用户。我们可以从那里取消请求。
2.  收到的好友请求— 它包含了我们收到的所有请求。我们可以接受或拒绝他们。

## **edit_profile.html**

此页面包含我们更新个人资料所需的表格。我们可以更新我们的电子邮件，用户名，个人资料图片和简历。它有一个提交按钮来更新个人资料，并带我们回到个人资料页面。

## 朋友列表. html

在这里，我们显示我们所有的朋友，我们可以选择解除他们的好友关系。我们所有的朋友都显示为列表。我们还在边上显示了一张包含当前用户信息的卡片。

## search _ users.html

此页面显示当我们使用搜索栏按用户名称搜索用户时出现的所有用户。这个列表包含所有的搜索结果，我们有一个选项。我们还在边上显示了一张包含当前用户信息的卡片。

## users _ list.html

此列表包含推荐添加为好友的用户。是为了寻找和发现新的人，增加你的朋友。我们也可以选择添加朋友或取消所有这些用户的请求。我们还在侧面展示了一张包含当前用户信息的卡片。

这就是用户应用程序模板的全部内容。有很多 HTML 内容。希望你不会觉得无聊。但是我们必须这样做来保持教程的连贯性。我们现在剩下两个部分，将讨论 Feeds 应用程序及其模板。这些将处理所有的帖子，喜欢，评论等的逻辑和渲染。

我希望你喜欢阅读教程。非常感谢！

一个基于 Django Rest 框架的新系列，您应该看看它以增强您的 Django 技能:

[](/build-a-blog-website-using-django-rest-framework-overview-part-1-1f847d53753f) [## 使用 Django Rest 框架构建博客网站——概述(第 1 部分)

### 让我们使用 Django Rest 框架构建一个简单的博客网站，了解 DRF 和 REST APIs 如何工作，以及我们如何添加…

towardsdatascience.com](/build-a-blog-website-using-django-rest-framework-overview-part-1-1f847d53753f) 

一个类似的以 Django 为中心的系列(建立一个求职门户)将教你一些惊人的新概念，它是:

[](https://shubhamstudent5.medium.com/build-a-job-search-portal-with-django-overview-part-1-bec74d3b6f4e) [## 用 Django 构建求职门户——概述(第 1 部分)

### 让我们使用 Django 建立一个工作搜索门户，它允许招聘人员发布工作并接受候选人，同时…

shubhamstudent5.medium.com](https://shubhamstudent5.medium.com/build-a-job-search-portal-with-django-overview-part-1-bec74d3b6f4e) 

教程的下一部分是:-

[](https://shubhamstudent5.medium.com/build-a-social-media-website-with-django-feed-app-backend-part-4-d82facfa7b3) [## 用 Django 构建一个社交媒体网站——Feed 应用后端(第 4 部分)

### 在这一部分中，我们将重点放在构建 Feed 应用程序及其所有与喜欢、评论、帖子和所有相关的模型上

shubhamstudent5.medium.com](https://shubhamstudent5.medium.com/build-a-social-media-website-with-django-feed-app-backend-part-4-d82facfa7b3) [](/build-a-social-media-website-with-django-part-5-feed-app-templates-66ddad5420ca) [## 用 Django 构建一个社交媒体网站——第 5 部分(Feed 应用程序模板)

### 在第五部分中，我们将着重于为我们在上一个教程中定义的 Feed 应用程序构建模板。

towardsdatascience.com](/build-a-social-media-website-with-django-part-5-feed-app-templates-66ddad5420ca) 

学习 Web 开发的资源集合可以在这里找到

[](https://medium.com/javascript-in-plain-english/build-a-rest-api-with-node-express-and-mongodb-937ff95f23a5) [## 用 Node，Express 和 MongoDB 构建一个 REST API

### 让我们使用 Node、Express 和 MongoDB 构建一个遵循 CRUD 原则的 REST API，并使用 Postman 测试它。

medium.com](https://medium.com/javascript-in-plain-english/build-a-rest-api-with-node-express-and-mongodb-937ff95f23a5) [](https://medium.com/javascript-in-plain-english/build-a-simple-todo-app-using-react-a492adc9c8a4) [## 使用 React 构建一个简单的 Todo 应用程序

### 让我们用 React 构建一个简单的 Todo 应用程序，它教你 CRUD 的基本原理(创建、读取、更新和…

medium.com](https://medium.com/javascript-in-plain-english/build-a-simple-todo-app-using-react-a492adc9c8a4)