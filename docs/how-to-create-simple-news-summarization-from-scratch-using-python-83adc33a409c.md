# 如何使用 Python 从头开始创建简单的新闻摘要

> 原文：<https://towardsdatascience.com/how-to-create-simple-news-summarization-from-scratch-using-python-83adc33a409c?source=collection_archive---------26----------------------->

## 新闻摘要入门

![](img/766200f14dcb25cca24688f4fd5b6be8.png)

照片由 [Aaron Burden](https://unsplash.com/@aaronburden?utm_source=medium&utm_medium=referral) 在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

如果你是机器学习领域的新手，特别是自然语言处理(NLP ),你听说过自动新闻摘要，并且对它感兴趣。你一定在想，比如“如何做出一个好的模型？”，“我该学什么？”以及“我应该从哪里开始？”。

然后你开始搜索哪些方法有利于自动摘要，并找到诸如[](https://huggingface.co/transformers/model_doc/bart.html)****，**的方法，然后你尝试预先训练的模型，并对其结果留下深刻印象。那么你有兴趣为你的母语训练模型。但有一个问题，你生活在第三世界国家，没有预先训练好的模型可用，或者你没有任何超级计算机可以运行，或者你甚至没有找到你的语言的数据集，你也懒得单独给它贴上标签。**

**所以在这篇文章中，我会给你一个简单的方法来制作简单的新闻摘要。首先，让我们谈谈新闻摘要的方法。**

**一般来说，新闻摘要方法分为两种，即提取的和抽象的。提取摘要意味着识别文本的重要部分，并使其从原始文本中逐字产生句子的子集；而抽象概括在使用先进的自然语言技术解释和检查文本之后，以新的方式再现重要的材料，以产生新的、更短的文本，该文本传达来自原始文本的最关键的信息。**

**因为我们最初的目标是做一个简单的摘要，这里我们将使用提取摘要的方法。开始吧，如果想看这篇文章的完整代码，请访问我的 [**github**](https://github.com/fahmisalman/Summarizer-Indo) 。**

**首先，导入将要使用的包**

```
import numpy as np
import nltk
import re
```

**这里我们使用了 [**NumPy**](https://numpy.org) ， [**nltk**](https://www.nltk.org) ，以及 [**re**](https://docs.python.org/3/library/re.html) 库。NumPy 是用于数值计算的库，nltk 是广泛用于 NLP 的库，re 是用于正则表达式的库。**

**之后，我们定义将要执行的预处理功能。它旨在集中单词并消除不需要的字符。这里有 3 个预处理功能，即:**

*   **大小写折叠:将所有字母改为小写**
*   **清除:删除所有标点和数字，仅保留字母字符**
*   **标记化:将句子转换成标记**

**下面是预处理过程的代码**

```
def casefolding(sentence):
    return sentence.lower()def cleaning(sentence):
    return re.sub(r'[^a-z]', ' ', re.sub("’", '', sentence))def tokenization(sentence):
    return sentence.split()
```

**此外，您还可以添加其他功能，如停用词移除、词干提取或词汇化。但是，因为每种语言的用法可能不同，所以本文将不解释该功能，但我提供的 Github 链接中有一个示例。**

**接下来，我们还需要一个函数将整个故事转换成句子的集合**

```
def sentence_split(paragraph):
    return nltk.sent_tokenize(paragraph)
```

**接下来，我们将定义一个函数来计算文档中每个单词的数量。执行这个过程是为了给单词加权，目的是确定该单词是否有效果。以下是该过程的代码**

```
def word_freq(data):
    w = []
    for sentence in data:
        for words in sentence:
            w.append(words)
    bag = list(set(w))
    res = {}
    for word in bag:
        res[word] = w.count(word)
    return res
```

**然后，我们会做一个函数来计算每个句子的权重。这个过程是为了确定哪一个句子被认为最能代表整个故事。**

```
def sentence_weight(data):
    weights = []
    for words in data:
        temp = 0
        for word in words:
            temp += wordfreq[word]
        weights.append(temp)
    return weights
```

**现在，我们将使用的所有函数都已定义。我们举个案例吧。例如，我将使用一篇来自[](https://www.politico.com)**的文章，标题为“[《纽约时报》向一群愤怒的暴民投降了”。新闻业将会因此而遭殃。](https://www.politico.com/news/magazine/2020/05/14/bret-stephens-new-york-times-outrage-backlash-256494)****

****我们将从手动复制粘贴将网站上的新闻插入变量开始。如果你不想做复制粘贴，对网页抓取感兴趣，可以看我之前的文章《用 Python 做 4 行的 [**网页抓取新闻》。**](/scraping-a-website-with-4-lines-using-python-200d5c858bb1)****

****[](/scraping-a-website-with-4-lines-using-python-200d5c858bb1) [## 用 Python 实现 4 行新闻的网络抓取

### 抓取网站的简单方法

towardsdatascience.com](/scraping-a-website-with-4-lines-using-python-200d5c858bb1) 

```
news = """
IIn a time in which even a virus has become the subject of partisan disinformation and myth-making, it’s essential that mainstream journalistic institutions reaffirm their bona fides as disinterested purveyors of fact and honest brokers of controversy. In this regard, a recent course of action by the New York Times is cause for alarm.On December 27, 2019, the Times published a column by their opinion journalist Bret Stephens, “The Secrets of Jewish Genius,” and the ensuing controversy led to an extraordinary response by the editors.Stephens took up the question of why Ashkenazi Jews are statistically overrepresented in intellectual and creative fields. This disparity has been documented for many years, such as in the 1995 book Jews and the New American Scene by the eminent sociologists Seymour Martin Lipset and Earl Raab. In his Times column, Stephens cited statistics from a more recent peer-reviewed academic paper, coauthored by an elected member of the National Academy of Sciences. Though the authors of that paper advanced a genetic hypothesis for the overrepresentation, arguing that Ashkenazi Jews have the highest average IQ of any ethnic group because of inherited traits, Stephens did not take up that argument. In fact, his essay quickly set it aside and argued that the real roots of Jewish achievement are culturally and historically engendered habits of mind.Nonetheless, the column incited a furious and ad hominem response. Detractors discovered that one of the authors of the paper Stephens had cited went on to express racist views, and falsely claimed that Stephens himself had advanced ideas that were “genetic” (he did not), “racist” (he made no remarks about any race) and “eugenicist” (alluding to the discredited political movement to improve the human species by selective breeding, which was not remotely related to anything Stephens wrote).It would have been appropriate for the New York Times to acknowledge the controversy, to publish one or more replies, and to allow Stephens and his critics to clarify the issues. Instead, the editors deleted parts of the column—not because anything in it had been shown to be factually incorrect but because it had become controversial.Worse, the explanation for the deletions in the Editors’ Note was not accurate about the edits the paper made after publication. The editors did not just remove “reference to the study.” They expurgated the article’s original subtitle (which explicitly stated “It’s not about having higher IQs”), two mentions of Jewish IQs, and a list of statistics about Jewish accomplishment: “During the 20th century, [Ashkenazi Jews] made up about 3 percent of the U.S. population but won 27 percent of the U.S. Nobel science prizes and 25 percent of the ACM Turing awards. They account for more than half of world chess champions.” These statistics about Jewish accomplishments were quoted directly from the study, but they originated in other studies. So, even if the Times editors wanted to disavow the paper Stephens referenced, the newspaper could have replaced the passage with quotes from the original sources.The Times’ handling of this column sets three pernicious precedents for American journalism.First, while we cannot know what drove the editors’ decision, the outward appearance is that they surrendered to an outrage mob, in the process giving an imprimatur of legitimacy to the false and ad hominem attacks against Stephens. The Editors’ Note explains that Stephens “was not endorsing the study or its authors’ views,” and that it was not his intent to “leave an impression with many readers that [he] was arguing that Jews are genetically superior.” The combination of the explanation and the post-publication revision implied that such an impression was reasonable. It was not.Unless the Times reverses course, we can expect to see more such mobs, more retractions, and also preemptive rejections from editors fearful of having to make such retractions. Newspapers risk forfeiting decisions to air controversial or unorthodox ideas to outrage mobs, which are driven by the passions of their most ideological police rather than the health of the intellectual commons.Second, the Times redacted a published essay based on concerns about retroactive moral pollution, not about accuracy. While it is true that an author of the paper Stephens mentioned, the late anthropologist Henry Harpending, made some deplorable racist remarks, that does not mean that every point in every paper he ever coauthored must be deemed radioactive. Facts and arguments must be evaluated on their content. Will the Times and other newspapers now monitor the speech of scientists and scholars and censor articles that cite any of them who, years later, say something offensive? Will it crowdsource that job to Twitter and then redact its online editions whenever anyone quoted in the Times is later “canceled”?Third, for the Times to “disappear” passages of a published article into an inaccessible memory hole is an Orwellian act that, thanks to the newspaper’s actions, might now be seen as acceptable journalistic practice. It is all the worse when the editors’ published account of what they deleted is itself inaccurate. This does a disservice to readers, historians and journalists, who are left unable to determine for themselves what the controversy was about, and to Stephens, who is left unable to defend himself against readers’ worst suspicions.We strongly oppose racism, anti-Semitism and all forms of bigotry. And we believe that the best means of combating them is the open exchange of ideas. The Times’ retroactive censoring of passages of a published article appears to endorse a different view. And in doing so, it hands ammunition to the cynics and obfuscators who claim that every news source is merely an organ for its political coalition."""
```

之后，我们将通过把新闻切割成句子的形式来处理它，然后使用我们上面定义的函数对每个句子进行预处理。

```
sentence_list = sentence_split(news)
data = []
for sentence in sentence_list:
    data.append(tokenization(cleaning(casefolding(sentence))))
data = (list(filter(None, data)))
```

之后，我们将从我们定义的函数中统计文档中每个单词的数量。

```
wordfreq = word_freq(data)
```

然后，从单词计算的结果我们只需要计算每个句子的权重。

```
rank = sentence_rank(data)
```

之后，我们决定要输出多少个主要句子来表示新闻。例如，我将输出 2 个句子，所以这里我用 2 填充 n。然后，系统将选择权重最高的前两个句子。要看你选择多少个句子出现。

```
n = 2
result = ''
sort_list = np.argsort(ranking)[::-1][:n]
for i in range(n):
    result += '{} '.format(sentence_list[sort_list[i]])
```

要显示结果，请使用以下代码

```
print(result)
```

所以结果会是这样的。

```
The editors did not just remove “reference to the study.” They expurgated the article’s original subtitle (which explicitly stated “It’s not about having higher IQs”), two mentions of Jewish IQs, and a list of statistics about Jewish accomplishment: “During the 20th century, [Ashkenazi Jews] made up about 3 percent of the U.S. population but won 27 percent of the U.S. Nobel science prizes and 25 percent of the ACM Turing awards. Detractors discovered that one of the authors of the paper Stephens had cited went on to express racist views, and falsely claimed that Stephens himself had advanced ideas that were “genetic” (he did not), “racist” (he made no remarks about any race) and “eugenicist” (alluding to the discredited political movement to improve the human species by selective breeding, which was not remotely related to anything Stephens wrote).
```

不错吧？虽然它可能不如我们使用最先进的方法，但我们可以很好地理解我们简短新闻的本质。根据我的提示，通过使用这种方法，您可以生成自己的数据集，尽管它仍然有局限性，因为获得的结果并不总是您想要的方式。

祝你好运！**** 

****如果你喜欢这个帖子，我想推荐一篇启发了我 的 [**文章。**](https://medium.com/sciforce/towards-automatic-text-summarization-extractive-methods-e8439cd54715)****