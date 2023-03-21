# Python 和 R 之间翻译的基本指南

> 原文：<https://towardsdatascience.com/essential-guide-to-translating-between-python-and-r-7cb18b786e5d?source=collection_archive---------12----------------------->

## 使用 Python 或 R 知识有效学习另一种语言的简单方法。

![](img/85ad99ac125e4b154761a5d24008051d.png)

作为一个以英语为母语的人，当我小时候开始学习西班牙语时，三个关键的练习帮助我从笨拙地试图将我的英语单词翻译成西班牙语，到直接用西班牙语思考和回应(偶尔甚至做梦)。

1.  把新的西班牙语单词和我已经知道的英语单词联系起来。对比西班牙语和英语单词使我能很快理解新单词的意思。
2.  **多次重复这个词，并在许多不同的场景中使用它，让这些词在我脑海中反复出现。**
3.  利用**上下文**的线索让我更好地理解了这个词是如何以及为什么要用在它的同义词上。

当你第一次学习编码时，重复和语境化是必不可少的。通过不断的重复，你开始记忆词汇和语法。当您开始看到在不同的上下文环境中使用的代码时，通常通过项目开发，您能够理解不同的功能和技术是如何以及为什么被使用的。但是，不一定有一种简单的方法可以将新的思维方式与您所说的语言联系起来，这意味着您不仅仅是记住一个单词，而是必须对每个编程概念形成新的理解。甚至你写的第一行代码，打印(“Hello World！”)需要你学习 print 函数的工作原理，编辑器如何返回 print 语句，以及何时使用引号。当你学习第二种编程语言时，你可以将概念从你所知道的语言翻译到新的语言中，从而更有效、更快速地学习。

数据科学领域分为 Python 拥护者和 R 爱好者。但是，任何已经学习了其中一种语言的人都应该利用他们的优势，一头扎进另一种语言中，而不是宣布支持哪一方。Python 和 R 之间有无限的相似之处，有了这两种语言，您可以用最好的方式解决挑战，而不是把自己限制在工具棚的一半。下面是连接 R 和 Python 的简单指南，便于两者之间的转换。通过建立这些联系，反复与新语言交互，并与项目联系起来，任何懂 Python 或 R 的人都可以很快开始用另一种语言编程。

# 基础知识

将 Python 和 R 放在一起看，你会发现它们的功能和外观非常相似，只有语法上的细微差别。

数据类型

```
# Python                              # R
type()                                class()
type(5) #int                          class(5) #numeric
type(5.5) #float                      class(5.5) #numeric
type('Hello') #string                 class('Hello') #character
type(True) #bool                      class(True) #logical
```

分配变量

```
# Python                              # R
a = 5                                 a <- 5
```

导入包

```
# Python                              # R
pip install packagename               install.packages(packagename)import packagename                    library(packagename)
```

数学:是一样的——数学在所有语言中都是一样的！

```
# Python                              # R
+ - / *                               + - / *# The same goes for logical operators
< #less than                          < #less than
> #greater than                       > #greater than
<= #less than or equal to             <= #less than or equal to
== #is equal to                       == #is equal to
!= #is not equal to                   != #is not equal to
& #and                                & #and
| #or                                 | #or
```

调用函数

```
# Python                              # R
functionname(args, kwargs)            functionname(args, kwargs)print("Hello World!")                 print("Hello World!")
```

If / Else 语句

```
# Python                              # R
if True:                              if (TRUE) {
    print('Hello World!')                 print('Go to sleep!')
else:                                 } else {
    print('Not true!')                    print('Not true!') 
                                      }
```

列表和向量:这个有点夸张，但是我发现这个联系很有帮助。

*   在 python 中，列表是任何数据类型的有序项目的可变集合。在 Python 中索引列表从 0 开始，并且不包含。
*   在 R 中，向量是同一类型有序项目的可变集合。索引 R 中的向量从 1 开始，包括 1 和 1。

```
# Python                              # R
ls = [1, 'a', 3, False]               vc <- c(1, 2, 3, 4)# Python indexing starts at 0, R indexing starts at 1
b = ls[0]                             b = vc[1]
print(b) #returns 1                   print(b) #returns 1c = ls[0:1]                           c = vc[1:2]
print(c) #returns 1                   print(c) #returns 1, 2
```

对于循环

```
# Python                              # R
for i in range(2, 5):                 for(i in 1:10) {
    a = i                                 a <- i }
```

# 数据操作

python 和 R 都提供了简单而流畅的数据操作，使它们成为数据科学家的必备工具。这两种语言都配备了能够加载、清理和处理数据帧的包。

*   在 python 中，pandas 是使用 DataFrame 对象加载和操作数据框最常用的库。
*   在 R 中，tidyverse 是一个类似的库，它使用 data.frame 对象实现简单的数据帧操作。

此外，为了简化代码，两种语言都允许多个操作通过管道连接在一起。在 python 中。在 r 中使用%>%管道时，可以用来组合不同的操作。

读取、写入和查看数据

```
# Python                              # R
import pandas as pd                   library(tidyverse) # load and view data
df = pd.read_csv('path.csv')          df <- read_csv('path.csv')df.head()                             head(df)df.sample(100)                        sample(df, 100)df.describe()                         summary(df) # write to csv
df.to_csv('exp_path.csv')             write_csv(df, 'exp_path.csv')
```

重命名和添加列

```
# Python                              # R
df = df.rename({'a': 'b'}, axis=1)    df %>% rename(a = b)df.newcol = [1, 2, 3]                 df$newcol <- c(1, 2, 3)df['newcol'] = [1, 2, 3]              df %>% 
                                         mutate(newcol = c(1, 2, 3))
```

选择和过滤列

```
# Python                              # R
df['col1', 'col2']                    df %<% select(col1,col2)df.drop('col1')                       df %<% select(-col1)
```

筛选行

```
# Python                              # R
df.drop_duplicates()                  df %<% distinct()df[df.col > 3]                        df %<% filter(col > 3)
```

排序值

```
# Python                              # R
df.sort_values(by='column')           arrange(df, column)
```

汇总数据

```
# Python
df.groupby('col1')['agg_col').agg(['mean()']).reset_index()# R
df %>% 
    group_by(col1) %>% 
    summarize(mean = mean(agg_col, na.rm=TRUE)) %>%
    ungroup() #if resetting index
```

使用过滤器聚合

```
# Python
df.groupby('col1').filter(lambda x: x.col2.mean() > 10)# R
df %>% 
    group_by(col1) %>% 
    filter(mean(col2) >10)
```

合并数据框

```
# Python
pd.merge(df1, df2, left_on="df1_col", right_on="df2_col")# R
merge(df1, df2, by.df1="df1_col", by.df2="df2_col")
```

上面的例子是在 Python 和 r 之间建立思维上的相似性的起点。虽然大多数数据科学家倾向于使用这两种语言中的一种，但是精通这两种语言使您能够利用最适合您需求的工具。

请在下面留下您认为有用的任何相似之处和附加评论。