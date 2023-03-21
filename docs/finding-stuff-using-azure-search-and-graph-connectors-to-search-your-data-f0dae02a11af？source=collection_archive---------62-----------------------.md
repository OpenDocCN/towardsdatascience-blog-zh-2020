# 查找资料——使用 Azure 搜索和图形连接器来搜索您的数据。

> 原文：<https://towardsdatascience.com/finding-stuff-using-azure-search-and-graph-connectors-to-search-your-data-f0dae02a11af?source=collection_archive---------62----------------------->

## 微软 365 解决方案

## 搜索东西很费时间。让我们使用 Azure Search 和 Graph Connectors 使 Power BI 文档(或任何其他来源)可以在您的 Microsoft 365 和业务线应用程序中进行搜索

![](img/9f4a5e440db6f4ff7a1a86fcf5aeeb46.png)

照片由[埃迪·利贝丁斯基](https://unsplash.com/@supernov?utm_source=medium&utm_medium=referral)在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 拍摄

随着 Graph Connectors for Microsoft Search 的推出，我们现在可以使用 Graph Search API 从我们最喜欢的 Microsoft 365 应用程序(如 SharePoint)或我们的业务线应用程序中查询任何内容。

本文使用 Microsoft Power BI 公共文档作为我们搜索的来源，引导您创建这样一个连接器。

遵循此指南，您将从您的 Microsoft 365 应用程序中获得以下结果。

![](img/deb765c1105e34d649f2522320f00253.png)

从 Microsoft 365 应用程序中查询任何数据

## 创建图形连接器

为了创建我们的第一个连接器，我们打开[管理中心](https://admin.microsoft.com/)并进入**设置>微软搜索**。切换到**连接器**选项卡，点击**+添加**。

![](img/c7ec904880dfb6ac01e6eb3b8a92f8c4.png)

从内置连接器列表中选择**企业网站**，点击**下一步**。

![](img/ead69e8140eb6de89b7765f8a17e58fd.png)

给你的连接器起一个名字，比如**Power BI Fundamentals Docs**和一个 ID，比如`powerBiFundamentalsDocs`，**检查**条款和服务(值得一读！)并点击**下一步**。

![](img/606f5158bf7bc7618840fd0f0f645bb2.png)

我们现在输入将数据抓取到图表中的 URL。

对于我们的场景，我们将连接器指向 [Power BI 基础文档](https://docs.microsoft.com/en-us/power-bi/fundamentals/)。随意抓取网络上的任何其他文档或数据。

> 注意:根据您正在搜索的网站的大小，完全填充可能需要几个小时。

选择**认证类型**无**，点击**测试连接，点击**下一步**结束。

![](img/c366e78e46030f5c4b776cf9c52e6201.png)

随意过滤网址，甚至进一步或留为空白，以抓取所有低于给定的网址。点击下一个的**。**

![](img/4cb3fa24bfa4bafd20eb9926b259fe47.png)

保持模式不变，点击**下一步**。

![](img/8b1991352762ea2802646ca20a660be9.png)

**企业网站**连接器到目前为止还不支持权限，组织内的每个人都可以搜索到。点击下一个的**。**

![](img/0b7f531c2c00cfa78589aafba671a19f.png)

将**完全刷新间隔**设置为任意值，然后点击**下一步**。

> 重要提示:请始终遵守您抓取的任何网站的网页抓取和抓取政策。

![](img/49964cdc409131787e7a4cbc786cf966.png)

查看所有内容，点击**完成添加**。

![](img/34cf1872a0b65ea122d6496b0d1167a4.png)

创建连接器需要几分钟时间。使用上面的**刷新**图标刷新**连接状态**。

![](img/03b7a952fa071ff86668cbe5068a6682.png)

当连接器准备好时，点击**所需动作**栏中的**创建结果类型**。

![](img/df6d3a6b7c0c6f9e0eb31ce8c70aa0c3.png)

在接下来的几个步骤中，我们将定义搜索结果的显示方式以及结果中包含的数据。

将结果类型命名为**Power BI Fundamentals Docs**并点击 **Next。**

![](img/027a5b48afefd51cd0ce9c80825fc047.png)

选择我们之前创建的连接器，点击**下一步**。

![](img/1c18e9c552a438c2c960bd5d9787a265.png)

让**规则**保持原样。这将允许我们基于某些条件在各种结果类型之间切换。点击下一个的**。**

![](img/9a982a267d0d2543362f14eb6fb2e9f8.png)

在我们点击**启动布局设计器**之前，请注意下面的**可用属性**列表。这些是由**企业网站**连接器返回的属性。

![](img/6317644af36b25238816069dd9af030a.png)

在**布局设计器中，**我们从模板列表中选择一个模板来可视化搜索结果。我们选择第一个也是最简单的，点击**开始**。

![](img/6ba97764ca20bf1e08250d7bdfe3a801.png)

现在，我们用连接器的**可用属性**映射搜索结果的可视化表示。相应地放置**标题、URL、**和**描述**属性，点击**生成并复制 JSON** 。

![](img/0550ad2c86f1c47d522a6cf34885dcd2.png)

回到**结果类型**向导，粘贴剪贴板中生成的 JSON，点击**下一步**。

![](img/c25456e1e32ff1c2e4936bb53d782a54.png)

查看所有内容并点击**添加结果类型**。

![](img/531da36f1e90d7f582cba8b99414cb9a.png)

一旦确认，点击**完成**。

![](img/1293da7cb26c40eccaaa163c8ffaae0c.png)

现在是时候建立一个垂直市场了。这基本上是我们的用户在任何微软 365 应用程序(如 SharePoint)中看到搜索选项卡的方式。

点击**创建垂直**。

![](img/c3184f399eec15a4c4041997c934f2e0.png)

请给它一个用户友好的名称，如 **Power BI Fundamentals** ，然后单击**下一步**。

![](img/da6acab6e8348224caf9de83b5e945d9.png)

选择之前创建的连接器，点击**下一个**。

![](img/28510eb5ddd7a004809e37ef71df896a.png)

将**查询**留空，点击**下一个**。

![](img/d4b263277b3e5886c1fde6d891188178.png)

一旦确认，在点击**完成**之前，点击**启用垂直**。

![](img/f488b6191e62dbbbbbe1a7ffc6d3e57a.png)

就是这样！现在是时候从您的 SharePoint 中搜索一些内容了。

## 在 SharePoint 和其他地方搜索🔎

搜索内容非常简单，并且适用于各种场合。以下是几个你可以搜索的地方:

**SharePoint**

![](img/8e45b71c8b379151f978bc65ffb51a1b.png)

什么是 Power BI？

> 注意:要查看这些结果，用户必须分配有适当的许可证。根据[文档](https://docs.microsoft.com/en-us/microsoftsearch/connectors-overview#license-requirements)显示，要么是**微软 365 for enterprise E3/E5** 要么是**微软 365 Education A3/A5** 。根据我的测试，在任何情况下都可以通过 API 进行搜索(参见下面的示例)。

**Office 365 门户**

![](img/deb765c1105e34d649f2522320f00253.png)

什么是 Power BI？

**来自 Windows 应用商店的 Windows 10 Office 应用**

![](img/27e0f3119cea7cf395a5d0466b1ee203.png)![](img/6b78d2e96f9673b30fb1f8a53470560a.png)

什么是 Power BI？

## 使用图形搜索 API ⚡

除了从各种 Microsoft 365 应用程序中搜索连接器，我们还可以轻松地将它嵌入到我们的业务线应用程序、脚本或命令行中。

为此，我们使用[图形 API](https://docs.microsoft.com/en-us/graph/api/search-query?view=graph-rest-beta&tabs=http) ，以及它的搜索端点。最简单的测试方法是使用[图形浏览器](https://developer.microsoft.com/en-us/graph/graph-explorer)。

确保您已登录到图表浏览器。在您可以查询外部连接器之前，您需要您的应用程序的许可，在这种情况下，它是图形浏览器(它只是一个像任何其他应用程序一样的应用程序)。

为此，点击**修改权限**选项卡，并点击`External.Read.All`权限范围旁边的**同意**。

![](img/ded017c12a70f13ff398e7c611d080cb.png)

一旦授权，我们可以使用下面的有效负载向搜索端点发出一个 **put** 请求。

```
**put:** https://graph.microsoft.com/beta/search/query
```

请注意`contentSources`阵列，它必须与您的**连接器 ID** 匹配。

随意调整`query_string`，使用**运行查询**按钮发出请求，并浏览**响应预览**。

![](img/966fa6a761daa8207db9a890ba8bdebc.png)

这种方法适用于任何定制的应用程序或脚本。使用许多不同编程语言和平台的可用图形库，将搜索集成到应用程序中。

**就是这样！玩得开心！**

就这样吧👉，
塞巴斯蒂安