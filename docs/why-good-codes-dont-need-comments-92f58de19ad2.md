# 为什么好的代码不需要注释

> 原文：<https://towardsdatascience.com/why-good-codes-dont-need-comments-92f58de19ad2?source=collection_archive---------23----------------------->

## 糟糕的代码到处都是

![](img/3d35373becd62a6bc383dc966bab255e.png)

在 [Unsplash](https://unsplash.com/?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) 上由 [Max Duzij](https://unsplash.com/@max_duz?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) 拍照

我强烈建议您仔细看看下面两段代码

你喜欢哪一个？乍看之下，前者似乎做了很好的工作，清楚地描述了它的每一个操作。然而，这些指令是完全多余和过时的。它们只会给读者增加另一层困惑。*糟糕的评论是命名不当的变量和编程技巧不足的借口。*

如果是你写的，下面是你的同事在评估上面的代码时会想到的

*   他为什么要大声说出来？他认为他的代码阅读器是弱智吗？
*   *这行评论有什么意义？有什么隐藏的意思吗？*
*   为什么他仍然用单个字符作为变量的名字？人们在哪一年练习这个？

你不希望你的同事不知所措，试图通过你的评论来解释你的意图。通过解释程序中的所有小细节，你认为你是在帮他们的忙，但他们所理解的恰恰相反。

![](img/2e9259da3585cd6ced36aac505ea4059.png)

布鲁斯·马尔斯在 [Unsplash](https://unsplash.com/?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) 上拍摄的照片

现在，再仔细看看第二段代码。你觉得代码本身就能说明问题吗？不需要解释。

每个程序员都能写出机器能理解的代码，一个好的程序员写出人类能理解的代码。好的代码不需要注释，这里有五个原因。

# **不必要的评论会让人分心**

写程序的时候，你应该专注于算法、数据结构、性能，不要写那些没有必要的完美注释。

您的代码片段是一个流程。它从变量声明到函数定义，最后是核心执行。不要用除了代码本身没有任何其他解释的注释来打断这个流程。

> 好的代码有节奏，而平庸的代码有很多停顿

当其他程序员阅读你的作品时，他们倾向于将他们的思想插入到你的流程中。持续被多余的评论打断会让人分心。

注释应该在两个代码流之间建立联系，而不是将单个代码流分割成碎片。好的评论称赞你的代码，而不好的评论会让读者偏离主题。

在发表任何评论之前，问问你自己它是否有任何作用，而不是分散人们对你的程序的理解。好的代码有节奏，而平庸的代码有很多停顿。

# **冲动的评论根源于源代码**

代码库是人们合作的地方。它有许多版本，每个程序员都有平等的机会贡献他们的工作。你不想在公开的源代码中留下一个冲动的评论。它可能会被遗忘，被深埋在主要组件中。

一条不好的评论会让你的同事甚至未来的你变得难以理解和困惑。

想象一下三年前有人留下了这样的评论

你有足够的信心删除那个评论吗？已经生产三年了。你的作品已经一团糟了，这个评论会一直在那里。它是永久的，永恒的，永恒的。

注释将植根于您的源代码。把它放在那里的程序员可能没有恶意，他只是忘记了以后需要做什么。三年后，由于他已经离开了公司，没有人会知道最初的动机是什么。

帮自己和他人一个忙，不要在代码中留下那样的注释。它笨拙、草率，而且不切实际。万一你还不确定该怎么做，不要着急。

从短期来看，补丁可以及时投入生产，但从长远来看，*它把代码库变成了私人笔记。*

# **通过你的代码交流，而不是你的评论**

听起来很令人惊讶，你的程序有两个不同的通信目的:一个是与机器通信，另一个是与人类通信。机器完全理解你的指令，但对人类来说并非如此。没有面对面或直接的交谈，误解随时都可能发生。

你必须了解你的代码的受众。他们不是五年级学生。他们是你的同龄人，你的同事，或者是工作时坐在你旁边的人。他们不需要知道你为什么选择这样命名你的变量的每一个微小的原因，或者你如何创建这个程序的两页的故事。

![](img/43716c2b7de43e74c48a9aed4c0630cb.png)

照片由[米米·蒂安](https://unsplash.com/@mimithian?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)在 [Unsplash](https://unsplash.com/?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) 上拍摄

你应该开门见山。他们只想知道你的程序是如何工作的，如何使用它。直接、清晰、简洁。良好的沟通是不需要沟通的。

# **写评论是拖延的一种形式**

这就是我，经过一个小时的编码

这是我，在摆脱写不好的评论之后

人类的大脑天生懒惰。它倾向于用最不吓人的方式来完成任务。

开始编写程序最令人畏惧的事情是设置最初的几行。写一个长而详细的注释来解释算法是如何工作的，可能会给你的大脑一个信号，表明你正在实现某个目标。在你的评论中形成和选择正确的词语是浪费时间。

**完美主义扼杀生产力。你的第一个程序可能会出现混乱，但这完全没关系。没有人会阅读你的代码初稿。**

您可能想要多次迭代以产生最终版本。你花时间写世界上最好的评论是在欺骗自己。你以为你在工作，但事实上，你在拖延。

注释是开始编程的一种反生产力的方式。
先打草稿，后编辑。

# **差评误导读者**

我们谈到了分心，有时甚至更糟。有些评论比其他评论更有害。

你的代码审查员不知道如何读心。他们可以自由理解代码，这可能与你的不同。

当你和你的合作者不在同一个页面上时，程序会有一个大的转折。它可能被用来做一些最初没有被设计的事情。

当交流有问题时，一个不好的评论就会导致误解。结果可能会破坏当前版本，要求临时修复，或者重写整个逻辑。

*没有人理解没有人有不好的评论。*只有代码才会发光，这才是你应该专注的事情。

# 额外收获:真实存在的有趣评论

我个人认为源代码中最有趣的注释

```
/*
 * Dear Maintainer
 *
 * Once you are done trying to ‘optimize’ this routine,
 * and you have realized what a terrible mistake that was,
 * please increment the following counter as a warning
 * to the next guy.
 *
 * total_hours_wasted_here = 73
 *
 * undeclared variable, error on line 0
 *
 */
```

```
Exception up = new Exception("Something is really wrong.");
throw up;  //ha ha
```

```
// I dedicate all this code, all my work, to my wife, Darlene, who will 
// have to support me and our three children and the dog once it gets 
// released into the public.
```

```
// drunk, fix later
```

```
// Magic. Do not touch.
```

```
// They made me write it, against my will.
```

```
public boolean isDirty() {
    //Why do you always go out and
    return dirty;
}
```

你可以点击查看完整列表

# 结论

不用说，我已经表明了避免多余评论的观点。争取简洁、紧凑和直截了当的编程。编码快乐！