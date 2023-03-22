# 用 PyTorch 和 Torchtext 实现自然语言处理的深度学习

> 原文：<https://towardsdatascience.com/deep-learning-for-nlp-with-pytorch-and-torchtext-4f92d69052f?source=collection_archive---------4----------------------->

## PyTorch 和 Torchtext 教程

## Torchtext 的预训练单词嵌入，数据集 API，迭代器 API，以及带有 Torchtext 和 PyTorch 的训练模型

![](img/0fc0fc494ecdc6dcfc7e860d0fb77dbf.png)

图片由[克拉丽莎·沃森](https://unsplash.com/@clarephotolover)在 [Unsplash](https://unsplash.com/photos/jAebodq7oxk) 上拍摄

PyTorch 是我一直在使用的一个非常棒的深度学习框架。然而，说到 NLP，不知何故我找不到像 *torchvision* 那样好的实用程序库。原来 PyTorch 有这个 *torchtext* ，在我看来，缺少如何使用它的例子，文档[6]可以改进。此外，有一些很好的教程，如[1]和[2]，但是，我们仍然需要更多的例子。

本文的目的是向读者提供关于如何使用 *torchtext* 的示例代码，特别是使用预先训练的单词嵌入、使用数据集 API、使用迭代器 API 进行小批量，以及最后如何结合使用这些来训练模型。

# 使用 Torchtext 预训练单词嵌入

在预训练的单词嵌入中有一些替代方法，如 Spacy [3]，Stanza (Stanford NLP)[4]，Gensim [5]，但在本文中，我想重点介绍使用 *torchtext* 进行单词嵌入。

## 可用单词嵌入

你可以在 *torchtext* 看到[预训练单词嵌入列表。在撰写本文时，支持 3 个预先训练好的单词嵌入类:GloVe、FastText 和 CharNGram，没有关于如何加载的更多细节。这里的](https://pytorch.org/text/vocab.html#pretrained-word-embeddings)[列出了详尽的列表](https://pytorch.org/text/vocab.html#torchtext.vocab.Vocab.load_vectors)，但我有时会花时间去阅读，所以我会在这里列出列表。

```
**charngram.100d
fasttext.en.300d
fasttext.simple.300d
glove.42B.300d
glove.840B.300d
glove.twitter.27B.25d
glove.twitter.27B.50d
glove.twitter.27B.100d
glove.twitter.27B.200d
glove.6B.50d
glove.6B.100d
glove.6B.200d
glove.6B.300d**
```

有两种方法可以加载预训练的单词嵌入:启动单词嵌入对象或使用`Field`实例。

**使用现场实例**

你需要一些玩具数据集来使用它，所以让我们设置一个。

```
df = pd.DataFrame([
    ['my name is Jack', 'Y'],
    ['Hi I am Jack', 'Y'],
    ['Hello There!', 'Y'],
    ['Hi I am cooking', 'N'],
    ['Hello are you there?', 'N'],
    ['There is a bird there', 'N'],
], columns=['text', 'label'])
```

然后我们可以构造`Field` 对象来保存特性列和标签列的元数据。

```
from torchtext.data import Fieldtext_field = Field(
    tokenize='basic_english', 
    lower=True
)label_field = Field(sequential=False, use_vocab=False)# sadly have to apply preprocess manually
preprocessed_text = df['text'].apply(lambda x: text_field.preprocess(x))# load fastext simple embedding with 300d
text_field.build_vocab(
    preprocessed_text, 
    vectors='fasttext.simple.300d'
)# get the vocab instance
vocab = text_field.vocab
```

要获得预训练单词嵌入的真实实例，可以使用

```
vocab.vectors
```

**启动 Word 嵌入对象**

> 对于这些代码中的每一个，它将下载大量的单词嵌入，所以你必须有耐心，不要一次执行下面所有的代码。

*快速文本*

FastText 对象有一个参数:language，它可以是“simple”或“en”。目前他们只支持 300 个嵌入维度，如上面的嵌入列表中所提到的。

```
from torchtext.vocab import FastText
embedding = FastText('simple')
```

*查恩格拉姆*

```
from torchtext.vocab import CharNGram
embedding_charngram = CharNGram()
```

*手套*

手套对象有两个参数:名称和尺寸。您可以查看每个参数支持的可用嵌入列表。

```
from torchtext.vocab import GloVe
embedding_glove = GloVe(name='6B', dim=100)
```

## 使用单词嵌入

使用 *torchtext* API 使用单词嵌入超级简单！假设您已经将嵌入存储在变量`embedding`中，那么您可以像使用 python 的`dict`一样使用它。

```
# known token, in my case print 12
print(vocab['are'])
# unknown token, will print 0
print(vocab['crazy'])
```

正如你所看到的，它已经处理了未知的令牌，没有抛出错误！如果您尝试将单词编码成整数，您会注意到，默认情况下，unknown token 将被编码为`0`，而 pad token 将被编码为`1`。

# 使用数据集 API

假设变量`df`已经如上定义，我们现在通过为特征和标签构建`Field`来准备数据。

```
from torchtext.data import Fieldtext_field = Field(
    sequential=True,
    tokenize='basic_english', 
    fix_length=5,
    lower=True
)label_field = Field(sequential=False, use_vocab=False)# sadly have to apply preprocess manually
preprocessed_text = df['text'].apply(
    lambda x: text_field.preprocess(x)
)# load fastext simple embedding with 300d
text_field.build_vocab(
    preprocessed_text, 
    vectors='fasttext.simple.300d'
)# get the vocab instance
vocab = text_field.vocab
```

> 这里有一点警告，`Dataset.split`可能返回 3 个数据集( **train，val，test** )，而不是定义的 2 个值

# 使用迭代器类进行小型批处理

我没有找到任何现成的`Dataset` API 来加载 pandas `DataFrame`到 *torchtext* 数据集，但是创建一个还是很容易的。

```
from torchtext.data import Dataset, Example ltoi = {l: i for i, l in enumerate(df['label'].unique())}
df['label'] = df['label'].apply(lambda y: ltoi[y])class DataFrameDataset(Dataset):
    def __init__(self, df: pd.DataFrame, fields: list):
        super(DataFrameDataset, self).__init__(
            [
                Example.fromlist(list(r), fields) 
                for i, r in df.iterrows()
            ], 
            fields
        )
```

我们现在可以构建`DataFrameDataset`并用熊猫数据帧初始化它。

```
train_dataset, test_dataset = DataFrameDataset(
    df=df, 
    fields=(
        ('text', text_field),
        ('label', label_field)
    )
).split()
```

然后我们使用`BucketIterator`类来轻松地**构造迷你批处理迭代器**。

```
from torchtext.data import BucketIteratortrain_iter, test_iter = BucketIterator.splits(
    datasets=(train_dataset, test_dataset), 
    batch_sizes=(2, 2),
    sort=False
)
```

**记住使用 sort=False** 否则当你试图迭代`test_iter`时会导致错误，因为我们还没有定义 sort 函数，然而不知何故，默认情况下`test_iter`被定义为排序。

> 小提示:虽然我同意我们应该使用`DataLoader` API 来处理迷你批处理，但是目前**我还没有探索过**如何使用`DataLoader`和 torchtext。

# 培训 PyTorch 模型的示例

让我们使用 1 个嵌入层和 1 个线性层定义一个任意 PyTorch 模型。在当前的例子中，我没有使用预训练的单词嵌入，而是使用新的未训练的单词嵌入。

```
import torch.nn as nn
import torch.nn.functional as F
from torch.optim import Adamclass ModelParam(object):
    def __init__(self, param_dict: dict = dict()):
        self.input_size = param_dict.get('input_size', 0)
        self.vocab_size = param_dict.get('vocab_size')
        self.embedding_dim = param_dict.get('embedding_dim', 300)
        self.target_dim = param_dict.get('target_dim', 2)

class MyModel(nn.Module):
    def __init__(self, model_param: ModelParam):
        super().__init__()
        self.embedding = nn.Embedding(
            model_param.vocab_size, 
            model_param.embedding_dim
        )
        self.lin = nn.Linear(
            model_param.input_size * model_param.embedding_dim, 
            model_param.target_dim
        )

    def forward(self, x):
        features = self.embedding(x).view(x.size()[0], -1)
        features = F.relu(features)
        features = self.lin(features)
        return features
```

然后，我可以很容易地重复如下的训练(和测试)例程。

## 重用预先训练的单词嵌入

很容易将当前定义的模型修改为使用预训练嵌入的模型。

```
class MyModelWithPretrainedEmbedding(nn.Module):
    def __init__(self, model_param: ModelParam, embedding):
        super().__init__()
        self.embedding = embedding
        self.lin = nn.Linear(
            model_param.input_size * model_param.embedding_dim, 
            model_param.target_dim
        )

    def forward(self, x):
        features = self.embedding[x].reshape(x.size()[0], -1)
        features = F.relu(features)
        features = self.lin(features)
        return features
```

我做了 3 行修改。您应该注意到，我已经更改了构造函数输入以接受嵌入。此外，我还将`view`方法改为`reshape` ，并使用 get 操作符`[]` 而不是 call 操作符`()` 来访问嵌入。

```
model = MyModelWithPretrainedEmbedding(model_param, vocab.vectors)
```

# 结论

我已经完成了使用 *torchtext* 在 PyTorch 中处理文本数据的探索。我开始写这篇文章是因为我在使用互联网上现有的教程时遇到了困难。我希望这篇文章也能减少其他人的开销。

写这段代码需要帮助吗？这里有一个谷歌 Colab 的链接。

> [链接到 Google Colab](https://colab.research.google.com/github/ariepratama/python-playground/blob/master/dl-for-nlp-pytorch-torchtext.ipynb)

# 参考

[1]聂，A . torch text 教程。2017.[http://anie.me/On-Torchtext/](http://anie.me/On-Torchtext/)

[2]用 TorchText 教程进行文本分类。[https://py torch . org/tutorials/初学者/text _ 情操 _ngrams_tutorial.html](https://pytorch.org/tutorials/beginner/text_sentiment_ngrams_tutorial.html)

[3]节文档。[https://stanfordnlp.github.io/stanza/](https://stanfordnlp.github.io/stanza/)

[4] Gensim [文档。https://radimrehurek.com/gensim/](https://radimrehurek.com/gensim/)

[5]空间文件。[https://spacy.io/](https://spacy.io/)

[6] Tor [chtext 文档。https://pytorch.org/text/](https://pytorch.org/text/)