# 如何用 Python 生成 MS Word 表格

> 原文：<https://towardsdatascience.com/how-to-generate-ms-word-tables-with-python-6ca584df350e?source=collection_archive---------20----------------------->

## 30 秒内自动生成报告

![](img/c9cb89b679592109b91f41bac6a4efc6.png)

照片由[斯科特·格雷厄姆](https://unsplash.com/@sctgrhm?utm_source=medium&utm_medium=referral)在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄

在讨论自动化的时候，Python 是经常被提到的。与其他编程语言相比，易于使用的语法、大量的库和面向脚本的语言结构使 Python 在自动化方面更胜一筹。

小型和大型公司每天都使用 Word 和 Excel 等 Microsoft Office 工具生成报告。在本文中，我将提供关于通过 Python 在 MS Word 中生成数据表的端到端解释。

> *我将考虑一个场景，其中我们有许多新闻文章，我们需要生成一个 Word 文档，其中包含从这些文章中提取的统计数据。这些统计数据包括:*字数*，*句子数*和*平均单词长度*。*

## 属国

对于这个例子，我使用了 [Python 3.7](https://www.python.org/downloads/release/python-370/) 。您需要安装的唯一依赖项是 [python-docx](https://python-docx.readthedocs.io/en/latest/) 。您可以通过在您机器的终端中提交以下命令来安装这个依赖项。

```
pip install python-docx
```

## 从文章中提取统计数据

对于这个例子，我从 CNN 中随机选取了两篇体育文章。为了提取统计数据，我编写了下面的`describe_text(text)`函数。这个函数接受一个字符串参数(文章、评论等等)并返回一个统计数据字典，比如字符数、字数、句子数等等。

## 生成表格

首先你需要通过`Document`类实例化一个文档对象。对于每篇文章，通过调用`describe_text(text)`函数提取统计数据，并通过`add_table(row, columns)`函数创建一个表格。对于字典中的每个 stat，通过`add_row()`函数创建一行并添加相应的 stat。最后，通过调用`add_page_break()`函数在末尾添加一个分页符，以便在新的页面中显示每个表格。

关于纯 Python 函数的更多文本统计，请查看我下面的另一篇文章。

[](/10-pure-python-functions-for-ad-hoc-text-analysis-e23dd4b1508a) [## 用于即席文本分析的 10 个纯 Python 函数

### 不使用外部库压缩文本数据

towardsdatascience.com](/10-pure-python-functions-for-ad-hoc-text-analysis-e23dd4b1508a) 

# 结论

用 Python 生成 word 文档很容易。您所需要的就是 *python-docx* 库。这个库提供了在 MS Word 中完成大多数(但不是全部)事情的可能性。

以我的经验来看，我在几秒钟内就生成了数千份两位数页数的 Word 文档。我这样做是为了以更易读的格式组织日志数据，或者在分析产品评论文本数据后呈现书面发现。

[](/5-scenarios-where-beginners-usually-misuse-python-98bac34e6978) [## 新手通常误用 Python 的 5 种场景

### 更好地利用 Python

towardsdatascience.com](/5-scenarios-where-beginners-usually-misuse-python-98bac34e6978)