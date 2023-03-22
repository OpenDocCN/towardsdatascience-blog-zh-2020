# Streamlit 还不能取代 Flask

> 原文：<https://towardsdatascience.com/streamlit-can-not-yet-replace-flask-the-streamlit-2020-roadmap-64840564acde?source=collection_archive---------12----------------------->

## Streamlit 2020 路线图

我讨论了 Streamlit 的发展方向以及 Flask**目前的优势。 **Streamlit** 1.0 于 2019 年 10 月公布。**烧瓶**发布于 2010 年 4 月 1 日。对于五年后的机器学习生产部署， **Streamlit** 会比 **Flask** 更好吗？**

![](img/b85aedfa68bf46929cf10918cb7b63dd.png)

仪表板示例；资料来源:联合国人类住区规划署

## 介绍 Streamlit

**Streamlit** 是一个为机器学习科学家或工程师创建基于 Web 的前端的工具。具体来说， **Streamlit** 使用 **HTML** 、 **CSS** 和 **Javascript** 但不需要开发者知道 **HTML** 、 **CSS** 和 **Javascript** 。

**Streamlit** 团队正在使机器学习科学家能够在不使用 **Flask、Django、**或其他工具的情况下进行部署。

> 来自[https://www . streamlit . io](https://www.streamlit.io)
> 
> “Streamlit 是一个面向机器学习和数据科学团队的开源应用框架……全部免费。

**Streamlit** 是一个开源但封闭的框架，因为:

1.  机器学习科学家的目标是快速推出基于 **Python-** 的 Web GUI 前端(仪表板)。
2.  **Streamlit** 为第三方插件提供了一个框架，这是未来 **Streamlit** 版本**的次要考虑因素。**

在这篇文章中，我讨论了 Streamlit 的路线图，它似乎将走向何方，并将其与现在的 Streamlit**和 Streamlit**进行比较。

## 引入烧瓶

当建立网站时，你应该有一个健壮的框架来处理所有类型的功能。在 **Python** Web 前端软件工程师中最流行的微服务框架之一是 **Flask** 。

## 扩展、工具、插件

**烧瓶**发布于 2010 年 4 月 1 日。它开始是围绕 [Werkzeug](https://palletsprojects.com/p/werkzeug) 和 [Jinja](https://palletsprojects.com/p/jinja) 的包装。

**Flask** foundation 是一个插件架构。大多数插件都是大型 **Python** 包。我们的开发人员使用超过七个**烧瓶**扩展。例子包括 **Bootstrap** 、 **SQLAlchemy** 、 **Flask8** 等等。

我发现 Flask 是一个健壮的框架。所有的扩展使它更加强大。更重要的是，它通过提供适合您的包扩展来适应您的编程风格。

> 来自[https://palletsprojects.com/p/flask/](https://palletsprojects.com/p/flask/)
> 
> “由开发人员来选择他们想要使用的工具和库。社区提供了许多扩展，使得添加新功能变得容易”。

**Streamlit** 是一个封闭框架**。**目前，任何定制功能的唯一解决方案都是克隆**Streamlit****Github**repo 并放入您的定制更改。

蒸汽点燃的路线图承诺了什么:

> 来自[https://discuse . streamlit . io/t/the-streamlit-roadmap-big-plans-for-2020/2054](https://discuss.streamlit.io/t/the-streamlit-roadmap-big-plans-for-2020/2054)
> 
> “Streamlit 插件系统将让你能够编写任意 React 或 Javascript 代码，并将其插入到你的应用程序中。”

用于 **Streamlit** 的插件架构正处于[社区讨论阶段。](https://github.com/streamlit/streamlit/issues/327)

## 网络安全

在前一篇[帖子](/part-2-will-streamlit-cause-the-extinction-of-flask-395d282296ed)中，我写道:

> 我们还没有遇到一个基于 ML 或 DL 的 [**Flask**](https://www.fullstackpython.com/flask.html) 的微服务不能被重构为[**Streamlit**](https://www.streamlit.io/)**服务。**

**我能说什么呢？那是早期。我们*为在 ML 应用程序中部署漂亮的交互式 Web GUI 前端而感到兴奋。我们成了上层管理的典型代表。然后，他们展示了一个基于 Streamlit 的客户端，这个客户端在我们的防火墙之外并且想要它。***

**好吧…？没问题。我们问 Web 应用人员( *WA-dev)什么是好的解决方案？*他们告诉我们，如果用 ***烧瓶*** ，我们有很多选择*。***

**我们决定为[**Streamlit**](https://www.streamlit.io/)**开发一个(小型)安全登录插件。****

**为 [**Streamlit**](https://www.streamlit.io/) 开发的安全登录插件变成了一个兔子洞。三个版本之后，我们仍然没有获得通过*验收测试* ( *AC-Test* )的安全登录。**

**我们被上层管理人员拉出了兔子洞，他们建议(命令)我们等待 [**团队** beta](https://discuss.streamlit.io/t/the-streamlit-roadmap-big-plans-for-2020/2054) **的精简。****

**[**Streamlit for Teams**](https://discuss.streamlit.io/t/the-streamlit-roadmap-big-plans-for-2020/2054)**beta 将免费(暂时？)并在几个月后*上市*。它承诺认证，我们希望这意味着安全登录。Streamlit 也承诺日志记录和自动缩放(不确定那是什么)。****

# ****贮藏****

****在这一点上，未来的 **Streamlit** 版本不会帮助我们解决`st.cache` 生产问题。****

> ****来自[https://discuse . streamlit . io/t/the-streamlit-roadmap-big-plans-for-2020/2054](https://discuss.streamlit.io/t/the-streamlit-roadmap-big-plans-for-2020/2054)****
> 
> ****接下来是对其他函数类型的高速缓存的更多改进，以及一些使高速缓存更加简单的魔法。****

******烧瓶**也有缓存。 *Web 应用程序开发* ( *WP-dev* )使用主**烧瓶缓存**和 **werkzeug。**他们告诉我还有其他 **Flask** 附件可以缓存。****

******背景(不想看长毛狗的故事就跳过):******

****我们中的一些人喜欢在早上 6:00 到 6:30 之间早点来。我们注意到，大约在三月初，管理员启动了分析师的计算机。分析师在上午 8:00 到 8:30 之间到达*为什么要采用这一新程序？*****

****我们问管理员— *为什么是*？因为他们仪表板中的一个画面需要 30 到 45 分钟才能出现。分析师需要启动他们的计算机，所以当他们到达时，仪表板已经准备好了。****

****啊？为什么我们必须向管理员询问分析师仪表板的问题？首先，好消息。分析师非常喜欢这个新仪表板。现在，坏消息是。它没有被打破(在他们的思维模式中),分析师*相信*我们会把它拿走或者打破框架。(可能是我们偏执吧。他们实际上并没有这么说。)****

*****Dev* 知道从现在起三个月后，当画框花费一个小时或更多时间时，它将是*【不可接受】*。从现在起的 9 到 12 个月内，这个框架将被正式打破。****

****我们是怎么知道的？****

****该框架对时间序列数据进行了深度学习训练和预测。*列车*数据集在清晨从测试数据集实际值更新。我们中的一个人精心编写了逻辑，只在周末训练。然而，`st.cache`导致训练发生在星期二到星期六的清晨，或者每当*训练*数据集更新时。****

****有两种直接的补救措施:****

1.  ****训练数据集调整为三个月的每日数据(2000 万行)。它还没有三个月的数据，但在 4 月中旬之后，它将拥有超过三个月的数据。****
2.  ****`st.cache`被注释掉了。****

****从长远来看，为了加快速度，我们正在考虑修剪神经网络和/或重写 Swift 中的部分。我们中的一个人正在看 **cPython** ，而首席技术官说，“云”(我说——“目前无可奉告。”).读者可能有任何想法，请告诉我。****

******(结束背景)******

****AC-Test 禁止在生产代码中使用`st.cache`。我们使用`st.cache`进行*开发*实验、调试和单元测试。我们认为`st.cache`是为研究员和卡格勒设计的。如果我们对`st.cache.`的理解有误，请告诉我****

# ****在 Streamlit 应用程序之间传递状态****

****我们希望在 **Streamlit** 微服务(页面)之间传递状态数据。****

****Flask 没有这个问题，因为他们使用加密的 cookies 或者调用 URL 中的 args 来传递状态。****

******Streamlit** 路线图称之为“可编程状态”:****

> ****现在，让一个 Streamlit 应用程序存储内部状态，比如用户在表单中输入的信息，简直太棘手了。..我们希望给你…可编程的状态，这样你就可以构建具有顺序逻辑的应用程序，具有多个页面的应用程序，不断要求用户输入的应用程序，等等。****

****我们等待未来版本的**Streamlit**在页面间传递状态。****

## ****美术馆****

******Streamlit 的** [画廊](http://streamlit/)展示了 **Streamlit** 用户创建的赏心悦目的仪表盘式应用程序(有些相当聪明)。注意:所有应用都是数据工程或者机器学习或者深度学习。毕竟，AI 2.0 Web GUI 开发者和他们的用户是目标市场。****

> ****如果你想展示你的应用，只需发推特给我们 [@streamlit！](https://twitter.com/streamlit)****

******烧瓶**有很多画廊。下面是一些包含了 **Flask-Python** 源代码的例子。****

*   ****[**烧瓶**软件图库](https://devpost.com/software/built-with/flask)****
*   ****[**烧瓶**照片库](https://github.com/evac/Photo-Gallery)****
*   ******F** [**lask** 仪表盘:开源样板文件](https://dev.to/sm0ke/flask-dashboard-open-source-boilerplates-dkg)****
*   ****[**烧瓶**仪表盘——开源免费](https://www.codementor.io/@chirilovadrian360/flask-dashboards-open-source-and-free-yzzi4c16h) ( *WP-dev* 最爱)****

## ****为 Streamlit 提供更多资源****

****不确定参考的是不是极致，但是顾名思义就是牛逼。****

****[Awesome Streamlit 资源](https://awesome-streamlit.readthedocs.io/en/latest/awesome-list.html)****

****[](/streamlit-101-an-in-depth-introduction-fc8aad9492f2) [## Streamlit 101:深入介绍

### 利用 Airbnb 数据深入了解 Streamlit

towardsdatascience.com](/streamlit-101-an-in-depth-introduction-fc8aad9492f2) 

[牛逼图库](https://www.streamlit.io/gallery)

## Flask 的更多资源

很好的参考:[托盘项目](https://palletsprojects.com/p/)

## 摘要

我们希望 **Streamlit** 的未来版本能够为我们提供以下功能:

1.  安全登录；
2.  在基于 Streamlit 的网页之间传递状态；
3.  一个提供**精简**扩展、工具和插件的第三方生态系统。

最后一项不在**简化 it 的**路线图中，但在我们的**简化 it** 路线图中。

我收回我之前关于**烧瓶**的一些说法。

[](/part-2-will-streamlit-cause-the-extinction-of-flask-395d282296ed) [## Streamlit 会导致 Flask 灭绝吗？

### 可能对于机器学习(ML)和深度学习(DL)来说。对于其他全栈应用，大概不会！

towardsdatascience.com](/part-2-will-streamlit-cause-the-extinction-of-flask-395d282296ed) 

Flask 拥有我们现在生产部署一个 Web 微服务所需要的一切。**细流** *不*。

但是，我仍然坚持这个主张:

1.  一个好的全栈**烧瓶**程序员有多年的经验。他们需要知道 **Javascript** 、 **HTML** 、 **CSS 等..、**和堆栈中不同的 **POST/GET URL** 包
2.  黑客(就像机器学习科学家)只需要几周的经验就可以设计、开发和部署一个 **Streamlit** 生产就绪的基于网络的仪表板。

我们发现, **Flask** 是一个成熟的开放框架，拥有一个健壮的扩展生态系统。

Streamlit 目前是一个封闭的框架，我们依赖于它的未来版本。我们最大的希望是 **Streamlit** 成为一个具有插件架构解决方案的开放框架。****