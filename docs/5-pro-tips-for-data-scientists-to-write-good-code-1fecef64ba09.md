# 给数据科学家写好代码的 5 条建议

> 原文：<https://towardsdatascience.com/5-pro-tips-for-data-scientists-to-write-good-code-1fecef64ba09?source=collection_archive---------55----------------------->

![](img/fb855009c7258d8608b682a6b93e80f9.png)

luis gomes 摄于 Pexels

## 以下建议是根据我从学术界到作为数据科学家在不同数据和工程团队的各种项目中工作的经历而提出的。许多数据科学家(包括我自己)没有计算机科学或软件开发背景，因此可能没有受过正规培训或没有良好的代码编写习惯。这些技巧应该有助于新的或有抱负的数据科学家合作编写好的代码，并以更容易生产的方式构建模型。

## 使用版本控制

这对协作和备份都很重要。它允许我们跟踪项目在开发过程中的变化，对于协调任务和鼓励尽职调查非常有用。Git 是一个强大的版本控制软件，能够分支开发部分，跟踪和提交变更，从远程存储库推送和获取，并在必要时合并代码片段以克服冲突([https://git-scm.com/](https://git-scm.com))。

更多细节请见我的幻灯片“[版本控制& Git](https://www.slideshare.net/JasonByrne6/version-control-using-git-124782046)

## 让它具有可读性

协作编码的一个关键组成部分是将它交给其他开发人员审查和使用的能力，这意味着它必须是可读的。这包括使用适当的变量名和函数名，并在必要时添加解释性注释，以及定期包含介绍代码片段及其细节的文档字符串。遵循你正在使用的语言的相关风格指南也很重要，例如 Python 中的 PEP-8(【https://www.python.org/dev/peps/pep-0008/】T4)。

## 保持模块化

编写代码时，保持模块化很重要([https://en.wikipedia.org/wiki/Modular_programming](https://t.umblr.com/redirect?z=https%3A%2F%2Fen.wikipedia.org%2Fwiki%2FModular_programming&t=NzQ3MmY0OTMyNzI3NDMxYzM3YmFmYTMyZDNlZmQxNGYwODYwNTkxNixJSzdGMFJwVQ%3D%3D&b=t%3AyOP1_23-DRFIe3DPTeaQHA&p=https%3A%2F%2Fdatascienceunicorn.tumblr.com%2Fpost%2F171341668221%2Ftips-for-writing-good-code&m=1))。也就是说，将它分解成更小的部分，作为整个算法的一部分执行单独的任务。这种级别的功能使您可以轻松:

1.  控制变量的范围，
2.  重用代码模块，
3.  在进一步开发过程中重构代码，
4.  阅读、审查和测试代码。

## 编写测试

试着考虑什么样的测试可以和你的代码一起编写，以便检查你的假设和逻辑的有效性。这些测试可以是任何东西，从模拟预期的输入和输出，到检查代码功能的一系列单元测试。单元测试通常以可重复的方式测试最小可能代码单元(可以是方法、类或组件)的功能。例如，如果您正在对一个类进行单元测试，您的测试可能会检查该类是否处于正确的状态。通常情况下，代码单元是独立测试的:您的测试只影响和监控对该单元的更改。理想情况下，这形成了“测试驱动开发”框架的一部分，以鼓励所有的软件在集成或部署前都经过充分的审查和测试([https://en.wikipedia.org/wiki/Test-driven_development](https://t.umblr.com/redirect?z=https%3A%2F%2Fen.wikipedia.org%2Fwiki%2FTest-driven_development&t=MGZhYzQyMjU5ZmRmMmI0Y2NhNjU1OGZkMmNmZDIxZTk4NmQwZTI2OCxJSzdGMFJwVQ%3D%3D&b=t%3AyOP1_23-DRFIe3DPTeaQHA&p=https%3A%2F%2Fdatascienceunicorn.tumblr.com%2Fpost%2F171341668221%2Ftips-for-writing-good-code&m=1))，从而最大限度地减少以后重构和调试的时间。

## 生产代码

试着像将代码投入生产一样编写代码。这将形成良好的习惯，并在不可避免地(希望如此)投入生产时，使其易于扩大规模。

考虑“算法效率”并尝试优化以减少运行时间和内存使用。“大 O 符号”在这里很重要(【https://en.wikipedia.org/wiki/Big_O_notation】T2)。

还要考虑你的代码环境或生态系统，避免依赖，例如，在代码级别(Python virtualenv:[https://conda . io/docs/user-guide/tasks/manage-environments . html](https://t.umblr.com/redirect?z=https%3A%2F%2Fconda.io%2Fdocs%2Fuser-guide%2Ftasks%2Fmanage-environments.html&t=OTA5NjM2NWE4OTViYzYzMjMzY2JjMDEyOGQ2YjI3ODRiNTViYTc3ZSxJSzdGMFJwVQ%3D%3D&b=t%3AyOP1_23-DRFIe3DPTeaQHA&p=https%3A%2F%2Fdatascienceunicorn.tumblr.com%2Fpost%2F171341668221%2Ftips-for-writing-good-code&m=1))或在操作系统级别(Docker containers:[https://www.docker.com/what-container](https://t.umblr.com/redirect?z=https%3A%2F%2Fwww.docker.com%2Fwhat-container&t=NGU2Yzk4ZjRlODY3YzE5ODZkYTFjNTAxNzkzZWVhNWYyOGY2N2Y4YyxJSzdGMFJwVQ%3D%3D&b=t%3AyOP1_23-DRFIe3DPTeaQHA&p=https%3A%2F%2Fdatascienceunicorn.tumblr.com%2Fpost%2F171341668221%2Ftips-for-writing-good-code&m=1))进行虚拟化。

生产级代码还应该使用“日志记录”，以便于在执行代码时查看、检查和诊断问题(【https://en.wikipedia.org/wiki/Log_file】T4)。

显然，编程和开发领域的深度和广度要大得多，但是希望这些技巧能够帮助人们从代码不多的背景进入数据科学领域。尽情享受吧！

照片由 [**路易斯·戈麦斯**](https://www.pexels.com/@luis-gomes-166706?utm_content=attributionCopyText&utm_medium=referral&utm_source=pexels) 拍摄自 [**Pexels**](https://www.pexels.com/photo/blur-close-up-code-computer-546819/?utm_content=attributionCopyText&utm_medium=referral&utm_source=pexels)