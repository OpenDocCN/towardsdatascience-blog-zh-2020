# 你的网站是否泄露了敏感信息？

> 原文：<https://towardsdatascience.com/xs-leaks-is-your-website-leaking-the-sensitive-information-c190eff5d548?source=collection_archive---------56----------------------->

## 对于导致用户信息泄露的 XSLeaks(跨站点泄露)缺乏关注

![](img/5ff0ae202b9ce8219f7157f2633053e4.png)

[来源](https://unsplash.com/photos/fPxOowbR6ls)

大多数开发人员都熟悉并了解这些安全漏洞[【XSS】](https://www.youtube.com/watch?v=L5l9lSnNMxg)(跨站脚本)、[【CSRF】](https://www.youtube.com/watch?v=vRBihr41JTo)(跨站请求伪造)或 [SQL 注入](https://www.youtube.com/watch?v=_jKylhJtPmI)，但对可能导致用户信息泄露的 XSLeaks(跨站泄漏)却缺乏关注。在这篇博客中，我将对它做一个简单的介绍。

## **什么是 XSLeaks？**

XSLeaks 是一类漏洞，第三方恶意网站能够发送请求或将用户重定向到目标服务，然后测量侧信道信号(例如，响应时间、浏览器中的静态资源缓存、HTTP 响应状态代码、响应内容等)..)来基于这样的副信道信号推断和收集关于用户的信息。

## **现实世界**举例:你在看我的博客，如果我知道你是谁呢？？

2013 年，有一个[报告的错误](http://patorjk.com/blog/2013/03/01/facebook-user-identification-bug/)允许网站使用脸书来检测访问者是否是特定的人。脸书徽章的预览图像是基于当时用户的脸书 ID 动态生成的。如果用户 1234567 已经登录，图像将类似于图 1。如果其他用户试图加载用户 1234567 的徽章图像，则会加载脸书徽标图像，而不是个人资料图像(图 2)。基本上，只有用户自己可以查看他们的个人资料徽章预览。

> ![](”https://facebook.com/badge_edit.php?userid=1234567″)

![](img/bbe18c66dc36071496c1bd9b211a7bfb.png)

图一。有效的登录用户。

![](img/7086bb857b3ebcf8c7a52deb3c10627c.png)

图二。无效的登录用户。

尽管脸书已经很好地屏蔽了姓名和电子邮件地址的信息，但它仍然泄露了用户身份。黑客可以使用 JavaScript 悄悄加载预定义用户 id 列表的配置文件出价预览，然后检查响应图像的高度和宽度，以了解特定用户正在查看他们的页面。

如果上面的例子对你来说已经相当过时了，2018 年年中还有另一个[报告的 bug](https://www.tomanthony.co.uk/blog/facebook-bug-confirm-user-identities/) 。汤姆·安东尼发现了一种可以达到类似结果的方法。今天，与 2013 年相比，大多数脸书后端端点都受到了很好的保护，它们加载了`access-control-allow-origin`、`x-frame-options` 标头，以防止恶意网站使用 XHR 请求、iframe、图像标签等嵌入和调查 FB 内容。然而，Tom 发现一个 api(以用户 ID 作为参数)为匹配用户和非匹配用户提供了不同的`content-type`。然后，他使用了一个简单的 JavaScript 脚本，该脚本将获取一个用户 id 列表，并生成许多带有等同于端点的`src`的`script`标签。`script`标签当然会失败，但是由于响应内容类型的不同，它们会以不同的方式失败，并且可以通过`onload`和`onerror`事件处理程序检测到。此外，该端点似乎没有任何速率限制，他可以在一分钟内检查数千个 id。

如果上述漏洞没有被修复，人们可能会利用它们来跟踪特定的人(比如老板、前女友、名人..)阅读他们的博客，或者他们可以根据访问者显示不同的内容。更多关于 XS 泄露攻击的真实例子可以在这里找到。

# 怎么防守？

web 应用程序的本质通常会导致大量暴露在 web 中的端点暴露用户的敏感数据，并且浏览器实现使网站很容易受到 XSLeak 攻击。幸运的是，XSLeak 已经引起了浏览器制造商的注意，并且有一些机制可以避免它。

[**X-帧-选项**](https://developer.mozilla.org/en-US/docs/Web/HTTP/Headers/X-Frame-Options)

使用`X-Frame-Options: DENY`会导致任何在框架中加载页面的尝试失败。它不仅能防御[点击劫持](https://en.wikipedia.org/wiki/Clickjacking)，还能帮助防御 XSLeaks。如果页面需要嵌入到其他站点(广告、小部件等等)，那么页面中包含的信息应该尽可能的稳定和可预测。

[**same site cookie**](https://developer.mozilla.org/en-US/docs/Web/HTTP/Headers/Set-Cookie/SameSite)

在某些情况下，恶意网站可以删除受攻击网站的特定资源的缓存，迫使浏览器再次呈现这些信息，并检查浏览器是否确实缓存了最初删除的资源。这使得攻击者能够判断网站是否加载了特定的资源(如图像、脚本或样式表),从而识别用户或泄露用户的一些信息。

严格的同站点 cookies 有助于避免上述情况，因为它将用户手动键入 URL、单击书签或单击当前站点的链接所生成的请求与其他网站发起的请求区分开来。

你可以参考这个[链接](https://sirdarckcat.blogspot.com/2019/03/http-cache-cross-site-leaks.html)了解更多关于这种攻击的细节。

[**获取元数据请求头**](https://www.w3.org/TR/fetch-metadata/)

现代浏览器实现了 sec fetch 头，为服务器提供了更多的关于请求被触发的原因/位置的上下文。这些信息帮助服务器基于测试一组先决条件快速拒绝请求。

例如，由`img`元素生成的请求将导致包含以下 HTTP 请求头的请求:

```
Sec-Fetch-Dest: image
Sec-Fetch-Mode: no-cors
Sec-Fetch-Site: cross-site
```

服务器可以对端点实施策略检查，并使用`Sec-Fetch-Dest`快速拒绝非图像内容的请求。

# 结论

XSLeak 攻击领域的研究正在进行中，我相信在接下来的几年里会有更多的技术出现。作为一名开发者，重要的是要意识到这个问题有多严重，并开始更加注意保护你的网站免受这种攻击。

## 参考资料:

1.  [https://ports wigger . net/research/xs-leak-leaking-ids-using-focus](https://portswigger.net/research/xs-leak-leaking-ids-using-focus)
2.  [https://github.com/xsleaks/xsleaks/wiki/Defenses](https://github.com/xsleaks/xsleaks/wiki/Defenses)
3.  [http://patorjk . com/blog/2013/03/01/Facebook-user-identificati on-bug/](http://patorjk.com/blog/2013/03/01/facebook-user-identification-bug/)
4.  [https://www . tomanthony . co . uk/blog/Facebook-bug-confirm-user-identities/](https://www.tomanthony.co.uk/blog/facebook-bug-confirm-user-identities/)
5.  [https://ports wigger . net/daily-swig/new-xs-leak-techniques-reveal-fresh-ways-to-expose-user-information](https://portswigger.net/daily-swig/new-xs-leak-techniques-reveal-fresh-ways-to-expose-user-information)
6.  [https://medium . com/bugbountywriteup/cross-site-content-and-status-types-leakage-ef 2 dab 0a 492](https://medium.com/bugbountywriteup/cross-site-content-and-status-types-leakage-ef2dab0a492)
7.  [https://scarybestsecurity . blogspot . com/2009/12/cross-domain-search-timing . html](https://scarybeastsecurity.blogspot.com/2009/12/cross-domain-search-timing.html)