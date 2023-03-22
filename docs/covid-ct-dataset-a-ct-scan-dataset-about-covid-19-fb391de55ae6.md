# 关于新冠肺炎的 CT 扫描数据集

> 原文：<https://towardsdatascience.com/covid-ct-dataset-a-ct-scan-dataset-about-covid-19-fb391de55ae6?source=collection_archive---------22----------------------->

## 促进人工智能研究使用 CTs 对抗新冠肺炎病毒

(数据可在[https://github.com/UCSD-AI4H/COVID-CT](https://github.com/UCSD-AI4H/COVID-CT)获得)

截至 2020 年 3 月 30 日，冠状病毒疾病 2019(新冠肺炎)已影响到全球 775，306 人，并导致 37，083 人死亡。控制这种疾病传播的一个主要障碍是检测的低效和短缺。目前的测试大多基于逆转录聚合酶链反应(RT-PCR)。需要 4-6 小时才能得到结果，与新冠肺炎的快速传播速度相比，这是一段很长的时间。除了效率低下，RT-聚合酶链式反应测试试剂盒严重短缺。

这促使我们研究替代的检测方式，这可能比 RT-PCR 更快、更便宜、更有效，但与 RT-PCR 一样准确。我们尤其对 CT 扫描感兴趣。已经有几项研究 CT 扫描在筛查和检测新冠肺炎中的有效性的工作，并且结果是有希望的。然而，出于隐私考虑，这些作品中使用的 CT 扫描并不与公众分享。这极大地阻碍了更先进的人工智能方法的研究和发展，以更准确地测试基于 CT 的新冠肺炎。

为了解决这个问题，我们建立了一个 COVID-CT 数据集，其中包含 275 次新冠肺炎阳性的 CT 扫描，并对公众开放源代码，以促进新冠肺炎基于 CT 的检测的 R&D。从 760 份关于新冠肺炎的 medRxiv 和 bioRxiv 预印本中，我们提取报道的 ct 图像，并通过阅读这些图像的标题来手动选择那些包含新冠肺炎临床发现的图像。下图显示了数据集中的一些示例。

![](img/f025a9454efd23091da429e41fd7d496.png)

我们数据集中的新冠肺炎联系类型示例。

我们在 183 个 COVID ct 和 146 个非 COVID CT 上训练了一个深度学习模型，以预测 CT 图像是否对新冠肺炎呈阳性。我们的模型在 35 台 COVID CT 和 34 台非 COVID CT 上进行了测试，F1 得分为 0.85。结果表明，CT 扫描是有希望的筛查和测试新冠肺炎，但需要更先进的方法来进一步提高准确性。

数据和代码可在 https://github.com/UCSD-AI4H/COVID-CT[获得](https://github.com/UCSD-AI4H/COVID-CT)

更多详情请参考[https://github . com/UCSD-AI4H/COVID-CT/blob/master/COVID-CT-dataset . pdf](https://github.com/UCSD-AI4H/COVID-CT/blob/master/covid-ct-dataset.pdf)