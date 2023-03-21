# 你有软件工程师和数据科学家的技能吗？可能不会

> 原文：<https://towardsdatascience.com/do-you-have-the-software-engineer-and-data-scientist-skills-probably-not-7e8fb069e067?source=collection_archive---------62----------------------->

![](img/3c94f89a715c54c1a00ac3900985bb4d.png)

克里斯蒂娜@ wocintechchat.com 在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上的照片

## 采用技术成为更好的工程师和开发人员

## 为软件工程师和数据科学家开发人员的生产级别做准备

成为一名可靠的软件工程师和数据科学家开发人员，并为生产级编码做准备需要一些技巧。

*   编写干净的模块化代码
*   代码重构
*   编写高效的代码
*   添加有意义的文档
*   测试
*   原木
*   代码审查

这些都是需要培养的基本技能，在实施生产解决方案时会有所帮助。此外，数据科学家经常与软件工程师并肩工作，因此有必要很好地合作。这意味着熟悉标准实践，并能够在代码上与其他人有效协作。

## 干净的模块化代码

当一个数据科学家第一次开始编码时，即使他们已经编码多年，他们也经常努力以一种干净和模块化的方式编写代码。实际上，当在行业中工作时，代码有可能用在生产中。生产代码是在生产服务器上运行的一个软件，用于处理实时用户和目标受众的数据，例如，使用笔记本电脑中的软件产品，如 Microsoft Office、Google 或 Amazon。运行这些服务的代码称为产品代码。理想情况下，生产中使用的代码在公开之前应该满足几个标准，以确保可靠性和效率。首先，代码需要干净。当代码可读、简洁且简单时，它就是干净的。

这里有一个例子，用简单的英语说，是一个不干净的句子。

> 人们会注意到你的裤子被弄脏了，因为你的裤子是粉红色的，看起来像某种果汁的颜色。

这句话多此一举，令人费解。光是读这个就让人应接不暇。这可以改写为:

> 看起来你把草莓汁洒在裤子上了。

这句话达到了同样的效果。尽管如此，这句话还是简洁明了得多。

产品质量代码的特性对于软件开发中的协作和可维护性至关重要。编写干净的代码在行业环境中是非常重要的，因为在团队中工作需要不断地重复工作。这使得其他人更容易理解和重用代码。除了干净之外，代码还应该模块化。事实上，代码在逻辑上被分解成函数和模块。此外，产品质量代码的一个基本特征是使代码更加有组织、高效和可重用。在编程中，模块只是一个文件。类似地，封装代码可以在一个函数中使用，并通过在不同的地方调用该函数来重用它。另一方面，通过将代码封装到可以导入其他文件的文件中，模块允许代码被重用。

为了更好地理解什么是模块化代码，试着把它想象成把衣服放好。我们可以把所有的衣服放在一个容器里，但是要找到任何东西都不容易，可能是因为同一件衬衫或袜子有好几个版本。如果我们有一个放 t 恤的抽屉，一个放衬衫的抽屉，一个放袜子的抽屉，那就更好了。有了这样的设计，告诉别人如何找到合适的衬衫、裤子和袜子就容易多了。写模块化代码也是如此。

将代码分成逻辑函数和模块可以快速找到相关的代码。需要考虑将在不同地方重用的代码片段一般化，以防止编写额外的不必要的代码行。将这些细节抽象成这些函数和模块有助于提高代码的可读性。因此，以一种让团队更容易理解和迭代的方式进行编程对于生产来说是至关重要的。

## 重构代码

很容易忽略编写好的代码。具体来说，当开始为一个新的想法或任务编写代码时，只关注让它工作。通常，在开发的这个阶段，它会变得有点混乱和重复。此外，在代码完成之前，很难知道最好的方法是什么。例如，如果我们没有足够的代码实验来遵循，那么理解什么功能最好地模块化代码中的步骤可能是具有挑战性的。因此，在获得一个工作模型之后，必须回去做一些重构。

代码重构是一个术语，指的是在不改变外部功能的情况下，对代码进行重组，以改善其内部结构。重构允许在生产后清理和模块化代码。从短期来看，这可能是浪费时间，因为我们可能会转向下一个特性。但是，分配时间给重构代码加速时间。从长远来看，团队需要开发代码。持续地重构代码不仅使我们以后更容易回到代码上，而且还允许我们在不同的任务中重用不同的部分，并在这个过程中学习可靠的编程技术。重构代码的实践越多，它就变得越直观。

## 高效代码

在重构过程中，除了使代码清晰和模块化之外，提高代码的效率也是必不可少的。提高代码效率有两个方面:减少代码执行的时间，减少代码占用的空间和内存。两者都会对公司或产品的表现产生重大影响。因此，在生产环境中工作时，实践这一点很重要。

但是，需要注意的是，提高效率有多重要是视情况而定的。缓慢的代码，可能在一种情况下可行，但在另一种情况下不可行。例如，如果某些批处理数据准备流程每三天运行一次，持续几分钟，则可能不需要立即进行优化。另一方面，用于生成在社交媒体 feed 上显示的帖子的代码需要相对较快，因为更新是即时发生的。此外，在代码运行后花大量时间进行重构来清理或优化代码是必不可少的。理解这个过程对开发人员的价值是至关重要的。每次优化代码，我们都会获得新的知识和技能，随着时间的推移，这将使程序员变得更加高效。

## 证明文件

文档是软件代码中附带或嵌入的附加文本或图解信息。文档有助于阐明程序的复杂部分，使代码更容易阅读、导航，并快速传达如何以及为什么使用程序或算法的不同组件。可以在程序的不同级别添加几种类型的文档——首先，使用**行内注释**来阐明代码的行级文档。第二，函数或模块级文档使用 **docstrings** 来描述其用途和细节。最后，**项目级文档**使用各种工具(如自述文件)来记录整个项目的信息以及所有文件如何协同工作。

## 行内注释

在整个代码中，哈希符号后面的文本是行内注释。它们用于解释部分代码，并帮助未来的贡献者理解。评论有不同的使用方式，好的评论，好的评论，甚至用户的评论也有不同。使用注释的一种方式是记录复杂代码的重要步骤，以帮助读者理解。例如，有了函数的指导性注释，未来的贡献者不需要理解代码就能理解函数的功能。注释有助于理解每个代码块的用途，甚至有助于找出单独的代码行或方法。

然而，其他人会认为使用注释有助于证明糟糕的代码或代码需要遵循注释。这是需要重构的迹象。注释对于解释代码不能解释的地方很有价值——例如，为什么一个特定的方法以一种特定的方式实现背后的历史。有时，由于某些未定义的外部变量会导致副作用，可能会使用非常规或看似任意的方法。这些东西很难用代码来解释。这些用于检测图像边缘水平的数字看起来可能是任意的。尽管如此，程序员试验了不同的数字，并意识到这是一个适合这个特定用例的数字。

## 文档字符串

文档字符串或文档字符串是解释代码中任何函数或模块的功能的有价值的文档。理想情况下，代码中的所有函数都应该有文档字符串。文档字符串总是用三重引号括起来。docstring 的第一行是对函数用途的简要说明。如果一行文档足以结束文档字符串，单行文档字符串是完全可以接受的。但是，如果函数足够复杂，需要更长的描述，可以在一行摘要之后添加一个更完整的段落。docstring 的下一个元素是对函数参数的解释。它应该类似于列出参数，陈述它们的目的，并陈述参数应该是什么类型。最后，通常提供函数输出的一些描述。docstring 的每一部分都是可选的。然而，文档字符串是良好编码实践的一部分。它们有助于理解生成的代码。

![](img/3e15562db6cd31bb7f14e7fff02d48ae.png)

如果功能足够复杂，需要更长的描述，在一行摘要之后添加一个更全面的段落。

![](img/3297d1760fc93121fcb0b0591ee235f4.png)

docstring 的下一个元素是对函数参数的解释。我们可以列出参数，说明它们的目的，并说明参数应该是什么类型。最后，通常提供函数输出的一些描述。docstring 的每一部分都是可选的；然而，文档字符串是良好编码实践的一部分。

## 项目文件

项目文档对于让其他人理解代码为何以及如何相关是必不可少的，无论他们是项目的潜在用户还是对代码有贡献的开发人员。项目文档中重要的第一步是一个 **README** 文件。这通常是大多数用户与项目的第一次互动。无论是应用程序还是软件包，项目都应该附带一个 **README** 文件。至少，这应该解释它是做什么的，列出它的依赖项，并提供关于如何使用它的足够详细的说明。对于其他人来说，理解项目的目的必须尽可能的简单，并且快速的得到一些工作。

![](img/8e9a5c7a6f4c798e4c45b74ef5dcafb1.png)

来自 Github 页面的 README.md 文件示例

将所有想法和想法正式翻译到纸上可能有点棘手，但随着时间的推移，它会变得更好，并在帮助他人实现项目的价值方面产生重大影响。编写这些文档也有助于改进代码的设计。这也让以后的投稿人知道如何遵循初衷。

## 测试

在部署之前测试代码是必不可少的。这有助于在产生任何重大影响之前发现错误和错误结论。编写测试是软件工程中的标准实践。但是，测试往往是很多数据科学家刚入行时并不熟悉的做法。事实上，有时科学家提出的洞察数据，本应用于商业决策和公司产品，却是基于未经测试的代码结果。缺乏测试是与数据科学家一起工作的其他软件开发人员的常见抱怨。如果没有测试，由于软件问题，代码中有时会出现执行错误。它也可能根据错误的结论来支配商业决策和影响产品。如今，雇主正在寻找有技能为行业环境正确准备代码的数据科学家，包括测试他们的代码。

当一个软件程序崩溃时，这是非常明显的。出现错误，程序停止运行。然而，在数据科学过程中可能会发生许多问题，这些问题不像导致程序崩溃的功能错误那样容易被发现。所有代码看起来都运行顺利，完全不知道特定的值会被错误地编码。此外，功能被误用，或者意外的数据打破了假设。

这些错误更难发现，因为由于代码的质量，我们必须检查分析的质量和准确性。因此，应用适当的测试以避免意外并对结果有信心是至关重要的。事实上，测试已经被证明有如此多的好处，以至于有一个基于它的完整的开发过程叫做测试驱动 Development⁴.这是一个开发过程，在此过程中，在编写实现任务的代码之前，先编写任务测试。

## 测试驱动开发

测试驱动开发是在编写被测试的代码之前编写测试的过程。这意味着测试一开始会失败，当测试通过时，我们将知道如何完成任务的实现。这种开发代码的方式有许多在软件工程的标准实践中产生的好处。举个简单的例子，我们想写一个函数来检查一个字符串是否是一个有效的电子邮件地址。考虑一些要考虑的因素，比如字符串是否包含“@”符号和句点，并写出一个处理它们的函数，然后在终端中手动测试它。

尝试输入一个有效和一个无效的电子邮件地址，以确保它正常工作。用更多有效和无效的电子邮件地址尝试，其中一个会返回错误的结果。尝试创建一个测试来检查所有不同的 scenarios⁵.，而不是来来回回地做这件事这样，当我们开始实现一个功能时，我们可以运行这个测试来获得关于它是否在所有方面都工作的即时反馈。认为这个过程是一个函数调整。如果测试通过，实现就完成了。

当重构或添加代码时，测试有助于确保代码的其余部分在进行这些更改时不会中断。测试还有助于确保功能是可重复的，不受外部参数的影响，例如硬件和时间。数据科学的测试驱动开发相对较新，出现了许多实验和突破。

## 原木

日志对于理解运行程序时发生的事件很有价值。想象一下，一个模型每天晚上都在运行，第二天早上就在产生可笑的结果。日志消息有助于更好地了解原因、背景，并找出解决问题的方法。由于问题发生时我们并不在现场查看和调试，因此打印出描述性的日志消息来帮助追溯问题和理解代码中发生了什么是非常重要的。

看看几个例子，学习写好日志消息的技巧。

> **提示:专业而清晰**

```
**BAD:** 
Hmmm… this isn’t working???**BAD:** 
idk…. :(**GOOD:** 
Could not parse file.
```

> **提示:要简洁，使用正常大小写**

```
**BAD:** 
Start Product Recommendation Process.**BAD:** 
We have completed the steps necessary and will now proceed with the recommendation process for the records in our product database.**GOOD:** 
Generating product recommendations.
```

> **提示:选择合适的日志级别**

```
**DEBUG**
level you would use for anything that happens in the program.**ERROR**
level to record any error that occurs.**INFO**
level to record all actions that are user-driven or system specific, such as regularly scheduled operations.
```

> **提示:提供任何有用的信息**

```
**BAD:** 
Failed to read location data.**GOOD:** 
Failed to read location data: store_id 8324971.
```

## 代码审查

代码 reviews⁶有益于团队中的每个人，促进最佳编程实践并为生产准备代码。代码审查是工作中常见的 practice⁷，这是有充分理由的。审查彼此的代码有助于发现错误，确保可读性，检查产品级代码是否符合标准，并在团队中共享知识。它们对评审者和团队都是有益的。理想情况下，一个数据科学家的代码由另一个数据科学家审查，因为在数据科学中有特定的错误和标准需要检查——例如，数据泄漏、对功能的误解或不适当的评估方法。

在审查代码时，仔细检查一些问题。

```
**Is the code clean and modular?** * Can I understand the code easily?
* Does it use meaningful names and whitespace?
* Is there duplicated code?
* Can you provide another layer of abstraction?
* Is each function and module necessary?
* Is each function or module too long?**Is the code efficient?**
* Are there loops or other steps we can vectorize?
* Can we use better data structures to optimize any steps?
* Can we shorten the number of calculations needed for any steps?
* Can we use generators or multiprocessing to optimize any steps?**Is documentation effective?** * Are in-line comments concise and meaningful?
* Is there complex code that’s missing documentation?
* Do function use effective docstrings?
* Is the necessary project documentation provided?**Is the code well tested?** * Does the code high test coverage?
* Do tests check for interesting cases?
* Are the tests readable?
* Can the tests be made more efficient?**Is the logging effective?** * Are log messages clear, concise, and professional?
* Do they include all relevant and useful information?
* Do they use the appropriate logging level?
```

关于如何实际编写代码评审的一些提示。

**提示:使用代码棉绒**

这可以节省大量的代码审查时间。Code linter 可以自动检查编码标准。作为一个团队，就一个风格指南达成一致以处理关于代码风格的分歧也是一个好主意，无论这是一个现有的风格指南还是作为一个团队一起创建的。

**提示:解释问题并提出建议**

与其命令人们以特定的方式改变他们的代码，还不如向他们解释当前代码的后果，并由**提出**改进建议。如果他们理解流程并接受建议，而不是听从命令，他们会更容易接受反馈。他们也可能是故意以某种方式做这件事的，把它作为一个建议会促进建设性的讨论，而不是反对。

```
**BAD:** 
Make model evaluation code its own module - too repetitive.**BETTER:** 
Make the model evaluation code its own module. This will simplify models.py to be less repetitive and focus primarily on building models.**GOOD:** 
How about we consider making the model evaluation code its own module? This would simplify models.py to only include code for building models. Organizing these evaluations methods into separate functions would also allow us to reuse them with different models without repeating code.
```

**提示:保持你的评论的客观性**

尽量避免在评论中使用“我”和“你”这样的字眼。避免听起来很私人的评论，将评审的注意力转移到代码上，而不是代码本身。

```
**BAD:** 
I wouldn't groupby genre twice like you did here... Just compute it once and use that for your aggregations.**BAD:** 
You create this groupby dataframe twice here. Just compute it once, save it as groupby_genre and then use that to get your average prices and views.**GOOD:** 
Can we group by genre at the beginning of the function and then save that as a groupby object? We could then reference that object to get the average prices and views without computing groupby twice.
```

**提示:提供代码示例**

当提供代码评审时，节省作者的时间，并通过写出代码建议使他们容易对反馈采取行动。这表明我们愿意花一些额外的时间来审查他们的代码，并帮助他们解决问题。通过代码而不是解释来演示概念可能会快得多。

审查代码的示例:

```
first_names = [] 
last_names = [] for name in enumerate(df.name):     
  first, last = name.split(' ')     
  first_names.append(first)     
  last_names.append(last) df['first_name'] = first_names 
df['last_names'] = last_names**BAD:** 
You can do this all in one step by using the pandas str.split method.**GOOD:** 
We can actually simplify this step to the line below using the pandas str.split method.df['first_name'], df['last_name'] = df['name'].str.split(' ', 1).str
```

## 参考

[PEP 257 — Docstring 约定](https://www.python.org/dev/peps/pep-0257/)
[bootstrap github](https://github.com/twbs/bootstrap)
[ned batchelder](https://speakerdeck.com/pycon2014/getting-started-testing-by-ned-batchelder)
⁴[数据科学出错的四种方式以及测试驱动的数据分析如何帮助](https://www.predictiveanalyticsworld.com/machinelearningtimes/four-ways-data-science-goes-wrong-and-how-test-driven-data-analysis-can-help/6947/)
⁵ [集成测试](https://www.fullstackpython.com/integration-testing.html)
⁶ [代码评审指南](https://github.com/lyst/MakingLyst/tree/master/code-reviews)
⁷ [代码评审最佳实践](https://www.kevinlondon.com/2015/05/05/code-review-best-practices.html)

*免责声明:本文基于 Python 编程语言。也就是说，代码和文档样本将使用 Python 作为参考。*