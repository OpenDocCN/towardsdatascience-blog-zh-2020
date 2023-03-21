# Vespa.ai 和 CORD-19 公共 API

> 原文：<https://towardsdatascience.com/vespa-ai-and-the-cord-19-public-api-a714b942172f?source=collection_archive---------48----------------------->

## 体验 Vespa 的功能

Vespa 团队一直在马不停蹄地工作，以由[艾伦人工智能研究所](https://allenai.org/)发布的新冠肺炎开放研究数据集(CORD-19)为基础，组装 [cord19.vespa.ai](https://cord19.vespa.ai/) 搜索应用。前端的[和后端的](https://github.com/vespa-engine/cord-19/blob/master/README.md)[都是 100%开源的。后端基于](https://github.com/vespa-engine/sample-apps/tree/master/vespa-cloud/cord-19-search) [vespa.ai](https://vespa.ai/) ，一个强大的开源计算引擎。由于一切都是开源的，你可以以多种方式为项目做贡献。

![](img/89bdf27789b7a8dd4863cbff68492c91.png)

作为用户，您可以使用[前端](http://cord19.vespa.ai)搜索文章，或者使用[公共搜索 API](https://github.com/vespa-engine/cord-19/blob/master/cord-19-queries.md) 执行高级搜索。作为一名开发者，你可以通过向[后端](https://github.com/vespa-engine/sample-apps/tree/master/vespa-cloud/cord-19-search)和[前端](https://github.com/vespa-engine/cord-19/blob/master/README.md)发送请求来改进现有的应用程序，或者你可以派生并创建你自己的应用程序，无论是在本地[还是通过](https://docs.vespa.ai/documentation/vespa-quick-start.html) [Vespa Cloud](https://cloud.vespa.ai/getting-started.html) ，来试验[不同的方式来匹配和排列 CORD-19 文章](/learning-from-unlabelled-data-with-covid-19-open-research-dataset-cded4979f1cf?source=friends_link&sk=44fd9519db937036659d0e43c87310c5)。我写这篇文章的目的是给你一个概述，通过使用 cord19 搜索应用程序公共 API，Vespa 可以完成什么。这只是皮毛，但我希望它能帮助你找到正确的地方，了解更多的可能性。

# 简单查询语言

cord19.vespa.ai 查询接口支持 Vespa [简单查询语言](https://docs.vespa.ai/documentation/reference/simple-query-language-reference.html)，允许您快速执行简单查询。示例:

*   [+新冠肺炎+温度对病毒传播的影响](https://cord19.vespa.ai/search?query=%2Bcovid-19+%2Btemperature+impact+on+viral+transmission):如果你点击这个链接，你将会搜索到包含*新冠肺炎、温度*和短语*对病毒传播的影响*的文章。
*   [+title:" reproduction number "+abstract:MERS](https://cord19.vespa.ai/search?query=%2Btitle%3A%22reproduction+number%22+%2Babstract%3AMERS):该链接将返回标题中包含短语 *reproduction number* 和摘要中包含单词 *MERS* 的文章。
*   [+authors.last:knobel](https://cord19.vespa.ai/search?query=authors.last%3Aknobel) :返回至少有一个作者姓 *knobel 的文章。*

其他资源:

*   更多 cord19 的具体例子可以在 [cord19 API 文档](https://github.com/vespa-engine/cord-19/blob/master/cord-19-queries.md)中找到。
*   简单查询语言文档是查询语法的地方。

# Vespa 搜索 API

除了简单的查询语言，Vespa 还有一个更强大的[搜索 API](https://docs.vespa.ai/documentation/search-api.html) ，它通过名为 YQL 的 [Vespa 查询语言](https://docs.vespa.ai/documentation/query-language.html)来完全控制搜索体验。然后，我们可以通过向搜索端点 *cord19.vespa.ai* 发送 POST 请求来发送广泛的查询。以下是说明该 API 的 python 代码:

```
import requests # Install via 'pip install requests'endpoint = 'https://api.cord19.vespa.ai/search/'
response = requests.post(endpoint, json=**body**)
```

## 按查询术语搜索

让我们分解一个例子来提示您使用 Vespa 搜索 API 可以做些什么:

```
**body** = {
  'yql': 'select title, abstract from sources * where userQuery() and has_full_text=true and timestamp > 1577836800;',
  'hits': 5,
  'query': 'coronavirus temperature sensitivity',
  'type': 'any',
  'ranking': 'bm25'
}
```

**匹配阶段**:上面的 body 参数将为所有匹配`'query'`项中的任何一个(`'type': 'any'`)的文章选择标题和摘要字段，这些文章具有可用的全文(`has_full_text=true`)和大于 1577836800 的时间戳。

**排名阶段**:根据上述标准匹配文章后，Vespa 将根据文章的 [BM25 分数](https://docs.vespa.ai/documentation/reference/bm25.html) ( `'ranking': 'bm25'`)对其进行排名，并根据该排名标准返回前 5 篇文章(`'hits': 5`)。

上面的例子仅仅给出了搜索 API 的一点尝试。我们可以根据自己的需要定制匹配阶段和排名阶段。例如，我们可以使用更复杂的匹配操作符，如 Vespa weakAND，我们可以通过在上面的*正文*中添加`'default-index': 'abstract'`来限制搜索，只在摘要中查找匹配。我们可以通过将`'ranking'`参数更改为[搜索定义文件](https://github.com/vespa-engine/sample-apps/blob/master/vespa-cloud/cord-19-search/src/main/application/searchdefinitions/doc.sd)中可用的[等级配置文件](https://docs.vespa.ai/documentation/ranking.html)之一，在查询时试验不同的等级函数。

其他资源:

*   Vespa 文本搜索教程展示了如何逐步创建文本搜索应用程序。[第 1 部分](https://docs.vespa.ai/documentation/tutorials/text-search.html)展示了如何从头开始创建一个基本的应用程序。[第 2 部分](https://docs.vespa.ai/documentation/tutorials/text-search-ml.html)展示了如何从 Vespa 收集训练数据，并改进 ML 模型的应用。[第 3 部分](https://docs.vespa.ai/documentation/tutorials/text-search-semantic.html)展示了如何通过使用预先训练的句子嵌入开始语义搜索。
*   更多特定于 cord19 应用的 YQL 示例可以在 [cord19 API 文档](https://github.com/vespa-engine/cord-19/blob/master/cord-19-queries.md)中找到。

## 按语义相关性搜索

除了通过查询词进行搜索，Vespa 还支持语义搜索。

```
**body** = {
    'yql': 'select * from sources * where  ([{"targetNumHits":100}]nearestNeighbor(title_embedding, vector));',
    'hits': 5,
    'ranking.features.query(vector)': embedding.tolist(),
    'ranking.profile': 'semantic-search-title',
}
```

**匹配阶段**:在上面的查询中，我们通过使用[最近邻操作符](https://docs.vespa.ai/documentation/reference/query-language-reference.html#nearestneighbor)来匹配至少 100 篇文章(`[{"targetNumHits":100}]`)，这些文章在`title_embedding`和查询嵌入`vector`之间具有最小(欧几里德)距离。

**排序阶段**:匹配后我们可以用多种方式对文档进行排序。在这种情况下，我们使用一个名为`'semantic-search-title'`的特定 rank-profile，它是预定义的，根据标题和查询嵌入之间的距离对匹配的文章进行排序。

标题嵌入是在将文档提供给 Vespa 时创建的，而查询嵌入是在查询时创建的，并通过`ranking.features.query(vector)`参数发送给 Vespa。这个 [Kaggle 笔记本](https://www.kaggle.com/jkb123/semantic-search-using-vespa-ai-s-cord19-index)演示了如何通过使用[塞伯特-NLI 模型](https://huggingface.co/gsarti/scibert-nli)在 cord19 应用中执行语义搜索。

其他资源:

*   [文本搜索教程的第 3 部分](https://docs.vespa.ai/documentation/tutorials/text-search-semantic.html)展示了如何通过使用预先训练的句子嵌入开始语义搜索。
*   进入[排名页面](https://docs.vespa.ai/documentation/ranking.html)了解更多关于排名的一般情况，以及如何在 Vespa 中部署 ML 模型(包括 TensorFlow、XGBoost 等)。