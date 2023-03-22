# 基于词语相似度的文本分类

> 原文：<https://towardsdatascience.com/text-classification-using-word-similarity-2f0c5fc9f365?source=collection_archive---------36----------------------->

![](img/f3393a7eabbe824f74340cc73d77e40c.png)

尼克·舒利亚欣在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上的照片

## NLP / NLTK Wordnet /文本分类

## 利用数据科学发现 FCA 的弱势客户

# 背景

英国金融服务监管机构 FCA 在其 [2020/21 商业计划](https://www.fca.org.uk/publication/business-plans/business-plan-2020-21.pdf)中关注的一个关键领域是公平对待银行客户。继去年夏天的初步磋商后，他们最近更新了指南，以更好地阐明漏洞的驱动因素:

> 我们已经确定了 4 个可能增加漏洞风险的关键驱动因素。它们是:
> - **健康**-影响执行日常任务能力的残疾或疾病。
> - **生活事件** —重大生活事件，如丧亲、失业或关系破裂
> - **复原力** —承受财务或情感冲击的能力低
> - **能力** —对财务知识或理财信心(财务能力)低，以及其他相关领域的能力低，如读写能力或数字技能

随着与 COVID 相关的疾病和失业为这些驱动因素的蓬勃发展创造了肥沃的土壤，我想看看数据科学家如何帮助银行识别此类漏洞，以便他们能够更主动地提供支持服务。

我想象了一份客户和银行之间的电话对话，或者与聊天机器人或客户服务代理的打字对话。并期待 NLP 技术来帮助自动检测这些驱动程序。

**潜在策略:**
**1。单词相似度:**扫描文本段落中的关键词(例如休假)或它们的同义词。
2**2。从标记的例子中导出 n-gram 特征，并使用该模型对未来的文本进行分类。**

鉴于缺乏训练数据，我们采用策略 1。

# 履行

## 1.规范化漏洞驱动因素

首先，我们将漏洞的 4 个驱动因素分解成一个更长的不同类别的列表，然后我们可以在文本中明确地搜索:
1。死亡，
2。冗余，
3。疾病，
等…

## 2.为 NLTK Wordnet 翻译类别

由于我们将使用 NLTK 中可用的相似性评分器，我们需要将这些类别翻译成 Wordnet (NLTK 的词汇数据库/字典)中描述的正确定义。为此，我们可以使用一个简单的函数，给出该单词的同义词列表及其定义:

word_scorer

哪个 for**word _ scorer(‘冗余’)**会输出:

```
[('redundancy.n.01',
  'repetition of messages to reduce the probability of errors in transmission'),
 ('redundancy.n.02', 'the attribute of being superfluous and unneeded'),
 ('redundancy.n.03',
  '(electronics) a system design that duplicates components to provide alternatives in case one component fails'),
 ('redundancy.n.04', 'repetition of an act needlessly')]
```

并因此帮助我们创建我们将搜索的主题字典(这里的关键字是用户友好的主题，值是 Wordnet 友好的表示):

主题 _ 词典

## 3.标记段落

这里有一段话我们可能想分析一下(显然是关于冗余的):

> 我在一个县议会的成人学习部门工作，我的工作由政府拨款支付。当资金用完的时候，财政年度已经过了一半，突然没钱给我发工资了。我被叫去和其他四个人(他们的工作也依赖于资金)开会，这个消息告诉了我们。在裁员最终确定之前，将有 12 周的咨询期。我们可以选择拿着遣散费走人，或者我们可以在议会的另一个部门找份工作。如果我们拒绝提供给我们的工作，我们可能会失去裁员协议。

我们将对其进行标记，这样我们就可以遍历单词标记列表

删除停用词并标记文本

## 4.根据单个主题给文章打分

我们将按照以下步骤实现一个函数:

*   遍历文章中的单词标记，将每个单词输入 wordnet.synsets()以列出单词的所有可能用法(例如，参见上面列出的单词“redundancy”的可能用法)
*   遍历单词的同义词集，并比较它们与感兴趣的主题的高度相似性(我们将使用 [wup_similarity](https://www.geeksforgeeks.org/nlp-wupalmer-wordnet-similarity/) )。
*   如果这个词的任何可能用法与主题有很高的相似度(大于我们定义的相似度阈值)，我们说这个主题已经被提及，然后我们继续下一个词。
*   我们还可以添加 return_hits 选项，这将使我们能够报告哪些单词触发了相似性阈值。

## 5.对照整个主题词典给文章打分

然后，我们可以重用函数来遍历主题字典中的每个主题:

因此计算该段落与每个主题相似度，并返回被认为相似的单词。

将这些函数打包到一个名为 synonym.py 的模块中后，运行最后一个函数就很简单了:

运行 multi_topic_scorer

它输出下面的结果字典，我们可以看到冗余的主题在文章中被提到了 3 次，每次都是因为提到了单词“redundancy ”,尽管它可能是任何其他类似的单词或相似的单词，如果被使用的话，可能会增加分数:

```
 {'disability': (0, []),
 'death': (0, []),
 'health problems': (0, []),
 'being a carer': (0, []),
 'living alone': (0, []),
 'job loss (fired)': (0, []),
 'job loss (redundancy)': (3, ['redundancy', 'redundancy', 'redundancy']),
 'job loss (furlough)': (0, [])}
```

## 6.检查结果

我们还可以使用我们之前提到的 word_scorer 来比较上面列出的单词(在这种情况下，它只是“冗余”)与主题的 wordnet 定义(即。redundancy . n . 02′)，以查看“冗余”一词的哪种用法被认为是相似的

带有相似性得分的 word_scorer

```
[('redundancy.n.01',
  'repetition of messages to reduce the probability of errors in transmission',
  0.25),
 ('redundancy.n.02', 'the attribute of being superfluous and unneeded', 1.0),
 ('redundancy.n.03',
  '(electronics) a system design that duplicates components to provide alternatives in case one component fails',
  0.2222222222222222),
 ('redundancy.n.04', 'repetition of an act needlessly', 0.2222222222222222)]
```

返回的相似性分数指向第二个条目(本身),所以在这种情况下，这是一个多余的测试，但是在一个单词被列为不期望的相似的情况下，运行这个测试可以发现该单词的哪个用法被确切地认为是相似的。

# 结论

*   我们已经创建了一个简单的脚本，它能够根据预先定义的感兴趣的主题对一段文本进行分类(在本例中:银行的脆弱客户)。
*   这种方法不需要带标签的训练数据，因为我们正在计算预先计算的词向量之间的相似性得分。
*   这种方法还能够执行多标签文本分类，其中一段文本(甚至该文本中的每个单词)可以潜在地分配给多个主题，如果它们足够相似的话。这对于识别具有多个漏洞的客户非常重要。

总而言之:简单、优雅、灵活的解决方案！

GitHub 回购包含在下面，欢迎任何反馈。

[](https://github.com/noahberhe/Vulnerable-Customers) [## Noah berhe/弱势客户

### 根据 FCA 指南 4 漏洞的关键驱动因素:健康状况，NLP 如何用于识别易受攻击的客户

github.com](https://github.com/noahberhe/Vulnerable-Customers) 

…感谢您的阅读！