# 告诉我一个故事，艾。一个我喜欢的。

> 原文：<https://towardsdatascience.com/tell-me-a-story-ai-one-that-i-like-4c0bc60f46ae?source=collection_archive---------42----------------------->

![](img/4ffe988fd5e2c97ee3409f6d7a324ff0.png)

书的形象:Pixabay。代码图像:Unsplash。

## 编写一个强化学习系统来讲述你会喜欢的故事

既然我们都被孤立了，我们比以往任何时候都更需要故事。但并不是所有的故事都让我们感兴趣——喜欢浪漫的人可能会对神秘故事嗤之以鼻，反之亦然。有谁比 AI 更适合给我们讲为我们量身定做的故事？

在这篇文章中，我将通过给我们讲故事来指导你编写一个人工智能的过程和代码，使隔离变得更加有趣和有趣——这些故事是为我们的口味量身定制的。

这篇文章将被分成几个部分:

*   *1 |蓝图。*项目的鸟瞰图以及项目的组成部分。
*   *2 |程序演示*。在所有东西都被编码后，系统能力的演示，作为某种预览。
*   *3 |数据加载和清洗。*加载数据并准备好进行处理。
*   4 |寻找最有鉴别力的情节。项目的第一个组成部分，使用 K-Means 寻找最能区分用户对故事品味的情节。
*   *5 |总结剧情*。使用基于图形的摘要来获得每个情节的摘要，这是 UI 不可分割的一部分。
*   *6 |推荐引擎*使用简单的预测机器学习模型来推荐新故事。
*   *7 |汇集一切*。规划生态系统结构，将所有部分结合在一起。

# 1 |蓝图

我们希望人工智能给我们讲故事。理想情况下，在真正的技术复兴时尚中，我们希望训练一个递归神经网络或其他一些生成方法。然而，根据我处理文本生成的经验，它们要么需要花费*非常非常*长的时间来训练，要么最终过度适应数据，从而违背了“原始文本生成”的目的。另外说明一下，在 Kaggle 上，训练一个表现良好的模型所需的时间超过了 8 个小时，即操作时间限制，而 ka ggle(据我所知)是训练深度学习模型最高效的免费平台。

相反，我希望这个项目是快速和通用的，每个人都可以实现。这个人工智能不是训练 RNN 或生成模型，而是在“故事数据库”中搜索人类创造的故事，并找到最好的一个。这不仅保证了基本的常识和故事质量(由人类制作，为人类服务)，而且速度更快。

对于我们的“故事数据库”，我们将使用 Kaggle 上的维基百科[电影情节](https://www.kaggle.com/jrobischon/wikipedia-movie-plots)数据集。拥有 35000 部电影的故事，包括所有类型、国家和时代，这是你能找到的最好的故事数据库。

![](img/2e2307f3b0ff6ccf016aa08f09ed0c5d.png)

在这些列中有发行年份、标题、电影的国籍、类型和情节的文本描述。

![](img/62fe996e43cd52475ecee80fb6de1da4.png)

现在我们有了数据，我们可以开始设计一个粗略的大纲/蓝图。

1.  该程序打印出五个左右最具鉴别性的故事的快速摘要(对这些故事的看法将更好地区分用户的口味。例如，像《教父》这样几乎每个人都喜欢的故事在品味上不会很有鉴赏力，因为每个人都喜欢它)。
2.  用户对他们是否喜欢、不喜欢或者对每个故事概要保持中立进行评级。
3.  该程序接受用户对这五个有区别的故事的品味，并输出完整长度故事的摘要。如果用户感兴趣，程序输出完整的故事。在每个完整的故事之后，程序会询问用户的反馈。该程序从实时反馈中学习，并试图提出更好的建议。(强化学习系统。)

请注意，选择大约五个最有区别的故事是为了用有限的数据量给模型提供尽可能多的信息。

![](img/a4ffa394572705331626380fe0dfdd03.png)

我们强化学习系统的基本路线图。就是不断的学习！

# 2 |系统演示

最初，这个程序会询问你对三个故事快照的看法。在程序看来，这三个故事最能代表数据中的每个分类。

![](img/df53f47568ab9ecc3df95b310ac24ce1.png)

在你回答了前三个问题以大致判断你的口味后，模型将开始生成它认为你会喜欢的故事。

![](img/eea3707a303cc860d2240abe5f7f296a.png)

如果你对某个故事的片段感兴趣，程序会打印出整个故事供你阅读。

![](img/0cf4bd86deb4b1490354e2abb2fbff56.png)

你是否喜欢一个故事被添加到模型的训练数据中，以改进它的推荐。当你阅读故事时，这个模型不断地学习。如果你不喜欢一个片段，程序不会打印出完整的故事，并继续生成一个新的。

![](img/9f979200e9bebdd5089477a239318120.png)

在对几个谋杀和警察片段做出“1”反应后，该程序开始学习，并开始向该方向推荐越来越多的故事。

![](img/e0c51155a83aa84b30cb7b98e3d38935.png)

这个程序就像蒙特卡罗树搜索一样，开始朝着优化回报的方向前进，当它走得太远(也许它已经偏离你喜欢的故事类型太远)而无法优化你的体验时，它就会撤退。

# 3 |数据加载和清理

我们将用`pandas` ' `load_csv`加载数据。

```
import pandas as pd
data = pd.read_csv('/kaggle/input/wikipedia-movie-plots/wiki_movie_plots_deduped.csv')
data.head()
```

![](img/35d83f13cb6fd27d7a37d8e3667f9b3f.png)

我们有电影的发行年份、片名、国籍、导演、演员、类型、该电影的维基百科页面的 URL 以及情节的文本描述。我们可以摆脱导演和演员——对于我们的推荐算法或聚类方法来说，有太多的类(确切地说，有 12593 个独特的导演和 32182 个演员)无法从中获得任何东西。然而，这种类型是有用的，有少量的类别——30 个类别的电影超过 100 部——代表 80%的电影(其余的属于更小的子类别，可以简单地标记为“其他”)。因此，我们可以放弃导演和演员。

```
data.drop(['Director','Cast'],axis=1,inplace=True)
```

![](img/2b7618ff60937cc3ee8211d9ae71aeff.png)

我们遇到的另一个问题是括号引用。正如你所熟悉的，维基百科引用其来源时会在括号内加上一个与来源相对应的数字(例如[3])。

```
"Grace Roberts (played by Lea Leland), marries rancher Edward Smith, who is revealed to be a neglectful, vice-ridden spouse. They have a daughter, Vivian. Dr. Franklin (Leonid Samoloff) whisks Grace away from this unhappy life, and they move to New York under aliases, pretending to be married (since surely Smith would not agree to a divorce). Grace and Franklin have a son, Walter (Milton S. Gould). Vivian gets sick, however, and Grace and Franklin return to save her. Somehow this reunion, as Smith had assumed Grace to be dead, causes the death of Franklin. This plot device frees Grace to return to her father's farm with both children.[1]"
```

例如，在上面的字符串中，我们想要删除[1]。对此最简单的解决方案是创建一个包含每个括号值([1]、[2]、[3]、[98]、[99])的列表，并从字符串中删除它们。这是可行的，因为我们可以确定没有一篇文章的引用次数超过 99 次。虽然不是最有效的，但它可以完成混乱的字符串索引或拆分工作。

```
blacklist = []
for i in range(100):
    blacklist.append('['+str(i)+']')
```

这就产生了“黑名单”——一个充满我们不需要或不想要的引用的列表。

```
def remove_brackets(string):
    for item in blacklist:
        string = string.replace(item,'')
    return string
```

使用黑名单，我们可以创建一个应用于该列的函数`remove_brackets`。

```
data['Plot'] = data['Plot'].apply(remove_brackets)
```

这就结束了我们的初级数据清理。

# 4 |汇总情节

我们系统的一个关键要素是总结情节。因为故事通常太长而无法阅读，所以总结故事很重要，这样用户就可以选择是否继续阅读。

我们将使用基于图形的摘要，这是最流行的文本摘要方法。它创建一个文档单元图(而大多数其他方法使用句子作为基本单元)，然后选择具有适合该场景的 PageRank 版本的节点。Google 最初的 PageRank 版本使用类似的基于图形的方法来查找网页节点。

PageRank 算法计算图中节点的“中心性”,这在测量句子中的相关信息内容时很有用。该图是使用单词袋特征序列和基于余弦相似度的边权重构建的。

我们将使用`gensim`库来总结长文本。与前面的示例一样，实现起来很简单:

```
import gensim
string = '''
The PageRank algorithm outputs a probability distribution used to represent the likelihood that a person randomly clicking on links will arrive at any particular page. PageRank can be calculated for collections of documents of any size. It is assumed in several research papers that the distribution is evenly divided among all documents in the collection at the beginning of the computational process. The PageRank computations require several passes, called “iterations”, through the collection to adjust approximate PageRank values to more closely reflect the theoretical true value.
Assume a small universe of four web pages: A, B, C and D. Links from a page to itself, or multiple outbound links from one single page to another single page, are ignored. PageRank is initialized to the same value for all pages. In the original form of PageRank, the sum of PageRank over all pages was the total number of pages on the web at that time, so each page in this example would have an initial value of 1\. However, later versions of PageRank, and the remainder of this section, assume a probability distribution between 0 and 1\. Hence the initial value for each page in this example is 0.25.
The PageRank transferred from a given page to the targets of its outbound links upon the next iteration is divided equally among all outbound links.
If the only links in the system were from pages B, C, and D to A, each link would transfer 0.25 PageRank to A upon the next iteration, for a total of 0.75.
Suppose instead that page B had a link to pages C and A, page C had a link to page A, and page D had links to all three pages. Thus, upon the first iteration, page B would transfer half of its existing value, or 0.125, to page A and the other half, or 0.125, to page C. Page C would transfer all of its existing value, 0.25, to the only page it links to, A. Since D had three outbound links, it would transfer one third of its existing value, or approximately 0.083, to A. At the completion of this iteration, page A will have a PageRank of approximately 0.458.
In other words, the PageRank conferred by an outbound link is equal to the document’s own PageRank score divided by the number of outbound links L( ).
 In the general case, the PageRank value for any page u can be expressed as: i.e. the PageRank value for a page u is dependent on the PageRank values for each page v contained in the set Bu (the set containing all pages linking to page u), divided by the number L(v) of links from page v. The algorithm involves a damping factor for the calculation of the pagerank. It is like the income tax which the govt extracts from one despite paying him itself.
'''
print(gensim.summarization.summarize(string))
```

[输出]:

```
In the original form of PageRank, the sum of PageRank over all pages was the total number of pages on the web at that time, so each page in this example would have an initial value of 1\. 
The PageRank transferred from a given page to the targets of its outbound links upon the next iteration is divided equally among all outbound links. If the only links in the system were from pages B, C, and D to A, each link would transfer 0.25 PageRank to A upon the next iteration, for a total of 0.75\. Since D had three outbound links, it would transfer one third of its existing value, or approximately 0.083, to A.
```

总结是有意义的(如果你愿意阅读整篇文章的话)。图表汇总是最有效的汇总方法之一，我们将在我们的图表中使用它。让我们创建一个函数`summary`，它接收文本并输出摘要。然而，我们需要满足两个条件:

*   如果文本的长度小于 500 个字符，那么只返回原始文本。总结会使它过于简短。
*   如果文本只有一个句子，`genism`不能处理它，因为它只选择文本中的重要句子。我们将使用`TextBlob`对象，它有一个属性。将文本拆分成句子的句子。如果一篇课文的第一句话等于正文，那么这篇课文就只有一句话。

```
import gensim
from textblob import TextBlobdef summary(x):
    if len(x) < 500 or str(TextBlob(x).sentences[0]) == x:
        return x
    else:
        return gensim.summarization.summarize(x)
data['Summary'] = data['Plot'].apply(summary)
```

如果不满足这两个条件中的任何一个，它将返回文本的摘要。然后，我们将创建一个列`Summary`来总结这些图。

这需要几个小时来运行。但是，这是一次性成本，有一个总结可以节省以后的时间。

让我们看看它在数据集的一些示例文本上的表现:

```
"The earliest known adaptation of the classic fairytale, this films shows Jack trading his cow for the beans, his mother forcing him to drop them in the front yard, and beig forced upstairs. As he sleeps, Jack is visited by a fairy who shows him glimpses of what will await him when he ascends the bean stalk. In this version, Jack is the son of a deposed king. When Jack wakes up, he finds the beanstalk has grown and he climbs to the top where he enters the giant's home. The giant finds Jack, who narrowly escapes. The giant chases Jack down the bean stalk, but Jack is able to cut it down before the giant can get to safety. He falls and is killed as Jack celebrates. The fairy then reveals that Jack may return home as a prince."
```

总结为

```
'As he sleeps, Jack is visited by a fairy who shows him glimpses of what will await him when he ascends the bean stalk.'
```

为什么，这是一个伟大的完整的故事的结果！这些摘要读起来很快，并能让你对电影情节中的重要句子有一个很好的了解。这已经很好地解决了。

应用于真实数据集的可分叉笔记本中的代码可以在[这里](https://www.kaggle.com/washingtongold/tell-me-a-story)找到。

# 5 |寻找最具鉴别力的故事情节

为了找到最有区别的故事情节，我们将使用 K 均值将情节的文本聚类到最佳数量的聚类中。文本的聚类标签以及电影的国籍、类型和年份将被聚类以在电影中找到聚类。最接近每个质心(聚类中心)的电影应该最能代表它们的聚类，因此最具歧视性。这个想法背后的主要思想是

> 询问用户有多喜欢那些最具歧视性的电影，对于一个之前没有用户口味信息的模型来说，应该提供了最多的信息。

电影的国籍、类型和年份都代表了电影在文本中可能传达的方面，这有助于我们快速找到合适的推荐。理论上，最“准确”的建议是非常非常长的图的矢量化之间的某种相似性，但这需要很长时间。相反，一个图可以用它的属性来“表示”。

对文本进行聚类是一次性的事情，它不仅为我们提供了用于对电影进行聚类的附加特征，还提供了在我们实际进行推荐时用作电影属性的特征。

![](img/236a9f6c5894f415ad44796e8137dc61.png)

我们开始吧！首先，我们需要删除所有的标点符号，并使所有的文本小写。我们可以使用正则表达式创建一个函数`clean()`来做这件事。

```
import string
import re
def clean(text):
    return re.sub('[%s]' % string.punctuation,'',text).lower()
```

使用`pandas` ' `.apply()`，该功能可以应用于所有的地块。

```
data['Cleaned'] = data['Plot'].apply(clean)
```

现在，让我们对数据进行矢量化。我们将使用术语频率逆文档频率(TF-IDF)矢量器。这个矢量器还可以帮助区分重要的单词和不重要的单词，从而得到更好的聚类。矢量器强调在一个文档中出现较多但在整个语料库中出现较少的单词(更有鉴别能力),而不强调在所有文档中出现的单词。

```
from sklearn.feature_extraction.text import TfidfVectorizer
vectorizer = TfidfVectorizer(stop_words='english',max_features=500)
X = vectorizer.fit_transform(data['Plot'])
```

我们将把非常非常稀疏的矩阵保存到变量`X`中。因为 K-Means 是基于距离的，这意味着它会受到维数灾难的影响，我们应该通过将向量中的最大元素数设置为 500 来尽最大努力降低矢量化文本的维数。(当我没有实现`max_features`限制时，文本将除了集群 2 中的一个文本和集群 3 中的一个文本之外的所有文本分类为集群 1。这是 K-Means 上的维数灾难的结果——距离是如此扭曲，以至于在 TF-IDF 的词汇中可能有成千上万个维度，除了离群值之外的一切都被归入一个类别。

出于同样的原因，在我们将数据输入 K-Means 模型之前，对数据进行缩放是一个好主意。我们将使用`StandardScaler`，它将数据放在`-1`到`1`的范围内。

```
from sklearn.preprocessing import StandardScaler
scaler = StandardScaler()
X = scaler.fit_transform(X)
```

现在，是时候训练 K 均值模型了。理想情况下，集群的数量(我们需要问的问题的数量)将在 3 到 6 之间，包括 3 和 6。

因此，我们将对列表`[3, 4, 5, 6]`中的每个聚类数运行 K-Means 模型。我们将评估每个分类的轮廓分数，并找出最适合我们数据的分类数。

首先，让我们初始化两个列表来存储聚类数和轮廓分数(我们的图的 *x* 和 *y* ):

```
n_clusters = []
scores = []
```

接下来，我们将导入`sklearn`的`KMeans`和`silhouette_score`。

```
from sklearn.cluster import KMeans
from sklearn.metrics import silhouette_score
```

然后，对于四个可能的聚类中的每个聚类数，我们将使用 *n* 个聚类来拟合 KMeans 模型，然后将该聚类数的轮廓分数附加到列表`scores`。

```
for n in [3,4,5,6]:
    kmeans = KMeans(n_clusters=n)
    kmeans.fit(X)
    scores.append(silhouette_score(X,kmeans.predict(X)))
    n_clusters.append(n)
```

从这里，我将在 Kaggle 上单击“Commit ”,让它自己运行——运行一遍需要几个小时。

最后发现表现最好的聚类数是三个聚类，轮廓得分最高。

![](img/dbe7a2bd703daae5d0773d6e43379477.png)

现在我们有了文本标签，我们可以开始将电影作为一个整体进行聚类。然而，我们确实需要采取一些措施来清理数据。

比如`Release Year`从 1900 年代开始计时。如果取字面上的整数值，会使模型混乱。相反，让我们创建一个返回电影年龄的列`Age`，它就是 2017 年(数据库中最年轻的电影)减去电影发行的年份。

```
data['Age'] = data['Release Year'].apply(lambda x:2017-x)
```

现在，年龄从 0 开始计数，实际上意味着一些东西。

`Origin/Ethnicity`专栏很重要——一个故事的风格和味道通常可以追溯到它的来源。然而，这个列是绝对的，这意味着它的形式是`[‘American’, ‘Telegu’, ‘Chinese’]`。要把它转换成机器可读的东西，我们需要一次性编码，用 sklearn 的`OneHotEncoder`很容易做到。

```
from sklearn.preprocessing import OneHotEncoder
enc = OneHotEncoder(handle_unknown=’ignore’)
nation = enc.fit_transform(np.array(data[‘Origin/Ethnicity’]) .reshape(-1, 1)).toarray()
```

现在，nation 存储每一行的独热编码值。一行的每个索引代表一个惟一的值——例如，第一列(每行的第一个索引)代表`‘American’`。

![](img/8dcf25e395f4f3bff46689b639ea8193.png)

然而，目前，它只是一个数组——我们需要在数据中创建列，以便将信息真正转化为我们的数据。因此，对于每一列，我们将命名该向量的列对应的国家(`enc.categories_[0]`返回原始列的数组，`nation[:,i]`索引数组中每行的第`i`个值)。

```
for i in range(len(nation[0])):
    data[enc.categories_[0][i]] = nation[:,i]
```

![](img/c4822e0a9679e476f11a3c80ec5a6a13.png)

我们已经成功地将每个故事的国籍数据添加到我们的数据中。让我们对故事的类型做同样的处理。这可能比国籍更重要，因为它传达了关于故事在某种程度上讨论什么的信息，而机器学习模型根本不可能识别这些信息。

然而，有一个问题:

```
data[‘Genre’].value_counts()
```

![](img/c8d11e10efc0436a9aa771477c081588.png)

部分剪断；类别比较多。

似乎很多流派都不为人知。不要担心，我们稍后会解决这个问题。现在，我们的目标是对这种类型进行编码。我们将遵循与之前类似的过程，但略有不同——因为有太多的流派因名称不同而被视为不同(例如，“戏剧喜剧”和“浪漫喜剧”)，但几乎相同，我们将只选择最受欢迎的 20 个流派，并将其余的归入 20 个流派中的一个。

```
top_genres = pd.DataFrame(data['Genre'].value_counts()).reset_index().head(21)['index'].tolist()
top_genres.remove('unknown')
```

请注意，我们最终从列列表中删除了'`unknown`'，这就是最初选择前 21 个流行流派的原因。接下来，让我们根据`top_genres`处理流派，这样，如果一个流派不在前 20 个最流行的流派中，它将被替换为字符串值“`unknown`”。

```
def process(genre):
    if genre in top_genres:
        return genre
    else:
        return 'unknown'
data['Genre'] = data['Genre'].apply(process)
```

然后，像以前一样，我们将创建一个一键编码器的实例，并将它对数据的转换以数组形式保存到变量`genres`中。

```
enc1 = OneHotEncoder(handle_unknown='ignore')
genres = enc1.fit_transform(np.array(data['Genre']).reshape(-1, 1)).toarray()
```

为了将数组集成到数据中，我们将再次创建列，其中数据中的每一列都用数组中的一列填充。

```
for i in range(len(genres[0])):
    data[enc1.categories_[0][i]] = genres[:,i]
```

太好了！我们的数据是一次性编码的，但是我们仍然有一个未知值的问题。现在所有的数据都被一次性编码了，我们知道列`unknown`的值为 1 的行需要估算它们的类型。因此，对于每个需要估算类型的索引，我们将用一个`nan`值替换它们的类型，这样我们稍后使用的 KNN 估算器就可以识别出它是一个缺失值。

```
for i in data[data['unknown']==1].index:
    for column in ['action',
       'adventure', 'animation', 'comedy', 'comedy, drama', 'crime',
       'crime drama', 'drama', 'film noir', 'horror', 'musical', 'mystery', 'romance', 'romantic comedy', 'sci-fi', 'science fiction', 'thriller', 'unknown', 'war', 'western']:
        data.loc[i,column] = np.nan
```

既然所有缺失值都被标记为缺失，我们可以使用 KNN 估算值。然而，除了发行年份和国籍，恐怕没有太多的数据来估算这些数据。让我们使用 TF-IDF 矢量器，从故事中选择前 30 个单词，作为 KNN 正确分配体裁的额外补充信息。

文本必须总是事先清理，所以我们将使用正则表达式删除所有标点符号，并使一切小写。

```
import re
data['Cleaned'] = data['Plot'].apply(lambda x:re.sub('[^A-Za-z0-9]+',' ',str(x)).lower())
```

我们将设置英语标准停用词，并将最大功能数设置为 30。清理后的矢量化内容将以数组的形式存储到变量`X`中。

```
from sklearn.feature_extraction.text import TfidfVectorizer
vectorizer = TfidfVectorizer(stop_words=’english’,max_features=30)
X = vectorizer.fit_transform(data[‘Cleaned’]).toarray()
```

像前面一样，我们将把数组`X`中的每一列信息转移到我们的数据中的一列，用`X`中的列所对应的单词来命名每一列。

```
keys = list(vectorizer.vocabulary_.keys())
for i in range(len(keys)):
    data[keys[i]] = X[:,i]
```

![](img/ef285139c48cf653c464404534e93089.png)

这些词将为故事内容提供更多的语境，有助于推断故事类型。最后，是时候估算一下流派了！

```
from sklearn.impute import KNNImputer
imputer = KNNImputer(n_neighbors=5)
column_list = ['Age', 'American', 'Assamese','Australian', 'Bangladeshi', 'Bengali', 'Bollywood', 'British','Canadian', 'Chinese', 'Egyptian', 'Filipino', 'Hong Kong', 'Japanese','Kannada', 'Malayalam', 'Malaysian', 'Maldivian', 'Marathi', 'Punjabi','Russian', 'South_Korean', 'Tamil', 'Telugu', 'Turkish','man', 'night', 'gets', 'film', 'house', 'takes', 'mother', 'son','finds', 'home', 'killed', 'tries', 'later', 'daughter', 'family','life', 'wife', 'new', 'away', 'time', 'police', 'father', 'friend','day', 'help', 'goes', 'love', 'tells', 'death', 'money', 'action', 'adventure', 'animation', 'comedy', 'comedy, drama', 'crime','crime drama', 'drama', 'film noir', 'horror', 'musical', 'mystery','romance', 'romantic comedy', 'sci-fi', 'science fiction', 'thriller','war', 'western']imputed = imputer.fit_transform(data[column_list])
```

我们的估算器将识别标记为缺失值的`np.nan`值，并自动使用关于国籍和数据中的单词的包围数据，以及电影的年代来估算类型。结果以数组的形式保存到一个变量中。像往常一样，我们将转换数据:

```
for i in range(len(column_list)):
    data[column_list[i]] = imputed[:,i]
```

在删除了我们已经一次性编码或不再需要的列之后，比如用于`Genre`或分类`Genre`变量的`Unknown`列…

```
data.drop(['Title','Release Year','Director','Cast','Wiki Page','Origin/Ethnicity','Unknown','Genre'],axis=1,inplace=True)
```

…数据准备就绪，没有丢失值。KNN 插补的另一个有趣的方面是，它可以给出十进制值——也就是说，一部电影有 20%是西方的，还有其他一些是另一种类型，或者几种类型。

![](img/f89d52e43f28823c95c419f8d9b5838f.png)

这些特性都将很好地服务于集群。结合之前实现的文本聚类标签，这些特征应该是一个很好的指标，表明一个故事在人们的偏好领域中所处的位置。最后，我们可以开始聚类——像以前一样，我们将故事分成 3、4、5 或 6 个聚类，看看哪一个表现最好。

```
from sklearn.cluster import KMeans
from sklearn.metrics import silhouette_score
Xcluster = data.drop(['Plot','Summary','Cleaned'],axis=1)
score = []
for i in [3,4,5,6]:
    kmeans = KMeans(n_clusters=i)
    prediction = kmeans.fit_predict(Xcluster)
    score = silhouette_score(Xcluster,prediction)
    score.append(score)
```

绘制分数时…

![](img/25b6788c5900e9be36dfd375dd311bc5.png)

…像以前一样，三个集群表现最好，具有最高的轮廓分数。让我们仅在三个集群上训练我们的 KMeans:

```
from sklearn.cluster import KMeans
Xcluster = data.drop(['Plot','Summary','Cleaned'],axis=1)
kmeans = KMeans(n_clusters=3)
kmeans.fit(Xcluster)
pd.Series(kmeans.predict(Xcluster)).value_counts()
```

![](img/7f0c4e88df580e411be5ef76a3c73bd4.png)

每个集群都有相对均匀的电影分布，这很好。让我们得到聚类中心，可以用`.cluster_centers_`方法访问它:

```
centers = kmeans.cluster_centers_
centers
```

![](img/e671f1091f0f9af7cb9669c5053b195d.png)

部分剪断。

首先，让我们给每个项目分配标签。

```
Xcluster['Label'] = kmeans.labels_
```

对于每个聚类，我们希望找到在欧几里德距离方面最接近聚类中心的数据点。这将是该集群中最具代表性的一个。两点 *p* 和 *q* 之间的距离由 *p* 和 *q* 对应尺寸的差的平方和的平方根给出。你可以在这里找到我对欧几里德距离的证明(多维度中的勾股公式)[。](/understanding-the-mathematics-of-higher-dimensions-ab180e0bb45f?source=---------21------------------)

![](img/90a09df28258042a6ff8056a90b4d7ae.png)

因为欧几里德距离是 l2 范数，这可以用`numpy`的线性代数能力:`np.linalg.norm(a-b)`很容易地计算出来。

让我们查看整个代码以计算代码，并找到与分类具有最小欧几里得距离的故事。

```
for cluster in [0,1,2]:
    subset = Xcluster[Xcluster['Label']==cluster]
    subset.drop(['Label'],axis=1,inplace=True)
    indexes = subset.index
    subset = subset.reset_index().drop('index',axis=1)
    center = centers[cluster]
    scores = {'Index':[],'Distance':[]}
```

这将初始化搜索。首先，我们存储其标签等于我们当前正在搜索的分类的故事。之后，我们从子集中删除'【T3]'。为了存储原始索引供以后参考，我们将把索引存储到一个变量`indexes`中。之后，我们将重置`subset`上的索引以确保平滑的行索引。然后，我们将选择当前聚类的中心点，并启动一个包含两列的字典:一个列表用于存储主数据集中某个故事的索引，另一个列表用于存储得分/距离。

```
for index in range(len(subset)):
   scores['Index'].append(indexes[index])
   scores['Distance'].append(np.linalg.norm(center-np.array( subset.loc[index])))
```

这段代码遍历子集内的每一行，记录当前索引，并计算和记录它与中心的距离。

```
scores = pd.DataFrame(scores)
    print('Cluster',cluster,':',scores[scores['Distance']==scores['Distance'].min()]['Index'].tolist())
```

这将分数转换成熊猫数据帧用于分析，并打印出离中心距离最近的故事的索引。

![](img/dc0d7a7df86328440dfefe886e6adb7c.png)

看起来具有最近欧几里德距离的第一个分类有四个故事，但是分类 1 和分类 2 只有一个故事。让我们来看看这些是什么故事。

***集群 0:***

```
data.loc[4114]['Summary']
```

[输出]:

```
'On a neutral island in the Pacific called Shadow Island (above the island of Formosa), run by American gangster Lucky Kamber, both sides in World War II attempt to control the secret of element 722, which can be used to create synthetic aviation fuel.'
```

***集群 1:***

```
data.loc[15176]['Summary']
```

[输出]:

```
'Jake Rodgers (Cedric the Entertainer) wakes up near a dead body. Freaked out, he is picked up by Diane.'
```

***集群 2:***

```
data.loc[9761]['Summary']
```

[输出]:

```
'Jewel thief Jack Rhodes, a.k.a. "Jack of Diamonds", is masterminding a heist of $30 million worth of uncut gems. He also has his eye on lovely Gillian Bromley, who becomes a part of the gang he is forming to pull off the daring robbery. However, Chief Inspector Cyril Willis from Scotland Yard is blackmailing Gillian, threatening her with prosecution on another theft if she doesn\'t cooperate in helping him bag the elusive Rhodes, the last jewel in his crown before the Chief Inspector formally retires from duty.'
```

太好了！我们已经得到了三个最有区别的故事情节。虽然它对人类来说可能不那么有歧视性，但在我们的机器学习模型的头脑中，这些将立即给它提供最多的信息。

# 6 |推荐引擎

推荐引擎将只是一个机器学习模型，预测哪些电影情节有更高的机会被用户高度评价。该引擎将接受电影的特征，如其年龄或国籍，以及一个 TF-IDF 矢量化版本的摘要，限于 100 个特征(不同的词)。

每个电影情节的目标将是 1 或 0。模型将根据可用数据(用户已评级的故事)进行训练，并预测用户对故事进行正面评级的概率。接下来，最高概率图将被推荐给用户，并且用户对该故事的评级将被记录，并且该故事将被添加到训练数据列表中。

![](img/118a2388ab5a0aca6295b7adc16afa71.png)

作为训练数据，我们将简单地使用每部电影的数据属性。

![](img/d58b4b03735aeed0bcec7c85093de12e.png)

我们可能会想要一个决策树分类器，因为它可以进行有效的预测，快速训练，并开发一个高方差的解决方案，这是推荐系统努力的方向。

在下一部分中，我们将把我们之前的工作整合在一起。

# 7 |将所有这些整合在一起

我们先从用户对三部最具歧视性的电影的评分进行编码开始。对于每个输入，程序确保输出为 0 或 1。

```
import time
starting = []
print("Indicate if like (1) or dislike (0) the following three story snapshots.")print("\n> > > 1 < < <")
print('On a neutral island in the Pacific called Shadow Island (above the island of Formosa), run by American gangster Lucky Kamber, both sides in World War II attempt to control the secret of element 722, which can be used to create synthetic aviation fuel.')
time.sleep(0.5) #Kaggle sometimes has a glitch with inputs
while True:
    response = input(':: ')
    try:
        if int(response) == 0 or int(response) == 1:
            starting.append(int(response))
            break
        else:
            print('Invalid input. Try again')
    except:
        print('Invalid input. Try again')print('\n> > > 2 < < <')
print('Jake Rodgers (Cedric the Entertainer) wakes up near a dead body. Freaked out, he is picked up by Diane.')
time.sleep(0.5) #Kaggle sometimes has a glitch with inputs
while True:
    response = input(':: ')
    try:
        if int(response) == 0 or int(response) == 1:
            starting.append(int(response))
            break
        else:
            print('Invalid input. Try again')
    except:
        print('Invalid input. Try again')print('\n> > > 3 < < <')
print("Jewel thief Jack Rhodes, a.k.a. 'Jack of Diamonds', is masterminding a heist of $30 million worth of uncut gems. He also has his eye on lovely Gillian Bromley, who becomes a part of the gang he is forming to pull off the daring robbery. However, Chief Inspector Cyril Willis from Scotland Yard is blackmailing Gillian, threatening her with prosecution on another theft if she doesn't cooperate in helping him bag the elusive Rhodes, the last jewel in his crown before the Chief Inspector formally retires from duty.")
time.sleep(0.5) #Kaggle sometimes has a glitch with inputs
while True:
    response = input(':: ')
    try:
        if int(response) == 0 or int(response) == 1:
            starting.append(int(response))
            break
        else:
            print('Invalid input. Try again')
    except:
        print('Invalid input. Try again')
```

![](img/50ec9ce69cb4df4687af6f1545737ed9.png)

这个很好用。接下来，让我们将数据存储到训练数据集 DataFrame 中，并从数据中删除这些索引。

```
X = data.loc[[9761,15176,4114]].drop( ['Plot','Summary','Cleaned'],axis=1)
y = starting
data.drop([[9761,15176,4114]],inplace=True)
```

现在，是时候创建一个循环了。我们将在当前的训练集上训练一个决策树分类器。

```
from sklearn.tree import DecisionTreeClassifier
subset = data.drop(['Plot','Summary','Cleaned'],axis=1)
while True:
    dec = DecisionTreeClassifier().fit(X,y)
```

然后，对于数据中的每一个指标，我们都会做一个概率预测。

```
dic = {'Index':[],'Probability':[]}
subdf = shuffle(subset).head(10_000) #select about 1/3 of data
for index in tqdm(subdf.index.values):
     dic['Index'].append(index)
     dic['Probability'].append(dec.predict_proba(  np.array(subdf.loc[index]).reshape(1, -1))[0][1])
     dic = pd.DataFrame(dic)
```

为了确保快速选择，我们将随机选择大约 1/3 的数据，方法是随机排列数据并选择前 10，000 行。该代码将索引保存到数据帧中。

![](img/467bb8a4e884968cf70c14e5880c35fb.png)

最初，它以概率 1 对许多电影进行评级，但随着我们的进步和模型学习更多，它将开始做出更高级的选择。

```
index = dic[dic['Probability']==dic['Probability'].max()] .loc[0,'Index']
```

我们将把表现最好的电影的索引保存到变量`index`中。

现在，我们需要从`data`中获取关于`index`的信息并显示出来。

```
print('> > > Would you be interested in this snippet from a story? (1/0/-1 to quit) < < <')
print(data.loc[index]['Summary'])
time.sleep(0.5)
```

在验证用户的输入是 0、1 或-1 之后，退出…

```
while True:
        response = input(':: ')
        try:
            if int(response) == 0 or int(response) == 1:
                response = int(response)
                break
            else:
                print('Invalid input. Try again')
        except:
            print('Invalid input. Try again')
```

…我们可以开始增加我们的训练数据。然而，首先，如果用户想退出，我们必须打破这个循环。

```
if response == -1:
        break
```

此外，不管用户喜欢还是不喜欢这部电影，我们仍然要将它添加到训练数据中(目标只是不同):

```
X = pd.concat([X,pd.DataFrame(data.loc[index].drop(['Plot','Summary','Cleaned'])).T])
```

最后，如果响应等于 0，我们将把 0 附加到 *y* 上。用户不想喜欢这个故事。

```
if response == 0:
        y.append(0)
```

然而，如果用户喜欢，程序将打印完整的故事。

```
else:
        print('\n> > > Printing full story. < < <')
        print(data.loc[index]['Plot'])
        time.sleep(2)
        print("\n> > > Did you enjoy this story? (1/0) < < <")
```

我们将再次收集用户的输入，并确保它是 0 或 1，

```
while True:
      response = input(':: ')
      try:
          if int(response) == 0 or int(response) == 1:
              response = int(response)
              break
          else:
              print('Invalid input. Try again')
      except:
          print('Invalid input. Try again')
```

…并相应地给 *y* 加 0 或 1。

```
if response == 1:
      y.append(1)
else:
      y.append(0)
```

最后，我们将从数据中删除这个故事，这样用户就不会再看到同一个故事。

```
data.drop(index,inplace=True)
```

我们已经完成了！每次迭代，训练数据都会更新，模型会越来越精确。

# 感谢阅读！

我希望你喜欢！也许你会用这个程序来阅读几个有趣的情节，或者可能会检查情节所属的电影。在这个孤立的时期，处理数据中的问题和挑战，创造一些东西来娱乐我们，这很有趣。

如果你想玩玩这个程序，你可以在这里找到它。它仍然可以得到显著的改进，所以我对你们可以用这些代码做什么很感兴趣！