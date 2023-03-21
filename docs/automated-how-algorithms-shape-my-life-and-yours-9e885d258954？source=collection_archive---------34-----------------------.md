# 自动化:算法如何塑造你我的生活

> 原文：<https://towardsdatascience.com/automated-how-algorithms-shape-my-life-and-yours-9e885d258954?source=collection_archive---------34----------------------->

## 算法的目标不是给用户他们喜欢的内容，而是让用户更可预测。

![](img/80d9b3ea19b8a75b4c647cfe51a18e2c.png)

泰勒·拉斯托维奇摄于[佩克斯](https://www.pexels.com/photo/black-iphone-7-on-brown-table-699122/?utm_content=attributionCopyText&utm_medium=referral&utm_source=pexels)

*这原本是我关于机器人、自动化和人工智能的免费博客* [*上的专题。*](https://democraticrobots.substack.com/p/automated-how-algorithms-shape-a)

我认为，上周我对推荐系统的分析的逻辑后续([值得一读](https://democraticrobots.substack.com/p/recommendations-are-a-game-a-dangerous))将是我可以了解到的关于影响你我的算法的演练， ***在低层次上*** 。这是一项调查，包含一些公司使用的具体指标和我发现的主题的高级分析。大多数情况下，它会变成一篇完整的研究论文，这篇文章是我发现的文章的精华，如果你对某个特定的球员感兴趣，可以通过链接阅读更多内容。

在做这项研究时，算法互动的点比我预期的要多得多——我的意思是，算法可以以比我们预期的更多的方式接触我们和互动。有人应该研究人类大脑中算法竞争的游戏理论。抖音的算法如何帮助它留住 Youtube 可能吸引的用户？如果人们想弄清楚一个应用程序是如何工作的，希望这是一个起点。

如果你想从这篇文章中获得最大的价值，点击几个链接。

![](img/3c1eec2ba4cc8b0f100e53efad466be6.png)

来源——纽约伊萨卡的作者。

推荐系统和这些平台的一个总主题是:*如果我们可以预测用户想要做什么，那么我们就可以把这个特性加入到我们的系统中。*

**一般问题**的表述是:*复合互动如何改变我们的行为方式，以及如何损害我们的福祉。*

# 核心玩家

脸书、苹果、亚马逊、网飞和谷歌通常是最令人垂涎的公司，它们对世界人口有着巨大的影响。这篇文章并没有深入到这些公司的所有地缘政治和伦理问题，它只是试图展示它们是如何运作的。

## 脸谱网

该算法旨在向个人展示他们圈子中的事物，并让他们参与其中。这考虑了内容的来源、用户的地理位置、用户的社会地位、用户参与的历史、付费广告等等。该算法已经被研究得足够好，以至于意识到它具有戏剧性的效果，但对任何一个输入进行调节都是不可能的，因此通常被称为危险的**黑盒**。这种情况的二阶效应是，算法最终会预测你所在的圈子，并引导你进入其中部(如果你的圈子一开始是温和的，有时会被称为极化)。

*我断断续续地使用 Instagram，最大的影响可能是促使我尝试美化我的生活，但谢天谢地，我认为这种影响很小。*

【来源:[华尔街日报](https://www.wsj.com/articles/how-facebooks-master-algorithm-powers-the-social-network-1508673600)， [WSJ2](https://www.wsj.com/articles/facebook-knows-it-encourages-division-top-executives-nixed-solutions-11590507499) ， [Verge](https://www.theverge.com/2020/5/26/21270659/facebook-division-news-feed-algorithms) ，[脸书艾](https://www.facebook.com/business/news/good-questions-real-answers-how-does-facebook-use-machine-learning-to-deliver-ads/)

## 推特

该算法类似于脸书的算法，但更有活力——尤其是对于拥有大量受众的用户。从 Twitter 本身来说，它考虑了以下特性:

> *推文本身:推文的新近性、媒体卡(图片或视频)的存在、总互动(如转发或点赞的数量)*
> 
> *推文作者:你过去与该作者的互动，你与他们的联系强度，你们关系的起源*
> 
> *你:你发现过去参与过的推特，你使用推特的频率和频繁程度*

说实话，这个不多说。鉴于 Twitter 的怪癖，我可以看到他们有一个模糊的端到端优化器，没有太多的控制。

*我绝对沉迷于 Twitter 上的知识活力和对其他知识分子的访问。这些蓝色的通知泡泡让我。*

【来源:[萌芽社交](https://sproutsocial.com/insights/twitter-algorithm/)，[推特博客](https://blog.twitter.com/engineering/en_us/topics/insights/2017/using-deep-learning-at-scale-in-twitters-timelines.html)，[Hootsuite](https://blog.hootsuite.com/twitter-algorithm/)——一家社交媒体营销公司】

## 油管（国外视频网站）

细看一个神秘的娱乐平台。虽然由一家美国上市公司管理，但有多项研究显示了跟踪激进化途径的建议(见下文 NYT)，甚至显示了儿童令人不安的材料(纽约时报 2)。丰富的内容使得一系列的推荐具有难以置信的深度和影响力。既然[公司正在与抖音](https://www.theverge.com/2020/4/1/21203451/youtube-tiktok-competitor-shorts-music-google-report)竞争，我希望看到更多的公众见解。

*我在 Youtube 上消费了很多休闲内容。我认为观看乔·罗根的视频片段让我获得了一些特朗普的广告(是的，拜登的广告更多)，但它让我思考我正在观看的视频是如何漂移的。*

【来源: [NYT](https://www.nytimes.com/interactive/2019/06/08/technology/youtube-radical.html) ，[纽约时报](https://www.nytimes.com/2017/11/04/business/media/youtube-kids-paw-patrol.html)，[购物化](https://www.shopify.com/blog/youtube-algorithm)——是的他们分析 Youtube 算法？]

## 谷歌

作为第一个广泛有用的搜索引擎，每个人都使用谷歌。谷歌使用各种工具来索引网页(参见 [PageRank](https://en.wikipedia.org/wiki/PageRank) 作为起点),包括文本、到其他页面的链接、网站上读者的历史等等。它在信息传播方面的作用至关重要，我担心它会有无法追踪的偏见。

在反垄断听证会和竞争对手的背景下，他们的地位更加岌岌可危。近年来，谷歌改变了搜索结果，加入了更多的广告和自我参考结果(尤其是在旅游和购物等有利可图的搜索中)。这种自我参照在我看来是与谷歌竞争的角度。

*谷歌控制着我查找其他学术论文、博客、新闻等的方式。我不知道如何衡量超越大的影响。*

【来源:[《华尔街日报》](https://www.wsj.com/articles/how-google-interferes-with-its-search-algorithms-and-changes-your-results-11573823753)， [The Mark Up](https://themarkup.org/google-the-giant/2020/07/28/google-search-results-prioritize-google-products-over-competitors) ，[搜索引擎杂志](https://www.searchenginejournal.com/google-algorithm-history/)

![](img/167517f771f4f3ac394b6fc1be8958a5.png)

来源-作者。

# 未来的玩家

在这些领域，我看到算法以新的方式发挥作用，特别是以可能对一些人不利或有有害影响的方式。

## 新闻

传统媒体在线转移(《纽约时报》、《华尔街日报》等。)和新的在线出版物(媒体等)。)将圈住我们的政治和全球世界观。Clickbait 已经转变为阅读时间，但这从一个 clickbait 标题变成了一个 clickbait 标题+一个 readbait 介绍性段落。我在自己的中型文章中看到了这一点的影响——我的写作被调整到算法上(这很有效——教程、代码示例和列表比深入分析获得了更多的浏览量)。全世界的阅读人群正在 Substack 上重新崛起(还有其他平台——比如[上下文是最近的事件](https://nymag.com/intelligencer/2020/07/andrew-sullivan-see-you-next-friday.html))。

除了 clickbait,《纽约时报》开始在新闻平台上使用研究参与算法(具体来说，就是所谓的语境强盗— [研究中的语境](http://proceedings.mlr.press/v32/agarwalb14.pdf))。坦率地说，当我阅读新闻时，我想阅读带有次要补充意见的事件。我不想控制我接收国内和国际新闻的方式，但这是一个预期的发展。

*Medium 把我从 clickbait 逼到 Substack(这里)。我订阅了更多的时事通讯，如果趋势继续发展，我可能会退订《NYT》和《华尔街日报》。美国完全被特朗普毁掉的画面对我来说太狭隘了，没有给我足够广泛的教育——作为一个狂热的特朗普批评者。*

[来源: [NYT](https://open.nytimes.com/how-the-new-york-times-is-experimenting-with-recommendation-algorithms-562f78624d26)

## 购物

亚马逊希望成为购物的首选搜索引擎。他们正在扩展到许多不同的购物领域(如食品杂货、零售等)，以更好地了解个人需求。当你让 Alexa 提醒你一些事情时，那也可以被记录下来。这些构成了广告和销售的多模态推荐系统。

脸书创建了一个新的市场，试图对抗亚马逊的统治。脸书希望成为 Shopify(没有其他公司，Shopify 不是亚马逊的竞争对手)和供应商的中间人，创造一个人们可以购买任何东西的平台。在不同的激励(金融)下，看看这种情况如何发展会很有趣。

我已经采取措施减少使用亚马逊。它的推荐很奇怪，充满了重复的内容，但仍然非常方便。脸书市场的前景让我想到了更加自动化的客户-供应商连接。

【来源:】菜谱 [快递](https://www.repricerexpress.com/amazon-a10-algorithm-updates/)，[轴](https://www.axios.com/facebook-shops-online-marketplace-569242f4-da58-4f9b-8036-c21dca03a66b.html)，[销售](https://sellics.com/blog-amazon-seo/)

## 服务

新的应用程序将在许多领域提供价值:包括金融市场准入、客户分析、拼车等。首先，这些服务将较少受到公众监督，但时间会证明，它们中的许多将会产生极其有害的算法效果。我在未来的玩家中加入了服务，因为从采用到出现问题的时间尺度将会非常短(应用可以在用户中爆炸，[参见 Zoom](https://www.theverge.com/2020/4/23/21232401/zoom-300-million-users-growth-coronavirus-pandemic-security-privacy-concerns-response) 的例子)。负面指标通常落后于采用，除了在[最不经思考的服务可能永远](https://twitter.com/_alialkhatib/status/1288179135211114497)的例子中。

【来源:[加州大学伯克利分校金融科技](https://faculty.haas.berkeley.edu/morse/research/papers/discrim.pdf)，[性别化人工智能失败](https://twitter.com/_alialkhatib/status/1288179135211114497)，[零工经济](https://www.newscientist.com/article/2246202-uber-and-lyft-pricing-algorithms-charge-more-in-non-white-areas/#:~:text=The%20algorithms%20that%20ride%2Dhailing,to%20create%20a%20racial%20bias.&text=%E2%80%9CBasically%2C%20if%20you're,your%20ride%2C%E2%80%9D%20says%20Caliskan.)

# 二阶效应

这些是未讨论的当前应用程序，值得就可能的副作用(最初积极的应用程序)提出质疑。

## 消费设备

苹果手表试图为用户检测房颤，这带来了一个巨大的假阳性问题(见下面的 StatNews)。毫无疑问，技术公司将试图为他们的设备添加更多功能，随着数百万用户的市场渗透，不可避免的假阳性率将具有潜在的医疗和财务影响。这些设备和误导性数据点的复合效应将在 21 世纪 20 年代被观察到——不知情者面临的风险最大。

【来源:[苹果](https://machinelearning.apple.com/research/learning-with-privacy-at-scale)、 [StatNews](https://www.statnews.com/2019/01/08/apple-watch-iffy-atrial-fibrillation-detector/#:~:text=In%20people%20younger%20than%2055,79.4%20percent%20of%20the%20time.) 、 [Oura](https://ouraring.com/wvu-rockefeller-neuroscience-institute-and-oura-health-unveil-study-to-predict-the-outbreak-of-covid-19-in-healthcare-professionals)

## 公共医疗/卫生工具

在健康领域有许多新公司，比如将分析锻炼的 stirry . ai 和将分析血液测试以给你健康评分的 Bloodsmart.ai(基于标准化的、经年龄调整的死亡风险)。这是初步的，但是游戏化一个人的健康来了。在苹果的“填充戒指”手表活动之后，仅仅几步之遥就是健康仪表板，告诉用户他们的习惯如何在数字上缩短他们的寿命。

我现在不怎么使用追踪器了，但是仅仅是在我的手表上有一个估计卡路里的读数就让我意识到当一天不锻炼的时候吃得太多了。这个问题会因血液指标或情绪健康的嘈杂读数而加剧。

【来源:[奋斗](https://strive.ai/2020/03/keep-up-with-strive-ai/)，[血拼](https://bloodsmart.ai/)

# 这不是机器人博客吗？

我们在关于推荐器和算法的民主化自动化的最后两次迭代中所说的一切都将在未来十年适用于机器人系统。我预测，在大型科技公司(根据一些研究[提供的信息](https://democraticrobots.substack.com/p/10-years-of-automation-in-1-year))管理的机器人的激励下，机器人与人类互动的渗透率将大幅增加。

少数制造个人机器人的小公司可能看起来更令人兴奋(例如 [Hello Robotics](https://hello-robot.com/) )，但市场份额一开始会相对较小(高成本)，而许多公司可以用具体化的自主代理取代人类收银员和销售人员。

[我做了一个资源](https://github.com/natolambert/robot-ethics-books)追踪这些算法什么时候出错，还有一些哲学背景。

[](https://github.com/natolambert/robot-ethics-books) [## NATO Lambert/机器人-伦理-书籍

### 为对机器人伦理学感兴趣的人收集的读物。我从一系列机器故障开始…

github.com](https://github.com/natolambert/robot-ethics-books) [](https://robotic.substack.com/) [## 自动化大众化

### 一个关于机器人和人工智能的博客，让它们对每个人都有益，以及即将到来的自动化浪潮…

robotic.substack.com](https://robotic.substack.com/) 

像这样？请在 https://robotic.substack.com/订阅我关于机器人、自动化和人工智能的直接时事通讯。