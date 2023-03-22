# 清理、重构和模块化:提高 Python 代码和职业生涯的“必备”基础

> 原文：<https://towardsdatascience.com/cleaning-refactoring-and-modular-the-must-foundations-to-improve-your-python-code-and-carrer-65ef71cdb264?source=collection_archive---------16----------------------->

## 一些代码习惯如何将您的开发过程和职业生涯带到一个全新的水平

![](img/11d1889351db184828673ee77a3a42d7.png)

资源: [Jonatan Pie at Unplash](https://unsplash.com/photos/3l3RwQdHRHg)

> “任何傻瓜都能写出计算机能理解的代码。优秀的程序员会写出人类能理解的代码。”
> 
> **马丁·福勒**

我们都知道，在开发过程的高峰期，我们通常更关注于让我们的程序工作，而不是让它完全可读。当我们面临一个前所未有的问题，并且有一个紧迫的期限来交付工作时，这种情况会变得更加复杂。有时，我们不得不求助于堆栈溢出来寻找解决方案，或者花些时间阅读所有类似的问题，直到找到解决问题的新思路。

> "当你的程序一塌糊涂，但它做了它的工作."

![](img/c27c506a4fa1049be4dbae4e5dab6262.png)

来源:[Tumblr 上的凯凯](https://k-eke.tumblr.com/post/190861786446/when-your-program-is-a-complet-mess-but-it-does)

我经常使用堆栈溢出，但至少对我来说，我花在寻找解决方案上的时间减少了我通常用来使代码更干净和可读的时间(一个艰难而漫长的时间)。如果这种情况也发生在你身上，也许是因为我们更关注“如何解决”而不是让其他开发人员可以使用它，嗯… **我们想让它工作！尽管我们有时间格式化和记录一些东西，但是谁从来没有花几个小时去理解一些多年前的代码呢？原因很简单，编写干净、可读的代码是一件艰苦而累人的事情，但我们应该经常思考伟大的“鲍勃叔叔”的下面这句话:**

> “花在阅读和写作上的时间比远远超过 10 比 1。作为编写新代码工作的一部分，我们不断地阅读旧代码。…[因此，]让它易于阅读会让它更易于书写。”
> 
> **罗伯特·c·马丁，** [**干净的代码:敏捷软件工艺手册**](https://www.goodreads.com/work/quotes/3779106)

我们必须放在心里，作为开发者、程序员、软件工程师、数据科学家等等，**我们真正的受众不是计算机，而是其他程序员(包括我们自己)**。正如鲍勃叔叔的句子所定义的那样，我们通常花更多的时间阅读文档或其他人的代码，而不是制作新的代码，那么为什么不在这部分花更多的时间(不管有多累)并在未来帮助你或其他人呢？

![](img/12adc00031cc125cb35b6c7337ed83f5.png)

来源:[Thom Holwerd](https://www.osnews.com/story/author/thom-holwerda/)[a at OS news 漫画](https://www.osnews.com/story/19266/wtfsm/)

这不仅会让你成为一个更好的程序员，而且还会帮助你的产品的可伸缩性和可维护性，同时减少错误的数量([这是真的](https://www.wired.com/2005/11/historys-worst-software-bugs/))和系统复杂性/变更或增加的风险。如果这些对你来说还不够，我可以再给你一个改变旧习惯的想法！

> “编写代码的时候，要把最终维护你代码的人想象成一个知道你住哪儿的暴力精神病患者。”
> 
> 约翰·伍兹

那么让我们看看如何做这件事的一些方法？下面是实现高质量和干净代码的一些方法的总结。

# 重构

![](img/68fe335a0d8148b7369396762cc46821.png)

来源:[XKCD 的兰道尔·门罗](https://xkcd.com/378/)

重构是一种在不改变外部功能的情况下，重构代码以改善其内部结构的方法。这背后的心态是:你设法让它工作了吗？回到开头，把你的程序清晰化，模块化！当您有几个要添加的功能时，一开始就这样做似乎是浪费时间，但是一步一步地这样做将给您带来以下好处:

*   长期减少工作量；
*   更容易维护代码；
*   增加可重用性；
*   减少在未来或新项目中这样做的时间(做得越多，你在这项活动中就会变得越快)
*   如果你尝试做得比之前的重构更好，你一定会很快掌握这项技能；
*   这项技能在就业市场上非常有价值，会突出你的个人资料(看看 LinkedIn 或其他网站上的“渴望拥有”的职位就知道了)

好吧！我理解重构的优势，但是我该怎么做呢？很简单，以下是实现这一点的一些方法:

*   首先，试着理解[代码复杂性在 Python](https://www.datacamp.com/community/tutorials/analyzing-complexity-code-python) 中是如何工作的，以及度量这种复杂性的度量标准(例如，代码行数、[圈复杂度](https://audiolion.github.io/python/2016/10/17/reducing-cyclomatic-complexity.html)、 [Halstead 度量标准](https://www.geeksforgeeks.org/software-engineering-halsteads-software-metrics/)、[可维护性指数](http://www.projectcodemeter.com/cost_estimation/help/GL_maintainability.htm))；
*   知道了复杂性是如何工作的，你现在要做的最多的事情就是重命名名字、模块、函数、类、方法，并检查你是否在你的代码中应用了一些[过程编程](https://en.wikipedia.org/wiki/Procedural_programming)；
*   最后，检查一些[复杂性反模式](https://deepsource.io/blog/8-new-python-antipatterns/) …瞧，你的程序将会大放异彩！

如果你想更深入地了解这个主题，[这里有一篇优秀的文章](https://realpython.com/python-refactoring/)，它一步一步地解释了如何在 Python 中实现重构，这里有一些可用的代码指标的汇编！如果你喜欢书，我建议如下:

*   [*Python 反模式*](https://pythonizame.s3.amazonaws.com/media/Book/python-anti-patterns/file/18a14302-7c50-11e7-ba9c-040196293901.pdf)*(AWS)；*
*   [*重构:改进现有代码的设计*](https://www.amazon.com.br/Refactoring-Improving-Design-Existing-Code/dp/0201485672) *(马丁·福勒)；*

一些不错的视频讲座或研讨会:

*   [*用老谋深算*](https://stribny.name/blog/2019/05/measuring-python-code-complexity-with-wily) *(PyCon 2019)测量 Python 代码复杂度；*
*   [*重构 Python:为什么以及如何重构你的代码*](https://www.youtube.com/watch?v=D_6ybDcU5gc) *(PyCon 2016)。*

# 干净的代码

![](img/2524bd5f688856a8ec277238058d5db5.png)

来源:[迷因生成器](https://memegenerator.net/instance/39435171/uncle-bob-something-smells-its-your-code)

“干净的代码”不是一种方法或一套规则，而是一种哲学，它带来了一些技术，简化了代码的编写和阅读。再次引用鲍勃叔叔的话:

> “干净的代码不是按照一套规则编写的。通过学习一系列启发法，你不会成为一名软件工匠。专业精神和工匠精神来自推动学科发展的价值观。”
> 
> **罗伯特·c·马丁，** [**干净的代码:敏捷软件工艺手册**](https://www.goodreads.com/work/quotes/3779106)

你知道前面提到的想法吗？我们有很紧的时间来执行这项任务，我们把很多注意力放在结果上，而不是可读和干净的代码。他很清楚地说明了如何解决这个问题，编写糟糕代码的错误完全取决于编写代码的人。

> “没有什么比糟糕的代码对开发项目的影响更深远、更长期了。糟糕的计划可以重做，糟糕的需求可以重新定义。不好的团队动态是可以修复的。但是糟糕的代码会腐烂发酵，成为拖累团队的不可阻挡的重量。”
> 
> **罗伯特·c·马丁，** [**干净代码:敏捷软件工艺手册**](https://www.goodreads.com/work/quotes/3779106)

那我们怎么解决这个问题？取决于你在哪里(你在路上用坏习惯多久？)，这可能很容易，也可能很难，消除坏习惯很复杂，但不要放弃，只要使用以下步骤，随着时间的推移，你会找到窍门的！

*   ***一段干净的代码应该是优雅的*** *到令人愉悦的程度；*
*   ***一个干净的代码必须是描述性的、隐含的类型，*** *例如，对布尔使用“is_”或“has_”来明确它是一个条件；*
*   ***一个干净的代码必须一致但区分清楚，*** *例如，“age_list”和“age”比“ages”和“age”更容易区分；*
*   ***干净的代码必须避免缩写，尤其是单个字母，*** *只对计数器和常见的数学变量使用缩写，但请记住，如果您的团队有不同的角色(例如，全栈工程师与数据科学家一起工作)，可能有必要提供更具描述性的名称；*
*   ***干净的代码必须表明长名称不同于描述性名称*** *，仅具有相关信息的描述性名称；*
*   ***一个干净的代码必须有 79 个字符左右的行，***[**学会换行**](https://www.interviewqs.com/ddi_code_snippets/break_long_line_python)**[*缩进*](https://www.dummies.com/programming/python/how-to-indent-and-dedent-your-python-code/) *一行和/或多行；***
*   *****一个干净的代码一定是有据可查的，*** *在我看来，* [*Google 风格的例子*](https://sphinxcontrib-napoleon.readthedocs.io/en/latest/example_google.html) *是最完整的，但是你可以找到你最喜欢的，开始使用它；***
*   *****一个干净的代码必须恰当地使用空格，*** *用一致的缩进组织你的代码，用空行分隔各个部分；***
*   *****一段干净的代码必须遵循*** [***PEP 8 准则***](https://www.python.org/dev/peps/pep-0008/?#code-lay-out) ***进行代码布局；*****
*   *****一个干净的代码必须遵循*** [***法则，对于 OOP 来说***](https://tech.jonathangardner.net/wiki/Law_of_Demeter) ***。*****

**幸运的是，一些工具可以帮助我们保持代码的整洁！我们可以使用 Linters 来分析它，并检测各种类型的“ [lint](https://en.wikipedia.org/wiki/Lint_(software)) ”，然后能够分析代码错误、危险的代码模式、代码风格和潜在的意外结果。[这里是 Python 可用的 Linters 的汇编](https://books.agiliq.com/projects/essential-python-tools/en/latest/linters.html)以及每一个与其他的不同之处。如果你想深入这个话题，我推荐这几本书:**

*   ***【必备】* [*干净的代码:敏捷软件工艺手册*](https://www.amazon.com.br/Clean-Code-Handbook-Software-Craftsmanship-ebook/dp/B001GSTOAM) *(罗伯特·c·马丁&迪安·万普勒)；***
*   **[*有效 Python: 59 种具体方法写出更好的 Python*](https://www.amazon.com.br/Effective-Python-Specific-Write-Better/dp/0134034287)*(Breet Slatkin)；***

**如果你喜欢视频和研讨会，我建议你:**

*   **[*干净代码—鲍勃大叔*](https://www.youtube.com/watch?v=7EmboKQH8lM)**
*   **[*清理 Python 中的代码*](https://www.youtube.com/watch?v=n_Y-_7R2KsY)*(PyCon CZ)；***
*   **[*将代码转换成漂亮、惯用的 Python*](https://www.youtube.com/watch?v=OSGv2VnC0go)*(PyCon 2013)***

# **模块化程序设计**

**![](img/d27ea1ec18931f2976a86be26f5d4e5a.png)**

**来源:[疯狂世界的 Manu Cornet](https://bonkersworld.net/building-software)**

**编写模块化代码是软件开发中的一个重要步骤，因为它允许在模块中使用相同的代码，通过引用它来在程序的不同位置执行特定的操作。这种方法方便了大型程序的调试，增加了代码的可重用性和可读性，提高了可靠性，也有助于与多个开发人员或团队一起编程。为了继续，我认为您已经知道如何构建 Python 项目，但是如果您不知道，请在继续之前看一下这里。总之，要制作模块化代码，必须遵循以下提示:**

*   *****不要重复自己:*** *概括和巩固函数或循环中重复的代码，不惜一切代价避免* [*意大利面代码*](https://en.wikipedia.org/wiki/Spaghetti_code)*；***
*   *****抽象出逻辑来提高可读性:*** *这用描述性函数名来提高可读性，但是要小心使用，因为你可能会过度设计；***
*   *****尽量减少实体数量:*** *用函数调用代替内联逻辑是有利弊的；***
*   *****函数应该做一件事:*** *如果你的函数名中有一个“and”，考虑重构，你的函数也必须有少于 10 行的代码；***
*   *****任意变量名在某些函数中可以更有效:*** *一般函数中的任意变量名其实可以让代码可读性更强；***
*   *****每个函数尽量少用三个参数:*** *记住我们模块化是为了简化我们的代码，让它工作起来更有效率。如果您的函数有很多参数，您可能需要重新考虑如何将其拆分。***

**我建议你看一看[的一篇优秀文章就是这篇](https://www.datacamp.com/community/tutorials/modules-in-python?utm_source=adwords_ppc&utm_campaignid=10267161064&utm_adgroupid=102842301792&utm_device=c&utm_keyword=&utm_matchtype=b&utm_network=g&utm_adpostion=&utm_creative=332602034364&utm_targetid=dsa-429603003980&utm_loc_interest_ms=&utm_loc_physical_ms=1001622&gclid=EAIaIQobChMI8f_Eh5LX6gIVjIiRCh0CTw_yEAAYASAAEgIst_D_BwE)，我认为必须看的一本书是《Python 的搭便车者指南》，可以在这里免费获得[。](https://docs.python-guide.org/)**

# **现在呢？**

**写好代码的问题，尽管在几个 IT 领域都存在，而且主要是在新手中，但在数据科学领域已经讨论了很多。这在很大程度上是由于数据科学家接触了广泛的学术学科，导致他们在编写干净和高级代码所需的一些技能方面缺乏经验(例如，软件工程原理、范例、干净代码、测试、日志)。尽管有这个问题，我们仍然有时间来解决这个问题，让我们帮助我们和其他人实现新的代码习惯！**

**我希望这篇文章能为那些不仅想改进代码，还想一起利用团队的人提供指导！**

**![](img/3c8e5c5ef7459e6d10f1c17e36ea6f32.png)**