# 构建和部署端到端假新闻分类器

> 原文：<https://towardsdatascience.com/building-and-deploying-end-to-end-fake-news-classifier-caebe45bd30?source=collection_archive---------59----------------------->

## [变更数据](https://towardsdatascience.com/tagged/data-for-change)

![](img/c287c4dd7578e4bae96962130b74714e.png)

罗马克拉夫特在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 拍摄的照片

在这个智能手机和互联网的数字时代，假新闻像野火一样传播，它看起来就像真新闻，对社会造成了很大的损害。因此，在本教程中，我们将构建一个假新闻分类器，并将其作为一个 web 应用程序部署在云上，以便任何人都可以访问它。它不会像谷歌或 facebook 的假新闻分类器那样好，但根据从 Kaggle 获得的数据集，它会相当不错。

> 在我们开始之前，为了让你有动力，让我向你展示一下在本教程结束时你将能够构建的 web 应用程序 [**假新闻分类器**](http://real-fake-news-classifier.herokuapp.com/) **。**现在你已经看到了最终产品，让我们开始吧。

***注*** *:我假设你熟悉基本的机器学习技术、算法和软件包。*

我将本教程分为三个部分:

1.  探索性数据分析
2.  预处理和模型训练
3.  在 Heroku 上构建和部署 Web 应用程序

现在，如果您是初学者，我建议您安装 Anaconda 发行版，因为它附带了数据科学所需的所有软件包，并设置了一个虚拟环境。

如果你想跟随这个教程，这里是我的 GitHub 上的源代码链接:[https://github.com/eaofficial/fake-news-classifier](https://github.com/eaofficial/fake-news-classifier)。

你可以在这里获得数据集[](https://www.kaggle.com/clmentbisaillon/fake-and-real-news-dataset)*或者你可以克隆我的 GitHub 库。*

# *1.探索性数据分析*

*![](img/342d839189efd4cbd1d54a1dd299ccbd.png)*

*照片由[元素 5 数码](https://unsplash.com/@element5digital?utm_source=medium&utm_medium=referral)在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄*

*在项目目录中创建一个名为 eda.ipynb 或 eda.py 的文件。*

*我们将首先导入所有需要的包。*

```
**#Importing all the libraries*
**import** **warnings**
warnings.filterwarnings('ignore')
**import** **numpy** **as** **np**
**import** **pandas** **as** **pd**
**import** **matplotlib.pyplot** **as** **plt**
**import** **seaborn** **as** **sns**
**import** **nltk**
**import** **re**
**from** **wordcloud** **import** WordCloud
**import** **os***
```

*现在，我们将首先使用`pd.read_csv()`读取假新闻数据集，然后我们将探索该数据集。*

*在上述笔记本的单元格 4 中，我们统计了每个主题中的样本假新闻的数量。我们还将使用 seaborn count plot `sns.coountplot()`绘制其分布图。*

*我们现在将绘制一个词云，首先将所有新闻连接成一个字符串，然后生成标记并删除停用词。文字云是一种非常好的可视化文本数据的方式。*

*正如您在下一个单元格中看到的，现在我们将 true.csv 作为真实新闻数据集导入，并执行与我们在 fake.csv 上执行的步骤相同的步骤。您会注意到真实新闻数据集中的一个不同之处是，在*列中，有一个出版物名称，如 *WASHINGTON (Reuters)* ，由连字符(-)分隔。**

**看起来真实的新闻是可信的，因为它来自一家出版社，所以我们将从新闻部分中分离出出版物，以使本教程的预处理部分中的数据集一致。现在，我们将只探索数据集。**

**如果您继续下去，可以看到新闻主题列在真实和虚假新闻数据集中的分布是不均匀的，所以我们稍后将删除该列。我们的 ***EDA 到此结束。*****

> **现在我们可以用你们期待已久的东西弄脏我们的手了。我知道这部分令人沮丧，但 EDA 和预处理是任何数据科学生命周期中最重要的部分**

# **2.预处理和模型训练**

**![](img/a0e1d5cc99ca682134f251be2e8a1490.png)**

**[卡洛斯·穆扎](https://unsplash.com/@kmuza?utm_source=medium&utm_medium=referral)在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上的照片**

**在这一部分中，我们将对我们的数据执行一些预处理步骤，并使用之前从 EDA 中获得的见解来训练我们的模型。**

## **预处理**

**要按照本部分中的代码操作，请打开 train ipynb 文件。所以，不再多说，让我们开始吧。**

**像往常一样导入所有的包并读取数据。我们将首先从真实数据*文本*栏中删除路透社。由于有些行没有路透社，所以我们将首先获得这些指数。**

**从文本中删除路透社或推特信息**

*   **文本只能在“—”处拆分一次，它总是出现在提及出版物来源之后，这给了我们出版物部分和文本部分**
*   **如果我们没有得到文本部分，这意味着没有给出该记录的出版细节**
*   **Twitter 上的推文总是有相同的来源，一个最长 259 个字符的长文本**

```
***#First Creating list of index that do not have publication part*
unknown_publishers = []
**for** index,row **in** enumerate(real.text.values):
    **try**:
        record = row.split(" -", maxsplit=1)
        *#if no text part is present, following will give error*
        record[1]
        *#if len of publication part is greater than 260*
        *#following will give error, ensuring no text having "-" in between is counted*
        **assert**(len(record[0]) < 260)
    **except**:
        unknown_publishers.append(index)**
```

**用一行代码总结一下，上面的代码所做的是获取真实数据集中不存在发布者的 ***text*** 列的索引。**

**现在我们将把路透社从文本栏中分离出来。**

```
***# separating publishers from the news text*
publisher = []
tmp_text = []
for index,row **in** enumerate(real.text.values):
    if index **in** unknown_publishers:
        *#add text to tmp_text and "unknown" to publisher*
        tmp_text.append(row)

        publisher.append("Unknown")
        continue
    record = row.split(" -", maxsplit=1)
    publisher.append(record[0])
    tmp_text.append(record[1])**
```

**在上面的代码中，我们遍历 text 列并检查 index 是否属于，如果是，那么我们将文本添加到 publishers 列表中。否则，我们将文本分为出版商和新闻文本，并添加到各自的列表中。**

```
***#Replace existing text column with new text*
*#add seperate column for publication info*
real["publisher"] = publisher
real["text"] = tmp_text**
```

**上面的代码非常简单明了，我们添加了一个新的 publisher 列，并用不带 Reuter 的新闻文本替换了 text 列。**

**我们现在将检查真实和虚假新闻数据集中的文本列中是否有任何缺失值，并删除该行。**

**如果我们检查假新闻数据集，我们会看到有许多行缺少文本值，整个新闻都出现在`title`列中，因此我们将合并`title`和`text`列。**

```
**real['text'] = real['text'] + " " + real['title']
fake['text'] = fake['text'] + " " + fake['title']**
```

**接下来，我们将向数据集添加类，删除不必要的列，并将数据合并为一个。**

```
***# Adding class info* 
real['class'] = 1 
fake['class'] = 0*# Subject is diffrent for real and fake thus dropping it* *# Also dropping Date, title and Publication* real.drop(["subject", "date","title",  "publisher"], axis=1, inplace=**True**) fake.drop(["subject", "date", "title"], axis=1, inplace=**True**)*#Combining both into new dataframe* data = real.append(fake, ignore_index=**True**)**
```

**删除停用字词、标点符号和单字符字词。(在任何 NLP 项目中非常常见和基本的任务)。**

## **模特培训**

## **矢量化:Word2Vec**

**![](img/dbf4175958b7ae5ee0eac2757b509a7f.png)**

**尼克·莫瑞森在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 拍摄的照片**

**Word2Vec 是使用浅层神经网络学习单词嵌入的最流行的技术之一。它是由托马斯·米科洛夫于 2013 年在谷歌开发的。单词嵌入是最流行的文档词汇表示。它能够捕捉文档中单词的上下文、语义和句法相似性、与其他单词的关系等。**

**如果你想了解更多，点击 [***这里***](/introduction-to-word-embedding-and-word2vec-652d0c2060fa)**

**让我们创建我们的 Word2Vec 模型。**

```
**#install gensim if you haven't already
#!pip install gensim
import gensim*#Dimension of vectors we are generating*
EMBEDDING_DIM = 100
*#Creating Word Vectors by Word2Vec Method*
w2v_model = gensim.models.Word2Vec(sentences=X, size=EMBEDDING_DIM, window=5, min_count=1)*#vocab size*
len(w2v_model.wv.vocab)
*#We have now represented each of 122248 words by a 100dim vector.***
```

**这些向量将被传递给 LSTM/GRU，而不是单词。1D-CNN 可以进一步用于从向量中提取特征。**

**Keras 有一个名为“**嵌入层**的实现，它将创建单词嵌入(向量)。因为我们是用 gensim 的 word2vec 做的，所以我们会将这些向量加载到嵌入层中，并使该层不可训练。**

**我们不能将字符串传递给嵌入层，因此需要用数字来表示每个单词。**

**记号赋予器可以用数字来表示每个单词**

```
***# Tokenizing Text -> Repsesenting each word by a number*
*# Mapping of orginal word to number is preserved in word_index property of tokenizer**#Tokenized applies basic processing like changing it yo lower case, explicitely setting that as False*
tokenizer = Tokenizer()
tokenizer.fit_on_texts(X)X = tokenizer.texts_to_sequences(X)**
```

**我们创建了单词索引和向量之间的映射矩阵。我们用它作为嵌入层的权重。嵌入层接受单词的数字符号，并向内层输出相应的向量。它向下一层发送一个零向量，用于将被标记为 0 的未知单词。嵌入层的输入长度是每个新闻的长度(由于填充和截断，现在是 700)。**

**现在，我们将创建一个序列神经网络模型，并在嵌入层中添加从 w2v 生成的权重，还添加一个 LSTM 层。**

```
***#Defining Neural Network*
model = Sequential()
*#Non-trainable embeddidng layer*
model.add(Embedding(vocab_size, output_dim=EMBEDDING_DIM, weights=[embedding_vectors], input_length=maxlen, trainable=False))
*#LSTM* 
model.add(LSTM(units=128))
model.add(Dense(1, activation='sigmoid'))
model.compile(optimizer='adam', loss='binary_crossentropy', metrics=['acc'])**
```

**现在让我们使用`sklearn train_test_split`方法将数据集分成训练集和测试集。**

**让我们使用`model.fit(X_train, y_train, validation_split=0.3, epochs=6)`来训练模型。这需要一些时间，在我的机器上花了大约 40 分钟，所以坐下来喝点咖啡，放松一下。**

**训练完成后，我们将在`test`数据集上进行测试，并使用`classification_report()`方法生成报告。**

**哇，我们获得了 99%的准确性，具有良好的精确度和召回率，因此我们的模型看起来很好，现在让我们将它保存在磁盘上，以便我们可以在我们的 web 应用程序中使用它。**

# **3.构建和部署 web 应用程序**

**![](img/2419e1bf9cc5e5e8c3b6f5338f82f44c.png)**

**照片由[沙哈达特·拉赫曼](https://unsplash.com/@hishahadat?utm_source=medium&utm_medium=referral)在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄**

**在这一部分，我不会讲太多的细节，我建议你仔细阅读我的代码，它非常容易理解。如果你一直坚持到现在，你必须有相同的目录结构，如果没有，那么只需改变`app.py`文件中的路径变量。**

**现在将整个目录上传到 GitHub 存储库中。**

**我们将在 Heroku 上托管我们的 web 应用程序。如果你还没有，在 Heroku 上创建一个免费帐户，然后:**

1.  **点击创建新应用程序**
2.  **然后选择一个名称**
3.  **选择 GitHub，然后选择您想要保留的存储库**
4.  **点击部署。**

**嘣，它完成了，你的假新闻分类器现在是活的。**

# **结论…**

**如果您已经完成了，那么恭喜您，现在您可以构建和部署一个复杂的机器学习应用程序了。**

**我知道这很难理解，但你能走到这一步还是值得称赞的。**

> ****注意:**该应用程序适用于大多数新闻，只需记住粘贴整段新闻，最好是美国新闻，因为数据集被限制为美国新闻。**

**如果我们还没有见过面，我是 ***Eish Kumar*** 你可以在 Linkedin 上关注我:[https://www.linkedin.com/in/eish-kumar/](https://www.linkedin.com/in/eish-kumar/)。**

**关注我更多这样的文章。**