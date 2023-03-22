# 用数据科学分析音扇艺术

> 原文：<https://towardsdatascience.com/analyzing-sonic-fan-art-with-data-science-fddcaa8bbb68?source=collection_archive---------48----------------------->

## 使用 BeautifulSoup 刮除异常值的教程

音速小子的粉丝已经达到了互联网上很少有粉丝享有的恶名程度。众所周知，这种艺术是扭曲的、令人不安的，而且在很多情况下是露骨的。在我最新的 Youtube 视频中，我搜集了 DeviantArt 来分析扇子艺术，以确定它是否真的名副其实。这篇文章将带你了解我是如何做到的。

我首先想了解一下 DeviantArt(一个粉丝艺术分享网站)上有多少声波艺术作品。这里不需要刮擦——我只是在 DeviantArt 上搜索“Sonic ”,并记录出现了多少结果。我也为相似的角色做了同样的事情，像史莱克和皮卡丘。以下是这些可视化的结果:

![](img/e9174aec833cb8a7346516e41382a32b.png)

DeviantArt 上每个角色的粉丝数量。根据知识共享协议授权的作者和人物剪贴画。

献给索尼克的粉丝数量超过了其他角色，达到了 140 万左右。哇！既然我们已经看到了 DeviantArt 上蓬勃发展的声波文化，是时候回答我们的研究问题了:声波粉丝艺术真的那么令人不安吗？

首先，我使用 BeautifulSoup 来收集帖子(完整代码可以在 Github 的[这里获得)。下面的代码抓取了搜索“sonic”时出现的前 2200 个链接](https://github.com/vastava/data-science-projects/tree/master/sonic)

```
base_url = "[https://www.deviantart.com/search/deviations](https://www.deviantart.com/search/deviations)?"
urls = np.array([])
for i in range(50):
    if i == 0:
        url = base_url + "q=sonic"
    else:
        url = base_url + "page=" + str(i) + "&q=sonic"
    request=urllib.request.Request(url,None,headers)
    if url in urls:
        pass
    else:
        bs = BeautifulSoup(urlopen(request), "html.parser")
        links = [item.get("href") for item in bs.find_all(attrs={"data-hook" : "deviation_link"})]
        urls = np.append(urls, links)

len(urls)
```

然后，我从每个 URL 中检索了几个属性，包括帖子的标题、标签以及每个帖子的浏览量、收藏和评论。下面是检索这些数据的代码:

```
for i in range(num):
    print(deviationurls[i])
    request=urllib.request.Request(deviationurls[i],None,headers) 
    bs = BeautifulSoup(urlopen(request), "html.parser")
    vals = [item.text for item in bs.find_all("span", class_="iiglI")]
    tag = [item.text for item in bs.find_all("span", class_="_3uQxz")]
    #print(vals)
    if len(vals) == 3:
        faves.append(vals[0])
        comments.append(vals[1])
        views.append(vals[2])
    else:
        faves.append(vals[0])
        comments.append(0)
        views.append(vals[1])
    tags.append(tag)
    titles.append(bs.find_all("h1")[0].text)
```

有时，注释字段为空，因此出现 if/else 条件。

这样做之后，你现在可以构建一个用于分析的声波风扇艺术品的数据框架！你的可能看起来和我的不一样，因为刮的时候可能会出现不同的艺术作品供你查询。但这是我的刮刀产生的数据集。

这是我的数据集中的艺术作品出版的年份分布。你可以看到 2010 年代的作品代表了大多数——也许人们在这个时候开始张贴更多的作品，或者也许那些作品只是我刮的时候首先出现的。

![](img/6c6c73144db48a72319a639f4fc7bb68.png)

音帆作品出版年份分布。图片作者。

我不能在这篇博文中包含任何来自数据集的图片，因为我没有权利在这里重新发布它们。然而，我确实对[我的视频](https://www.youtube.com/watch?v=x_XR-K1cL7w)中观看次数最多、最受欢迎的 10 件粉丝作品做出了反应。

在我的数据集中，两个被浏览最多的艺术品是人物塑造者([第一](https://www.deviantart.com/chriserony/art/Sonic-Charrie-Maker-108512914)和[第二](https://www.deviantart.com/sonicschilidog/art/Sonic-Fan-Character-Doll-Maker-178459528))。这并不奇怪，因为音速爱好者的很大一部分是制作原创角色，或“OCs”，并围绕这些角色写故事，就像任何爱好者一样。(然而，声波迷将这种消遣带到了一个新的水平。试着谷歌一下你的名字+“刺猬”，看看我在说什么)

[动画短片](https://www.deviantart.com/thewax/art/Sonic-and-a-balloon-21494567)、[漫画](https://www.deviantart.com/bonus-kun/art/Sonic-s-Eye-Infection-98655916)和[人物](https://www.deviantart.com/darkspeeds/art/Sonic-Character-Face-Reference-85228381) [参考文献](https://www.deviantart.com/darkspeeds/art/Sonic-Character-Eyes-Reference-41545204)也很受欢迎。

我的最终目标是分析 DeviantArt 上的艺术家使用的标签。哪些标签与哪些标签一起使用？为了做到这一点，我在我的数据框架中使用了“标签”列来创建一个关联矩阵。这是一个有点复杂的过程——第一步是创建一个用于解析的标签“语料库”。

```
data = np.unique(df.tags)[1:]
(data[0])
data_real = []
for strdata in data:
    str1 = strdata.replace(']','').replace('[','')
    l = str1.replace("'",'').split(",")
    l = [item.strip(' ') for item in l]
    data_real.append(l)texts = [[word.lower() for word in line] for line in data_real]
corpus = []
for text in texts:
 corpus.append(‘ ‘.join(text))
corpus
```

创建完语料库后，我使用 scikit-learn 创建了相关矩阵。

```
from sklearn.feature_extraction.text import CountVectorizer
cv = CountVectorizer(ngram_range=(1,1), stop_words = 'english') # You can define your own parameters
X = cv.fit_transform(corpus)
Xc = (X.T * X) # This is the matrix manipulation step
Xc.setdiag(0)
names = cv.get_feature_names() # This are the entity names (i.e. keywords)
df_tags = pd.DataFrame(data = Xc.toarray(), columns = names, index = names)
df_tags
```

最终的数据帧应该是这样的(是的，它会很大)。这里，数字表示两个标签相互使用的时间。

![](img/116aec1916d64429761b57bae63da7f5.png)

标签之间相关性的数据框架。图片作者。

我首先使用像 networkx 和 matplotlib 这样的 Python 库来可视化网络，但是这些都是难以分辨的混乱。很明显，我需要一个更强大的软件。

![](img/0ae2363c6acde26d96ba558218141bed.png)![](img/3d60c17a35244684606ee8fea99d7d4e.png)

我开发的第一个网络。图片作者。

我将数据帧保存到一个 CSV 文件中，并输入到 gephi 中。在网络中，单词越大，使用的次数就越多。一个单词和另一个单词越接近，这些标签一起使用的次数就越多。在对网络功能进行微调后，它最终看起来是这样的:

![](img/ce6fba457487a00f626ce47befc88793.png)

声波风扇艺术标签网络。

这个网络显然是巨大的，但环顾四周是很有趣的——我建议花几分钟放大和环绕它，看看某些单词之间的联系。或者，你可以看一下[我的总结这个分析的 YouTube 视频](https://www.youtube.com/watch?v=x_XR-K1cL7w)，看看我从网络上拿了什么。

就这样结束了！总之，虽然有令人不安或扭曲的声波风扇艺术的例子，但我认为我的分析表明，这肯定不是社区中最受欢迎的，甚至不是大多数作品。事实上，其中大部分是有益健康的。

如果你认为这篇文章还有很多需要改进的地方，请考虑观看我关于这个主题的视频。这篇文章更多的是解释我是如何做分析的。