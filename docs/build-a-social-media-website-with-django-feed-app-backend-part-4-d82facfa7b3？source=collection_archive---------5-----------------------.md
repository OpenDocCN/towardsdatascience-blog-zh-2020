# 用 Django 构建一个社交媒体网站——Feed 应用后端(第 4 部分)

> 原文：<https://towardsdatascience.com/build-a-social-media-website-with-django-feed-app-backend-part-4-d82facfa7b3?source=collection_archive---------5----------------------->

## 在这一部分中，我们将重点构建 Feed 应用程序及其所有与赞、评论、帖子以及所有相关表单和视图相关的模型。

![](img/1bce17cc8050dc0970cdb10a421ff9ac.png)

[图片由 Freepik 拍摄](https://www.freepik.com/vectors/background)

所以，在教程系列的[第二部](/build-a-social-media-website-with-django-users-app-part-2-7f0c0431ccdc)和[第三部](/build-a-social-media-website-with-django-part-3-users-app-templates-c87ad46682be)中，我们已经分别讨论了用户 app 后端及其模板。

所以，在这一部分(第四部)，我们将开始讨论 Feed app。在这一部分，我们将重点关注 Feed 应用程序的后端(模型、视图、表单等)。)，我们将在下一部分讨论 Feed 应用程序的模板。

在继续之前，我想确保您已经阅读了本系列前面的所有部分；否则，阅读这篇文章就没什么意义了，尽管如果你只想关注具体细节而不是整个网站，你也可以这么做。

正如我在前面的部分中提到的，我不会详细讨论各种术语，因为这太长了。我会把重点放在代码的主要方面，而不是沉迷于简单的术语。

为了更好地理解一切，您应该熟悉 Django 术语。希望你能在第二部分中熟悉他们，但是如果你有疑问，Django 的官方网站是一个了解他们的好地方。

因此，让我们继续构建我们在教程的第一部分中创建的 Feed 应用程序。我们将首先从创建模型开始。

## models.py

在这个 python 文件中，我们将定义我们的模型。您可能已经知道，定义模型是为了告诉 Django 在数据库中保存什么，并定义不同模型之间的关系以及它们有什么特征。

要了解更多关于 Django 模型的信息，请访问这个由 Mozilla 开发者[撰写的关于模型的精彩教程](https://developer.mozilla.org/en-US/docs/Learn/Server-side/Django/Models)。它深入讨论了模型。

当你对模型的工作方式感到满意后，你可以继续为我们的社交媒体网站制作模型。

因此，在这个 Feed 应用程序中，我们将有三个模型——一个用于帖子，一个用于评论，最后一个用于赞。

因此，像往常一样，我们将不得不首先导入各种项目。这将包括默认的 Django 用户模型和时区。

那么，我们来看看我们的第一个模型——**Post**模型。它有五个参数:-

1.  描述——这是帖子的一部分，用户可以在这里输入与他发布的图片相关的小描述。它是可选的，因为我们不想强迫用户输入描述。它的最大长度为 255 个字符，是一个 CharField。
2.  pic——这是帖子最重要的部分——图片。用户将上传他们选择的图片进行上传。它将保存在提到的文件路径中。它使用一个 ImageField。
3.  date _ posted——它将使用 Django 的 datetime 字段，并将为每个帖子设置时间戳。我们将使用默认时间作为当前时间。
4.  用户名-这是外键关系。这是一个多对一的关系，因为一个用户可以有很多帖子，但是一个帖子只能属于一个用户。当用户被删除时，文章也会被删除，*on _ delete = models . cascade .*的用法证明了这一点，它将文章与用户模型联系起来。
5.  标签——这用于接收帖子的相关标签。它可以留空，最多 100 个字符。标签可以帮助搜索相关的文章。

接下来，我们描述 *__str__，*，它决定 Django 如何在管理面板中显示我们的模型。我们将它设置为显示描述作为查询对象。

我们还定义了 *get_absolute_url* 来获取这篇文章的绝对 url。

接下来，我们有**评论**模型。它有四个参数:-

1.  post-这是连接帖子和评论的外键。一条评论可以针对一篇文章，但是一篇文章可以有多条评论。删除帖子也会删除评论。
2.  用户名—这是一个外键，将注释与用户相关联。删除用户时，评论也将被删除。
3.  comment —这是保存相关注释的 CharField。它的最大字符限制为 255 个字符。
4.  comment _ date——它将使用 Django 的 datetime 字段，并将为每个注释设置时间戳。我们将使用默认时间作为当前时间。

接下来，我们有了最终的模型— **Likes。它有两个参数:-**

1.  用户——表示喜欢帖子的用户。删除用户会删除类似内容。
2.  张贴——是张贴类似内容的帖子。删除帖子也会删除所有喜欢的帖子。

所以，这就总结了我们的 *models.py* 文件。让我们看一下代码:-

## 管理. py

它会很短，只有几行字。它表示我们将注册到我们的管理面板的模型。我们将在这里注册我们所有的三个模型。

## **forms.py**

要了解 Django 中表单工作的更多信息，请访问 Django 自己编写的官方教程。然后继续学习教程。

我们在 *forms.py* 文件中定义了两个表单。

1.  NewPostForm —这是任何用户发布新帖子的地方。它接受三个字段，即描述、字段和标签。保存时提供了 user_name，因为不会向用户询问它。
2.  NewCommentForm —类似于 NewPostForm，我们有这个表单来接受新的评论。我们只接受要发布的评论，稍后提供帖子和用户。

## view.py

现在，我们将定义 views.py 文件。它将包含我们所有的视图(如何在 web 浏览器中呈现文件)。它直接将数据传递给模板。

阅读 Django 的官方教程，以更好地理解观点。看完教程，我们继续前进。

我们将首先在我们的 *views.py* 文件中导入所有我们需要的东西。

由于视图文件太大，我们可以根据自己的喜好来创建它，所以我将给出每个视图的简单概述，您可以阅读下面的代码来更好地理解。所以，让我们一个一个来看:

1.  post listview——这个视图处理所有文章的显示，顺序是新文章放在最前面。每页显示 10 篇文章，所以我们需要移动到下一页查看更多。此外，如果用户没有通过身份验证，我们不会给他喜欢这个帖子的选项。如果用户被认证，我们显示用户是否喜欢它。
2.  UserPostListView —该视图几乎类似于 PostListView。排序和分页是相同的。唯一的区别是这个页面只显示某个用户的帖子。
3.  post_detail —该视图处理单个帖子的显示。它还显示评论表单，让用户对文章进行评论，并显示所有评论。它还显示喜欢计数，并允许您喜欢或不喜欢。
4.  create_post —该视图处理新帖子的创建。它保存数据并将当前用户添加为用户名，然后提交它。
5.  PostUpdateView —该视图处理帖子的更新。我们可以在这个视图的帮助下编辑我们的文章。
6.  post_delete —该视图处理帖子的删除。当用户需要时，它会删除帖子。
7.  search _ posts——这与我们在[用户应用后端(第二部分)](/build-a-social-media-website-with-django-users-app-part-2-7f0c0431ccdc)中的 search_users 的工作方式类似。它接收输入，并根据标签搜索帖子。
8.  like —该视图处理帖子的 like 事件。这是在 AJAX 请求的帮助下完成的，这样页面就不会在每次用户喜欢或不喜欢时刷新。它的工作方式是，如果帖子已经被用户喜欢，点击喜欢按钮会删除喜欢，但如果它还没有被喜欢，它会喜欢这个帖子。然后，它将响应作为 JSON 转储，并作为 HTTP 响应传递。

这总结了 views.py 文件。下面是该文件的代码。

## urls.py

该文件包含 Feed 应用程序的所有 URL。所有这些 URL 都将包含在主 *urls.py* 文件中。

下面是提要应用程序的 *urls.py* 文件的代码。

如果你想看到完整的代码，查看[项目的 Github Repo](https://github.com/shubham1710/ByteWalk) 。另外，你可以通过访问[这个链接](http://bytewalk.me/)来试用这个应用程序。它托管在 Heroku 上，媒体和静态文件在 Google 云存储上。

教程的下一部分:-

[](https://shubhamstudent5.medium.com/build-a-social-media-website-with-django-part-5-feed-app-templates-66ddad5420ca) [## 用 Django 构建一个社交媒体网站——第 5 部分(Feed 应用程序模板)

### 在第五部分中，我们将着重于为我们在上一个教程中定义的 Feed 应用程序构建模板。

shubhamstudent5.medium.com](https://shubhamstudent5.medium.com/build-a-social-media-website-with-django-part-5-feed-app-templates-66ddad5420ca) 

一个基于 Django Rest 框架的新系列，您应该看看它以增强您的 Django 技能:

[](/build-a-blog-website-using-django-rest-framework-overview-part-1-1f847d53753f) [## 使用 Django Rest 框架构建博客网站——概述(第 1 部分)

### 让我们使用 Django Rest 框架构建一个简单的博客网站，了解 DRF 和 REST APIs 如何工作，以及我们如何添加…

towardsdatascience.com](/build-a-blog-website-using-django-rest-framework-overview-part-1-1f847d53753f) 

一个类似的以 Django 为中心的系列(建立一个求职门户)将教你一些惊人的新概念，它是:

[](https://shubhamstudent5.medium.com/build-a-job-search-portal-with-django-overview-part-1-bec74d3b6f4e) [## 用 Django 构建求职门户——概述(第 1 部分)

### 让我们使用 Django 建立一个工作搜索门户，它允许招聘人员发布工作并接受候选人，同时…

shubhamstudent5.medium.com](https://shubhamstudent5.medium.com/build-a-job-search-portal-with-django-overview-part-1-bec74d3b6f4e) 

React 上一篇很棒的文章是:

[](https://medium.com/javascript-in-plain-english/build-a-simple-todo-app-using-react-a492adc9c8a4) [## 使用 React 构建一个简单的 Todo 应用程序

### 让我们用 React 构建一个简单的 Todo 应用程序，它教你 CRUD 的基本原理(创建、读取、更新和…

medium.com](https://medium.com/javascript-in-plain-english/build-a-simple-todo-app-using-react-a492adc9c8a4) [](https://medium.com/javascript-in-plain-english/build-a-rest-api-with-node-express-and-mongodb-937ff95f23a5) [## 用 Node，Express 和 MongoDB 构建一个 REST API

### 让我们使用 Node、Express 和 MongoDB 构建一个遵循 CRUD 原则的 REST API，并使用 Postman 测试它。

medium.com](https://medium.com/javascript-in-plain-english/build-a-rest-api-with-node-express-and-mongodb-937ff95f23a5)