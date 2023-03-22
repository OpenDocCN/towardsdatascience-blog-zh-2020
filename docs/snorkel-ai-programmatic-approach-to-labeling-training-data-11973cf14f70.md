# 浮潜人工智能:标记训练数据的程序化方法

> 原文：<https://towardsdatascience.com/snorkel-ai-programmatic-approach-to-labeling-training-data-11973cf14f70?source=collection_archive---------26----------------------->

## 浮潜人工智能标记和结构化训练数据的根本不同的方法是人工智能广泛采用的最后一个缺失的部分吗？

![](img/e08dbbc74baf461f9d423a152cda12f4.png)

沙哈达特·拉赫曼在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄的照片

OpenAI 最近发布的 [GPT-3 API](https://openai.com/blog/openai-api/) 让 Twitterverse 火了起来，开发者使用其庞大的语言模型来生成从文本到代码的一切:

GPT-3 生成反应代码

虽然对 GPT-3 的炒作和关注是合理的，但人工智能的另一个令人兴奋的发展在很大程度上被忽视了。7 月 14 日，2019 年从斯坦福人工智能实验室剥离出来的[潜泳人工智能](https://www.snorkel.ai/)在来自 Greylock、GV 和 In-Q-Tel 的[1500 万美元的资助](http://www.finsmes.com/2020/07/snorkel-ai-raises-15m-in-funding.html)下浮出水面。此次发布会展示了一个机器学习平台，它可以编程地标记和准备训练数据，以加速 ML 模型的构建和部署过程。浮潜的早期用户包括[谷歌](https://ai.googleblog.com/2019/03/harnessing-organizational-knowledge-for.html)、[英特尔](https://dl.acm.org/doi/abs/10.1145/3329486.3329492)、[苹果](https://arxiv.org/abs/1909.05372)、[斯坦福医学](https://www.sciencedirect.com/science/article/pii/S2666389920300192)。尽管该项目仍处于初级阶段，但 still 的人工智能方法可能是企业人工智能和人工智能/人工智能在各种垂直领域的广泛采用的重要突破。

# 手工标注训练数据瓶颈

尽管机器学习框架(例如 [Tensorflow](https://www.tensorflow.org/) 、 [PyTorch](https://pytorch.org/) 、 [Keras](https://keras.io/) )、硬件(例如 GPU、TPUs)和全面研究(例如 AlphaGo、GPT-3)都有了巨大的改进，但准备训练数据在很大程度上仍然是一个手动过程。从在图像上绘制边界框到注释音频文件，数据科学家要么手工标记大量文件，要么将任务众包给一群合同工。训练和建立深度学习模型所需的海量数据加剧了这一问题。由于各种开源工具和云托管的工作负载，深度学习比以往任何时候都更容易实现，这一瓶颈变得更加明显。

![](img/5e63b9094e790cedffa2d659538c219e.png)

计算机视觉注释工具:由英特尔公司提供—[https://github . com/opencv/cvat/raw/0 . 2 . 0/cvat/apps/documentation/static/documentation/images/cvat . jpg](https://github.com/opencv/cvat/raw/0.2.0/cvat/apps/documentation/static/documentation/images/cvat.jpg)，CC BY-SA 4.0，[https://commons.wikimedia.org/w/index.php?curid=74718749](https://commons.wikimedia.org/w/index.php?curid=74718749)

将人工标记过程与机器学习其他阶段的重大改进进行比较。我们现在有 AutoML 和其他编程方法来加快特征工程和超参数调整。基础架构供应工具和云架构使这些模型的部署比以往任何时候都更容易。另一方面，手动标记数据不可扩展(至少不具有成本效益，尤其是当涉及专业知识和隐私时，例如医学图像、财务报表)，并且极易出错。

浮潜的团队认为，手动标记训练数据的过程从根本上被打破了。在今天 ML 里程碑式的巨大成功背后隐藏着一个隐藏的成本。例如，费李非博士和她在斯坦福人工智能实验室的团队花了两年时间创建了 ImageNet，这一基础数据集导致了谷歌、Clarifai、DeepMind、百度和华为的令人难以置信的研究。为了让数据真正成为数字经济中的新石油，将数据集标注转变为迭代开发过程至关重要。

> “尽管在人工智能上花费了数十亿美元，但很少有组织能够像他们希望的那样广泛有效地使用它。这是因为可用的解决方案要么忽略了当今人工智能最重要的部分——为现代方法提供燃料的标记训练数据——要么依赖大量的人类标签员来产生它。”—浮潜人工智能首席执行官亚历克斯·拉特纳

# 数据的程序化标记

潜泳人工智能的创始团队——亚历克斯·拉特纳(华盛顿大学助理教授)和克里斯·雷(斯坦福大学副教授，2015 年麦克阿瑟天才研究员)——试图实现基于规则的系统，以程序化标签和建立高质量的训练数据。想法是允许程序员或领域专家定义[标记函数](https://papers.nips.cc/paper/6523-data-programming-creating-large-training-sets-quickly)来生成标记数据。在[潜泳博客](https://www.snorkel.ai/07-14-2020-snorkel-ai-launch)上使用的例子以法律文档为例，其中法律分析师创建了一个函数，如果文档的标题中有“雇佣”，就将文档标记为“雇佣合同”。

虽然基于规则的系统具有简单和直接的优势，但它也很脆弱，因为它缺乏处理已定义模式之外的输入的鲁棒性(例如，如果使用“合同”而不是“雇用”会发生什么)。因此，通气管流采用由标记函数创建的数据集，将其视为充满噪声的生成模型，并使用弱监督 ML 模型来“去噪”或推广该算法。本质上，它结合了人类定义的规则和机器学习算法，以创建更强大的标记数据集。

# 通气管流量

虽然最初的开源项目[scupk 项目](https://github.com/snorkel-team/snorkel)专注于训练数据的编程方法，但 scupk Flow 进一步解决了构建端到端 ML 解决方案的问题。这包括数据扩充(例如添加旋转或模糊图像)、数据切片(例如获取数据子集)以及生产级 ML 应用所需的操作组件(例如监控、警报、IaaC)。

目前对浮潜流的访问是有限的([需要请求演示](https://www.snorkel.ai/demo))，但浮潜背后的基本想法似乎是超越科技巨头的民主化机器学习的缺失环节。请关注这家初创公司，看看它如何在未来几年重塑 ML 行业。