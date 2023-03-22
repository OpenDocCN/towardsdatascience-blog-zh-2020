# 向您的 Dash 应用程序添加广告身份验证

> 原文：<https://towardsdatascience.com/a-simple-way-to-integrate-dash-with-ldap-4d5d059ee3a6?source=collection_archive---------14----------------------->

## 使用 Flask 集成 Dash 和 LDAP 的一个非常简单的方法

![](img/2d91e0c400751875ea7aa7d6d78fa764.png)

在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上由 [Dane Deaner](https://unsplash.com/@danedeaner?utm_source=medium&utm_medium=referral) 拍摄的照片

在大多数组织中，登录凭证由目录服务(如 Active Directory)管理。如果您设法将 [Dash](https://pypi.org/project/dash/) 与您组织的目录服务相集成，那么用户只需使用他们现有的凭证登录您的 Dash 应用程序。一种常见的方式是通过 [LDAP](https://en.wikipedia.org/wiki/Lightweight_Directory_Access_Protocol) (一种用于与 Active Directory 等目录服务对话的开放协议)和 [Flask](https://pypi.org/project/Flask/) (一种轻量级 web 应用程序框架)。在这篇文章中，我将演示如何用 Python 中的 Dash 以最简单的方式集成 LDAP 和 FLASK。

# 先决条件

在进入任何代码之前，您需要已经启动并运行了 LDAP。这是一个重要的步骤，超出了本文的范围，本文只关注 Python 方面的内容。然而，如果你需要广告整合，我假设你在一个有自己 IT 部门的大公司工作，希望这个部门(1)不是你,( 2)愿意帮助你！

假设 LDAP 已经启动并运行，继续运行以下命令，从 [GitHub](https://github.com/epetrovski/dash_ldap) 克隆示例代码:

```
git clone https://github.com/epetrovski/dash_ldap
```

我们还需要几个库来构建一个 Dash 应用程序，并让它与 LDAP 一起工作——总共四个。如果您已经使用了 [Pipenv](https://pypi.org/project/pipenv/) ，只需在克隆的 dash_ldap 目录内的终端中运行以下命令，即可获得一个安装了依赖项的完整的工作虚拟环境:

```
pipenv sync
```

如果您不想弄乱 Pipenv，只需安装您需要的依赖项:

```
pip install dash flask flask-simpleldap gunicorn
```

# app.py 演练

你需要的所有代码都在 [app.py](https://github.com/epetrovski/dash_ldap/blob/main/app.py) 里。让我们一行一行地过一遍。

第一个技巧是让 Dash 在一个 Flask 应用程序中运行，这个应用程序向外界提供 Dash 应用程序。这是通过在初始化时将 Flask 对象传递给 Dash 应用程序的服务器参数来实现的，如下所示:

```
app = Dash(__name__, server=Flask(__name__))
```

然后，为了举例，我们定义一个 Dash 应用程序。这个应用程序只是一个简单的占位符，但是不要担心，当你用高级回调等构建自己的应用程序时。，它不需要对 LDAP 集成设置进行任何更改。

```
app.layout = html.P('Hello world!')
```

您现在需要通过更新 Flask 的配置字典来为 LDAP 配置 Flask。下面的详细信息只是一个占位符，因为您的凭据将取决于您公司的 LDAP 设置—再次询问！

```
app.server.config.update({
   'LDAP_BASE_DN': 'OU=users,dc=example,dc=org',
   'LDAP_USERNAME': 'CN=user,OU=Users,DC=example,DC=org',
   'LDAP_PASSWORD': os.getenv('LDAP_PASSWORD')})
```

你可能已经注意到，我冒昧地在`LDAP_PASSWORD`后面加了一个`os.getenv()`。我假设你永远不会在代码中存储密码，而是使用类似于[环境变量](https://help.ubuntu.com/community/EnvironmentVariables)的东西。

非常重要的是，现在我们需要通过用`LDAP(app.server).basic_auth_required()`包装所有已经用 Flask 注册的视图函数来保护它们:

```
for view_func in app.server.view_functions:
   app.server.view_functions[view_func] = LDAP(app.server).basic_auth_required(app.server.view_functions[view_func])
```

最后，我们可能需要一种方法来快速运行 Flask 应用程序以进行测试。通常我们会使用`app.run()`，但是因为这个应用是在 Flask 服务器上提供的，所以我们使用`app.run_server()`:

```
if __name__ == '__main__':
   app.run_server()
```

# 测试一下

您可以通过运行以下命令来确认 AD 集成正在运行:

```
python3 app.py
```

现在导航到 [http://127.0.0.1:8000](http://127.0.0.1:8000) ，您应该会看到一个登录提示。输入你的身份证明，你应该会看到“你好，世界！”首页。

既然您对一切顺利运行感到满意，请记住，使这一切工作的关键部分是 [flask-simpleldap](https://flask-simpleldap.readthedocs.io/en/latest/) 包。这是一个很棒的 Python 包，它可以解决一个非常乏味但却至关重要的 It 需求。

# 投入生产

如果您在生产中运行 Dash 应用程序，您应该在生产级 http 服务器上运行该应用程序，如 [gunicorn](https://gunicorn.org/) 。您已经安装了这个，所以只需运行:

```
gunicorn app:server
```

这是因为在`app.py`中，我定义了:

```
server = app.server
```

这就是你要的，一个登录受保护的 Dash 应用程序，集成了一个目录服务，适合生产！