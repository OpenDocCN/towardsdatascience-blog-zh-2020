# 简单的抽象文本摘要，带有预训练的 T5-文本到文本转换转换器

> 原文：<https://towardsdatascience.com/simple-abstractive-text-summarization-with-pretrained-t5-text-to-text-transfer-transformer-10f6d602c426?source=collection_archive---------4----------------------->

## 使用 T5 文本到文本转换器的文本摘要

![](img/4820e1afe552d93d191ead31431966e4.png)

图片来自 [Pixabay](https://pixabay.com/) 并由[艺术 Chrome 插件](https://chrome.google.com/webstore/detail/aiartist/odfcplkplkaoehpnclejafhhodpmjjmk)风格化

T5 是 Google 的一个新的 transformer 模型，它以端到端的方式进行训练，以文本作为输入，以修改后的文本作为输出。你可以在这里了解更多关于[的信息。](https://ai.googleblog.com/2020/02/exploring-transfer-learning-with-t5.html)

它使用在大型文本语料库上训练的文本到文本转换器，在多个自然语言处理任务上实现了最先进的结果，如摘要、问题回答、机器翻译等。

今天我们将看到如何使用 huggingface 的变形金刚库来总结任何给定的文本。T5 是一种抽象的总结算法。这意味着它将在必要时重写句子，而不仅仅是直接从原文中提取句子。

开始之前，请在您的 jupyter 笔记本电脑或 conda 环境中安装这些库:

```
!pip install transformers==2.8.0
!pip install torch==1.4.0
```

## 文本输入:

```
The US has "passed the peak" on new coronavirus cases, President Donald Trump said and predicted that **some states would reopen** this month.**The US has over 637,000 confirmed Covid-19 cases and over 30,826 deaths, the highest for any country in the world.**At the daily White House coronavirus briefing on Wednesday, Trump said new guidelines to reopen the country would be announced on Thursday after he speaks to governors.**"We'll be the comeback kids, all of us," he said.** "We want to get our country back."The Trump administration has previously fixed May 1 as a possible date to reopen the world's largest economy, but the president said some states may be able to return to normalcy earlier than that.
```

## T5 汇总:

```
The us has over 637,000 confirmed Covid-19 cases and over 30,826 deaths. President Donald Trump predicts some states will reopen the country in **april**, he said. "we'll be the comeback kids, all of us," **the president** says.
```

## 分析输出:

如果你看到算法已经智能地总结了提及**四月**虽然它在最初的故事**中从未被提及。**还把**何**换成了**总裁。**缩短并删除句子“超过 30，826 人死亡……”中逗号后的额外信息

## 代码:

与一些用户报告的 Github Gist 无法加载的代码相同:

```
import torch
import json 
from transformers import T5Tokenizer, T5ForConditionalGeneration, T5Config

model = T5ForConditionalGeneration.from_pretrained('t5-small')
tokenizer = T5Tokenizer.from_pretrained('t5-small')
device = torch.device('cpu')

text ="""
The US has "passed the peak" on new coronavirus cases, President Donald Trump said and predicted that some states would reopen this month.

The US has over 637,000 confirmed Covid-19 cases and over 30,826 deaths, the highest for any country in the world.

At the daily White House coronavirus briefing on Wednesday, Trump said new guidelines to reopen the country would be announced on Thursday after he speaks to governors.

"We'll be the comeback kids, all of us," he said. "We want to get our country back."

The Trump administration has previously fixed May 1 as a possible date to reopen the world's largest economy, but the president said some states may be able to return to normalcy earlier than that.
"""

preprocess_text = text.strip().replace("\n","")
t5_prepared_Text = "summarize: "+preprocess_text
print ("original text preprocessed: \n", preprocess_text)

tokenized_text = tokenizer.encode(t5_prepared_Text, return_tensors="pt").to(device)

# summmarize 
summary_ids = model.generate(tokenized_text,
                                    num_beams=4,
                                    no_repeat_ngram_size=2,
                                    min_length=30,
                                    max_length=100,
                                    early_stopping=True)

output = tokenizer.decode(summary_ids[0], skip_special_tokens=True)

print ("\n\nSummarized text: \n",output)

# Summarized output from above ::::::::::
# the us has over 637,000 confirmed Covid-19 cases and over 30,826 deaths. 
# president Donald Trump predicts some states will reopen the country in april, he said. 
# "we'll be the comeback kids, all of us," the president says.
```

上面提到的汇总输出是-

```
The us has over 637,000 confirmed Covid-19 cases and over 30,826 deaths. President Donald Trump predicts some states will reopen the country in **april**, he said. "we'll be the comeback kids, all of us," **the president** says.
```

在上面的代码中需要注意的关键点是，在将文本传递给 T5 摘要生成器之前，我们在文本前面加上了**" summary:"**。

如果你看一下[论文](https://arxiv.org/abs/1910.10683)，对于其他任务，我们需要做的就是在我们的文本前面加上相应的字符串，例如:**“将英语翻译成德语:”**用于翻译任务。

祝 NLP 探索愉快，如果你喜欢它的内容，请随时在 Twitter 上找到我。

如果你想学习使用变形金刚的现代自然语言处理，看看我的课程[使用自然语言处理生成问题](https://www.udemy.com/course/question-generation-using-natural-language-processing/?referralCode=C8EA86A28F5398CBF763)