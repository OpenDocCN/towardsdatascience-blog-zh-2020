# 使用机器学习分析韩国流行音乐|第 1 部分—数据收集和清理

> 原文：<https://towardsdatascience.com/analyzing-k-pop-using-machine-learning-part-1-data-collection-cleaning-4b407baf7bce?source=collection_archive---------54----------------------->

## [K-Pop 机器学习教程系列](https://towardsdatascience.com/tagged/kpop-ml-tutorial)

## 这是教程的第 1 部分，我在这里收集 K-Pop 数据并清理数据。

![](img/8f9a125d3bf15bc183f4935a5b9d8392.png)

[萨维里·博博夫](https://unsplash.com/@dandycolor?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)在 [Unsplash](https://unsplash.com/?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) 上的照片

**视频版**附解说:[**https://youtu.be/lkhorCY5tFA**](https://youtu.be/lkhorCY5tFA)

我的整个**代码**:[**https://github . com/import Data/kpop-analysis/blob/master/K _ pop _ Data _ cleaning . ipynb**](https://github.com/importdata/kpop-analysis/blob/master/K_pop_Data_Cleaning.ipynb)

# 介绍

作为一个在韩国出生和长大的人，我是听着 K-pop 长大的。这些年来，韩国流行音乐成为了一种全球现象，它的流行程度至今仍让我难以置信。

所以，我认为使用机器学习来分析 K-pop 以探索有趣的见解会很酷。谢谢查宁(又名。[数据教授](https://www.youtube.com/channel/UCV8e2g4IWQqK71bbzGDEI4Q)为理念！

在这篇文章中，我将向您展示数据科学周期中的数据收集和数据清理阶段。

# 数据收集

我不得不做一些谷歌搜索来找到数据集。经过一番搜索，我发现这个网站有一个 excel 文件。这是一项关于社交媒体和韩国流行音乐的调查，我觉得很有趣。我喜欢他们问的问题，也喜欢最近进行的调查。

该数据集包含来自世界各地的 240 名 K-pop 粉丝，共有 22 个调查问题。

数据集链接:Rraman，Saanjanaa (2020): KPOP 数据. xlsx. figshare。数据集。[https://doi.org/10.6084/m9.figshare.12093648.v2](https://doi.org/10.6084/m9.figshare.12093648.v2)

# 数据清理

数据清理是一个重要的步骤，因为您需要用于 EDA 和模型构建的最干净的数据。如果你把垃圾放进去，那么你会从模型中得到垃圾。

数据集可能有前导空格和尾随空格。所以，我决定用这个函数去掉那些空白。然后我删除了第一列“时间戳”,因为它没有用。

函数来删除数据帧中的前导和尾随空格

因为列名就是问题，而且它们太长，所以我决定给它们取代码名来简化列。

![](img/01a8f1f26878ca14b7cd0982882022fa.png)

重命名列

接下来，检查数据集是否有空值。

![](img/a81215bc8ccc5aeb6a37ec1687aabad8.png)

检查空值

有三列包含空值。首先，让我们检查只有一个空值的列。

我发现 life_chg 和 money_src 中的空值都是“n/a”，于是干脆用字符串“none”代替。

对于“daily_MV_hr”列，我决定用平均值替换空值。有多种方法可以处理空值(删除行，分配唯一的类别，或者可以运行回归模型来预测丢失的值，等等)，但是我认为用平均值替换它们是最好的选择。

我取 1 和 4 的平均值，即 2.5 小时，去掉了“小时”这个词。我注意到有些类别在范围内，所以为了简单起见，我取了这些范围的平均值。我创建了一个特殊的函数来处理这个问题。

函数在一些有范围而另一些没有范围时寻找平均值

![](img/cf3a597ea99c8b9e1c170abb021baf25.png)

清洁“每日 MV 小时”色谱柱之前和之后

我意识到这个数据集有点乱。所以我重复了类似的步骤来清洁每根柱子。

*   “yr_listened”栏

![](img/31705f2affa5a60973d4c34f4194dd6c.png)![](img/281e7c85f6a1d94898b39569a9a060d4.png)

清理“yrs _ listened”列的过程

我将只向您展示每一列之前和之后的图片。

*   “每日新闻人力资源”栏目

![](img/bf2114ce7e52b45441bcd553a502acaf.png)![](img/9696777c5e022a282b69be52fd43c7f6.png)

《每日 _ 音乐 _hr》前后

*   “年度支出”栏

![](img/826d216cbf1d873538d0ec939ff14168.png)![](img/1b2c68b4a7dcd1ef8e214427866face4.png)

“yr _ merch _ spent”之前和之后

*   “年龄”栏

![](img/80a9f53c5f3b1691c366a31119bcfa87.png)

前后“年龄”

*   “收藏组”栏

![](img/1fccac82a1a54636275e8d252081d572.png)

原始列值

![](img/2ab6bec227f33c584e5ec9b3e055e9f1.png)

制作一个单独的列来查找每个人喜欢的组的数量

![](img/2da747e4177ada036583ba848f42083c.png)

BTS 与其他的单独列

*   “nes_medium”列

![](img/4b2a9696edf2d730eac211a23b810f1c.png)

原始列值

![](img/09ce4c0b3ec368f0d2440f6491e78e50.png)

简化的列值

*   “追求”栏目

![](img/d861b4438551d7aab0f4fc8738e84550.png)

原始列值

![](img/76c4f10fc4108ccf2e9ae2dde9ee79ba.png)

简化的列值

*   “时间常数”栏

![](img/6299eb85cb76f7bfe01487f663630c84.png)

原始列值

![](img/291ecdabc0d639633e3072e30dd13a8e.png)

简化的列值

*   “生活 _ 变化”栏

![](img/cb95d0b9a17a6a6eee74574b7d32b67a.png)

原始列值

![](img/21a7f3c43f5acd09d833267d0c92c312.png)

简化的列值

*   “pos_eff”列

![](img/e94c3a7b520854b471fa2251e7d8dd85.png)

原始列值

![](img/fd5a860242e41ff409f7cc190a9624b7.png)

简化的列值

*   “money_src”列

![](img/7d47a962b36d2d92a0fe6808d71cd019.png)

原始列值

![](img/6221952e2fa5335d4f212b8d03811ea0.png)

简化的列值

*   “疯狂 _ev”专栏

![](img/0e323b2aa8403f14af341d1603940817.png)

原始列值

![](img/6c29ea0114ef76fb4879546f9c2a3d8a.png)

简化的列值

*   “国家”栏

![](img/7e7aea8a4cefbfde946c4d259c968e43.png)

原始列值

![](img/4e2ff6375d8bd017cd1aa300154cc100.png)

简化的列值

终于清理完数据了！

我将清理后的数据框保存到一个 CSV 文件中，以供教程的下一部分使用。

![](img/01a38125172a5700312346adfb8595e8.png)

将清理后的数据帧保存到 CSV

在第 2 部分，我将讨论本教程的探索性数据分析部分。敬请期待！