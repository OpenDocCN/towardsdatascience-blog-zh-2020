# 1 个破坏你生产力的错误

> 原文：<https://towardsdatascience.com/1-mistake-ruining-your-productivity-ae6e52ef2693?source=collection_archive---------53----------------------->

## 改变这一习惯，在进行下一次分析时提高你的效率。

![](img/413d679ec55728c863c63bbdb4598574.png)

照片来自[约书亚·科尔曼](https://unsplash.com/@joshstyle)在 [Unsplash](https://unsplash.com/)

完成分析后，不要乱扔笔记本。当你开始下一个项目时，它会毁了你的工作效率。当我从事不同的数据科学项目时，我回头看我创建的笔记本，并想，“这些代码可以重用吗？”答案几乎总是肯定的！开发一个可重用的代码库将允许你运行重复的分析，并且当你与他人分享你的代码时，使它更具可读性。

# 什么是代码重用？

代码重用是将现有的代码或笔记本用于新的功能或应用。正如《T4 系统编程》一书中所说的，代码重用有很多好处，它允许更好的代码可读性、更好的代码结构和更少的测试。

保留旧笔记本有助于代码重用。您可以开始看到您正在重复的代码模式，并使用它们来开发函数或类。这种重构工作将帮助您在避免重复的同时改进代码。您将拥有更多可维护的功能，这些功能在将来需要时更容易更新。当您创建这些函数时，您可以开始合并单元测试来验证您的函数是否正常工作。单元测试是一项有价值的工作，通过向您展示您的功能是否产生了预期的结果，有助于避免将来出现问题。

# 为什么要实践代码重用？

通过创建函数/类在以后的项目中使用的代码重用是一种有价值的技术，它将帮助您在运行您的分析时变得更有效率并节省时间。正如 Matlab 关于代码重用的文章中所讨论的，代码的模块化也将使多个个体能够容易地使用相同的功能。如果您决定为您的数据科学项目创建您的软件库，您会有许多代码重用的例子。这个库将包含许多函数和类，负责数据科学工作的不同方面。

学习使用可重用和面向对象的代码也将允许您使用自定义实现来实现新的行为。例如，您可能会发现自己使用的数学库不包含您需要的函数或以特定方式行动的能力。学习重用代码和用 OOP 编写将允许你扩展库的功能，使之适合你的用例。

Thomas Huijskens 提出了一个很好的观点，即你的代码应该可以投入生产。当您想要重新运行一个分析时，学习创建函数和清理代码对您也是有价值的。如果你开发了一个有用的分析，你会想把它展示给不同的管理层或者推动一个业务单元的行动。在这种情况下，您的代码应该易于重新运行和维护。在函数中清理代码并使其可读将使您在下次需要分析时更容易重新运行和重新创建结果。您可能会发现，随着您继续开发，您的分析和可视化会推广到其他团队或客户。编写函数将有助于使您的分析可重复，代码可读。

# 最后的想法

保留您的旧笔记本并开发可重用的代码将有助于提高您在数据科学方面的生产力。创建一个可重用的代码库将允许您运行重复的分析，并在与他人共享您的代码时使其更具可读性。您的代码应该是生产就绪的，这样任何人都可以从您停止的地方开始，重新运行分析，并理解代码。当您开始下一个项目时，考虑使用函数，编写清晰的文档，并找到高重用性的领域。

# 附加阅读

*   Richard John Anthony，在[系统编程](https://www.sciencedirect.com/book/9780128007297/systems-programming)，2016 第 7.5.4 章机会出现时重用代码
*   Matlab [什么是代码重用？](https://www.mathworks.com/help/rtw/ug/what-is-code-reuse-58d9ced3ba27.html)
*   Arho Huttunen 对代码重用的误解
*   对于数据科学家来说，唯一有用的代码是 Thomas Huijskens 的生产代码

如果你想阅读更多，看看我下面的其他文章吧！

[](/stop-wasting-your-time-and-consult-a-subject-matter-expert-f6ee9bffd0fe) [## 停止浪费你的时间，咨询一个主题专家

### 在从事数据科学项目时，请一位主题专家来审查您的工作可能会有所帮助。

towardsdatascience.com](/stop-wasting-your-time-and-consult-a-subject-matter-expert-f6ee9bffd0fe) [](/top-3-books-for-every-data-science-engineer-e1180ab041f1) [## 每位数据科学工程师的前三本书

### 我放在书架上的伟大资源，我喜欢介绍给软件工程师和数据科学家。

towardsdatascience.com](/top-3-books-for-every-data-science-engineer-e1180ab041f1) [](/do-we-need-object-orientated-programming-in-data-science-b4a7c431644f) [## 在数据科学中我们需要面向对象编程吗？

### 让我们讨论一下作为一名数据科学家转向面向对象编程的利弊。

towardsdatascience.com](/do-we-need-object-orientated-programming-in-data-science-b4a7c431644f) [](/keys-to-success-when-adopting-a-pre-existing-data-science-project-9f1225fb0275) [## 采用现有数据科学项目的成功关键

### 代码本来可能不是你的，但现在是你的了。那么接下来呢？

towardsdatascience.com](/keys-to-success-when-adopting-a-pre-existing-data-science-project-9f1225fb0275) [](https://medium.com/the-innovation/top-7-lessons-learned-from-a-year-of-meetings-cbf419910649) [## 从一年的会议中吸取的 7 大教训

### 日历上有如此多的会议，有时 it 会觉得它们没有达到应有的效果。

medium.com](https://medium.com/the-innovation/top-7-lessons-learned-from-a-year-of-meetings-cbf419910649)