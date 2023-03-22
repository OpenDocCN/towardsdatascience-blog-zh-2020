# 分步指南:使用 Python 进行数据科学的比例采样

> 原文：<https://towardsdatascience.com/step-by-step-guide-proportional-sampling-for-data-science-with-python-8b2871159ae6?source=collection_archive---------14----------------------->

## 从头开始使用 python 理解数据科学所需的比例采样的概念和实现

![](img/54767b89602baf13849fb073f266e674.png)

图片由 [jakob5200](https://pixabay.com/users/jakob5200-10067216/?utm_source=link-attribution&utm_medium=referral&utm_campaign=image&utm_content=5078866) 来自 [Pixabay](https://pixabay.com/?utm_source=link-attribution&utm_medium=referral&utm_campaign=image&utm_content=5078866)

骰子的形象让我想起了我小时候玩的游戏，比如蛇和梯子或者鲁道。

掷骰子时，在任何时间间隔可能出现的数字的可能性是 1-6。六个通常是最好的结果，而一个在大多数情况下可能不是很好。

然而，如果你能比其他数字更多次地掷出数字 6，这不是很棒吗？

这个技巧正是比例采样所实现的，我们将在下一节中进一步研究其逐步实现的细节。但首先，让我们理解这些概念的必要性。

数学在数据科学和机器学习中扮演着重要的角色。

概率统计就是这样一个基本要求。概率涉及预测未来事件发生的可能性，而统计学涉及对过去事件发生频率的分析。我们利用这些概念来处理机器学习和数据科学问题的一个重要方面叫做**采样**。

在统计学、质量保证和调查方法学中，**抽样**是从统计总体中选择个体子集，以估计总体特征。统计学家试图用样本来代表所讨论的人口。

基本上，**抽样**是统计分析中使用的一个过程，其中从一个较大的总体中抽取预定数量的观察值。

与测量整个人口相比，抽样的两个主要优点是成本更低和数据收集更快。

有许多抽样方法，如随机抽样、均匀抽样等。在本文中，我们将主要分析比例抽样的逐步方法。

比例抽样是一种按重量比例选取元素的方法，即对象的重量越大，被选中的机会就越大。

参考前面所述的骰子示例，像五或六这样的较大数字比像一或二这样的较小数字更有可能被选中。

让我们通过一些一步一步的程序实现和一些 python 代码来理解这一点。

![](img/7e85d71f050d9a6e080b8be7f254d985.png)

林赛·亨伍德在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上的照片

# 比例取样的逐步方法；

执行比例采样所需导入的唯一附加库是 python 中的 random 模块。该模块可以按如下方式导入:

```
import random
```

这个模块是唯一的额外要求。剩下的步骤可以用简单的 python 代码来执行。

在我们继续之前，让我们定义我们的输入。作为一个例子，我将使用一个从 1 到 6 的数字列表来表示骰子。你可以随意选择你的问题的任意输入。

```
A = [1,2,3,4,5,6]
```

## 第一步:

> 计算输入的所有元素的总和。
> 
> Sum = A[0] + A[1] + ……。+ A[n]
> 其中，n =已定义输入的最后一个元素的索引

## 代码:

在上面的代码块中，我们将输入列表存储在变量“a”中。我们将继续计算列表中所有元素的总和。我已经使用了上面提到的简单方法，但是如果你觉得舒服的话，你也可以使用一个具有高级功能的更紧凑的方法。

## 第二步:

> 计算输入列表中每个元素的归一化总和。
> 
> A0 ' = A[0]/Sum；a1 ' = A[1]/Sum；………..
> 其中，A’表示每个元素的归一化和

## 代码:

下一个代码块计算分配给输入的每个元素的归一化和。我使用了一个字典变量“C”来相应地存储这些值。

## 第三步:

> 使用所有标准化值计算每个元素的累积和。
> 
> A0 ~ = A0’；A1 ~ = A0 ~+A1 '；…….
> 其中，A~代表每个元素的累积和

## 代码:

上面的代码块使用我们在上一步中找到的标准化值来计算每个元素的累积和。我使用了另一个字典变量‘D ’,它将存储所有需要的累积值。

## 第四步:

> 在(0，1)范围内选择一个随机值。
> 
> r =随机均匀(0.0，1.0)
> 
> 如果 r≤ A0~ =返回 choice-1；
> else 如果 r≤ A1~ =返回 choice-2；
> ………..依此类推，直到覆盖所有元素。

## 代码:

在下一个代码块中，我使用了我们在程序开始时导入的随机模块。使用 0-1 范围内的随机数，我们可以计算加权和出现的概率。

观察代码，尝试直观地理解我们在上面的代码块中试图实现的内容。与较小的数字相比，较大的数字有更大的机会被选中，因为它们之间的桥梁在范围内出现得更高。

如果你对这一步感到困惑，我强烈建议你试着把它写在纸上，并解决这个计算是如何工作的。

现在让我们进入最后一个代码块，并理解最后一步。

## 第五步:

> 最终实现。

## 代码:

上面显示的最后一个代码块用于随机数生成和计数。上面的函数用于运行第一个定义的函数 99 次，并检查每个发生可能性的计数。

期望的输出对于数字 6 具有最高的计数，对于数字 5 具有第二高的计数，依此类推，直到我们对于数字 1 具有最少的计数。

第一次运行的一个输出示例如下所示。

> 注意:由于我们使用的是随机函数，每次输出都会不同。所以如果你得不到和我一样的输出也不用担心。要考虑的重要事情是哪个数字的计数最大。

## 输出:

```
[6, 5, 4, 3, 2, 1]
count of 6 is 30
count of 5 is 24
count of 4 is 20
count of 3 is 11
count of 2 is 10
count of 1 is 4
```

每次运行程序时，输出可能会有所不同。不要求您获得如上所示的相同输出。出现这种情况的原因是因为我们使用了随机模块。需要注意的要点是，与其他数字相比，最大的重量数字具有更高的计数。

这样，我们成功地实现了比例抽样。这个概念在数据科学的许多方面都非常重要，在这些方面，需要给予较大的权重更高的优先级。

骰子的引用就是一个很好的例子。如果我有这个骰子，那么我赢得游戏的机会会高得多！

![](img/7b4c0d1680b2815a518ff6e7acd01070.png)

Sebastien Gabriel 在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上的照片

# 结论:

至此，我们已经到了文章的结尾。我们理解数学对于数据科学的重要性，特别是在使用骰子的例子所需的概率和统计方面。

然后，我们进一步讨论了采样的主题及其在数据科学领域的优势。然后，我们具体探讨了比例抽样技术，一步一步的指南和完整的代码。

如果你对我们今天在这篇文章中提到的比例抽样有任何疑问，请在评论区联系我。

看看我的其他一些文章，你可能会喜欢读！

[](/10-most-popular-programming-languages-for-2020-and-beyond-67c512eeea73) [## 2020 年及以后最受欢迎的 10 种编程语言

### 讨论当今 10 种最流行的编程语言的范围、优缺点

towardsdatascience.com](/10-most-popular-programming-languages-for-2020-and-beyond-67c512eeea73) [](/must-use-built-in-tool-for-debugging-your-python-code-d5f69fecbdbe) [## 必须使用内置工具来调试你的 Python 代码！

### python 调试器模块指南，包含有用的代码和命令。有效且高效地利用这一工具…

towardsdatascience.com](/must-use-built-in-tool-for-debugging-your-python-code-d5f69fecbdbe) [](/5-best-python-project-ideas-with-full-code-snippets-and-useful-links-d9dc2846a0c5) [## 带有完整代码片段和有用链接的 5 个最佳 Python 项目创意！

### 为 Python 和机器学习创建一份令人敬畏的简历的 5 个最佳项目想法的代码片段和示例！

towardsdatascience.com](/5-best-python-project-ideas-with-full-code-snippets-and-useful-links-d9dc2846a0c5) [](/opencv-complete-beginners-guide-to-master-the-basics-of-computer-vision-with-code-4a1cd0c687f9) [## OpenCV:用代码掌握计算机视觉基础的完全初学者指南！

### 包含代码的教程，用于掌握计算机视觉的所有重要概念，以及如何使用 OpenCV 实现它们

towardsdatascience.com](/opencv-complete-beginners-guide-to-master-the-basics-of-computer-vision-with-code-4a1cd0c687f9) 

谢谢你们坚持到最后。我希望你们都喜欢这篇文章。祝大家有美好的一天！