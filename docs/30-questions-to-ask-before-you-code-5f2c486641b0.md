# 编码前要问的 30 个问题

> 原文：<https://towardsdatascience.com/30-questions-to-ask-before-you-code-5f2c486641b0?source=collection_archive---------31----------------------->

## [入门](https://towardsdatascience.com/tagged/getting-started)

## 在为我的下一个数据科学工程项目编码之前，我会问这些问题。

![](img/af704b942334029894662e1347925be4.png)

照片来自[像素](https://www.pexels.com/)上的[棉花兄弟](https://www.pexels.com/@cottonbro)

我是一名软件工程师和数据科学家，在笔记本和软件包中编写代码。编码时最难学的一课是"**在写代码之前先停下来想一想。投入到编码中感觉很棒，但是你可能会错过更大的画面或者基于当前假设的项目的一个重要方面。软件工程和数据科学可以带来令人兴奋的项目组合，包括数据清理、自动化、开发运维、分析开发、工具等等。一些项目可以很容易地跨越几个不同的领域。从最初的工作中退一步，开始思考手头的问题，这是很有价值的。至此，我想向你介绍我在着手一个新项目之前考虑的 30 个常见问题。**

# 用例:过去、现在和未来

我想考虑的第一个领域是用例。当我着手一个项目时，我可能只知道最初的用例，但是在与其他涉众、客户或团队成员交谈后，可能会有更多的用例。

```
1\. Sit down and think about your current use case. How would you design the code for this use case? 
2\. If there are past use cases you can examine, would you design the code differently to accommodate those?
3\. Are there potential future use cases that may differ from your past and present cases? How would these change the way you develop your code? 
4\. Thinking about your code structure, sit down, and discuss it with one or more other developers. Would they approach the problem differently? Why?
5\. As you develop your code, consider how it can expand in the future. Would that be an easy feat, or will it be hard to accomplish? How can you make it more reusable or repeatable?
```

# 数据采集和清理

在理解了项目可能的用例之后，下一个要考虑的领域是数据获取和清理。根据您使用的数据，您可能需要考虑如何在工作中吸收、清理和利用这些数据。

```
6\. What data is required for this project to be successful? 
7\. Do you need more than one dataset that requires some aggregation, or will you utilize one dataset? Do you need to get this data yourself? If so, how will you get this data? 
8\. Will your code handle the I/O in your Python package or within a separate notebook that runs the code? 
9\. Will you interface with another team that already gives you access through a database or API? 
10\. Are there any processes you need to develop around the acquisition or cleaning this data that will ease the process? 
11\. What format is your data? Does this format matter to your code?
```

# 自动化、测试和 CI/CD 管道

在理解了我的项目的用例以及你需要什么数据之后，我想回答的下一个问题是关于过程的自动化。我喜欢尽我所能实现自动化，因为这可以让我专注于其他工作。考虑使用可以根据需要运行的快速脚本来自动化代码的不同部分，创建按计划运行的作业，或者利用 CI/CD 管道来执行部署等日常任务。

```
12\. Should the output of the work get created regularly? Will, you ever need to repeat your analysis or output generation?
13\. Does the output get used in a nightly job, a dashboard, or frequent report? 
14\. How often should the output be produced? Daily? Weekly? Monthly? 
15\. Would anyone else need to reproduce your results?
16\. Now that you know your code structure, does it live in a notebook, standalone script, or within a Python package? 
17\. Does your code need to be unit tested for stability?
18\. If you need unit testing for your Python package, will you set up a CI/CD pipeline to run automated testing of the code? 
19\. How can a CI/CD pipeline help ensure your work is stable and doing as expected? 
20\. If you are creating an analytic, do you need to develop metrics around the analytics to prove they produce the expected results?
21\. Can any aspect of this work be automated?
```

# 可重用性和可读性

我关注的最后一组问题集中在代码的可重用性和可读性上。你的团队中会不会有新人捡起你的代码，学习它，并快速使用它？

我喜欢写文档，这意味着我的代码通常会被很好地注释，并附有如何运行它的例子。当引入新的开发者时，添加文档、例子和教程是非常有用的，因为他们可以很快加入进来，并感觉他们正在很快做出贡献。没有人喜欢花很长时间去理解事情，并感觉自己在为团队做贡献。

就可重用性而言，这是您的用例可以派上用场的地方。如何在你的用例中使用你的代码，但又足够一般化，让其他人从你停止的地方继续，并在他们的用例中使用它。

```
22\. You have looked at your use cases. Can you standardize the classes or methods to fit the code's expansion? 
23\. Is it possible to create a standardized library for your work?
24\. Can you expand your work to provide a utility or tool to others related to this work?
25\. As you write code, is it clear to others what the code is doing? 
26\. Are you providing enough documentation to quickly onboard a new data scientist or software developer?
```

# 代码审查

最后，还有代码审查。在开始编码之前考虑代码评审似乎有点奇怪，但是让我们后退一步。在你的评估或经常性的讨论中，你会遇到一些常见的问题吗？思考这些案例，并在开发代码时利用它们。从过去关于代码实现的评论中学习，并不断发展您的技能。

```
27\. Do you need to have a code review for your project? 
28\. If so, what comments do you anticipate getting on your work? 
29\. Who can you meet with before the code review to discuss architecture or design decisions? 
30\. What comments are you required to address, and what comments can you use for later investigation?
```

# 摘要

总的来说，这听起来可能很多，但这 30 个问题是思考如何设计下一个数据科学工程项目的好方法。当你回顾过去时，你在开始一个新项目之前会考虑哪些事情？我关注五个方面:

*   确定此项目工作存在哪些用例。
*   了解所需的数据采集和清理。
*   研究在您的流程中添加自动化、测试和 CI/CD 管道的可能性。
*   努力使您的代码可读和可重用，以便快速加入新的团队成员，并使代码在以后的阶段易于扩展。
*   利用代码评审的优势，并从中学习。