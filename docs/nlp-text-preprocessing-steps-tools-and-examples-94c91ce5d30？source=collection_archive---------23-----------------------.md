# 自然语言处理文本预处理:步骤、工具和例子

> 原文：<https://towardsdatascience.com/nlp-text-preprocessing-steps-tools-and-examples-94c91ce5d30?source=collection_archive---------23----------------------->

## 为自然语言处理任务预处理文本的标准逐步方法。

文本数据无处不在，从你每天的脸书或 Twitter 新闻，到教科书和客户反馈。数据是新的石油，文本是我们需要钻得更深的油井。在我们真正使用油之前，我们必须对它进行预处理，使它适合我们的机器。对于数据也是一样，我们必须清理和预处理数据以符合我们的目的。这篇文章将包括一些简单的方法来清理和预处理文本分析任务的文本数据。

我们将在新冠肺炎推特数据集上模拟这种方法。这种方法有三个主要组成部分:
首先，我们清理和过滤所有非英语的推文/文本，因为我们希望数据的一致性。
其次，我们为复杂的文本数据创建一个简化版本。
最后，我们对文本进行矢量化处理，并保存它们的嵌入内容以供将来分析。

如果您想查看代码:请随意查看[第 1 部分](https://github.com/viethoangtranduong/covid19-tweets/blob/master/1.1.%20clean%20%26%20filter%20english%20tweets.ipynb)、[第 2 部分](https://github.com/viethoangtranduong/covid19-tweets/blob/master/1.2.%20clean%20text%2C%20tags%20%26%20date.ipynb)和[第 3 部分](https://github.com/viethoangtranduong/covid19-tweets/blob/master/1.3.%20clean%20%26%20get_embeddings.ipynb)嵌入[这里](https://github.com/viethoangtranduong/covid19-tweets)的代码。你也可以在这里查看整个项目的 blogpost 和[代码](https://github.com/viethoangtranduong/covid19-tweets) [。](https://github.com/viethoangtranduong/covid19-tweets)

![](img/3819a74b37630bf9512b72936187297e.png)

([来源](https://pixabay.com/illustrations/personal-data-data-click-3914806/))

## **第 1 部分:清洁&过滤器文本**

首先，为了简化文本，我们希望将文本标准化为只有英文字符。此功能将删除所有非英语字符。

```
def clean_non_english(txt):
    txt = re.sub(r'\W+', ' ', txt)
    txt = txt.lower()
    txt = txt.replace("[^a-zA-Z]", " ")
    word_tokens = word_tokenize(txt)
    filtered_word = [w for w in word_tokens if all(ord(c) < 128 for c in w)]
    filtered_word = [w + " " for w in filtered_word]
    return "".join(filtered_word)
```

我们甚至可以通过删除停用词做得更好。停用词是英语句子中出现的对意思没有多大贡献的常用词。我们将使用 nltk 包来过滤停用词。由于我们的主要任务是使用词云可视化推文的共同主题，这一步是必要的，以避免常见的词，如“the”、“a”等。
然而，如果你的任务需要完整的句子结构，比如下一个单词预测或语法检查，你可以跳过这一步。

```
import nltk
nltk.download('punkt') # one time execution
nltk.download('stopwords')
from nltk.corpus import stopwords
stop_words = set(stopwords.words('english'))def clean_text(english_txt):
    try:
       word_tokens = word_tokenize(english_txt)
       filtered_word = [w for w in word_tokens if not w in stop_words]
       filtered_word = [w + " " for w in filtered_word]
       return "".join(filtered_word)
    except:
       return np.nan
```

对于 tweets，在清理之前我们需要考虑一个特殊的特性:提及。您的数据可能有(或没有)这样的特殊特性，这是具体情况具体分析，而不是普遍要求。因此，在盲目清理和预处理之前，要充分了解您的数据！

```
def get_mention(txt):
    mention = []
    for i in txt.split(" "):
        if len(i) > 0 and i[0] == "@":
            mention.append(i)
            return "".join([mention[i] + ", " if i != len(mention) - 1 else mention[i] for i in range(len(mention))]
```

之前，我们清理非英语字符。现在，我们移除非英语文本(语义上)。Langdetect 是一个 python 包，允许检查文本的语言。它是 Google 的[语言检测](https://code.google.com/archive/p/language-detection/)库从 Java 到 Python 的直接端口。

```
from langdetect import detect
def detect_lang(txt):
    try:
        return detect(txt)
    except:
        return np.nan
```

然后我们过滤掉所有非英语的列。

第一部分的所有代码都可以在[这里](https://github.com/viethoangtranduong/covid19-tweets/blob/master/1.1.%20clean%20%26%20filter%20english%20tweets.ipynb)找到。

## 第 2 部分:简化复杂的数据—位置？

对于数字数据，好的处理方法是缩放、标准化和规范化。这个[资源](/scale-standardize-or-normalize-with-scikit-learn-6ccc7d176a02)有助于理解并将这些方法应用于您的数据。在这篇文章的范围内，我不会进一步讨论，因为其他[资源](/scale-standardize-or-normalize-with-scikit-learn-6ccc7d176a02)已经做了很好的工作。

对于分类数据，有许多方法。两种名义上的方法是标签编码器(为每个标签分配不同的数字)和一种热编码(用 0 和 1 的向量表示)。关于这些分类值的更多细节可以在[这里](/all-about-categorical-variable-encoding-305f3361fd02)找到。这个[资源](/all-about-categorical-variable-encoding-305f3361fd02)非常丰富，比我提到的这两种编码类型更多。

本帖将介绍一些降低数据复杂度的方法，尤其是位置数据。在我的数据集中，有一个位置栏，有作者的地址。然而，我不能对这些原始数据进行太多的分析，因为它们太杂乱和复杂了(有城市、县、州、国家)。因此，我们可以对文本进行标准化，并将其简化到“国家”级别(如果您感兴趣，也可以是州)。处理位置数据的包是 [geopy](https://pypi.org/project/geopy/) 。它可以识别正确的地址并将这些位置重新格式化为标准格式。然后，您可以选择保留您需要的任何信息。对我来说，国家已经够体面了。

```
from geopy.geocoders import Nominatim
geolocator = Nominatim(user_agent="twitter")def get_nation(txt):
    try:
        location = geolocator.geocode(txt)
        x = location.address.split(",")[-1]
        return x
    except:
        return np.nan
```

Python 有这么多包，太牛逼了。我相信你总能找到满足你特定需求的东西，就像我处理我杂乱的位置数据一样。祝你好运，简化这些杂乱的数据。

第二部分的代码可以在[这里](https://github.com/viethoangtranduong/covid19-tweets/blob/master/1.2.%20clean%20text%2C%20tags%20%26%20date.ipynb)找到。

## 第 3 部分:矢量化和嵌入

文本矢量化是将文本转换成值的向量来表示它们的含义。早期，我们有一种热门的编码方法，它使用一个向量，这个向量的大小是我们的词汇量，在文本出现的地方取值 1，在其他地方取值 0。如今，我们有更先进的方法，如[空间](https://spacy.io/usage/vectors-similarity)、[手套](https://nlp.stanford.edu/projects/glove/)，甚至[伯特](https://github.com/google-research/bert)嵌入。对于这个项目的范围，我将向您介绍 GloVe in python 和 Jupiter 笔记本。

首先，我们下载嵌入。你可以在这里手动下载或者直接在笔记本上下载。

```
!wget [http://nlp.stanford.edu/data/glove.6B.zip](http://nlp.stanford.edu/data/glove.6B.zip)
!unzip glove*.zip
```

然后，我们创建一个函数来矢量化每个数据点。这个句子是每个单词的意思表示。对于空句子，我们将其默认为零向量。

```
def vectorize(value, word_embeddings, dim = 100):
    sentences = value.to_list()
    sentence_vectors = []
    for i in sentences:
        if len(i) != 0:
            v = sum([word_embeddings.get(w, np.zeros((dim,))) for w in      i.split()])/(len(i.split())+0.001)
        else:
            v = np.zeros((dim,))
        sentence_vectors.append(v)
    sentence_vectors = np.array(sentence_vectors)
    return sentence_vectors
```

最后，我们对整个数据集进行矢量化，并将矢量化的 numpy 数组保存为一个文件，这样我们就不必在每次运行代码时都重复这个过程。矢量化版本将保存为 numpy 数组，格式为。npy 文件。Numpy 包便于存储和处理海量数组数据。

作为我个人的标准实践，我尝试在每个部分之后将所有数据保存为单独的文件，以便更灵活地评估数据和修改代码。

```
def vectorize_data(data = data, value = 'english_text', dim = 100):
    # Extract word vectors
    word_embeddings = {}
    f = open('glove.6B.{}d.txt'.format(str(dim)), encoding='utf-8')
    for line in f:
        values = line.split()
        word = values[0]
        coefs = np.asarray(values[1:], dtype='float32')
        word_embeddings[word] = coefs
    f.close() text_vec = vectorize(data[value], word_embeddings, dim)
    np.save("vectorized_{}.npy".format(str(dim)), text_vec)
    print("Done. Data:", text_vec.shape)
    return True
```

第三部分的代码在这里。

## 结论

数据预处理，特别是文本，可能是一个非常麻烦的过程。你的机器学习工程师工作流程的很大一部分将是这些清理和格式化数据(如果你的数据已经非常干净，那你是幸运的&所有数据工程师都为此做出了贡献)。

本帖所有的[代码](https://github.com/viethoangtranduong/covid19-tweets)都很抽象，可以应用到很多数据项目中(只需要改成列名，应该都可以正常工作)。在笔记本中，我还添加了异常函数来处理失败情况，确保您的代码不会中途崩溃。我希望它对你的项目有所帮助，就像我对我的项目有所帮助一样。

你可以在这里查看项目博客[。](https://github.com/viethoangtranduong/covid19-tweets)

祝你的项目好运，如果有什么需要改变和改进的地方，请告诉我！谢谢，下一篇帖子再见！

参考资料:
黑尔，J. (2020 年 2 月 21 日)。使用 Scikit-Learn 进行扩展、标准化或规范化。检索自[https://towards data science . com/scale-standard-or-normalize-with-scikit-learn-6 CCC 7d 176 a 02](/scale-standardize-or-normalize-with-scikit-learn-6ccc7d176a02)
KazAnova。(2017 年 9 月 13 日)。包含 160 万条推文的 Sentiment140 数据集。检索自[https://www.kaggle.com/kazanova/sentiment140](https://www.kaggle.com/kazanova/sentiment140)
普雷达，G. (2020 年 8 月 30 日)。COVID19 推文。检索自 https://www.kaggle.com/gpreda/covid19-tweets(2020 年 4 月 25 日)。关于分类变量编码的一切。检索自[https://towardsdatascience . com/all-about-category-variable-encoding-305 f 3361 FD 02](/all-about-categorical-variable-encoding-305f3361fd02)