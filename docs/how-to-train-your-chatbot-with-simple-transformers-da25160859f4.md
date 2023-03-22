# 如何用简单的变形金刚训练你的聊天机器人

> 原文：<https://towardsdatascience.com/how-to-train-your-chatbot-with-simple-transformers-da25160859f4?source=collection_archive---------10----------------------->

## 创造一个令人惊叹的对话式人工智能并不一定很难，当然也不需要花几个月的时间！迁移学习是方法。

![](img/b660e3697bdc17e27fe1d4110fa58ea4.png)

Bewakoof.com 官方在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上[的照片](https://unsplash.com/@bewakoofofficial?utm_source=medium&utm_medium=referral)

# 介绍

曾经主要出现在科幻小说中的聊天机器人和虚拟助手正变得越来越普遍。今天的谷歌助手和 Siri 还有很长的路要走，才能达到钢铁侠的 J.A.R.V.I.S .等，但旅程已经开始。虽然目前的对话式人工智能远非完美，但它们也远非像伊莱扎(ELIZA)这样的简单程序那样卑微。

远离典型的基于规则的聊天机器人，拥抱脸想出了一种基于转换器的方法来构建聊天机器人，让我们利用伯特和开放 GPT 等模型的最先进的语言建模能力。使用这种方法，我们可以快速构建强大而令人印象深刻的对话式人工智能，它可以超越大多数基于规则的聊天机器人。它还消除了构建良好的基于规则的聊天机器人所需的繁琐的规则构建和脚本编写。

[简单变形金刚](https://github.com/ThilinaRajapakse/simpletransformers)提供了一种快速、高效、简单地构建这些对话式人工智能模型的方法。简单的变形金刚实现建立在拥抱脸实现的基础上，这里给出了[和](https://github.com/huggingface/transfer-learning-conv-ai)。

# 设置

设置环境相当简单。

1.  从[这里](https://www.anaconda.com/distribution/)安装 Anaconda 或 Miniconda 包管理器
2.  创建新的虚拟环境并安装软件包。
    `conda create -n transformers python`
    `conda activate transformers`
    如果使用 Cuda:
    `conda install pytorch cudatoolkit=10.1 -c pytorch`
    其他:
    `conda install pytorch cpuonly -c pytorch`
3.  如果您使用 fp16 培训，请安装 Apex。请遵循此处的说明[。(从 pip 安装 Apex 给一些人带来了问题。)](https://github.com/NVIDIA/apex)
4.  安装简单变压器。
    `pip install simpletransformers`

# 资料组

我们将使用[人物聊天](http://arxiv.org/abs/1801.07243)数据集。请注意，您不需要手动下载数据集，因为如果在训练模型时没有指定数据集，简单转换器将自动下载数据集的格式化 JSON 版本(由 Hugging Face 提供)。

# 对话式人工智能模型

是在简单变形金刚中使用的类，用来做所有与对话式人工智能模型相关的事情。这包括培训、评估和与模型的交互。

目前，你可以通过`ConvAIModel`使用任何一款 OpenAI GPT 或 GPT-2 型号。然而，拥抱脸提供的[预训练模型](https://s3.amazonaws.com/models.huggingface.co/transfer-learning-chatbot/gpt_personachat_cache.tar.gz)开箱即用性能良好，在创建自己的聊天机器人时可能需要较少的微调。您可以从[这里的](https://s3.amazonaws.com/models.huggingface.co/transfer-learning-chatbot/gpt_personachat_cache.tar.gz)下载模型，并解压文档以跟随教程(假设您已经下载了模型并解压到`gpt_personachat_cache`)。

上面的代码片段创建了一个`ConvAIModel`，并用预先训练好的权重加载转换器。

`ConvAIModel`配有多种配置选项，可在文档[中找到，此处为](https://github.com/ThilinaRajapakse/simpletransformers#additional-attributes-for-conversational-ai)。您还可以在简单的 Transformers 库[中找到全局可用的配置选项列表。](https://github.com/ThilinaRajapakse/simpletransformers#default-settings)

# 培养

您可以通过简单地调用`train_model()`方法来进一步微调角色聊天训练数据的模型。

这将下载数据集(如果尚未下载)并开始训练。

要根据您自己的数据训练模型，您必须创建一个具有以下结构的 JSON 文件。

该结构遵循下面解释的角色聊天数据集中使用的结构。(文档[此处](https://github.com/ThilinaRajapakse/simpletransformers#data-format-1))

Persona-Chat 中的每个条目都是一个**字典**，有两个键`personality`和`utterances`，数据集是一个条目列表。

*   `personality` : **包含代理人个性的字符串列表**
*   `utterances` : **字典列表**，每个字典有两个键，分别是**字符串列表**。
*   `candidates`:【下一个 _ 话语 _ 候选人 _1...下一个候选是在会话数据中观察到的基本事实响应
*   `history`:【dialog _ turn _ 0，...dialog_turn N]，其中 N 是奇数，因为其他用户开始每个对话。

预处理:

*   句末句号前的空格
*   全部小写

假设您已经创建了一个具有给定结构的 JSON 文件，并将其保存在`data/train.json`中，您可以通过执行下面的行来训练模型。

```
model.train_model("data/minimal_train.json")
```

# 估价

通过调用`eval_model()`方法，可以对角色聊天数据集进行评估，就像训练一样简单。

与培训一样，您可以提供不同的评估数据集，只要它遵循正确的结构。

# 互动

虽然你可以通过计算评估数据集的指标来获得数字分数，但了解对话式人工智能有多好的最好方法是与它进行实际对话。

要与您刚刚训练的模型对话，只需调用`model.interact()`。这将从数据集中选择一个随机的个性，并让你从终端与之对话。

```
model.interact()
```

或者，您可以通过给`interact()`方法一个字符串列表来创建一个动态的角色！

# 包扎

提示:要加载一个经过训练的模型，您需要在创建`ConvAIModel`对象时提供包含模型文件的目录的路径。

```
model = ConvAIModel("gpt", "outputs")
```

就是这样！我希望这篇教程能帮助你创建自己的聊天机器人！