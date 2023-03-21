# 2020 年数据网站和投资组合的状态(以及如何制作)

> 原文：<https://towardsdatascience.com/the-state-of-data-websites-and-portfolios-in-2020-and-how-to-make-them-5e3318c183fc?source=collection_archive---------39----------------------->

![](img/0e4a7997ea072b7a50e40b04a066c273.png)

*卢克·皮特斯拍摄于* [*Unsplash*](https://unsplash.com/photos/B6JINerWMz0)

拥有一个可视化产品、网站或仪表板来展示你在编码/机器学习项目上的艰苦努力是一件真正壮观的事情！然而，这通常非常困难，因为通常需要大量的工具和技术。不过不要紧张，我们将讨论和比较**两个框架** (Dash 和 Streamlit)，这使得**创建令人印象深刻的投资组合**(没有陡峭的学习曲线)！

你刚刚创建了一个机器学习模型。花了很长时间，但*终于完成了*，你想享受一下你的胜利。你应该休息一下…但是聪明的老尤知道建立一个纪念碑来展示你的工作的重要性。

你采取*自然下一步*，查找如何建立一个网站。他们从像 Flask 和 Django 这样的 Python 框架开始，然后继续到 JavaScript，不久你就会陷入考虑使用哪个前端框架，以及如何解析 Python 后端(模型)和 JavaScript 前端(实际网站)之间的数据。哦，天啊…这是一个又长又黑的兔子洞，很难穿过。但是突然，你听说有一个简单网站的简单解决方案。你查找这个新的 shinny 框架(Streamlit)，它确实很容易😊并且使用快捷。不久你就会忘记所有的烦恼和不安全感！但是你突然意识到 Streamlit 的问题…它只适用于简单的 Jupyter 笔记本-esk 网站。对你来说，一切都在网络开发列车上。请求和 JavaScript，你来了😰。

虽然不一定非得这样，但你可以找到中间立场。一些简单到可以在几天内理解的东西，但也足够复杂…嗯，几乎任何东西都可以🤓！欢迎来到 Dash。您仍然需要了解一些 web 基础知识(HTML 和 CSS)，但至少您的开发之旅有一条清晰的前进道路。即使感觉有点笨拙，它也能很好地完成工作！

整个过程可以归结为三个决定:

1.  你想在页面上看到什么(文本、图表、表格、图像等)
2.  如何排列和样式化页面(使用 CSS)
3.  您希望用户如何与页面交互

不再有 JavaScript、HTTP 请求，甚至不再有多个独立的前端和后端框架！

# 开始使用 Dash

要开始，请确保您安装了 Dash。对于普通 Python 使用`pip install dash`，对于 Anaconda 使用`conda install -c conda-forge dash`。接下来，创建一个新的 Python 文件并导入相关的库:

```
import dash
import dash_core_components as dcc
import dash_html_components as html
```

如果你试着运行这个应用程序，你会注意到一件事——没有任何反应。这是因为我们实际上必须创建一个 Dash 应用程序对象，并告诉它启动。

```
app = dash.Dash(__name__, external_stylesheets=["https://codepen.io/chriddyp/pen/bWLwgP.css"])
app.title = "Allocate++"

if __name__ == "__main__":
    app.run_server(debug=True)
```

我们可以包含一个样式表(使用`external_stylesheets`的 CSS)并设置我们网站的标题(`app.title`)，让事情看起来更好。检查`__name__ == "__main__"`只是确保网站只在直接启动时启动(而不是在另一个文件中导入)。

如果我们尝试运行这段代码，在终端中我们会得到如下消息:

```
Running on http://127.0.0.1:8050/
Debugger PIN: 409-929-250
 * Serving Flask app "Main" (lazy loading)
 * Environment: production
   WARNING: This is a development server. Do not use it in a production deployment.
   Use a production WSGI server instead.
 * Debug mode: on
Running on http://127.0.0.1:8050/
Debugger PIN: 791-028-264
```

它表示您的应用程序已经启动，可以使用 URL `http://127.0.0.1:8050/`找到。虽然它目前只是一个空白页(真实的*花哨的*)，但它确实表明一切都运行良好。

一旦你准备好继续，试着添加一个标题:

```
app.layout = html.H1(children="Fancy-Schmancy Website")
```

保存文件后，该网站应该会自动重新加载。如果它没有重新加载，或者屏幕上有弹出窗口，你可能在源代码中有一个错误。更多信息请查看实际的终端/调试器。

现在你已经熟悉了如何获得一个基本的网站，让我们继续把你的概念转换成代码。它从所谓的布局开始，布局由组件组成。Dash 提供核心(`dash_core_components`)和 HTML ( `dash_html_components`)组件。您总是从使用 HTML 元素开始，因为它们为文本和组合组件提供了基本的构建块，然后才是核心组件。核心组件提供更多的交互性(图形、表格、复选框...).现在很自然地会问，如何设计网页的样式。简而言之，您可以使用 CSS(层叠样式表)来实现这一点。Dash 本身提供了对核心组件的具体概述，可信的 Mozilla 有令人惊叹的 HTML 和 CSS 介绍。如何使用这些元素的几个例子是[这里的](https://dash.plotly.com/layout)。

任何 Dash 应用程序的最后一部分都是使其具有响应性。获取你点击的按钮、你输入的文本和你上传的图片…做点什么吧！这是事情通常会变得困难的地方，但这里真的*还不算太糟糕*。使用 Dash，您需要定义的只是一个接收和控制特定元素属性的函数。*属性*以“@”符号开始。

```
@app.callback(
    [dash.dependencies.Output("output element id", "property to set value of")],
    [dash.dependencies.Input("input element id", "input property")]
)
def update_output(value):
       return value
```

我们可以通过向这些列表中添加更多的`Input`和`Output`对象来为多个元素做这件事！不过这里要注意一件事——更多的`Input`对象意味着函数需要更多的输入，更多的`Output`对象意味着要返回更多的值(听起来很明显，但是很容易忘记)。另外，请注意，您*不应该在这些函数中修改全局变量*(出于技术原因，这是一种反模式)。关于这些[回调](https://dash.plotly.com/basic-callbacks)提供了进一步的文档。

# 前进和 JavaScript

这就是你开始创建一个交互式的令人印象深刻的 web 应用程序所需要知道的一切！创建一个可能仍然很困难，但是关于 [Steamlit](https://docs.streamlit.io/en/stable/getting_started.html) 和 [Dash](https://dash.plotly.com/) 的官方文档和教程是惊人的。也有使用 [Dash](https://dash-gallery.plotly.host/Portal/) 和 [Streamlit](https://www.streamlit.io/gallery) 的示例应用程序的酷图库(这样你就可以学习其他人的例子)。

当然，JavaScript 也有用例。其实你可以用 [JavaScript/React](https://dash.plotly.com/plugins) 和 [D3.js](https://dash.plotly.com/d3-react-components) 为 Dash 搭建插件。见鬼，如果你已经熟悉了网络技术，那么使用它们可能会更容易。然而，使用 JavaScript **不再是建立网站**的 100%必要了(它更像是可选的)。了解 web 技术可能是有用的，但是如果你的目标不是成为一个全栈 web 开发人员，你不需要成为一个专家来制作一个华而不实的作品集🥳！

希望这对你有所帮助！Dash 帮我在一天内组装了我的第一个仪表盘。如果你做了一个很酷的网站、应用程序或作品集，一定要发表评论并告诉我。可以随意查看我的其他帖子——一些亮点是[实用编码工具](https://www.kamwithk.com/the-complete-coding-practitioners-handbook-ck9u1vmgv03kg7bs1e5zwit2z?guid=34cbed9b-13ac-43c7-94a3-dbfe4ac247a9&deviceId=a348da4b-4d6e-44a9-80b2-3456c05bf4d0)、 [web 报废](https://www.kamwithk.com/zero-to-hero-data-collection-through-web-scraping-ck78o0bmg08ktd9s1bi7znd19)和[机器学习](https://www.kamwithk.com/machine-learning-field-guide-ckbbqt0iv025u5ks1a7kgjckx)(与[实用项目](https://www.kamwithk.com/machine-learning-energy-demand-prediction-project-part-1-data-cleaning-ckc5nni0j00edkss13rgm75h4))。你可以关注我的[时事通讯](https://www.kamwithk.com/)和[推特](https://twitter.com/kamwithk_)获取更新😉。

*原载于*[*https://www.kamwithk.com*](https://www.kamwithk.com/the-state-of-data-websites-and-portfolios-in-2020-develop-a-dashboard-in-a-day-dash-vs-streamlit-and-is-javascript-still-king-ckckn2lib00egf6s1eupt0wgv)*。*