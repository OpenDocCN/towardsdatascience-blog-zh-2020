# 使用 HuggingFace DistilBERT 的简单快速的问题回答系统——提供了单个和批量推理的例子。

> 原文：<https://towardsdatascience.com/simple-and-fast-question-answering-system-using-huggingface-distilbert-single-batch-inference-bcf5a5749571?source=collection_archive---------22----------------------->

![](img/5ff3a7337af25ac0f142dd5c1c54ddfe.png)

图片来自 [Pixabay](https://pixabay.com/) 并由 [AiArtist Chrome 插件](https://chrome.google.com/webstore/detail/aiartist/odfcplkplkaoehpnclejafhhodpmjjmk)风格化(由我构建)

**问题回答**系统有很多用例，比如通过通读公司文件并找到完美答案来自动回复**客户的查询**。

在这篇博文中，我们将看到如何使用[**hugging face transformers**](https://huggingface.co/transformers/index.html)库中的 [**DistilBERT**](https://huggingface.co/transformers/model_doc/distilbert.html#distilbertforquestionanswering) 实现一个最先进的、超快的、轻量级的问答系统。

## 投入

输入将是一小段我们称之为**上下文、**和一个**问题-**

```
**Context :** The US has passed the peak on new coronavirus cases, President Donald Trump said and predicted that some states would reopen this month.The US has over 637,000 confirmed Covid-19 cases and over 30,826 deaths, the highest for any country in the world.**Question:** What was President Donald Trump's prediction?
```

## 输出

我们的**问答**系统的输出将是来自**上下文**段落的答案，如下所示—

```
**Answer:** some states would reopen this month.
```

## 让我们开始吧:

首先，我们来看看如何回答一个**单题**如上图。然后，我们将看到如何利用**批处理**从上下文中一次回答**多个问题**。

安装最新版本的[变形金刚](https://huggingface.co/transformers/)库-

```
pip install transformers==2.8.0
pip install torch==1.4.0
```

**单项推断:**

下面是用 **DistilBERT** 进行单一推理的代码:

输出将是:

```
**Question** What was President Donald Trump's prediction?**Answer Tokens:**
['some', 'states', 'would', 're', '##open', 'this', 'month']**Answer** :  some states would reopen this month
```

## 批量推断:

现在让我们试着用批量推理做同样的事情，我们试着传递三个问题，并以**批量**的形式得到它们的答案

三个问题是-

```
**1.** What was President Donald Trump's prediction?
**2\.** How many deaths have been reported from the virus?
**3\.** How many cases have been reported in the United States?
```

下面是用 DistilBERT 进行批量推理的代码:

输出将会是—

```
**Context** :  The US has passed the peak on new coronavirus cases, President Donald Trump said and predicted that some states would reopen this month.The US has over 637,000 confirmed Covid-19 cases and over 30,826 deaths, the highest for any country in the world.**Question**:  What was President Donald Trump's prediction?
**Answer**:  some states would reopen this month**Question**:  How many deaths have been reported from the virus?
**Answer**:  30 , 826**Question**:  How many cases have been reported in the United States?
**Answer**:  over 637 , 000
```

如果你对从**上下文**自动生成**问题**感兴趣，而不是对**问题回答**感兴趣，你可以在下面的链接中查看我的各种算法和开源代码-

1.  [是非题生成](https://medium.com/swlh/practical-ai-automatically-generate-true-or-false-questions-from-any-content-with-openai-gpt2-9081ffe4d4c9)
2.  [选择题生成](/practical-ai-automatically-generate-multiple-choice-questions-mcqs-from-any-content-with-bert-2140d53a9bf5)
3.  [为英语语言学习生成代词疑问句](https://medium.com/swlh/practical-ai-generate-english-pronoun-questions-from-any-story-using-neural-coreference-fe958ae1eb21)
4.  [语法 MCQ 一代](https://becominghuman.ai/practical-ai-using-pretrained-bert-to-generate-grammar-and-vocabulary-multiple-choice-questions-d92e4fbeeeaa)

编码快乐！

# 使用自然语言处理的问题生成——教程

我推出了一个非常有趣的 Udemy 课程，名为“使用 NLP 生成问题”,扩展了这篇博文中讨论的一些技术。如果你想看一看，这里是[链接](https://www.udemy.com/course/question-generation-using-natural-language-processing/?referralCode=C8EA86A28F5398CBF763)。

祝 NLP 探索愉快，如果你喜欢它的内容，请随时在 Twitter 上找到我。