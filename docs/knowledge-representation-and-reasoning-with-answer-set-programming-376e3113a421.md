# 基于回答集编程的知识表示和推理

> 原文：<https://towardsdatascience.com/knowledge-representation-and-reasoning-with-answer-set-programming-376e3113a421?source=collection_archive---------26----------------------->

## ..或者为什么企鹅不应该飞。

阅读声明式编程和命令式编程的区别，并从代码示例(答案集编程、Python 和 C)中学习。

![](img/506bde98da9e86ac108bf9aa5cdb668b.png)

企鹅“罗杰”——有点无聊，因为他不会飞。(图片由 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上的 [66 北](https://unsplash.com/@66north?utm_source=medium&utm_medium=referral)拍摄)

# 介绍

在工业和科学领域，计算问题的数量似乎是无限的。对来自大量可用数据的新见解有着巨大的需求。为了获得这些知识，专门的人使用各种编程语言来设计和实现算法。

然而，当前现实世界中的许多问题都是复杂的(组合)搜索问题，我们试图用这些问题来自动化和优化流程，并支持人们的决策。这不涉及算法的编码，而是给定知识的编码*。换句话说，给定关于所有要考虑的规则和约束的知识，什么才算解决方案。声明性编程表达其逻辑，而不是编写描述计算控制流的语句。*我们想要实现什么*，而不是关于*我们如何*实现它的陈述。*

这被称为声明式编程。

# 为什么是声明式编程？

使用一种有前途的声明性编程范例，即回答集编程(ASP，有时也称为回答集 Prolog)，比流行的命令式编程方法提供了意想不到的优势，例如:

*   简短解决方案(以代码行数衡量)
*   透明度(包括可读性)
*   可靠性

还有很多。一旦你把问题分解成最小的部分，写下所有必要的知识，你不仅解决了计算的挑战。一个很大的优势是，你已经将知识数字化，可以用它来解决进一步的问题，或者以其他方式利用它。

> E 经验和知识可持续地获得并防止损失(例如，通过员工退休或解雇)。

此外，它加强了“业务人员”和程序员之间的合作。人工智能的整合得到支持，各个层面的接受度都会提高。这是一个基本的过程，只有在公司所有部门的合作下才能实现。

# 什么是 ASP，它是如何工作的？

答案集程序由以事实、规则(`head(X) :- body(X).`)或约束形式表达的给定知识组成。该程序由求解器加载，并返回一个“稳定模型”。这个所谓的*答案集*由所有可以使用给定的规则和约束导出的事实组成。生成有限数量的稳定模型作为解，最终选择其中的一个。
***注意:*** *接地及解决问题不在本文讨论范围内。*

让我们从逻辑学的角度考虑一个关于鸟类和企鹅的著名例子。众所周知，鸟会飞。这可以编码为 answert 集合规则:

```
canFly(X) :- bird(X).
```

规则读作*“如果* `*X*` *是鸟，那么* `*X*` *可以飞”*。现在我们来补充更多的知识！例如，事实告诉无条件的真理，就像海鸥'马尔文'是一只鸟，但企鹅'罗杰'也是一只。

```
canFly(X) :- bird(X).
bird(malvin).
bird(roger).== MODEL OUTPUT ==
Anwer: 1
bird(malvin) bird(roger) canFly(malvin) canFly(roger)
SATISFIABLE
```

因此，答案集告诉我们已知的事实，马尔文和罗杰是鸟类，但也得出结论，他们能够飞行。我们当中的生物学爱好者知道，企鹅不会飞，因此模型是错误的！

![](img/5c3bb34d8b018c325b14676ccfa7ddeb.png)

海鸥‘马尔文’——稍微炫耀一下，因为他会飞。(照片由菲尔·博塔在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄)

为了得到可接受的结果，我们需要以事实和完整性约束的形式向程序添加更多的知识。在这里，模型需要知道罗杰不仅是一只鸟，也是一只企鹅，而且没有一只企鹅会飞。

```
canFly(X) :- bird(X).
bird(malvin).
bird(roger).
seagull(malvin).
penguin(roger).
:- canFly(X), penguin(X).== MODEL OUTPUT ==
UNSATISFIABLE
```

这并没有带来预期的结果，并显示了彻底和非常仔细地思考问题以及解决方案是多么重要。这个模型告诉我们，就像程序员所说的，一只鸟可以飞，但是对于企鹅家族的鸟却没有任何建议。为了避免这一点，我们可以补充说，只有不是企鹅的鸟才能飞。

```
canFly(X) :- bird(X), not penguin(X).
bird(malvin).
bird(roger).
seagull(malvin).
penguin(roger).
:- canFly(X), penguin(X).== MODEL OUTPUT ==
Answer: 1
bird(malvin) bird(roger) seagull(malvin) penguin(roger) canFly(malvin)
SATISFIABLE
```

在第[1]行中添加这个 knwoledge，稳定模型由预期结果组成。

# 命令式编程与声明式编程

## ..解决数独游戏

让我们看另一个例子来突出上面提到的 ASP 的优点。数独是一个众所周知的益智游戏，流行解释搜索问题。给定一个包含 1 到 9 之间的数字或空格的初始 9x9 网格，所有空格都必须用数字填充。如果您找到所有的值，使得每一行、每一列和每一个 3×3 的子方块都包含数字 1-9，并且每个数字只出现一次，那么您就赢得了 Sudoko。

![](img/b5097d147a6f878b8529c81dcd6c5549.png)

典型的数独游戏(摘自 Potassco.org)。

## Python(命令式)

下面的代码片段将展示如何用 Python 解决数独问题。

用 Python 解数独。

## c(命令式)

用 C 解决数独游戏看起来和 Python 很像。方法上没有明显的区别。

用 c 语言解数独。

Python 和 C 都显示了描述计算的控制流的书面语句，因此，'*如何求解'*,以及如何为即将到来的步骤改变状态。

## ASP(声明性)

最后但同样重要的是，让我们看看 ASP 代码片段——初始化后，我们能够用不到 10 行代码解决**的数独游戏！**

```
% Initialize the game (given numbers)
sudoku(1, 1, 5).
sudoku(1, 2, 3).
sudoku(1, 5, 7).
sudoku(2, 1, 6).
sudoku(2, 4, 1).
sudoku(2, 5, 9).
sudoku(2, 6, 5).
sudoku(3, 2, 9).
sudoku(3, 3, 8).
sudoku(3, 8, 6).
sudoku(4, 1, 8).
sudoku(4, 5, 6).
sudoku(4, 9, 3).
sudoku(5, 1, 4).
sudoku(5, 4, 8).
sudoku(5, 6, 3).
sudoku(5, 9, 1).
sudoku(6, 1, 7).
sudoku(6, 5, 2).
sudoku(6, 9, 6).
sudoku(7, 2, 6).
sudoku(7, 7, 2).
sudoku(7, 8, 8).
sudoku(8, 4, 4).
sudoku(8, 5, 1).
sudoku(8, 6, 9).
sudoku(8, 9, 5).
sudoku(9, 5, 8).
sudoku(9, 8, 7).
sudoku(9, 9, 9). % define the grid
n(1..9).
x(1..9).
y(1..9).% each field contains exactly one number from 1 to 9
{sudoku(X,Y,N): n(N)} = 1 :- x(X) ,y(Y).% helper
subgrid(X,Y,A,B) :- x(X), x(A), y(Y), y(B),(X-1)/3 == (A-1)/3, (Y-1)/3 == (B-1)/3.% constraints
:- sudoku(X,Y,N), sudoku(A,Y,N), X!=A.
:- sudoku(X,Y,N), sudoku(X,B,N), Y!=B.
:- sudoku(X,Y,V), sudoku(A,B,V), subgrid(X,Y,A,B), X != A, Y != B.#show sudoku/3.== MODEL OUTPUT ==
Answer: 1
('sudoku', (2, 1, 6)), ('sudoku', (1, 2, 3)), ('sudoku', (9, 4, 2)), ('sudoku', (7, 2, 6)), ('sudoku', (7, 3, 1)), ('sudoku', (1, 9, 2)), ('sudoku', (5, 1, 4)), ('sudoku', (7, 4, 5)), ('sudoku', (4, 3, 9)), ('sudoku', (9, 1, 3)), ('sudoku', (4, 5, 6)), ('sudoku', (5, 6, 3)), ('sudoku', (2, 4, 1)), ('sudoku', (8, 1, 2)), ('sudoku', (8, 8, 3)), ('sudoku', (7, 9, 4)), ('sudoku', (8, 7, 6)), ('sudoku', (5, 4, 8)), ('sudoku', (7, 6, 7)), ('sudoku', (8, 6, 9)), ('sudoku', (6, 5, 2)), ('sudoku', (9, 7, 1)), ('sudoku', (3, 4, 3)), ('sudoku', (4, 7, 4)), ('sudoku', (3, 3, 8)), ('sudoku', (4, 8, 2)), ('sudoku', (1, 7, 9)), ('sudoku', (9, 9, 9)), ('sudoku', (9, 8, 7)), ('sudoku', (5, 9, 1)), ('sudoku', (4, 4, 7)), ('sudoku', (6, 9, 6)), ('sudoku', (7, 7, 2)), ('sudoku', (2, 7, 3)), ('sudoku', (5, 5, 5)), ('sudoku', (1, 5, 7)), ('sudoku', (1, 1, 5)), ('sudoku', (6, 3, 3)), ('sudoku', (2, 6, 5)), ('sudoku', (2, 9, 8)), ('sudoku', (8, 3, 7)), ('sudoku', (3, 6, 2)), ('sudoku', (3, 8, 6)), ('sudoku', (2, 8, 4)), ('sudoku', (1, 3, 4)), ('sudoku', (8, 2, 8)), ('sudoku', (3, 7, 5)), ('sudoku', (9, 5, 8)), ('sudoku', (4, 9, 3)), ('sudoku', (6, 4, 9)), ('sudoku', (7, 5, 3)), ('sudoku', (2, 5, 9)), ('sudoku', (8, 5, 1)), ('sudoku', (5, 3, 6)), ('sudoku', (4, 6, 1)), ('sudoku', (3, 9, 7)), ('sudoku', (1, 4, 6)), ('sudoku', (9, 2, 4)), ('sudoku', (5, 8, 9)), ('sudoku', (1, 8, 1)), ('sudoku', (6, 6, 4)), ('sudoku', (5, 7, 7)), ('sudoku', (7, 1, 9)), ('sudoku', (3, 5, 4)), ('sudoku', (6, 8, 5)), ('sudoku', (5, 2, 2)), ('sudoku', (2, 3, 2)), ('sudoku', (8, 9, 5)), ('sudoku', (9, 6, 6)), ('sudoku', (9, 3, 5)), ('sudoku', (6, 2, 1)), ('sudoku', (3, 1, 1)), ('sudoku', (4, 2, 5)), ('sudoku', (6, 1, 7)), ('sudoku', (4, 1, 8)), ('sudoku', (8, 4, 4)), ('sudoku', (2, 2, 7)), ('sudoku', (3, 2, 9)), ('sudoku', (1, 6, 8)), ('sudoku', (6, 7, 8)), ('sudoku', (7, 8, 8))}
SATISFIABLE
```

***注意*** *:例如，使用一个 ASP 的 Python 包装器，结果看起来就像上面的代码示例一样方便。*

与 Python 和 C 语言相比，这完全是关于“是什么”而不是“怎么做”——这是两种方法之间的根本区别。

在最开始，我们定义数独板是 9x9 的单元(或区域),我们使用数字 1 到 9 作为填充值。遵循每个字段只能填写 1 到 9 之间的一个数字的基本规则，我们定义了一个小助手。它说明哪些字段引用同一个*子网格*。最后，我们只需要约束来告诉求解器什么是可能的，什么是不可能的。第一行检查选择的数字在列中是否唯一，而第二个约束检查行的规则。第三个约束完善了游戏规则，根据该规则，一个数字在子网格中也必须是唯一的。

就这样。我承认语法需要时间来适应。但是一旦你熟悉了它，你就能创造出非常易读的程序，为我们解决高度复杂的问题。

# 摘要

在知识表示和推理领域，ASP 是一个非常有前途的知识保存和声明性问题解决工具。

**简短的解决方案** —除了最初绘制数独游戏的努力之外，ASP 还提供了迄今为止最短的解决方案(以代码行数衡量)。
**—无需创建用户不清楚的规则，也无需创建与程序状态相关的规则。只有游戏的基本条件和三个规则必须被编码，才能得到正确的结果。
这在给出的例子中可能并不重要，但在许多工业应用中却是一个主要问题。**

**仅举几个现实世界的例子，美国国家航空航天局的航天飞机决策支持、瑞士的火车调度(世界上最密集的火车网络之一)或地中海沿岸最大的转运站的团队建设都是其潜力的证明。**

## **结果**

**人工智能在工业界的接受度还是很低的。此外，许多公司被现有的海量数据淹没了。**

> **将运营主题专家的知识与人工智能的优势结合在一起。**

**ASP 为数字化和保护现有知识提供了独特的机会。由于良好的可读性和对流程以及要解决的问题的必要理解，IT 和业务走得更近，从而获得更好和更可持续的成功。**

## **你喜欢这个故事吗？**

**我很欣赏这篇文章的点赞和转发——很快会有更多关于这个话题的内容！您还可以在此处找到执行摘要[。](/how-machine-learning-made-ai-forget-about-knowledge-representation-and-reasoning-cbec128aff56)**