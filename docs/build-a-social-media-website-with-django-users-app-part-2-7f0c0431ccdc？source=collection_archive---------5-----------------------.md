# 用 Django 构建一个社交媒体网站——用户应用后端(第 2 部分)

> 原文：<https://towardsdatascience.com/build-a-social-media-website-with-django-users-app-part-2-7f0c0431ccdc?source=collection_archive---------5----------------------->

## 在这一部分中，我们重点创建用户应用程序和所有与用户配置文件、身份验证、朋友模型等相关的模型。

![](img/1bce17cc8050dc0970cdb10a421ff9ac.png)

[图片由 Freepik 拍摄](https://www.freepik.com/vectors/background)

因此，在教程的[第一部分](/build-a-social-media-website-using-django-setup-the-project-part-1-6e1932c9f221)中，我们学习了如何设置我们的项目，并设置各种认证后端和其他关于设置文件中所需的各种安装和应用程序设置的细节。

如果你还没有阅读[的第一部分](/build-a-social-media-website-using-django-setup-the-project-part-1-6e1932c9f221)，请确保在继续之前先完成它，因为我们将在这一部分中以它为基础。

可能会有很多你第一次遇到的术语，比如你可能第一次听说过视图或模型，所以我会简单介绍一下。我不能详细谈论这些，因为我们的重点不是教你基本的概念，而是建立一个社交媒体网站。因此，我将链接到一些好的资源，供您参考这些特定的主题，然后继续本教程。

因此，让我们继续构建我们在上一个教程中创建的用户应用程序。我们将首先从创建模型开始。

## models.py

在这个 python 文件中，我们将定义我们的模型。您可能已经知道，定义模型是为了告诉 Django 在数据库中保存什么，并定义不同模型之间的关系以及它们有什么特征。

要了解更多关于 Django 模型的信息，请访问这个由 Mozilla 开发者[撰写的关于模型的精彩教程](https://developer.mozilla.org/en-US/docs/Learn/Server-side/Django/Models)。它深入讨论了模型。

当你对模型的工作方式感到满意后，你可以继续为我们的社交媒体网站制作模型。

因此，我们将有两个模型——一个用于用户的个人资料，另一个用于好友请求。

在开始编写模型之前，我们需要导入许多东西。我们将使用 Django 的默认用户模型与我们的配置文件模型建立一对一的关系，即每个用户都有一个配置文件。

我们还使用 *autoslug* 来根据用户名自动生成 URL 的 slug。

例如，姓名为 Tom 的用户会将 slug 设为 *tom。*这将有助于我们制作有意义的 URL，因为*用户/tom* 将指向 tom 的个人资料，而不是数字。

所以，要使用 *autoslug，*我们需要先安装它。这可以通过简单的 pip 安装来完成。参考下文:

```
pip install django-autoslug
```

此外，我们需要安装 pillow 库来处理 Profile 模型中的图像。

```
pip install pillow
```

安装后，我们可以使用以下代码行将其导入 models.py:

```
from autoslug import AutoSlugField
```

在完成所有必需的导入之后，我们可以开始编写模型了。

所以，我们的第一个模型是**剖面**模型。它有五个参数:-

1.  **用户** —这是与 Django 用户模型的一对一关系。 *on_delete=models。CASCADE* 表示删除用户，我们也会销毁档案。
2.  **图片** —这将存储用户的个人资料图片。我们还提供了一个默认图像。我们需要定义保存图片的位置。
3.  **段塞** —这将是段塞字段。我们使用 AutoSlugField，并将它设置为从用户字段生成一个 slug。
4.  **bio** —这将存储一个关于用户的小介绍。这里， *blank=True* 表示可以留空。
5.  **朋友** —这是一个带有配置文件模型的多对多字段，可以留空。这意味着每个用户可以有多个朋友，并且可以与多人成为朋友。

接下来，我们描述 *__str__，*，它决定 Django 如何在管理面板中显示我们的模型。我们将它设置为显示用户名作为查询对象。

我们还定义了 *get_absolute_url* 来获取这个概要文件的绝对 url。

接下来，我们定义一个函数，在创建用户后立即创建一个概要文件，这样用户就不必手动创建概要文件。

接下来，我们定义我们的**朋友**模型。它有三个参数:-

1.  **to_user** —这表示好友请求将被发送到的用户。它将具有相同的 on_delete 参数，该参数决定当用户被删除时，我们也删除朋友请求。
2.  **from_user** —这表示发送好友请求的用户。如果用户被删除，它也将被删除。
3.  **时间戳** —不需要添加。它存储发送请求的时间。

正如你所注意到的，to_user 和 from_user 使用相同的外键，所以为了区分，我们需要使用 *related_name* 字段。

这样，我们的 *models.py* 文件就完成了。看看下面的代码，它显示了 models.py 文件。

在 *models.py* 文件之后，我们前进到 *admin.py* 文件。

## 管理. py

它会很短，只有几行字。它表示我们将注册到我们的管理面板的模型。我们将在这里注册我们的模特。

接下来，我们移动到 *forms.py.*

## forms.py

要了解 Django 中表单工作的更多信息，请访问 Django 自己编写的官方教程。然后继续学习教程。

我们在 *forms.py* 文件中定义了三个表单。

1.  **用户注册表单** —用于新用户的注册。我们使用 Django 的默认 UserCreationForm，定义表单中应该包含的内容。我们将电子邮件*设置为 Django 的 EmailField。然后我们告诉 Django，模型是 User，我们会要求用户在注册时填写字段。*
2.  UserUpdateForm —该表单将允许用户更新他们的个人资料。它将具有与注册表单相同的字段，但是我们将使用 Django 模型表单，而不是 UserCreationForm。
3.  **ProfileUpdateForm** —该表单将允许用户更新他们的个人资料。

因此，添加这三个表单将完成我们的 *forms.py* 文件。看看下面的代码:

因此，在这之后，我们创建了我们的 *forms.py* 文件。接下来，我们将看到 *views.py* 文件。

## views.py

现在，我们将定义 views.py 文件。它将包含我们所有的视图(如何在 web 浏览器中呈现文件)。它直接将数据传递给模板。

阅读 Django 的官方教程[可以更好地理解观点。看完教程，我们继续前进。](https://docs.djangoproject.com/en/3.1/topics/http/views/)

由于视图文件太大，我们可以根据自己的喜好来创建它，所以我将给出每个视图的简单概述，您可以阅读下面的代码来更好地理解。所以，让我们一个一个来看:

1.  **users_list** —这个视图将形成推荐给任何用户的用户列表，帮助他们发现可以交朋友的新用户。我们将从列表中过滤掉我们的朋友，也将我们排除在外。我们将首先添加我们朋友的朋友，他们不是我们的朋友。然后如果我们的用户列表仍然有低会员，我们会随机添加人来推荐(主要针对一个没有朋友的用户)。
2.  **好友列表** —该视图将显示用户的所有好友。
3.  这将帮助我们创建一个好友请求实例，并向用户发送一个请求。我们接收向其发送请求的用户的 id，这样我们就可以向他发送请求。
4.  **cancel _ friend _ request**—它将取消我们发送给用户的好友请求。
5.  **accept _ friend _ request**—它将用于接受用户的好友请求，我们将用户 1 添加到用户 2 的好友列表中，反之亦然。此外，我们将删除好友请求。
6.  **delete_friend_request** —允许用户删除他/她收到的任何好友请求。
7.  **delete_friend** —这将删除该用户的朋友，也就是说，我们将从用户 2 的朋友列表中删除用户 1，反之亦然。
8.  **个人资料视图** —这将是任何用户的个人资料视图。它将显示朋友的计数和用户及其朋友列表的帖子计数。此外，它将显示用户接收和发送的朋友请求，并可以接受，拒绝或取消请求。我们将添加条件和检查，以便只有有关的用户显示请求和发送列表，他们只有权力接受或拒绝请求，而不是任何人查看他/她的个人资料。
9.  注册 —这将让用户在我们的网站上注册。它将呈现我们在 forms.py 文件中创建的注册表单。
10.  **edit_profile** —这将让用户在我们创建的表单的帮助下编辑他们的个人资料。
11.  **search_users** —这将处理用户的搜索功能。它接受查询，然后过滤掉相关用户。
12.  **my_profile** —这与 profile_view 相同，但它将仅呈现您的个人资料。

为了更好地理解，请仔细阅读下面的代码。

它总结了我们的用户应用程序。我们剩下的是 *urls.py，*我们不会将它们包含在用户应用程序中。我们会将它直接添加到我们的*照片分享*应用程序中。

你可以查看这个[简单教程](https://tutorial.djangogirls.org/en/django_urls/)来了解更多关于 Django 中的 URL。

## urls.py(照片共享)

该文件包含网站的所有 URL。它有一个 *include('feed.urls')* ，包含了 feed 应用程序的所有 URL，我们将在下一个教程中构建。

我们将 photoshare 应用程序的所有 URL 直接包含在主 urls.py 文件中。请看下面的文件:

如果你想看到完整的代码，查看[项目的 GitHub Repo](https://github.com/shubham1710/ByteWalk) 。此外，您可以通过访问[此链接](http://bytewalk.me)来试用此应用程序。它托管在 Heroku 上，媒体和静态文件在 Google 云存储上。

一个类似的以 Django 为中心的系列(建立一个求职门户)将教你一些惊人的新概念，它是:

[](https://shubhamstudent5.medium.com/build-a-job-search-portal-with-django-overview-part-1-bec74d3b6f4e) [## 用 Django 构建求职门户——概述(第 1 部分)

### 让我们使用 Django 建立一个工作搜索门户，它允许招聘人员发布工作并接受候选人，同时…

shubhamstudent5.medium.com](https://shubhamstudent5.medium.com/build-a-job-search-portal-with-django-overview-part-1-bec74d3b6f4e) 

如果您想在 Django 的基础上更进一步，学习 Django Rest 框架，这个新系列会让您兴奋不已:

[](/build-a-blog-website-using-django-rest-framework-overview-part-1-1f847d53753f) [## 使用 Django Rest 框架构建博客网站——概述(第 1 部分)

### 让我们使用 Django Rest 框架构建一个简单的博客网站，以了解 DRF 和 REST APIs 是如何工作的，以及我们如何添加…

towardsdatascience.com](/build-a-blog-website-using-django-rest-framework-overview-part-1-1f847d53753f) 

本教程的下一部分是:

[](https://medium.com/@shubhamstudent5/build-a-social-media-website-with-django-part-3-users-app-templates-c87ad46682be) [## 用 Django 构建一个社交媒体网站——第 3 部分(用户应用模板)

### 在第三部分中，我们将着重于为我们在上一个教程中定义的用户应用程序构建模板。

medium.com](https://medium.com/@shubhamstudent5/build-a-social-media-website-with-django-part-3-users-app-templates-c87ad46682be) [](https://shubhamstudent5.medium.com/build-a-social-media-website-with-django-feed-app-backend-part-4-d82facfa7b3) [## 用 Django 构建一个社交媒体网站——Feed 应用后端(第 4 部分)

### 在这一部分中，我们将重点放在构建 Feed 应用程序及其所有与喜欢、评论、帖子和所有相关的模型上

shubhamstudent5.medium.com](https://shubhamstudent5.medium.com/build-a-social-media-website-with-django-feed-app-backend-part-4-d82facfa7b3) [](/build-a-social-media-website-with-django-part-5-feed-app-templates-66ddad5420ca) [## 用 Django 构建一个社交媒体网站——第 5 部分(Feed 应用程序模板)

### 在第五部分中，我们将着重于为我们在上一个教程中定义的 Feed 应用程序构建模板。

towardsdatascience.com](/build-a-social-media-website-with-django-part-5-feed-app-templates-66ddad5420ca) 

本系列文章之后还有一些文章可供阅读:

[](https://medium.com/javascript-in-plain-english/build-a-simple-todo-app-using-react-a492adc9c8a4) [## 使用 React 构建一个简单的 Todo 应用程序

### 让我们用 React 构建一个简单的 Todo 应用程序，教你 CRUD 的基本原理(创建、读取、更新和…

medium.com](https://medium.com/javascript-in-plain-english/build-a-simple-todo-app-using-react-a492adc9c8a4) [](https://medium.com/javascript-in-plain-english/build-a-rest-api-with-node-express-and-mongodb-937ff95f23a5) [## 用 Node，Express 和 MongoDB 构建一个 REST API

### 让我们使用 Node、Express 和 MongoDB 构建一个遵循 CRUD 原则的 REST API，并使用 Postman 测试它。

medium.com](https://medium.com/javascript-in-plain-english/build-a-rest-api-with-node-express-and-mongodb-937ff95f23a5)