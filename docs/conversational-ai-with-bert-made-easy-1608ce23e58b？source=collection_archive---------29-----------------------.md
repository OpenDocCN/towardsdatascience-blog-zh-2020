# 与伯特的对话人工智能变得容易

> 原文：<https://towardsdatascience.com/conversational-ai-with-bert-made-easy-1608ce23e58b?source=collection_archive---------29----------------------->

## 使用 Rasa 和 Huggingface 即插即用变压器

一年多来，我一直在尝试使用 NLP 和 python 来自动安排约会，多亏了一个令人惊叹的免费开源对话工具 Rasa，我终于成功了。

事实是一年前，当我开始这个项目时，我现在使用的工具几乎不可用。当然不是我们今天看到的易于使用的形式。作为人工智能和 NLP 科学发展速度的一个证明，去年(2019 年 6 月)的这个时候，像 BERT 这样的变形金刚刚刚离开学术研究的领域，刚刚开始在谷歌和脸书等科技巨头的生产中看到。BERT 论文本身在 2018 年 10 月才发表(下面是论文链接)。

今天，由于像 [Rasa](https://rasa.com/) 和 HuggingFace 这样的开源平台，BERT 和其他变压器架构可以以一种简单的即插即用方式使用。此外，Rasa 的数据科学家开发了一种基于转换器的特殊分类器，称为[双重意图实体转换器](https://blog.rasa.com/introducing-dual-intent-and-entity-transformer-diet-state-of-the-art-performance-on-a-lightweight-architecture/)或 DIET 分类器，为同时提取实体和分类意图的任务量身定制，这正是我们在阿根廷 [Kunan S.A.](https://www.kunan.com.ar/) 与我的团队开发的约会安排虚拟助理 MyTurn 的产品。

![](img/affd2670a2f77145d151920e76d290a3.png)

饮食结构图(image| **Rasa**

# 证据就在 p̶u̶d̶d̶i̶n̶g̶ f1 的成绩中

对于定量的人来说，在我用 DIET 替换了基于 ye ole Sklearn 的分类器后，我的 F1 分数在实体提取和意图分类方面都提高了 30%以上！！！！比较下图中的 SklearnIntentClassifier 和 diet_BERT_combined。如果你曾经设计过机器学习模型，你会知道 30%是很大的。就像那次你意识到你把车停在了洒水器的水管上。当事情以它们应该的方式运行时，这是多么令人惊奇的事情啊！

![](img/acbbef6922e3b2a06c1f0672846d28f5.png)

模型评估(image |**bubjanes w/streamlit**)

![](img/54fb9fae13335b01c8b608a199267662.png)

意向分类原始数字(image| **bubjanes**

# 智能的蓝图:config.yml

是科学还是艺术？都不是。它尝试超参数的每一种可能的组合，并选择给你最高指标的配置。这叫做网格搜索。

需要注意的是，伯特不是一粒神奇的药丸。事实上，对于我的特定任务和训练数据，单独的 BERT 预训练单词嵌入并没有提供好的结果(参见上面的 diet_BERT_only)。这些结果明显比 2019 年旧的基于 Sklearn 的分类器差。也许这可以通过在阿根廷科尔多瓦的非正式西班牙语聊天中发现的地区行话和口语来解释，我们的培训数据就是从那里生成的。根据 HuggingFace 文档，我们使用的多语言预训练 BERT 嵌入是“在最大的维基百科中的前 104 种语言的大小写文本上训练的”。

然而，我们获得的最高性能模型是通过使用 DIET 在我们自己的 Córdoba 数据上训练定制特征，然后在前馈层中将这些监督嵌入与 BERT 预训练嵌入相结合(参见上面 diet_BERT_combined 中的结果)。下面的小图显示了如何将基于科尔多瓦数据训练的“稀疏特征”与前馈层中的 BERT“预训练嵌入”相结合。此选项非常适合于训练数据很少的西班牙语项目。也就是说，组合模型的性能仅略好于使用没有 BERT 预训练嵌入的 DIET 的模型(参见上面 diet_without_BERT 的结果)。这意味着对于非英语语言的聊天机器人来说，训练数据量适中，DIET 架构可能就是你所需要的。

![](img/82cd84ab5763246559054a7627aebe7a.png)

将自定义监督单词嵌入和 BERT 预训练单词嵌入结合到前馈层的示意图。(图片| **拉莎**)

# **即插即用，适用于 realz**

在安装了 [Rasa](https://rasa.com/docs/rasa/user-guide/installation/) ，并构建了一个符合你需求的助手(我建议在做这件事之前先看看 Rasa 的 YouTube [教程](https://www.youtube.com/watch?v=rlAQWbhwqLA&list=PL75e0qA87dlHQny7z43NduZHPo6qd-cRc))之后，BERT 嵌入的实现是如此简单，几乎令人失望。

下面是我们使用的配置文件的一个例子。这一长串的超参数可能看起来让人不知所措，但是相信我，这比一年前容易多了。

![](img/8c5bc8fc9c88303c9ac77aa749a27fff.png)

Rasa 提供的可用超参数(image| **bubjanes** )

您必须下载 BERT 依赖项:

```
pip install "rasa[transformers]"
```

要集成 BERT 或在 HuggingFace 网站上可用的任何其他预训练模型，只需用您想要使用的任何预训练嵌入替换下面行中的**model _ weights**hyperparameter。

```
- name: HFTransformers NLP 
  model_weights: “bert-base-multilingual-cased” 
  model_name: “bert”
```

我们使用了**Bert-base-multilingual-cased**，因为它是适用于西班牙语的最佳模型。

参见我们的 [github](https://github.com/kunan-sa/the-conversational-ai-pipeline) 获取本文中提到的配置文件的完整示例和其他链接。

# 结论

Rasa 的美妙之处在于它简化了自然语言理解(NLU)、命名实体识别(NER)和对话管理(DM)的模型训练，这是面向任务的对话系统所需的三个基本工具。尽管我们做了很多好的编程来使我们的系统工作得像现在这样好，但是在没有任何真正的 Python 技能的情况下，您可能可以完成大约 80%的 Rasa 虚拟助手的构建。

随着自然语言处理领域令人兴奋的进步，如变形金刚和预训练的单词嵌入，对话式人工智能领域近年来取得了长足的进步，从说“对不起，我不明白”的机器人，到真正成为曾经需要单调乏味的人类工作的日常任务的可行解决方案。

从哲学上讲，这项技术的目标不是用机器人取代人类，而是将重复和“机器人化”的日常任务，如数据输入或预约安排，分配给虚拟助理，并将人类的大脑空间留给需要只有人类才具备的技能的工作类型，如创造力和批判性思维。MyTurn 是一个简单但有先见之明的例子，表明对话式人工智能不是为大型科技公司保留的工具，而是事实上每个人都可以通过 Rasa 和 HuggingFace 等免费开源技术获得。

![](img/ea1f9eaf3d3fc77f2ffaa73a29e4d754.png)

建议阅读:

汤姆·博克里什，乔伊·福克纳，尼克·鲍洛夫斯基，艾伦·尼科尔， *Rasa:开源语言理解和对话管理*，2017 年 12 月 15 日

阿希什·瓦斯瓦尼、诺姆·沙泽尔、尼基·帕尔马、雅各布·乌兹科雷特、莱昂·琼斯、*关注是你所需要的一切*，2017 年 12 月 6 日

高剑锋(微软)，米卡赫尔·格里(微软)，李立宏(谷歌)，*对话式人工智能的神经方法:问题回答，面向任务的对话和社交聊天机器人*，2019 年 9 月 10 日

Jacob Devlin Ming-Wei Chang Kenton Lee Kristina Toutanova Google AI Language， *BERT:用于语言理解的深度双向转换器的预训练*，2018 年 10 月 11 日