# 多亏了伯特，总结已经商品化了

> 原文：<https://towardsdatascience.com/summarization-has-gotten-commoditized-thanks-to-bert-9bb73f2d6922?source=collection_archive---------8----------------------->

![](img/75a56ec97d7c3103378a99a214234c51.png)

照片由 [Aaron Burden](https://unsplash.com/@aaronburden?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) 在 [Unsplash](https://unsplash.com/s/photos/summary?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)

你曾经不得不把一份冗长的文件总结成要点吗？或者为文档提供执行摘要？如你所知，对我们人类来说，这个过程是冗长而缓慢的——我们需要阅读整个文档，然后关注重要的句子，最后，将句子改写成一个连贯的摘要。

这就是自动总结可以帮助我们的地方。机器学习在总结方面已经走了很长的路，但仍有很大的发展空间。通常，机器摘要分为两种类型—

*提取摘要*:提取原始文档中出现的重要句子。

![](img/d8433a36943aeac563a47b74942af89e.png)

摘录摘要的类比

*抽象概括*:概括文档中包含的重要观点或事实，不逐字重复。当人们被要求总结一份文件时，这就是我们通常想到的。

![](img/f1e7c96d11d38c694b55b1e968318d11.png)

抽象概括的类比

我想向大家展示一下最近使用 BERT_Sum_Abs 进行抽象摘要的一些结果，这是刘洋和米雷拉·拉帕塔在[使用预训练编码器进行文本摘要](https://arxiv.org/pdf/1908.08345.pdf)中描述的最先进的自然语言处理摘要模型。

# 抽象 BERT 摘要性能

摘要的目的是将一个文档压缩成一个较短的版本，同时保留其大部分含义。抽象摘要任务需要语言生成能力来创建包含源文档中没有的新单词和短语的摘要。摘要通常被定义为二进制分类任务，其标签指示一个文本范围(通常是一个句子)是否应该包括在摘要中。

下面是 *BERT_Sum_Abs* 在标准摘要数据集上的表现: *CNN* 和 *Daily Mail* 这些都是基准中常用的。评估指标被称为 [ROGUE](http://www.ccs.neu.edu/home/vip/teach/DMcourse/5_topicmodel_summ/notes_slides/What-is-ROUGE.pdf) F1 得分—

![](img/2238d12299bf65ed6d3f9bf79bc22845.png)

基于刘洋和米雷拉·拉帕塔的[带预训练编码器的文本摘要](https://arxiv.org/pdf/1908.08345.pdf)

结果显示 *BERT_Sum_Abs* 优于大多数非基于变压器的模型。更好的是，该模型背后的代码是开源的，并且可以在 [Github](https://github.com/huggingface/transformers/tree/master/examples/summarization/bertabs) 上实现。

# 演示和代码

让我们看一个用 *BERT_Sum_Abs* 总结文章的例子。我们将选择下面的故事来总结— [美联储官员表示，各国央行行长在应对冠状病毒方面步调一致](https://www.nytimes.com/2020/03/05/business/economy/fed-rate-cut-coronavirus.html)。这是全文——

```
The Federal Reserve Bank of New York president, John C. Williams, made clear on Thursday evening that officials viewed the emergency rate cut they approved earlier this week as part of an international push to cushion the economy as the coronavirus threatens global growth.Mr. Williams, one of the Fed’s three key leaders, spoke in New York two days after the Fed slashed borrowing costs by half a point in its first emergency move since the depths of the 2008 financial crisis. The move came shortly after a call between finance ministers and central bankers from the Group of 7, which also includes Britain, Canada, France, Germany, Italy and Japan.“Tuesday’s phone call between G7 finance ministers and central bank governors, the subsequent statement, and policy actions by central banks are clear indications of the close alignment at the international level,” Mr. Williams said in a speech to the Foreign Policy Association.Rate cuts followed in Canada, Asia and the Middle East on Wednesday. The Bank of Japan and European Central Bank — which already have interest rates set below zero — have yet to further cut borrowing costs, but they have pledged to support their economies.Mr. Williams’s statement is significant, in part because global policymakers were criticized for failing to satisfy market expectations for a coordinated rate cut among major economies. Stock prices temporarily rallied after the Fed’s announcement, but quickly sank again.Central banks face challenges in offsetting the economic shock of the coronavirus.Many were already working hard to stoke stronger economic growth, so they have limited room for further action. That makes the kind of carefully orchestrated, lock step rate cut central banks undertook in October 2008 all but impossible.Interest rate cuts can also do little to soften the near-term hit from the virus, which is forcing the closure of offices and worker quarantines and delaying shipments of goods as infections spread across the globe.“It’s up to individual countries, individual fiscal policies and individual central banks to do what they were going to do,” Fed Chair Jerome H. Powell said after the cut, noting that different nations had “different situations.”Mr. Williams reiterated Mr. Powell’s pledge that the Fed would continue monitoring risks in the “weeks and months” ahead. Economists widely expect another quarter-point rate cut at the Fed’s March 18 meeting.The New York Fed president, whose reserve bank is partly responsible for ensuring financial markets are functioning properly, also promised that the Fed stood ready to act as needed to make sure that everything is working smoothly.Since September, when an obscure but crucial corner of money markets experienced unusual volatility, the Fed has been temporarily intervening in the market to keep it calm. The goal is to keep cash flowing in the market for overnight and short-term loans between banks and other financial institutions. The central bank has also been buying short-term government debt.“We remain flexible and ready to make adjustments to our operations as needed to ensure that monetary policy is effectively implemented and transmitted to financial markets and the broader economy,” Mr. Williams said Thursday.
```

开始之前，我们需要获取模型代码，安装依赖项并下载数据集，如下所示，您可以在自己的 Linux 机器上轻松执行这些操作:

开始总结的 Linux 命令

按照上面的代码，我们现在执行下面显示的 python 命令来汇总 **/dataset2** 目录中的文档:

```
python run_summarization.py \
    --documents_dir bertabs/dataset2 \
    --summaries_output_dir bertabs/summaries_out \
    --batch_size 64 \
    --min_length 50 \
    --max_length 200 \
    --beam_size 5 \
    --alpha 0.95 \
    --block_trigram true \
    --compute_rouge true
```

这里的参数如下—

> **documents_dir** — *要汇总的文档所在的文件夹*
> **summaries _ output _ dir**—*要写入摘要的文件夹。默认为文件夹，文件为*
> **Batch _ size**—*Batch size per GPU/CPU for training*
> **beam _ size**—*每个示例开始的波束数*
> **block _ trigram**—*是否阻止波束搜索*
> **Compute _ ROUGE**仅适用于 CNN/DailyMail 数据集
> **alpha** — *波束搜索中长度罚分的 alpha 值(值越大罚分越大)*
> **min _ length**—*摘要的最小令牌数*
> **max _ length**—*摘要的最大令牌数*

一旦 *BERT_Sum_Abs* 完成了这篇文章，我们就获得了以下摘要:

```
The Fed slashed borrowing costs by half a point in its first emergency move since the depths of the 2008 financial crisis. Rate cuts followed in Canada, Asia and the Middle East on Wednesday. The Bank of Japan and European Central Bank have yet to further cut borrowing costs, but they have pledged to support their economies.
```

总结的另一个例子[研究表明，低碳水化合物饮食可以预防、逆转大脑中与年龄相关的影响](https://news.stonybrook.edu/newsroom/study-shows-low-carb-diet-may-prevent-reverse-age-related-effects-within-the-brain/)得出以下总结:

```
The research team focused on the Presymptomatic period during which prevention may be most effective. They showed that communication between brain regions destabilizes with age, typically in the late 40's, and that destabilization associated with poorer cognition. The good news is that we may be able to prevent or reverse these effects with diet, mitigating the impact of encroaching Hypometabolism by exchanging glucose for ketones as fuel for neurons.
```

# 结论

如你所见，BERT 正在渗透到 NLP 的各个方面。这意味着我们看到 NLP 的性能[每天都在接近人类的水平](https://super.gluebenchmark.com/leaderboard/)同时还是开源的。

NLP 的巨大商品化正在到来，每一个新的 NLP 模型不仅建立了新的基准记录，而且任何人都可以使用。就像 10 年前 OCR 技术变得商品化一样，NLP 在未来几年也将如此。随着 NLP 的商品化，许多较小的参与者正在被打乱，因为他们在 NLP 技术 R&D 的护城河在他们眼前蒸发了。

NLP 的这种商品化应该引导人工智能公司转移重点，进入特定的垂直领域，建立解决领域痛点的护城河，而不是将 NLP 技术作为核心价值主张。具体来说，在这里您可以看到技术已经商品化的摘要，因为新的研究成果很快被实现为开源代码，同时超过了以前的基准。