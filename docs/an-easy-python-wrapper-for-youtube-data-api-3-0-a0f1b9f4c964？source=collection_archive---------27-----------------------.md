# 一个简单的 YouTube 数据 API 3.0 的 Python 包装器

> 原文：<https://towardsdatascience.com/an-easy-python-wrapper-for-youtube-data-api-3-0-a0f1b9f4c964?source=collection_archive---------27----------------------->

## [业内笔记](https://pedram-ataee.medium.com/list/notes-from-industry-265207a5d024)

## 一个收集 YouTube 数据和运行有趣的数据科学项目的伟大工具

![](img/2b3ea5ac624373a1f9d74c59d776f930.png)

[轴箱](https://unsplash.com/@rachitank?utm_source=medium&utm_medium=referral)在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍照

YouTube 是教育、娱乐、广告等等的主要来源之一。YouTube 拥有如此多的数据，数据科学家可以用它们来运行有趣的项目或开发产品。如果你是一个新手或者数据科学家专家，你一定听说过情感分析，这是自然语言处理的主要应用之一。例如，情感分析正被用于监控社交媒体或顾客评论。

当你在网上搜索时，你可以很容易地找到几个收集了亚马逊产品评论或 IMDB 电影评论的情感分析数据集。尽管如此，让您处理在线数据的 API 服务并不多。几周前，我决定在 YouTube 视频评论上运行一个情感分析项目。

> 对于没有经验的数据科学家来说，YouTube 数据 API 服务可能有点混乱。这就是为什么我决定编写一个用户友好的 Python 包装器来加速数据科学社区的开发。

我配置好一切，进行了我的实验。幸运的是，谷歌推出了一个强大的 API 来搜索符合特定搜索标准的 YouTube 视频。然而，我发现他们的数据服务对于没有经验的数据科学家来说可能有点混乱。这就是为什么我决定为 YouTube 数据 API 编写一个名为`youtube-easy-api`的用户友好的 Python 包装器。该模块帮助社区更快地运行更有趣的数据科学项目。在本文中，我想向您展示如何使用这个模块。希望你喜欢它。

[](https://read.amazon.ca/kp/embed?asin=B08D2M2KV1&preview=newtab&linkCode=kpe&ref_=cm_sw_r_kb_dp_8QSSFbEFAX8DD&reshareId=CK4K0MBDHW11XYFQ5EFE&reshareChannel=system) [## 人工智能:非正统的教训:如何获得洞察力和建立创新的解决方案…

### 通过 Kindle 分享。描述:这不是你的经典 AI 书。人工智能:非正统的教训是一个…

read.amazon.ca](https://read.amazon.ca/kp/embed?asin=B08D2M2KV1&preview=newtab&linkCode=kpe&ref_=cm_sw_r_kb_dp_8QSSFbEFAX8DD&reshareId=CK4K0MBDHW11XYFQ5EFE&reshareChannel=system) 

# 获取您的 Google 证书

首先，您必须设置您的凭据，然后才能使用此模块。如果你有你的 Google API 密匙，你可以跳过这一节；否则，看看下面的视频。当你想初始化`youtube-easy-api`模块时，你必须通过`API_KEY`。

# 从 PyPI 服务器安装模块

首先你要从 PyPI 服务器安装 Google 开发的必备库如下:`google-api-python-client`、`google-auth-oauthlib`、`google`。然后，您可以使用`pip`命令安装`youtube-easy-api`模块，类似于上面的库。

```
pip3 install youtube-easy-api
```

[](https://pypi.org/project/youtube-easy-api/) [## youtube-easy-api

### 这个模块提供了一个简单的界面来提取 Youtube 视频元数据，包括标题、评论和统计数据。你…

pypi.org](https://pypi.org/project/youtube-easy-api/) 

# 现在，您可以开始使用该模块了…

`youtube-easy-api`模块目前支持两种方法`search_videos`和`get_metadata`。

## -如何在 YouTube 视频中搜索

您可以指定一个关键字，并使用`search_videos`方法在所有 YouTube 视频中进行搜索。这个方法接受一个`search_keyword`并返回一个有序的字典列表，每个字典包含`video_id`、`title`和`channel`。你可以在下面找到一个例子。必须在初始化步骤通过`API_KEY`。

```
from youtube_easy_api.easy_wrapper import *

easy_wrapper = YoutubeEasyWrapper()
easy_wrapper.initialize(api_key=API_KEY)
results = easy_wrapper.search_videos(search_keyword='python',
                                     order='relevance')
```

`order`参数指定 API 响应中使用的排序方法。默认值是`relevance`(即，按照与搜索查询的相关性排序)。根据原始 API 文档，其他可接受的值如下。

*   `date` —按时间倒序排序(创建日期)
*   `rating` —从最高评级到最低评级排序
*   `viewCount` —按视图数量从高到低排序

如上所述，`search_videos`方法返回一个字典列表。如果使用上面的调用，结果的第一个元素如下。

```
results[0]['video_id'] = 'rfscVS0vtbw'
results[0]['title'] = 'Learn Python - Full Course for Beginners ...[Tutorial]'
results[0]['channel'] = 'freeCodeCamp.org'
```

## -如何提取 YouTube 视频的元数据

有了`video_id`，就可以提取所有相关的元数据，包括标题、评论和统计数据。注意，URL 中还使用了`video_id`。因此，您可以使用`search_videos`方法、网页抓取工具或手动选择来检索`video_id`。你可以在下面找到一个例子。必须在初始化步骤通过`API_KEY`。

```
from youtube_easy_api.easy_wrapper import *

easy_wrapper = YoutubeEasyWrapper()
easy_wrapper.initialize(api_key=API_KEY)
metadata = easy_wrapper.get_metadata(video_id=VIDEO_ID)
```

最终可以提取出`metadata`字典中存储的`title`、`description`、`statistics`、`contentDetails`、`comments`等所有 YouTube 视频元数据；`get_metadata`方法的输出。你可以在下面找到一个例子。

```
metadata = easy_wrapper.get_metadata(video_id='f3lUEnMaiAU')
print(metadata['comments][0])'Jack ma is like your drunk uncle trying to teach you a life lesson. Elon musk is like a robot trying to imitate a human'
```

现在，您可以访问情感分析项目所需的所有数据。还有一点，你有一个每天调用谷歌设置的这项服务的限额。因此，如果超过这个限制，您将会遇到一个错误。你必须等待第二天或认购增加你的每日限额。

# 感谢阅读！

如果你喜欢这个帖子，想支持我…

*   *跟我上* [*中*](https://medium.com/@pedram-ataee) *！*
*   *在* [*亚马逊*](https://www.amazon.com/Pedram-Ataee/e/B08D6J3WNW) *上查看我的书！*
*   *成为* [*中的一员*](https://pedram-ataee.medium.com/membership) *！*
*   *连接上*[*Linkedin*](https://www.linkedin.com/in/pedrama/)*！*
*   *关注我* [*推特*](https://twitter.com/pedram_ataee) *！*

[](https://pedram-ataee.medium.com/membership) [## 通过我的推荐链接加入 Medium—Pedram Ataee 博士

### 作为一个媒体会员，你的会员费的一部分会给你阅读的作家，你可以完全接触到每一个故事…

pedram-ataee.medium.com](https://pedram-ataee.medium.com/membership)