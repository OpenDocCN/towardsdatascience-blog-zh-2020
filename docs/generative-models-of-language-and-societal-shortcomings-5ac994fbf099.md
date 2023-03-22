# 语言的生成模型和社会缺陷

> 原文：<https://towardsdatascience.com/generative-models-of-language-and-societal-shortcomings-5ac994fbf099?source=collection_archive---------45----------------------->

## OpenAI 生成模型的简史及其误用对社会的潜在影响。

对缺点的理解并不完全取决于某项特定技术的缺点或缺陷，而是取决于这项技术如何改变世界，无论是好是坏。

2015 年，一家名为 OpenAI 的公司由埃隆·马斯克、山姆·奥特曼、彼得·泰尔等人创立并向公众亮相。创始人以及微软和印孚瑟斯、[等大公司承诺总计 10 亿美元](https://www.bbc.com/news/technology-35082344)用于创建“[非营利企业](https://www.bbc.com/news/technology-35082344)”。

最初，OpenAI 是作为一个非营利组织成立的，部分原因是当时对人工智能近年来的发展感到担忧。OpenAI 作为非营利组织的理由与这些担忧没有太大关系，而是根据一篇介绍性博客文章的以下内容[:](https://openai.com/blog/introducing-openai/)

> “作为一个非营利组织，我们的目标是为每个人而不是股东创造价值。研究人员将被强烈鼓励发表他们的工作，无论是作为论文、博客帖子还是代码，我们的专利(如果有的话)将与世界共享。我们将与许多机构的其他人自由合作，并期望与公司合作研究和部署新技术。”

2017 年，斯蒂芬·霍金(Stephen Hawking)将人工智能的出现描述为很可能是“我们文明史上最糟糕的事件”，突显出整个社会对其发展缺乏监督。早在 2014 年，马斯克本人在麻省理工学院(MIT)发表演讲时就认为，人工智能的发展是人类“最大的生存威胁”。

![](img/cf3a2d5a200acadc489cd9be60cefd7d.png)

先锋大厦，OpenAI 和 Neuralink 的办公室所在地。(HaeB / [CC BY-SA](https://creativecommons.org/licenses/by-sa/4.0)

几年后，在 2018 年，OpenAI [公布了第一篇关于生成性预培训的论文](https://cdn.openai.com/research-covers/language-unsupervised/language_understanding_paper.pdf)，这就是后来被称为 GPT 的开端。

这篇论文由亚历克·拉德福德和其他三位 OpenAI 同事撰写，得出的结论是，语言的生成模型实际上可以获得“重要的世界知识”和“处理长期依赖的能力”。由此，该模型还将能够解决“诸如问题回答、语义相似性评估、蕴涵确定和文本分类”等任务。

## GPT-2:

关于 GPT-2 的研究论文于 2019 年 2 月发布，但由于围绕滥用措施的潜在利用，特别是关于假新闻的构建和发展的原因，其全面功能的发布被扣留，这一主题将在下文详述。

Transformer 语言模型的工作方式是通过人类主动输入至少几个单词到一页，然后模型继续自动预测和生成接下来人工生成的几行应该是什么。

*《卫报》*进行了一项类似于[的实验](https://www.theguardian.com/technology/2019/feb/14/elon-musk-backed-ai-writes-convincing-news-fiction)，以乔治·奥威尔的小说《1984》开头为特色。该行如下所示:

> 这是四月里一个晴朗寒冷的日子，时钟正敲 13 下

GPT-2 设法使用这一行，这一行只产生了以下生成的文本片段，按照*卫报*及其文章，怪异地遵循“[模糊的未来主义语气和小说风格](https://www.theguardian.com/technology/2019/feb/14/elon-musk-backed-ai-writes-convincing-news-fiction):

> “我在车里，正要去西雅图找一份新工作。我加油，插钥匙，然后让它跑。我只是想象着今天会是什么样子。一百年后。2045 年，我是中国农村贫困地区一所学校的老师。我从中国历史和科学史开始。”

当然，如果第二次、第三次或第四次输入完全相同的文本，该模型不会重复。有趣的是，这个文本明显不同于其他人工智能生成模型，尤其是在理解领域。

![](img/c5618d1b4e79f24ed191ac7fa2bc109c.png)

弗兰基·查马基在 Unsplash 上的照片

OpenAI 现任研究副总裁 Dario Amodei 表示，与以前相比，使用的模型“[大了 12 倍，数据集大了 15 倍，范围也大了很多](https://www.theguardian.com/technology/2019/feb/14/elon-musk-backed-ai-writes-convincing-news-fiction)。实质上，向 GPT-2 提供的大量数据是其卓越的质量和可理解性背后的主要原因。

如果你有兴趣亲自体验 GPT-2，[亚当·金](https://twitter.com/AdamDanielKing)创建了一个网站，利用 GPT-2 的完整模型，并将其命名为[与变形金刚](https://talktotransformer.com/)对话。

## GPT-3:

2020 年 5 月，GPT-3 在 GitHub 上发布，与 GPT-2 的开发相比，它的大小有了天文数字的增长。前身模型是利用完整版本中的 15 亿个参数构建的。相比之下，GPT-3 是用 1750 亿个参数建造的，增加了 115 倍。

虽然由于 GPT-3 数据集和模型规模的增加，该模型的性能水平(与自然语言处理相关)甚至比 GPT-2 更好，但 OpenAI 也公开表示该模型可能接近其绝对极限。

这篇论文详细介绍了 GPT-3 的成功、局限性、更广泛的影响以及更多的内容，有效地证实了简单地向一个模型扔更多的数据并不能在无限的轨道上产生更好的结果。

它还围绕滥用的可能性，特别是在创建更大的模型时:

> “任何依赖生成文本的对社会有害的活动都可能被强大的语言模型所增强。示例包括错误信息、垃圾邮件、网络钓鱼、滥用法律和政府程序、欺诈性学术论文写作和社会工程借口。这些应用中的许多限制了人们写出足够高质量的文本。产生高质量文本生成的语言模型可以降低执行这些活动的现有障碍，并提高它们的效率。”

一个语言生成模型必须发展得越多，特别是与它所拥有的数据和信息的数量相关，它就越适用于一般用途的应用程序，包括但不限于它对恶意行为的使用。

## 假文本的产生和问题:

虽然 OpenAI 最初是一家非营利性企业，但该公司转向了盈利模式，或者用他们的话说，一种“[利润上限](https://openai.com/blog/openai-lp/)模式，根据他们的博客和 *TechCrunch* 的说法，这种模式将允许他们“[在超过某个点](https://techcrunch.com/2019/03/11/openai-shifts-from-nonprofit-to-capped-profit-to-attract-capital/)后削减投资回报。

围绕这一举动的基本原理不仅仅是获得更多的投资资本，而是植根于一种理解，即积累比他们已经拥有的更多的计算能力，“建造人工智能超级计算机”等等，将需要在未来几年内投资[数十亿美元](https://openai.com/blog/openai-lp/)。

![](img/fcba8c3934093d22d8b5d3f4093e14f1.png)

安娜·阿尔尚蒂在 Unsplash 上的照片

向盈利模式的转变当然与保持透明度和问责制有关，但对于 OpenAI 生产和开发 GPT 等模型也存在其他问题。

如前所述，2019 年 2 月，OpenAI [拒绝发布 GPT-2](https://www.theguardian.com/technology/2019/feb/14/elon-musk-backed-ai-writes-convincing-news-fiction) ，纯粹是因为其被用于恶意目的的可能性。像 GPT-2 这样的模型的出现导致学者和记者将生成的文本称为“ [deepfakes for text](https://www.theguardian.com/technology/2019/feb/14/elon-musk-backed-ai-writes-convincing-news-fiction) ”，该公司花时间“[讨论技术突破的后果](https://www.theguardian.com/technology/2019/feb/14/elon-musk-backed-ai-writes-convincing-news-fiction)”。

滥用的例子包括上述主题:

> “错误信息、垃圾邮件、网络钓鱼、滥用法律和政府程序、欺诈性学术论文写作和社会工程借口”

该公司承认，这些领域需要产生“高质量文本”的能力，但这正是为什么所谓的“deepfakes for text”的发展令人担忧，特别是在新闻、学术研究和小说家等领域。

该技术的当前状态，至少就 OpenAI 的开发而言，还远远没有完全消除和消除基于人类的输入。这一事实，再加上处理预先确定的语言和文本的神经网络的发展很可能肯定有其局限性，并没有引起人们的警惕。

尽管如此，不应该低估对其增长的关注，特别是随着时间的推移技术不断发展。

除了这些改进之外，OpenAI 还从该技术“太危险而不能积极使用”的心态转变为根据 [*The Verge*](https://www.theverge.com/2020/6/11/21287966/openai-commercial-product-text-generation-gpt-3-api-customers) 发布“其第一个[商业产品](https://openai.com/blog/openai-api/)”。

该公司最终得出结论，鉴于目前的技术状态，对 GPT-2 的恶意使用的担忧是不必要的，但新的发展要大得多，甚至更准确，并且“[现在是你的，但要付出代价](https://www.theverge.com/2020/6/11/21287966/openai-commercial-product-text-generation-gpt-3-api-customers)”。

![](img/76eefadb720ee3fda0876c6e7aa2e128.png)

照片由[安德鲁·尼尔](https://unsplash.com/@andrewtneel?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)在 [Unsplash](https://unsplash.com/s/photos/journalism?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) 上拍摄

受到人工创建文本的发展积极影响的领域的例子包括“[微软新闻和 MSN 组织](https://www.theverge.com/2020/5/30/21275524/microsoft-news-msn-layoffs-artificial-intelligence-ai-replacements)”，其中“[数十名记者和编辑工作者](https://www.theverge.com/2020/5/30/21275524/microsoft-news-msn-layoffs-artificial-intelligence-ai-replacements)”截至 2020 年 5 月已被解雇。根据 *The Verge* 的一篇文章，这是由于[微软更大力度地依靠人工智能来挑选新闻和内容……在 MSN.com，在微软的 Edge 浏览器内，以及在该公司的各种微软新闻应用](https://www.theverge.com/2020/5/30/21275524/microsoft-news-msn-layoffs-artificial-intelligence-ai-replacements)。

随着技术的发展，现实世界的影响肯定存在，而且只会继续增长。如果没有与这种生成性文本模型的增加的生产相关的任何种类的监督，整个工作领域在未来都有被关闭的风险。

虽然具体日期当然还不能确定，但微软的举动是所有类型的新闻和内容创作可能不幸未来的开始的一部分。