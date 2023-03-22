# 在 Power BI 中创建网络图

> 原文：<https://towardsdatascience.com/create-a-network-graph-in-power-bi-be232991ee06?source=collection_archive---------9----------------------->

## 数据科学/电力 BI 可视化

## 点击几下鼠标就能构建网络图的快速入门指南。

![](img/31a11ef0a56f43a135ef8ddfb35851c3.png)

艾莉娜·格鲁布尼亚克在 [Unsplash](https://unsplash.com/s/photos/network?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) 上的照片

*在上一篇* [*的文章*](/from-dataframe-to-network-graph-bbb35c8ab675) *中，我写了一个使用 networkx 和 matplotlib 可视化熊猫数据帧的快速入门指南。虽然学习和探索 Python 中的网络图很有趣，但我开始思考如何向那些机器上没有安装 Python 或 Jupyter Notebook 的人展示结果。在*[*task us*](https://www.taskus.com/)*，我们在大部分报告中使用 Power BI，因此我开始寻找一种定制的 Power BI 可视化技术，它可以获取数据并将其转换为有意义的网络图。*

进入网络导航器。

Network Navigator 是微软创建的 Power BI 中的一个自定义可视化工具。它允许您“通过平移和放大受力控制的节点布局(可以预先计算或实时制作动画)来浏览节点链接数据。”在本帖中，我们将通过使用自定义可视化工具创建网络图所需的步骤。

首先，让我们得到我们的数据。你可以在这里下载样本数据集[。然后，我们可以将数据加载到 Power BI Desktop 中，如下所示:](https://github.com/ecdedios/networkx-quick-start)

![](img/c131ec5e2845ed5f6c93eb348030fe10.png)

选择文本/CSV 并点击“连接”。

![](img/39682eafc60bfb6f32aef0fd6de9578c.png)

在 Windows 资源管理器文件夹中选择文件，然后单击打开:

![](img/b36b747ca9fdd67cff24cb95d20428f2.png)

点击“转换数据”。

![](img/ddc21489be1240c9c4eedffdcf8540bf.png)

点击“使用第一行作为标题”。

![](img/003af8dae63b98400ad775584b046c91.png)

点击“关闭并应用”。

![](img/70f549becd3b19a2585d863b1bb03c50.png)

接下来，找到“可视化”面板末端的三个点。

![](img/e5b6ef216b4c8b518cec743591b7b3a9.png)

并选择“获得更多视觉效果”。

![](img/4ad61c2a4d82d0c71bdf8ab5df4343b5.png)

将鼠标光标指向搜索文本框，输入“网络”，按“回车”键，然后单击“添加”按钮。

![](img/ff71127942c57981a062daa96d7c3840.png)![](img/7d0fde9d65c09b353eb3cf3f7d87c44a.png)

稍等片刻，您将看到下面的通知。单击“确定”按钮关闭通知。

![](img/7c8d9948b0c8e3d4d879daf0cfbe94af.png)

你会看到一个新的图标出现在“可视化”面板的底部，如下所示。

![](img/e841312e1229707c4455f39c1e75b61f.png)

点击新图标，你会看到类似下图的东西。

![](img/3f7701f09ed682b6635d8ccc66f6c101.png)

选择新的可视占位符后，单击“Fields”面板中的“Reporter”和“Assignee ”,它会自动将列分配为源节点和目标节点。

![](img/52665234343f87a00b9c716fab5e0a55.png)

让我们通过点击画笔图标来添加标签。

![](img/a2067437d5a6abaefe48d3b50c5ac428.png)

单击“布局”展开该部分，并向下滚动，直到看到“标签”部分。

![](img/58622262656ccbbce227b6b359547313.png)

点击“标签”下的切换开关将其打开

![](img/7d445147e2c140a51ffda0749be715ec.png)

瞧啊。

![](img/7bc4c39d6885b642de0d0b2355d29e58.png)

就是这样！只需点击几下鼠标，我们就能从 csv 文件创建网络图。

我希望你喜欢今天关于 Power BI 最酷的视觉效果之一的帖子。网络图分析是一个大话题，但我希望这个温和的介绍将鼓励你探索更多，扩大你的曲目。

在下一篇文章中，我将分享我从懒鬼到数据科学家的旅程，我希望它能激励其他人，而不是被仇恨者劝阻。

*敬请期待！*

你可以通过[推特](https://twitter.com/ecdedios)或 [LinkedIn](https://www.linkedin.com/in/ednalyn-de-dios/) 联系我。

[1]:商业应用程序—微软应用程序源。(2020 年 5 月 16 日)。*网络领航员图表*[https://app source . Microsoft . com/en-us/product/power-bi-visual s/wa 104380795？src = office&tab = Overview](https://appsource.microsoft.com/en-us/product/power-bi-visuals/WA104380795?src=office&tab=Overview)