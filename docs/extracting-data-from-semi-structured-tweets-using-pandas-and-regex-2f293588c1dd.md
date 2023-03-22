# 使用 Pandas 和 regex 从半结构化推文中提取数据

> 原文：<https://towardsdatascience.com/extracting-data-from-semi-structured-tweets-using-pandas-and-regex-2f293588c1dd?source=collection_archive---------22----------------------->

## 使用系列字符串函数和正则表达式从文本中提取数字数据

![](img/b499a9883676374259f1e248f9571eb0.png)

华盛顿州渡口。在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上由[奥克](https://unsplash.com/@jetcityninja?utm_source=medium&utm_medium=referral)拍摄的照片

今天，我们将华盛顿州渡轮推文转化为以小时为单位的等待时间。推文有一些结构，但似乎不是自动化的。目标是转变:

*   Edm/King —埃德蒙兹和金斯敦码头状态—等待 2 小时
*   Edm/King —金斯敦码头状态—等待两小时
*   Edm/King —埃德蒙兹终端状态—等待一小时
*   Edm/King —离开埃德蒙兹的司机无需长时间等待
*   Edm/King —从金斯敦和埃德蒙兹出发需等待一小时

输入特定位置的等待时间(以小时为单位)(例如，埃德蒙兹:2，n/a，1，0，1。金斯敦:2，2，不适用，不适用，1。)

有趣的是，还有一些更不标准的推文:

*   Edm/King —更新—埃德蒙兹和金斯敦码头状态，2 小时 Edm，1 小时 King
*   EDM/King——在金斯敦没有长时间等待——在埃德蒙兹等待一小时，晚班船
*   Edm/King —金斯敦早上 6:25 发车取消。一小时，等等
*   EDM/King——从埃德蒙兹或金斯敦出发不再需要长时间等待
*   Edm/King —埃德蒙兹和金斯敦码头状态—等待 2 小时
*   Edm/King —埃德蒙兹终端状态—等待 90 分钟
*   Ed/Ki —埃德蒙兹等待时间— 60 分钟

您可以看到，这是一个非常特殊的任务，但它可以转移到各种设置，例如 1)操作 Pandas 字符串系列数据，2)使用正则表达式在字符串中搜索目标信息。所以我们去兜兜风吧！

![](img/777ee6fccf0d53335c1eaef5fab43b6d.png)

照片由[查德·佩尔托拉](https://unsplash.com/@chadpeltola?utm_source=medium&utm_medium=referral)在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄

# 用熊猫操纵弦系列

在任何清理之前，我的推文实际上(大部分)遵循这样的模式:Edm/King——无论什么——无论什么……有时[没有] WSP 登机牌……链接到华盛顿州轮渡通知页面”。我想删除这些对我当前意图无用的部分，以得到我需要的位置和时间数据。

pandas 通过 Series.str.method()方便地提供了各种字符串处理方法。点击查看总结文档[。向上滚动查看更多创意和使用细节。](https://pandas.pydata.org/pandas-docs/stable/user_guide/text.html#method-summary)

在这种情况下，我使用了`.str.lower()`、`.str.strip()`和`.str.replace()`。注意`.str.replace()`默认为 regex=True，不像基本的 python 字符串函数。

```
# changing text to lowercase and removing the url, 
df['tweet_text'] = df['tweet_text'].str.lower()\
                                   .str.replace('https:.*', '')# removing route indicator ('edm/king -'), extra whitespace
df['tweet_text'] = df['tweet_text'].str.replace('edm/king -', '')\
                                   .str.strip()# removing wsp boarding pass indicataor
wsp = ', no wsp boarding pass required|, wsp boarding pass required'
df['tweet_text'] = df['tweet_text'].str.replace(wsp, '')
```

我还用它将我的列名从['Tweet permalink '，' Tweet text '，' time']修改为['tweet_permalink '，' tweet_text '，' time']

```
df.columns = df.columns.str.lower().str.replace(‘ ‘, ‘_’)
```

![](img/6b11bbe0a4e7629a2380f950c77a1d5c.png)

[Jordan Steranka](https://unsplash.com/@jordansteranka?utm_source=medium&utm_medium=referral) 在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 拍摄的照片

# 使用正则表达式(regex)

## 正则表达式。包含带有或的()

我想说，如果 tweet.contains('1 '或' 1 '或' 60 分钟')，那么返回 1。

不幸的是，python 字符串没有方法`.contains()`。您可以使用`bool(string.find() +1)`来解决这个问题。但是这个不支持正则表达式。所以我的代码应该是这样的:

```
if (bool(tweet.find('1') +1) or bool(tweet.find('one') +1)
    or bool(tweet.find('60 minute')):

    return 1
```

取而代之的是正则表达式，我将我的 bool 转换计数减少到 1:

```
if bool(re.search('1|one|60 minute', tweet)): return 1
```

请注意，如果您只是检查一个字符串在另一个字符串中，您可以像这样使用基本 python 的`in`:

```
if 'one' in tweet:
```

## 正则表达式匹配多种方式来表达同一件事

完全披露，这需要一点点的试验和错误为我的情况。在识别出其中包含等待时间的推文后，我想对没有(延长)等待的推文进行适当的分类，并将它们与没有等待时间的推文(“埃德蒙兹码头等待时间标志停止服务”)和碰巧没有等待时间的推文(“金斯敦码头等待时间——一小时”)区分开来。没有 wsp 登机牌”)。

这里你需要在你的文本中寻找模式。我决定使用“no”后跟不确定数量的字符(在正则表达式句点(。)代表任何字符，星号(*)代表任何重复次数)，后跟“等待”。

```
bool(re.search('no.*wait', text))
```

在评估等待时间之前，我确实切断了非常常见的“[不] wsp 登机牌”和华盛顿州渡轮更新网页的持续链接。这有助于简化文本搜索过程，减少意外匹配导致的错误机会。

额外提示:将多个 csv 加载到单个数据帧中。使用`glob`获取所有匹配正则表达式路径名的文件。在这种情况下，我需要数据文件夹中以 csv 结尾的所有文件。从那里，您可以读取它们中的每一个(这里有一个元组理解和`pd.read_csv()`，并使用`pd.concat()`将它们连接在一起

```
import globall_files = glob.glob("./data/*.csv")df_from_each_file = (pd.read_csv(f) for f in all_files)
df = pd.concat(df_from_each_file, ignore_index=True)
```

像往常一样，查看 GitHub [repo](https://github.com/allisonhonold/tweets_to_data_ferry_wait_blog) 中的完整代码。编码快乐！

![](img/7e0aa9b4444dfcfdb720c80f0dcb02cf.png)

在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上由 [Lidya Nada](https://unsplash.com/@lidyanada?utm_source=medium&utm_medium=referral) 拍摄的照片