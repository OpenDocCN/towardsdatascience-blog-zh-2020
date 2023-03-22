# 使用 Flask 和 Python 创建 RESTful Web APIs

> 原文：<https://towardsdatascience.com/creating-restful-apis-using-flask-and-python-655bad51b24?source=collection_archive---------0----------------------->

## 编程；编排

## 用 Flask 构建 Web APIs 的综合指南

![](img/1c5d35fa1148efa189149be0eb045685.png)

图片来自 [Pixabay](https://pixabay.com/?utm_source=link-attribution&amp;utm_medium=referral&amp;utm_campaign=image&amp;utm_content=2682712) 的 [Gerd Altmann](https://pixabay.com/users/geralt-9301/?utm_source=link-attribution&amp;utm_medium=referral&amp;utm_campaign=image&amp;utm_content=2682712)

lask 是一个广泛使用的微型 web 框架，用于在 Python 中创建 API。这是一个简单而强大的 web 框架，旨在快速轻松地开始使用，并能够扩展到复杂的应用程序。

从[文档](https://flask.palletsprojects.com/en/1.1.x/foreword/)中，

> “微型”并不意味着您的整个 web 应用程序必须适合一个 Python 文件(尽管它确实可以)，也不意味着 Flask 缺乏功能。微框架中的“微”意味着 Flask 旨在保持核心简单但可扩展。

![](img/bcd0caeda4c500b9a5748e6aa4ae643a.png)

来源:Flask 的[文件](https://flask.palletsprojects.com/en/1.1.x/_images/flask-logo.png)

# 装置

使用 pip 安装烧瓶

```
pip install Flask
```

# 最小烧瓶应用程序

```
from flask import Flask

app = Flask(__name__)

@app.route('/hello/', methods=['GET', 'POST'])
def welcome():
    return "Hello World!"

if __name__ == '__main__':
    app.run(host='0.0.0.0', port=105)
```

将该文件保存为`app.py` *(或任何其他你想要的文件名)*，并进入终端并键入`python app.py`(即`python <filename>.py`)

您应该会看到类似这样的内容:

```
* Running on http://0.0.0.0:105/ (Press CTRL+C to quit)
```

启动任何网络浏览器，然后前往`http://localhost:105/hello/`查看应用程序的运行情况。

现在，让我们逐行理解代码的工作原理:

`from flask import Flask` →导入烧瓶类

`app = Flask(__name__)` →创建类的一个实例

`@app.route('/hello/', methods=['GET', 'POST'])` →我们使用`route()`装饰器来告诉 Flask 哪个 URL 应该触发这个函数。
`methods`指定允许哪些 HTTP 方法。默认为`['GET']`

`if __name__ == '__main__'` → `__name__`是 Python 中的一个特殊变量，取脚本名的值。这一行确保我们的 Flask 应用程序仅在主文件中执行时运行，而不是在导入到其他文件中时运行

`app.run(host='0.0.0.0', port=105)` →运行烧瓶应用程序

指定我们希望 flask 应用程序运行的服务器。`host`的默认值为`localhost`或`127.0.0.1`

`0.0.0.0`的意思是*“本地机器上的所有 IPv4 地址”。这确保了从所有地址都可以到达服务器。
默认`port`值为`5000`，您可以设置参数`port`以使用您选择的端口号。*

# 可变规则

您可以使用`<variable_name>`向 URL 添加可变部分。该函数接收变量作为关键字参数。

```
from flask import Flask
app = Flask(__name__)

@app.route('/<int:number>/')
def incrementer(number):
    return "Incremented number is " + str(number+1)

@app.route('/<string:name>/')
def hello(name):
    return "Hello " + name

app.run()
```

运行上面的代码来启动 Flask 应用程序。
打开浏览器并转到`http://localhost:5000/Jimit`，您将看到输出为`Hello Jimit`，当您转到`http://localhost:5000/10`时，输出将为`Incremented number is 11`。

# 返回 JSON 可序列化输出

Flask 应用程序中函数的返回值应该是 JSON 可序列化的。您可以使用`jsonify`使您的输出 JSON 可序列化。该函数包装`json.dumps()`以将 JSON 输出转换成带有*application/JSON*mime-type 的响应对象。

## 示例 1:

这个例子展示了如何对字典对象使用`jsonify`:

```
from flask import jsonify

@app.route('/person/')
def hello():
    return jsonify({'name':'Jimit',
                    'address':'India'})
```

这将发送如下的 JSON 响应:

```
{
  "address": "India", 
  "name": "Jimit"
}
```

## 示例 2:

还可以使用`jsonify`自动将列表和元组序列化为 JSON 响应。

```
from flask import jsonify

@app.route('/numbers/')
def print_list():
    return jsonify(list(range(5)))
```

这将产生输出:

```
[
  0, 
  1, 
  2, 
  3, 
  4
]
```

# 重定向行为

```
@app.route('/home/')
def home():
    return "Home page"

@app.route('/contact')
def contact():
    return "Contact page"
```

在上面的例子中，`home`端点的 URL 有一个尾随斜杠，而`contact`端点的 URL 缺少尾随斜杠。

这会导致两种不同的行为:

1.  对于`home`端点，如果您访问不带结尾斜杠的 URL，那么 Flask 会将您重定向到带结尾斜杠的 URL。
2.  对于`contact`端点，如果您访问带有结尾斜杠的 URL，那么它将导致状态 404 Not Found。

# 返回状态代码

通过按如下方式指定状态代码，可以将状态代码与响应一起返回:

```
@app.route('/teapot/')
def teapot():
    return "Would you like some tea?", 418
```

对这个 URL 的响应将是带有状态码`418`的`Would you like some tea?`，而不是通常的`200`。

# 请求前

通过使用`app.before_request` decorator，您可以指定一个应该在请求被处理之前一直执行的函数。

```
@app.before_request
def before():
    print("This is executed BEFORE each request.")

@app.route('/hello/')
def hello():
    return "Hello World!"
```

对于这个例子，首先在服务器上打印语句`This is executed BEFORE each request.`，然后执行`hello`端点的功能。当您希望记录请求以进行监控时，这尤其有用。

# 访问请求数据

要访问请求数据，请使用以下命令

```
from flask import request
```

您可以使用以下属性来提取随请求一起发送的数据:

`[request.data](https://flask.palletsprojects.com/en/1.1.x/api/#flask.Request.data)` →以字符串形式访问输入的请求数据

`[request.args](https://flask.palletsprojects.com/en/1.1.x/api/#flask.Request.args)` →访问解析后的 URL 参数。返回`ImmutableMultiDict`

`[request.form](https://flask.palletsprojects.com/en/1.1.x/api/#flask.Request.form)` →访问表单参数。返回`ImmutableMultiDict`

`[request.values](https://flask.palletsprojects.com/en/1.1.x/api/#flask.Request.values)` →返回组合了`args`和`form`的`CombinedMultiDict`

`[request.json](https://flask.palletsprojects.com/en/1.1.x/api/#flask.Request.json)` →如果 *mimetype* 为`application/json`则返回解析后的 JSON 数据

`[request.files](https://flask.palletsprojects.com/en/1.1.x/api/#flask.Request.files)` →返回包含所有上传文件的`MultiDict`对象。每个键是文件名，值是`FileStorage`对象。

`[request.authorization](https://flask.palletsprojects.com/en/1.1.x/api/#flask.Request.authorization)` →返回一个`[Authorization](https://werkzeug.palletsprojects.com/en/1.0.x/datastructures/#werkzeug.datastructures.Authorization)`类的对象。它代表客户端发送的一个*授权*头。

# app.run()参数

`app.run()`在服务器上运行应用程序。除了`host`和`port`
之外，还有各种参数可以与`[app.run()](https://flask.palletsprojects.com/en/1.1.x/api/?highlight=threaded#flask.Flask.run)`一起使用，其中包括:

`debug` →如果`debug`参数设置为`True`，那么服务器将在代码更改时自动重新加载，并在出现未处理的异常时显示交互式调试器。默认为`False`

`use_reloader` →当`use_reloader`设置为`True`时，当代码改变时，服务器将自动重启。默认为`False`

`threaded` →当`threaded`设置为`True`时，进程将在单独的线程中处理每个请求。默认为`False`

`ssl_context` →连接的 SSL 上下文。如果服务器应该自动创建上下文，则需要`[ssl.SSLContext](https://docs.python.org/3/library/ssl.html#ssl.SSLContext)`、格式为`(cert_file, pkey_file)`的元组或字符串`'adhoc'`。默认值为`None`，即 SSL 被禁用。当我们想在 HTTPS 而不是 HTTP 上托管 Flask 应用程序时，使用这个。

# 蓝图

蓝图允许我们将不同的端点分成子域。

`home.py`

```
from flask import Blueprint
home_bp = Blueprint('home', __name__)

@home_bp.route('/hello/')
def hello():
    return "Hello from Home Page"
```

`contact.py`

```
from flask import Blueprint
contact_bp = Blueprint('contact', __name__)

@contact_bp.route('/hello/')
def hello():
    return "Hello from Contact Page"
```

`app.py`

```
from flask import Flask

from home import home_bp
from contact import contact_bp

app = Flask(__name__)

app.register_blueprint(home_bp, url_prefix='/home')
app.register_blueprint(contact_bp, url_prefix='/contact')

app.run()
```

注意，在两个蓝图中，`/hello/`路线都在调用`hello`函数。

当你去`http://localhost:5000/home/hello`时，输出将是`Hello from Home Page`
而当你访问`http://localhost:5000/contact/hello`时，输出将是`Hello from Contact Page`

# 记录

您可以使用以下方法在 Flask 应用程序中记录语句

```
app.logger.debug('This is a DEBUG message')
app.logger.info('This is an INFO message')
app.logger.warning('This is a WARNING message')
app.logger.error('This is an ERROR message')
```

## 参考

*   [https://flask.palletsprojects.com/en/1.1.x/foreword/](https://flask.palletsprojects.com/en/1.1.x/foreword/)

## 资源

这篇文章的所有代码片段都可以在我的 [GitHub 页面](https://jimit105.github.io/medium-articles/Creating%20RESTful%20APIs%20using%20Flask%20and%20Python.html)上找到。

## 推荐阅读

[用 Flask、Flask-RESTPlus / Flask-RESTX 和 Swagger UI 构建 Python API](https://medium.com/p/7461b3a9a2c8)

## 让我们连接

领英:[https://www.linkedin.com/in/jimit105/](https://www.linkedin.com/in/jimit105/)
GitHub:[https://github.com/jimit105](https://github.com/jimit105)
推特:[https://twitter.com/jimit105](https://twitter.com/jimit105)