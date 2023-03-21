# 使用 GitHub 创建漂亮的静态网页

> 原文：<https://towardsdatascience.com/building-a-beautiful-static-webpage-using-github-f0f92c6e1f02?source=collection_archive---------26----------------------->

## 查找模板和为静态网页创建表单的位置

![](img/ce025c7cbe80cd7dc282e158eaa2d04e.png)

使用 GitHub Pages 托管的我的个人网页

你有没有想过建立自己的网页？也许你对 HTML 有一些经验(你好 Myspacers)，也许你没有。这篇构建自己网页的教程是对 GitHub 页面教程的延伸。本文将介绍从哪里获得 HTML 模板以及如何创建动态表单，以便其他用户可以在您的 GitHub Pages 站点上连接到您。如果你正在经营一个有大量网络流量的大企业，我不推荐使用 GitHub 页面，因为静态网页的功能有限。对于个人网页或小型企业，GitHub 页面是一个很好的选择。TL；博士在总结的底部。

## 创建网站的步骤

1.  **查看该域名是否可用**:我推荐使用域名搜索工具，比如 Whois.net。此工具允许您查看该域是否可用或谁拥有该位置。有些域名不包括域名所有者信息，而是有域名托管站点的联系信息。
2.  购买域名:你可以通过任意数量的网站购买可用的域名。我会避免购买域名从另一个人或更糟，域名停车公司。
3.  **设置 GitHub:** 对于不熟悉的人来说， [GitHub](https://github.com/) 是一个版本控制的代码库。简单来说就是云端的代码备份。您可以注册一个电子邮件地址，并完成如何创建存储库的教程。存储库是你存储网站资料的地方。一旦您能够创建一个存储库，您将添加您的模板材料。在创建模板之前，我们可以将存储库连接到 GitHub 页面。为此，请单击位于存储库屏幕右上角的设置选项卡。向下滚动到标题为 **GitHub 页面**的部分。编辑 Source 选项卡，将存储库附加到 Github 页面。再次向下滚动到 GitHub 页面部分。您现在可以选择使用 [**Jekyll**](http://jekyllthemes.org/) 添加模板。
4.  **编辑模板:** Github 和 Jekyll 一起提供了一个简单的 HTML 模板。Jekyll 很棒，可能适合你的网站设计需求，但是我喜欢从 [HTML5 到](https://html5up.net/)的模板。您可以下载一个模板，将其解压缩，然后通过单击绿色克隆或下载按钮旁边的上传文件按钮，将文件直接添加到您的存储库中。您可以通过简单地拖放文件将模板直接添加到存储库中。编辑模板以个性化您的网页。
5.  **将您的存储库连接到您的域(棘手的部分):**再次点击 settings 选项卡，向下滚动到 GitHub 页面。在自定义域部分输入您的域，然后单击保存。此更改将在您的存储库中创建一个 CNAME 文件。**转到您购买域名的网站，并导航到您可以管理域名设置的位置。你正在寻找一个像 CNAME，A，或 NS** 项目的屏幕。将 CNAME 更改为指向您的 GitHub 页面库。这个名字可以在 GitHub Pages 部分的文本*“自定义域允许您从* `*your-repository.github.io*` *以外的域为您的站点提供服务”下找到。*您可以将 A 名称更改为指向 GitHub Pages 服务器。我会把 GitHub A 的四个名字都加上:`185.199.108.153 | 185.199.109.153 | 185.199.110.153 | 185.199.111.153`。在 GitHub 页面标签下有一个强制 HTTPS 的复选框。出于安全原因，如果可以，请选中此框。
6.  **在静态网页上创建动态联系表单:** GitHub 页面非常适合静态网页(也就是说，如果你不做更改，它们就不会更新)，但是，如果你想添加一个需要第三方帮助的联系表单。查看 Formspree.io，了解如何向 html 文档添加表单模板的教程。基本上，你用 html 创建一个表单或者使用他们的模板，然后指向你的账户链接。html 将包括类似于`<form action="https:formspree.io/yourformlink" method="POST"`的东西，后跟你的表单和 html 标签来关闭所有东西。下面是一个 HTML 格式的示例要点。

## 概述

这就是使用 [Github Pages](https://pages.github.com/) 创建个人网页的要点。探索[杰基尔](http://jekyllthemes.org/)和 [HTML5 UP](https://html5up.net/) 上的一些模板。对于那些想要一些动态内容的人来说，比如联系方式，使用第三方如[forms spree . io](https://formspree.io/)将你的电子邮件链接到静态网页。我用这些方法创建的网站包括我的个人网站[codyglickman.com](https://codyglickman.com/)和我的初创公司 Data Dolittle 的网站[datadolittle.com](https://datadolittle.com/)。感谢 Formspree.io 的 Cole 让我在静态页面上创建 html 表单变得简单。你可以在 LinkedIn 上找到我。感谢您与本文互动！如果您有任何问题或意见，请留言或通过我的个人网站与我联系。