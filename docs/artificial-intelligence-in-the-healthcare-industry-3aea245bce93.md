# 医疗保健行业中的人工智能

> 原文：<https://towardsdatascience.com/artificial-intelligence-in-the-healthcare-industry-3aea245bce93?source=collection_archive---------76----------------------->

## 人工智能如何引领诊断和药物发现的未来

![](img/b6e93125b67c5167503a53e296a66e2b.png)

Jair Lázaro 在 [Unsplash](https://unsplash.com/?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) 上的照片

人工智能是推动医疗保健未来发展的技术之一。人工智能并不打算(也不可能)完全自动化医疗保健流程。人工智能机器人医生和护士不是这里的目标。相反，与人工智能并肩工作的医疗保健专业人员将开创一个前所未有的病人护理效率的新时代。在这篇文章中，我们将回顾人工智能如何改善当前围绕患者诊断和药物发现的系统，并考虑阻止人工智能在医疗保健中实施的障碍。

# 诊断

目前，误诊是一个巨大的问题:仅在美国，估计每年就有 1200 万人遭受诊断错误，每年在美国医院中估计有 40，000 到 80，000 人因误诊而死亡(来源:[【激烈医疗】](https://www.fiercehealthcare.com/hospitals-health-systems/jhu-1-3-misdiagnoses-results-serious-injury-or-death#:~:text=1%20cause%20of%20serious%20medical,or%20permanent%20damage%20or%20death.))。通过 AI，特别是卷积神经网络(CNN)，可以更准确地从医学成像中诊断疾病。在我们讨论 CNN 如何诊断疾病之前，让我们先了解一下 CNN 是什么。

![](img/a201cf94fa6b8b23ce517f3c521cd4b6.png)

来源:[技术运行](https://technologiesrunning.blogspot.com/)【CC BY 4.0】

人工神经网络模仿人脑的生物神经网络，并可以基于过去数据中识别的模式进行预测。CNN 是一种神经网络，通常包括卷积层和最大池层。卷积将滤镜应用于构成输入图像的像素集合。这导致激活。这种过滤器的重复应用产生了一种称为特征图的激活图，它本质上告诉计算机关于图像的信息。卷积层之后是最大池层。在 max-pooling 层中，图像上的过滤器检查每个部分中的最大像素值(部分的大小由程序员指定)，然后使用最大像素值创建一个新的更小的图像。这些较小的图像有助于计算机更快地运行模型。当卷积层和最大池层连接到神经网络的输入和输出层时，该模型能够使用过去的标记数据来预测新图像。

![](img/fd423c5828e3f418bbf457767bded51b.png)

来源:[门德利](https://data.mendeley.com/datasets/rscbjbr9sj/2)【CC BY 4.0】

上面这些 x 光图像描绘了一个正常人、一个细菌性肺炎患者和一个病毒性肺炎患者的肺部。带有每个条件的标记图像的数据集将用于训练 CNN。在训练模型之后，我们可以将患者的 x 射线输入模型，它会将 x 射线分类为指示健康的人或感染了细菌性肺炎或病毒性肺炎的人。

实现人工智能来诊断疾病似乎非常有前途:研究发现，在正确诊断疾病方面，模型的表现与人类专业人员不相上下。最近，斯坦福机器学习小组开发了一个模型，可以在短短 10 秒内诊断肺炎！在未来，将人工智能与诊断医生结合在一起可以减少误诊的机会。

# 药物发现

![](img/6fb59d96d047eb27a95e694738766ac0.png)

由[亚当·尼格西奥鲁克](https://unsplash.com/@adamsky1973?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)在 [Unsplash](https://unsplash.com/s/photos/pill?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) 上拍摄的照片

药物研发是人工智能似乎准备颠覆的另一个医疗保健领域。由于所涉及的高度复杂性，将新药推向市场既耗时(> 10 年)又昂贵(平均花费 26 亿美元)。此外，一种药物获得 FDA 批准的可能性不到 12%(来源: [PhRMA](http://phrma-docs.phrma.org/sites/default/files/pdf/rd_brochure_022307.pdf) )。通过利用神经网络寻找新药，制药公司的目标是同时减少这一过程所需的时间和金钱。

神经网络由输入层、隐藏层和输出层组成。输入层是输入数据的地方，隐藏层是具有权重和偏差的神经元执行计算的地方，输出层中的激活函数给出最终输出。当神经网络在大的标记数据集上训练时，它们的预测输出与实际输出进行比较，并且误差函数用于更新权重/偏差(反向传播)。神经网络利用大量信息快速学习和预测的能力使其成为药物发现的理想选择。

在药物发现过程的第一步，研究人员寻求在分子水平上理解人类疾病。一旦研究人员形成一个想法，他们就专注于确定一个药物靶点，当药物化合物与其相互作用时，该靶点可以治疗或预防疾病。随着药物靶标的确定，研究人员筛选大量的化合物，直到他们找到最终可能成为药物的少数化合物。仅这一过程就需要三到六年。神经网络可以大大加快这个速度；例如，人工智能药物发现公司 twoXAR 已经将这一过程缩短到仅三个月左右。

# 挑战

同样重要的是要记住，人工智能在医疗保健领域的应用将面临各种障碍。

*   AI 在某些情况下难免会出现诊断错误；与人为错误相比，患者可能会对人工智能错误表现出更多的关注。根据医疗事故法，医生可以因误诊而被起诉，但目前不存在因人工智能误诊而提起的诉讼。
*   对大数据集的需求给医疗保健领域采用人工智能带来了一些挑战。医疗数据不容易获得，这使得开发有效的人工智能模型变得很困难。此外，从患者那里收集数据会引发隐私问题。一个潜在的解决方案是通过区块链分类账匿名存储病人。
*   偏见是另一个需要注意的重要问题。人工智能中的偏见是一个存在于医疗保健应用之外的问题，并扩展到对整个技术的广泛关注。由于人工智能模型根据它们在训练数据集中所学的知识进行预测，如果训练数据偏向于具有特定种族、性别、位置等的患者，它们可能无法推广到所有患者。因此，确保在训练数据集中代表不同的人群是至关重要的。

# 参考

[1] PhRMA，[生物制药研究&开发](http://phrma-docs.phrma.org/sites/default/files/pdf/rd_brochure_022307.pdf)，PhRMA 手册

[2]英特尔人工智能，[人工智能如何革新药物发现](https://www.forbes.com/sites/intelai/2019/02/11/how-ai-is-revolutionizing-drug-discovery/#60d15c64eab4)，福布斯

[3] W. Nicholson Price II，[医疗保健中人工智能的风险和补救措施](https://www.brookings.edu/research/risks-and-remedies-for-artificial-intelligence-in-health-care/#:~:text=While%20AI%20offers%20a%20number,health%2Dcare%20problems%20may%20result.)，布鲁金斯

感谢阅读！

我是 Roshan，16 岁，对人工智能的应用充满热情。如果你对人工智能更感兴趣，可以看看我关于新冠肺炎推文情感分析的文章。

在 Linkedin 上联系我:[https://www.linkedin.com/in/roshan-adusumilli/](https://www.linkedin.com/in/roshan-adusumilli/)