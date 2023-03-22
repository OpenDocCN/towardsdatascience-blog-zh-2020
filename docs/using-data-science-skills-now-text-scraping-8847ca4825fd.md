# 现在使用数据科学技能:文本抓取

> 原文：<https://towardsdatascience.com/using-data-science-skills-now-text-scraping-8847ca4825fd?source=collection_archive---------45----------------------->

## 有一个繁琐的文档搜索任务？用 python 通过 5 个简单的步骤实现自动化。

![](img/b341c9dc23631ba4a91e8b8b7e55a556.png)

图片来自[皮克斯拜](https://pixabay.com/?utm_source=link-attribution&utm_medium=referral&utm_campaign=image&utm_content=2049626)

我们都被赋予了任务，而这些任务在本质上是乏味的。它们是手动的，很烦人。如果这是一个一劳永逸的项目，我们就努力完成它。有时候你知道将来会再次被要求。这时，您只需通过 python 脚本自动完成任务，这样就不会再有人受苦了。这是其中的一个例子。

最近，我被要求提供我们的数据分析师团队查询的每个数据库表、视图和列名的列表。十年，在此期间可能有 30 个数据分析师，在不同的位置有数百个不同的文档。每周都会添加新的表和列。我制作的任何列表都会频繁更改。这需要某种自动化。

> 我不可能手动查看成千上万的文档来寻找 SQL。那是疯狂的谈话！

我创建了一个可重复的 python 脚本。它也可以适用于不同的情况，如下所示。

*   确定包含特定客户引用的每个文档。
*   指出每一份提及某一法院案例或法律的文件。
*   客户更改了法定名称，需要更新哪些文件？
*   识别所有文档中的名字。

我已经确定了完成这项任务的五个步骤

## 1.你要去哪里找？

确定所有需要搜索的文件共享、代码库和内部网站点。

共享文件共享

确定团队使用的所有文件共享。如果人们把文件藏在他们的本地设备上，让他们把它们转储到一个共享的驱动器文件夹中。

Git/Bitbucket

决定是否要将 Git 仓库克隆到一个文件夹中来抓取 Git 本身。我只需要搜索某个主要的代码分支，所以我只是将它克隆到一个文件夹中。您甚至可以从您的开发工作中获得代码的本地克隆。

内部网站点

决定你将如何处理内部或外部网站。如果你使用外部网站，你可能需要先收集并转储你的发现。内部网站也是如此，尽管你可以使用 Sharepoint 或 Confluence APIs。在我的例子中，我对代码本身感兴趣，所以我不需要参考我们内部网站上的文档。

## 2.您想搜索什么类型的文件？

我在寻找运行的 SQL 和脚本来提取数据。我把搜索范围限制在。txt，。SQL，。hql，。您可能有其他类型的文件来拉。

## 3.你需要找到什么？

您可能在寻找一个特定的单词或术语。在我的例子中，我不得不寻找许多术语。您需要创建一个这些术语的列表。因为我没有所有可能的表、视图和列的列表，所以我必须生成这个列表。我将使用 DB2 作为例子:

## 4.创建你的单词表。

```
-- Generate my word list [DB2](https://www.ibm.com/support/knowledgecenter/SSEPEK_11.0.0/cattab/src/tpc/db2z_catalogtablesintro.html)
-- List of Tables in my set of databases
SELECT DISTINCT NAME , 'TABLE' AS TYPE, '' AS STATEMENT
FROM SYSIBM.SYSTABLES
WHERE CREATOR IN (<list of my databases>)UNION-- List of Columns in my set of databases
SELECT DISTINCT NAME , 'COLUMN' AS TYPE, '' AS STATEMENT
FROM SYSIBM.SYSTABLES
WHERE CREATOR IN (<list of my databases>)UNION-- List of Views in my set of databases, 
-- include statements for complex views
SELECT DISTINCT NAME , 'VIEW' AS TYPE, STATEMENT
FROM SYSIBM.SYSTABLES
WHERE CREATOR IN (<list of my databases>)
;
```

这里有一些您可以从中提取的其他数据库目录表。

```
-- SQLServer
SELECT NAME , 'TABLE' AS TYPE
FROM SYS.OBJECTS
WHERE TYPE_DESC = 'USER_TABLE'
UNION...SELECT NAME, 'COLUMN' AS TYPE FROM SYS.COLUMNS-- Oracle
SELECT .... sys.dba_tables and sys.dba_tab_columns-- [Glue Catalog](https://boto3.amazonaws.com/v1/documentation/api/latest/reference/services/glue.html#Glue.Client.get_databases)
this technique uses boto3 and get_database, get_tables (contains column info)
```

一旦你创建了一个单词列表，你就可以开始提取文档并扫描它们。

## 5.创建代码以生成匹配文件。

我在下面为这个例子创建了这个通用代码。我知道嵌套循环不是最有效的方法，但它对我的任务来说很好(与几百个文档相比，50k+单词)。

```
# import required packages
import pandas as pd
import glob
import os
from string import punctuation
import re
import sys# where are you searching?
folderPath = '<path to your top folder>\\'# read in all of the documents 
# I want to search in all of the folders, so I set recursive=Truedocs = glob.glob(folderPath + '/**/*.txt',recursive=True)
docs.extend(glob.glob(folderPath + '/**/*.sql',recursive=True)
docs.extend(glob.glob(folderPath + '/**/*.py',recursive=True)
#... keep adding whatever types you need# I need to remove punctuation, particularly the '.' separator between the database name and the table name
punctuation = '''!()-;:\,./?@%'''# Read in the word list that has been created
word_df = pd.read_csv('mywordlist.csv')
word_list = []
word_list = word_df['NAME'].tolist()# Create the dataframe to hold the matching words 
# and the file found in (optional)
matched_words = pd.DataFrame(columns = ['NAME', 'FILE'])"""Loop through all the files , remove punctuationn, split each line into elements and match the elements to the list of words"""
for doc in docs:
    if os.path.isfile(doc):
        f=open(os.path.join(folderPath,doc), 'r')
        for line in f:
            for ele in line:
                if ele in punctuation:
                    line = line.replace(ele, " ")
            line_list = []
            line_list = line.split() for x in line_list:
                matches = matches.append({'NAME': str(x), 'FILE': doc}, ignore_index=True# write my output file
matches.to_csv('matches.csv')
```

输出提供了匹配单词的列表以及保存匹配单词的文件名/位置。

额外的好处是这个列表包含了这个单词的每个实例。如果需要计数，可以很容易地将该逻辑应用到这个文件中。

## 结论

这个简单的脚本代替了一小时又一小时的苦差事。当六个月后有人再次提出这个要求时，很容易被复制。

采取措施消除你工作日中的苦差事。完全值得。

欢迎在下面分享你的代码片段。一如既往，有许多不同的方法来编码解决方案；我的样本只是其中之一。这可能不是最有效或最好的，但这种方法是可行的。