# 什么是 API，你能给我演示一下吗？

> 原文：<https://towardsdatascience.com/reflection-on-my-first-api-project-67d6cee31ce4?source=collection_archive---------67----------------------->

## 数字世界的搭便车指南

## 反思我的第一个 API 项目

![](img/d830d1342099b7fb418f7df2aa8b1b1c.png)

[迈克尔·布朗宁](https://unsplash.com/@michaelwb?utm_source=medium&utm_medium=referral)在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍照

作为一个新手，API 对我来说一直很神秘。我看过多个定义，也看过多个 Youtube 视频，但还是很抽象。

常见的解释总是类似于“允许两个应用程序相互对话的软件中介”或者“我们可以向它发送一个接收信息的请求…”等等…

但是，这是一个物理问题吗？我可以导入并使用的包？或者类似于编程语言的东西？我知道，它允许应用程序互相交谈，但是，那是什么东西？

如果你有同样的问题，请继续阅读。我希望这个博客能有所帮助。

## 应用程序接口

API 是一组代码。在这里，你现在正看着一个。

这是我最近项目的一个 API。该项目是一个网络应用程序，允许人们玩一个琐事游戏。这个 API 允许 web 应用程序显示所有问题，问题也可以按类别显示。

**是什么让这段代码成为 API？**

在我看来，它通常可以分为两大部分。

第一部分是第 1 部分:URL 和方法。这部分有两个目的。首先，***` ` @****app . route(…)``*是一个明显的信号，告诉你，你正在查看一个 API。其次，在这一部分，API 监听来自客户端的请求，即传入的 URL 地址。

如果你想知道为什么`` *@app.route()* `` `工作以及为什么每个 API 都以这种方式启动，这里有两个有用的资源。

[](https://stackoverflow.com/questions/47467658/flask-why-app-route-decorator-should-always-be-the-outermost) [## Flask:为什么 app.route() decorator，应该总是在最外面？

### 比方说，我有一个手工制作的@ log in-required decorator:from func tools import wraps def…

stackoverflow.com](https://stackoverflow.com/questions/47467658/flask-why-app-route-decorator-should-always-be-the-outermost)  [## 非魔法事物- Flask 和@app.route -第 1 部分

### 我已经有一段时间没有发博客了，所以我想是时候在我的博客上开始一个新的系列了。这是第一个…

ains.co](https://ains.co/blog/things-which-arent-magic-flask-part-1.html) 

第二部分和第三部分是 API 的本质或工作。这就是我们需要 API 的原因。在第 2 部分中，API 从数据库中检索客户机请求的信息。在第 3 部分中，API 告诉服务器应该如何响应客户机，以便客户机能够理解。

如果以上段落比较抽象。看看下面的代码。这些是我的前端代码。看它们是如何相互对应的。只是一个脚注，我用 React 和 Flask 和这个应用程序。

让我从另一个角度向你展示 API 是如何允许后端和前端相互对话的。

![](img/dd7779e31901f61fe6668a3b432ef5d4.png)

由 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上的 [Dhruv](https://unsplash.com/@dhruvywuvy?utm_source=medium&utm_medium=referral) 拍摄的照片

对于我的前端，我使用了 React。因此，我去: *localhost:3000* 查看我的网页。如果我做的一切都正确，网页上的所有按钮都应该工作。每当我点击一个按钮，相应的信息，存储在数据库中，应该显示在网页上。

假设现在我用不同的方式命名了一个变量。在我的后端，我把变量“total_questions”改成了“all_questions”，但是在我的前端，还是“result.total_questions”。会发生什么？

不出所料，当我单击网页上的一个按钮来检索所有问题时，我会遇到一个错误。

但是，如果我去 localhost 5000，即 [http://127.0.0.1:5000/](http://127.0.0.1:5000/) 。这是允许我访问我的服务器的 URL，只要我键入 write URL:[http://127 . 0 . 0 . 1:5000/questions](http://127.0.0.1:5000/questions)不会出现任何错误，我仍然可以看到所有的问题完美地显示出来。

这意味着服务器确切地知道如何从数据库中获取信息。我的功能完全正常。只是由于名称不匹配，它无法将此信息传输到前端。

如果你有一个不同的 URL 地址，同样的事情也会发生。您的后端 URL 地址可以在端口 5000 上很好地工作，但是您的前端会有错误。

我希望现在能更清楚什么是 API，以及 API 如何连接前端和后端。

当我第一次开始学习计算机网络时，我对“服务器”和“客户机”这两个术语与餐馆的比喻如此吻合而着迷。前台/客户是走进餐厅的客户。服务员就像餐馆里的服务员，他们端上菜肴，满足顾客的任何需求。数据库就像厨房。

如果我们用这个比喻，API 就是一个菜单。当顾客点“玛格丽塔比萨饼”时，他/她知道会发生什么。服务员也很清楚什么是“玛格丽塔披萨”。没有人需要解释:这是马苏里拉奶酪，罗勒，橄榄油等…这就是 API 的力量。

![](img/d830d1342099b7fb418f7df2aa8b1b1c.png)

照片由[迈克尔·布朗宁](https://unsplash.com/@michaelwb?utm_source=medium&utm_medium=referral)在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄

## 其他要点:

除了对 API 有非常清晰的理解之外，我还学到了关于 if 语句的惨痛教训。

我在看我卡了很久的两个端点。我在思考为什么他们花了我这么长时间来调试。我意识到一件事，在两次中，我都写了一个 *if 语句*，试图抓住一个特殊的场合。然而，因为我没有意识到我的 *if 语句*有问题，所以我不知道这两个 *if 语句*都是无用的。

这个特殊场合只是跳过了 if 语句并像普通场合一样进入了其他代码行。我以为是我逻辑有问题，所以花了那么多时间查也没有结果。

现在我知道了:反复检查 if 语句。仅仅因为它们在那里并且没有引起任何错误消息，并不意味着它们正在工作。它们是 if 语句，因此可以轻松跳过。

关于我的问题的更多细节，我把:

```
>> if x is None:
do y
```

但应该是

```
>> if x == []:
do y
```

我试图捕捉从用户那里接收到一个空输入的情况，这个输入碰巧是空字符串格式的。在 Python 中，null 或 none 不同于空字符串。这就是为什么我的两个 if 语句都没用。

None 不是空字符串，none 不是 0，none 就是 none…懂了吗？

GIF via [GIPHY](https://giphy.com/gifs/Qa5QRwDKJgZoQBMPnT/html5)

我仍然觉得这种差异非常有趣。对于那些想知道的人，下面有一个关于区别的很好的解释。简单来说，在 Python 中把“无”解释为“我不知道”比“什么都没有”更好。

[](https://www.quora.com/What-is-the-difference-between-Null-and-empty-string-in-Python#:~:text=All%20objects%20in%20Python%20are,of%20the%20string%20is%20zero.) [## Python 中 Null 和空字符串有什么区别？

### 回答(20 个中的 1 个):Python 根本没有空字符串。Python 确实有一个 None 值，但它不是一个字符串，而是一个…

www.quora.com](https://www.quora.com/What-is-the-difference-between-Null-and-empty-string-in-Python#:~:text=All%20objects%20in%20Python%20are,of%20the%20string%20is%20zero.) 

这些是我从这个项目中学到的详细而艰难的经验。

你看，这就是为什么你需要项目。

干杯，继续编码。

GIF via [GIPHY](https://giphy.com/gifs/E3L5goMMSoAAo/html5)

其他资源:

另一篇解释 API 的文章。我喜欢这个图，但是把 API 画成一个盒子会让它有点混乱。

[](https://medium.com/@perrysetgo/what-exactly-is-an-api-69f36968a41f) [## API 到底是什么？

### 你有没有听说过“API”这个词，并想知道这到底是什么？你是否有一个模糊的想法，但是…

medium.com](https://medium.com/@perrysetgo/what-exactly-is-an-api-69f36968a41f)