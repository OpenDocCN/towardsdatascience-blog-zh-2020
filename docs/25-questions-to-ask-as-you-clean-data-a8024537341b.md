# 清理数据时要问的 25 个问题

> 原文：<https://towardsdatascience.com/25-questions-to-ask-as-you-clean-data-a8024537341b?source=collection_archive---------20----------------------->

## 我在数据清理之前问这些问题，所以我知道开始时会遇到什么。

![](img/17fcb0614cf6521f3df0e3e13c70bbf9.png)

图片来自[像素](https://www.pexels.com/)上的 [bongkarn thanyakij](https://www.pexels.com/@bongkarn-thanyakij-683719)

我是一名软件工程师和数据科学家，在笔记本和软件包中编写代码。如果你还没有听说过，你应该在编码之前停下来想一想，同样的概念也适用于数据清理。从最初的工作中退一步，开始思考手头的问题和您将清理的数据，这是很有价值的。考虑数据的最终用例也是一个好主意。您需要在报告或仪表板中使用它吗？数据会被很多人使用还是被一个人使用？您需要多久清理一次这些数据？至此，我将向您介绍我在处理数据清理之前考虑的 25 个常见问题。

# 数据摄取

当我开始一个项目时，我首先要考虑的是在我开始清理它之前，我将如何接收这个项目的数据。根据我从哪里获取数据，我可能需要执行不同的数据清理步骤。假设数据已经来自另一个团队。在这种情况下，在我接收数据之前，数据可能是干净的，这使我在处理数据之前更容易预处理数据。

```
1\. Do you need to ingest the data and then clean it, or is it cleaned at the source? 
2\. If you are reading in files that contain your data, can you clean and overwrite the file, or will you need to save it somewhere else to keep the raw file separate? 
3\. Do you need to save your cleaned data or keep it in a dataframe and save your analysis's output?
4\. Do you need a backup of the data somewhere? 
5\. What happens if the files become corrupted as you are cleaning it? Are you prepared to start over?
```

# 空白或 Null 字段

收到数据后，下一步是理解如何处理空值。数据集中很少会出现空值。相反，用户会添加一些晦涩难懂的大值，如-9999 或 9999、其他字符或单词。最好后退一步，理解用户是如何向数据集中添加空值的，并采取措施看看您将如何清除这些值。一旦知道了这些值在数据集中是如何表示的，就可以开始清除它们或在需要的地方输入值。

```
6\. Are there values that you can remove as empty or NULL values such as -1, -9999, 9999, or other characters? 
7\. Will these values be imputed as a string, numeric, or category? 
8\. Will you drop values that are empty or NULL? 
9\. Can these values be empty or NULL, and still have the data make sense to provide valuable action? If this value is missing, can you provide actionable insights? 
10\. Can work with those who created the data to develop a standard for what is considered empty or NULL?
```

# 文本字段

我要找的下一个东西是文本字段。当我使用文本字段时，我或者将它们用作类别(下面将讨论),或者用作将被显示或用于附加信息的纯文本。但是，如果您使用文本字段进行建模，会发生什么呢？您可能需要考虑不同类型的清理或自然语言处理技术来处理您的数据，例如词干化、词汇化或删除填充词。我发现处理文本字段更加困难，因为可能会有拼写、缩写、错误类型信息等方面的变化。

```
11\. Are there spelling mistakes in the column that you need to consider? 
12\. Can a word or abbreviation be spelled multiple ways? 
13\. Could there be more than one abbreviation for the same thing? 
14\. Do you need to extract data from the text field? If so, will you use regular expressions, stemming, lemmatization, remove filler words, white space, etc.? 
15\. Do you have timestamp or other numeric data type columns that read as strings but should be another data type?
```

# 类别和布尔值

当清理数据时，类别和布尔是最容易处理的两种数据类型，因为它们的列中变化较少。对于类别，我倾向于查看为给定列列出的唯一类别，以便更好地理解我正在处理的不同值。然后，我可以根据需要将这些信息返回给主题专家(SME ),以获得可能不清楚的类别的定义，例如映射到定义的数字或单个字母值。

```
16\. Will you keep your categories in a human-readable format or convert them using one-hot encoding? 
17\. Do you have too many or too little categories? 
18\. Do you have duplicate categories? Duplication can appear due to misspellings or added white space such as 'R' and 'R '. 
19\. Should you add another category if there are items that do not fit into the rest? 
20\. How will you represent boolean values? 0 and 1, True and False. Pick a schema and use it for all boolean columns to have consistency.
```

# 数字字段

最后，还有数字字段。我在查看数值字段时完成的一项常见任务是了解这些列中的汇总统计信息和数据分布。这种快速分析有助于了解数据的结构，以及是否存在任何明显的异常值。查看这些数据后，您可能还需要考虑与您的数值点相关的任何单位。您是否假设了特定列的单位，但似乎有些不对劲？在你继续你的工作之前，仔细检查这一点会有所帮助。

```
21\. Does the data columns' distribution seem appropriate, or do you need to investigate an issue?
22\. Do you need to know what metrics the data stands for, such as feet vs. inches or Celcius vs. Fahrenheit? Will the difference matter to your end calculations?
23\. Is the column all numeric, or are there other values to clean out from the data?
24\. Have you converted the column to the same data type, such as int or float? Will this conversion affect your end-use case for the data? 
25\. Is your data continuous or categorical? How will that data be used?
```

# 摘要

在数据科学项目中，数据清理和预处理可能会占用大量时间。在接收数据时，考虑在接收数据之前可能已经完成了多少清理工作，以及需要做多少清理工作。当您查看数据时，首先要考虑的是如何处理 null 或空数据值。你能清除它们吗，或者你需要在它的位置上估算一个值吗？一旦您做出了决定，您就可以开始查看每一列中的实际数据点，并制定一个计划来清理那些您认为合适的列。列可以包括类别、布尔值、数字和文本字段等内容。在建模前清理和预处理数据时，每种方法都需要考虑不同的因素。

***在清理数据时，您会问哪些类型的问题？***

如果你想阅读更多，看看我下面的其他文章吧！

[](/top-3-books-for-every-data-science-engineer-e1180ab041f1) [## 每位数据科学工程师的前三本书

### 我放在书架上的伟大资源，我喜欢介绍给软件工程师和数据科学家。

towardsdatascience.com](/top-3-books-for-every-data-science-engineer-e1180ab041f1) [](/do-we-need-object-orientated-programming-in-data-science-b4a7c431644f) [## 在数据科学中我们需要面向对象编程吗？

### 让我们讨论一下作为一名数据科学家转向面向对象编程的利弊。

towardsdatascience.com](/do-we-need-object-orientated-programming-in-data-science-b4a7c431644f) [](/thoughts-on-finding-a-data-science-project-d4b74893f50) [## 关于寻找数据科学项目的思考

### 有时候你需要在工作中把一个项目交给别人，这没什么。

towardsdatascience.com](/thoughts-on-finding-a-data-science-project-d4b74893f50)