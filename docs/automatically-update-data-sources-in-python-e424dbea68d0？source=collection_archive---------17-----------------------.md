# 在 Python 中自动更新数据源

> 原文：<https://towardsdatascience.com/automatically-update-data-sources-in-python-e424dbea68d0?source=collection_archive---------17----------------------->

## 数据科学基本 Python 指南

## 以新冠肺炎数据集为例的分步指南

![](img/1ebcfb8d5f00d07a2436fc8010378df8.png)

由 [Chaeyun Kim](https://www.linkedin.com/in/ChaeyunKim) 更新数据集 Python 插图

在 Data Sciences 中，很多时候，您会从开源网站或 git 存储库中为您的 Python 项目获得一个外部数据源，比如一个 *csv* 表。其中一些数据集可能会定期更新。然后，您可能会在运行 Python 脚本以获得最新结果之前，打开 web 浏览器下载数据集。本文将向您展示一个基本指南，告诉您如何自动化这个过程，以一种简单的方式保持您的数据是最新的。

# 从网站(URL)更新数据集

可以从 *URL 中检索几个开源数据集。您*可以使用`urllib`模块直接在 Python 中打开并读取 *URL* 。它是大多数 Python 版本中预装的模块。所以你可以导入这个模块，使用下面的代码从任何 *URL* 下载数据。

```
**import urllib.request****url = '***<Your URL Here>***'
output = '***<Your output filename/location>***'
urllib.request.urlretrieve(url, output)**#.. Continue data analysis with updated output file
```

例如，下面的示例脚本显示了从罗伯特·科赫研究所(RKI，德国疾病控制和预防研究机构)更新德国新冠肺炎数据的示例。该数据为 *csv* 格式，可通过此 [*URL*](https://opendata.arcgis.com/datasets/dd4580c810204019a7b8eb3e0b329dd6_0.csv) 获取。在实际操作中，一些*URL*可能会随着时间的推移而改变或贬值，这使得 Python 脚本运行出错，因此我们可以包含`try/except`来检查 URL 是否仍然有效。假设你想保存下载文件的所有版本，你可以使用`datetime`模块给输出文件名添加一个时间戳。

从 URL 下载文件的 Python 脚本示例(示例:来自 [RKI](https://opendata.arcgis.com/datasets/dd4580c810204019a7b8eb3e0b329dd6_0.csv) 的新冠肺炎案例，使用 Python3.8)

*请注意，如果使用 macOS，应该先安装 Python 证书，然后才能使用`urllib`库。你可以通过运行 Macintosh HD>Applications>python 3 . x 文件夹中的`install certificates.command`文件轻松做到这一点。

# 从 Git 存储库更新数据集

如果您的 python 项目依赖于来自 git 存储库的数据。在您克隆存储库并将数据集获取到您的本地机器之后，您可能需要定期执行 git fetch 或 pull 来保持您的数据集是最新的。在这种情况下，您可以使用`GitPython`模块自动更新您的项目数据集。要使用这个模块，你必须先安装它，你可以很容易地使用`pip`这样做。

```
**pip install gitpython**
```

如果遇到错误，您可以查看完整的 GitPython [文档](https://gitpython.readthedocs.io/en/stable/intro.html)来检查您的操作系统环境可能没有的需求。安装完模块后，您可以轻松地按照这些代码在每次运行代码时自动执行 git pull。

```
**import git****repo = git.Repo('<your repository folder location>')
repo.remotes.origin.pull()**#... Continue with your Code.
```

例如，[JHU·CSSE](https://github.com/CSSEGISandData/COVID-19)Github 仓库提供了`csv`格式的新冠肺炎时间序列数据集，这些数据集每天都在更新。将存储库克隆到您的机器上之后，您可以将上述代码添加到 Python 脚本的开头，以便每次都自动获取最新版本。这样，您可以确保您的结果是最新的。

## 一些提示

对于 git 认证，如果您使用`ssh`，这将非常简单。如果您使用`HTTPs`，将您的用户名和密码保存到您的环境中会更方便。在 Mac 和 Windows 中，首次输入后会自动存储鉴定信息。在 Linux 中，您可以通过更改凭证超时来强制机器记住您的凭证。例如，您可以使用以下命令让您的计算机记住您的凭据一年。(31536000 秒)

```
$ **git config --global credential.helper 'cache --timeout=31536000'**
```

## 作者单词

本文展示了在 Python 脚本的开头直接从任何 URL 或 git 存储库更新数据集的分步指南。这样，您可以确保基于最新的数据集运行脚本。

我希望你喜欢这篇文章，并发现它对你的日常工作或项目有用。如果您有任何问题或意见，请随时给我留言。

关于我&查看我所有的博客内容:[链接](https://joets.medium.com/about-me-table-of-content-bc775e4f9dde)

安全**健康**健康**吗！💪**

**感谢您的阅读。📚**