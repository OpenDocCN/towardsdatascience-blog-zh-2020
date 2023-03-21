# CoronaXiv:人工智能驱动的基于弹性搜索的新冠肺炎研究论文搜索引擎

> 原文：<https://towardsdatascience.com/coronaxiv-b2b36d725e2e?source=collection_archive---------52----------------------->

## [变更数据](https://towardsdatascience.com/tagged/data-for-change)

## 利用 NLP 和 ElasticSearch 的力量以人工智能的方式为对抗新冠肺炎疫情做出贡献！

![](img/732a26bbe8d745d87f9956921c5f5d45.png)

CoronaXiv 标志。来源:作者

我们在 2020 年 HackJaipur 打造的产品 [**CoronaXiv**](https://github.com/arghyadeep99/CoronaXiv) 从全国 350 多个团队中脱颖而出，获得了 ***【最佳弹性搜索产品】*** *的称号。我们在这篇博文中解释了我们制作 CoronaXiv 的方法。*

# 什么是 CoronaXiv？

CoronaXiv 是一个由 [ElasticSearch](http://elastic.co/) 驱动的人工智能搜索引擎，它索引了 **60k+的研究论文**，这些论文是为了应对全球疫情的 Corona 病毒而堆积起来的，那里的研究人员每天都在发布新的论文，试图了解这种病毒的本质，并试图找到治愈它的方法。CoronaXiv 是一款 web 应用程序(即将成为 PWA ),基于以下技术架构:

1.  瓶
2.  PyTorch(伯特模型)
3.  nltk
4.  vue . j
5.  弹性搜索
6.  Python3
7.  赫罗库

如您所见，CoronaXiv 融合了许多现代工具、框架和模型。这帮助我们让我们的东西快速工作，节省了我们重新发明轮子的时间！我们的项目在这里[可用](https://devfolio.co/submissions/coronaxiv)。要试用 CoronaXiv，请访问[http://www . CoronaXiv 2 . surge . sh](http://www.coronaxiv2.surge.sh)。(目前，由于我们用尽了 ElasticSearch 的免费限制，它已经下降，但我们很快就会恢复！)

# CoronaXiv 解决什么问题？

许多研究人员在全球各地远程工作，这些地方的封锁限制各不相同。一些研究人员在实验室工作，而一些在家里工作。为了帮助他们努力击败疫情冠状病毒，任何研究人员拥有一个专门的新冠肺炎论文搜索引擎都将非常方便，并且还可以获得人工智能推荐的其他类似论文的链接，从而节省他们的时间。在这场与全球疫情的战斗中，每一秒都是宝贵的，因此，我们建立了 CoronaXiv，这是一个以弹性搜索为动力的人工智能搜索引擎，用于搜索与冠状病毒相关的研究论文。

在当前的场景中，人们会执行 Google 搜索来查找一些研究论文。然而，通常情况下，某些关键词会产生与疫情冠状病毒无关的结果，并且也缺乏 UX，因为用户每次都必须回到主屏幕，从一篇论文切换到另一篇论文。使用 CoronaXiv，人们可以轻松地直接访问论文，使用不同的聚类可视化来帮助用户理解不同论文的关系，并基于关键字识别论文，或者访问基于相似域聚类的论文。

![](img/55273b5d88c8d4ae9f7eba9c1382a4d6.png)

来源: [Freepik](https://images.app.goo.gl/PotppHaHqUCNhcJCA)

CoronaXiv 是一个搜索平台，允许研究人员从互联网上堆积的杂乱研究论文中搜索他们希望看到的最重要的论文，因为每天都有新的新冠肺炎研究论文问世。到目前为止，CoronaXiv 已经积累了超过 6 万篇研究论文，可以根据各种标准顺利地对研究论文进行索引，还允许研究人员根据各种过滤器搜索论文，如发布日期范围、同行评审、总浏览量/引用量、H-index 等。我们目前正致力于增加更多这样的过滤器，以提高生活质量。

# 我们是如何构建 CoronaXiv 的？

![](img/ed6dd645bb23bba8003610741c5d99d6.png)

来源: [Freepik](https://images.app.goo.gl/HrzzD7pNq64WAgmF8)

眼前的问题不是小事。作为一项巨大的任务投入其中无异于自杀。因此，我们需要将我们的主要目标分解成几个更小的目标(模块化方法)来实现它们。我们项目开发中涉及的步骤包括:

1.  预处理 Kaggle 上提供的 [CORD-19 数据集](https://www.kaggle.com/allen-institute-for-ai/CORD-19-research-challenge)
2.  使用 Altmetrics & Scimago 等外部 API 获取额外的元数据
3.  使用 [CovidBERT](https://huggingface.co/gsarti/covidbert-nli) (根据医学文献微调的预训练 BERT 模型)生成单词嵌入
4.  使用主成分分析(PCA)和 t 分布随机邻居嵌入(tSNE)来降低嵌入维数，确保我们的搜索引擎的更快性能
5.  从 60k+论文语料库中获取聚类信息并提取关键词
6.  将研究论文语料库的最终 CSV 与 ElasticSearch 整合
7.  构建 web 应用的前端，并将其与 ElasticSearch 集成

步骤 1-5 都是在 Jupyter 笔记本上执行的，该笔记本将在未来公开。第 6-7 步是真正的故事展开的地方。

# 构建 CoronaXiv 时面临的挑战

CORD-19 数据集的预处理阶段花费了很长时间，但我们最终提取了关于要使用和显示的每张纸的有用信息，并生成了嵌入，以帮助人工智能决策。由于多次失败的调用请求，API 非常慢，占用了我们黑客马拉松的大量时间。在由于 PCA 和运行模型的时间约束而保留的信息上进行权衡，其中时间约束被给予优先权。

![](img/6c4538024ba5207c6d4fa87763ca1e3d.png)

来源: [Freepik](https://images.app.goo.gl/9RVXzXpLDgwPTY3a6)

对于基于关键字的索引，我们可以对每个查询使用基本的 TF-IDF，然后随时与关键字进行匹配。更重要的是，我们的服务器在处理多个请求的情况下会出现瓶颈，因为我们只有一个深度学习模型实例，因此它一次只能服务一个请求。因此，这将是一个极其缓慢的实现，并且会占用大量的资源和时间。这时我们遇到了 **ElasticSearch** 。

![](img/e9180bfe4440b16bcbb5c05ab716eeb0.png)

弹性搜索

[ElasticSearch](https://www.elastic.co/) ，是一个分布式、开源的搜索和分析引擎，针对不同类型的文本数据。使用 Elastic，人们可以基于关键字执行高效且极快的全文搜索。使用我们的 CovidBERT 嵌入和弹性搜索的能力，我们可以得到最相关的论文的结果，按重要性递减的顺序排序。论文摘要或摘录总结了整篇论文并讨论了论文中所讨论的内容。因此，由于并非所有论文都有摘要，所以数据是在论文摘录的基础上进行索引的。这有助于加快检索过程，因为我们已经从一个非常压缩的论文内容表示中建立了索引。

老实说，我们以前从未使用过 [ElasticSearch](http://elastic.co/) ，我们对此没有任何经验。所以我们从文档开始。这个包中解释得很好的[文档](https://elasticsearch-py.readthedocs.io/en/master/)指导我们以一种极其有效和简洁的方式使我们的服务器与 ElasticSearch 交互。由于我们的技术栈是作为后端的 **Flask** 和作为前端的 **Vue.js** ，我们决定使用 ElasticSearch 的官方 Python 包，这个包可以在这里[找到](https://github.com/elastic/elasticsearch-py)，并由世界各地的开发人员不断维护。

我们用这个包创建了索引，每次前端发出请求时，服务器都会与 ElasticSearch 交互，并提供与特定标签相关的结果，然后我们将结果提供给用户。我们还创建了一个脚本，可以帮助我们在特定的索引中批量插入许多文档。该脚本基本上接受您的 ElasticSearch 帐户的凭证，创建一个索引，并一次添加多个文档。

# 最终产品(或者是？)

我们的最终产品在黑客马拉松的时间框架内勉强完成。感谢 2020 年 hack Jaipur 的组织者非常友好和理解，由于许多人在封锁期间面临的互联网问题，应多名参与者的请求，截止日期延长了 5 个小时。该项目的演示如下:

CoronaXiv 演示

这是我们团队的一个非常雄心勃勃的项目，我们希望我们可以通过每个版本的更新功能来不断改进我们的网站。因此，我们的团队期待着维护这个网站，至少直到世界摆脱这个疫情。我们将请求 ElasticSearch 团队帮助我们在免费层下维护这个项目，或者赞助我们雄心勃勃的项目！

![](img/5292614f3ab084163623b002447bcc12.png)

来源: [Freepik](https://images.app.goo.gl/ZboSqNUj7tBZMy5P9)

# 我们对 ElasticSearch 和 HackJaipur 2020 的体验

一旦我们理解了文档，就很容易把它和我们的项目联系起来。ElasticSearch 帮助我们扩展了我们的产品，否则我们的产品会留在本地主机或其他地方。我们很快就掌握了使用 ElasticSearch 的诀窍，这要归功于 Elastic 的高级开发者倡导者**阿拉文德·普特勒乌**举办的 ElasticSearch 演示研讨会。

这是我们团队的第一次远程黑客马拉松，也是国家级的！为期一周的研讨会非常吸引人，令人兴奋。因为这是一个虚拟的黑客马拉松，我们使用 WhatsApp 和 Telegram 进行交流。我们真的很喜欢 HackJaipur 给我们的这种独特的体验，我们想对 HackJaipur 2020 的整个组织团队和赞助商如[大联盟黑客](http://mlh.io)、 [ElasticSearch](http://elastic.co/) 、GitHub 等表示感谢。

![](img/695a0922509e45e2170883b156631352.png)

2020 年 HackJaipur 随机团队的成功外出！来源:[剪贴画](https://www.clipart.email/download/13881281.html)

# 关于开发商

我们是**团队随机**。我们忙着做决定。随着每个时代的到来，我们做出下一个决定。毕竟这个世界上的任何事情都是随机的！您可以通过我们会员的个人资料联系他们:

1.  Arghyadeep Das: [GitHub](https://www.github.com/arghyadeep99) ， [LinkedIn](https://www.linkedin.com/in/arghyadeep-das/)
2.  纳齐基特·布塔: [GitHub](https://www.github.com/nachiketbhuta) ， [LinkedIn](https://www.linkedin.com/in/nachiket-bhuta-3061ba144/)
3.  尼兰什·马图尔: [GitHub](https://www.github.com/neelansh15) ， [LinkedIn](https://www.linkedin.com/in/neelansh-mathur/)
4.  杰耶什·尼夫: [GitHub](https://www.github.com/Techno-Disaster) ， [LinkedIn](https://www.linkedin.com/in/techno-disaster/)

编码快乐！😄