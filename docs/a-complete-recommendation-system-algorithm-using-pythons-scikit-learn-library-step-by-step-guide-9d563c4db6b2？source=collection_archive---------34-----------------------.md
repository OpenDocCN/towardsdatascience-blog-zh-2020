# 使用 Python 的 Scikit-Learn 库的完整推荐系统算法:分步指南

> 原文：<https://towardsdatascience.com/a-complete-recommendation-system-algorithm-using-pythons-scikit-learn-library-step-by-step-guide-9d563c4db6b2?source=collection_archive---------34----------------------->

![](img/6adaaf1b26d70484cbe978315c70ff63.png)

罗伯特·祖尼科夫在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄的照片

## 一个简单而有用的算法，只有几行代码

推荐系统的开发可以是自然语言处理(NLP)中的常见任务。YouTube 或网飞使用类似的技术向他们的客户推荐。他们分析客户以前的行为，并在此基础上为他们推荐类似的材料。在本文中，我将讨论如何使用 python 中的 scikit-learn 库开发一个电影推荐模型。它涉及许多复杂的数学。但是 scikit-learn 库有一些很棒的内置函数，可以处理大部分繁重的工作。随着练习的进行，我将解释如何使用这些函数及其工作。

# 数据集概述

在这个练习中，我将使用一个电影数据集。我在这一页的底部给出了数据集的链接。请随意下载数据集，并在笔记本上运行所有代码，以便更好地理解。以下是数据集的链接:

[](https://github.com/rashida048/Some-NLP-Projects/blob/master/movie_dataset.csv) [## rashida 048/Some-NLP-项目

### 此时您不能执行该操作。您已使用另一个标签页或窗口登录。您已在另一个选项卡中注销，或者…

github.com](https://github.com/rashida048/Some-NLP-Projects/blob/master/movie_dataset.csv) 

以下是电影推荐模型的逐步实现:

1.  导入包和数据集。

```
from sklearn.metrics.pairwise import cosine_similarity
import pandas as pd
import numpy as np
from sklearn.feature_extraction.text import CountVectorizer
from sklearn.metrics.pairwise import cosine_similaritydf = pd.read_csv("movie_dataset.csv")
```

2.数据集太大。因此，我不能在这里显示截图。以下是数据集的列。它们不言自明。列名会告诉你里面的内容是什么。

```
df.columns#Output:
Index(['index', 'budget', 'genres', 'homepage', 'id', 'keywords',        'original_language', 'original_title', 'overview', 'popularity',        'production_companies', 'production_countries', 'release_date',        'revenue', 'runtime', 'spoken_languages', 'status', 'tagline', 'title',        'vote_average', 'vote_count', 'cast', 'crew', 'director'],       dtype='object')
```

3.选择要用于模型的特征。我们不需要使用所有的功能。有些不适合这个模型。我选择这四个特征:

```
features = ['keywords','cast','genres','director']
```

请随意为实验添加更多功能或不同的功能。现在，将这些特征组合起来，用这四列组成一列。

```
**def** combine_features(row):
    **return** row['keywords']+" "+row['cast']+" "+row['genres']+" "+row['director']
```

4.如果我们有空值，这可能会在算法中产生问题。用空字符串填充空值。

```
for feature in features:
    df[feature] = df[feature].fillna('')
df["combined_features"] = df.apply(combine_features,axis=1)
```

5.将数据拟合并转换到“计数矢量器”函数中，为矢量表示准备数据。当您通过“计数矢量器”函数传递文本数据时，它会返回每个单词的计数矩阵。

```
cv = CountVectorizer() 
count_matrix = cv.fit_transform(df["combined_features"])
```

6.使用“余弦相似度”来查找相似度。这是一种寻找相似性的动态方法，该方法测量多维空间中两个向量之间的余弦角。这样，文档的大小就无关紧要了。这些文档可能相距很远，但它们的余弦角可能相似。

```
cosine_sim = cosine_similarity(count_matrix)
```

这个“余弦 _sim”是一个二维矩阵，它是一个系数矩阵。我不打算在这里详细解释余弦相似性，因为这超出了本文的范围。我想展示，如何使用它。

7.我们需要定义两个函数。其中一个函数从索引中返回标题，另一个函数从标题中返回索引。我们将很快使用这两个功能。

```
def find_title_from_index(index):
    return df[df.index == index]["title"].values[0]
def find_index_from_title(title):
    return df[df.title == title]["index"].values[0]
```

8.拍一部我们用户喜欢的电影。就拿《星球大战》来说吧。然后用上面的函数找到这部电影的索引。

```
movie = "Star Wars"
movie_index = find_index_from_title(movie)
```

《星球大战》的指数是 2912。正如我前面提到的，步骤 6 中的“余弦 _sim”是相似性系数的矩阵。该矩阵的行 2912 应该提供所有具有“星球大战”的电影的相似性系数。所以，找到矩阵‘余弦 _ sim’的第 2912 行。

```
similar_movies = list(enumerate(cosine_sim[movie_index]))
```

我使用枚举来获取索引和系数。‘similar _ movies’是包含索引和系数的元组列表。我喜欢在 python 中使用“枚举”。很多时候它会派上用场。

9.按照系数的逆序对列表“相似电影”进行排序。这样，最高的系数将在顶部。

```
sorted_similar_movies = sorted(similar_movies,key=lambda x:x[1],reverse=True)[1:]
```

我们没有从列表中选择第一部，因为列表中的第一部将是《星球大战》本身。

10.使用函数“find_title_from_index”获得与“星球大战”相似的前五部电影。

```
i=0
for element in sorted_similar_movies:
    print(find_title_from_index(element[0]))
    i=i+1
    if i>5:
        break
```

与《星球大战》最相似的五部电影是:

帝国反击战

绝地归来

星球大战:第二集——克隆人的进攻

星球大战:第三集——西斯的复仇

星球大战:第一集——幽灵的威胁

螺旋…加载

有道理，对吧？这就是电影推荐模型。如果你有任何问题，请随时问我。这是我在本教程中使用的数据集。

## 结论

这个推荐系统非常简单，但是很有用。这种类型的推荐系统在许多现实场景中都很有用。希望你能善用它。

## 更多阅读:

[](/a-complete-anomaly-detection-algorithm-from-scratch-in-python-step-by-step-guide-e1daf870336e) [## Python 中从头开始的完整异常检测算法:分步指南

### 基于概率的异常检测算法

towardsdatascience.com](/a-complete-anomaly-detection-algorithm-from-scratch-in-python-step-by-step-guide-e1daf870336e) [](/a-complete-k-mean-clustering-algorithm-from-scratch-in-python-step-by-step-guide-1eb05cdcd461) [## Python 中从头开始的完整 K 均值聚类算法:分步指南

### 还有，如何使用 K 均值聚类算法对图像进行降维

towardsdatascience.com](/a-complete-k-mean-clustering-algorithm-from-scratch-in-python-step-by-step-guide-1eb05cdcd461) [](/best-free-courses-for-computer-science-software-engineering-and-data-science-50cd88cafd74) [## 编程、软件工程和数据科学的最佳免费课程

### 麻省理工、哈佛和斯坦福等顶尖大学的免费课程

towardsdatascience.com](/best-free-courses-for-computer-science-software-engineering-and-data-science-50cd88cafd74) [](/how-to-present-the-relationships-amongst-multiple-variables-in-python-70f1b5693f5) [## 如何在 Python 中呈现多个变量之间的关系

### 了解如何使用 Python 中的多元图表和绘图来呈现要素之间的关系

towardsdatascience.com](/how-to-present-the-relationships-amongst-multiple-variables-in-python-70f1b5693f5) [](/logistic-regression-model-fitting-and-finding-the-correlation-p-value-z-score-confidence-8330fb86db19) [## 逻辑回归模型拟合和寻找相关性，P 值，Z 值，置信度…

### 静态模型拟合和提取的结果从拟合模型使用 Python 的 Statsmodels 库与…

towardsdatascience.com](/logistic-regression-model-fitting-and-finding-the-correlation-p-value-z-score-confidence-8330fb86db19)