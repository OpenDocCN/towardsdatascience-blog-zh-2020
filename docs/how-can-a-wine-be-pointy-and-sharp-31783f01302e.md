# 葡萄酒怎么会又尖又尖？

> 原文：<https://towardsdatascience.com/how-can-a-wine-be-pointy-and-sharp-31783f01302e?source=collection_archive---------34----------------------->

## 使用主题建模来揭开葡萄酒评论的神秘面纱，并通过创建基于内容的推荐系统来帮助消费者找到他们喜欢的葡萄酒。

![](img/074c23b7da241bc4a86989ff9577096d.png)

[https://winefolly.com/tips/40-wine-descriptions/](https://winefolly.com/tips/40-wine-descriptions/)

> 葡萄酒是如何变得尖尖的、尖锐的、像激光一样的或多肉的？葡萄酒描述(尤其是侍酒师写的评论)通常包含看似随意的形容词，这可能会让消费者非常困惑。之前在一家葡萄酒商店工作过，我亲眼目睹了这些描述和评论是如何造成混乱的。如果对葡萄酒的评论没有意义，消费者怎么知道他们是否会喜欢一瓶酒？如果他们喜欢一种酒，他们怎么能找到类似的瓶子呢？

**该项目的目标:** 1)使用自然语言处理和主题建模来发现葡萄酒评论的趋势 2)创建一个推荐系统，根据第一部分中的主题建模找到类似的(和更便宜的)葡萄酒。

**数据:**这个项目的数据集是在 Kaggle 上找到的，由一个 2017 年刮葡萄酒爱好者杂志的用户创建。它有 13 万条葡萄酒评论，有许多有用的功能，如:葡萄品种、点数、价格、地区，当然还有侍酒师的评论。要更详细地预览功能和数据，请参见数据集[这里的](https://www.kaggle.com/zynicide/wine-reviews)。

# 步骤 1:清理和探索性数据分析

与任何数据科学项目一样，EDA 和清理是第一步。有人恨，有人爱，我们都要做。这个项目的大部分 EDA 是为了娱乐/练习，对于 NLP 或推荐系统来说不是太必要，所以我不会详细介绍。然而，如果你想看我的代码和过程，可以在 [Github](https://github.com/atersakyan/Projects/tree/master/MetisProject4) 上查看这个项目。

# 步骤 2:为主题建模做准备

## **什么是主题建模？**

首先，什么是主题建模？主题建模是一种 NLP 模型，它分析文档中的单词，试图找到它们之间潜在的“主题”(主题)。例如，假设我们有四个句子(在 NLP 中也称为文档):1)“我喜欢狗和猫”2)“葡萄酒有草莓的味道”3)“狗的皮毛是黑色的”4)“葡萄酒闻起来有猫粮的味道”。如果我们思考这些文件中的主题，很明显第一和第三个是关于动物的，第二个是关于酒的，第四个可能是这两个主题的结合。主题模型基本上是用一种更加数学化的方法来做这件事的。

主题建模有许多变体，如 NMF、LDA 和 LSA，每一种都有自己独特的数学基础。此外，这些模型可以在各种 Python 包中运行，如 spaCy、Gensim、scikit-learn 等。每个包需要稍微不同的步骤，尤其是在文本预处理中。虽然我尝试了各种模型和软件包，但我表现最好的模型是在 Gensim 中完成的 LDA 模型，所以这篇博客文章概述了我的经验。

## **为主题建模准备文本**

在进入主题建模之前，我们需要准备将要运行模型的文本。对于这个分析，我正在寻找葡萄酒描述中的潜在主题，所以我只使用了数据集中的*描述*列。这是侍酒师对每款酒的书面评论的特点。

预处理步骤如下:

*   **去掉**:标点符号，数字，把所有东西都变成小写
*   **去除停用词**:停用词是极其常见的词，如“a”和“the”。每个软件包都内置了您可以自定义的停用字词集。通过一个迭代的过程，我把我的停用词表定制成大于 15k 个单词。在这一部分花点时间是值得的，因为它有助于澄清你的话题。参见 [Github](https://github.com/atersakyan/Projects/blob/master/MetisProject4/2_WineNLP_Cleaning%2BLDA.ipynb) 上的代码。
*   **标记化**:将描述列分割成单个单词块。
*   **词条和/或词干**:这允许像“跑步”、“跑”和“跑”这样的词被同等对待，并有助于减少独特词出现的次数。
*   **单词包**:将文本转换成单词包，这样我们就可以对其进行建模。单词包分别处理每个单词(想象一个包，里面有类似拼字游戏的单词块)，然后计算每个单词的出现次数。在此之后，您可以使用 TF-IDF 或计数矢量器来使模型更加健壮，但这里介绍的模型没有使用这些工具。关于使用 TF-IDF 和 CV 的其他型号，请参见 Github 代码。

![](img/fd1ece7fc5e711028a34f447b6e2177b.png)

词汇化后出现频率最高的词。注意顶部的单词是描述性的，而不是停用词。

# 步骤 3:运行 LDA 模型

现在，文本已经准备好进行建模了。如前所述，主题建模有许多不同的方法和包。在接下来的章节中，我将讨论我最终的 Gensim LDA 模型。

## **什么是 LDA (** 潜在狄利克雷分配)？

LDA 是一种概率模型，它假设语料库中的每个文档都由主题分布组成，每个主题都由单词分布组成。该模型的目标是学习每个文档的主题混合，以及每个主题的单词混合。当使用 LDA 进行 NLP 时，我们挑选主题的数量(这部分比较棘手，涉及许多迭代)，并且模型被迫具有 *n* 个主题的主题分布。该模型在词-主题级别以及主题-文档级别分析文本。当遍历迭代时，模型会问:*一个单词属于一个主题的概率是多少，这个文档由这些主题组成的概率是多少？*

下图帮助我想象 LDA 是如何工作的。假设我们有三个主题——三角形的每个角都是一个独特的主题。在这种情况下，每个文档由三个主题的一定比例组成。如果我们的文档是主题的混合，它看起来就像右上角的三角形。如果文档是主题二和主题三的混合，那么它看起来就像左下角的三角形。LDA 有两个影响主题构成的超参数( *alpha，beta* )，但我只是在我的模型中使用了默认值。[这篇](https://medium.com/@lettier/how-does-lda-work-ill-explain-using-emoji-108abf40fa7d)中的文章很好地描述了他们。

![](img/963916d39ae394a675b0a5f96ca74e7f.png)

## 在 Gensim 中运行 LDA 模型

```
# Dictionary- word to numeric id mapping
dictionary = corpora.Dictionary(df.tokenized)# Corpus- transform texts to numerical form (bag of words)
corpus = [dictionary.doc2bow(doc) for doc in df.tokenized]# LDA Model- 15 topics, 20 passes
lda_model = models.LdaModel(corpus, id2word=dictionary, num_topics=15, passes=20 )# Print topics
lda_model.print_topics(15)
```

运行模型后，您应该检查您的主题，看它们是否有意义。这是 NLP 的棘手之处——没有正确的答案。你需要用你的直觉和领域知识来看看题目是否有意义。你可能要试几次。运行上面的模型后，一些主题看起来像这样:

```
(1,
  '0.136*"oak" + 0.069*"vanilla" + 0.045*"toast" + 0.021*"toasted" + 0.020*"toasty" + 0.017*"buttered" + 0.016*"caramel" + 0.015*"richness" + 0.015*"wood" + 0.014*"oaky"'),(7,
  '0.034*"finish" + 0.030*"apple" + 0.021*"sweet" + 0.019*"pear" + 0.018*"pineapple" + 0.018*"citrus" + 0.018*"nose" + 0.016*"melon" + 0.015*"white" + 0.014*"tropical"'), (9,
  '0.060*"blackberry" + 0.044*"black" + 0.029*"chocolate" + 0.025*"tannin" + 0.018*"ripe" + 0.017*"cherry" + 0.017*"syrah" + 0.016*"tannic" + 0.016*"oak" + 0.015*"show"'),
```

这些话题对我来说很有意义。第一个可能是黄油，橡木，夏敦埃酒的代表。第九种可能是巧克力和橡木红葡萄酒。

# 第四步:推荐相似但更便宜的葡萄酒

> 想象一下，你去老板家吃饭，喝了一些好酒。第二天你查看评论，发现是 200 美元。也许你想找一些口味相似、价格更实惠的东西。确定你喜欢的口味和导航混乱的描述可能会令人生畏。

## 推荐系统

既然主题建模已经完成，我们可以使用这些结果来创建一个推荐系统。有两种主要类型的推荐系统:基于内容和协同过滤。对于这个项目，我创建了一个基于内容的推荐系统。基于内容的模型使用关于用户和/或项目的附加信息(例如人口统计)，而不是依赖于用户-项目交互。这里，因为我的模型没有任何用户信息，所以内容是葡萄酒描述的主题分布。因此，该模型推荐具有类似主题细分的葡萄酒。

![](img/3edc9a3942e5a751ff1f7a52f11e035e.png)

协同过滤与基于内容的对比(来源:[https://www . research gate . net/figure/Content-based-filtering-vs-Collaborative-filtering-Source _ fig 5 _ 323726564](https://www.researchgate.net/figure/Content-based-filtering-vs-Collaborative-filtering-Source_fig5_323726564))

用最简单的术语来说，推荐系统通过比较项目的距离来工作。这可以是余弦相似性、欧几里德距离、詹森-香农距离、库尔巴克-莱布勒散度或许多其他选项。它们都只是测量物体在空间中的距离。对于这个项目，我使用了 Jensen-Shannon 距离，因为这是一种测量概率分布之间距离的方法——这就是 LDA 模型中每个文档的主题分布。

为了在 python 中实现这一点，我利用了 SciPy 的熵度量。不用深究数学，只需知道詹森-香农是基于库尔贝克-莱布勒距离，而 SciPy 的熵只是库尔贝克-莱布勒距离的一个度量。Jensen-Shannon 距离的范围从 0 到 1，0 表示两个分布最接近/最相似。

## **运行推荐系统的步骤**

获得 Jensen-Shannon 距离的第一步是将我们的矩阵从 LDA 转换成密集的文档 X 主题矩阵。在这个新的矩阵中，每一行都是葡萄酒评论，每一列都是每种葡萄酒的主题细分。一旦我们有了这个矩阵，我们运行文档向量之间的距离度量(即 Jensen-Shannon 距离)。

![](img/5fe26224ffc12cbbbb3921861ef018c1.png)

文档 X 主题矩阵

这包括几个简单的步骤:

```
# Convert from bow to sparse matrix to dense matrix to pandas
# Transform out of bow
corpus_lda = lda_model_tfidf[corpus]# Convert bow corpus into sparse matrix with docs as columns
csc_mat_lda = gensim.matutils.corpus2csc(corpus_lda)# Turn docs into rows, and convert to np array
# Now we have a doc X topic numpy matrix
doc_topic_array_lda = csc_mat_lda.T.toarray()# Convert to pandas for ease and readability
df_lda = pd.DataFrame(doc_topic_array_lda)# Set column names to topic #s for pandas df
df_lda.columns = [f'topic_{x}' for x in np.arange(0,len(df_lda.columns))]
```

如果我们在数据框架中随机访问一种葡萄酒，我们可以很容易地看到它的主题构成。

```
# Pick a random wine to examine its topic breakdown
df_lda.loc[123]
________________________________# Looks like this wine is primarily made up of topics 6, 9, and 14
topic_0     0.000000
topic_1     0.000000
topic_2     0.000000
topic_3     0.082272
topic_4     0.067868
topic_5     0.000000
topic_6     0.253451
topic_7     0.000000
topic_8     0.107688
topic_9     0.287275
topic_10    0.000000
topic_11    0.000000
topic_12    0.000000
topic_13    0.000000
topic_14    0.179217
```

## 终于到了提出一些建议的时候了

现在我们有了文档 X 主题矩阵，我们只需要运行一些函数来提取与我们的查询最相似的葡萄酒。

函数 1)在一种葡萄酒和语料库的其余部分之间查找 JSD 2)查找最相似的葡萄酒

这些函数的输出是最相似的葡萄酒的 T4 指数。它看起来像这样:

```
# Get the topic distribution of a query wine to find similar wines to
new_wine = doc_topic_array_lda[123].Tmost_similar_wine_ilocs = get_most_similar_wines(new_wine,full_matrix)
________________________________

array([123, 3436, 52985, 59716, 101170, 37219, 43017, 99717,         80216, 9732, 101690, 40619, 66589, 14478, 1068, 1157, 67821, 100428, 8895, 8894])
```

需要注意的一点是，在运行这个函数时， *new_wine* 是我们要从中寻找推荐的一个查询葡萄酒的主题分布。可能是你在老板家喝的酒。

好了，在我们最终做出一些葡萄酒推荐之前，还有一个步骤。来自前面函数的熊猫索引是来自文档 X 主题矩阵的 *ilocs* ,该矩阵只包含主题分布作为列——没有关于葡萄酒的信息。我们需要将这个数据帧与原始数据帧合并，这样当我们提取最相似的葡萄酒时，我们就有了所有的信息。如果我们希望向用户提供关于葡萄酒的描述性信息，而且希望使用描述性信息(比如价格)作为过滤器，这一点很重要。只要确保它们合并正确，否则你会有一些不匹配。一旦完成，你就可以创建一个封装了所有东西的最终函数，并且有一个价格范围的输入。

# 工作中的推荐系统示例

说明最终模型的最好方式是用一个例子。我从阿根廷挑选了一款 215 美元的马尔贝克，并在最终推荐系统中进行了测试。我还添加了一个价格过滤器，以便找到一个价格低于 50 美元的类似瓶子。最受推荐的葡萄酒是华盛顿州售价 35 美元的赤霞珠葡萄酒。

我绘制了两种葡萄酒的主题分布图，它们非常相似。这意味着推荐系统正在工作。当然，它并不完美，所有的主题建模都可以迭代多次，以获得更好、更清晰的主题，这反过来会返回更好的推荐。

![](img/962316a98e774d62b72d4b586fba74dc.png)![](img/73b5baed6b6c370750b7257e32e80536.png)

查询葡萄酒(左)和热门推荐葡萄酒(右)的主题分布

# 摘要

这个项目的目标是:1)使用自然语言处理主题建模来发现葡萄酒评论中的共同主题 2)创建一个基于内容的推荐系统来找到相似的葡萄酒。

在本文中，我回顾了文本预处理的步骤、简要的 LDA 概述以及 Gensim 中的 LDA 主题建模。最后，我介绍了构建基于内容的推荐系统的步骤。通过这个项目，我能够实现我的两个目标，并创建一个最终模型，根据文本描述的风味特征推荐葡萄酒。

这个项目的所有代码都在我的 [Github](https://github.com/atersakyan/Projects/tree/master/MetisProject4) 上。请随时在 [LinkedIn](https://www.linkedin.com/in/atersakyan/) 上联系我。