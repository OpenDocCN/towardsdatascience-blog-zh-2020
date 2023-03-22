# 使用暹罗伯特网络的快速语义搜索

> 原文：<https://towardsdatascience.com/quick-semantic-search-using-siamese-bert-networks-1052e7b4df1?source=collection_archive---------8----------------------->

## 使用 Sentence-BERT 库创建适合在大型语料库上进行语义搜索的固定长度的句子嵌入。

![](img/48e9b08dd174860db8c2da10030408f7.png)

图片由 [PDPhotos](https://pixabay.com/users/PDPhotos-16/?utm_source=link-attribution&amp;utm_medium=referral&amp;utm_campaign=image&amp;utm_content=3578) 提供，并从 [Pixabay](https://pixabay.com/users/PDPhotos-16/?utm_source=link-attribution&amp;utm_medium=referral&amp;utm_campaign=image&amp;utm_content=3578) 下载。

我一直在开发一个工具，需要嵌入基于 BERT 的语义搜索。想法很简单:获取语料库中每个句子的 BERT 编码，然后使用余弦相似度来匹配查询(一个单词或另一个短句)。根据语料库的大小，为每个句子返回一个 BERT 嵌入需要一段时间。我试图使用一个更简单的模型(如 ELMO 或伯特即服务)，直到我看到尼尔斯·雷默斯和伊琳娜·古雷维奇的 2019 年 ACL 论文“[句子-伯特:使用暹罗伯特网络的句子嵌入](https://www.aclweb.org/anthology/D19-1410.pdf)”。这篇论文的源代码可以从 [Github](https://github.com/UKPLab/sentence-transformers) 获得，并且 [PyPi](https://pypi.org/project/sentence-transformers/) 已经准备好了要被 pip 安装的句子-BERT 库(如果你使用 Python 的话)。在这篇博文中，我将简要描述这篇论文解决的问题，并展示一个 Google Collab 笔记本，它展示了使用 SOTA 伯特模型实现语义搜索功能是多么容易。

# 为什么是连体和三联体网络结构？

BERT 使用交叉编码器网络，将两个句子作为变压器网络的输入，然后预测目标值。BERT 能够在语义文本相似性任务上实现 SOTA 性能，但是两个句子都必须通过完整的网络。在由 10000 个句子组成的语料库中，找到相似的句子对需要大约 5000 万次推理计算，大约需要 65 个小时(如果你有合适的计算能力，例如 V100 GPU)。令人印象深刻的是，BERT 可以在大约 5 秒钟内提供 10000 个句子的编码！！！

句子-BERT 使用连体和三元网络结构微调预训练的 BERT 网络，并向 BERT 的输出添加汇集操作，以导出固定大小的句子嵌入向量。所产生的嵌入向量更适合于向量空间内的句子相似性比较(即，可以使用余弦相似性进行比较)。

# …为什么不是伯特即服务？

流行的 [BERT-As-A-Service](https://github.com/hanxiao/bert-as-service/) 库通过 BERT 运行一个句子，并通过**对模型的输出**进行平均来导出一个固定大小的向量。BERT-As-A-Service 也没有在生成有用的句子嵌入的上下文中进行评估。

# 用句子-BERT 库进行语义搜索

我创建了一个 Google Colab 笔记本来演示如何使用 Sentence-BERT 库创建一个语义搜索示例。Github 回购可从这里获得:[SiameseBERT _ semantic search . ipynb](https://github.com/aneesha/SiameseBERT-Notebook/blob/master/SiameseBERT_SemanticSearch.ipynb)

Collab 笔记本的要点版本如下所示: