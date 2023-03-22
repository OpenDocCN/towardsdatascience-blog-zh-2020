# 创建您的第一个聊天机器人！

> 原文：<https://towardsdatascience.com/create-your-first-chatbot-20636e7581e?source=collection_archive---------24----------------------->

![](img/d717360215aa453502f6077b34a01bfb.png)

图片来自 [Pixabay](https://pixabay.com/?utm_source=link-attribution&utm_medium=referral&utm_campaign=image&utm_content=3186713) 的[LukáSKU cius](https://pixabay.com/users/LukessCz-7997380/?utm_source=link-attribution&utm_medium=referral&utm_campaign=image&utm_content=3186713)

# 30 秒解释聊天机器人如何工作

无论您是数据科学家、数据分析师还是软件工程师；无论你是否对 NLP 工具和方法有很强的把握，如果你在这里，你可能想知道聊天机器人是如何工作的，如何建立一个，但从来没有这个需要或机会。嗯……你现在在这里，所以让我们把它做好。

你会惊讶地发现，你很可能对创建自己的聊天机器人很熟悉

# 用例？

可能是查询一个简单的数据库、订购、预订、客户服务等等。我们每天都和聊天机器人互动。

# 里面是什么？

聊天机器人都是关于所谓的规则匹配引擎。

以最简单的形式；这就意味着当你收到一条信息时，你会怎么处理它？

一种方法是重复最初输入的内容。虽然这看起来过于简单和无趣，但是在你使用聊天机器人进入更复杂的领域之前，这个想法是一个很好的基础。

让我们把手弄脏吧

`def parrot_talk(incoming_message):`

`bot_response = "Hi, my name is Randy the Parrot, you said: " + message`

`return bot_response`

`print(parrot_talk("hello!"))`

显然这是一个简单的例子，让我们把它提高一个档次。

`responses = {`

`"Hello": "Hey! How can I help you?",`

`"I need a refund": "I'm happy to help, can you give me more detail?"`

`}`

`def respond(message):`

`if message in responses:`

`return responses[message]`

`respond("I need a refund?")`

类似于你第一次看到的，这是一个客服聊天机器人。我们在上面的字典中所做的是建立与我们的客户可能说的话相关的关键字…这些关键字然后映射到适当的响应。

显然，这里的限制是，它要求消息与字典中的键完全相同。

我们可以很快变得非常严格，对任何给定的消息使用正则表达式，创建一个标志来指示文本中各种术语的存在&然后关闭该标志来填充我们的响应。

为了更进一步，我们可以部署 ML 算法，该算法可以预测消息是否属于给定的意图或主题，然后我们可以将其映射回适当响应的字典。

# 挑战

你见过 LinkedIn 上的自动回复吗？你应该试着通过点击这些提示来进行对话…除了有趣，这也是对复杂和缺乏复杂的有趣探索。对于聊天机器人来说，最困难的事情之一是跟踪任何给定对话状态的能力，一旦你在 LinkedIn 上进行聊天，这一点就会非常明显。

# 结论

乍看之下，聊天机器人非常容易创建。祝数据科学快乐！