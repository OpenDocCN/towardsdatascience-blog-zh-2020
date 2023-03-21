# 用弹性搜索设计最佳多语言搜索引擎

> 原文：<https://towardsdatascience.com/designing-an-optimal-multi-language-search-engine-with-elasticsearch-3d2de1b9636d?source=collection_archive---------13----------------------->

## 设计多语言弹性搜索索引的四种不同方法

![](img/883189cf328242e594ab48e9526d8230.png)

乔尔·那仁在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄的照片

当我为 [NewsCatcherAPI](https://newscatcherapi.com/) 设计 Elasticsearch index 时，我遇到的最大问题之一是处理多语言新闻文章。

我知道 Elasticsearch 为最流行的语言预建了分析器。问题是“我如何管理不同语言的文档，以便可以一起搜索(如果需要的话)？”

**重要提示**:在我们的例子中，我们已经用正确的语言标记了每个文档。尽管如此，这并不是本帖中描述的所有方法都必须的。

同样，对于这个帖子设置，让我们假设每个文档(新闻文章)只有两个字段:`**title**`和`**language**`。其中`**language**`是标题的语言。为了简单起见，假设只能有两种不同的语言:英语(`**en**`)和法语(`**fr**`)。

# 为什么要关心语言呢？

每种语言在很多方面都不同(*我会说 4 种语言，所以给我一些学分*)。[词汇化](https://nlp.stanford.edu/IR-book/html/htmledition/stemming-and-lemmatization-1.html#:~:text=Lemmatization%20usually%20refers%20to%20doing,is%20known%20as%20the%20lemma%20.)、[词干化](https://nlp.stanford.edu/IR-book/html/htmledition/stemming-and-lemmatization-1.html#:~:text=Lemmatization%20usually%20refers%20to%20doing,is%20known%20as%20the%20lemma%20.)、[停用词](https://en.wikipedia.org/wiki/Stop_words)。所有这些在每种语言的基础上都是独一无二的。

所以，如果你想让 Elasticsearch 明白“dogs”只是“dog”的复数形式，或者“different”和“different”同根同源——你必须使用[语言特定的分析器](https://www.elastic.co/guide/en/elasticsearch/reference/current/analysis-lang-analyzer.html#analysis-lang-analyzer)。(甚至对于英语来说！)

首先，我将描述我在网上找到的两种方法，并解释为什么我不喜欢它们。然后，我将提出我的解决方案，我们曾在[新闻发布会](https://newscatcherapi.com/)上使用过。最后，我会留下一个链接，链接到一个非常先进的方法，可以自动检测语言。

# 方法 1。多领域

这种方法的思想是使用`[**fields**](https://www.elastic.co/guide/en/elasticsearch/reference/7.x/multi-fields.html)`参数多次索引文本字段。

例如，如果我们想创建一个索引，用标准、英语和法语分析器索引同一个文本字段:

```
PUT your_index_name_here
{
  "mappings": {
    "properties": {
      "title": { 
        "type": "text",
        "fields": {
          "en": { 
            "type":     "text",
            "analyzer": "english"
          },
          "fr": { 
            "type":     "text",
            "analyzer": "french"
          }
        }
      }
    }
  }
}
```

因此，`**title**`变量被索引 3 次。现在，要搜索所有语言，您必须执行多匹配搜索。例如:

```
GET your_index_name_here/_search
{
  "query": {
    "multi_match": {
      "query": "ce n'est pas une méthode optimale",
      "fields": [ 
        "title",
        "title.en",
        "title.fr"
      ],
      "type": "most_fields" 
    }
  }
}
```

## 多字段方法的优势

1.  易于实施
2.  即使数据没有标注语言，也能正常工作

## 多字段方法的缺点

当你只有两种语言时，这是可以接受的，但是假设你有 10 种语言(就像我们一样)。

1.  索引时间较慢。每种语言的索引
2.  更多存储空间。每个语言索引占用存储空间
3.  昂贵的查询。在[阅读更多关于我的另一篇文章](https://codarium.substack.com/p/optimizing-elasticsearch-performance)

前两点可能没那么糟，然而，第三点就糟了。假设你有 10 种语言。要搜索整个数据库，您必须编写一个多匹配查询，该查询必须同时搜索 10 个不同索引的字段(加上索引碎片的数量)。

综上所述，对于 2-3 种语言的索引(和宽松的预算)，这可能是一个可接受的选择。

# 方法二。多指数

在 Stackoverflow 上可以得到的最流行的答案(假设每个文档的语言在索引前是已知的)。

为每种语言创建单独的索引。例如，一个英文文本的索引我们称之为，法文文本的索引为。

然后，如果你知道搜索的语言，你可以把它导向正确的索引。

## 多指数方法的优势

1.  不会多次存储相同的信息

## 多指数方法的缺点

1.  管理多个指数。另外，不同语言的文档不会遵循统一的分布
2.  [从集群的角度来看，索引并不是免费的，因为每个索引都有一定程度的资源开销。](https://www.elastic.co/blog/how-many-shards-should-i-have-in-my-elasticsearch-cluster)
3.  需要通过所有索引来搜索公共字段。

关于最后一点。假设我们有一个时间戳字段，我们想检索本周发表的所有文章。为此，您必须在所有索引上按发布的日期时间字段进行筛选。从技术上讲，这根本不是问题，只需通过通配符从[多个索引中搜索你的字段。](https://www.elastic.co/guide/en/elasticsearch/reference/current/multi-index.html)

例如，如果我们想同时搜索和，只需使用。

但是，这并不比使用多匹配搜索更好。现在是多索引搜索。

# 方法三。使用摄取处理器识别正确的字段

我的策略如下:

1.  创建一个索引
2.  对于每种语言，创建它自己单独的字段(不是子字段)
3.  设置摄取处理器，根据语言参数值设置`**title_{lang}**`字段

## 每种语言都有一个单独的字段

```
PUT your_index_name_here
{
  "mappings": {
    "properties" : {
        "language" : {
          "type" : "keyword"
        }, 
        "title" : {
          "type" : "text"
        },   
        "title_en" : {
          "type" : "text",
          "analyzer" : "english"
        },  
        "title_fr" : {
          "type" : "text",
          "analyzer" : "french"
        }
      }
    }
}
```

我们的源数据没有 nor 字段。因此，我们必须设置摄取节点的管道来解决这个问题。

## 摄取节点

根据[官方文件](https://www.elastic.co/guide/en/elasticsearch/reference/7.x/ingest.html):

> *在实际的文档索引发生之前，使用摄取节点预处理文档。摄取节点拦截批量和索引请求，应用转换，然后将文档传递回索引或批量 API。*

我们将使用一个[设置处理器](https://www.elastic.co/guide/en/elasticsearch/reference/7.x/set-processor.html)根据`**language**`值将`**title**`值“复制”到`**title_en**`或`**title_fr**`。

我们必须编写一个简单的[无痛脚本](https://www.elastic.co/guide/en/elasticsearch/reference/master/modules-scripting-painless.html)来使 set 处理器有条件。

我们创建了一个名为" **langdetect** "的接收管道

```
PUT _ingest/pipeline/langdetect
{
  "description": "copies the text data into a specific field depending on the language field",
  "processors": [
		{
      "set": {
        "if": "ctx.language == 'en'",
        "field": "title_en",
        "value": "{{title}}"
      }
    },
	{
      "set": {
        "if": "ctx.language == 'fr'",
        "field": "title_fr",
        "value": "{{title}}"
      }
    }
   ]
}
```

它有 2 个处理器，将根据`**language**`字段的值设置`**title_en**`和`**title_fr**`字段。

根据我们的流水线，如果`**language**`字段的值等于“en ”,那么将`**title_en**`字段设置为`**title**`字段中的值。因此，英语文本将由标准分析器(`**title**`字段)和英语分析器(`**title_en**`字段)进行分析。

现在，当创建管道时，我们必须将它“附加”到索引上。因此，让我们更新我们的索引设置:

```
PUT /your_index_name_here/_settings
{
    "default_pipeline" : "langdetect"
}
```

## 优势

1.  支持多语言的单一索引

## 缺点

1.  较慢的索引时间
2.  仅在语言已知时有效

对于 [NewsCatcherAPI](https://newscatcherapi.com/) 的例子，当用户想要用英语搜索时，她必须在我们的 API 中将语言参数设置为`**en**`。我们，在后端，将通过`**title_**` + `**{lang}**`进行搜索，在`**en**`的情况下是`**title_en**`。这比那要复杂一点，但应该足以解释这篇博文。

# 方法 4。检测 Elasticsearch 中的语言，然后进行适当的索引

[在 Elasticsearch 中使用语言识别的多语言搜索](https://www.elastic.co/blog/multilingual-search-using-language-identification-in-elasticsearch)

# 结论

当我意识到用 Elasticsearch 管理多语言搜索索引并不容易且显而易见时，有点惊讶。我不得不花很多时间来找出我们用例的最佳情况。

希望这份“小抄”能为你节省一点时间！

如果有不清楚的地方，请在评论中提问。

顺便说一下，如果你需要帮助你的 Elasticsearch 集群/索引/设置，我会提供咨询。

artem[at]news catcher API[dot]com

*原载于*[*https://codarium.substack.com*](https://codarium.substack.com/p/designing-an-optimal-multi-language)*。*