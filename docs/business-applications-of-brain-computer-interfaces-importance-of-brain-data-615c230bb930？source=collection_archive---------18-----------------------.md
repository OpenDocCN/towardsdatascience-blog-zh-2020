# 脑-机接口的商业应用&脑数据的重要性

> 原文：<https://towardsdatascience.com/business-applications-of-brain-computer-interfaces-importance-of-brain-data-615c230bb930?source=collection_archive---------18----------------------->

## 非侵入性商用脑机接口和脑数据的挑战

![](img/7c9c93991a05479dcc5381dc2ae0eeee.png)

Bret Kavanaugh 在 [Unsplash](https://unsplash.com/s/photos/brain?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) 上拍摄的照片

人们经常想知道使用脑机接口能实现什么(BCI)。通过这篇文章，我觉得我可以帮助人们从商业角度更多地了解这项技术，这要归功于我的经验。事实上，在过去的六个月里，我一直在为运动员设计一款非侵入式商业 BCI。

在本文中，我将介绍主要的非侵入性和非医疗商业 BCI 应用，介绍大脑数据在新商业模式中的战略作用，提到该领域的领先公司，并分享我构建 BCI 原型的经验。

# **有创与无创脑机接口**

在开始之前，我必须解释一些关于 BCI 的关键因素。

脑机接口被设计用来读取你的大脑电信号(思想、感觉等等)——有时通过脑电图(存在不同的方法)。

> **脑电图(EEG):** 使用附着在头皮上的小电极检测你大脑中的电活动的监测方法。这项活动在**脑电图**记录中显示为波浪线。( [1](https://www.mayoclinic.org/tests-procedures/eeg/about/pac-20393875)

![](img/9011de296d06e12802d0bcbfa1300a18.png)

[来源](https://commons.wikimedia.org/wiki/File:3112189_pone.0020674.g001.png)

## 非侵入性和侵入性 BCIs 的主要区别。

侵入性 BCI“需要通过手术在头皮下植入电极来传递大脑信号”( [2](https://en.wikipedia.org/wiki/Brain%E2%80%93computer_interface) )。使用侵入式脑机接口，我们通常会得到更准确的结果。

然而，你可能会因手术而遭受副作用。不幸的是，手术后，可能会形成疤痕组织，使大脑信号变弱。而且，身体“可以排斥植入的[电极](https://www.sciencedirect.com/science/article/pii/S1110866515000237?via%3Dihub)”([3](https://www.sciencedirect.com/science/article/pii/S1110866515000237?via%3Dihub))。

> 最著名的入侵 BCI 可能是来自 [Neuralink](https://www.neuralink.com/) 的那个。

**部分侵入性 BCI** 装置也存在。通常，它们被植入颅骨内，但留在大脑外。从技术角度来看，“它们比非侵入性脑机接口产生更准确的结果，并且比侵入性脑机接口风险更低”( [4](https://en.wikipedia.org/wiki/Brain%E2%80%93computer_interface) )。

> 迄今为止，公众可用的大多数 BCI 应用程序都是非侵入性的。

# 为什么要开发商业脑机接口

首先，你要知道 BCI 并不是什么新鲜事物。事实上，科学家们从 20 世纪 70 年代[到现在](https://en.wikipedia.org/wiki/Brain%E2%80%93computer_interface)一直在为医学目的研究脑机接口。由于医疗领域的进步，这项技术“最近”进入了消费领域。

其他技术(人工智能、虚拟现实……)的发展也使公司对商业非侵入性脑机接口的研究具有战略意义(超越了显而易见的医疗领域)。因此，越来越多的初创公司和大型科技公司正试图开发非侵入性脑机接口。

BCIs 被视为战略性的原因各不相同。首先，**它们代表了潜在的“下一个硬件接口”** ( [5](https://www.zdnet.com/article/what-is-bci-everything-you-need-to-know-about-brain-computer-interfaces-and-the-future-of-mind-reading-computers/) )。

## **硬件接口**

我们控制设备的方式即将迎来一场重大变革。在未来几年，用我们的大脑控制设备将成为常态。你可以想象，对于大型科技公司来说，开始准备他们的产品生态系统以适应未来的现实是很关键的。

## 与人工智能的协同

另一个战略要素是将人工智能和 BCI 结合起来，并借助它们创造独特的体验。人工智能已经在一些商业脑机接口中使用。机器学习(ML)可以用于实时分析和分类脑电波。这在“试图测量用户意图”( [6](https://www.sciencedirect.com/science/article/pii/S1110866515000237) )时非常有用。

ML 还有助于“将具有高度可变性和非平稳噪声的脑电活动解码成有意义的信号”( [7](https://jinglescode.github.io/2020/01/17/deep-learning-bci-intro/) )。**在构建 BCI 应用程序时，ML 已经被证明是有用的。**

此外，“EEG 数据集是高维的，具有大量参数的深度学习模型在直接学习生鸡蛋信号的背景下是有趣的”( [8](http://try.socialax.co/wjjuy22/eeg-deep-learning-github.html) )。

结合人工智能和 BCI 可以创造新的机会，让用户体验全新的东西(竞争优势)。例如，BCIs 可以用来利用观众的大脑活动和生成敌对网络来创建新内容。

## 战略市场

**对于开发 BCI 解决方案的公司来说，一些市场可以迅速盈利。例如，营销/广告领域可以产生大量收入。**甚至尽管目前还没有为营销目的而设计的设备(如果我错了，请写信给我)或应用程序，但研究表明，BCIs 将与它们一起使用。

事实上，一些研究已经指出，BCIs 可以用来评估商业广告产生的[注意力水平](https://ieeexplore.ieee.org/abstract/document/5335045/)。**可以肯定的是，许多公司都有兴趣为营销专业人士开发一种测量这一关键 KPI 的解决方案。**

## 增强人类

脑机接口也具有战略意义，因为我们可以收集与人体相关的数据。这种收集战略数据的能力非常重要，因为我们已经进入了“人类扩增”的时代。

> **增强人类:** **指提高人类生产力或能力，或以某种方式增加人体的技术。(**[**9**](https://www.techopedia.com/definition/29306/human-augmentation)**)**

BCIs 将完善或取代智能手表等现有智能设备，并最终帮助我们实现更多目标。在接下来的几年里，我希望看到越来越多的初创公司参与构建能够分析用户情绪、帮助改善和集中注意力等的 BCI。

> 我们的项目:我目前正在从事一个项目，通过对大脑数据的分析，帮助运动员提高他们的身体表现。我们使用计算机视觉将这些数据与传感器和过去的表现结合起来。目标是更好地理解运动员对某些情况的反应，并帮助他们更好地管理他们的压力。经过几次测试后，我们已经开始研究一种非侵入式 BCI，它将帮助个人更好地了解他们的运动表现。

## 走向侵袭性脑机接口

我还认为，公司也有兴趣开发非侵入式 BCI 解决方案，将他们的品牌与这种技术联系起来，并迅速与客户建立信任。一旦入侵性 BCIs 成为主流，这种信任将成为战略。

## 大脑数据

也许公司投资 BCIs 的最大原因是大脑数据。**如你所知，用 BCIs 监测大脑活动会产生大量信息**。

的确，你的大脑会产生一种独特的脑电波模式，给你自己的个人神经指纹。你对一些视觉元素的反应方式，你的睡眠和注意力数据等。所有这些都可以被捕获并出售给其他公司。

> 谁拥有大脑数据，它的用途是什么？

**拥有最多大脑数据的公司可能会比其他公司拥有显著的竞争优势**。想象一下数据网络效应的影响，但应用于 BCI 和大脑数据…

![](img/58edffa1e382bcb4e0b8e6ed72edfa4c.png)

此外，获得某人的大脑数据可以加强锁定效应。例如，你的大脑数据可以被记录并用于个人识别系统。这个简单的“功能”可以加强您与 BCI 制造商的关系。

> 谁拥有大脑数据，它的用途是什么？

# 为商业目的构建 BCIs 的挑战

您可能知道，商业非侵入性 BCI 市场仍处于起步阶段。出于这个原因，公司仍在努力寻找最佳的方法和方法论。

**在我看来，我们还需要 2 到 3 年才能看到 BCIs 变得更加主流。**根据一些行业专家的说法，主要的挑战是“找到仍能从 BCIs 早期迭代中受益的中间市场”( [10](https://venturelabs.ca/earbuds-the-next-brain-computer-interface-a-venturelabs-interview-with-kristina-pearkes-cto-and-cofounder-at-orbityl/) )。

> 创建易于使用、可访问、直观、安全且具有良好准确性的脑机接口仍然是一个重大挑战。

## 设备

这种设备本身就是一个重大挑战(昂贵、难以操作、可能很重、不够谨慎等等)。一些公司正在探索开发与监控脑电活动的耳塞耳机整合的传感器的想法。此外，每个人都有大量的校准工作要做...我们距离像智能手机一样直观的设备还很远。

## 展示有用性

在一些使用案例中，很难说服客户购买 BCI。人们真的会关心用戴着特定设备的大脑来控制设备吗？答案不是那么明显。

例如，在灯光控制的情况下，你也可以只使用遥控器。我希望看到越来越多的初创公司转向寻找最佳用例。由于需要大量的技术开发和资金，这种必要性对 BCI 的初创企业来说是一个重大风险。

> 也很难为 BCI 解决方案找到合适的价格。对 BCI 公司来说，在生产成本和零售价格之间找到合适的平衡点并非易事。

## 绕轴旋转

几家 BCI 公司已经考虑转向(从脑机接口转向可穿戴设备)。事实上，睡眠监测等一些商业应用已经代表了一个拥挤的市场。从短期来看，为 BCI 玩家找到相关且独特的中介应用至关重要。

## **脑电图**

BCI 解决方案的另一个值得一提的潜在问题与脑电图数据有关。对于非侵入性脑机接口，“电极和大脑之间的头骨和组织导致信号更弱，信息传递更慢”( [11](https://www.ncbi.nlm.nih.gov/pmc/articles/PMC3304110/) )。显然，这限制了用户控制设备的能力。研究人员正试图克服这些限制，以提高脑电图在医疗和非医疗应用中的效用。

在某些情况下，用户基本上必须进入“冥想状态以实现对大脑调制的控制”( [12](https://www.ncbi.nlm.nih.gov/pmc/articles/PMC4465229/) )，这可以允许控制设备。最后，目前的非侵入式脑机接口往往需要强化训练。

## 生态系统

就像 AIoT 战略一样，我认为 BCI 公司面临的主要挑战之一将是整合由其他公司的解决方案组成的生态系统。如果明天，大型科技公司决定建立只适用于某些产品的 BCI，创业公司将很难与之竞争。

> 最终，BCIs 可以强化锁定效应。

BCIs 将在智能家居行业发挥重要作用，并有可能在长期内取代智能手机。我担心非侵入性的 BCI 产业或多或少会像智能手机产业一样(寡头垄断)。

BCI 在智能环境领域的应用不仅限于家庭。还有为工作场所或汽车行业设计的开发项目。由于这些原因，我担心我们的市场会有一些 BCI 设备和其他公司制造的许多不同的“应用程序”。

![](img/f2a562caf95769c298fd5a783b62b81c.png)

## 规章制度

不知何故，该行业在哪些设备符合 BCI(消费者端)标准方面缺乏清晰度。此外，大脑数据的问题仍然不清楚。我们可以出售大脑数据吗？我们是否必须遵守某些规则，例如 GDPR？如果 BCI 被黑了会怎么样？…

谈到医疗设备，“食品药品监督管理局监管一切——包括一些 BCI。然而，一些 BCI 不被归类为医疗器械，而是直接销售给消费市场”( [13](https://undark.org/2020/04/22/brain-technology-interface/) )。

# 潜在的非侵入性和非医疗商业 BCI 应用列表

下面，我列出了最常见的非侵入性和非医疗 BCI 应用。

【BCIs 能做什么？
目前，商业脑机接口往往侧重于理解你的情绪状态或你打算做出哪些动作。正如 Jo Best 所提到的那样，“当某人在想‘是’或‘不是’时，BCI 可以感知，但探测更具体的想法仍然超出了大多数商用脑机接口的范围。”( [14](https://www.zdnet.com/article/what-is-bci-everything-you-need-to-know-about-brain-computer-interfaces-and-the-future-of-mind-reading-computers/) )

而且，“大多数商业 BCI 只能解读你的想法，而不能把任何想法植入用户的头脑。然而，围绕人们如何通过脑机接口进行交流的实验工作已经开始。我们可以想象，很快我们将能够通过分享大脑的神经活动来与远方的人分享经验。

> 对于 BCI 技术来说，现在还为时尚早，但我们已经看到了有希望的结果。

## 非侵入式脑机接口的不同应用和研究包括:

*   **睡眠模式分析**
*   **疲劳和脑力负荷分析。**
*   **情绪检测**。例如，“一个监控用户大脑的系统，根据温度、湿度、照明和其他因素相应地调整空间。”( [16](https://www.tmcnet.com/topics/articles/2019/09/30/443396-what-brain-computer-interfaces.htm) ) **近日，日产与 Bitbrain** 合作，展示了首款 [**脑车接口**](https://www.bitbrain.com/blog/nissan-brain-to-vehicle-technology) **原型车。**
*   **情绪分析**
*   **控制装置**(机械臂等)。)
*   **利用脑电波的个人识别系统**。
*   **利用[经颅直接刺激](https://en.wikipedia.org/wiki/Transcranial_direct-current_stimulation)增加身体动作和反应时间**。
*   **职场分析/生产力最大化。**例如，有项目开发一个应用程序来[分析操作员的认知状态](https://ieeexplore.ieee.org/abstract/document/5641772/)，精神疲劳和压力水平。
*   **营销领域:**在这个领域，“有研究指出，脑电图可以用来评估不同媒体的商业和政治广告产生的[关注度](https://ieeexplore.ieee.org/abstract/document/5335045/)。脑机接口技术还可以提供对这些广告记忆的洞察。( [17](https://www.tmcnet.com/topics/articles/2019/09/30/443396-what-brain-computer-interfaces.htm) )一般来说，“BCIs 可以用来优化互联网广告或电视插播广告”( [18](https://undark.org/2020/04/22/brain-technology-interface/) )。
*   **教育领域:**在这个领域，“BCIs 可以[帮助识别每个学生学习信息](https://arxiv.org/abs/1003.2660)的清晰程度，允许教师根据结果个性化他们与每个学生的互动”( [19](https://www.tmcnet.com/topics/articles/2019/09/30/443396-what-brain-computer-interfaces.htm) )。
*   娱乐领域:在这个领域中，脑机接口可以用于视频游戏。例如，玩家可以只用一个 BCI 来控制他们的化身。就电影而言，BCIs 可以利用观众的大脑活动来帮助创作互动电影。”在未来，“未来的观众将能够沉浸其中，并通过他们的联合大脑活动来集体控制一部电影”。
*   **军事领域:**在这个领域“在国防高级研究计划局(DARPA)，BCI 已经被士兵们用来驾驶一群无人机”( [22](https://inbrain.tech/bci-allows-military-pilots-to-control-multiple-drones-telepathically/1109/) )。

# 致力于商业非侵入性 BCI 的公司

下面，我试图列出一些目前正在研究非侵入性、非医疗商业脑机接口的公司。如果你知道更多的公司，不要犹豫，联系我，我会把他们加入名单。

**脸书**
脸书[据报道以 10 亿美元收购了 BCI 公司 CTRL-labs。脸书正在进行几个项目。其中一个与翻译思想有关。第二个是关于解释某人想要单独从他们的大脑信号做出的动作。](https://www.zdnet.com/article/facebooks-mind-reading-tech-startup-deal-could-completely-change-how-we-control-computers/)

**Neurable** Neurable 专注于创造“日常”的脑机接口。

**NextMind** NextMind，做了一个“非侵入性的神经接口，它坐在一个人的后脑勺上，将脑电波翻译成可以用来控制兼容软件的数据”( [23](https://www.wired.com/story/nextmind-noninvasive-brain-computer-interface/) )。

***其他公司:***

*   轨道基
*   顺行学
*   Thync
*   NeuroSky
*   Emotiv
*   交互轴

对于 BCI 技术来说，现在还为时尚早，但结果是有希望的。然而，“这一开创性领域的科学家明确表示，我们甚至还没有触及脑机接口潜在应用的表面(BCI)”([24](https://www.news-medical.net/news/20190911/Brain-computer-interface-huge-potential-benefits-and-formidable-challenges.aspx))。

## 如果你对 BCIs 感兴趣，我推荐以下链接:

*   [脑机接口:应用与挑战](https://www.sciencedirect.com/science/article/pii/S1110866515000237?via%3Dihub)
*   [想象一个新的界面:不用说一句话的免提通信](https://tech.fb.com/imagining-a-new-interface-hands-free-communication-without-saying-a-word/)
*   [BCI 技术的新应用:工业工作条件的心理生理优化](https://ieeexplore.ieee.org/abstract/document/5641772)
*   一个教育性的脑机接口
*   [在中国和意大利受试者中，与记忆商业广告相关的 EEG 频谱活动增强](https://ieeexplore.ieee.org/abstract/document/6098615)
*   [利用高分辨率脑电图技术研究观看商业广告时的大脑活动](https://ieeexplore.ieee.org/abstract/document/5335045)
*   [脸书的“读心术”技术启动交易可能会彻底改变我们控制电脑的方式](https://www.zdnet.com/article/facebooks-mind-reading-tech-startup-deal-could-completely-change-how-we-control-computers/)