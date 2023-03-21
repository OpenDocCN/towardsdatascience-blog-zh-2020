# 使用 Python、Dash 和 Moviepy 制作您自己的视频编辑器应用程序！

> 原文：<https://towardsdatascience.com/make-your-own-video-editor-app-with-python-dash-moviepy-f0dd57c2b68e?source=collection_archive---------24----------------------->

## ...以及如何处理 Dash 应用中的大型用户输入文件

![](img/811bb1a77b120515be6636c2dee98ea1.png)

来自 [Pixabay](https://pixabay.com/?utm_source=link-attribution&utm_medium=referral&utm_campaign=image&utm_content=205) 的 [NASA 图像](https://pixabay.com/users/NASA-Imagery-10/?utm_source=link-attribution&utm_medium=referral&utm_campaign=image&utm_content=205)背景视频

Plotly 的 [Dash](https://plotly.com/dash/) 框架是一个很棒的 Python 库，无需编写 HTML 或 JavaScript 就可以快速轻松地制作交互式 web 应用程序！ [Moviepy](https://zulko.github.io/moviepy/) 是一个优秀的以编程方式编辑视频文件的库。然而，开箱即用的 Dash 受到浏览器存储限制，这使得处理存储密集型视频文件变得困难。本文将介绍我们如何使用 [np-8 的 Dash-uploader](https://github.com/np-8/dash-uploader) 库来绕过大约 100 MB–200 MB 的浏览器存储限制，这将打开 Dash 应用程序，直到非常大的用户输入文件(视频或数据)。我们还将涉及到[dity view 的](https://github.com/thedirtyfew/dash-extensions/)`[ServerSideOutput](https://github.com/thedirtyfew/dash-extensions/)`(dash-extensions 的一部分)，它允许在服务器上轻松存储数据。你可以查看一下这个 GitHub 库[,它包含了 3 个 Dash 应用程序文件，我将在下面一一介绍。](https://github.com/shkiefer/dash_large_files)

# 存储问题...举例来说

好了，先说一些数据吧！我编写了一个脚本(`st_make_data.py`)，它创建了一个类似 json 格式的文件，模拟从微控制器串行传来的传感器数据。数据只是正弦和余弦曲线。我用它制作了几个行数逐渐增加的文件(10k、100k、1m、& 5m 行)，对应于大约 0.8MB、8MB、82MB、& 409MB 的文件大小。

这里我们有一个简单的 Dash 应用程序(`user_large_data_sql.py`)，它接收数据，解析数据并绘制数据(允许用户为每个轴选择数据)。上传数据有两个选项:标准 Dash `dcc.Store`对象和 Dash-uploader `du.Upload`对象(它也使用自己的 dcc)。存储对象以旧文件名)。

这个的回调有点特殊。它检查上下文以查看哪个上传对象被触发。如果标准的 upload 对象被触发，它将解析 base64 对象并从文件中收集行，就像处理小数据一样。最终，这个回调只是启用一个按钮来处理数据，但是这就是我们在处理较大文件时遇到麻烦的地方。

![](img/1b30bb596b2d4a19efc0d74f0f0bfc92.png)

较小的文件通过 dcc 工作。上传组件，但最大的文件通过回调错误

# dash-上传者来拯救！

下一个回调执行处理(按下`Process Data`按钮后)。它处理来自`dcc.Upload` 组件的`base64`数据，或者在服务器上打开文件，如果它是通过 Dash-uploader 组件来的话。然后，我们创建一个 sqlite 引擎，并将处理过的 dataframe 数据写入其中。

当文件通过 Dash-uploader 对象上传时，即使最大的文件也能正常工作！唯一的问题是策划。在绘图回调中，我将绘图的总行数限制为 200k，以免浏览器过载。如果需要的话，这可以通过重采样(对于绘图)来处理，但是对于这个例子来说似乎有点多。

# 简单的服务器端数据存储

在最后一个例子中，我创建了临时数据库文件来存储服务器上的大量数据。来自 [Dash-Extensions](https://github.com/thedirtyfew/dash-extensions/) 库的`ServerSideOutput`丰富使得在服务器上存储数据更加简单。我们不需要读写文件(也不需要保存文件名),我们只需要在回调中将对象发送回去。在这种情况下，它是经过处理的数据帧。需要注意的关键是装饰器顶部的特殊`ServersideOutput()`，我们只是在返回语句中传递了`df`。`user_large_data_cache.py`文件使用服务器端缓存。

# Python Web 应用程序视频编辑器

现在，在 web 应用程序中编写一个成熟的视频编辑器是不现实的。但是对于不了解媒体编辑的用户来说，自动完成一些简单的任务会很有帮助。给视频添加公司标志或一些文字。或者拍一部延时电影。也许转换格式和缩小添加到演示文稿。团队可以使用许多相对简单的固定编辑方案。

有了大文件上传功能，我们现在可以将用户的视频文件上传到服务器上进行处理。Moviepy 有很多功能。我们将浏览的示例应用程序几乎不会涉及它能做什么。

![](img/3b0f43a200cb73ac4fdbe8a5ad68781c.png)

使用最基本的视频编辑器

我使用[Dash-bootstrap-components](https://dash-bootstrap-components.opensource.faculty.ai/)来帮助布局和造型。`du.Upload`是 dash-uploader 组件，用于大文件上传。然后我添加几个`dbc.FormGroups`来存放用户输入。当组合多个 Dash 应用程序时，全局变量`APP_ID`保持组件名称的唯一性。这里不需要，只是习惯。我利用 Moviepy `TextClip.list(‘font’)`方法获取系统可用的字体，并在初始加载时填充下拉菜单。最后，我有 2 个 Div 元素用于发送图像预览(最终电影的第一帧)和视频预览。

至于回调，首先是 dash-uploader 回调，它将本地文件名链接到 dcc。存储组件。另一个回调检查上传视频的持续时间，并填充`Subclip End (s)` 和`Video Width (px)`用户输入。这些可能会被用户忽略，但是知道限制是很好的。

主要的两个回调用于创建预览帧和预览(和最终)视频。对于预览图像，我基本上应用了所有的视频编辑，但不是将蒙版附加到剪辑上，而是在将数组发送到 PIL 之前使用 numpy 函数应用蒙版。PIL 用于将图像数据写入缓冲区，然后编码为 base64 字符串，这样图像数据就可以直接嵌入，而不必处理磁盘上的文件。

创建预览图像的回调

视频预览回调也创建全尺寸电影，并返回链接信息和启用下载按钮。这可能应该是分开的，因为你必须等到预览和最终视频都创建后才能预览。作业！

现在我设置我的 Dash 文件有点不同。我将所有的回调封装在一个`add_dash()`函数中，该函数接受 Dash app 对象并在被调用时附加回调。这有助于将多个 Dash 应用程序集成在一起。请参阅本文了解更多信息:

[](/embed-multiple-dash-apps-in-flask-with-microsoft-authenticatio-44b734f74532) [## 使用 Microsoft Authenticatio 在 Flask 中嵌入多个 Dash 应用程序

### 唷！一次旅程(通过示例)让所有部分走到一起，为多个 Dash 应用程序构建一个可扩展的模板…

towardsdatascience.com](/embed-multiple-dash-apps-in-flask-with-microsoft-authenticatio-44b734f74532) 

有了这个设置，我在`if __name == ‘__main__’`语句中完成了所有的配置。

嗯，就是这样。我希望您发现这很有用，并帮助您解决大用户文件的问题。感谢阅读！