# 如何使用管道的魔力

> 原文：<https://towardsdatascience.com/how-to-use-the-magic-of-pipelines-6e98d7e5c9b7?source=collection_archive---------46----------------------->

## 使用 ETL 拯救世界

![](img/7d0f6c4c2d30b3d076b41d573924e5d5.png)

斯文·库契尼奇在 [Unsplash](https://unsplash.com/) 上的照片

您肯定听说过管道或 ETL(提取转换负载)，或者见过库中的一些方法，甚至听说过任何创建管道的工具。但是，您还没有使用它。所以，让我向你介绍管道的奇妙世界。

在了解如何使用它们之前，我们必须了解它是什么。

管道是一种包装和自动化流程的方式，这意味着流程将始终以相同的方式执行，具有相同的功能和参数，并且结果将始终符合预定的标准。

因此，正如您可能猜到的，目标是在每个开发阶段应用管道，以试图保证设计的过程不会与理想化的过程不同。

![](img/151282a0a1a8d9c275a829c2fa408eea.png)

用 [Kapwin](https://www.kapwing.com/explore/woody-and-buzz-lightyear-everywhere-meme-template) g 制成

管道在数据科学中有两个特别的用途，要么是在生产中，要么是在建模/勘探过程中，这两个用途都非常重要。此外，它使我们的生活更容易。

第一个是数据 ETL。在制作过程中，分支要大得多，因此，花费在其中的细节层次也大得多，但是，可以总结为:

e(提取)—我如何收集数据？如果我要从一个或几个网站，一个或多个数据库，甚至一个简单的熊猫 csv 来收集它们。我们可以把这个阶段看作是数据读取阶段。

t(转换)—要使数据变得可用，我需要做些什么？这可以被认为是探索性数据分析的结论，这意味着在我们知道如何处理数据(移除特征、将分类变量转换为二进制数据、清理字符串等)之后。)，我们将其全部编译在一个函数中，该函数保证清理总是以相同的方式进行。

l(加载)—这只是将数据保存为所需的格式(csv、数据库等)。)某处，要么云端，要么本地，随时随地使用。

这个过程的创建非常简单，只需拿起探索性数据分析笔记本，将 pandas *read_csv* 放入一个函数中；编写几个函数来准备数据并在一个函数中编译它们；最后创建一个函数保存前一个函数的结果。

有了这些，我们就可以在 python 文件中创建 main 函数，并且用一行代码执行创建的 ETL，而不用冒任何更改的风险。更不用说在一个地方改变/更新所有东西的好处了。

第二个，可能是最有利的管道，有助于解决机器学习中最常见的问题之一:参数化。

我们多少次面临这些问题:选择哪种模式？我应该使用规范化还是标准化？

![](img/656651735156f23c26d1a10f08048d2c.png)

“作者捕获的屏幕截图”

scikit-learn 等库为我们提供了流水线方法，我们可以将几个模型及其各自的参数方差放入其中，添加归一化、标准化甚至自定义流程等预处理，甚至在最后添加交叉验证。之后，将测试所有可能性，并返回结果，或者甚至只返回最佳结果，如下面的代码所示:

```
def build_model(X,y):                          
 pipeline = Pipeline([
        ('vect',CountVectorizer(tokenizer=tokenize)),
        ('tfidf', TfidfTransformer()),
        ('clf', MultiOutputClassifier(estimator=RandomForestClassifier()))                           ])# specify parameters for grid search                           parameters = { 
    # 'vect__ngram_range': ((1, 1), (1, 2)),  
    # 'vect__max_df': (0.5, 0.75, 1.0),                                
    # 'vect__max_features': (None, 5000, 10000),
    # 'tfidf__use_idf': (True, False),
    # 'clf__estimator__n_estimators': [50,100,150,200],
    # 'clf__estimator__max_depth': [20,50,100,200],
    # 'clf__estimator__random_state': [42]                                                   } 

# create grid search object                          
cv = GridSearchCV(pipeline, param_grid=parameters, verbose=1)                                                   return cv
```

在这个阶段，天空是极限！管道内部没有参数限制。然而，根据数据库和所选参数的不同，可能需要很长时间才能完成。即便如此，这也是一个非常好的研究工具。

我们可以添加一个函数来读取来自数据 ETL 的数据，并添加另一个函数来保存创建的模型，这样我们就有了模型 ETL，结束了这个阶段。

尽管我们谈论了所有的事情，创建管道的最大优点是代码的可复制性和可维护性，这是指数级提高的。

那么，开始创建管道还等什么呢？

这方面的一个例子可以在这个[项目](https://github.com/Rpinto02/DisasterResponsePipelines)中找到。