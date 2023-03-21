# 用 PySpark 和 Spark-NLP 进行自然语言处理

> 原文：<https://towardsdatascience.com/natural-language-processing-with-pyspark-and-spark-nlp-b5b29f8faba?source=collection_archive---------7----------------------->

## 深入阅读金融服务消费者投诉文本

![](img/1872c12931655d7ca4cdbff882017792.png)

当我们深入挖掘客户投诉时，寻找宁静。汤姆·盖诺尔在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上的照片

> 问题:哪些词(来自抱怨)明显是对等的？

今天，我们深入美国消费者金融保护局的[金融服务消费者投诉数据库](https://catalog.data.gov/dataset/consumer-complaint-database)，查看针对公司的投诉文本。问题:哪些词(来自抱怨)明显是对等的？我们将使用 Spark-NLP 研究文本清理、标记化和旅鼠，使用 PySpark 进行计数，以及 tf-idf(术语频率-逆文档频率)分析。

我用了约翰·斯诺实验室的 Spark-NLP 库。你可以从[他们](https://www.johnsnowlabs.com/spark-nlp/)，或者[维基百科](https://en.wikipedia.org/wiki/Spark_NLP)(大概也是他们写的，只是风格不同)。

# **安装 Spark-NLP**

约翰·斯诺实验室提供了几个不同的快速入门指南——这里[这里](https://nlp.johnsnowlabs.com/docs/en/quickstart)和这里[这里](https://github.com/JohnSnowLabs/spark-nlp)——我发现一起使用很有用。

如果您尚未安装 PySpark(注意:PySpark 版本 2.4.4 是唯一受支持的版本):

```
$ conda install pyspark==2.4.4
$ conda install -c johnsnowlabs spark-nlp
```

如果你已经有了 PySpark，一定要把 spark-nlp 安装在和 PySpark 相同的通道里(你可以从 conda 列表里查看通道)。在我的例子中，PySpark 安装在我的康达-福吉频道上，所以我使用

```
$ conda install -c johnsnowlabs spark-nlp — channel conda-forge
```

我已经安装了 PySpark，并设置为与 Jupyter 笔记本一起使用，但是如果没有，您可能需要在终端中设置一些额外的环境变量(正如第二个快速入门指南中提到的，但不是第一个，所以…)

```
$ export SPARK_HOME=/path/to/your/spark/folder
$ export PYSPARK_PYTHON=python3
$ export PYSPARK_DRIVER_PYTHON=jupyter
$ export PYSPARK_DRIVER_PYTHON_OPTS=notebook
```

# **Spark-NLP 入门**

如果您希望使用预安装的数据集，因此不需要访问 spark 会话，可以从下面两行开始:

```
import sparknlp
sparknlp.start()
```

在我的例子中，我需要 SparkSession 从 parquet 文件中加载我的数据，所以我将添加。config("spark.jars.packages "，" com . johnsnowlabs . NLP:spark-NLP _ 2.11:2 . 3 . 5 ")到我的 SparkSession.builder

```
from pyspark.sql import SparkSession# start spark session configured for spark nlp
spark = SparkSession.builder \
     .master('local[*]') \
     .appName('Spark NLP') \
     .config('spark.jars.packages', 
             'com.johnsnowlabs.nlp:spark-nlp_2.11:2.3.5') \
     .getOrCreate()
```

就是这样！你站起来了！

# **停止字**

Spark-NLP 没有内置的停用词词典，所以我选择使用 NLTK 英语停用词，以及在我的数据集中找到的“xxxx”修订字符串。

```
from nltk.corpus import stopwordseng_stopwords = stopwords.words('english')
eng_stopwords.append('xxxx')
```

# **设置您的文本操作管道**

首先，导入您的工具:

```
from sparknlp.base import Finisher, DocumentAssembler
from sparknlp.annotator import (Tokenizer, Normalizer,
                                LemmatizerModel, StopWordsCleaner)
from pyspark.ml import Pipeline
```

大多数项目在开始时需要 DocumentAssembler 将文本转换成 Spark-NLP 注释器就绪的形式，在结束时需要 Finisher 将文本转换回人类可读的形式。您可以从注释器[文档](https://nlp.johnsnowlabs.com/docs/en/annotators)中选择您需要的注释器。

在设置管道之前，我们需要用适当的输入初始化注释器。我们将使用典型的 Spark ML 格式`.setInputCols(*list of columns*)`和`.setOutputCol(*output column name*)`，以及其他特定于注释器的函数(参见[文档](https://nlp.johnsnowlabs.com/docs/en/annotators))。每个输出列都将是下面的注释器的输入列。

```
documentAssembler = DocumentAssembler() \
     .setInputCol('consumer_complaint_narrative') \
     .setOutputCol('document')tokenizer = Tokenizer() \
     .setInputCols(['document']) \
     .setOutputCol('token')# note normalizer defaults to changing all words to lowercase.
# Use .setLowercase(False) to maintain input case.
normalizer = Normalizer() \
     .setInputCols(['token']) \
     .setOutputCol('normalized') \
     .setLowercase(True)# note that lemmatizer needs a dictionary. So I used the pre-trained
# model (note that it defaults to english)
lemmatizer = LemmatizerModel.pretrained() \
     .setInputCols(['normalized']) \
     .setOutputCol('lemma')stopwords_cleaner = StopWordsCleaner() \
     .setInputCols(['lemma']) \
     .setOutputCol('clean_lemma') \
     .setCaseSensitive(False) \
     .setStopWords(eng_stopwords)# finisher converts tokens to human-readable output
finisher = Finisher() \
     .setInputCols(['clean_lemma']) \
     .setCleanAnnotations(False)
```

现在我们准备定义管道:

```
pipeline = Pipeline() \
     .setStages([
           documentAssembler,
           tokenizer,
           normalizer,
           lemmatizer,
           stopwords_cleaner,
           finisher
     ])
```

# **使用管道**

我导入并选择我的数据，然后使用 pipeline.fit(数据)。转换(数据)。例如:

```
# import data
df = spark.read.load('../data/consumer_complaints.parquet',
                     inferSchema='true', header='true')# select equifax text data as test
data = df.filter((df['company'] == 'EQUIFAX, INC.')
           & (df['consumer_complaint_narrative'].isNull() == False))
data = data.select('consumer_complaint_narrative')# transform text with the pipeline
equifax = pipeline.fit(data).transform(data)
```

这将返回一个 DataFrame，其中添加了管道注释器中指定的列。因此 equifax.columns 返回:

```
['consumer_complaint_narrative',
 'document',
 'token',
 'normalized',
 'lemma',
 'clean_lemma',
 'finished_clean_lemma']
```

当我们更仔细地观察整理器的输出时，在这个例子中是“finished_clean_lemma”，我们看到每个记录都是一个单词列表—例如`[address, never, …]`、`[pay, satisfied, …]`。

# **对文本进行计数矢量化**

为了让每个单词都在同一水平线上，我使用了`pyspark.sql`分解功能。

```
from pyspark.sql.functions import explode, colequifax_words = equifax_words.withColumn('exploded_text', 
                               explode(col('finished_clean_lemma')))
```

现在文本准备好`.groupby().count()`来获得每个单词的计数。

然后，我将结果转换为 pandas，并使用字典理解将表转换为字典(这可能不是最优雅的策略)。

```
counts = equifax_words.groupby('exploded_text').count()
counts_pd = counts.toPandas()
equifax_dict = {counts_pd.loc[i, 'exploded_text']: 
                counts_pd.loc[i, 'count'] 
                for i in range(counts_pd.shape[0])}
```

完全披露:即使使用在所有四个内核上运行的 Spark，为前 20 名抱怨者做这件事也要花费大量的时间——这是大量的计算！

# **Tf-idf**

现在我已经将每个公司的投诉计数的文本矢量化(也就是转换成{word1: count1，word2: count2…})了，我已经准备好获取每个公司单词集中每个单词的 tf-idf。

助手功能:

```
def term_frequency(BoW_dict):
     tot_words = sum(BoW_dict.values())
     freq_dict = {word: BoW_dict[word]/tot_words 
                  for word in BoW_dict.keys()}
     return freq_dictfrom math import logdef inverse_document_frequency(list_of_dicts):
    tot_docs = len(list_of_dicts)
    words = set([w for w_dict in list_of_dicts 
                   for w in w_dict.keys()])
    idf_dict = {word: log(float(tot_docs)/
                      (1.0 + sum([1 for w_dict in list_of_dicts 
                              if word in w_dict.keys()]))) 
                    for word in words}
    return idf_dictdef tf_idf(list_of_dicts):
     words = set([w for w_dict in list_of_dicts 
                  for w in w_dict.keys()])
     tf_idf_dicts = []
     idfs = inverse_document_frequency(list_of_dicts)
     for i, w_dict in enumerate(list_of_dicts):
          w_dict.update({word: 0 for word in words 
                         if word not in w_dict.keys()})
          tf = term_frequency(w_dict)
          tf_idf_dicts.append({word: tf[word]*idfs[word] 
                               for word in words})
     return tf_idf_dicts
```

综合起来看:

```
list_of_word_dicts = [company_complaint_word_counts_dict[company] 
                      for company in companies]
tf_idf_by_company_list = tf_idf(list_of_word_dicts)
tf_idf_by_company_dict = {c: tf_dict 
           for c, tf_dict in zip(companies, tf_idf_by_company_list)}
```

# **我们每个顶级公司的投诉有什么独特之处**

为了找到让每家公司与众不同的词，我找到了我感兴趣的公司中 tf-idf 得分最高的词。

不幸的是，这表明我没有做足够的工作来清理我的数据。所有 tf-idf 得分最高的单词都是错别字或组合在一起的单词(如“tobe”、“calledthem”等)。).有时候 1000 个字符串不带空格。所以在这里，我添加了一个基于`nltk.corpus words.words()`列表的过滤器。不幸的是，tobe 实际上是一个单词，所以它仍然出现在一些热门搜索结果中。鉴于数据集中缺少空间的问题，我怀疑人们是否在谈论“北非和中非一些地区的传统外衣，由一段布料缝制成宽松的长裙或披在身上并系在一个肩膀上”( [Collins Dictionary](https://www.collinsdictionary.com/us/dictionary/english/tobe) )。

让我们来看看我们十大公司的一些顶级 tf-idf 评分词:

Equifax:经销商，原告，重新插入，可证明，可证明，运行

益百利:经销商，原告，可证明的，可证明的，运行者，再插入

跨国联盟:经销商，原告，可证明的，可证明的，顺从的，重新插入

美银:商户、止赎、柜员、白金、梅隆(？)，火器，mesne

富国银行:保护，止赎，评估，保护主义者，出纳员，交易商

摩根大通:蓝宝石，西南，商人，航空公司，探险家，出纳员

花旗银行:仓库，促销，固特异，威望，商人，股息

资本一:科尔，水银，品味，果园，商人，风险，再比尔

Navient 解决方案:先锋、mae、无补贴、文凭、再认证、毕业

奥克文金融:回家，取消抵押品赎回权，悬念，流氓，复式

我们确实看到了不同类别的金融机构，金融局从提供更多抵押贷款和店面银行业务的银行以及美国教育部贷款服务机构 Navient 得出了不同的结果。

同时，这个结果也相当令人沮丧。这些最重要的话，感觉不是超级有见地。我在想:

1.  也许我们需要看的不仅仅是这些热门词汇，比如前 100 名？
2.  或者，tf-idf 可能不是这项工作的合适工具。由于对这些公司的投诉如此之多，许多词在所有或几乎所有公司的投诉“文集”中至少出现一次。这个因素，逆文档频率，压倒了文本频率。我想知道如果我使用所有的公司，而不仅仅是前 20 名，结果会有什么不同。

一如既往，在 GitHub [repo](https://github.com/allisonhonold/spark-blog-tfidf) 中找到更多(代码)。

编码快乐！

![](img/1aa57793e64de13a6c874afd85b1bdc2.png)

在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上由 [Austin Schmid](https://unsplash.com/@schmidy?utm_source=medium&utm_medium=referral) 拍摄的照片