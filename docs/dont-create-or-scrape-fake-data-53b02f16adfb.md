# 获取假数据的最简单方法

> 原文：<https://towardsdatascience.com/dont-create-or-scrape-fake-data-53b02f16adfb?source=collection_archive---------22----------------------->

## 这个简单的方法涵盖了大多数用例

编写代码的一个很好的经验法则，尤其是在 Python 中，是在你自己开始编写代码之前，在 PyPi 上寻找一个模块[或者仅仅使用 Google。](https://pypi.org)

如果没有人做过你正在尝试做的事情，那么你仍然可以找到文章、部分代码或一般指南。

如果有人*在你找到你需要的所有东西或者至少是其他人如何完成或试图完成的例子之前就已经完成了。*

在这种情况下，生成假数据是很多很多人以前做过的事情。在 PyPi 上搜索“虚假数据”会产生超过 10，000 个包。

![](img/176d296dba72c6c3fddb707f712c34e8.png)

照片由[马库斯·斯皮斯克](https://unsplash.com/@markusspiske?utm_source=medium&utm_medium=referral)在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 拍摄

# 哪一个？

在我看来，Faker 是其中的佼佼者。这个包唯一不能解决您需求的时候是当您需要一些罕见格式或数据类型的假数据的时候。即便如此，如果可能的话，我仍然建议使用 Faker 并重塑它所生成的内容。

以下是一些可用的数据生成器:

*   名字
*   地址
*   文本(段落、句子)
*   国际电脑互联网地址
*   severely subnormal 智力严重逊常
*   生日
*   用户代理字符串
*   电话号码
*   车牌号
*   条形码
*   …以及更多内容，[点击](https://faker.readthedocs.io/en/master/providers.html)查看完整列表。

您所要做的就是通过命令行上的 pip 来安装它:

```
pip install faker
```

或者如果你在 Jupyter 笔记本上，只需加上感叹号:

```
!pip install faker
```

# 生成用户

现在是好东西！生成 1000 个假用户简介就这么容易( **粗体**中的**是 Faker 代码，其余是熊猫)。**

```
**from faker import Faker**
import pandas as pd**faker = Faker()**
df = pd.DataFrame()for i in range(1000):
    df = df.append(**faker.profile()**, ignore_index=True)
```

这是这些数据的一个示例:

# 生成字段

如果您想获得一个单独的字段而不是一个完整的配置文件，也很简单:

```
from faker import Faker
faker = Faker()# Get a random address
faker.address()# Get a random person's name
faker.name()
```

同样，还有更多的字段可用，[您可以在文档中找到它们。](https://faker.readthedocs.io/en/master/providers.html)你甚至可以创建自己的数据提供商，[这里有一些](https://faker.readthedocs.io/en/stable/communityproviders.html)已经由社区提供的数据。

Faker 还支持多种语言，通过命令行运行，并植入随机数发生器以获得一致的结果。

希望这能为你节省一些时间！我使用 Faker 为压力测试、速度测试，甚至测试模型管道的错误生成数据。