# 从 Javascript: Flask 和 fetch API 与 Python 对话

> 原文：<https://towardsdatascience.com/talking-to-python-from-javascript-flask-and-the-fetch-api-e0ef3573c451?source=collection_archive---------2----------------------->

## 使用 Python 处理动态 web 界面或可视化所需的数据。

![](img/e2d1f1888f6abab25f8491f43bc6f7ef.png)

一个样本网络— D. Ellis 2020

在数据科学领域，通常需要使用一系列工具，每种工具都与其工作相关。一个需要使用 web 界面可视化的角色，但是处理 Python 脚本，通常最好在 [d3](https://d3js.org/) 或 [THREE.js](https://threejs.org/) 中构建一个定制的可视化来显示它，然后根据需要获取数据。本文介绍了如何创建一个简单的 flask 应用程序，它可以使用 Fetch API 向 web 接口提供数据。

# 创建烧瓶应用程序

我们首先构建一个包含一个空的`templates`文件夹和一个`app.py`文件的存储库。

## App.py

我们的`app.py`文件包含创建 web 界面所需的数据。为此，我们使用 flask ( `pip install flask` ) python 库。为此，我们可以使用以下模板:

```
########  imports  ##########
from flask import Flask, jsonify, request, render_template
app = Flask(__name__)#############################
# Additional code goes here #
######################################  run app  #########
app.run(debug=True)
```

在这里，我们首先导入所需的功能、应用程序结构和运行命令来启动应用程序。

## Index.html

定义了 flask 应用程序后，我们现在需要创建一个模板网页。这可以通过将文件`index.html`放在模板目录中来完成。

```
<body>
<h1> Python Fetch Example</h1>
<p id='embed'>**{{embed}}**</p><p id='mylog'/>
<body>
```

因为我们使用它作为模板，所以我们可以对某些关键字使用 react 风格的替换语法。在这个例子中，下面代码片段中的`{{embed}}`将被替换为`embed_example`字符串。

```
[@app](http://twitter.com/app).route(**'/'**)
def home_page():
    example_embed='This string is from python'
    return render_template('index.html', embed=example_embed)
```

这定义了当到达 flask app 的主页时运行的代码，需要在 `app.py` **内的** `imports` **和** `app.run()` **行之间添加**。****

## **运行应用程序**

最后，我们可以运行应用程序，使用`python app.py`并在 web 浏览器中导航到`[https://127.0.0.1:5000/](https://127.0.0.1:5000/)`进行查看。

# 获取和发布—它们是如何工作的？

在传输数据时，我们依赖于 fetch API 中的 GET 和 POST 函数。这些术语不言自明:

*   POST 是指将信息发送到一个位置，类似于寄信。
*   GET 指的是数据的检索——你知道你有邮件，所以你去邮局领取(索取)。

## 测试功能

在`app.py`中，我们可以为 GET 请求创建一个 URL。下面的代码定义了调用 URL 时的响应。

```
[@app](http://twitter.com/app).route('**/test**', methods=['GET', 'POST'])
def testfn(): # GET request
    if request.method == 'GET':
        message = {'greeting':'Hello from Flask!'}
        return jsonify(message)  # serialize and use JSON headers # POST request
    if request.method == 'POST':
        print(request.get_json())  # parse as JSON
        return 'Sucesss', 200
```

在一个 GET 请求之后，我们定义一个包含一个`greeting`元素的字典并序列化它。接下来，这将被发送回调用的 javascript 程序。

在将下面的代码添加到`app.run()`命令之前并执行它之后，我们可以访问`[https://127.0.0.1:5000/](https://127.0.0.1:5000/)test`——它应该会产生下面的结果:

```
{
  "greeting": "Hello from Flask!"
}
```

## 从 Javascript 调用数据

现在我们已经设置了服务器端的东西，我们可以使用 fetch 命令从其中检索数据。为此，我们可以使用如下的`fetch`承诺:

```
 fetch(**'/test'**)
      .then(function (response) {
          return response.json();
      }).then(function (text) {
          console.log('GET response:');
          console.log(text.greeting); 
      });
```

这里我们对`/test`运行一个 GET 请求，它将返回的 JSON 字符串转换成一个对象，然后将`greeting`元素打印到 web 控制台。通常，JavaScript 代码应该嵌套在 HTML 文档中的`<script>`标记之间。

# 从服务器请求数据

现在我们有了一个工作示例，我们可以扩展它以包含实际数据。实际上，这可能涉及访问数据库、解密一些信息或过滤一个表。

出于本教程的目的，我们将创建一个数据数组，并从中索引元素:

```
######## Example data, in sets of 3 ############
data = list(range(1,300,3))
print (data)
```

## 设置寻呼呼叫

在我们的 Flask 应用程序中，我们可以向 GET 请求添加可选参数——在本例中，是我们感兴趣的数组索引。这是通过页面 URL `/getdata/<index_no>`中的附加页面扩展指定的。然后，该参数被传递到页面函数中，并在 return 命令中进行处理(`data[int(index_no)]`)。

```
######## Data fetch ############
[@app](http://twitter.com/app).route('**/getdata/<index_no>**', methods=['GET','POST'])
def data_get(index_no):

    if request.method == 'POST': # POST request
        print(request.get_text())  # parse as text
        return 'OK', 200

    else: # GET request
        return 't_in = %s ; result: %s ;'%(index_no, data[int(index_no)])
```

## 写入提取请求

fetch 请求保持不变，只是将 GET URL 改为包含我们感兴趣的数据元素的索引。这是在`index.html`的 JS 脚本中完成的。

```
var index = 33;fetch(**`/getdata/${index}`**)
      .then(function (response) {
          return response.text();
      }).then(function (text) {
          console.log('GET response text:');
          console.log(text); 
      });
```

这一次，我们不是返回一个对象，而是返回一个字符串并解析它。然后可以根据需要在代码中使用它——无论是更新图形还是显示消息。

# 摘要

我们探索了一种使用 python 提取数据并将其提供给 javascript 代码进行可视化的方法(替代方法包括 web/TCP 套接字和文件流)。我们能够创建一个服务器端 python 代码来预处理或解密数据，并且只向客户端提供所需的信息。

如果我们有不断更新的数据、大型(高资源)数据集或我们不能直接提供给客户端的敏感数据，这是非常有用的。

*本文中使用的附带示例代码可以在:*找到

[](https://github.com/wolfiex/FlaskFetch) [## wolfiex/FlaskFetch

### 使用 Fetch API 在 flask 和一个提供服务的网页之间进行通信

github.com](https://github.com/wolfiex/FlaskFetch)