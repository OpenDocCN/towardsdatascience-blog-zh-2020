# SQL 中的字符串函数

> 原文：<https://towardsdatascience.com/string-functions-in-sql-9f08fabfa60e?source=collection_archive---------30----------------------->

## 分析 SQL 中的字符串远不止像“%…%”这样简单

![](img/892395a0dda0aff03ae74fe138771437.png)

使用 SQL 解开字符串看起来很困难，但是通过了解正确的函数会变得更容易

虽然 SQL 有时在功能更丰富的分析环境(如具有丰富功能库的 R 或 Python)旁边有一个“永久伴娘”的名声，但在过去几年中，处理字符串的标准函数越来越多地被采用，这大大增加了在 SQL 中处理字符串的可能性。

翻译和替换是许多数据库系统中都有的另外两个功能。它们已经存在很多年了，但是一些数据库，比如 SQL Server，只是最近才添加了它们；MySQL 仍然没有添加翻译。

这两个函数都用来替换字符串。REPLACE 用另一个字符串替换整个字符串，而 TRANSLATE 一次交换一个字符。

因此，您可以像这样使用 REPLACE，将一个完整的姓名或单词替换为另一个完整的姓名或单词:

```
SELECT REPLACE('I''m Bruce Wayne','Bruce Wayne', 'Batman');
```

结果:

```
I'm Batman
```

另一方面，as TRANSLATE 用其他特定字符替换字符，这对于克服数据中的可重复问题非常有用。例如，如果少量的字母或符号字符与数字数据混合在一起，妨碍了它们被正确地视为数字，那么这是一个很好的函数——只需使用 translate 来删除这些字符。另一个用途是，如果要导出为 csv 格式的表格中的文本数据中使用了逗号。如果您保留逗号并尝试导出，则很有可能无法正确检测到列，因为新列将被插入到不应该插入的位置。

例如，想象一个国家名称列表，其中，例如，朝鲜被列为“朝鲜，民主主义人民共和国”，而中国被列为“中国，人民共和国”——它们的官方英文名称被呈现，以便能够在朝鲜和中国下进行索引。

如果保留逗号，国家名称列有时会被拆分，破坏列与数据的对应关系。因此，您需要删除逗号。尝试以下方法:

```
SELECT TRANSLATE('China,Peoples Republic',',',' ')China Peoples Republic
```

TRANSLATE 函数的诀窍在于，要替换的字符列表和将要替换它们的列表需要顺序相同，这样 SQL 就知道如何处理它们。然而，总的来说，这是一个非常灵活的功能。

近年来标准中引入的另一个主要函数是 LISTAGG 函数。SQL Server 中也有此函数，语法与 STRING_AGG 类似。此函数将一列中不同单元格的字符串值连接成一个字符串，分隔符由用户指定。

使用此函数的一种方法是，如果您以组件形式存储姓名和地址数据(名、姓、门牌号码、街道名称等的单独列)，这样可以更容易地搜索特定属性，然后您可以将组件组合成完整的姓名和地址，并采用您喜欢的格式。尝试以下方法:

```
CREATE TABLE Names
(Names varchar(10));INSERT INTO Names Values
('John')
,('Paul')
,('George')
,('Ringo');SELECT STRING_AGG(Names,' and ')
FROM NamesResult: 
John and Paul and George and Ringo
```

知道如何在 SQL 中处理字符串似乎很困难。然而，这在很大程度上是因为那些帮助你正确处理字符串的函数并不像它们应该的那样广为人知。掌握上面讨论的功能会给你额外的灵活性来处理弦乐。

罗伯特·德格拉夫的书《管理你的数据科学项目》[](https://www.amazon.com/Managing-Your-Data-Science-Projects/dp/1484249062/ref=pd_rhf_ee_p_img_1?_encoding=UTF8&psc=1&refRID=4X4S14FQEBKHZSDYYMZY)**》已经通过出版社出版。**

*[*在 Twitter 上关注罗伯特*](https://twitter.com/RobertdeGraaf2)*