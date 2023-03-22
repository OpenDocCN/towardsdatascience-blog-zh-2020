# 边缘人工智能:未来的人工智能架构

> 原文：<https://towardsdatascience.com/will-edge-ai-be-the-ml-architecture-of-the-future-42663d3cbb5?source=collection_archive---------35----------------------->

## 了解 Edge AI 的基础知识及其在人工智能领域日益增长的重要性

![](img/0e6d03d823966125157cbcbafcda8125.png)

授权给作者的图像

Edge AI 描述了一类 ML 架构，其中 AI 算法在设备上本地处理(在网络边缘)。使用 Edge AI 的设备不需要连接就能正常工作，可以在没有连接的情况下独立处理数据和做出决策。了解为什么这在人工智能的现代应用中变得越来越重要。

# **典型的 ML 架构**

其中一个你应该很熟悉，将会有一个 ML 模型，精心制作，训练和托管在云基础设施上，预测请求从设备发送到云基础设施。这些请求包括向基于云的 API 发送请求，并通过互联网接收响应。

这些请求包括向基于云的 API 发送请求，然后通过互联网接收响应。当传输的数据很小(如文本片段)时，这通常是一种成功的方法，但当数据较大(如高质量的照片或视频)时，这种方法就会失效。在网络覆盖差(或无网络覆盖)的地区，即使中等大小的数据也会造成问题。

# **边缘艾**

边缘人工智能的想法是让模型生活在网络边缘的设备上(因此得名)。然后，人工智能算法在设备上进行本地处理，不再需要互联网连接来处理数据和生成有用的结果。

2020 年，德勤[预测](https://www2.deloitte.com/cn/en/pages/technology-media-and-telecommunications/articles/tmt-predictions-2020-ai-chips.html#:~:text=We%20predict%20that%20in%202020,sold%20and%20their%20dollar%20value.)将售出超过 7.5 亿个边缘人工智能芯片，这些芯片在设备上执行或加速机器学习任务，而不是在远程数据中心，这意味着 26 亿美元的收入。

# 边缘经营的优势

Edge AI 在传统的 ML 架构上提供了很多改进。首先，消除了任何网络传输所涉及的延迟，这在某些用例中可能是至关重要的。流数据所涉及的电池消耗不再是一个问题，允许更长的电池寿命，并且数据通信的相关成本显著降低。

这对于许多用例来说是非常有益的。像海上风电场这样的偏远地区的传感器可以预装算法，使它们能够在没有复杂的互联网连接基础设施的情况下做出决定。

![](img/63ee58c124c3cb94e8fd17bfc390db7f.png)

尼古拉斯·多尔蒂在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄的照片

类似地，[这种方法正被用于](http://www.oilandgastechnology.net/news/adding-intelligence-pipeline-monitoring)监控地下气体管道的流量，在这种情况下，基于云的策略不可行。传感器测量流速和压力，以确定管道的健康状况，如果检测到泄漏的迹象，阀门可以关闭。

![](img/289af9bf152ed5cfa92159c5149a813b.png)

照片由[米卡·鲍梅斯特](https://unsplash.com/@mbaumi?utm_source=medium&utm_medium=referral)在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄

# Edge AI 的其他现实应用

Edge AI 并不是偏远地区的专利，它已经在离家更近的商业街上被采用。

![](img/e2409ba81a439cddcd4d2c4328d21a85.png)

授权给作者的图像

英国化妆品品牌 *Lush* 在一项新举措中使用了一种边缘人工智能方法；他们的 *Lush Labs* 应用程序最近增加了 *Lush Lens* 功能。

设计用于帮助减少包装需求的*镜头*用于通过智能手机的摄像头扫描产品。在引擎盖下，应用程序中有一个图像识别模型，利用 Edge AI 来降低电池消耗和网络要求。正确识别产品后，用户无需包装即可获得详细的产品信息。

了解更多关于 Lush Lens 如何使用 AI 来减少包装的信息[在这里](https://uk.lush.com/article/evolution-continues-lush-labs-app)。

![](img/b86326490434691fe957916802af2a1a.png)

授权给作者的图像

最后，边缘人工智能芯片可能会进入越来越多的消费设备，如高端智能手机、平板电脑、智能扬声器、可穿戴设备和生物植入物。它们还将用于许多企业市场:机器人、相机、传感器和其他物联网设备。

# 有什么弊端吗？

![](img/94fd4fee68c45732b975900cbb77bc00.png)

授权给作者的图像

复杂的机器学习模型通常体积很大，在某些情况下，将这些模型转移到小型设备上是不可行的。模型需要简化，这不可避免地会降低准确性。

边缘设备的计算能力有限，进一步限制了可以执行的人工智能任务。

Edge AI 通常涉及将模型部署到各种设备类型(和操作系统版本)，这可能会增加失败的可能性。因此，在芯片准备好流通之前，通常需要进行大量的测试。

# 后续步骤

1.向领先的人工智能芯片制造商 [ARM](https://www.arm.com/solutions/artificial-intelligence) 了解更多信息

2.了解更多关于 [Ancoris 数据、分析& AI](https://www.ancoris.com/solutions/data_analytics_ai)

3.与[作者](https://www.linkedin.com/in/google-cloud-platform/)联系