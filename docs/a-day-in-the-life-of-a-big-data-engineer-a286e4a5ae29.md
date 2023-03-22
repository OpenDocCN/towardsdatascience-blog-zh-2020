# 大数据工程师的一天

> 原文：<https://towardsdatascience.com/a-day-in-the-life-of-a-big-data-engineer-a286e4a5ae29?source=collection_archive---------30----------------------->

## 为巴黎的一家广告技术巨头工作

*事实:*我处理大约 **200 PB** 的数据

> 根据 [Gizmodo](https://gizmodo.com/how-large-is-a-petabyte-5309889) :
> 
> 1 PB 相当于 2000 万个四屉文件柜，里面装满了文本或大约 13 年的高清电视视频。
> 
> 50 PB 是人类有史以来所有语言的全部书面作品

![](img/a43d8fb0b0eff0ce5fb3a984e85b26a4.png)

这是我们的数据中心的样子。来自 [Pexels](https://www.pexels.com/photo/interior-of-office-building-325229/?utm_content=attributionCopyText&utm_medium=referral&utm_source=pexels) 的[曼纽尔·盖辛格](https://www.pexels.com/@artunchained?utm_content=attributionCopyText&utm_medium=referral&utm_source=pexels)的照片

我在早上 7 点醒来

我给自己冲了第一杯咖啡。滚动浏览科技新闻，我启用了电子邮件通知。*你不应该在开始工作前打开电子邮件*，但是我们的数据随时可能发生任何事情，我们必须随时了解情况。

我大约在早上 8:30 到达办公室

还没有人在这里，因为我是空地上最早的鸟儿之一。安顿下来后，我看了看我们的数据监控工具:*资源消耗图、存储条形图、异常警报*。如果数据有问题，我必须检查一下。一些数据可能会丢失或具有不寻常的值。有时数据交易崩溃，我们最终没有数据。万一发生那些不好的事情，我需要第二杯咖啡。

![](img/b795bda16c9f48587eab4990d005e7b2.png)

斯蒂芬·道森在 [Unsplash](https://unsplash.com/s/photos/graph?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) 拍摄的照片

接下来，我检查我们的**数据调度器**。我们每天摄入数百万亿字节。我们不能手工处理从一个管道到另一个管道的所有数据。我们需要数据调度程序，通过它我们可以执行查询，在数据库之间复制数据，最重要的是，为这些命令设置一个计时器。调度器是数据管道的重要组成部分，数据量超过了人工处理的能力。我们所有的数据调度程序都是内部的，因为我们工程师喜欢手工制作。

```
AN EXAMPLE INSTRUCTION FOR A SCHEDULER[**1\. Create table if not exists**](#)
- Schema:
  + Column 1: name + data type
  + Column 2: name + data type
- Storage format: text
- Table name: Table A[**2\. SQL query**](#)
INSERT INTO Table A
SELECT Column 1, Column 2
FROM Table B
WHERE $CONDITIONS[**3\. Frequency = Daily**](#)
```

我每天早上 **10:30** 和队友开每日例会。*我们互相告诉对方前一天我们做了什么，那天我们要做什么，以及是否有什么阻碍了我们。我们尽量说得简洁，以节省每个人的时间，因为我们讨厌冗长的会议。我发现这些紧凑的聚会很有帮助，因为它保持了每个团队成员之间最少的互动。*

我通过做一些代码审查回到我的日常任务中。*你的编程在融入生产之前需要经过同行的评判*。他们可以批准、拒绝或调整你的一些工作。我意识到，不亲自和某人交谈，很难评估他们的代码。我通常选择来到他们的办公桌前，确保我们达成共识。你不想成为一个 ***【批准荡妇】***——一个总是不审核队友工作就批准他们工作的工程师。用你希望自己的代码被评估的方式来评估他们的代码。

![](img/6f0fbaf454fb9ccd93f354766e7f0156.png)

凯文·Ku 在 [Unsplash](https://unsplash.com/s/photos/code?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) 上拍摄的照片

中午，我的团队在公司食堂一起吃午饭。我们聊生活、工作、技术问题。每个人都可以开诚布公地讨论任何事情。我们的午休时间不长，很快我们就聚在厨房里喝另一杯咖啡——这是我的第三杯咖啡。

大约下午 **2 点左右**我又开始了我的软件开发工作。我们数据工程师仍然使用与数据相关的软件。我读取 SQL 查询，在 Hadoop 生态系统中运行测试，验证修改是否不会给存储带来太大压力，最后提交更改。开发可能会因为各种原因而停滞不前:代码无法编译，生产和测试环境不匹配，查询中的拼写错误。我重新倒了一杯咖啡。我数不清了。

下午晚些时候，我通常会接到执行产品发布的要求。我们希望更新表格，刷新查询，或者启动一个全新的管道。在一家专注于工程的公司工作，我很高兴看到我们自动化了每一个软件程序。我只需要按下一些按钮，让脚本处理整个事情。如果一切顺利，我们将有一个新版本的数据系统开始运行。

**下午 6 点**，该收工了。我确保数据调度程序仍按预期运行，监控仪表板中没有警告，没有人抱怨数据丢失或不正确。我们与世界各地的员工合作，所以我不会对半夜收到错误信息感到惊讶。现在，一切似乎都很好，所以我拿起我的电脑和我的同事说再见。

![](img/07c2092b28b24691b7748e380a086e44.png)

由[礼来朗姆酒](https://unsplash.com/@rumandraisin?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)在 [Unsplash](https://unsplash.com/s/photos/reading-books?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) 上拍摄的照片

关闭所有与工作相关的通知后，我通过阅读非技术书籍来享受一天的最后一刻，从而结束大数据工程师典型的一天。