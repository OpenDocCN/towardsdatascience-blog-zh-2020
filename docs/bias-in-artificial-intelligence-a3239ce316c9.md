# 人工智能中的偏见

> 原文：<https://towardsdatascience.com/bias-in-artificial-intelligence-a3239ce316c9?source=collection_archive---------29----------------------->

![](img/8f25eaddfef30cc27aa7e79bc95e4b51.png)

## 大数据时代的不平等、种族主义和歧视

人工智能和机器学习都很牛逼。它们让我们的手机助手能够理解我们的声音，并为我们预订优步。人工智能和机器学习系统在亚马逊为我们推荐书籍，类似于我们过去喜欢的书籍。他们甚至可能让我们在约会应用程序中有一个惊人的匹配，并遇到我们生命中的爱。

所有这些都是人工智能的酷但潜在无害的应用:如果你的语音助手不理解你，你可以打开优步应用程序，自己订购一辆汽车。如果亚马逊向你推荐一本你可能不喜欢的书，一点点研究就能让你抛弃它。如果一个应用程序带你去和一个不适合你的人相亲，你甚至可能会和一个性格令人困惑的人度过一段美好的时光。

然而，当人工智能被用于更严重的任务时，如过滤求职者，发放贷款，接受或拒绝保险请求，甚至用于医疗诊断，事情就变得很糟糕。所有以前的决定，部分由人工智能系统辅助或完全由人工智能系统处理，可以对某人的生活产生巨大的影响。

对于这些类型的任务，必须有争议地研究输入机器学习系统的数据，这些系统是人工智能应用的核心，试图避免使用信息代理:用来替代另一个对某项任务来说更合法和精确但不可用的数据的数据片段。

让我们来看看由机器学习系统自动处理的汽车保险请求的例子:如果在模型中使用邮政编码而不是纯粹的驾驶和支付指标作为变量，生活在贫困和不受重视地区的优秀司机可能会拒绝汽车保险请求。

除了这些代理之外，人工智能系统还依赖于以另一种方式训练它们的数据:在人口的非代表性样本中进行训练，或者对被贴上某种偏见标签的数据进行训练，都会在最终的系统中产生相同的偏见。

我们来看一些源于 AI 的偏差的例子。

# Tay:令人不快的推特机器人

![](img/3be18daca15068c2d00822e0e235c817.png)

tay(*Thinking you*)是一款 Twitter 人工智能聊天机器人，旨在模仿一名 19 岁美国女孩的语言模式。它是由微软在 2016 年开发的，用户名为 [TayandYou](https://twitter.com/TayandYou) ，被放在平台上的目的是与其他用户进行对话，甚至从互联网上传图像和迷因。

在 16 个小时和 96000 条推文之后，它不得不被关闭，因为它开始发布煽动性和攻击性的推文，尽管已经被硬编码为避免某些主题的列表。因为机器人从它的对话中学习，当与之互动的用户开始在推特上发布政治上不正确的短语时，机器人学习这些模式，并开始发布关于某些主题的冲突消息。

机器学习系统从它们看到的东西中学习，在这种情况下，Tay 采用的这种鹦鹉学舌的行为给微软带来了巨大的公开耻辱，以[这封信](https://blogs.microsoft.com/blog/2016/03/25/learning-tays-introduction/#sm.00000gjdpwwcfcus11t6oo6dw79gw)结束，因为他们的 19 岁女孩变成了新纳粹千禧聊天机器人。

在下面的[链接](https://gizmodo.com/here-are-the-microsoft-twitter-bot-s-craziest-racist-ra-1766820160)中，你可以找到一些 Tay 推文的例子。

[](https://gizmodo.com/here-are-the-microsoft-twitter-bot-s-craziest-racist-ra-1766820160) [## 以下是微软推特机器人最疯狂的种族主义咆哮

### 昨天，微软发布了 Tay，这是一个青少年对话的人工智能聊天机器人，旨在模仿用户并与用户实时交谈…

gizmodo.com](https://gizmodo.com/here-are-the-microsoft-twitter-bot-s-craziest-racist-ra-1766820160) 

现在，想象一下，如果像这样的聊天机器人不是用于社交网络，而是被用作虚拟心理学家或类似的东西。或者想象一下，机器人开始针对社交媒体中的特定人群进行攻击。对它说话的人可能会受重伤。

# 谷歌的种族主义图片应用

![](img/37bffd8fcb3adc86b92a82cffab33b48.png)

另一家大型科技公司，这次是谷歌，也有一些关于偏见和种族主义的问题。2015 年，谷歌照片中谷歌图像识别的一些用户收到了应用程序将黑人识别为大猩猩的结果。谷歌为此道歉，并表示图像识别技术仍处于早期阶段，但他们会解决这个问题。你可以在下面的[链接](https://www.bbc.com/news/technology-33347866)中读到所有相关内容。

[](https://www.theverge.com/2018/1/12/16882408/google-racist-gorillas-photo-recognition-algorithm-ai) [## 谷歌通过从其图像标签技术中移除大猩猩来“修复”其种族主义算法

### 早在 2015 年，软件工程师 Jacky Alciné指出，谷歌照片中的图像识别算法是…

www.theverge.com](https://www.theverge.com/2018/1/12/16882408/google-racist-gorillas-photo-recognition-algorithm-ai) 

如果像谷歌这样强大、技术先进的公司会有这种问题，想象一下成千上万的其他企业在没有这种专业知识的情况下创建人工智能软件和应用程序。这很好地提醒了我们，训练人工智能软件保持一致性和健壮性是多么困难。

然而，这不是谷歌在图像和人工智能方面的唯一问题。手持温度计枪已经在整个 COVID 疫情广泛使用，[谷歌的云视觉软件](https://cloud.google.com/vision/?utm_source=google&utm_medium=cpc&utm_campaign=emea-es-all-en-dr-bkws-all-all-trial-e-gcp-1009139&utm_content=text-ad-none-any-DEV_c-CRE_253509621337-ADGP_Hybrid+%7C+AW+SEM+%7C+BKWS+~+EXA_M:1_ES_EN_ML_Vision+API_google+cloud+vision-KWID_43700053285283956-aud-606988878214:kwd-203288730967-userloc_9061036&utm_term=KW_google%20cloud%20vision-NET_g-PLAC_&ds_rl=1242853&ds_rl=1245734&ds_rl=1242853&ds_rl=1245734&gclid=CjwKCAjwxqX4BRBhEiwAYtJX7RCaQCZSKKUo67nk_ramNO2h90lMPveZOVc8hpnfjOupLemNxrTqChoCaQAQAvD_BwE)(一种用于检测和分类图像中物体的服务)必须快速学习识别这种设备，以便使用包含很少图像的数据集对它们进行正确分类，因为这些设备尽管不是新设备，但最近才为公众所知。

![](img/6b16504cef00d26b3cb88e358507a56e.png)

图片来自[来源](https://twitter.com/nicolaskb/status/1244921742486917120)。

上一张图片显示了当一个黑皮肤的人拿着一支温度计枪时，它是如何被归类为枪的，而当一个浅棕色皮肤的人拿着它时，它又是如何被归类为单筒望远镜的。谷歌产品战略和运营总监特雷西·弗雷(Tracy Frey)在这起病毒事件后写道:

> “这个结果是不可接受的。认识到这一结果与种族主义之间的联系很重要，我们对这可能造成的任何伤害深感抱歉。”

谷歌解决这一问题的方式是改变云视觉返回枪支或火器所需的置信概率(上图中出现的 61%)，然而，这只是人工智能模型结果显示的变化，而不是模型本身，这再次凸显了在许多情况下让这些系统正常运行的困难，特别是在数据很少的情况下。

如果像这样的系统被用来在街上使用监控摄像头定位潜在的有害或可疑的个人，会怎么样？无辜的人可能仅仅因为他们的肤色而成为危险的目标。

# 最新有偏见的 AI 新闻:

最近，世界上一些顶级人工智能研究人员之间围绕人工智能偏见的话题进行了很多讨论，这些讨论源于论文 [*“脉冲:通过生成模型的潜在空间探索进行自我监督的照片上采样”*](https://arxiv.org/abs/2003.03808) *。这个模型使用人工智能将低分辨率图像转换为更高分辨率的图像，如下面的推文所示。*

这条推文附有一个谷歌 Colab 笔记本(一个编程环境)的链接，任何人都可以在那里运行代码，并使用不同的图像尝试该模型。这很快导致人们发现 PULSE 似乎偏向于输出白人的图像，让一个具体的用户用巴拉克·奥巴马的像素化图像来响应前一个用户，这被重建成一个白人的图像。

这篇论文的作者对此做出了回应，在论文中增加了一个偏见部分，并包括一个模型卡:一个阐明模型细节的文件，它的目的，用于评估它的指标，训练它的数据，以及不同种族的结果分类，以及一些道德考虑。我认为在构建机器学习模型时创建这种文档是一种很好的实践，应该更频繁地进行。

您可以在下面的链接中找到关于这个主题的讨论和更多信息。

[](https://thegradient.pub/pulse-lessons/?utm_campaign=The%20Batch&utm_medium=email&_hsmi=91009266&_hsenc=p2ANqtz-_IIn_r2KtiZQpf3OGgusrmLPCXxkBLdSy-fSEayQeQKgQ-srfey1xs-kuatrzTwvSN-1SZJhWaASQg0Jd0hBiSatpwsw&utm_content=91009266&utm_source=hs_email) [## 脉冲模型的教训和讨论

### 在这篇文章中，我将试图总结最近在人工智能研究者中发生的讨论的一些内容，作为…

thegradient.pub](https://thegradient.pub/pulse-lessons/?utm_campaign=The%20Batch&utm_medium=email&_hsmi=91009266&_hsenc=p2ANqtz-_IIn_r2KtiZQpf3OGgusrmLPCXxkBLdSy-fSEayQeQKgQ-srfey1xs-kuatrzTwvSN-1SZJhWaASQg0Jd0hBiSatpwsw&utm_content=91009266&utm_source=hs_email) 

# 人工智能偏见的其他例子

除了这些在媒体上引起共鸣的案例之外，还有许多其他不太为人所知的案例，这些案例中的模特也有类似的歧视味道。可以为每一个写一节，但是这里我们将简要地提及它们，如果需要的话，允许读者进一步研究。

*   与男性相比，女性不太可能在谷歌上看到高薪工作的广告。为显示这些而建立的模型增加了个人信息、浏览历史和互联网活动等二手信息。 [***链接***](https://www.theguardian.com/technology/2015/jul/08/women-less-likely-ads-high-paid-jobs-google-study) 。
*   **算法陪审团:使用人工智能预测累犯率** s .一个用于预测个人在被释放后是否会再次犯罪的预测模型(因此用于延长或减少个人的监禁时间)显示了种族偏见，对黑人比对白人更严厉。 [***链接***](http://www.yalescientific.org/2020/05/an-algorithmic-jury-using-artificial-intelligence-to-predict-recidivism-rates/) 。
*   **优步的 Greyball:逃避世界各地的当局:**从优步应用程序收集的数据被用来逃避当地当局，他们试图在法律不允许该服务的国家取缔他们的骑手。这本身并不是偏见的例子，但它将重点放在人工智能可以做什么来歧视某些用户(在这种情况下是警察)，以及它如何被用于自私的利益。 [***链接***](https://www.nytimes.com/2017/03/03/technology/uber-greyball-program-evade-authorities.html) 。
*   最后，对人工智能来说，并不是所有的都是坏消息。以下链接展示了人工智能系统如何**减少大学招聘**应用中的偏见: [***链接***](https://www.fastcompany.com/90342596/schools-are-quietly-turning-to-ai-to-help-pick-who-gets-in-what-could-go-wrong) ***。***

# 对于这一切，我们能做些什么？

我们已经看到了如果人工智能系统开始显示种族、性别或任何其他类型的偏见会发生什么，但是，我们能做些什么呢？

为了规范这些数学模型，第一步必须从建模者本身开始。当创建这些模型时，设计者应该尽量避免使用过于复杂的数学工具，以免混淆模型的简单性和可解释性。他们应该非常仔细地研究用于构建这些模型的数据，并尽量避免使用危险的代理。

此外，他们应该始终考虑模型的最终目标:让人们的生活更轻松，为社区提供价值，提高我们的整体生活质量，通过商业或学术界，而不是专注于机器学习指标，如准确性或均方差。此外，如果模型是为一个特定的业务建立的，另一个通常的成功标准可能必须放在第二个平面上:经济利润。除了这一利润之外，模型在决策方面的结果也应该被检查:按照我们的保险例子，模型的创建者应该看看谁被拒绝，并试图理解为什么。

随着我们进入一个更加数据驱动的世界，政府可能需要介入，为人工智能模型在金融、保险、医药和教育等特定领域的使用提供公平透明的监管。所有这些都是任何个人生活的基本部分，应该非常小心地对待。

作为人工智能从业者，创建系统的人有责任重新审视他们收集和使用数据的方式。最近的提议为记录[模型](https://arxiv.org/abs/1810.03993?utm_campaign=The%20Batch&utm_medium=email&_hsmi=91009266&_hsenc=p2ANqtz-8OD0_NHO30-TpMgvsHrmFxugShXWUEsysFir15-Akhh-Fi4Mh9ptkp-PQyzBIReL1pk6f850_pZRhVA7R5iR96Q6kQRA&utm_content=91009266&utm_source=hs_email)和[数据集](https://arxiv.org/abs/1803.09010?utm_campaign=The%20Batch&utm_medium=email&_hsmi=91009266&_hsenc=p2ANqtz-8sluC1yD_1T2RlnZVoMlfTb4r-DgOk9gbyrBFjzOKDM_qO8k5HTvniyl_tK8HJnO-rz7ZTtdJcl9nAoh-iMXsm-Yi9ZA&utm_content=91009266&utm_source=hs_email)设定了标准，以在有害偏见扎根之前将其剔除，使用之前提到的模型卡和类似的数据集系统:数据表。

除此之外，我们应该尝试建立非黑盒的、可解释的模型，审计这些模型，并仔细跟踪它们的结果，花时间手工分析一些结果。

最后，我们可以教育更广泛的社区和公众如何使用数据，可以用它做什么，它会如何影响他们，并让他们透明地知道他们何时被人工智能模型评估。

# 结论和额外资源

就是这样！一如既往，我希望你**喜欢这篇文章**，并且我设法帮助你了解更多关于人工智能中的偏见，它的原因，影响，以及我们如何与之斗争。

如果您想了解有关该主题的更多信息，您可以在这里找到一些附加资源:

*   解决人工智能和自动化中的性别偏见。
*   [AI 注定是种族主义者和性别歧视者吗？](https://uxdesign.cc/is-ai-doomed-to-be-racist-and-sexist-97ee4024e39d)
*   [人工智能和偏见——IBM 研究](https://www.research.ibm.com/5-in-5/ai-and-bias/#:~:text=Biases%20find%20their%20way%20into,humans%20and%20machines%20that%20learn.)
*   [数学毁灭的武器:大数据如何增加不平等并威胁民主，作者凯茜·奥尼尔。](https://amzn.to/2Wf6IpG)

*如果你想了解更多关于机器学习和人工智能的知识* [***关注我上媒***](https://medium.com/@jaimezornoza) *，敬请关注我的下期帖子！另外，你可以查看* [***这个资源库***](https://howtolearnmachinelearning.com/) *来获得更多关于机器学习和人工智能的资源！*

*   封面图片来自[](https://unsplash.com/)*。*
*   *图标来自 [***平面图标***](https://www.flaticon.com/) 。*
*   *所有其他图像都是自己制作的。*