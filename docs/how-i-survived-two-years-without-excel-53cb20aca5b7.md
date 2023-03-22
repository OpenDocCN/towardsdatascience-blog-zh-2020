# 我是如何熬过两年没有 Excel 的日子的

> 原文：<https://towardsdatascience.com/how-i-survived-two-years-without-excel-53cb20aca5b7?source=collection_archive---------49----------------------->

## 2018 年，我停止使用 Excel 冷火鸡，Python 使我成为一名更好的专业人士。这个故事讲述了我从 Excel 走向 Python 世界的个人历程。

![](img/d0a2167bad247a7baab2a54f4fc2b563.png)

在 [Unsplash](https://unsplash.com/s/photos/python?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) 上由 [Hitesh Choudhary](https://unsplash.com/@hiteshchoudhary?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) 拍摄的照片

两年前，我做了一个重大的改变，我不再使用 Excel，转而使用 Python。自从 1996 年我开始学习机械工程以来，Excel 已经成为我生活的一部分。不知不觉中，Excel 成了我日常工作的一部分，我用它来处理工作和家庭中的大部分事情。它只是偷偷溜进来，从未离开，直到我意识到它在限制我。我注意到世界的其他地方都在前进，我必须改变自己，为一个编码成为规范的新世界做准备。

> 最初的 100 个小时是最艰难的，但也是最有收获的。

我突然停止，一气呵成，没有回头。我觉得这是我做出改变的唯一途径。最初的 100 个小时是最艰难的，但也是最有收获的。当第一个 Excel 需求出现时，我感到不舒服，我感到紧张，几乎开始发抖，因为我知道我必须用 Python 来做。晚上我会做一些最基本的事情，比如从文件中读取数据(比如别人发来的 Excel 表格)，对列排序，重命名列，改变格式(datetime…arghh)，对数据分组，计算新列等等。我多次感到沮丧，因为我知道如何在 Excel 中通过几次点击来完成这些事情，但我坚持下来了，因为我决心不故态复萌，不给面子。然而，每次我设法把事情做完，我都感到非常满意，并且有更多的精力去迎接下一个挑战。

> 一天晚上，我突然意识到，当我感到沮丧时，我在学习，换句话说，沮丧是整个经历的一部分。挫折就像戒断症状，也就是一件好事。

在我停止之前，我确实通过学习 Python 的一些基础知识为自己做了准备。我知道，如果没有准备，我会很快回到我的老套路。这些是我所做的事情，目的是为了让我能够轻松地放弃 excel。如果这个列表对你来说太长了，**专注于学习熊猫**，因为这个库最接近 Python 中的 excel 和你的生命线。

我读这些书给我一个坚实的基础:

*   [面向儿童的 Python:有趣的编程介绍](https://www.amazon.com/Python-Kids-Playful-Introduction-Programming/dp/1593274076) — *学习 Python 的基本语法*
*   [与 Python 的数据角力](https://www.amazon.com/Data-Wrangling-Python-Tools-Easier/dp/1491948817)——*学习熊猫的诀窍，替换 excel 的库(你的生命线)*
*   [使用 Scikit-Learn 和 Tensorflow 进行机器学习](https://www.amazon.com/Hands-Machine-Learning-Scikit-Learn-TensorFlow/dp/1491962291) — *学习机器学习的概念，以激励我向前看并保持动力*

阅读像你这样的普通人写的关于 Python 和编码的文章也会有所帮助(也就是说，你并不孤单)。我开始阅读各种关于 Medium.com 的出版物，比如:

*   [编程](https://medium.com/topic/programming)
*   [Python 直白的英文](https://medium.com/python-in-plain-english)
*   [代码突发](https://codeburst.io/)
*   走向数据科学

我通过 MITs 开放课件(免费)学习了一门课程:“计算机科学和 Python 编程导论”本课程向您介绍基本概念和 Spider IDE。它迫使你通过实践刚刚学到的东西来建立新的习惯。我不认为你需要学习整个纳米学位来学习 Python，一门课程就足够了，然后迅速应用到现实生活的项目中。

[](https://ocw.mit.edu/courses/electrical-engineering-and-computer-science/6-0001-introduction-to-computer-science-and-programming-in-python-fall-2016/) [## 计算机科学和 Python 编程导论

### 6.0001 计算机科学和 Python 编程的介绍是为很少或没有…

ocw.mit.edu](https://ocw.mit.edu/courses/electrical-engineering-and-computer-science/6-0001-introduction-to-computer-science-and-programming-in-python-fall-2016/) 

> 我认为学习 Python 不需要读一个纳米学位。最重要的是做真正的项目。

一旦做好准备，我就觉得可以轻松地跨过去，就像处理任何其他成瘾问题一样(我的情况很好)，你需要迅速养成新习惯。在这种情况下，新习惯意味着你需要在现实生活项目中工作；最好是你工作中的项目。对于所有这些项目，我使用了 [Jupyter Lab](https://jupyterlab.readthedocs.io/en/stable/) 。

我参与的一些项目:

*   分析维护记录以计算设备的平均修复时间(MTTR)
*   分析维护活动，并确定这些活动的类型和成本的长期趋势
*   分析实时工厂信息(数百万行传感器数据，如压力、温度和流量)，以了解导致跳闸和故障的事件
*   应用 [pyautogui](https://pyautogui.readthedocs.io/en/latest/) 来自动批准休假——以及 IT 请求(并且通过每 5 分钟移动鼠标来看起来很忙…)
*   使用[镶嵌方](https://opensource.google/projects/tesseract)在技术图纸上进行光学字符识别
*   安全和维护记录的 NLP 主题建模
*   关于设备故障(阀门、压缩机、冷却器、仪器仪表)的机器学习

正如你从这些项目中看到的，我很快进入了 Python 的更高级的使用。这些项目无法在 excel 中完成，这只是展示了离开 excel 是多么的自由，并最终使我成为一名更好的专业人士。

![](img/3e1101dde1e5405096a9bf30c1bbdbea.png)

照片由[布鲁斯·马尔斯](https://unsplash.com/@brucemars?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)在 [Unsplash](https://unsplash.com/s/photos/success?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) 上拍摄

这是不是意味着我完全不用 Excel 了？不完全是不幸的。只有当有人给我发来一份 excel 表格，其中包含了大量的数据争论，并且重做每一件事会花费太多的时间时，我才会使用 excel。然而，我停止了创建新的 excel 表格，现在开始默认使用 Python，因为我知道我需要保持我新获得的技能。

对我来说，两年前离开 Excel 给了我启发，并开启了许多新的机会。我已经生存了 2 年，我知道我会继续下去。再见 Excel，不会想你的…