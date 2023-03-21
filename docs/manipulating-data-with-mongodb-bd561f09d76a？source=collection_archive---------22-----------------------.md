# 使用 MongoDB 操作数据

> 原文：<https://towardsdatascience.com/manipulating-data-with-mongodb-bd561f09d76a?source=collection_archive---------22----------------------->

## 使用 PyMongo 学习 CRUD 的基础知识

![](img/ca53d1c2ee456e22fe99c3eefd4895e5.png)

马库斯·斯皮斯克在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄的照片

访问第 1 部分[这里](/lets-get-you-started-with-mongodb-part-1-85c64828c815)来设置你的 MongoDB。

# 介绍

在上一篇文章中，我们设法了解了为什么 MongoDB 如此强大，以及如何设置它和导入我们的数据。

设置好一切之后，我们现在可以用 PyMongo 学习 MongoDB 的基础知识了。我将带您了解 CRUD 操作，它代表:

*   c——创建:创建数据并将其插入到数据库中
*   r-Read:从数据库中读取数据
*   U —更新:选择并更新数据库中的数据
*   d-删除:删除数据库中的数据

# 用 Pymongo 连接到 Python

下面的代码显示了我们如何连接到我们的客户端:

```
import pymongo
from pymongo import MongoClient
import pprintclient = MongoClient("<CONNECTION STRING>")
```

用自己的字符串替换`<CONNECTION STIRNG>`。如果您不确定它是什么，请参考本系列的第 1 部分！它应该是这样的:

```
mongodb+srv://analytics:[analytics-pw@mflix-b23yi.mongodb.net](mailto:analytics-pw@mflix-b23yi.mongodb.net)
```

# 创造

只需使用`insert_one()`功能即可插入数据:

```
from datetime import datetimenew_data = {
    "_id": "unique_id_here",
    "name": "random_name",
    "email": "[random_email@email.com](mailto:random_email@email.com)",
    "text": "This is a random text!",
    "date": datetime.utcnow()
}insert_result = client.sample_mflix.movies.insert_one(new_data)
pprint.pprint(insert_result.acknowledged)
pprint.pprint(insert_result.inserted_id)
```

![](img/daca94ff2cdf7a2543680ef46212e51a.png)

输出

在上面的代码中，我们将新数据定义为一个字典。您可以包含任意数量的字段(记住，MongoDB 是灵活的，您不需要有一组固定的键)。请注意我们是如何用`client.sample_mflix.movies`来引用我们的数据库的。它遵循`client.database.collections`的层次结构。

最重要的领域是`_id`。MongoDB 中的每个文档都由一个惟一的标识符标识。如果您要插入两个具有相同标识符的数据，您将会遇到一个错误。

让我们用 Compass 验证新数据是否已插入:

![](img/38a43f8b4e53da8e99605a80745782e4.png)

当你想在 Compass 中搜索某个东西时，你必须用下面的格式`{'<KEY>':'<VALUE'}`来指定它。

`insert_one()`返回我们两个输出:

1.  已确认:如果插入成功，则为真，否则为假
2.  inserted_id:文档的唯一 id。您可以使用这个 ID 在 Compass 中搜索新数据

*但是如果我想插入很多数据呢？*

```
from datetime import datetimenew_data_1 = {
    "_id": "unique_id_here_1",
    "name": "random_name",
    "email": "[random_email@email.com](mailto:random_email@email.com)",
    "text": "This is a random text!",
    "date": datetime.utcnow()
}new_data_2 = {
    "_id": "unique_id_here_2",
    "name": "random_name",
    "email": "[random_email@email.com](mailto:random_email@email.com)",
    "text": "This is a random text!",
    "date": datetime.utcnow()
}final_data = [new_data_1,new_data_2]insert_result = client.sample_mflix.movies.insert_many(final_data)
```

你只需要把`insert_one`改成`insert_many`。就这么简单！

# 阅读

有很多方法可以读取你的数据。您可以使用 MongoDB 提供的许多其他功能进行过滤、分组、排序、限制、计数、求和。我将介绍一些基础知识。

## find_one()

`find_one()`允许我们检索数据库中的第一批数据

```
client.sample_mflix.movies.find_one()
```

## 投影取景器

`finder`允许我们指定想要过滤的关键字和值。`projection`允许我们指定我们想要返回的内容。

```
finder = {
    'countries': {'$all': ['USA', 'Italy']}
}
projection={
    'title':1,
    'countries':1,
    'rated': 1
}
pprint.pprint(list(client.sample_mflix.movies.find(finder,projection)))
```

*   `'$all'`表示`'countries'`字段必须包含所有指定的内容。在我们的例子中，我们想要检索在美国和意大利制作的电影。请注意，这种语法非常类似于您在 Compass 的搜索栏中键入的内容。
*   在`projection`下，我们指定想要返回的键。如果要返回，请将值设置为 1；如果不想返回该键，请将值设置为 0。如果您忽略该键，它将被视为您不想返回该键。

![](img/5c2edd21f3411fcff97ac998729ac906.png)

结果子集

## 总计

PyMongo 中的大部分数据读写都可以使用一个[聚合](https://docs.mongodb.com/manual/reference/operator/aggregation-pipeline/)框架来完成。聚合是一种集合类方法。这意味着它只能应用于集合/表。聚合框架需要管道或“模板”:

```
pipeline = [
    {
        '$group': {
            '_id': {"rated": "$rated"},
            'count': {"$sum": 1}
        }
    },
    {
        '$sort': {'count': -1}
    }
]pprint.pprint(list(client.sample_mflix.movies.aggregate(pipeline)))
```

*   `'$group` stage 允许我们通过指定的键对输出进行分组。
*   `'_id':{"rated":"$rated"}`是我们指定想要分组的关键字的地方。在这种情况下，我想按惟一的`rated`分组。美元符号($)是字段路径标识符。它允许我们从数据库中检索精确的值。
*   `'count': {"$sum": 1}`将自动为每个唯一的`rated`增加 1。
*   `'$sort':{'count': -1}`告诉我们的管道按照计数的降序(-1)对结果进行排序。

![](img/6ffb50dffc6ab55cbedb566276e83ba9.png)

结果

## 方面

Facet 允许我们很好地对数据进行分组。它类似于`$group` ，但它允许我们定制我们想要的数据分组方式。

```
pipeline = [
    {
        '$sortByCount': '$countries'
    },
    {
        '$facet':{
            'top country combinations': [{'$limit': 10}],
            'unusual combinations shared by' : [{
                '$skip' : 10
        },
        {
                '$bucketAuto': {
                    'groupBy': "$count",
                    'buckets': 5,
                    'output': {
                        'country combinations': {"$sum":1}
                    }
                }
            }
            ]
        }
    }
]pprint.pprint(list(client.sample_mflix.movies.aggregate(pipeline)))
```

*   `‘top country combinations’: [{‘$limit’: 10}]`告诉我们的渠道产生前 10 个国家及其各自的数量
*   `‘$skip’ : 10`允许我们跳过前 10 个结果，因为它已经被归类到`'top country combinations'`下
*   `‘$bucketAuto’: {`是一种根据特定指标将剩余结果分组到指定最大数量的桶/箱中的方法。

![](img/243f84d3bdb766c2a2bc728ff74da742.png)

结果

在结果中，我们首先受到电影数量排名前 10 位的国家的欢迎。继续，我们有 3 个桶。对于 MongoDB，`min`是包含性的，`max`是排他性的。这意味着有些电影的国家组合与最少 2 部其他电影和最多 6 部其他电影相似。这类电影的数量是 469 部。另一方面，我们还有`min: 1`和`max: 1`。这意味着有 1695 部电影拥有独特的国家组合。

## 清理、过滤和修改数据到另一个集合中

对于这一节，我将参考原来的`movies_initial.csv`，因为它是不干净的。在将数据导出到另一个名为`movies_subset`的集合之前，我们将清理一部分数据。

```
pipeline = [
    {
        '$limit': 100
    },
    {
        '$project': {
            'title': 1,
            'directors': {'$split': ["$director", ", "]},
            'released': {
                '$cond': {
                    'if': {'$ne': ["$released", ""]},
                    'then': {
                        '$dateFromString': {
                            'dateString': "$released"
                        }
                    },
                    'else': ""}},
            'imdb': {
                'id': "$imdbID",
                'rating': "$imdbRating",
                'votes': "$imdbVotes"
                }
        }
    },
    {
        '$out': "movies_subset"
    }
]pprint.pprint(list(client.mflix.movies_initial.aggregate(pipeline)))
```

*   我们首先将要检索的数据数量限制为 100
*   我们想要标题字段
*   目录字段被拆分成一个数组。例如，`"Mike, Tom, Harry"`将被分割成`["Mike", "Tom", "Harry"]`
*   在 released 字段中，如果字段不为空，我们实际上是将日期和时间字符串转换为 DateTime 对象。如果字段为空，我们将返回一个空字符串。
*   `‘if’: {‘$ne’: [“$released”, “”]}`指定如果我们的 released 字段不等于空字符串，我们将继续使用子句中的内容。
*   最后`$out`允许我们将最终数据导出到一个新的集合中

![](img/09cecc8d075cc738923413a42cd171b2.png)

我们现在有了新的系列！

## 过滤器

有三种方法可以过滤你的结果。

1.  $匹配

```
pipeline = [
    {
        '$match': {'countries': ['USA', 'Italy']}
    }
]
pprint.pprint(list(client.sample_mflix.movies.aggregate(pipeline)))
```

2.探测器

```
finder = {'countries': ['USA', 'Italy']}
pprint.pprint(list(client.sample_mflix.movies.find(finder)))
```

3.指南针

```
{'countries': ['USA', 'Italy'], 'rated' : "UNRATED"}
```

以上 3 个都一样。嗯……不完全一样，我在第三点里面偷偷加了一个滤镜。但是您可以看到如何在一行中输入多个过滤器！

*注意，* `*$all*` *和* `*$match*` *非常不同。* `*$match*` *要求完全匹配。* `*$all*` *只要找到指定的值就会返回结果。*

## 经营者

您可以在过滤器或搜索中包含某些比较运算符。

```
# find all movies except the ones which are of adult genre
filters = {
    'year': {'$gte': 1989, '$lt': 2000},
    'genre': { '$not' : {'$eq': 'Adult'} }
}pprint.pprint(list(client.sample_mflix.movies.find(filters)))
```

*   `$gte`代表“大于或等于”
*   `$lt`代表“小于”
*   `{ ‘$not’ : {‘$eq’: ‘Adult’} }`表示我们需要不等于“成人”的值

有 20 多个操作员。我强烈建议您查看[文档](https://docs.mongodb.com/manual/reference/operator/query/)！

## 排序和限制

排序和限制非常简单。我们经常把它们结合起来，使我们的输出可读。您不仅可以将它们输入到聚合框架中，还可以将其用作 Python 方法。

```
from pymongo import DESCENDING
sort_key = "imdb.rating"movies = client.sample_mflix.movies.find({}) \
                  .sort(sort_key, DESCENDING).limit(100)list(movies)
```

首先，我们指定我们想要排序值的关键字。注意，我们键入`imdb.rating`是因为`rating`(一个标量)嵌套在`imdb`(一个数组)中。

注意，在我们的`.find()`方法中，我们输入了一个空字典。这告诉我们的查找器搜索整个集合。

最后，我们按降序对结果进行排序，只返回前 100 个结果。

# 更新

之前，我们学习了如何将数据修改到新的集合中。现在，我们将看看如何就地修改我们的数据。

## 逐文档更新一个文档

我们在代码中所做的是，首先，遍历标题为“泰坦尼克号”的所有电影。接下来，我添加了两个字段，`comments`和`num_mflix_comments`，并实例化了它们。最后，我删除了`poster`字段，因为我不再需要它们了。

```
titanic_filter = {'title': "Titanic"}for movie in client.sample_mflix.movies.find(titanic_filter):
    update_doc = {
        "$set": {
            "comments": [],
            "num_mflix_comments": 0
        },
        "$unset": {
            "poster": ""
        }
    }
    client.sample_mflix.movies.update_one({'_id': movie['_id']}, update_doc)
```

*   `$set`用指定的值替换字段的值
*   `$unset`仅删除存在的特定字段

我们可以使用 Compass 验证代码是否有效:

![](img/79826479efb43d0af1d9f93eca7dfff1.png)

`update_one`一次更新一个文档。如果是一个小数据集，这不是问题。然而，如果我们有一个大的数据集，我们会想要批量更新我们的数据

## 批量更新文档

批量更新集合与逐个更新非常相似。不同之处在于，每次迭代，我们将更新保存在一个数组中，而不是更新。同时，我们跟踪我们所处的迭代次数。当我们达到批量时，我们将使用`bulk_write`将所有更新写入我们的数据库

```
from pymongo import UpdateOnetitanic_filter = {'title': "Titanic"}
batch_size = 10
updates = []
count = 0for movie in client.sample_mflix.movies.find(titanic_filter):
    update_doc = {
        "$set": {
            "comments": [],
            "num_mflix_comments": 0
        },
        "$unset": {
            "poster": ""
        }
    }

    updates.append(UpdateOne({'_id': movie['_id']}, update_doc))count += 1
    if count == batch_size:
        client.sampel_mflix.movies.bulk_write(updates)
        updates = []
        count = 0if updates:         
    client.sampel_mflix.movies.bulk_write(updates)
```

一定要记住用最后两行代码来捕捉最后一批代码！

# 删除

我们只需要两个方法`delete_one()`和`delete_many()`。

我们来看看收视率低于 3.5 的电影有多少

```
filter = {"imdb.rating": {'$lte':3.5}}before = list(client.sample_mflix.movies.find(filter))
len(before)
```

![](img/0ba7c4ea9554075d471746df04d6afa8.png)

我们注意到有 178 部电影。我们删除其中一个:

```
client.sample_mflix.movies.delete_one(filter)
```

![](img/6df725f9113299add35b54a2eda59228.png)

我们现在只剩下 177 个，因为我们删除了其中一个。

现在，让我们删除所有评分低于 3.5 的电影

```
client.sample_mflix.movies.delete_many(filter)
```

![](img/c2ef035d41088c6ffa42646c6858cae5.png)

成功！

# 结论

恭喜你！您现在可以在 MongoDB 中操作数据了。我强烈建议您查看文档。我只能解释这么多功能。然而，文档包含了完成项目所需的每一种方法。我非常相信学会如何阅读文档是成为一名软件工程师/数据科学家的重要部分。

# 参考

1.  【https://www.coursera.org/learn/introduction-mongodb 
2.  [https://docs.mongodb.com/](https://docs.mongodb.com/)
3.  代号:[https://github.com/bensjx/MongoDB_Demo/](https://github.com/bensjx/MongoDB_Demo/upload)