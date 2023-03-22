# 网页抓取模板

> 原文：<https://towardsdatascience.com/the-web-scraping-template-by-durgesh-samariya-e2c663f196ef?source=collection_archive---------31----------------------->

## 网页抓取步骤总结。

![](img/17374b0b605d533c2e6d40013f061184.png)

由[马文·迈耶](https://unsplash.com/@marvelous?utm_source=medium&utm_medium=referral)在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 拍摄的照片

Web 抓取是自动从 web 中提取数据的过程。Web 报废包括从网页中获取数据，并根据需要存储它们。

在这篇文章中，我将分享我的模板，我用它来节省我的时间，避免一次又一次地写同样的东西。我使用 Python 编程语言进行 web 报废。

免责声明:此模板并不适用于所有网站，因为所有网站都不相同。然而，它在大多数时候是有效的。本帖不是教程帖。

# TL；速度三角形定位法(dead reckoning)

如果你想查看我的模板，请点击这里查看。

# 加载所需的库

第一步是加载所有需要的库。让我们导入所有必需的库。我正在使用 [BeautifulSoup](https://www.crummy.com/software/BeautifulSoup/bs4/doc/) 库进行网页清理。除此之外，我正在使用 [Pandas](https://pandas.pydata.org/docs/) 和 [request](https://requests.readthedocs.io/en/master/) 库。导入之前，请确保系统中安装了所有库。

```
!pip3 install bs4, pandas, request
```

# 从语法上分析

现在，所有需要的库都已加载。请求一个网站对于从网上删除数据是非常重要的。一旦请求成功，网站的全部数据/内容都是可用的。然后我们可以解析 bs4 的 URL，这使得内容以纯文本形式可用。

只需添加您想要删除的 URL 并运行单元格。

# 提取所需的元素

现在我们可以使用 soup.find()和 soup.find_all()方法从网页中搜索所需的标签。通常，我的目标是存储数据的表。首先，我总是搜索标题。通常，标题可以在标签中找到。所以让我们找到它们并存储在 Python list 中。

现在我们的标题存储在一个名为 headings 的列表中。现在，让我们找到可以在标签中找到的表体。

现在我们有了标题和内容。是时候将它们存储在 DataFrame 中了。在这里，我创建了一个名为`data`的数据帧。

最后，我们为将来的使用准备好了数据。我喜欢在将数据保存为 CSV 格式之前进行一些数据分析。

# 数据分析

分析数据至关重要。使用 pandas，我们可以使用 head()、describe()和 info()等不同的方法来执行数据分析。除此之外，您还可以检查列名。

一旦您分析了数据，您可能想要清理它们。这一步是可选的，因为在创建数据时并不总是需要。但是，有时候是需要的。

# 数据清理

这个模板有一些数据清理过程，比如从数据中删除不需要的符号和重命名列名。如果你愿意，你甚至可以添加更多的东西。

现在我们的数据可以保存了。

# 将数据保存到 CSV 中

让我们将数据保存在 CSV 文件中。您需要更改文件名。

这个帖子到此为止。从网上删除数据不仅仅局限于这个模板。有很多事情可以做，但这都取决于网站。这个模板有一些你必须重复的东西，这样可以节省你的时间。你可以在这里找到一个完整的 jupyter 笔记本。

**感谢阅读。快乐的网页抓取！**

如果你喜欢我的工作并想支持我，我会非常感谢你在我的社交媒体频道上关注我:

*   支持我的最好方式就是跟随我上 [**中级**](https://medium.com/@themlphdstudent) 。
*   订阅我的新 [**YouTube 频道**](https://www.youtube.com/c/themlphdstudent) 。
*   在我的 [**邮箱列表**](http://eepurl.com/hampwT) 报名。

**如果你错过了我的循序渐进指南**

[泰坦尼克号起航|第一部分](https://medium.com/towards-artificial-intelligence/getting-started-with-titanic-kaggle-447a309f2d19)

《泰坦尼克号》入门|第二部

如果你错过了我的 python 系列。

*   第 0 天:[挑战介绍](https://medium.com/@durgeshsamariya/100-days-of-machine-learning-code-a9074e1c42c3)
*   第 1 天: [Python 基础知识— 1](https://medium.com/the-innovation/python-basics-variables-data-types-and-list-59cea3dfe10f)
*   第 2 天: [Python 基础知识— 2](/python-basics-2-working-with-list-tuples-dictionaries-871c6c01bb51)
*   第 3 天: [Python 基础知识— 3](https://medium.com/towards-artificial-intelligence/python-basics-3-97a8e69066e7)
*   第 4 天: [Python 基础— 4](https://medium.com/javarevisited/python-basics-4-functions-working-with-functions-classes-working-with-class-inheritance-70e0338c1b2e)
*   第 5 天: [Python 基础知识— 5](https://medium.com/towards-artificial-intelligence/python-basics-5-files-and-exceptions-by-durgesh-samariya-5d892d170640)

**我希望你会喜欢我的其他文章。**

*   [给初学者的 Git](https://medium.com/dev-genius/learn-terminal-git-commands-in-3-minutes-3997b354382e)
*   [八月月度阅读清单](https://medium.com/@durgeshsamariya/august-2020-monthly-machine-learning-reading-list-by-durgesh-samariya-20028aa1d5cc)
*   [集群资源](https://medium.com/towards-artificial-intelligence/a-curated-list-of-clustering-resources-fe355e0e058e)
*   [离群点检测资源](https://medium.com/dev-genius/curated-list-of-outlier-detection-resources-35ed27d0e46e)