# 使用 Python Django、React 和 Docker 构建完全生产就绪的机器学习应用程序

> 原文：<https://towardsdatascience.com/build-a-fully-production-ready-machine-learning-app-with-python-django-react-and-docker-c4d938c251e5?source=collection_archive---------4----------------------->

## 使用 Django、PostgreSQL、React、Redux 和 Docker 构建生产级机器学习应用程序的完整分步指南

![](img/e66e0023169315527667706339167a64.png)

维克多·加西亚在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄的照片

# 先决条件

熟悉 Python、JavaScript、HTML、CSS 和一些基本的 Linux 命令。虽然我会尽我所能解释一切，但如果你在理解任何概念上有困难，请参考 Django、React、Docker 的官方文档，或任何其他必要的文档。

您需要在系统上安装 Python、Node、Postgres 和 Docker。Python 和 Node 分别带有它们默认的包管理器，即 pip 和 npm。我将在本文中使用这些包管理器，但是也可以随意使用其他适合您的包管理器。对于本文，我使用了 Python 3.7.4、Node 12.16.2、Postgres 12.3 和 Docker 19.03.12。Docker 是作为 Docker 桌面版安装的，它与 docker-compose 版本 1.26.2 预打包在一起。

我使用 VS Code 作为我的编辑器，因为这是一个免费的编辑器，具有许多功能和内置的终端，对于我们的用例来说非常方便。同样，请随意使用您选择的编辑器。

对于这个项目，正如您稍后将看到的，我们将创建 REST APIs。为了测试我们创建的 API，您需要安装 Postman。你可以从[这里](https://www.postman.com/downloads/)得到。

# 我们要建造什么？

# 后端— Django，Django REST 框架，PostgresSQL

我们将使用 Django REST 框架创建一个简单的机器学习应用程序，该应用程序根据样本花的特征(即萼片和花瓣的尺寸，即长度和宽度)来预测样本花的种类。我们已经在[之前的文章](https://datagraphi.com/blog/post/2019/12/19/rest-api-guide-productionizing-a-machine-learning-model-by-creating-a-rest-api-with-python-django-and-django-rest-framework)中详细介绍了这一点。请熟悉一下那篇文章。我们将在这里使用相同的 Django 应用程序，并根据需要进行一些修改。在上一篇文章中，Django 应用程序与一个 SQLite 数据库相连接。然而，对于本文，我们将使用 Postgres 作为我们的数据库，因为 Postgres 更适合生产构建。Django 附带了一个很棒的管理仪表板。通过管理仪表板，我们可以将用户注册到我们的应用程序中，然后用户可以与我们的机器学习应用程序进行交互以进行预测。因此，我们的 Django 应用程序将服务于我们的后端和管理任务。

# 前端—反应

一旦用户被创建，他们将能够登录到一个门户——前端，并通过 REST APIs 与我们在后端运行的机器学习模型进行交互。用户将能够执行两件事情:

*   进行预测——用户将能够提供样本鸢尾花数据，然后按下按钮进行预测。一旦用户点击按钮上的 enter，用户定义的虹膜数据将通过 POST API 请求发送到后端。后端将处理这些数据，并根据集成到后端的经过训练的机器学习模型进行预测。预测的鸢尾花种类将会立即发送回前端的用户。
*   在前端更改他们的密码，因为该密码最初将由管理员通过管理仪表板用默认密码设置。

# 我们的应用程序——Docker 的生产版本

*Docker 部分是可选的。如果你只想知道如何集成 Django 和 React 来创建一个简单但健壮的机器学习应用程序，你可以跳过本文中的这一部分。*

我们使用 docker 的原因不仅是因为这几乎是生产应用程序的标准，而且正如您将看到的，它允许将我们开发的所有工作无缝集成到一个独立于平台的 docker 工作流中，并允许使用简单的 docker 命令使部署更加容易，而不是使用多个命令运行单个应用程序。我们还将把 Nginx 添加到我们的 docker 工作流中，这将允许我们在 Docker 容器中运行 Django 和 React 应用程序。

# 创建我们的后端

# 创建一个本地 Python 环境并安装必要的包

确保您的系统中安装了 Python 3.7 +。如果可能的话，查看官方 Python [下载部分](https://www.python.org/downloads/)。

创建一个名为“ *DockerDjangoReactProject* 的空项目文件夹。打开终端，导航到这个文件夹，键入'*代码。*'，

```
D:\Projects\DockerDjangoReactProject> code .
```

然后回车。这将打开 VS 代码，并将这个文件夹设置为项目/工作文件夹。

我们将首先创建一个本地 python 环境，在这个环境中存储我们的后端应用程序所需的所有 python 包。创建 Python 环境有几种不同的方法。这里我们将使用' *virtualenv* '方法。首先，确保在 Python 的基础安装中安装了' *virtualenv* '包。如果没有，通过执行“ *pip install virtualenv* ”进行安装。

```
D:\Projects\DockerDjangoReactProject> pip install virtualenv
```

接下来，使用' *virtualenv* '创建本地 python 环境，如下所示。*同样，您可能需要根据需要在下面加上前缀‘python 3-m’。*

```
D:\Projects\DockerDjangoReactProject> virtualenv localPythonEnv
```

使用'*localPythonEnv \ Scripts \ activate*'激活您的 python 环境，如下所示。注意我用的是 windows，Linux 和 Mac 上的等效命令是'*source localPythonEnv/bin/activate '。一旦环境被激活，你就不需要在任何 python 命令前加上前缀‘python 3-m’。*

```
D:\Projects\DockerDjangoReactProject> localPythonEnv\Scripts\activate
(localPythonEnv) D:\Projects\DockerDjangoReactProject>
```

在我们的项目文件夹中创建一个 requirements.txt 文件，该文件包含我们项目所需的所有 Python 包的以下信息。您可以省略下面的版本号来获取最新的软件包:

```
Django==3.1
djangorestframework==3.11.1
django-rest-auth==0.9.5
django-cors-headers==3.5.0
psycopg2-binary==2.8.5
pandas==1.1.1
scikit-learn==0.23.2
joblib==0.16.0
gunicorn==20.0.4
```

在项目文件夹中，执行下面的命令，在我们的本地 Python 环境中安装所有这些包。

```
(localPythonEnv) D:\Projects\DockerDjangoReactProject> pip install -r requirements.txt
```

# 创建一个新的 Django 项目和 mainapp

在我们的 DockerDjangoReactProject 文件夹中创建一个名为 backend 的新文件夹。

```
(localPythonEnv) D:\Projects\DockerDjangoReactProject> mkdir backend
(localPythonEnv) D:\Projects\DockerDjangoReactProject>cd backend
```

通过运行下面的命令，在后端文件夹中创建一个新的 Django 项目。我们称我们的 Django 项目为 mainapp。

```
(localPythonEnv) D:\Projects\DockerDjangoReactProject\backend> django-admin startproject mainapp
```

默认情况下，当 Django 创建一个项目时，它会在项目文件夹中创建一个与项目同名的应用程序。

```
D:\PROJECTS\DOCKERDJANGOREACTPROJECT\BACKEND
└───mainapp
     └───mainapp
```

这就是为什么我们从一开始就把我们的应用程序叫做 mainapp，这样当我们进入包含其他应用程序的项目文件夹时，我们就知道哪个是主应用程序。现在将外部的 mainapp 文件夹重命名为“ *django_app* ”。

最后，后端文件夹中的文件夹结构应该如下所示:

```
D:\PROJECTS\DOCKERDJANGOREACTPROJECT\BACKEND
└───django_app
    │   manage.py
    │
    └───mainapp
            asgi.py
            settings.py
            urls.py
            wsgi.py
            __init__.py
```

# 创建一个新的 Django 预测应用程序(不像以前那样是默认的 mainapp)

从命令行导航到 django_app 文件夹

```
(localPythonEnv) D:\Projects\DockerDjangoReactProject\backend>cd django_app
```

并通过执行以下操作创建一个新的 Django 应用程序。

```
(localPythonEnv) D:\Projects\DockerDjangoReactProject\backend\django_app>django-admin startapp prediction
```

您会看到一个新的应用程序与' *mainapp* '并排创建。

```
D:\PROJECTS\DOCKERDJANGOREACTPROJECT\BACKEND
└───django_app
    │   manage.py
    │
    ├───mainapp
    │       asgi.py
    │       settings.py
    │       urls.py
    │       wsgi.py
    │       __init__.py
    │
    └───prediction
            admin.py
            apps.py
            models.py
            tests.py
            views.py
            __init__.py
```

顾名思义，' *mainapp* '是服务于 Django 应用程序的主应用程序。然而，所有其他功能将由我们创建的其他自定义应用程序来执行，例如我们的预测应用程序。我们稍后会添加另一个名为“*用户*的应用程序。

# 将我们新创建的应用程序添加到项目设置

打开 mainapp 文件夹中的 settings.py 文件。在底部的已安装应用部分添加我们的新应用“*预测*”。

```
INSTALLED_APPS = [
    'django.contrib.admin',
    'django.contrib.auth',
    'django.contrib.contenttypes',
    'django.contrib.sessions',
    'django.contrib.messages',
    'django.contrib.staticfiles',
    'prediction',
]
```

# 将本地 PostgresSQL 数据库连接到 django_app

确保您安装了 PostgreSQL 12+。如果可能，查看官方 PostgreSQL [下载部分](https://www.postgresql.org/download/)。我使用 windows 官方下载附带的 pgAdmin 创建了一个名为' *predictiondb* '的新的空数据库。请注意，第一次在浏览器中打开 pgAdmin 时，您需要指定一个默认用户和密码。您可以添加' *postgres_user* '作为用户，添加' *postgres_password* '作为密码。

![](img/4571a8bd23441b7b859484476a598d6b.png)

作者图片

![](img/35385ff499be6605524ab484a7c291ea.png)

作者图片

在 mainapp 文件夹中创建一个名为 local_settings.py 的文件。

```
D:\PROJECTS\DOCKERDJANGOREACTPROJECT\BACKEND\DJANGO_APP
│   manage.py
│
├───mainapp
│       asgi.py
│       settings.py
│       urls.py
│       wsgi.py
│       __init__.py
│       local_settings.py
│
└───prediction
```

这将用于覆盖 settings.py 文件的某些设置。打开 settings.py 文件，并在最后添加以下内容。

```
#########################################
    ##  IMPORT LOCAL SETTINGS ##
#########################################try:
    from .local_settings import *
except ImportError:
    pass
```

现在，我们将在 local_settings.py 文件中提供 PostgreSQL 连接设置。打开 local_settings.py 文件，并添加以下信息:

```
DATABASES = {
    'default': {
        'ENGINE': 'django.db.backends.postgresql_psycopg2',
        'NAME': 'predictiondb',
        'USER': 'postgres_user',
        'PASSWORD': 'postgres_password',
        'HOST': '127.0.0.1',
        'PORT': '5432',
    }
}
```

正如您在上面的设置中所看到的，' *NAME* '属性是您的数据库的名称，而' *USER* '和' *PASSWORD* '需要与您在系统上设置 pgAdmin 或 PostgreSQL 时使用的相同。

# 迁移 django_app 模式

此时，我们需要将现有的模式迁移到数据库中。这是因为通常在 Django 中，我们创建数据模型来与数据库交互。尽管此时我们只添加了一个应用程序 prediction，并且没有定义任何数据模型，但是如果您转到 settings.py 文件中的 INSTALLED_APPS 部分，您会看到已经存在许多默认应用程序。这些预装应用中的一些已经有了它们的模型(或模式定义)。为了使我们的 django_app 模式/模型与数据库保持一致，我们需要在继续之前先执行一次迁移。

确保您位于 django_app 文件夹中，并且本地 Python 环境已激活。执行 python manage.py 迁移。

```
(localPythonEnv) D:\Projects\DockerDjangoReactProject\backend\django_app>python manage.py migrate
```

如果您的本地数据库可以正确地连接到您的 django_app，那么您应该会在您的终端上看到如下消息。

```
Operations to perform:
  Apply all migrations: admin, auth, contenttypes, sessions
Running migrations:
  Applying contenttypes.0001_initial... OK
  Applying auth.0001_initial... OK
  Applying admin.0001_initial... OK
  Applying admin.0002_logentry_remove_auto_add... OK
  Applying admin.0003_logentry_add_action_flag_choices... OK
  Applying contenttypes.0002_remove_content_type_name... OK
  Applying auth.0002_alter_permission_name_max_length... OK
  Applying auth.0003_alter_user_email_max_length... OK
  Applying auth.0004_alter_user_username_opts... OK
  Applying auth.0005_alter_user_last_login_null... OK
  Applying auth.0006_require_contenttypes_0002... OK
  Applying auth.0007_alter_validators_add_error_messages... OK
  Applying auth.0008_alter_user_username_max_length... OK
  Applying auth.0009_alter_user_last_name_max_length... OK
  Applying auth.0010_alter_group_name_max_length... OK
  Applying auth.0011_update_proxy_permissions... OK
  Applying auth.0012_alter_user_first_name_max_length... OK
  Applying sessions.0001_initial... OK
```

# 创建超级用户

接下来，我们将为我们的项目创建一个超级用户/管理员。这是为了让超级用户可以登录到管理仪表板，监控项目和添加用户，我们将在后面看到。运行命令“python manage.py createsuperuser”。这将要求您提供用户详细信息和密码。每次提供它们并按回车键。

```
(localPythonEnv) D:\Projects\DockerDjangoReactProject\backend\django_app>python manage.py createsuperuser
```

创建超级用户后，终端将显示“超级用户创建成功”最后。注意，这个超级用户信息将存储在我们前面创建的 PostgreSQL 数据库中。这就是为什么我们需要提前执行迁移，以便用户的 django_app 数据模型与 PostgreSQL 服务器中的 users 表保持同步。

# 检查 Django 是否成功运行。

从 django_app 文件夹运行命令' *python manage.py runserver* '。

```
(localPythonEnv) D:\Projects\DockerDjangoReactProject\backend\django_app>python manage.py runserver
```

如果您正确地遵循了上述步骤，您将会在命令行中看到类似下面的消息。

```
System check identified no issues (0 silenced).
September 03, 2020 - 18:19:47
Django version 3.1, using settings 'mainapp.settings'
Starting development server at http://127.0.0.1:8000/
Quit the server with CTRL-BREAK.
```

打开你的网络浏览器，进入网址“ [http://127.0.0.1:8000/](http://127.0.0.1:8000/) ”。您应该会在浏览器中看到以下内容:

![](img/6389d31ed4ab067267de457106781021.png)

作者图片

如果你去[http://127 . 0 . 0 . 1:8000/admin/](http://127.0.0.1:8000/admin/)路由，你会看到一个如下图所示的管理面板。您可以使用之前创建的超级用户名称和密码登录到管理视图。那个超级用户是我们 Django 应用程序的管理员。

![](img/055813d5ac63c1a364cd02009bc4efea.png)

作者图片

登录后，您将能够看到以下内容。

![](img/4d6bc385fd94d6a59cc9d28e253fa9f1.png)

作者图片

点击 **+** 添加用户。你会发现作为 django_app 的管理员，你可以很容易地添加用户。

![](img/8463bec5a8ed33b12bb7d7b3713bf548.png)

作者图片

在终端上按 Ctrl+C，关闭 django_app 运行服务器。

# 创建我们的预测视图

在之前的[帖子](https://datagraphi.com/blog/post/2019/12/19/rest-api-guide-productionizing-a-machine-learning-model-by-creating-a-rest-api-with-python-django-and-django-rest-framework)中，我们已经创建了我们的预测机器学习模型。你可以参考这篇文章，看看机器学习模型是如何创建的。在预测应用文件夹中创建一个名为' *mlmodel* 的文件夹。将我们在上一篇文章中创建的文件'*irisrandomforestclassifier . joblib*'复制到这个' *mlmodel* '文件夹中。为了您的方便，该文件存储在这里的 [github](https://github.com/MausamGaurav/PredictionAPIWithDjangoRESTDemoApp/blob/master/APIProjectFolder/Prediction/classifier/IRISRandomForestClassifier.joblib) 上。

```
D:\PROJECTS\DOCKERDJANGOREACTPROJECT\BACKEND\DJANGO_APP\PREDICTION
│   admin.py
│   apps.py
│   models.py
│   tests.py
│   views.py
│   __init__.py
│
└───mlmodel
        IRISRandomForestClassifier.joblib
```

编辑预测应用程序文件夹中的 apps.py 文件，如下所示:

我们在 apps.py 文件中加载 ml 模型的原因是，通过这种方式，分类器只需加载一次，即在与 Django 应用程序建立会话连接时，从而减少了开销。我们正在使用官方 scikit-learn [文档](https://scikit-learn.org/stable/modules/model_persistence.html)建议的' *joblib* '加载方法将训练和保存的分类器加载回内存。

接下来，我们将在预测应用程序中创建一个 API 视图。由于我们要借助 Django Rest 框架来创建 REST APIs，我们之前通过 requirements.py 文件安装了这个框架，所以一定要在 settings.py 文件中添加' *rest_framework* '作为一个应用。

```
# Application definitionINSTALLED_APPS = [
    'django.contrib.admin',
    'django.contrib.auth',
    'django.contrib.contenttypes',
    'django.contrib.sessions',
    'django.contrib.messages',
    'django.contrib.staticfiles',
    'prediction',
    'rest_framework',
]
```

现在，编辑预测应用程序文件夹中的 views.py 文件，如下所示:

因此，我们创建了一个基于类的视图' *IRIS_Model_Predict* ，它继承了默认的 API view*django rest _ framework。*基本上，该视图从 URL 请求(我们将在下面定义)中获取 JSON 输入数据，处理数据，并根据 apps.py 文件中定义的加载的机器学习模型进行预测。

# 创建 URL

接下来，我们需要为我们的后端应用程序定义路由。其中一条路由，即管理路由，您刚刚在上面看到，它将您带到管理视图。打开 mainapp 文件夹中的 urls.py 文件。进行更改，使 urls.py 文件如下所示。

所以我们基本上是说，任何以/api/(比如[http://www.example.com/api/...)](http://www.example.com/api/...))开头的路由都应该将我们带到 prediction.urls 文件，该文件将包含路由的剩余部分。

要定义路径的其余部分，请在预测应用程序文件夹中创建一个 urls.py 文件。该文件应包含以下内容。

也就是说，我们将“预测”添加到 URL 路径/api/中，这样，如果我们发出 api URL 请求，如[http://www.our-final-app.com/api/predict/](http://www.our-final-app.com/api/predict/)，我们将被带到 IRIS_Model_Predict 视图，从该视图中，我们将得到一个预测响应，该响应是对随请求一起发送的样本 iris flower 数据的响应。

# 测试我们的预测 API

像以前一样，通过执行'*python manage . py runserver*'来重启我们的 django_app 服务器。

打开 Postman 进行新的 POST 请求。由于我们的 django_app 运行在本地服务器上，我们将向[http://127 . 0 . 0 . 1:8000/API/predict/](http://127.0.0.1:8000/api/predict/)或[http://localhost:8000/API/predict/](http://localhost:8000/api/predict/)发出 API 请求。我们将发送以下样本鸢尾花数据作为 JSON 来进行预测。

```
{
	"sepal length (cm)": 6.5,
	"sepal width (cm)":3,
	"petal length (cm)":2,
	"petal width (cm)":1
}
```

向邮递员提出如下要求。JSON 样本花数据应该以 Body->raw->JSON 格式发送。

![](img/bb66637be97b5353228ca35a44c404f3.png)

作者图片

你会看到加载的机器学习模型预测样本的花卉种类为“setosa”。

这意味着我们的预测 API 工作正常。

# 向我们的 REST APIs 添加身份验证

尽管我们的预测 API 工作正常，但在现实世界中，我们希望将对我们应用程序的 API 请求的访问权限仅限于注册用户。因此，我们希望增加功能，注册用户可以得到认证。在我们的例子中，工作方式如下:

*   管理员使用默认密码通过管理员视图添加新用户。
*   新用户然后能够登录到我们的应用程序，并选择他们选择的密码。
*   然后，用户使用他们的用户名和密码发出 REST API 登录请求。
*   然后，我们应用程序中的登录视图生成一个令牌，并将这个令牌发送回用户。
*   然后，用户使用这个令牌对预测 API 进行身份验证。只有通过身份验证的用户才能够使用之前创建的 IRIS_Model_Predict 视图进行预测，并且向未通过身份验证的用户发送错误响应。

要创建如上所述的登录视图，并在登录时返回令牌，我们将使用一个名为 django-rest-auth 的包，我们之前已经将它与 requirements.py 文件一起安装了。

在 settings.py 的已安装应用中，添加' *rest_framework.authtoken* 和' *rest_auth* 作为应用，如下所示:

```
# Application definitionINSTALLED_APPS = [
    'django.contrib.admin',
    'django.contrib.auth',
    'django.contrib.contenttypes',
    'django.contrib.sessions',
    'django.contrib.messages',
    'django.contrib.staticfiles',
    'prediction',
    'rest_framework',
    'rest_framework.authtoken',
    'rest_auth',
]
```

然后，我们需要使用“ *python manage.py migrate* ”将我们新添加的应用程序的应用程序模式迁移到数据库。

```
(localPythonEnv) D:\Projects\DockerDjangoReactProject\backend\django_app>python manage.py migrate
```

这将在我们的数据库中创建一个名为‘authtoken _ token’的新表。你可以从 pgAdmin 中看到这一点。

![](img/5e4258842b32342f5dd18c0902e0d72b.png)

作者图片

您还可以在 Django 管理视图中看到同一个表。该表将存储为所有用户创建的令牌，并在用户注销时删除它们。

![](img/e17172419304b319feabf7817a573c5e.png)

作者图片

现在，我们需要在 django_app 中创建一个名为' *users* '的应用程序，它可以通过 REST APIs 处理用户登录/注销，在登录时返回令牌，在注销时使令牌过期。幸运的是，所有这些都很容易处理，因为我们已经安装了 django-rest-auth。然而，我们需要首先创建我们的“用户”应用程序。停止正在运行的 django_app 服务器，并运行以下命令:

```
(localPythonEnv) D:\Projects\DockerDjangoReactProject\backend\django_app>django-admin startapp users
```

像之前一样，在 mainapp.settings.py 文件的 INSTALLED_APPS 部分添加“用户”作为已安装的应用程序。请注意，我们已经重新排列了下面的列表，因此预测和用户都在底部。

```
# Application definitionINSTALLED_APPS = [
    'django.contrib.admin',
    'django.contrib.auth',
    'django.contrib.contenttypes',
    'django.contrib.sessions',
    'django.contrib.messages',
    'django.contrib.staticfiles', 'rest_framework',
    'rest_framework.authtoken',
    'rest_auth', 'prediction',
    'users',
]
```

打开用户文件夹中的 views.py 文件。

```
└───users
        admin.py
        apps.py
        models.py
        tests.py
        views.py
        __init__.py
```

修改文件，如下所示。

因此，本质上我们只是使用来自 rest_auth 的 LoginView，对于注销，我们确保只有登录的用户能够通过使用他们在登录时收到的令牌进行身份验证来注销。这就是 APILogoutView 中的身份验证和权限类所做的事情。现在，在测试我们的 API 登录和注销视图之前，我们还需要做一件事。我们需要实际定义 API URL 请求来执行登录和注销(视图)。就像之前在' *mainapp* 中的 urls.py 文件中添加行' *path('api/auth/'，include(' users . URLs ')*'。注意，我们需要将这一行添加到前面的'*路径(' api/'，include(' prediction . URLs ')【T5 ')行之上。这是为了让 Django 不会混淆，并把所有以' *api* '开头的内容发送到预测应用程序。因此,' mainapp '中的 urls.py 文件应该如下所示。*

就像前面一样，在用户应用程序中创建一个 urls.py 文件，并将下面的代码放在 urls.py 文件中。

现在创建了 URL 和视图，我们应该能够通过 Postman 测试我们的 API 登录和注销视图。如果一切设置正确，我们应该能够在登录后从服务器取回令牌。重启 django_app 服务器，打开 Postman。现在向'*http://127 . 0 . 0 . 1:8000/API/auth/log in/*'发出 post 请求，使用您创建的超级用户登录详细信息作为请求数据，如下所示。注意:在下面的截图中，我将使用' *sample_user'* 作为演示用户。

![](img/c994a5496dab11540710969d78a59945.png)

作者图片

您应该能够得到一个令牌作为响应，如上所示。如果您转到管理门户，您将能够看到用户的令牌。

![](img/6b655f4de5e7e4d0bc59fd8ef87791e0.png)

作者图片

这意味着我们的登录 API 工作正常。现在我们需要检查注销 API。在 Postman 中打开一个新的标签页，向“*http://127 . 0 . 0 . 1:8000/API/auth/logout/*”添加一个帖子请求。在 headers 选项卡中，添加一个名为' *Authorization* 的新密钥，其值为 Token，后跟您之前收到的令牌。

![](img/55055c9b37389fcad48dac584ed985ae.png)

作者图片

点击发送，你应该会看到一条信息。

```
{
    "detail": "Successfully logged out."
}
```

如果您现在转到管理面板并刷新，您会看到令牌已从服务器中删除。

现在，我们需要将这个身份验证功能添加到 REST API 预测视图中，以便只有注册用户才能执行预测。这真的很简单。只需将下面几行添加到我们的 IRIS_Model_Predict 视图中。

```
authentication_classes = [TokenAuthentication]
permission_classes = [IsAuthenticated]
```

因此，我们的预测应用程序中的 views.py 如下所示。

如果您现在像以前一样对'*http://127 . 0 . 0 . 1:8000/API/predict/*'执行 post 请求，您将会得到一个错误。

```
{
    "detail": "Authentication credentials were not provided."
}
```

这是因为您现在需要首先登录，接收一个令牌，然后将该令牌与预测 API 请求一起发送以进行身份验证。因为您已经注销，所以再次使用 Postman 登录，并提供新收到的令牌作为授权头，以及用于发出请求的 JSON iris flower 数据。

![](img/3c027c65d72b5455245c645ace238a6b.png)

作者图片

因此，我们在预测 REST API 中添加了身份验证。现在，在我们进入前端之前，我们需要做一些事情。首先，我们需要使用 REST APIs 添加一个密码更新特性。其次，我们需要启用跨源资源共享( [CORS](https://developer.mozilla.org/en-US/docs/Web/HTTP/CORS) )，以便我们的前端可以在运行于同一个物理服务器上时向我们的后端发出请求。

# 创建一个密码更新 REST API

在 users.views.py 文件中从' *rest_auth.views* 导入' *PasswordChangeView* ，创建如下' *APIPasswordUpdateView* '类。我们这次只需要添加' *TokenAuthentication* '作为认证方法。users.views.py 文件如下所示。

现在我们需要为 API 定义 URL 路由。打开 users.urls.py 文件，并对其进行如下更改。

现在我们可以用 Postman 测试一下，如下所示。不要忘记像以前一样在授权头中提供我们的身份验证令牌。在正文中，我们需要用字段' *new_password1* '和' *new_password2* '提供两次新密码。

![](img/d291978443b75472993148569814e81a.png)

作者图片

注意，这次我们使用 Postman 中的表单数据字段来发送数据，而不是以前使用的 JSON 格式。如果一切正常，您会看到消息。

```
{
    "detail": "New password has been saved."
}
```

# 将 CORS 添加到 Django 应用程序中

如果我们不添加 CORS，那么我们将无法从本地机器上运行的 React 前端对 Django 应用程序进行 REST API 调用，当我们为生产环境设置 Docker 时也是如此。要添加 CORS，我们需要安装一个名为' *django-cors-headers* 的包，我们之前已经用 requirements.txt 文件安装了这个包。我们需要在 mainapp.settings.py 文件中添加'*corshe headers*'作为已安装的应用程序(在 rest_auth 下面)。

```
INSTALLED_APPS = [
    ... 'rest_framework',
    'rest_framework.authtoken',
    'rest_auth',
    'corsheaders', ...
]
```

在 installed apps 部分的下面，您需要修改中间件，如下所示，也就是说，我们必须在最上面修改 cors 中间件:

```
MIDDLEWARE = [
    ...
    'corsheaders.middleware.CorsMiddleware',
    'django.middleware.security.SecurityMiddleware',
    ...
]
```

打开 local_settings.py 文件，并在底部添加以下内容。

```
#################################################################
    ##  (CORS) Cross-Origin Resource Sharing Settings ##
#################################################################
CORS_ORIGIN_ALLOW_ALL = True
```

就这样，我们已经为 Django 应用程序设置了 CORS。您可以通过参考官方开发者[页面](https://github.com/adamchainz/django-cors-headers)来更改更多高级选项的设置。但是现在，我们应该准备好了。

现在继续我们的前端 React 应用程序🍺！！

# 创建我们的前端

# 使用创建-反应-应用程序创建应用程序

确保您系统上安装了节点。如果没有，从[官方网站](https://nodejs.org/en/download/)下载并安装。在 DockerDjangoReactProject 文件夹中创建一个名为 frontend 的新文件夹。从终端 cd 到前端文件夹。我们将创建一个名为 react_app 的新项目/应用程序。从终端输入下面的代码，'*' npx create-react-app react _ app*'。

```
D:\Projects\DockerDjangoReactProject\frontend>npx create-react-app react_app
```

以上需要几分钟来完成，因为 node 会下载一些包来创建我们称为 react_app 的应用程序。因此，我建议你休息一会儿，直到这个过程结束。如果你面临 npx 的问题，你可以使用 yarn 来创建我们的前端应用程序。要使用纱线，安装[纱线](https://classic.yarnpkg.com/en/docs/install)，并执行以下操作。

```
D:\Projects\DockerDjangoReactProject\frontend>yarn create react-app react_app
```

创建 react_app 后，将 cd 放入 react_app 文件夹并执行' *npm start* '。

```
D:\Projects\DockerDjangoReactProject\frontend>cd react_app
D:\Projects\DockerDjangoReactProject\frontend\react_app>npm start
```

这将在 [http://127.0.0.1:3000 启动开发服务器。您可以在此 URL 打开服务器索引页面，您会在浏览器中看到如下内容。](http://127.0.0.1:3000.)

![](img/573f00d3f64b06039cbd010216f3d55e.png)

作者图片

如果你想学习 React 的一些基本概念，那么你可以参考我之前写的这篇[文章](https://datagraphi.com/blog/post/2020/1/25/hands-on-guide-to-key-concepts-of-react-js)。但是在本文中，我们将只讨论需要什么。

# **创建模板登录视图**

我们可以采用与 react_app 中的 django_app 类似的方法，首先创建视图，然后定义将我们带到这些视图的 URL。首先，我们将创建我们的登录视图。在我们的 src 文件夹中创建一个名为' *components* '的新文件夹。在“*组件*”文件夹中创建一个名为“ *Login.js* ”的新文件。我将使用一个名为[材质 UI](https://material-ui.com/) 的包来创建我的组件。这是因为 Material UI 已经包含了一些设计非常好的组件，我们可以直接使用它们。安装材料用户界面如下。

```
D:\Projects\DockerDjangoReactProject\frontend\react_app>npm install @material-ui/core @material-ui/icons @material-ui/lab
```

*注意，无论我们安装什么包，都需要从 react_app 文件夹中安装。安装后，你可以在“package.json”文件中看到这些包。如果 npm 给你制造了一些问题，你也可以使用 yarn。*

```
D:\Projects\DockerDjangoReactProject\frontend\react_app>yarn add @material-ui/core @material-ui/icons @material-ui/lab
```

一旦材料用户界面安装完毕，转到[材料用户界面](https://material-ui.com/)的网站，在左侧栏中，您可以在“入门”部分找到模板子部分。在模板页面上，您可以找到登录模板源代码的链接。

![](img/c4ed87feb49babc82a2c481f4b7a6f28.png)

作者图片

复制代码并将其粘贴到我们刚刚创建的 Login.js 文件中。我们将进一步修改我们的模板页面。但是首先，我们需要创建将我们带到这个登录页面的路由/URL，以便我们可以在浏览器中查看它。我们需要安装一个名为“ *react-router-dom* 的包，通过执行以下任一操作来安装它。

```
D:\Projects\DockerDjangoReactProject\frontend\react_app>npm install react-router-domD:\Projects\DockerDjangoReactProject\frontend\react_app>yarn add react-router-dom
```

在 App.js 文件的同一级别的“src”文件夹中创建一个名为“Urls.js”的新文件。Urls.js 文件应该包含以下内容。

所以基本上这就是说，如果浏览器路由有确切的路径' *login* '，在这种情况下，Urls.js 应该从' *Login.js* '文件返回登录视图。

要查看实际效果，请打开 src 文件夹中的 App.js 文件，并进行以下修改。

此外，删除 src 文件夹中所有以前由 App.js 或 index.js 使用的不必要的文件——app . CSS、logo.svg、index.css。删除行*“import”。/index . CSS '；'*来自 index.js

现在，如果您从浏览器进入“*http://localhost:3000/log in*”，您会看到登录页面显示出来。

![](img/942823031da169b62507aabcff3046a4.png)

作者图片

这意味着我们的基本路由/URL 和视图设置在 React 前端工作。我们需要删除“*记住我*”、“*忘记密码？*‘和’*没有户口？从登录视图注册*组件。在 Login.js 文件中，删除以下内容。

```
.....
         <FormControlLabel
            control={<Checkbox value="remember" color="primary" />}
            label="Remember me"
          />
.....
          <Grid container>
            <Grid item xs>
              <Link href="#" variant="body2">
                Forgot password?
              </Link>
            </Grid>
            <Grid item>
              <Link href="#" variant="body2">
                {"Don't have an account? Sign Up"}
              </Link>
            </Grid>
          </Grid>
.....
```

我们还需要一个用户名字段，而不是电子邮件字段。此外，将默认导出函数的名称改为从以前的'*登录*'登录，并删除顶部的版权函数和登录函数内部对它的引用。Login.js 文件应该如下所示。

```
....
export default function Login() {
  ..... return (
    <Container component="main" maxWidth="xs">
     .....
        <form className={classes.form} noValidate>
          <TextField
            variant="outlined"
            margin="normal"
            required
            fullWidth
            id="username"
            label="User Name"
            name="username"
            autoComplete="username"
            autoFocus
          />
     ....
        </form>
     ......
    </Container>
  );
}
```

因此，浏览器登录页面将如下所示。

![](img/859820d5f0e053bf9e20482bb04daefb.png)

作者图片

# 创建带有布局的基本主视图

在继续之前，让我们也添加一个主页视图。这样，一旦用户登录，就会被重定向到主页。此时，如果你在[*http://127 . 0 . 0 . 1:3000/*](http://127.0.0.1:3000/)进入主页，你只会看到一个空白页。这是因为我们之前修改了 App.js，只为一个 URL 路由添加了一个视图——登录路由。转到物料 UI 网站- >组件- >应用程序栏，复制“*简单应用程序栏*的代码。在我们的组件文件夹中创建一个名为' *TopBar.js* 的文件，并将复制的代码粘贴到其中。现在在 components 文件夹中创建一个名为 Footer.js 的文件，内容如下。

现在在 components 文件夹中创建另一个名为*‘layout . js’*的文件。Layout.js 应该包含下面的代码。

我们将使用 Layout.js 页面来布局我们的主页。请注意，对于我们的简单应用程序来说，使用布局简直是一种过度杀戮。然而，我仍然想展示它，因为使用布局设计网站的不同页面被认为是一个好的设计原则。基本上，布局的工作方式是，我们将传递一个代表网页主体的组件作为布局函数的道具，布局将在页面顶部添加一个顶栏，在底部添加一个页脚。**就这么简单！！**布局函数中有趣的一点是‘props . children’。这将检索作为道具传递给布局函数的页面内容，并将其显示在顶栏和页脚之间。

现在创建一个简单的主页，它将只显示文本主页。组件中的 Home.js 文件应该如下所示。

现在我们需要为我们的主页创建 URL 路由，就像我们为登录页面所做的那样。修改 src 文件夹中的 Urls.js 文件，如下所示。

```
......import Login from "./components/Login";
import Home from "./components/Home";function Urls(props) { ....
                <Switch>
                    <Route exact path="/login/"> <Login {...props} /></Route>
                    <Route exact path="/"> <Home {...props}/></Route>
                </Switch>
          .....
};.....
```

如果你在浏览器中进入主页，你会看到文本

# 主页

显示，但你看不到顶栏和页脚。要使用上面讨论的布局，即同时看到顶栏和页脚，请修改 App.js 文件，如下所示。

在 App.js 中进行上述更改后，如果您转到主页，将会看到下面的内容。注意，在上面的例子中，Urls 组件被传递给 Layout 组件，Layout 组件用 props.children 呈现 Urls 组件返回的任何内容，如前所述。

![](img/31b89835607e4bb2dc2be155459b2f3e.png)

作者图片

如果您再次进入登录页面，您会看到登录页面也应用了顶栏和页脚。

![](img/56bb07e88f49d5e5da2071cd04cc3b3e.png)

作者图片

酷！这是我们想要的。如你所见，我们的布局工作正常。

将 TopBar.js 组件中的新闻文本更改为“Iris Species Predictor”。此外，删除左侧的菜单图标按钮，因为我们希望我们的应用程序尽可能简单。

![](img/01019f42251571ddf43144c04b12dfbb.png)

作者图片

# 创建一个简单的设置文件

我们将定义一个 settings.js 文件，就像我们在 django_app 的' *mainapp* 中一样。该文件只定义了一些基本的设置变量— API_SERVER 和 SESSION_DURATION。API_SERVER 定义了 django_app 后端运行的 URL。这个 URL 在开发和生产环境中会有所不同，因为在生产环境中，我们会在云服务器上托管我们的应用程序。因此，我们不是在实际代码中修改它，而是定义一个设置文件，在这里我们可以非常容易地修改值。另一个环境变量 SESSION_DUARTION 定义了用户从浏览器中自动注销的时间。应该在 src 文件夹中创建 setting.js 文件，其内容如下。

# 创建用于身份验证的 Redux 存储

至此，我们已经为用户登录添加了一个基本的路由和模板视图。要添加真正的登录功能，我们需要对我们的后端进行 API 调用以进行身份验证。此外，我们需要在浏览器中成功登录后存储 API 令牌，这样，一旦用户登录，他们就可以使用这个 API 密钥向我们的后端发出进一步的请求。我们将使用 Redux 来完成这项工作。对于所有这些，我们需要安装以下软件包。就像之前一样执行以下任一操作。

```
D:\Projects\DockerDjangoReactProject\frontend\react_app>npm install redux react-redux redux-thunk axiosD:\Projects\DockerDjangoReactProject\frontend\react_app>yarn add redux react-redux redux-thunk axios
```

React 中的 Redux 一开始可能有点棘手。我不会说得太详细，只是简单地描述一下基本原理。通常在 React 中，组件有状态。根据定义，基于类的组件有与之相关的状态。也可以用像 useState()这样的 React 钩子来定义功能组件的状态。因此，组件有各自的状态。如果我们想把一个组件的状态传递给另一个组件，那么我们可以用 props 来实现。如果我们有许多依赖于相同状态变量的组件，这些道具会变得非常复杂。因此，我们希望有一个全球性的国家。在 React 中有许多方法可以实现这一点，例如 React 上下文。然而，一般来说，通过设计上下文是为了简化传递道具。Redux 是另一种更广泛接受的为应用程序创建全局状态的方法。在 Redux 中，我们有商店的概念，它由三个基本概念进一步表示:

*   行动
*   还原剂
*   商店

你可能会通过[官方指南](https://redux.js.org/basics/reducers)来详细了解这些，但本质上，React 中 Redux 的哲学真的很简单:

> 每当调度一个动作时，Reducer 对存储中的状态对象进行更改。存储是将动作、缩减器和状态对象集合在一起的对象。

让我们从为我们的应用程序创建商店开始。创建一个名为*的文件夹，将*存储在我们的 src 文件夹中。在 store-folder 中创建一个名为“authActionTypes.js”的文件，包含以下内容。这定义了动作类型。

在 store-folder 中创建另一个名为“authActions.js”的文件，内容如下。如果你读了下面的评论，这些授权动作的作用是不言自明的。还请注意，在下面的一些函数中，我们使用' *axios* '对我们的后端进行 rest API 调用。此外，‘*local storage*在浏览器中存储信息，即检索到的令牌和到期时间。你可能会问，当我们已经打算使用 Redux 存储来存储全局变量时，为什么还要使用浏览器的 localStorage 呢？这是因为 Redux 存储只能在浏览器中的每个选项卡或每个会话中使用。一旦用户关闭其中一个标签上的应用程序，商店就不存在了。也就是说这个存储只会存在于记忆中。这就是为什么我们使用 localStorage，这样一旦用户登录，他们就不需要在每个选项卡上登录应用程序。还有一些其他的方法可以实现这一点，比如 redux-persist，但是为了简单起见，我们只使用 localStorage。另一个原因是，我们不仅希望全局状态，还希望 Redux store 的一些分派(动作)是全局可用的。

因此，我们定义了动作类型、动作功能和一些返回调度(动作)组合的功能。现在我们要定义减速器。reducer 实际上定义了我们的应用程序全局状态和改变这些状态的方法。官方文件对减速器的定义如下。

> *reducer 是一个纯函数，取前一个状态和一个动作，返回下一个状态。*
> 
> *(前一状态，动作)= >下一状态*

正如您将在下面看到的，reducer 是我们初始状态的一个相当简单的定义，以及每当一个动作被分派到存储时改变这些状态的方法。分派给商店的动作的例子可以是:

```
// Dispatch some actions
store.dispatch(actions.authLogin(username, password)
```

现在让我们定义我们的 reducer，它将根据调度的动作改变全局状态变量。在我们的 store-folder 中创建一个名为' *authReducer.js* '的文件，内容如下。

据此，我们创建了我们的 Redux 商店。我们的应用程序 src 应该如下所示。

```
D:\PROJECTS\DOCKERDJANGOREACTPROJECT\FRONTEND\REACT_APP\SRC
│   App.js
│   App.test.js
│   index.js
│   serviceWorker.js
│   setupTests.js
│   Urls.js
│   settings.js
│
├───components
│       Login.js
│       TopBar.js
│       Layout.js
│       Footer.js
│       Home.js
│
└───store
        authActionTypes.js
        authActions.js
        authReducer.js
```

# 将 Redux 存储连接到我们的应用程序

完成所有重要的设置后，我们就可以实现登录功能了！我们现在需要告诉我们的应用程序使用上面的 redux 存储。我们的应用程序的入口点是 index.js 页面。打开 index.js 文件，将其修改为如下所示。

如您所见，我们导入之前创建的 authReducer，然后创建 redux 存储。然后，这个 redux store 可供我们的应用程序的所有组件使用(包括主应用程序组件！)通过在入口点 index.js 文件中用提供者包装器包装 App 组件。

从这里开始，我们的单个组件可以使用 *connect* 高阶包装器和两个自定义函数来访问这个全局存储，这两个自定义函数是 *mapStateToProps* 和*mapdispatchtopros*。首先显示了 App 组件的示例。如下所示修改 App.js 文件。下面，在我们的 App 组件中，当 App 第一次加载时，我们要调度 authCheckState()函数(即— *检查浏览器中是否已经存在令牌，令牌是否还没有过期。如果是这样，我们希望将“authSuccess”动作分派给存储，这将基本上更新 Redux 状态以反映用户已经过身份验证。如果不是，Redux 状态将被改变以反映用户被注销)*。此外，从 Redux 存储中派生出三个定制对象——*is authenticated*、 *token* 和 *logout* 函数——然后将它们作为道具传递给所有其他组件。

# 使用 Redux store 添加登录功能

接下来，我们将登录组件与 Redux store 连接起来，以使用一些不是从 App 组件作为道具传递下来的 Redux store 对象。如下所示修改 Login.js 文件。

在上述中，进行了以下更改。

```
.....import { connect } from 'react-redux';
import * as actions from '../store/authActions';......function Login(props) {
  ....
  const [username, setuserName] = React.useState(null);
  const [password, setPassword] = React.useState(null); const handleFormFieldChange = (event) => {
    switch (event.target.id) {
      case 'username': setuserName(event.target.value); break;
      case 'password': setPassword(event.target.value); break;
      default: return null;
    } }; const handleSubmit = (e) => {
    e.preventDefault();
    props.onAuth(username, password);
  } return (
    <Container component="main" maxWidth="xs">
      .....
        <form className={classes.form} noValidate onSubmit={handleSubmit}>
          <TextField
           ........
            onChange={handleFormFieldChange}
          />
          <TextField
           .........
            onChange={handleFormFieldChange}
          />
      ......
    </Container>
  );
}const mapDispatchToProps = dispatch => {
  return {
      onAuth: (username, password) => dispatch(actions.authLogin(username, password))
  }
}export default connect(null, mapDispatchToProps)(Login);
```

注意，在上面我们不需要 mapStateToProps，因为我们不需要任何其他 Redux store 状态变量。因此，代替' *mapStateToProps* '函数，我们只是将 null 传递给底部的连接包装器。然而，我们确实需要 dispatch authLogin 函数，所以我们使用' *mapDispatchToProps* '函数。此外，在登录功能组件中，我们用“ *useState* ”钩子定义状态。组件状态用户名和密码用 onChange = { handleFormFieldChange }子句更新，并且在用户提交表单时调用' *handleSubmit* '箭头函数。

为了验证我们的登录功能是否与 Redux store 一起工作，您需要为 Chrome 安装“ *Redux DevTools* ”扩展。打开前端登录页面，确保 Redux DevTools 扩展正在工作。另外，确保 Django 服务器和 PostgreSQL 服务都在您的系统上运行。现在，您应该尝试使用超级用户或您用 Django 后端创建的任何其他用户登录。如果一切设置正确，您将在 Redux DevTools 扩展中看到类似下面的 Redux 身份验证分派操作。

![](img/dcb7e352944cd129f282baec4bd246aa.png)

作者图片

如果您单击 state 选项卡，您还会看到 auth Redux 存储中的全局状态变量，如下所示。

![](img/db664f87adc6e57f90374fa9f0d4fb2b.png)

作者图片

您还可以在浏览器中检查本地存储，您会看到我们的身份验证令牌和过期时间(在我们的示例中，根据 settings.js 文件，从登录起+5 小时)也已设置好。

![](img/ddad642a68d73b3a4117294ccf7562cd.png)

作者图片

太棒了。！看起来我们用于认证的 Redux 存储运行良好。

# 登录时重定向到主页

要在通过身份验证后重定向到主页(或者实际上是大多数 web 应用程序中常见的上一页),请修改 Login.js 组件，如下所示。使用 useEffect，我们可以确保一旦用户通过身份验证，他们就会被重定向到上一页。

```
...import { useHistory, useLocation } from "react-router-dom";....function Login(props) {
  .... let history = useHistory();
  let location = useLocation();
  let { from } = location.state || { from: { pathname: "/" } }; React.useEffect(() => {
    if (props.isAuthenticated) { history.replace(from) };
  }); ....
}....
```

在制作了上面的页面之后，如果您已经像在前面的部分中一样进行了身份验证，并且登录页面保持打开，您将会看到您已经被重定向到您的主页。

除了以上所述，还有另一个我们想要的功能。我们还希望，如果一个用户没有通过身份验证，并试图打开我们的应用程序的主页或任何其他页面，用户被重定向到登录页面。为此，我们的 src 文件夹中的 Urls.js 文件应该更改如下。

# 添加注销功能

在 TopBar.js 文件中进行以下更改。基本上，如果用户通过了身份验证，我们希望显示一个注销按钮，否则不显示任何内容(因为如果用户没有通过身份验证，那么根据我们在上一节中所做的第二个功能更改，他们已经在登录页面上了)。

```
....export default function TopBar(props) {
  const classes = useStyles(); return (
    <div className={classes.root}>
      <AppBar position="static">
        <Toolbar>
          <Typography variant="h6" className={classes.title}>
            Iris Species Predictor
          </Typography>
          {props.isAuthenticated ? <Button color="inherit" onClick={()=>props.logout()}>Logout</Button> : null}
        </Toolbar>
      </AppBar>
    </div>
  );
}
```

厉害！！现在，您可以点击 logout 按钮，并在 Redux DevTools 扩展中查看注销操作。

![](img/0610992790499b270cc213f0adc00384.png)

作者图片

# 创建预测用户界面

好了，这么多只是为了让认证工作。然而，好的一面是，我们以结构化的方式介绍了最棘手的 React 概念之一 Redux，希望这将在许多其他项目中帮助您。

事不宜迟，让我们创建用户界面来与我们的机器学习模型进行交互。这个用户界面将出现在主页上。因此，一旦用户登录后被重定向到主页，他们就会看到这个界面。修改 Home.js 文件，如下所示。

代码基本上是不言自明的。我们已经创建了四个使用自定义滑块的滑块，自定义滑块是使用材质 UI 主题创建的。这可能看起来很复杂，但实际上并不复杂。因为所有这些都是从[material-ui.com](https://material-ui.com/components/slider/)的官方幻灯片示例中提取的。你可以参考这些例子。我们已经用 React 钩子创建了两个功能组件状态变量，useState。“dimension”状态变量是一个对象，而“prediction”只是一个字符串。当用户更改滑块时，使用 spread 运算符更新尺寸状态变量。当用户点击“预测”按钮时，会对我们之前创建的预测 REST API 进行一个 post API 调用。如果 API 调用成功，则向用户显示预测的虹膜种类。如果出现错误，将显示为警报。浏览器中的预测用户界面如下所示。

![](img/046d50e118ac04e1fbc6e2422809dff7.png)

作者图片

使用滑块输入花的尺寸后，您可以按下“预测”按钮并查看预测的效果！

# 添加密码更新功能

在我们进入 Docker 部分之前，我们需要做最后一件事——添加密码更新功能。首先，我们需要创建 PasswordUpdate 视图。在 components 文件夹中创建一个名为“PasswordUpdate.js”的新文件。我只是复制了 Login.js 文件，将其重命名为 PasswordUpdate.js，并做了一些更改来创建这个视图。该文件的内容如下所示。

因此在这个视图中，有两个密码文本字段。如前所述，状态变量已经使用 useState 为这些字段添加。一旦用户对这些字段中的任何一个进行了更改，如果这些字段不匹配，则这些字段的错误属性将变为活动状态。此外，字段下方的帮助文本将以红色显示密码不匹配。当用户在 handleSubmit 函数中点击 submit 时，如果密码仍然不匹配，就会向用户显示一个警告。如果密码相同，那么 handleSubmit 函数将对 REST API 密码更新 URL 进行 post API 调用。如果服务器发送一个 OK 响应，那么同样使用 useState 创建的 success 状态变量将变为 true。如果成功状态为真，我们的 PasswordUpdate 组件将显示一条消息，“密码更新成功！”在顶端。

要查看所有这些操作，让我们创建一个 URL 路由，它会将我们带到视图。在 Urls.js 文件中进行以下更改。

```
....
import PasswordUpdate from "./components/PasswordUpdate";
.....// A wrapper for <Route> that redirects to the login screen if you're not yet authenticated.
....function Urls(props) {
        ....
            <BrowserRouter>
                <Switch>
                    ....
                    <PrivateRoute exact path="/update_password/" isAuthenticated={props.isAuthenticated}><PasswordUpdate {...props}/></PrivateRoute>
                </Switch>
            </BrowserRouter>
      ....
};export default Urls;
```

最终的 Urls.js 文件如下所示。

此外，更改 TopBar.js 文件，以便我们可以在右上角看到两个按钮，其中一个将带我们到 PasswordUpdate 视图，另一个将带我们到主页。我们还添加了主页按钮，因为在密码更新后，用户可以单击该按钮返回主页。TopBar.js 如下所示。

现在是时候来看看我们的密码更新了。单击顶栏中的“更新密码”按钮，该按钮将在上述更改后出现。

![](img/0f4007835203d63284dbbfeb160e71d4.png)

作者图片

在密码更新页面上，如果您的密码不匹配，您会看到类似下面的错误。

![](img/e44289bb2e29077f21d6f550fe3c460a.png)

作者图片

如果您输入的两个密码相同，并且在按下“更新密码”按钮时，服务器返回状态代码 200 (OK ),您将会看到类似下面的内容。

![](img/21043cd2b3c3c58b77f8489c2eea4fbc.png)

作者图片

现在，您可以单击顶部栏上的“主页”按钮返回到您的主页，并根据您的需要进行更多的预测。

![](img/aa0607e6bb85ebd2463c230bafaf9fef.png)

作者图片

所以你走吧。我们已经使用 React 为我们的最终用户创建了一个简单而优雅的机器学习预测界面🍺！现在开始包装 Docker 中的机器学习应用程序🍺🍺！！

# Docker —为我们的应用程序创建一个 Docker 平台

文章的 Docker 部分在我的 wesbite 上，这里—[https://data graphi . com/blog/post/2020/8/30/Docker-guide-build-a-fully-production-ready-machine-learning-app-with-react-django-and-PostgreSQL-on-Docker](https://datagraphi.com/blog/post/2020/8/30/docker-guide-build-a-fully-production-ready-machine-learning-app-with-react-django-and-postgresql-on-docker)，我最初就是在这里创作了这篇文章。希望你也喜欢🍺！