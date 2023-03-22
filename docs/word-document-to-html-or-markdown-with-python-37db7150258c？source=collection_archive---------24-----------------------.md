# 用 Python 将 Word 文档转换成 HTML 或 Markdown

> 原文：<https://towardsdatascience.com/word-document-to-html-or-markdown-with-python-37db7150258c?source=collection_archive---------24----------------------->

## 初学者使用 Python 的一个例子。

![](img/5f67937d5abd116b0a3f0071480c795e.png)

阿曼达·琼斯在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上的照片

这篇短文将指导您如何将？docx word 文档到简单网页文档(*)。html* )或 Markdown 文档(*)。md* )在一些基于 Python 的 CLI 的帮助下— [**Mammoth**](https://pypi.org/project/mammoth/) 。

根据 [Statista 调查](https://www.statista.com/forecasts/1011672/popular-office-suite-brands-in-the-us)(2020 年 1 月 6 日)的统计，微软办公套件是最受欢迎的办公软件。你可以很容易地记笔记、短报告、教程文档等。用微软的 Word。并且，您可能希望将文档内容作为 web 文档与您的一些朋友、同事、客户共享(*)。html* ))或 Markdown 文档(*)。md* )。在过去，在网上托管一些 web 文档可能成本很高，但现在云服务对于一个公共文档来说非常便宜甚至免费(例如 [GitHub Pages](https://pages.github.com/) )。

## 让我们开始吧。

# 安装猛犸

要安装 Mammoth，请确保您的 PC 上安装了 Python 和 PIP。然后，您可以打开 CMD 或终端并使用以下命令:

```
$ **pip install mammoth**
```

# 将 Docx 转换为 HTML

使用 CLI:

```
$ **mammoth input_name.docx output_name.html**
```

使用 Python:

**转换。docx 到。用 Python 制作的 HTML&猛犸**(作者)

# 将 Docx 转换为 MD

使用 CLI:

```
$ **mammoth .\sample.docx output.md --output-format=markdown**
```

使用 Python:

**转换。docx 到。md 与巨蟒&猛犸象**(作者)

差不多就是这样！本文展示了一个使用 Python 从 Docx 转换到 web 文档的示例。我希望你喜欢这篇文章，并发现它对你的日常工作或项目有用。如果您有任何问题或意见，请随时给我留言。

关于我&查看我所有的博客内容:[链接](https://joets.medium.com/about-me-table-of-content-bc775e4f9dde)

安全**健康****吗**！💪

**感谢您的阅读。📚**