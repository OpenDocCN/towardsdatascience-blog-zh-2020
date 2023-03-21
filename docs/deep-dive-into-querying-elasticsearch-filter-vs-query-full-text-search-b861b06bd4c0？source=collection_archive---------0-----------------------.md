# 深入查询 Elasticsearch。过滤器与查询。全文搜索

> 原文：<https://towardsdatascience.com/deep-dive-into-querying-elasticsearch-filter-vs-query-full-text-search-b861b06bd4c0?source=collection_archive---------0----------------------->

## 或者如何理解缺少哪些官方文档

![](img/0e2019ccf41334cea219a485efec6bfd.png)

由[埃胡德·纽豪斯](https://unsplash.com/@paramir?utm_source=medium&utm_medium=referral)在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 拍摄的照片

如果我必须用一句话来描述 Elasticsearch，我会说:

> 当**搜索**与**分析**大规模相遇时(接近实时)

Elasticsearch 是目前十大最受欢迎的开源技术之一。公平地说，它结合了许多本身并不独特的关键特性，然而，当结合起来时，它可以成为最好的搜索引擎/分析平台。

更准确地说，Elasticsearch 之所以如此受欢迎，是因为它结合了以下特点:

*   使用相关性评分进行搜索
*   全文搜索
*   分析(聚合)
*   无模式(对数据模式没有限制)，NoSQL，面向文档
*   丰富的数据类型选择
*   水平可伸缩
*   容错的

为我的副业项目与 Elasticsearch 一起工作时，我很快意识到官方文档看起来更像是从所谓的文档中“压榨”出来的。我不得不在谷歌上搜索了很多次，所以我决定收集这篇文章中的所有信息。

在本文中，我将主要写查询/搜索 Elasticsearch 集群。有许多不同的方法可以实现或多或少相同的结果，因此，我将尝试解释每种方法的利弊。

更重要的是，我将向您介绍两个重要的概念——**查询和过滤上下文**——它们在文档中没有得到很好的解释。我会给你一套规则，告诉你什么时候用哪种方法更好。

如果读完这篇文章后，我想让你记住一件事，那就是:

> 查询时真的需要给文档打分吗？

## 查询上下文与过滤上下文

当我们谈论 Elasticsearch 时，总会有一个相关性分数。**相关性分数是一个严格的正数，表示每个文档满足搜索标准的程度。**该分数相对于分配的最高分数，因此，分数越高，文档与搜索标准的相关性越好。

但是，过滤器和查询是两个不同的概念，在编写查询之前，您应该能够理解这两个概念。

一般来说，**过滤上下文**是一个是/否选项，其中每个**文档要么匹配查询，要么不匹配**。一个很好的例子是 SQL `WHERE`后跟一些条件。SQL 查询总是返回严格符合条件的行。SQL 查询不可能返回不明确的结果。

> **过滤器被自动缓存，不会影响相关性分数。**

另一方面，elastic search**查询上下文**向您展示**每个文档与您的需求的匹配程度**。为此，查询使用分析器来查找最佳匹配。

经验法则是**对**使用过滤器:

*   是/否搜索
*   搜索精确值(数字、范围和关键字)

**使用查询进行**:

*   不明确的结果(某些文档比其他文档更适合)
*   全文搜索

> 除非你需要相关性分数或全文搜索，否则请尽量使用过滤器。滤镜“更便宜”。

另外， **Elasticsearch 会自动缓存过滤器**的结果。

在第 1 部分。第二。我将谈到查询(可以转换成过滤器)。请不要混淆**结构化 vs 全文**和**查询 vs 过滤器**——那是两回事。

# 1.结构化查询

结构化查询也称为**术语级查询**，是一组检查文档是否应该被选择的查询方法。因此，在许多情况下并不真正需要相关性分数——文档要么匹配，要么不匹配(尤其是数字)。

术语级查询仍然是查询，因此它们将返回分数。

**术语查询**

返回字段值与条件完全匹配的文档。术语查询在某种程度上是 SQL `select * from table_name where column_name =...`的替代

术语查询直接进入倒排索引，这使得它很快。当处理文本数据时，最好仅对`keyword`字段使用`term`。

```
GET /_search
{
    "query": {
        "term": {
            "<field_name>": {
                "value": "<your_value>"
            }
        }
    }
}
```

**默认情况下，术语查询是在查询上下文中运行的，因此，它会计算得分。即使返回的所有文档的分数都相同，也需要额外的计算能力。**

## 带过滤器的术语查询

如果我们想加速术语查询并将其缓存，那么它应该被包装在一个`constant_score`过滤器中。

还记得经验法则吗？如果您不关心相关性分数，请使用此方法。

```
GET /_search
{
    "query": {
        "**constant_score**" : {
            "**filter**" : {
                "term" : {"<field_name>" : "<your_value>"}
            }
        }
    }
}
```

现在，该查询不计算任何相关性分数，因此速度更快。而且，它是自动缓存的。

快速建议——对于`text`字段，使用`match`而不是`term`。

请记住，术语查询直接指向倒排索引。术语查询接受您提供的值，并按原样搜索它，这就是为什么它非常适合查询没有任何转换就存储的`keyword`字段。

**条款查询**

您可能已经猜到，术语查询允许您返回与**至少一个**精确术语匹配的文档。

术语查询在某种程度上是 SQL 的替代品`select * from table_name where column_name is in...`

理解 Elasticsearch 中的查询字段可能是一个列表很重要，例如`{ "name" : ["Odin", "Woden", "Wodan"] }`。如果您执行的`terms`查询包含以下名称之一，那么该记录将被匹配——它不必匹配字段中的所有值，而只需匹配一个值。

```
GET /_search
{
    "query" : {
        "terms" : {
            "name" : ["Frigg", "Odin", "Baldr"]
        }
    }
}
```

## 术语集查询

与术语查询相同，但这次您可以指定在查询的字段中应该有多少确切的术语。

您可以指定需要匹配的数量——一个、两个、三个或全部。但是，这个数字是另一个数字字段。因此，每个文档都应该包含这个编号(特定于这个特定的文档)。

## 范围查询

返回查询字段值在定义范围内的文档。

相当于 SQL `select * from table_name where column_name is between...`

范围查询有自己的语法:

*   `gt`大于
*   `gte`大于或等于
*   `lt`小于
*   `lte`小于或等于

字段值应≥ 4 且≤ 17 的示例:

```
GET _search
{
    "query": {
        "range" : {
            "<field_name>" : {
                "gte" : 4,
                "lte" : 17
            }
        }
    }
}
```

范围查询也适用于日期。

## 正则表达式、通配符和前缀查询

Regexp 查询返回字段与您的[正则表达式](https://en.wikipedia.org/wiki/Regular_expression)匹配的文档。

如果你从未使用过正则表达式，那么我强烈建议你至少了解一下它是什么以及什么时候可以应用它。

Elasticsearch 的 regexp 是 Lucene 的。它有标准的保留字符和运算符。如果你已经使用过 Python 的`re`包，那么在这里使用它应该不成问题。唯一不同的是 Lucene 的引擎不支持`^`、`$`等锚点运算符。

您可以在官方文档中找到 regexp 的完整列表。

除了 regexp 查询，Elsticsearch 还有通配符和前缀查询。从逻辑上讲，这两个只是正则表达式的特例。

不幸的是，我找不到关于这 3 个查询的性能的任何信息，因此，我决定自己测试一下，看看是否有什么显著的不同。

在使用`rehexp`和`wildcard`查询比较通配符查询时，我没有发现任何性能差异。如果你知道有什么不同，请， [tweet](https://twitter.com/bugaralife) me。

## **已有查询**

由于 Elasticsearch 是无模式的(或者没有严格的模式限制)，当不同的文档有不同的字段时，这是一种相当常见的情况。因此，知道一个文档是否有某个字段是非常有用的。

> [Exists 查询返回包含字段索引值的文档](https://www.elastic.co/guide/en/elasticsearch/reference/7.x/query-dsl-exists-query.html)

```
GET /_search
{
    "query": {
        "exists": {
            "field": "<your_field_name>"
        }
    }
}
```

# 2.全文查询

全文查询适用于非结构化文本数据。**全文查询利用了分析器。**因此，我将简要概述 Elasticsearch 的分析器，以便我们可以更好地分析全文查询。

## 弹性搜索的分析管

每次将`text`类型的数据插入到 Elasticsearch 索引中时，都会对其进行分析，然后存储在倒排索引中。因为 analyzer 还适用于全文搜索，所以根据您对 analyzer 的配置，它会影响您的搜索功能。

***分析仪管道*** 由三个阶段组成:

字符过滤器(0+) →记号赋予器(1) →记号过滤器(0+)

总有一个**记号赋予器**和**零个或多个字符&记号过滤器**。

***1)字符过滤器*** 按原样接收文本数据，然后它可能在数据被标记化之前对其进行预处理。字符过滤器用于:

*   替换匹配给定正则表达式的字符
*   替换匹配给定字符串的字符
*   清除 HTML 文本

***2)记号赋予器*** 将字符过滤后接收的文本数据(如果有)分解成记号。例如，`whitespace` tokenizer 简单地通过空白字符(这不是标准的)来分隔文本。因此，`Wednesday is called after Woden.`将被拆分成`Wednesday,` `is,` `called,` `after,` `Woden.`。有许多[内置标记器](https://www.elastic.co/guide/en/elasticsearch/reference/7.x/analysis-tokenizers.html)可用于创建定制分析器。

***标准记号赋予器*** 在删除标点符号后用空格将文本断开。对于绝大多数语言来说，这是最中性的选择。

除了标记化， ***标记化器*** 还执行以下操作:

*   跟踪代币顺序，
*   注意每个单词的开头和结尾
*   定义令牌的类型

***3)令牌过滤器*** 对令牌应用一些变换。您可以选择将许多不同的令牌过滤器添加到您的分析器中。一些最受欢迎的是:

*   小写字母
*   词干分析器(适用于多种语言！)
*   删除重复项
*   转换为等效的 ASCII 码
*   模式的变通方法
*   令牌计数限制
*   令牌的停止列表(从停止列表中删除令牌)

现在，当我们知道分析器由什么组成时，我们可能会考虑如何处理我们的数据。然后，我们可能[通过选择合适的组件来组成一个最适合我们情况的分析器](https://www.elastic.co/guide/en/elasticsearch/reference/7.x/analyzer-anatomy.html)。分析器**可以在每个字段的基础上指定。**

理论够了，我们来看看默认分析器是怎么工作的。

**标准分析仪**为默认分析仪。它有 0 字符过滤器，标准记号，小写和停止记号过滤器。您可以随心所欲地构建您的定制分析器，但是也有一些内置分析器。

一些最有效的开箱即用分析器是语言分析器，它们利用每种语言的特性进行更高级的转换。因此，如果您事先知道数据的语言，我建议您从标准分析器切换到数据语言之一。

**全文查询将使用索引数据时使用的同一分析器。**更准确地说，您的查询文本将与搜索字段中的文本数据经历相同的转换，因此两者处于同一级别。

## 匹配查询

匹配查询是查询`text`字段的标准查询。

除了`text`类型字段之外，我们可以将`match`查询称为`term`查询的等效查询(而在处理文本数据时，`term`应该只用于`keyword`类型字段)。

```
GET /_search
{
  "query" : {
    "match" : {
      "<text_field>" {
        "query" : "<your_value>"
      }
    }
  }
}
```

默认情况下，传递给`query`参数(必需的)的字符串将由应用于搜索字段的分析器进行处理。除非您使用`analyzer`参数自己指定分析仪。

当您指定要搜索的短语时，它会被分析，结果总是一组标记。默认情况下，Elasticsearch 将在所有这些令牌之间使用`OR`操作符。这意味着至少应该有一个匹配——尽管匹配越多，得分越高。您可以在`operator`参数中将其切换到`AND`。在这种情况下，要返回文档，必须在文档中找到所有的标记。

如果您希望在`OR`和`AND`之间有一些东西，您可以指定`[minimum_should_match](https://www.elastic.co/guide/en/elasticsearch/reference/7.x/query-dsl-minimum-should-match.html)`参数，该参数指定应该匹配的子句的数量。它可以用数字和百分比来指定。

`fuzziness`参数(可选)允许你省略错别字。 [Levenshtein 距离](https://en.wikipedia.org/wiki/Levenshtein_distance)用于计算。

如果您将`match`查询应用到`keyword`字段，那么它将执行与`term`查询相同的操作。更有趣的是，如果您将存储在倒排索引中的令牌的确切值传递给`term`查询，那么它将返回与`match`查询完全相同的结果，但速度更快，因为它将直接进入倒排索引。

## 匹配短语查询

与`match`相同，但顺序和接近度很重要。匹配查询不知道顺序和接近度，因此，只可能用不同类型的查询来实现短语匹配。

```
GET /_search
{
    "query": {
        "match_phrase" : {
            "<text_field>" : {
                "query" : "<your_value>",
                "slop" : "0"
            }
        }
    }
}
```

`match_phrase`查询有`slop`参数(默认值为 0 ),负责跳过术语。因此，如果指定`slop`等于 1，那么短语中的一个单词可能会被省略。

## 多匹配查询

多匹配查询的工作与`match`相同，唯一的区别是它应用于多个字段。

```
GET /_search
{
  "query": {
    "multi_match" : {
      "query":    "<your_value>", 
      "fields": [ "<text_field1>", "<text_field2>" ] 
    }
  }
}
```

*   可以使用通配符指定字段名称
*   默认情况下，每个字段的权重相等
*   每个字段对分数的贡献可以增加
*   如果在`fields`参数中没有指定字段，则将搜索所有符合条件的字段

`multi_match`有[不同类型的](https://www.elastic.co/guide/en/elasticsearch/reference/7.x/query-dsl-multi-match-query.html)。我不会在这篇文章中一一描述，但我会解释最流行的:

`best_fields` type(默认)更喜欢在一个字段中找到搜索值的标记的结果，而不是在不同字段中找到搜索标记的结果。

`most_fields`与`best_fields`型有些相反。

`phrase`类型的行为与`best_fields`相似，但搜索的是类似于`match_phrase`的整个短语。

我强烈建议浏览一下官方文件,看看这些领域的分数是如何计算出来的。

# 3.复合查询

复合查询将其他查询包装在一起。复合查询:

*   组合 te 分数
*   更改包装查询的行为
*   将查询上下文切换到筛选器上下文
*   以上任意组合

## 布尔查询

[**布尔查询**](https://www.elastic.co/guide/en/elasticsearch/reference/7.x/query-dsl-bool-query.html) **将其他查询组合在一起。**它是最重要的复合查询。

布尔查询允许您将查询上下文中的搜索与过滤上下文搜索相结合。

布尔查询有四个可以组合在一起的事件(类型):

*   `must`或“必须满足条款”
*   `should`或“如果满足条款，相关性分数的附加分数”
*   `filter`或“必须满足该条款，但不计算相关性分数”
*   `must_not`或“与必须成反比，对相关性得分没有贡献”

`must`和`should` → **查询上下文**

`filter`和`must_not` → **过滤上下文**

对于熟悉 SQL 的人来说`must`是`AND`而`should`是`OR`运算符。因此，必须满足`must`子句中的每个查询。

## 提升查询

对于大多数查询来说，Boosting 查询类似于`boost`参数，但并不相同。提升查询返回匹配`positive`子句的文档，并降低匹配`negative`子句的文档的分数。

## 常数分数查询

正如我们在前面的`term`查询示例中看到的，`constant_score`查询将任何查询转换成关联分数等于`boost`参数(默认为 1)的过滤上下文。

**总结一下**，Elasticsearch 适合现在很多用途，有时候很难理解什么是最好用的工具。

我希望您记住的主要一点是，您并不总是需要使用最先进的功能来解决简单的问题。

如果您不需要相关性分数来检索数据，请尝试切换到过滤器上下文。

此外，理解 Elasticsearch 如何在幕后工作是至关重要的，因此我建议您始终了解您的分析器是做什么的。

在 Elasticsearch 中有更多的查询类型。我试着描述一下用得最多的。我希望你喜欢它。

让我知道你是否愿意阅读另一个帖子，在那里我给出了所有问题的真实例子。

我计划在 Elasticsearch 上发布更多帖子，所以不要错过。

这是一个相当长的，所以如果你到了那里:

```
About meMy name is Artem, I build [newscatcherapi.com](https://newscatcherapi.com/) - ultra-fast API to find news articles by any topic, country, language, website, or keyword.I write about Python, cloud architecture, elasticsearch, data engineering, and entrepreneurship.
```