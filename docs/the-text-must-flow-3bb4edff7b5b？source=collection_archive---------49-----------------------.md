# 文本必须流动

> 原文：<https://towardsdatascience.com/the-text-must-flow-3bb4edff7b5b?source=collection_archive---------49----------------------->

## 在弗兰克·赫伯特的沙丘上训练一个生成文本模型

![](img/d38d3625a94357433f19ba0d7840e328.png)

图片由[皮克斯拜](https://pixabay.com/?utm_source=link-attribution&amp;utm_medium=referral&amp;utm_campaign=image&amp;utm_content=2435404)的[格雷格·蒙塔尼](https://pixabay.com/users/GregMontani-1014946/?utm_source=link-attribution&amp;utm_medium=referral&amp;utm_campaign=image&amp;utm_content=2435404)拍摄

《沙丘》讲述了遥远未来的一个封建社会的故事。它聚焦于一个公爵和他的家人，他们被迫成为沙漠星球阿拉基斯的管家。弗兰克·赫伯特在 1965 年出版了这部经典著作。几乎任何现代科幻小说都可以追溯到沙丘的一些元素。你认为卢卡斯是从哪里得到塔图因的灵感的？

我最近完成了沙丘，它的续集(沙丘的弥赛亚)，刚刚开始系列的第三部，沙丘的孩子。有六个故事最初是赫伯特写的，然后*的儿子写了一吨*。我没读过那些。

我一直在探索生成建模，并想尝试一些文字！我觉得用 Dune 试试会很有趣。更“经典”的机器学习模型用于预测和聚类等事情。生成式建模允许模型创建它所学习的训练数据的新颖配置。最近的一个例子就是 StyleGAN，看看它的实际应用。

[这里是我在这个项目中使用的 Colab 笔记本的链接](https://drive.google.com/file/d/15Z7SNBnBL12acmUGvvMLQ-OoMspb-B5k/view?usp=sharing)。

## 我的过程

*   获得文本数据的语料库
*   清理数据！我有一些 unicode 字符，每当有分页符时就有“page”这个词——真烦人。每一章的开头都有摘录自一本真实世界的回忆录或书，我决定把这些拿出来。我还删除了每一章的后半部分，以帮助处理时间。
*   令牌化！这是删除标点符号，使事情小写，然后分裂成一个个单词的长字符串。该模型将从这些单词标记的顺序和频率中学习。还要注意，我们不会像这样删除 NLP 任务的停用词
*   建立模型。一定要使用 LSTM 层，输出层是词汇的大小。基本上，它所做的是给定一小部分文本，对下一个单词可能是什么进行分类
*   训练模型！Keras 建议在事情听起来不再像废话之前至少 20 个纪元。在我不小心关闭我的笔记本电脑并结束 Colab 会话之前，我已经 33 岁了。哎呀。
*   生成文本！我将在下面展示模型的一些输出

# 第一章:男爵的

我想测试一个纪元后，只是为了看看它吐出了什么。种子词是“男爵”——这本书的一个卑鄙的对手。

```
‘Baron The Baron Of The Baron Of The Baron Of The Baron Of The Baron Of The Baron Of The Baron Of The Baron Of The Baron Of The Baron Of The Baron Of The Baron Of The Baron Of The Baron Of The Baron Of The Baron Of The Baron Of The Baron Of The Baron Of The Baron Of The Baron Of The Baron Of The Baron’
```

就这样一直持续下去。一点都不太好！

33 个纪元后的模型表现明显更好，但它仍然陷入循环，只是不停地重复各种名词。以下是种子词“Spice”的输出:

```
The Spice Itself Stood Out To The Left Wall The Fremen Seeker Followed The Chains The Troop Was A Likely Shadow And The Natural Place Of The Great Places That Was A Subtle City Of The Room'S Features That The Man Was A Master Of The Cavern The Growing The Bronze The Sliding Hand
```

以下是“保罗”(主要角色)的输出:

```
Paul Stood Unable To The Duke And The Reverend Mother Ramallo To The Guard Captain And The Man Looked At Him And The Child Was A Relief One Of The Fremen Had Been In The Doorway And The Fedaykin Control Them To Be Like The Spice Diet Out Of The Wind And The Duke Said I Am The Fremen To Get The Banker Said When The Emperor Asked His Fingers Nefud I Know You Can Take The Duchy Of Government The Sist The Duke Said He Turned To The Hand Beside The Table The Baron Asked The Emperor Will Hold
```

以下是“她看了看”的输出:

```
'She Looked At The Transparent End Of The Table Saw A Small Board In The Room And The Way Of The Old Woman He Had Been Sent By The Wind Of The Duke And The Worms They Had Seen The Waters Of The Desert And The Sandworms The Troop Had Been Subtly Prepared By The Wind Of The Worm Had Been Subtly Always In The Deep Sinks Of The Women And The Duke Had Been Given Last Of Course But The Others Had Been In The Fremen Had Been Shaped On The Light Of The Light Of The Hall Had Had Seen'
```

# 想法和下一步措施

我认为这肯定显示了进步和改善。我想把它训练到至少 100 个纪元，但是进展很慢。每个时期大约 11 分钟，所以总共超过 18 个小时。我需要一台更好的电脑。

非常感谢 Susan Li 对这个主题的精彩描述，我在进行这个探索时使用了她的例子。这里是她的文章的链接。

最后，我想补充一点，我没有忘记这么做的讽刺意味。在沙丘宇宙中，在远古的某个时刻,“思维计算机”背叛了人类，几乎将他们消灭。在这本书的时代，计算机已经被“心智”所取代——人类被培育和训练来模仿计算机的计算能力。