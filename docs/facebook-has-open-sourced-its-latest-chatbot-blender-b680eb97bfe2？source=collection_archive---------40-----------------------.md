# 脸书公开了其最新的聊天机器人搅拌机

> 原文：<https://towardsdatascience.com/facebook-has-open-sourced-its-latest-chatbot-blender-b680eb97bfe2?source=collection_archive---------40----------------------->

## 这是一个更像人类的聊天机器人，击败了谷歌成为世界上最好的聊天机器人

![](img/829c4d7777716485ad2c5536ec04a9a6.png)

亚历克斯·哈尼在 [Unsplash](https://unsplash.com/s/photos/facebook-ai?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) 上的照片

4 月 29 日，脸书人工智能研究院([展会](https://ai.facebook.com/blog/state-of-the-art-open-source-chatbot))宣布，它已经建立并开源了一个新的聊天机器人 Blender。

[](https://ai.facebook.com/blog/state-of-the-art-open-source-chatbot/) [## 最先进的开源聊天机器人

### 脸书人工智能有一个开源的搅拌机，有史以来最大的开放域聊天机器人。它在…方面胜过其他产品

ai.facebook.com](https://ai.facebook.com/blog/state-of-the-art-open-source-chatbot/) 

据 [FAIR](https://ai.facebook.com/blog/state-of-the-art-open-source-chatbot) 报道，Blender 是有史以来最大的 ***开放域*** 聊天机器人。它也是第一个拥有多种对话技能的聊天机器人，包括扮演角色、表现出同理心和讨论几乎任何话题的能力。FAIR 声称 Blender 击败了 Google 的 Meena 成为世界上最好的。

如果你想了解更多，请查看 Github 和报纸。

*   GitHub:[https://parl.ai/projects/blender/](https://parl.ai/projects/blender/)
*   论文:[https://arxiv.org/abs/2004.13637](https://arxiv.org/abs/2004.13637)

# 击败谷歌成为世界上最好的

费尔声称 Blender 是最大的*聊天机器人，并且是世界上最好的。*

*预先训练和微调的搅拌机模型可在 [GitHub](https://parl.ai/projects/blender/) 上获得。该模型包含多达 94 亿个参数，比现有最大的系统——[谷歌最新的聊天机器人 Meena](https://ai.googleblog.com/2020/01/towards-conversational-agent-that-can.html) 多 3.6 倍。*

*![](img/c074708a0fd177eef4dce5d57c4842e7.png)*

*与脸书 Blender 的对话*

*当谷歌在 1 月份发布 Meena 时，谷歌称它是世界上最好的聊天机器人。现在，根据脸书自己的成对人类评估，*

*   *75%的评估者认为 Blender 比 Meena 更有吸引力，*
*   *67%的评估者认为 Blender 的表现更像人类。*
*   *49%的评估者最初无法区分聊天机器人 Blender 和真人。*

*其他聊天机器人和 Blender 的主要区别在于 Blender 有能力扮演一个角色，表现出同理心，并讨论几乎任何话题。*

*在撰写本文时，谷歌尚未对这一说法发表评论。*

# *搅拌机食谱*

*根据 FAIR 的说法，Blender 令人印象深刻的结果是:*

1.  *巨大规模的训练数据，*
2.  *混合技能的附加数据集*
3.  *生成策略。*

*与其他自然语言处理项目一样，Blender 接受了大规模的训练，涉及 15 亿次公开的 Reddit 对话。*

*此外，使用**混合技能对话(BST)** 对该模型进行了微调，这加强了以下技能的模型:*

*   *引人入胜地运用个性( [PersonaChat](https://arxiv.org/abs/1801.07243) )*
*   *运用知识([维基百科向导](https://l.facebook.com/l.php?u=https%3A%2F%2Farxiv.org%2Fabs%2F1811.01241&h=AT3AkJOjTbb74J5xdkbK7mtlycsdvC-CdSLpBP2HeVdVF_4Gj34MCUTHd7NLmH-Wi3xBDboAa7YTNzNEUqxFadcr3YDe9Aph_pmHoKtzkOADnQaEJ7DcwUvuKeuSzifjDigBtvo_))*
*   *移情的展示([移情对话](https://arxiv.org/abs/1811.00207))*
*   *能够无缝融合这三者*

*在训练和微调过程中，使用了许多可能的**生成策略**，包括波束搜索、下一个令牌采样、n 元语法分块和仔细选择搜索超参数。*

*![](img/75388c7cf6f04bd6c9148d699d1c0510.png)*

*9.4B 参数模型需要至少两个 32gb V100 GPUs 进行交互。*

# *Blender 还是会出错*

*虽然结果看起来很优秀，但 Blender 的技能在对话系统中仍远未达到人类水平的智能。*

*到目前为止，该团队只评估了 14 轮对话流的聊天机器人。研究人员说，Blender 可能会在几天或几周的谈话过程中重复和乏味。*

*另一个问题是 Blender 无法记住更早的对话。Blender 使用标准的 Transformer 架构，该架构有 128 个 BPE 历史令牌的硬限制，因此不可能扩展他们从用户那里学到的东西或关于用户的东西，请参考他们以前说过的东西。*

*Blender 还倾向于整理事实，这是深度学习的一个限制，因为它基于统计相关性而不是知识来生成句子。*

# *防止聊天机器人使用有毒或有偏见的语言*

*你还记得 **Tay** 吗？ **Tay** 是一个聊天机器人，最初由微软公司于 2016 年 3 月 23 日通过 Twitter 发布；当该机器人开始通过其 Twitter 帐户发布煽动性和攻击性的推文时，它引起了随后的争议，导致微软在推出后仅 16 小时就关闭了该服务。*

*![](img/00b4b9df191257e6f13a6c66689b2493.png)*

*“泰”在不到 24 小时的时间里从“人类超级酷”变成了十足的纳粹*

*任何聊天机器人都面临着同样的挑战:如何防止它说出有毒或有偏见的语言。这些聊天机器人被训练成模仿人与人之间的对话，它们有时可以从人与人的数据中学习不良特征。*

*FAIR 一直试图通过要求众包工作人员过滤掉有毒语言来解决这个问题。然而，这似乎是一个不可能的任务，因为 Reddit 数据集太大了。此外，研究小组还试图在有毒语言出现之前应用分类器来检测它，但他们指出，这样的分类器仍然不是绝对正确的。*

# *尽情享受吧！*

*大概就是这样。感谢阅读。*

*如果你想有一个发挥和阅读细节，这里有链接*

*   *GitHub:【https://parl.ai/projects/blender/ *
*   *论文:[https://arxiv.org/abs/2004.13637](https://arxiv.org/abs/2004.13637)*