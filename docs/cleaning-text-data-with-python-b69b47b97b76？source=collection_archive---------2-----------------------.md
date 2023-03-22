# 使用 Python 清理文本数据

> 原文：<https://towardsdatascience.com/cleaning-text-data-with-python-b69b47b97b76?source=collection_archive---------2----------------------->

![](img/98d6096dbebf67469ae19f37e1e74955.png)

安妮·斯普拉特在 [Unsplash](https://unsplash.com/?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) 上的照片

## 你需要的只是 NLTK 和 re 库。

数据格式并不总是表格格式。随着我们进入大数据时代，数据以非常多样化的格式出现，包括图像、文本、图表等等。

因为格式相当多样，从一种数据到另一种数据，所以将这些数据预处理成计算机可读的格式是非常必要的。

在本文中，我想向您展示如何使用 Python 预处理文本数据。如题所示，你所需要的就是 NLTK 和 re 库。

为了向您展示这是如何工作的，我将从一个名为[的 Kaggle 竞赛中获取一个数据集。灾难推文 NLP](https://www.kaggle.com/c/nlp-getting-started/overview)。

> 我已经创建了一个谷歌 Colab 笔记本，如果你想跟着我一起。要访问，你可以点击这个链接[这里](https://colab.research.google.com/drive/1DzUJllfwvD0Ct_GxknWCIPygejZohwJY?usp=sharing)

# 该过程

## 将文本小写

在我们开始处理文本之前，最好先把所有的字符都小写。我们这样做的原因是为了避免任何区分大小写的过程。

假设我们想从字符串中删除停用词，我们使用的技术是将非停用词组合成一个句子。如果我们不是小写那些，停止字不能被发现，它将导致相同的字符串。这就是为什么降低文本的大小写是必要的。

在 Python 中做到这一点很容易。代码看起来像这样，

```
**# Example**
x = "Watch This Airport Get Swallowed Up By A Sandstorm In Under A Minute [http://t.co/TvYQczGJdy](http://t.co/TvYQczGJdy)"**# Lowercase the text**
x = x.lower()print(x)**>>> watch this airport get swallowed up by a sandstorm in under a minute** [**http://t.co/tvyqczgjdy**](http://t.co/tvyqczgjdy)
```

## 删除 Unicode 字符

一些推文可能包含 Unicode 字符，当我们以 ASCII 格式看到它时，它是不可读的。大多数情况下，这些字符用于表情符号和非 ASCII 字符。为了消除这一点，我们可以使用这样的代码，

```
**# Example**
x = "Reddit Will Now QuarantineÛ_ [http://t.co/pkUAMXw6pm](http://t.co/pkUAMXw6pm) #onlinecommunities #reddit #amageddon #freespeech #Business [http://t.co/PAWvNJ4sAP](http://t.co/PAWvNJ4sAP)"**# Remove unicode characters**
x = x.encode('ascii', 'ignore').decode()print(x)**>>> Reddit Will Now Quarantine_** [**http://t.co/pkUAMXw6pm**](http://t.co/pkUAMXw6pm) **#onlinecommunities #reddit #amageddon #freespeech #Business** [**http://t.co/PAWvNJ4sAP**](http://t.co/PAWvNJ4sAP)
```

## 删除停用词

这样做之后，我们可以删除属于停用词的单词。停用词是一种对文本意义没有显著贡献的词。因此，我们可以删除这些词。为了检索停用词，我们可以从 NLTK 库中下载一个语料库。这是如何做到这一点的代码，

```
import nltk
nltk.download()
**# just download all-nltk**stop_words = stopwords.words("english")**# Example**
x = "America like South Africa is a traumatised sick country - in different ways of course - but still messed up."**# Remove stop words**
x = ' '.join([word for word in x.split(' ') if word not in stop_words])print(x)**>>> America like South Africa traumatised sick country - different ways course - still messed up.**
```

## 删除提及、标签、链接等术语。

除了我们删除 Unicode 和停用词，还有几个术语我们应该删除，包括提及、标签、链接、标点等等。

要去除这些，如果我们只依赖一个定义好的角色，那是很有挑战性的。因此，我们需要通过使用正则表达式(Regex)来匹配我们想要的术语的模式。

Regex 是一个特殊的字符串，它包含的模式可以匹配与该模式相关的单词。通过使用它，我们可以使用名为 re 的 Python 库搜索或删除那些基于模式的内容。为此，我们可以这样实现它，

```
import re**# Remove mentions**
x = "[@DDNewsLive](http://twitter.com/DDNewsLive) [@NitishKumar](http://twitter.com/NitishKumar)  and [@ArvindKejriwal](http://twitter.com/ArvindKejriwal) can't survive without referring @@narendramodi . Without Mr Modi they are BIG ZEROS"x = re.sub("@\S+", " ", x)print(x)
**>>>      and   can't survive without referring   . Without Mr Modi they are BIG ZEROS****# Remove URL**
x = "Severe Thunderstorm pictures from across the Mid-South [http://t.co/UZWLgJQzNS](http://t.co/UZWLgJQzNS)"x = re.sub("https*\S+", " ", x)print(x)
**>>> Severe Thunderstorm pictures from across the Mid-South****# Remove Hashtags**
x = "Are people not concerned that after #SLAB's obliteration in Scotland #Labour UK is ripping itself apart over #Labourleadership contest?"x = re.sub("#\S+", " ", x)print(x)
**>>> Are people not concerned that after   obliteration in Scotland   UK is ripping itself apart over   contest?****# Remove ticks and the next character**
x = "Notley's tactful yet very direct response to Harper's attack on Alberta's gov't. Hell YEAH Premier! [http://t.co/rzSUlzMOkX](http://t.co/rzSUlzMOkX) #ableg #cdnpoli"x = re.sub("\'\w+", '', x)print(x)
**>>> Notley tactful yet very direct response to Harper attack on Alberta gov. Hell YEAH Premier!** [**http://t.co/rzSUlzMOkX**](http://t.co/rzSUlzMOkX) **#ableg #cdnpoli****# Remove punctuations**
x = "In 2014 I will only smoke crqck if I becyme a mayor. This includes Foursquare."x = re.sub('[%s]' % re.escape(string.punctuation), ' ', x)print(x)
**>>> In 2014 I will only smoke crqck if I becyme a mayor. This includes Foursquare.****# Remove numbers**
x = "C-130 specially modified to land in a stadium and rescue hostages in Iran in 1980... [http://t.co/tNI92fea3u](http://t.co/tNI92fea3u) [http://t.co/czBaMzq3gL](http://t.co/czBaMzq3gL)"x = re.sub(r'\w*\d+\w*', '', x)print(x)
**>>> C- specially modified to land in a stadium and rescue hostages in Iran in ...** [**http://t.co/**](http://t.co/)[**http://t.co/**](http://t.co/)**# Replace the over spaces**
x = "     and   can't survive without referring   . Without Mr Modi they are BIG ZEROS"x = re.sub('\s{2,}', " ", x)print(x)
**>>>  and can't survive without referring . Without Mr Modi they are BIG ZEROS**
```

## 组合它们

在您了解预处理文本的每个步骤之后，让我们将它应用于一个列表。如果你仔细观察这些步骤的细节，你会发现每种方法都是相互关联的。因此，有必要将它应用到一个函数上，这样我们就可以同时按顺序处理它。在我们应用预处理步骤之前，这里是样本文本的预览，

```
**Our Deeds are the Reason of this #earthquake May ALLAH Forgive us all
Forest fire near La Ronge Sask. Canada
All residents asked to 'shelter in place' are being notified by officers. No other evacuation or shelter in place orders are expected
13,000 people receive #wildfires evacuation orders in California 
Just got sent this photo from Ruby #Alaska as smoke from #wildfires pours into a school**
```

我们应该做几个步骤来预处理文本列表。他们是，

1.  创建一个包含所有预处理步骤的函数，它返回一个预处理过的字符串
2.  使用名为 Apply 的方法应用函数，并用该方法链接列表。

代码看起来会像这样，

```
**# # In case of import errors
# ! pip install nltk
# ! pip install textblob**import numpy as np
import matplotlib.pyplot as plt
import pandas as pd
import re
import nltk
import string
from nltk.corpus import stopwords**# # In case of any corpus are missing 
# download all-nltk**
nltk.download()df = pd.read_csv('train.csv')
stop_words = stopwords.words("english")
wordnet = WordNetLemmatizer()def text_preproc(x):
  x = x.lower()
  x = ' '.join([word for word in x.split(' ') if word not in stop_words])
  x = x.encode('ascii', 'ignore').decode()
  x = re.sub(r'https*\S+', ' ', x)
  x = re.sub(r'@\S+', ' ', x)
  x = re.sub(r'#\S+', ' ', x)
  x = re.sub(r'\'\w+', '', x)
  x = re.sub('[%s]' % re.escape(string.punctuation), ' ', x)
  x = re.sub(r'\w*\d+\w*', '', x)
  x = re.sub(r'\s{2,}', ' ', x)
  return xdf['clean_text'] = df.text.apply(text_preproc)
```

这是它的结果，

```
**deeds reason may allah forgive us
forest fire near la ronge sask canada
residents asked place notified officers evacuation shelter place orders expected
 people receive evacuation orders california 
got sent photo ruby smoke pours school**
```

# 最后的想法

这就是如何使用 Python 对文本进行预处理。希望你可以应用它来解决文本数据相关的问题。如果有什么想法，可以在下面评论下来。此外，您可以在 Medium 上跟踪我，以便跟进我的文章。谢谢你。

## 参考

[1][https://docs.python.org/3/library/re.html](https://docs.python.org/3/library/re.html)
【2】[https://www.nltk.org/](https://www.nltk.org/)
【3】[https://www.kaggle.com/c/nlp-getting-started/overview](https://www.kaggle.com/c/nlp-getting-started/overview)