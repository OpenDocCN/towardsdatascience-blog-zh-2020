# 自然语言处理的 Lovecraft 第 4 部分:潜在语义分析

> 原文：<https://towardsdatascience.com/lovecraft-with-natural-language-processing-part-4-latent-semantic-analysis-70aa2fa2161b?source=collection_archive---------65----------------------->

## 应用降维技术将 TF-IDF 向量转换成更有意义的 H. P. Lovecraft 故事的表示。

![](img/5c0c2fa1aa630b85a97d4aa5bff1c9b0.png)

图片来自 [Pixabay](https://pixabay.com/?utm_source=link-attribution&utm_medium=referral&utm_campaign=image&utm_content=3394066) 的[darkworx](https://pixabay.com/users/DarkWorkX-1664300/?utm_source=link-attribution&utm_medium=referral&utm_campaign=image&utm_content=3394066)

这是我正在进行的系列文章中的第四篇，我将不同的自然语言处理技术应用于 [H. P .洛夫克拉夫特](https://en.wikipedia.org/wiki/H._P._Lovecraft)的作品。关于本系列的前几篇文章，请参见[第 1 部分—基于规则的情感分析](/lovecraft-with-natural-language-processing-part-1-rule-based-sentiment-analysis-5727e774e524)、[第 2 部分—标记化](/lovecraft-with-natural-language-processing-part-2-tokenisation-and-word-counts-f970f6ff5690)、[第 3 部分— TF-IDF 向量](/lovecraft-with-natural-language-processing-part-3-tf-idf-vectors-8c2d4df98621)。

这篇文章主要基于 TF-IDF 向量的概念，这是一个文档的向量表示，基于文档和整个语料库中单个单词的相对重要性。下一步，我们将使用潜在语义分析(LSA)将这些向量转换成低维表示。LSA 使得基于意思而不是精确的单词用法来搜索文档成为可能，这通常会比 TF-IDF 产生更好的匹配。

我们将首先讨论使用 TF-IDF 的缺点，以及为什么调整这些向量是有意义的。然后，我们将澄清一些我个人认为令人困惑的数学术语。最后，我们重复上一篇文章中的步骤，创建一个 Lovecraft 故事的矢量表示，看看我们能否使用聚类分析得出有意义的分组。

## TF-IDF 与潜在语义分析

正如我们在上一篇文章中看到的，TF-IDF 向量是语料库中各个文档的多维向量表示。如果我们在所有文档中有 10，000 个唯一的单词，并且有 500 个文档，我们可以创建一个 500 x 10，000 的矩阵，其中每一行都是表示文档的向量，唯一的单词形成维度，并且每个元素表示特定单词在特定文档中的相对重要性。在这个过程中，我们会丢失一些信息，最重要的是单词的顺序，但 TF-IDF 仍然是一种令人惊讶的强大方法，可以将一组文档转换为数字，并在其中进行搜索。

然而，TF-IDF 有几个问题:

*   它侧重于拼写和单词用法——你可以重新表述一个句子，得到一个完全不同的 TF-IDF 表示，即使意思根本没有变化；
*   词汇匹配可能将一些单词组合在一起，但是同义词将被单独处理；
*   TF-IDF 背后的数学是严格关于频率的，并假设频率是唯一重要的东西。

为了解决这些问题，我们将不再关注单个单词，而是提出“主题”。就拿洛夫克拉夫特的作品来说吧。不用数他用了多少次“可怕的”或“可怕的”，我们会有“梦境描述”、“伟大的老故事”和“反移民情绪”这样的话题(是的，嗯，没有人是完美的)。与一个或另一个主题强烈对应的单词将增加这些主题在各自故事中的总得分。然后，我们不再衡量词频之间的接近程度，而是只考虑聚合主题。这基本上是潜在语义分析背后的思维过程，它会找到属于一起的单词，因为它们与相同的主题高度相关。

我们将应用这种方法来加强我们对故事进行的聚类分析。人们可以希望，如果我们考虑聚合的主题而不是单个的单词，感觉它们应该属于一起的故事将有更高的机会被分配到同一个组。

然而，这种降维技术更重要、更广泛的应用是在分类问题上。NLP 数据往往具有大量词汇和相对少量的观察值。如果你分析 5000 条推文，你可能会有 10000 个单词，这意味着你会有两倍于观察的特征，这肯定不是理想的。如果你设法减少这些维度，同时保留大部分信息，这将使你的模型更强大。

所以唯一的问题是，我们如何提出这些话题？这就是奇异值分解发挥作用的地方。然而，在我们研究数学来管理期望值之前，我想提一件事:我提到的那些主题，那些干净的、标签很好的主题——我们可以忘记得到任何类似的东西，除非你正在处理几个非常好的例句。鉴于洛夫克拉夫特故事的规模和同质性，我认为不可能在主题如何产生的背后提出一个好的推理。但是这并不是这个分析的目标，我们只是想检查一下我们是否能想出一个自动将故事分组的结构。

## 数学概念

好了，让我们看看创建主题向量背后的数学概念！正如我在引言中提到的，我发现这些概念在文献中经常互换使用的方式令人困惑。希望这些高层次的定义能够澄清一些困惑。

*   [](https://en.wikipedia.org/wiki/Singular_value_decomposition)**【奇异值分解】 :将一个 m×n 矩阵分解成三个维度分别为 m×m、m×n 对角线和 n×n 的矩阵的乘积，它有大量不同的应用，详情请查看维基百科页面。关键是，SVD 本身就是分解。**
*   **[**【主成分分析(PCA)**](https://en.wikipedia.org/wiki/Principal_component_analysis) :一个降维应用，可以使用 SVD 的结果来实现(在其他技术中，但 SVD 绝对是一个真正流行的方法)。PCA 常用于可视化或处理高维数据。当我们将数据“压缩”到一个较低的维度表示中时，我们希望保留观察值中最大可能的方差。我在《动作中的自然语言处理》一书中发现了一个很好的类比(见参考资料),即你有一个三维物体，想要将阴影投射到二维表面，所以你找到了一个可以清晰识别阴影的角度。**
*   **[**【潜在语义分析(LSA)**](https://en.wikipedia.org/wiki/Latent_semantic_analysis) :基本上与 PCA 相同的数学，应用于 NLP 数据。该数据可以是任何向量表示，我们将使用 TF-IDF 向量，但它也适用于 TF 或简单的单词包表示。这就是我们如何找到文档的“主题”。**

## **LSA 在爱情小说 TF-IDF 向量中的应用**

**现在，最后，让 LSA 继续我们的故事！如上所述，LSA 可以应用于各种 NLP 表示，但是我们现在在 TF-IDF 向量上使用它。**

**我们有一个来自之前项目的叫做`result`的稀疏矩阵，它包含了我们语料库中 63 个故事的 TF-IDF 表示。一般来说，在进行主成分分析之前，需要对向量进行归一化，以便将不同的维度缩放到相同的大小。对于 TF-IDF 向量，scikit-learn 已经完成了大部分工作，但是我们仍然可以将向量集中在 0 附近。为此，我们不得不放弃稀疏矩阵格式:**

```
result_df = result.toarray()
result_df = pd.DataFrame(result_df)
result_df = result_df - result_df.mean()
```

**现在我们有了一个 pandas 数据框架，无需对模型做进一步的调整就可以使用。**

**正如 scikit-learn 中的许多内容一样，创建模型非常简单:**

```
from sklearn.decomposition import PCApca = PCA(n_components = 50)
```

**最重要的参数是`n_components`，它定义了我们希望模型聚合到多少个不同的组件(在 NLP 问题中是“主题”)。(有关其他参数的详细信息，请参见[文档](https://scikit-learn.org/stable/modules/generated/sklearn.decomposition.PCA.html)。)那么作为题目数量的 50 从何而来呢？这需要用一种叫做`explained_variance_ratio_`的方法进行一些微调:**

```
pca.fit(result_df)
sum(pca.explained_variance_ratio_)
```

**这将决定我们通过将超过 17，000 的原始矩阵压缩到 50 而保留的故事之间的差异百分比。在我们的例子中，这个保存的比率大约是 87%,乍一看这肯定是可疑的，但是我们必须记住，使用 63 个组件，我们将能够保存 100%的原始信息。**

**对我来说，87%就足够了。你当然可以走一条更科学的路线，计算一些不同数量的成分的解释方差比，然后找到“肘”点。一旦我们对配置满意，我们就可以调用`fit_transform`:**

```
result_df = pd.DataFrame(pca.fit_transform(result_df))
```

**新的`result_df`将是一个 63 行(原始故事)和 50 列(人工创建的“主题”)的矩阵。**

**现在，我们基本上可以重复前一篇文章中的相同过程，只是功能数量要少得多。**

**运行 k 均值模型:**

```
kmeans_models = {}
for i in range(2,10+1):
    current_kmean = KMeans(n_clusters=i).fit(result_df)
    kmeans_models[i] = current_kmean
```

**并将结果组织在熊猫数据框架中:**

```
cluster_df_2 = pd.DataFrame()
cluster_df_2['title'] = filenames
for i in range(2, 10+1):
    col_name = str(i) +'means_label'
    cluster_df_2[col_name] = kmeans_models[i].labels_
```

**上面的`cluster_df_2`对象在我的 [GitHub repo](https://github.com/MatePocs/lovecraft/blob/master/results/word_counts/lsa_clustering.csv) 中以 CSV 格式提供。**

## **结论**

**让我们看看 LSA 集群的表现有多好！**

**我们再一次遇到了与上一篇关于 TF-IDFs 的文章类似的问题:我不认为这个分组问题有一个真正优雅的解决方案，而且一个人类专家也会发现这个任务非常困难，如果不是不可能的话。**

**话虽如此，我还是设法找到了一些不错的模式。例如，如果我们看一下 4-means 组，有三组非常不同的故事:**

*   **第二组包含了大多数经常提到的洛夫克拉夫特故事的“梦序列”系列；**
*   **第 0 组只有一个故事:[同上(1928)](https://en.wikipedia.org/wiki/Ibid_(short_story)) ，这是一篇非常怪异的文章，即使用洛夫克拉夫特的标准来衡量，这也是一本关于罗马士兵的模拟传记，其中很大一部分文字是随意的罗马人名；**
*   **第三组故事的主要主题是坟墓和活死人:坟墓(1917 年)，赫伯特·韦斯特——复活者(1922 年)，在地下室(1925 年)，黑暗的鬼斯通(1935 年)是这一组的故事；**
*   **最后，第一组，嗯，第一组有所有其他的故事...**

**这似乎是不同集群的共同主题。该算法通常会分离出几个非常怪异和独特的故事，并将其余的故事放在一个大组中。真正受欢迎和著名的 Lovecraft 故事几乎总是被分配到同一个组，而不管集群的数量。**

**我个人认为这还是相当可观的成绩。我们设法将 Lovecraft 故事的含义压缩到 50 个人工创造的主题中，即使创造的群体并不十分出色，你也一定可以发现模式。比 TF-IDF 表现好吗？我也这么认为我设法在 LSA 结果中发现了以前没有的好模式。**

**我们可以尝试使用更少的维度来进一步压缩含义，这可以改善聚类。然而，我认为主要的问题是我们的语料库非常小，只有 63 个文档。可悲的是，我们再也没有原创的爱情故事了。**

## **参考**

**霍布森、科尔和汉尼斯(2019 年)。自然语言处理实践:理解、分析和用 Python 生成文本。曼宁出版，2019。**

**[](https://scikit-learn.org/stable/modules/generated/sklearn.decomposition.PCA.html) [## sk learn . decomposition . PCA-sci kit-learn 0 . 23 . 1 文档

### 主成分分析。使用数据的奇异值分解进行线性降维…

scikit-learn.org](https://scikit-learn.org/stable/modules/generated/sklearn.decomposition.PCA.html) 

洛夫克拉夫特全集:

[https://arkhamarchivist . com/free-complete-love craft-ebook-nook-kindle/](https://arkhamarchivist.com/free-complete-lovecraft-ebook-nook-kindle/)**