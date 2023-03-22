# API 爬网:遵守速率限制

> 原文：<https://towardsdatascience.com/api-crawls-respecting-rate-limits-30cc0f76ca98?source=collection_archive---------33----------------------->

## 这叫爬行是有原因的。

任何通过网络 API 公开信息的人都有可能对 API 客户端实施速率限制。这降低了[拒绝服务攻击](https://en.wikipedia.org/wiki/Denial-of-service_attack)的风险，帮助 API 提供商控制和预测他们的基础设施成本，并限制 API 故障的严重性。

爬行通常可以，跑步通常不行。怎么爬得快？

![](img/2738df412989da24f740d3ecf3c83b66.png)

由[维达尔·诺德里-马西森](https://unsplash.com/@vidarnm?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)在 [Unsplash](https://unsplash.com/s/photos/spider?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) 上拍摄的照片

# 第一步:了解自己的极限

一些 API 使得调用者很容易检查他们的速率限制。GitHub API 是我最喜欢的例子。它[在响应报头](https://developer.github.com/v3/#rate-limiting)中返回速率限制信息，并提供一个 [*/rate_limit* 端点](https://developer.github.com/v3/rate_limit/)，该端点不计入调用者的速率限制。

对于其他 API，您负责跟踪自己的使用情况。通读 API 文档，注意您将访问的端点上的速率限制。

此外，知道你将如何被处罚超过率限制。搜索谷歌太快会屏蔽超过 24 小时。太快抓取 GitHub 通常会锁定你最多一个小时。

# 第二步:预算

确定爬网中将涉及多少个进程，计算每个进程在爬网的单个步骤中将进行的速率受限 API 调用的数量，并确定每个进程的预算。

有几种方法可以强制执行预算:

1.  让你的爬虫知道速率限制。生成每个 crawler 进程，其速率限制与相对于其群组中其余 crawler 的爬行量成比例。当您可以在开始爬网之前轻松地将输入划分到各个爬网程序时，这种方法非常有效。
2.  使用速率受限队列将作业提供给 crawler 进程。如果每个爬行器进程的输入都是在爬行过程中动态生成的，那么这样做效果很好。
3.  在出站代理中强制实施速率限制。像 *nginx* 这样的代理提供了现成的功能。[下面是一个如何使用 *nginx* 来限制 API 请求的例子。](https://www.monterail.com/blog/2011/outbound-api-rate-limits-the-nginx-way)

无论你选择做什么，做一些算术，确保不超出你的预算。

# 第三步:当风险很高时，增加一个关闭开关

有时，超出速率限制的成本高得惊人。如果 API 在您的应用程序中提供了一个关键功能，或者如果它的定价在无法承受的上层，就会发生这种情况。如果是这种情况，实现一个 kill switch，如果你太接近你的速率限制，它会关闭你所有的爬虫。

# 第四步:当风险很低时，慢慢后退

如果超过 API 速率限制不是世界末日，你的爬虫应该简单地重试他们的 API 调用，增加尝试之间的时间间隔，直到达到一定的尝试次数。这种技术的名称是*后退重试*。补偿通常是指数的，这意味着每次失败的尝试，API 调用之间的时间间隔都会被放大一个固定的倍数 *r > 1* 。这篇 AWS 文章是一个很好的战略指南。

这些是需要遵循的简单准则，遵循它们将使您轻松地将 API 抓取扩展到笔记本电脑之外。祝你好运，爬虫伙伴！

*Neeraj Kashyap(跟我上*[*Twitter*](https://twitter.com/zomglings)*和上*[*GitHub*](https://github.com/nkashy1)*)*