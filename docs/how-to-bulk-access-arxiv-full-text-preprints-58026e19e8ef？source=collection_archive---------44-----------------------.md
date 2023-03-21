# 如何批量获取 arXiv 全文预印本

> 原文：<https://towardsdatascience.com/how-to-bulk-access-arxiv-full-text-preprints-58026e19e8ef?source=collection_archive---------44----------------------->

## 使用 Python3 和 MacOS X 命令行

![](img/3a289203fab69e14dda3a98c373355d4.png)

来源:[西科夫](https://stock.adobe.com/contributor/204072892/sikov?load_type=author&prev_url=detail)，via [土坯股票](https://stock.adobe.com/images/download-data-storage-business-technology-network-internet-concept/214879849)

自 1991 年成立以来，arXiv(科学预印本的主要数据库)已经收到了近 130 万份投稿。所有这些数据在分析中都是有用的，所以我们可能希望能够批量访问全文。这篇文章讲述了我们如何使用 Python 3 和 MacOS X 命令行来实现这一点。

# 使用命令行批量访问全文的步骤

尽管数据就在服务器上，但由于服务器容量有限，不建议直接抓取 arXiv。然而，arXiv 已经承认了批量访问的需求，通过[在亚马逊 S3](https://arxiv.org/help/bulk_data_s3) 上提供所有全文，每月更新。

arXiv 将它们的源文件存储在`arxiv`桶中。请求者必须付费。数据存储在按日期排序的大 tar 文件中(该文件的最后修改时间)。这有点不方便，因为我们需要处理整个 arXiv 语料库，即使我们只对特定的类别感兴趣。

下面我们将介绍下载全文的步骤，目标是`astro-ph`类别。

## 1.设置一个 AWS 帐户。

为了能够从亚马逊 S3 下载任何东西，你需要一个亚马逊网络服务(AWS)账户。在这里报名。你必须注册一张信用卡。

## 2.创建您的配置文件。

创建一个名为`config.ini`的文件，并添加您的 AWS 配置:

```
[DEFAULT]
ACCESS_KEY **=** access**-**key**-**value
SECRET_KEY **=** secret**-**key**-**value
```

要获取您的键值，请按照这里的说明[获取根访问键](https://www.cloudberrylab.com/blog/how-to-find-your-aws-access-key-id-and-secret-access-key-and-register-with-cloudberry-s3-explorer/)。

不要公开此文件！

## 3.创建 S3 资源并设置配置。

创建一个将从命令行运行的 Python 文件。在其中，初始化一个 S3 资源，这是一个面向对象的服务接口，并将其配置为使用您的 root 访问。

## 4.检查`arxiv`桶元数据。

arXiv 在一个名为`src/arXiv_src_manifest.xml`的文件中维护它们的桶中数据的有用信息。下载这个文件，以便更好地理解我们在做什么。

*注意:代码将只显示与当前步骤相关的新的或更改的部分，但是 Python 文件包含了从所有步骤到这一步的代码。完整的文件将显示在最后。*

浏览清单文件中的存储桶元数据。

当我们运行这段代码时，我们将看到清单文件最后一次更新的时间，以及`arxiv` bucket 保存了多少 tar 文件和总大小。当我运行这段代码时(2018 年 6 月 10 日)，这个桶包含 1，910 个 924 GB 大小的 tar。

## 5.下载`astro-ph`源文件！

请记住，每个 tar 都包含嵌套的 tar gzs，后者又包含特定类别的源文件。虽然我们必须下载每个 tar，但是我们可以提取这些源文件，而不需要解压缩所有 tar 和 tar gzs。我们还将筛选源文件，只提取标有`astro-ph`类别的文件，但也有其他类别可用。

# 完整代码

**如果你想阅读更多我的文章或者探索数以百万计的其他文章，你可以注册成为中级会员:**

[](https://brienna.medium.com/membership) [## 通过我的推荐链接加入 Medium-briena Herold

### 作为一个媒体会员，你的会员费的一部分会给你阅读的作家，你可以完全接触到每一个故事…

brienna.medium.com](https://brienna.medium.com/membership) 

**您还可以订阅我的电子邮件列表，以便在我发布新文章时得到通知:**

[](https://brienna.medium.com/subscribe) [## 每当布蕾娜·赫罗尔德发表。

### 每当布蕾娜·赫罗尔德发表。通过注册，您将创建一个中型帐户，如果您还没有…

brienna.medium.com](https://brienna.medium.com/subscribe) 

**你可能会对我的其他一些故事感兴趣:**

[](/how-to-download-twitter-friends-or-followers-for-free-b9d5ac23812) [## 如何免费下载 Twitter 好友或关注者

### Python 中的简单指南

towardsdatascience.com](/how-to-download-twitter-friends-or-followers-for-free-b9d5ac23812) [](/how-to-bulk-access-arxiv-full-text-preprints-58026e19e8ef) [## 如何批量获取 arXiv 全文预印本

### 使用 Python3 和 MacOS X 命令行

towardsdatascience.com](/how-to-bulk-access-arxiv-full-text-preprints-58026e19e8ef) [](/collecting-data-from-the-new-york-times-over-any-period-of-time-3e365504004) [## 从《纽约时报》收集任何时期的数据

### Python 中的简单指南

towardsdatascience.com](/collecting-data-from-the-new-york-times-over-any-period-of-time-3e365504004)