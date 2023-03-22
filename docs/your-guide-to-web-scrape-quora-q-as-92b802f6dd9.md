# 你的网刮 Quora 问答指南

> 原文：<https://towardsdatascience.com/your-guide-to-web-scrape-quora-q-as-92b802f6dd9?source=collection_archive---------43----------------------->

## 数据科学

## 只需几行代码，你就能抓取 Quora 的数据

![](img/24c0162e782e05b2ff51ccd53e4750ba.png)

来源:由 [Flickr](https://www.flickr.com/photos/thomashawk/5370003859) 中的[托马斯·霍克](https://www.flickr.com/photos/thomashawk/)(CC By-NC 2.0)

在本教程中，我将向您展示如何使用 [Anaconda](https://www.anaconda.com/) Jupyter 笔记本和 [BeautifulSoup](https://www.crummy.com/software/BeautifulSoup/) 库来执行 web 抓取。

我们将从 Quora 搜集问题和答案，然后我们将它们导出到[Pandas](http://pandas.pydata.org/)library data frame，然后导出到一个. CSV 文件。

让我们直接进入正题，然而，如果你正在寻找一个指南来理解网络抓取，我建议你阅读来自 [Dataquest](https://www.dataquest.io/blog/web-scraping-tutorial-python/) 的这篇文章。

# 1-使用一个 URL

让我们导入我们的库

```
import urllib
import requests
from bs4 import BeautifulSoup
import pandas as pd
```

插入你的 Quora 网址，一个例子显示。

```
url = ‘[https://www.quora.com/Should-I-move-to-London'](https://www.quora.com/Should-I-move-to-London')
```

然后让我们向 web 服务器发出一个`GET`请求，它将为我们下载给定网页的 HTML 内容。

```
page = requests.get(url)
```

现在，我们可以使用 BeautifulSoup 库来解析这个页面，并从中提取文本。我们首先必须创建一个`BeautifulSoup`类的实例来解析我们的文档:

```
soup = BeautifulSoup(page.content, ‘html.parser’)
```

然后，我们将创建熊猫数据框架来包含我们想要的问答。

```
df = pd.DataFrame({‘question’: [],’answers’:[]})
```

现在是从网页中选择问答类的时候了，类在抓取时被用来指定我们想要抓取的特定元素。

```
question = soup.find(‘span’, {‘class’: ‘ui_qtext_rendered_qtext’})
answers = soup.find_all(‘div’, attrs={‘class’: ‘ui_qtext_expanded’})
```

然后，我们可以通过将结果添加到之前创建的数据框架中来得出结论。

```
for answer in answers:
     df = df.append({‘question’: question.text,
         ‘answers’: answer.text
          }, ignore_index=True)
df
```

将结果导出到 CSV 文件的时间。

```
df.to_csv(‘One_URLs.csv’)
```

# 2-使用多个 URL

这一次我们将一起努力在同一时间内刮出不止一页。这一过程几乎与相对变化相同。

从导入库开始

```
import urllib
import requests
from bs4 import BeautifulSoup
import pandas as pd
```

添加所有你需要的网址，我们现在有 2 个

```
url = [‘[https://www.quora.com/What-are-best-places-to-relocate-and-settle-in-UK'](https://www.quora.com/What-are-best-places-to-relocate-and-settle-in-UK'), ‘[https://www.quora.com/Should-I-relocate-to-the-UK'](https://www.quora.com/Should-I-relocate-to-the-UK')]
```

创建我们的数据框架

```
df = pd.DataFrame({‘question’: [],’answers’:[]})
```

现在，我们将执行一个 for 循环，该循环将迭代两个 URL 来执行相同的过程，然后它可以将结果保存到 DataFrame 中。

```
for i in url:
   page = requests.get(i)
   soup = BeautifulSoup(page.content, “html.parser”)
   question = soup.find(‘span’, 
            {‘class’:      ‘ui_qtext_rendered_qtext’})
   answers = soup.find_all(‘div’, 
                attrs={‘class’:    ‘ui_qtext_expanded’})
   for answer in answers:
        df = df.append({‘question’: question.text,
          ‘answers’: answer.text
            }, ignore_index=True)
df
```

现在，让我们将数据帧导出到 CSV 文件。

```
df.to_csv(‘Two_URLs.csv’)
```

您现在应该对如何从 Quora 中抓取和提取数据有了很好的理解。一个很好的下一步，如果你对网络抓取有点熟悉，你可以选择一个网站，自己尝试一些网络抓取。

快乐编码:)