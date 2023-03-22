# 使用 T5 文本到文本转换器模型从任何内容生成布尔型(是/否)问题

> 原文：<https://towardsdatascience.com/generating-boolean-yes-no-questions-from-any-content-using-t5-text-to-text-transformer-model-69f2744aff44?source=collection_archive---------31----------------------->

## 使用 BoolQ 数据集和 T5 文本到文本转换器模型的问题生成算法的训练脚本和预训练模型。

![](img/bbced2d527390d2e344b1f2c593aa9c8.png)

图片来自 [Pixabay](https://pixabay.com/)

# 投入

我们程序的输入将是任何一般的**内容/段落**

```
**Months earlier, Coca-Cola had begun “Project Kansas.” It sounds like a nuclear experiment but it was just a testing project for the new flavor. In individual surveys, they’d found that more than 75% of respondents loved the taste, 15% were indifferent, and 10% had a strong aversion to the taste to the point that they were angry.**
```

# 输出

输出将是从上述输入生成的**布尔(是/否)**问题。

**布尔(是/否)**从 **T5 模型**生成的问题:

```
**1: Does coca cola have a kansas flavor?
2: Is project kansas a new coca cola flavor?
3: Is project kansas the same as coca cola?**
```

今天我们将看看如何从 [Huggingface 的变形金刚](https://github.com/huggingface/transformers)库中训练一个 T5 模型来生成这些布尔问题。我们还将看到如何使用提供的**预训练模型**来生成这些**布尔(是/否)**问题。

# 实际使用案例(学习聊天机器人)

![](img/ee0ef96926d6af643667b84a81195be2.png)

来自[平面图标](https://www.flaticon.com/)的图标

我想象一个场景，作为一名学生，你和一个聊天机器人互动学习一个概念**。聊天机器人会根据你的回答向你展示教科书章节中相关的字节大小的片段。它还想让**实时评估**你是否已经**理解了**提出的主题。为课本章节的每个片段手动**预生成评估**是不切实际的。**聊天机器人**可以利用这个**算法**实时生成**布尔(是/否)**问题，评估你对题目的**理解**。**

**让我们开始吧—**

# **资料组**

**![](img/8c2204f37fd1b562d36e5e7894d0ec1d.png)**

**来自[平面图标](https://www.flaticon.com/)的图标**

**我用**[**BoolQ**](https://github.com/google-research-datasets/boolean-questions)数据集收集了**段落**、**问题**和**答案**三元组，准备了训练集和验证集。****

******boolQ** 数据集具有以下格式-****

```
**{
  **"question"**: "is france the same timezone as the uk",
  **"passage"**: "At the Liberation of France in the summer of 1944, Metropolitan France kept GMT+2 as it was the time then used by the Allies (British Double Summer Time). In the winter of 1944--1945, Metropolitan France switched to GMT+1, same as in the United Kingdom, and switched again to GMT+2 in April 1945 like its British ally. In September 1945, Metropolitan France returned to GMT+1 (pre-war summer time), which the British had already done in July 1945\. Metropolitan France was officially scheduled to return to GMT+0 on November 18, 1945 (the British returned to GMT+0 in on October 7, 1945), but the French government canceled the decision on November 5, 1945, and GMT+1 has since then remained the official time of Metropolitan France."
  **"answer"**: false,
  **"title"**: "Time in France",
}**
```

****有一段“**短文**，有对应的“**问题**和正确的布尔“**答案**”——对或错。****

****我们会详细讨论你如何-****

1.  ****使用我的**预训练的**模型为任何给定的内容生成**布尔问题**。****
2.  ****使用我的**训练**代码和数据集在你自己的 **GPU** 机器上复制结果。****

# ****训练算法— T5****

****![](img/2e8f91232c6c6156897031d2b9ffb545.png)****

****用[扁平图标](https://www.flaticon.com/authors/freepik)生成的图标****

****T5 是 Google 的一个新的 transformer 模型，它以端到端的方式进行训练，将**文本作为输入**，将修改后的**文本作为输出**。你可以在这里了解更多。****

****它使用在大型文本语料库上训练的文本到文本转换器，在多个 NLP 任务上实现了**最先进的**结果，如摘要、问题回答、机器翻译等。****

****我将“**通道”**和**“答案”**作为**输入**给我的 T5 变压器模型，并训练它生成**“问题”**作为**输出**。****

# ****密码****

****使用**预训练**模型和**训练**具有给定数据的模型的所有代码可在-****

****[](https://github.com/ramsrigouthamg/generate_boolean_questions_using_T5_transformer) [## ramsrigouthamg/generate _ boolean _ questions _ using _ T5 _ transformer

### 使用这个程序，您可以从任何内容中生成布尔型(是/否)问题。一篇详细的媒体博文解释了…

github.com](https://github.com/ramsrigouthamg/generate_boolean_questions_using_T5_transformer) 

# 使用预先训练的模型

Python 文件 [t5_inference.py](https://github.com/ramsrigouthamg/generate_boolean_questions_using_T5_transformer/blob/master/t5_inference.py) 包含了下面给出的所有代码。

首先，安装必要的库-

```
!pip install torch==1.4.0
!pip install transformers==2.9.0
!pip install pytorch_lightning==0.7.5
```

以任何文本/段落作为输入运行**推理**，查看生成的**布尔问题**

上述代码的**输出**为-

```
**Context: ** Months earlier, Coca-Cola had begun “Project Kansas.” It sounds like a nuclear experiment but it was just a testing project for the new flavor. In individual surveys, they’d found that more than 75% of respondents loved the taste, 15% were indifferent, and 10% had a strong aversion to the taste to the point that they were angry.**Beam decoding [Most accurate questions] ::**Does coca cola have a kansas flavor?
Is project kansas the same as coca cola?
Is project kansas a new coca cola flavor?**TopKP decoding [Not very accurate but more variety in questions] ::**Does coca cola have a koala flavor?
Is kakao the same as project kansas?
Was project ksoda a real thing?**Time elapsed  1.2351574897766113**
```

# 训练你自己的模型

所有用于训练的训练代码和数据集都可以在提到的 Github repo 中获得。我们将经历我用来训练模型的步骤。

# 1.数据准备

文件[boolQ _ prepare _ train _ validation _ dataset . ipynb](https://github.com/ramsrigouthamg/generate_boolean_questions_using_T5_transformer/blob/master/boolQ_prepare_train_validation_dataset.ipynb)包含准备训练和验证数据集的所有代码。我将 boolQ 数据集作为 JSON 文件，并将其转换为 csv 文件。

# 2.培养

感谢 [Suraj Patil](https://madewithml.com/@patil-suraj/) 给了我们一个神奇的 [Colab 笔记本](https://colab.research.google.com/drive/176NSaYjc2eeI-78oLH_F9-YV3po3qQQO?usp=sharing#scrollTo=SDVQ04fGRb1v)来训练 T5 完成任何文本到文本的任务。我从 Colab 笔记本上借用了大部分训练代码，只更改了数据集类和训练参数。我使 dataset 类适应了我们的 boolQ 数据集。

培训代码可作为培训[使用。](https://github.com/ramsrigouthamg/generate_boolean_questions_using_T5_transformer/blob/master/train.py)Github 回购中的 py。

你需要做的就是**在任一台 **GPU** 机器上克隆**repo，安装 **requirements.txt** ，运行 **train.py** 来训练 T5 模型。

在 **p2.xlarge** (AWS ec2)上训练该模型 4 个时期(默认)需要大约 5-6 个小时。

数据集类如下所示—

关键是我们如何向 T5 模型培训师提供我们的输入和输出。我将“**通道”**和**“答案”**作为**输入**给我的 T5 变压器模型，并训练它生成**“问题”**作为**输出**，如下所示-

**输入格式到 T5 进行训练**

```
**truefalse**: yes **passage**: At the Liberation of France in the summer of 1944, Metropolitan France kept GMT+2 as it was the time then used by the Allies (British Double Summer Time). In the winter of 1944--1945, Metropolitan France switched to GMT+1, same as in the United Kingdom, and switched again to GMT+2 in April 1945 like its British ally. In September 1945, Metropolitan France returned to GMT+1 (pre-war summer time), which the British had already done in July 1945\. Metropolitan France was officially scheduled to return to GMT+0 on November 18, 1945 (the British returned to GMT+0 in on October 7, 1945), but the French government canceled the decision on November 5, 1945, and GMT+1 has since then remained the official time of Metropolitan France. **</s>**
```

**输出格式到 T5 进行训练**

```
Is france the same timezone as the uk? **</s>**
```

**注:**文本" **truefalse** : yes "或" **truefalse** : no "应生成一个**适当的**布尔问题，其答案为文本中给出的" yes "或" no "。但是我尝试训练 T5 时，效果并不好。所以确保你**不要依赖**对生成的布尔问题给出的**答案**的初始标记“是”或“否”。

总数据集只有大约 **~10k** 个样本，因此生成的问题质量有时**不是很好**。请注意。**** 

**祝 NLP 探索愉快，如果你喜欢它的内容，请随时在 Twitter 上找到我。**

**如果你想学习使用变形金刚的现代自然语言处理，看看我的课程[使用自然语言处理的问题生成](https://www.udemy.com/course/question-generation-using-natural-language-processing/?referralCode=C8EA86A28F5398CBF763)**