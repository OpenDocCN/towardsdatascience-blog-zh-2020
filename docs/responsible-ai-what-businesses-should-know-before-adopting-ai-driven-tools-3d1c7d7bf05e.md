# 负责任的人工智能:在采用人工智能驱动的工具之前，企业应该知道什么

> 原文：<https://towardsdatascience.com/responsible-ai-what-businesses-should-know-before-adopting-ai-driven-tools-3d1c7d7bf05e?source=collection_archive---------80----------------------->

## AI 会好到不真实吗？商业用户应该问的问题。

![](img/6cb4eaf46f9f09651f98e383c3991cf4.png)

照片由[张秀坤镰刀](https://unsplash.com/@drscythe?utm_source=medium&utm_medium=referral)在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄

过去，我们预计机器学习支持决策的唯一广泛使用的地方是科技公司。我们知道亚马逊已经使用人工智能来帮助筛选简历，网飞使用机器学习驱动的推荐系统来猜测你还想看什么节目。这种情况正在改变。

机器学习模型现在以自动贷款 assessments⁴的形式嵌入到我们的银行系统中，并通过生成风险分数来通知 sentencing⁵.，嵌入到我们的刑事司法系统中除了这些定制的模型，从 enforcement⁵法律到 retail⁶，更多的组织正在购买由人工智能默默驱动的软件。

随着这些工具被广泛采用，商业领袖需要向他们的软件提供商、数据科学家和机器学习专家提出正确的问题。

当设计、部署和集成不当时，机器学习工具可能会产生严重的后果。

## 严重后果？比如什么？

![](img/7f0ecdd49f8f1d3566e61d9b05c945bb.png)

照片由[杰森·德沃尔](https://unsplash.com/@jachan_devol?utm_source=medium&utm_medium=referral)在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 拍摄

**如果你的人工智能驱动工具的应用程序对人或社区进行评估或决策，歧视性偏见**就会发生。在你没有意识到的情况下，机器学习算法可以从训练数据中学习偏差。在过去，这导致恢复了对妇女有偏见的筛选工具或犯罪预测模型，导致对 communities⁵.族裔的过度监管

即使出于最好的意图，这种情况也会发生。你是一所试图利用机器学习更好地瞄准招聘活动的大学吗？您是否是一家利用大数据来识别需要资助的社区的非营利组织？如果历史对某一特定人群有偏见，那么你用来训练模型的数据就反映了这种偏见的结果。如果你不小心，你可能会使历史偏见永久化，即使是出于最好的意图。

**概念漂移**发生在支撑你的机器模型的条件发生变化，导致你的预测不再准确的时候。我们工作的世界瞬息万变，人类会根据这些变化的条件来调整我们的决策。如果我是一名企业校园招聘人员，我可以根据我对大学课程变化的了解来调整我的简历筛选标准，这些变化会导致不同的 GPA 分布。

受过训练的模型会忽略这些差异。随着时间的推移，模型被定型时的世界和模型进行预测时的世界之间的差异会扩大。在最好的情况下，业务用户会注意到差异，并相应地进行调整。最坏的情况是，一家企业可能会因为将核心功能委托给过时的模式而失去竞争力。

**每个模型(或模型集)都有其局限性**，因为它们在某些情况下高度准确，而在其他情况下可能是随机猜测。通常，当有大量训练数据可供学习模式时，模型表现良好，而在与之前看到的差异太大的示例中，模型表现不佳。

不知道在您的环境中，一个模型在什么情况下工作得好，什么情况下工作得差，意味着您不能构建考虑其缺点的业务流程。例如，如果我们知道某个算法在我们从未招聘过的大学可能表现不佳，那么让我们手动筛选来自新大学的候选人。

## 作为企业用户，我可以做些什么来准备我的组织？

![](img/9776cad231bcf52e643f44faef11c317.png)

由[拍摄](https://unsplash.com/@headwayio?utm_source=medium&utm_medium=referral)在[上前进](https://unsplash.com?utm_source=medium&utm_medium=referral)

在采用人工智能驱动的工具之前，请与您的人工智能专家进行讨论。

1.  **询问模型或工具在准确性之外的性能指标。**有时候，你会关心不同群体的准确性，比如少数民族是否得到与多数民族相似的预测。在其他情况下，您可能不太关心准确性，而更关心我们没有遗漏任何潜在的案例(回想一下)。
2.  **为模型部署后的监控制定一个计划，并确定重新评估和潜在重新训练模型的触发因素。**情况会发生变化，除非您积极监控情况，否则您和您的组织可能不会注意到这一点。确定指标以监控基础人群的分布。实现反馈机制来监控模型的准确性(或其他性能指标)。定期检查这些反馈，并积极主动地维护您的模型。
3.  了解你的模型的局限性，并在周围业务流程的设计中解决它们。在不同条件下对你的模型进行压力测试。你的模型表现如何？例如，如果它是一个影响客户的工具，在这些情况下的性能是否可以接受？如果没有，通过在需要的地方进行人工干预来计划这些限制。

Monique 是一名前管理顾问，也是一名训练有素的数据科学家。你可以在 medium.com/@monique_wong 的[查看她关于数据科学和商业的其他文章](https://medium.com/@monique_wong)

[1]杰弗里·达斯廷。(2018 年 10 月 9 日)。[https://www . Reuters . com/article/us-Amazon-com-jobs-automation-insight/Amazon-scraps-secret-ai-recruiting-tool-show-bias-against-women-iduskcn 1 MK 08g](https://www.reuters.com/article/us-amazon-com-jobs-automation-insight/amazon-scraps-secret-ai-recruiting-tool-that-showed-bias-against-women-idUSKCN1MK08G)

[2]利比·普卢默。(2017 年 8 月 22 日)。[https://www . wired . co . uk/article/how-do-Netflix-algorithms-work-machine-learning-helps-to-predict-what-viewers-will-like](https://www.wired.co.uk/article/how-do-netflixs-algorithms-work-machine-learning-helps-to-predict-what-viewers-will-like)

[3]詹姆斯·文森特。(2018 年 1 月 12 日)。[https://www . the verge . com/2018/1/12/16882408/Google-种族主义者-大猩猩-照片-识别-算法-ai](https://www.theverge.com/2018/1/12/16882408/google-racist-gorillas-photo-recognition-algorithm-ai)

4 凯瑟琳·瓦尔希。(2019 年 7 月 26 日)。[https://www . Forbes . com/sites/Douglas Merrill/2019/04/04/ai-is-coming-to-take-your-mortgage-disease/# 195 ee2b 87567](https://www.forbes.com/sites/douglasmerrill/2019/04/04/ai-is-coming-to-take-your-mortgage-woes-away/#195ee2b87567)

[5]郝凯伦。(2019 年 1 月 21 日)。[https://www . technology review . com/2019/01/21/137783/algorithms-criminal-justice-ai/](https://www.technologyreview.com/2019/01/21/137783/algorithms-criminal-justice-ai/)

【6】AI 创业公司。(2020 年 3 月 12 日)。[https://www.ai-startups.org/top/retail/](https://www.ai-startups.org/top/retail/)