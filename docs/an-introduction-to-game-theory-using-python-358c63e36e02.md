# 使用 Python 介绍博弈论

> 原文：<https://towardsdatascience.com/an-introduction-to-game-theory-using-python-358c63e36e02?source=collection_archive---------16----------------------->

## [入门](https://towardsdatascience.com/tagged/getting-started)

## 使用博弈论来提高我的 Python 技能，并开发编写函数的直觉

什么是博弈论？在他的书《真实的游戏:博弈论文本》中，肯·宾默尔将其描述为对群体内部理性互动的研究。本质上，无论何时你和另一个人打交道，你都在玩一个游戏。我第一次接触博弈论是在一堂微观经济学导论课上。这看起来很奇怪，也不太直观。几年后，我参加了一个高级赛局的博弈论课程。它充满了数学符号、图表和痛苦。与我接触编码的计量经济学和计算经济学课程(分别用 R 和 Julia)不同，我在学习博弈论的时候没有写过一行代码。回想起来，我在那门课程中学到的技能在我的数据科学之旅中受益匪浅。我决定重新访问博弈论，并用它来提高我的 python 技能(并回忆痛苦)。

![](img/7d3d20a9f62a05d6e9a762bba2c9cca0.png)

[叶韩晶](https://unsplash.com/@yejinghan?utm_source=medium&utm_medium=referral)摄于 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

先说经典的例子:囚徒困境。我们的两位选手是朱利安和兰迪。他们都被逮捕并被带到警察局，然后被分开到不同的审讯室。审问我们队员的侦探没有足够的证据，他们需要一份供词。以下是我们玩家的策略和收益:

*   如果一个玩家坦白，而另一个玩家保持沉默，金色飞贼就自由了，而另一个玩家要服刑 10 年。
*   如果两个玩家都遵守规则并保持沉默，每个玩家都会因虚假指控而被判入狱一年。
*   如果两个玩家互相告密，他们每个人都会被判九年。(地区检察官好心因为合作减了一年刑)。

我们称保持沉默为鸽派策略，告密为鹰派策略(我从宾莫尔那里得到了策略名称和收益，它们比我笔记中的要好)。让我们用 python 来创建朱利安的收益矩阵:

```
import numpy as np
import pandas as pd# create an array with Julian's payoffs
x = np.array([[-1, -10],[0, -9]])# re-label the rows and columns
jpm=pd.DataFrame(x, columns=['dove', 'hawk'])
jpm.index = ['dove', 'hawk']
jpmOut[1]: 
      dove  hawk
dove    -1   -10
hawk     0    -9
```

朱利安由行表示，而兰迪由列表示。例如，如果朱利安扮演鸽子，而兰迪扮演老鹰，朱利安的收益是-10(他坐牢的年数)。如果朱利安扮演老鹰，兰迪扮演鸽子，朱利安的收益是 0(不用坐牢)。

现在我们来创建兰迪的收益矩阵。为此，我们需要交换行和列，因为兰迪的收益矩阵是朱利安的转置矩阵。这在 python 中很容易做到:

```
# create Randy's payoff matrix
# remember that Randy's payoff matrix is the transpose of Julian's
rpm=jpm.T
rpmOut[2]: 
      dove  hawk
dove    -1     0
hawk   -10    -9
```

创建收益矩阵的另一种方法是使用 nashpy 包，如下所示:

```
# Julian's payoffs (row player)
x = np.array([[-1, -10],[0, -9]]) # Randy's payoffs (column player)
y = x.Timport nashpy as nash
prisoners_dilemma = nash.Game(x,y)
prisoners_dilemmaOut[3]: 
Bi matrix game with payoff matrices:
Row player:
[[ -1 -10]
 [  0  -9]]Column player:
[[ -1   0]
 [-10  -9]]
```

现在是时候创建收益表了。收益表是一个双矩阵，包含朱利安和兰迪的收益矩阵写在一起。首先，我把它画出来:

![](img/80a8d0fb56e70d5ce8b5ea559b54af5f.png)

朱利安的收益是第一个数字，兰迪的是第二个。如果朱利安扮演鹰派，兰迪扮演鸽派，收益是多少？如果你选择了(0，-10)，干得好！收益表显示了给定策略和对手策略时每个玩家的收益。收益表看起来很漂亮，但是让我们使用 quantecon 包在 python 中创建它:

```
import quantecon as qe
# create a list containing both players payoffs
pt = [[(-1,-1), (-10,0)], [(0,-10), (-9,-9)]]
g = qe.game_theory.NormalFormGame(pt)
print(g)2-player NormalFormGame with payoff profile array:
[[[ -1,  -1],  [-10,   0]],
 [[  0, -10],  [ -9,  -9]]
```

您可能已经注意到上面的收益列表是通过一个名为 NormalFormGame 的函数传递的(它也在输出中)。囚徒困境代表了一个正常形式的博弈。你会问，那是什么？它由以下三个条件组成:

1.  一组球员(朱利安和兰迪)
2.  每个玩家的一套策略(鸽子，鹰)
3.  对于每个玩家来说，他们都有自己的策略偏好

*   快速注释、策略和策略配置文件是不同的。策略配置文件是一个列表，包含每个玩家的一个策略。想想石头、剪子、布这个游戏。策略配置文件有:(石头，石头)，(布，石头)，(剪刀，石头)等。

我们游戏的策略配置如下:{ *(鸽子，鸽子)，(鸽子，鹰)，(鹰，鸽子)，(鹰，鹰)* }

既然我们理解了策略和收益，我们可以写一个函数来进一步明确我们的理解:

```
# write the payoff function
def payoff_function (x=str, y=str):
    if x == 'dove' and y == 'dove':
        print('Julian {}'.format(-1),':','Randy {}'.format(-1))
    elif x == 'dove' and y == 'hawk':
        print('Julian {}'.format(-10),':','Randy {}'.format(0))
    elif x == 'hawk' and y == 'hawk':
        print('Julian {}'.format(-9),':','Randy {}'.format(-9), ':', "NE")
    else:
        print('Julian {}'.format(0),':','Randy {}'.format(-10))
```

我们之前已经建立了策略轮廓*(鹰派，鸽派)* = (0，-10)。如果我们写的函数是正确的，这应该是我们的输出:

```
payoff_function('hawk', 'dove')
Julian 0 : Randy -10
```

瞧啊。但现在问题来了，我们游戏中的代理人将如何最大化他们的偏好？朱利安和兰迪保持沉默似乎对双方都有好处。然而，每个玩家都面临着相同的收益和策略。记住，这个游戏是同时进行的。每个玩家通过玩鹰来最大化他们的收益(这是每个玩家避免被锁定的唯一可能的方法)。所以，如果我们的玩家是理性的，并寻求最大化他们的偏好，他们会一直玩鹰。在不可靠的经济语言中，鹰派严格控制鸽派。因此，我们游戏的结果将会是(*霍克，霍克*)。这代表了纳什均衡:

```
payoff_function('hawk', 'hawk')
Julian -9 : Randy -9 : NE
```

说这篇博客仅仅触及了博弈论的表面是一种保守的说法。然而，我真的相信博弈论背后的直觉是对一个人在数据科学中取得成功所需的思维类型的补充。我计划在未来写更多关于博弈论的文章，所以请留意！

我要感谢我以前的教授(悲惨世界的商人)吴建斌给了我写这篇文章的灵感。点击查看他的个人网站[。你有问题、意见、担忧或批评吗？我很想听听你在评论区的看法！](https://sites.google.com/site/jiabinwuecon/)