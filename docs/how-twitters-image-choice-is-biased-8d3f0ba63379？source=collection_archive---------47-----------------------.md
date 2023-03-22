# 推特的形象选择是如何偏颇的

> 原文：<https://towardsdatascience.com/how-twitters-image-choice-is-biased-8d3f0ba63379?source=collection_archive---------47----------------------->

## 但这不一定是种族歧视

![](img/3813c143fad3454aec11f0177a4aeb42.png)

Ravi Sharma 在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄的照片

Twitter 显示共享图片的预览图片。如果长宽比不是想要的，图像需要被裁剪用于预览。

种植的方式应该不是随机的。人们似乎认为它是“聪明的”，因为它选择了图像中有趣或合理的部分。也许选择了一种点击量最大的作物。

到目前为止，一切顺利。但是现在有了这个:

你可能看不到它，但是有两个图像:一个是参议员麦康奈尔，一个是前总统奥巴马。Twitter 选择在两张图片上都显示麦康奈尔。

德国喜剧演员 Abdelkarim 指着推特说，最好是金发碧眼的人:

像这样的 [推文](https://twitter.com/JefCaine/status/1307441209338544148)还有很多[。对许多人来说，这种对某些图片的偏见似乎是种族歧视。](https://twitter.com/_jsimonovski/status/1307542747197239296)

# 推特预览中白人比黑人出现的次数多吗？

[后续推文](https://twitter.com/bascule/status/1307440596668182528)肯定非常有趣，表明可能存在问题。从局外人的角度来看，这很难说。Twitter 当然会收到成千上万的图片，并为它们创建预览。一个公司的合理目标是最大化点击量。例如，Twitter 可以简单地根据用户改变预览图像。这意味着 Twitter 试图估计哪些用户更频繁地点击哪些类型的图片。如果用户更频繁地点击白人，那么它会更频繁地显示白人。

我不是说这是会发生的事情，但它可能会发生。

我们假设不是。让我们假设每个人在所有情况下都看到相同的预览图像。那么你仍然可以对点击率进行全局优化。如果推特用户更频繁地点击白人，推特用户就会看到白人。

# 有偏见的数据导致有偏见的结果

2016 年,《卫报》发表了一篇非常相关的文章，内容是一个 AI 评判美貌并偏爱白人:

[](https://www.theguardian.com/technology/2016/sep/08/artificial-intelligence-beauty-contest-doesnt-like-black-people) [## 一场选美比赛由人工智能评判，机器人不喜欢黑皮肤

### 第一次由“机器”评判的国际选美比赛本应使用面部等客观因素…

www.theguardian.com](https://www.theguardian.com/technology/2016/sep/08/artificial-intelligence-beauty-contest-doesnt-like-black-people) 

这篇文章中有一个关键要点:

> 机器学习系统需要数据。如果数据有问题，机器学习系统就有问题。

这个人工智能是在一个几乎没有黑人的数据集上训练的。机器学习算法从中得出的概括是，黑人与赢得这场比赛没有很好的关联。这是正确的。它成问题的地方在于它被用来判断“客观”的美。它是一台机器，不能有偏见。对吗？

# 偏见和种族主义

这肯定是有趣的琐事，但它有更大的影响吗？推特上曝光少的效果甚至是负面的吗？

肯定很奇怪，肯定应该修好。不过，我不认为这是种族歧视。在我看来，一些 Twitter 开发者不太可能有邪恶的总体计划来伤害黑人，他们宁愿展示白人，以防两种图像作物是可能的。称这是种族歧视分散了我们对过去几个月里看到的许多种族歧视案例的注意力。人们知道行动者意识到他们的行动可能产生的影响的事件。导致死亡的事件。

一个软件开发人员编写一个比随机裁剪更智能的图像裁剪算法并不是种族歧视。如果下面的说法是正确的，那简直就是草率的工作。

我自己也试过上传四个天线宝宝的图片。你现在可能会因为它显示的是一个只有白色的图像而感到愤怒，或者只是得出结论说这个算法相当糟糕:

# 针对有偏见的 AI 产品，我们能做些什么？

算法可能有偏差的一个简单原因是类别不平衡。与生产数据相比，这还可能与训练数据的不匹配相结合。一个类只是在训练数据集中更频繁地出现。处理较频繁类别的一种技术是对不太频繁的类别进行过采样，或者对较频繁的类别进行欠采样。这意味着该算法可以更频繁地查看代表性不足的类的训练数据。

通常，还可以找到特定应用的解决方案。例如，Twitter 可以干脆不显示预览图片。或者显示随机的作物。让用户选择作物也是一种选择。当谷歌面临机器学习产品的偏见问题时，他们也选择了一个激烈的选择:

[](https://www.theguardian.com/technology/2018/jan/12/google-racism-ban-gorilla-black-people) [## 谷歌对意外算法种族主义的解决方案:禁止大猩猩

### 2015 年，谷歌因一种图像识别算法将黑人照片自动标记为…

www.theguardian.com](https://www.theguardian.com/technology/2018/jan/12/google-racism-ban-gorilla-black-people) 

后处理是另一种选择。如果你注意到两个收成好的候选人都有脸，抛硬币决定哪一个脸会出现。你也可以尝试变得超级聪明，识别名人，并根据他们在过去 24 小时内的标签/提及量对他们进行排名。

有更多的事情要考虑。如果你感兴趣，我鼓励你阅读下面的文章。

# 请参见

*   黄嘉圆、亚历山大·j·斯莫拉、亚瑟·格雷顿、卡斯滕·m·博格瓦特、伯恩哈德·肖尔科普夫:[通过未标记数据纠正样本选择偏差](https://papers.nips.cc/paper/3075-correcting-sample-selection-bias-by-unlabeled-data.pdf)
*   [贾斯普里特·桑德胡](https://www.linkedin.com/in/jaspreetsan/?originalSubdomain=fr) : [理解并减少机器学习中的偏差](/understanding-and-reducing-bias-in-machine-learning-6565e23900ac)
*   [Salma Ghoneim](https://www.linkedin.com/in/salma-ghoneim/) : [5 种偏见&如何在你的机器学习项目中消除它们](/5-types-of-bias-how-to-eliminate-them-in-your-machine-learning-project-75959af9d3a0)