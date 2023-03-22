# 你花哨的专有人工智能模型对我没有价值

> 原文：<https://towardsdatascience.com/your-fancy-proprietary-ai-model-has-no-value-to-me-2a7d40dfd8ca?source=collection_archive---------47----------------------->

## …除非您分享其目标、设计、测试和指标的细节

![](img/325c3eaf58db321701881739cc72f351.png)

图片由[皮克斯拜](https://pixabay.com/?utm_source=link-attribution&utm_medium=referral&utm_campaign=image&utm_content=5246634)的 Gerd Altmann 提供

各地的厂商，请不要告诉我你的机器学习(或者更好的是‘AI’)模型有 99%的准确率。不要告诉我，我们所需要做的就是‘插入你的数据’，然后一些闪亮的魔法就发生了。我不是唯一的一个。[最近，31 名科学家对谷歌健康的一项研究](https://www.nature.com/articles/s41586-020-2766-y.epdf?sharing_token=esGu0AMNcdnJWDaAaBr3BtRgN0jAjWel9jnR3ZoTv0M3qEeZVaYZS89U0ri_i4YI_lJ0an-lE15Ncv2e1v5F7I2jYVuc7_mR1WrnlDjpWJ6ANzSjO0KJiERmpzE097VzbFgywD7RRHVOYg305JycIUX7UQDWkLlgHtsoU72ppNzneeIGzsSR4Cr5dVRyVtV2UMxm2gs9pA22vydPt_RRBmPUqh6R2UPn-oziFm82hKA%3D&tracking_referrer=www.technologyreview.com)提出质疑，因为没有足够的公开证据来确定所提出的人工智能模型的可信度。我们都需要尽自己的一份力量来构建值得信赖的模型，为我们新的先进应用提供动力。被告知是减轻模型风险的第一步。

# 给我提供背景

当检查内部创建的模型或现成的供应商产品时，我会遵循一些必要的步骤。还有许多其他伟大的问题。请在评论区分享它们。

首先我们要明白，你在幕后做的是不是机器学习。如果你不能告诉我什么类型的模型产生的结果，让你的工程师打电话。

没有上下文，准确性度量是没有价值的。如果你正在为一个儿童动物学应用程序标记鱼的种类，百分之九点九的准确率是非常好的，但如果你正在识别来袭导弹，就没那么好了。

在私下讨论任何指标之前，需要检查以下三个问题。

**目标是什么？**

*   你是在促进什么还是阻止什么？

**事件发生的频率或罕见程度如何？**

*   给我数据。“非常罕见”不是一个好的回答。

**最坏的情况是什么？**

*   如果模型是错的会发生什么，包括假阳性和假阴性。
*   如果你不知道，你就不了解用例。你有工作要做。

# 给我提供证据和细节

一旦我理解了这些基础知识，我们就可以进入细节了。

**你从哪里得到的训练数据？**

*   如果购买，供应商是谁？
*   您是否在建模数据的基础上构建模型？
*   数据是如何收集和准备的？
*   交易、电话调查、网络应用、电话应用

**训练的模型类型:**

*   什么样的模型？
*   你是如何得到这些特别的模型的？
*   在做决定时，您是否依赖于特定的平台或架构？

**指标:**

*   准确是好的，但不仅仅是准确。请给我看看混淆矩阵。模型哪里做得好，哪里做得不好？
*   你能告诉我最好的预测吗？我需要验证它们是否“有意义”关联值是多少？有没有可能[数据泄露](https://en.wikipedia.org/wiki/Leakage_%28machine_learning%29)？

**偏置:**

*   你认为你的训练数据中存在哪些偏见(人口统计、地区、收集方法(iPhone 与 Android)、种族、健康和福利)？如果偏见是不可避免的，你做了什么来减轻它？
*   您是否通过分析模型实施的结果评估了可能的偏差影响？一个模型可以有绝对“干净”的数据，但仍然会产生意想不到的后果。性别、种族、民族和社会经济地位很容易泄露到数据库中，即使这些列没有被使用。如需详细示例，请阅读 Obermeyer 等人的“剖析用于管理人口健康的算法中的种族偏见”*SCIENCE*2019 年 10 月 25 日:447–453。它很好地详细说明了选择一个看似合理的优化目标(降低慢性病患者的医疗保健成本)如何导致黑人社区中有价值的健康计划候选人服务不足。

# 结论

随着机器学习和人工智能工作的快速发展，我们对模型设计和验证透明度的期望也应该快速发展。如果没有能力彻底理解模型及其产生的结果，对所有模型的不信任可能会出现，从而抑制增长和创造力。如果你还没有提问，请开始提问。我们所有的成功都有赖于此。

**参考文献**

[](https://www.technologyreview.com/2020/11/12/1011944/artificial-intelligence-replication-crisis-science-big-tech-google-deepmind-facebook-openai/?itm_source=parsely-api) [## 人工智能正在与复制危机搏斗

### 上个月,《自然》杂志发表了一篇由 31 名科学家撰写的对谷歌健康的一项研究的谴责性回应，该研究似乎…

www.technologyreview.com](https://www.technologyreview.com/2020/11/12/1011944/artificial-intelligence-replication-crisis-science-big-tech-google-deepmind-facebook-openai/?itm_source=parsely-api)  [## 人工智能中的透明性和再现性

### 订阅期刊获得为期 1 年的完整期刊访问权限 199.00 美元，每期仅 3.90 美元所有价格均为净价。增值税…

www.nature.com](https://www.nature.com/articles/s41586-020-2766-y.epdf?sharing_token=esGu0AMNcdnJWDaAaBr3BtRgN0jAjWel9jnR3ZoTv0M3qEeZVaYZS89U0ri_i4YI_lJ0an-lE15Ncv2e1v5F7I2jYVuc7_mR1WrnlDjpWJ6ANzSjO0KJiERmpzE097VzbFgywD7RRHVOYg305JycIUX7UQDWkLlgHtsoU72ppNzneeIGzsSR4Cr5dVRyVtV2UMxm2gs9pA22vydPt_RRBmPUqh6R2UPn-oziFm82hKA%3D&tracking_referrer=www.technologyreview.com) [](/how-you-can-prevent-systemic-racism-in-your-own-small-way-fc33cb57fdde) [## 你如何用自己的小方法防止系统性的种族歧视

### 这里有两种方法可以解决技术和数据科学中的偏见

towardsdatascience.com](/how-you-can-prevent-systemic-racism-in-your-own-small-way-fc33cb57fdde) 

“剖析用于管理人口健康的算法中的种族偏见”，作者 Obermeyer 等人*SCIENCE*2019 年 10 月 25 日:447–453