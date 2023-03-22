# 如何提取评论分类器模型以重用于实时分类

> 原文：<https://towardsdatascience.com/how-to-deploy-a-review-classifier-in-any-application-c1c0e5a0e8ff?source=collection_archive---------25----------------------->

## 使分类器可重用简介

有很多教程和例子都是关于用大部分可用数据集训练一个评论分类器，并用剩余部分测试它，看看它的表现如何。

很高兴看到如何建立一个分类器，以及它是如何执行的，但是我们如何导出这个分类器，并在任何应用程序中安装、运行，以便我们可以进行实时检查分类呢？

在本帖中，首先我们将利用欧洲 515，000 条酒店评论数据构建一个评论分类器，并对其进行一些领域调整，使其更加通用，而不是针对特定酒店，然后**我们将导出带有特征索引的分类器，以应用于我们选择的任何应用程序。**

> 如果你只对我们如何在其他应用程序中提取和应用分类器感兴趣，你可以去我们有一个分类器，现在做什么？部分。

点击这里下载数据集[。](https://www.kaggle.com/jiashenliu/515k-hotel-reviews-data-in-europe)

## 酒店数据集调查

我们这里有一个庞大的酒店数据集，它有许多功能，酒店地址，评论日期，平均分数，正面评论，负面评论等等。数据集中的每个条目都来自一个特定酒店的用户。每个用户都被要求对他们住过的酒店提供正面和负面的评价。值得一提的是，在某些情况下，用户只能选择其中之一。当用户未分别提供正面和负面评价时，我们会看到以下特征值:

*   没有阳性
*   没有负面影响

在这篇文章中，我们将只关注正面评论和负面评论，而不管哪个用户写了哪个酒店。

# 预处理

我们需要对数据集做一些预处理。首先，我们将创建一个新的数据集，只获取 1%的原始数据集的正面和负面评论。(在本文中，我们将只使用 5K 个数据条目，但是您可以稍后尝试使用整个数据集进行构建)。然后，我们将清除遗漏的评论(无负面、正面、无等等)。

## 删除空洞和无价的评论

让我们为项目创建一个文件夹，在该文件夹中找到数据集，然后创建一个新的 python 文件进行预处理。我把它命名为 hotel_review_pre_processor。

首先，我们将导入 pandas 和 numpy，然后加载原始数据集的 1%。

```
import pandas as pd
import numpy as np# load data
reviews_df = pd.read_csv("Hotel_Reviews.csv")#only get 1 percent of data. because the data is huge
reviews_df = reviews_df.sample(frac = 0.01, replace = False, random_state=42)
```

然后，让我们创建一个新的空数据框架，其中包含点评和 is_positive 特征，稍后我们将使用酒店数据集中的点评填充这些特征。

```
df = pd.DataFrame(columns=('review', 'is_positive'))
```

现在，我们将用空字符串替换数据集中的非正值和非负值，然后用 NaN 替换它们，最后删除它们:

```
#replace missing review info with empty strings
reviews_df = reviews_df.replace(['No Negative', 'No Positive'], ['', ''])#replace empty strings with NaN value (null) and then drop them.
reviews_df = reviews_df.replace(r'^\s*$', np.nan, regex=True)
reviews_df = reviews_df.dropna()
```

然后，我们将执行以下操作:

*   仅从数据集中获取负面评论和正面评论列。
*   为它们中的每一个创建单独的数据框
*   对于每个数据框中的每个条目，使用 review 和相应的 is_positive 值(0 表示负面评论，1 表示正面评论)向我们的新数据框(df)添加一个新条目。
*   然后，我们会将新的数据框保存为 reviews.csv。

```
#only get reviews.
reviews_df = reviews_df[["Negative_Review", "Positive_Review"]]#separate them
negative_reviews_df = reviews_df[["Negative_Review"]]
positive_reviews_df = reviews_df[["Positive_Review"]]#add all negative and positive reviews with related label 'is_positive' 0 for negative, 1 for positive.
for index, row in negative_reviews_df.iterrows():
    if "nothing" in row["Negative_Review"]:
        pass
    elif "Nothing" in row["Negative_Review"]:
        pass
    else:
        df = df.append({'review': row["Negative_Review"], 'is_positive': 0}, ignore_index=True)for index, row in positive_reviews_df.iterrows():
    if "nothing" in row["Positive_Review"]:
        pass
    elif "Nothing" in row["Positive_Review"]:
        pass
    else:
        df = df.append({'review': row["Positive_Review"], 'is_positive': 1}, ignore_index=True)#save the cleaned data as reviews.csv
endResult = df.to_csv('reviews.csv',index=False)
```

现在我们完成了预处理。我们将酒店数据集中的评论放入一个名为 reviews.csv 的新数据集中，该数据集中有 review 和 is_positive 功能。现在我们应该有大约 6500 条评论可供我们分析。正如我前面提到的，通过在下面一行中更改 frac 参数，您可以增加或减少您使用的数据集部分:

```
reviews_df = reviews_df.sample(frac = 0.01, replace = False, random_state=42)
```

## 使评论更加通用

现在我们已经处理了这些评论，并将它们重构为一个新的数据集，其中包含了我们感兴趣的特性，但是我们仍然有一个问题。由于原始数据集来自酒店评论，它们很可能包含酒店行业特有的词，如:酒店、床、淋浴、早餐等。如果我们想让我们的分类器以后变得通用，我们需要去掉这样的词。

但是首先，我们将去掉停用词(没有任何上下文含义的词，如:for、the、a、or、what 和 etc ),然后对评论中现有的词进行词干处理以删除重复的词。作为一个例子，当词干分析应用于下面的任何一个单词时，它们都将导致**运行**。

*   (跑步，跑步，跑步)->跑步

继续创建一个名为 review_cleaner.py 的文件。

我们将把它创建为一个单独的类，因为我们稍后将从多个文件中再次需要这个方法:

```
import nltk
from nltk.corpus import stopwords
from nltk.stem.porter import PorterStemmerclass ReviewCleaner:
    [@staticmethod](http://twitter.com/staticmethod)
    def clean_review(review):
        review = review.lower()
        review = review.split() ps = PorterStemmer() review = [ps.stem(word) for word in review
                    if not word in set(stopwords.words('english'))] review = ' '.join(review) return review
```

该方法将如下工作:

*   接受名为 review 的字符串参数
*   降低所有字符并拆分单词
*   对每个单词应用词干并删除任何停用词(如果存在)。
*   用带词干的单词创建一个名为 review 的新字符串并返回它

> 如果您曾经得到以下错误:“资源停用词没有找到”，即使您有 nltk 库。打开终端并执行以下命令:
> 
> - python3
> 
> -导入 nltk
> 
> - nltk.download('停用字词')
> 
> -这应该可以修复错误。

现在，让我们创建一个名为 review _ generifier.py 的文件，并添加以下代码块:

```
import pandas as pd
import numpy as np
from collections import Counter
from review_cleaner import ReviewCleaner as rcreviews_df = pd.read_csv("reviews.csv")for index, row in reviews_df.iterrows():
    cleared_review = rc.clean_review(row['review'])
    reviews_df.at[index, 'review'] = cleared_reviewmost_common_100_words = Counter(" ".join(reviews_df["review"]).split()).most_common(100)print(most_common_100_words)
```

> 请注意，根据您的机器，对数据集中的每个数据条目应用清除需要一段时间。

这里，我们将 ReviewCleaner 类中的 clean_review 方法应用于数据集中可用的每个评论。然后我们打印出出现次数最多的 100 个单词以及出现次数。(为简单起见，仅显示了 20 个):

```
[('room', 3067), ('staff', 1697), ('locat', 1596), ('hotel', 1499), ('breakfast', 1171), ('good', 1057), ('bed', 831), ('great', 771), ('help', 665), ('clean', 658), ('nice', 637), ('friendli', 636), ('comfort', 540), ('small', 513), ('stay', 478), ('excel', 469), ('bathroom', 393), ('love', 384), ('would', 382), ('could', 358)]
```

如你所见，有这样的词；房间、员工、酒店、早餐等(你可以在整个打印列表上看到更多)都是酒店行业特有的。如果我们想使它通用，我们不希望这样的词被输入到我们的分类器中。因此，我们现在要删除这些词。

现在，让我们创建一个要从评论中删除的单词列表:

```
unwanted_words = ['room', 'staff', 'locat', 'hotel', 'breakfast', 'bed', 'shower']
```

> 您还可以/应该尝试删除其他单词，看看它是否会影响您的分类器的准确性。

我们需要导入 **re** 来删除不需要的单词。使用其他导入在文件顶部导入 re，如下所示:

```
import re
```

现在，让我们从数据框中删除不需要的单词，如下所示:

```
reviews_df['review'] = reviews_df['review'].str.replace('|'.join(map(re.escape, unwanted_words)), '')
```

您可以获取常用单词并在更新数据框后再次打印它们，您应该会看到在 wanted_words 中声明的单词现在已经消失了。

现在，我们将把更新后的数据集保存为 **cleared_reviews.csv** ，并在末尾添加以下代码:

```
#save the cleaned data as cleared_reviews.csv
endResult = reviews_df.to_csv('cleared_reviews.csv')
```

从现在开始，我们将使用 cleared_reviews.csv 数据集:)

# 用随机森林分类器训练评论分类器

现在我们有了通用数据集，我们可以训练一个随机森林分类器(RFC)[【1】](/understanding-random-forest-58381e0602d2)模型作为我们的通用分类器。(请注意，审查分类器可以用许多不同的方法实现，我只想用 RFC)。

现在，让我们创建一个新文件 rf_classifier_training.py，并添加以下代码:

```
import pandas as pd
import numpy as np
import pickle
import jsondataset = pd.read_csv('cleared_reviews.csv')from sklearn.feature_extraction.text import CountVectorizer
cv = CountVectorizer(max_features = 1500)
X = cv.fit_transform(dataset["review"].values.astype('U')).toarray()
y = dataset["is_positive"]from sklearn.model_selection import train_test_splitX_train, X_test, y_train, y_test = train_test_split(X, y, test_size = 0.25)from sklearn.ensemble import RandomForestClassifiermodel = RandomForestClassifier(n_estimators = 501,
                            criterion = 'entropy')model.fit(X_train, y_train)
y_pred = model.predict(X_test)print('Score: ', model.score(X_test, y_test))
```

这里我们训练一个分类器如下:

*   初始化 CountVectorizer 以从我们的评论中获取 1500 个特征，这里每个特征对应一个词(它选择数据集中出现最多的 1500 个词)。
*   将数据集拆分为 75%和 25%，分别用于定型和预测。
*   创建一个 RFC 模型，包含 n 个估计量和标准参数(不同的参数值可以提高性能，如果你愿意可以试试)
*   训练模型，然后根据测试数据进行预测
*   最后打印出分类得分的分数。

打印分数应该在 0.90 左右，这意味着每 10 篇评论中，有 9 篇被成功归类为当前标签(正面或负面)。我认为这对 RFC 来说是相当好的，因为我们没有试图优化它的参数。

# 我们有了一个分类器，现在怎么办？

现在我们有了一个分类器，可以将评论分为正面或负面。但是我们如何提取它，以便在应用程序中对新数据进行分类呢？

为此，我们需要保存我们的模型。但这还不够。因为如果我们对特征(单词)一无所知，我们如何将新数据放入这个模型。更具体地说，我们不知道哪个词在特征空间中有什么索引。因此，我们还将为每个单词找到特征索引，并分别保存它们。

在 rf_classifier_training.py 中打印分数后，立即添加以下代码:

```
words = cv.vocabulary_
words_for_json = {}for k, v in words.items():
    words_for_json[k] = int(v)pickle.dump(model, open('reviewClassifier.pkl','wb'))
with open('word_feature_space.json', 'w') as fp:
    json.dump(words_for_json, fp)
```

[word: index]字典保存为 word_feature_space.json 下的 json 键值，分类器模型保存为 reviewClassifier.pkl。

现在再次运行 rf_classifier_training.py 来保存 json 和 pkl 文件。

> 这些文件不会包含在 github repo 中，因为模型的大小大约为 100 Mb。所以你必须运行 rf_classifier_training.py 来获取这些文件。

## 编写可以在任何应用程序中使用的分类器服务

现在我们有了模型和 json，我们可以编写一个服务来为任何给定的文本提供分类。

我们的服务如下:

*   清理收到的审核
*   用收到的评论创建一个向量
*   将这个向量拟合到模型中并进行预测
*   以二进制形式返回预测值(0 =负，1 =正)

让我们创建 review_classifier_service.py，如下所示:

ReviewClassifierService

值得从评论中给出关于向量创建的更多细节，我们检查评论中的每个词，如果它存在于我们的词空间(word_feature_space.json)中，我们将它添加到向量空间并增加字数，但是如果它不存在，我们就简单地丢弃它。

## 请求

现在，我们将向服务发出一个示例请求，以查看我们的分类器如何处理新数据，让我们创建 example_request.py:

```
from review_classifier_service import ReviewClassifierServiceservice = ReviewClassifierService()positive_sample = "It was great"
negative_sample = "It was horrible"
print(service.classify(positive_sample))
print(service.classify(negative_sample))
```

输出是:

```
{"outcome": 1}
{"outcome": 0}
```

如您所见，我们的分类器服务可以提供一个 json 响应，值 1 表示肯定，值 0 表示否定。为了简单起见，我在这里没有尝试不同的评论，但是它应该工作得很好，除非你提供一个棘手的评论(一个有许多正面和负面单词的评论=D)。请继续尝试不同的评论。

# 结论

在本文中，我们看到了如何处理数据以使其有用，如何使数据集更通用，如何构建评论分类器，更重要的是，如何提取该分类器模型并使其能够应用于我们想要的任何应用程序。您可以使用这些或类似的方法来获得您的分类器，然后将它们部署在您的后端，并为许多应用程序和客户端提供服务。

您可以在这里找到完整的存储库[。](https://github.com/emrepun/PunReviewClassifier)

保重:)