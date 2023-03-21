# 理解自然语言处理中的单词包

> 原文：<https://towardsdatascience.com/understanding-the-bags-of-word-in-nlp-ed6d1dd07fdb?source=collection_archive---------27----------------------->

![](img/2a32a553cf317ea1afe6c094ba2ec3d7.png)

[图像来源](https://www.pxfuel.com/en/free-photo-omlgb)

自然语言处理是人工智能的一个重要分支，许多有趣而重要的研究正在这里进行。作为一个机器学习爱好者，了解 NLP 的子流程很重要。

在这里，我想分享一个在 NLP 中使用的术语，并附有代码示例(链接到底部的笔记本)。“包话！！!"。

## NLP 中的单词包是什么？

单词包是一种用来找出文本(段落)中重要主题的方法。有哪些话题？假设你正在阅读下面的段落，

```
As a pet, cat is a very useful animal and helps in protecting or saving our rashan from rats. *The offspring of a cat is called as kitten*, it is a smaller and a cuter version of a cat. Cat has got four thin, short and sturdy limbs that helps it in walking, running and jumping for long distances.It’s bright eyes help it in seeing long distances and also help during the dark. Cats are found all over the world. There is no place without a cat. Sometimes a cat can be mistaken for a tiger cub, because of its extreme similarities with it.A cat’s body is completely covered with soft and beautiful fur. Cats make meaw meaw sound. God has provided cats with soft shoes or pads, which help a cat in walking without making a sound.
```

【文字鸣谢:【https://www.atozessays.com/animals/essay-on-cat/ 

作为一个人，当你读到这里的时候，你知道这一段是关于猫的。Cat 是上一段的一个重要话题。但是，

*   一台机器应该如何解决这个问题？
*   你怎么知道你的模型猫是这一段的一个重要话题？

```
**"The more frequent a word, the more important it might be!!!"**
```

这就是单词袋发挥作用的地方！！！

## 如何获得一个段落的单词袋？

1.  首先，使用标记化创建段落的标记，标记可以是文本的任何部分，例如单词、数字、标点符号或特殊字符
2.  应用必要的文本预处理步骤来过滤掉好的标记，例如小写单词、词汇化/词干化(将单词变成它们的根形式)、去除停用单词和标点符号等。
3.  统计每个单词出现的次数，找出最常见的单词。

```
# declare your text here
paragraph = "As a pet, cat is a very useful animal and helps in protecting or saving our rashan from rats........................."# tokenize the paragraph using word_tokenize,return tokens
tokens = word_tokenize(paragraph)# change the tokens to lower case
lower_tokens = [token.lower() for token in tokens]# Retain alphabetic words: alpha_only, eliminate punctions and special characters
alpha_only = [t for t in lower_tokens if t.isalpha()]#remove all stop words
stop_words = set(stopwords.words('english'))
filtered_tokens = [token for token in alpha_only if not token in stop_words]# lemmatize the words to bring them to their root form
wordnet_lemmatizer = WordNetLemmatizer()
lemmatized_tokens = [wordnet_lemmatizer.lemmatize(token) for token in filtered_tokens]# create bag of words
bag_of_words = Counter(lemmatized_tokens)# print the top 5 most common words
print(bag_of_words.most_common(5))Output:
[('cat', 11), ('help', 5), ('walking', 2), ('long', 2), ('without', 2)]
```

通过查看输出，您可以简单地说，这是一篇关于猫的文章，因为它是文章中出现频率最高的单词。现在，您可以使用这袋单词作为特征来进行 NLP 的下一步。

这只是一种方法，为了简单起见，我只添加了简单的预处理步骤。但是预处理步骤将因情况而异。阅读类似的作品，针对你的情况微调预处理步骤。

你可以看看这个例子的笔记本。希望这篇文章对你有帮助！！！

[https://github . com/mathan Raj-Sharma/sample-for-medium-article/blob/master/bag-of-words-nltk . ipynb](https://github.com/Mathanraj-Sharma/sample-for-medium-article/blob/master/bag-of-words-nltk.ipynb)