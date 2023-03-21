# 不是所有的名字都一视同仁

> 原文：<https://towardsdatascience.com/all-names-are-not-treated-equally-1e825ba34444?source=collection_archive---------19----------------------->

## *探究单词嵌入中的偏见如何影响简历推荐*

随着机器学习变得越来越流行，越来越多的任务被自动化。虽然这可能有许多积极的好处，如释放人类的时间来做更具挑战性的任务，但也可能有意想不到的后果。

任何机器算法都是使用人类创造的数据来训练的，这些数据可能传达生成数据或构建模型的人不打算或不想要的信息和想法。有许多模型包含对特定人群的非故意偏见的例子。你可能听说过[谷歌的面部识别算法将黑人标记为大猩猩](https://www.usatoday.com/story/tech/2015/07/01/google-apologizes-after-photos-identify-black-people-as-gorillas/29567465/)或者更近的[亚马逊简历推荐系统](https://www.reuters.com/article/us-amazon-com-jobs-automation-insight/amazon-scraps-secret-ai-recruiting-tool-that-showed-bias-against-women-idUSKCN1MK08G)的事故，该算法自学男性候选人更受欢迎，并惩罚包含“女性”一词的简历，该词可能出现在“女子高尔夫球队队长”等短语中

尽管这些算法得到了广泛的应用，但是如何解决它们所包含的偏差仍然是一个悬而未决的问题。这也是一个微妙的问题，因为没有明确的数学标准来衡量一个算法有多大的偏差。偏见也没有一成不变的定义；虽然我当然相信女性和任何人一样能够胜任亚马逊的任何工作，并为此而奋斗，但世界上还是有人不相信性别平等。尽管如此，有大量真实世界的例子表明，机器学习算法中的偏见是如何歧视人们并加剧我们社会中的不公正的。

研究人员正在积极研究机器学习算法中的偏见，我鼓励你查看他们的工作，我在本文末尾提供了链接。为了尝试探索偏见是如何影响简历推荐系统的，我自己进行了一些实验。这些只是一个开始，希望能激发更多的问题和研究，而不是解决这个非常复杂的问题。

在解释我的过程时，我会假设你了解机器学习、单词嵌入和自然语言处理。单词嵌入最初是通过谷歌的 Word2Vec 算法流行起来的，但斯坦福的手套嵌入也越来越受欢迎。为了更好地了解 Word2Vec，请查看 [Zafar Ali 的教程](https://medium.com/@zafaralibagh6/a-simple-word2vec-tutorial-61e64e38a6a1)，为了更好地了解 Glove，请查看 [Sciforce 的文章](https://medium.com/sciforce/word-vectors-in-natural-language-processing-global-vectors-glove-51339db89639)，为了更详细地了解 doc2vec，请查看 [Gidi Shperber 对 doc2vec 的介绍](https://medium.com/scaleabout/a-gentle-introduction-to-doc2vec-db3e8c0cce5e)。我将简要概述如何在推荐系统中使用单词嵌入，然后深入研究我的实验、结果和后续步骤。

**单词嵌入和推荐系统**

除了将单词向量相互比较之外，还可以使用各种技术，使用每个文档中单词的单词嵌入来比较文档。在我的实验中，这些文档是谷歌的招聘启事和不同的简历，但是文档可以是任何类型的文本数据。我没有比较代表单个单词的向量，而是创建了文档的向量表示以及它们在向量空间中的距离。在向量空间中具有彼此最接近的向量表示的文档将被认为彼此最相似并被推荐。

在这个实验中，我计算了文档的向量表示(质心),方法是删除停用词，然后取文档中所有剩余词的向量的平均值。在计算了我的示例职位发布和我测试的所有简历的文档质心之后，我比较了职位发布质心和每个测试简历质心之间的欧几里德距离和余弦距离，以对简历进行排序。一般来说，简历推荐系统不仅仅使用平均简历质心来进行推荐，所以请记住，生产中的系统会涉及更多的算法和步骤，但这个实验仍然可以提供见解。

**方法论**

在这种主观领域没有真正中立的选择，我天生就把我自己的信念融入到我如何设置和运行这个实验中。对于像我这样的研究人员来说，重要的是不仅要检查他们自己的偏见，还要查看其他研究，并与他人合作来减轻偏见。

没有一个名字或一组名字能真正代表任何一群人。许多人的名字并不反映他们自我认同的种族或性别。性别也不是一个二元结构，尽管为了这个实验的目的，我只比较了男性和女性的名字。我选择使用 Aylin Caliskan、Joanna J. Bryson 和 Arvind Narayanan 在他们的论文 [*中使用的一组名称，这些名称是从包含类似人类偏见的语言语料库中自动导出的语义*](https://science.sciencemag.org/content/sci/suppl/2017/04/12/356.6334.183.DC1/Caliskan-SM.pdf) 。我觉得自己没有资格去挑选甚至可以代表我不属于的一群人的名字，也不会有一组名字可以完全代表任何一个群体。我使用了他们使用的非裔美国人和欧洲人的名字，并进一步将他们分成不同性别的群体。我对比了八套简历，有以下几套名字:

●非裔美国人的名字

●欧美名字

●女性非裔美国人的名字

●男性非裔美国人的名字

●女性欧裔美国人的名字

●男性欧裔美国人名字

●女性名字(结合女性欧裔美国人名字和女性非裔美国人名字)

●男性姓名(结合男性欧裔美国人姓名和男性非裔美国人姓名)

我用美国英语进行这项研究，但这种调查应该在所有语言中进行，由母语人士主导研究。我选择了谷歌软件工程招聘，并创建软件工程简历，因为软件工程是一个公认的高薪领域，目前是男性主导的。谷歌是这一领域的领导者，也是一个非常受欢迎的雇主，所以使用谷歌的招聘信息似乎是一个合理的选择。我还选择了软件工程，因为我觉得通过利用我在数据科学研究中获得的专业知识和工作经验，我可以撰写一份合理的入门级简历。在未来的实验中，查看不同公司和不同领域的招聘信息会很有用。

为了起草我的通用入门级软件工程简历，我综合了来自 [indeed.com 的](https://www.indeed.com/career-advice/resume-samples/engineering-resumes/software-engineer?gclid=CjwKCAjw7_rlBRBaEiwAc23rhljs89CQEyUuLXLR7ZLKUxgc0B4Pm3dVrP89tFb_ljDLF5gLyw7bFxoCh_gQAvD_BwE)、[卡耐基梅隆大学计算机科学系的](https://www.cmu.edu/career/documents/sample-resumes-cover-letters/sample-resumes_scs.pdf)和 [monster.com 的](https://www.monster.com/career-advice/article/hair-stylist-resume-sample)样本软件工程简历。每份简历都包括教育部分、一年相关工作经验、一年非相关工作经验和软件工程项目部分。对于不相关的工作经历，我使用了来自[的文本，一份关于工作英雄](https://www.jobhero.com/resume-samples/barber/)的理发师简历。

![](img/bf520a7ca00e1d89cf24f8df17bcb78c.png)

*实验用简历；每份简历唯一改变的是名字。*

为了计算谷歌招聘信息的质心，我使用 300 维预训练手套嵌入采取了以下步骤:

1.  处理了职位发布的文本，创建了一个删除了标点符号和停用词的单词列表(关于停用词的更多信息，请查看[马志威关于停用词的帖子](https://medium.com/@makcedward/nlp-pipeline-stop-words-part-5-d6770df8a936))。)
2.  对于单词列表中的每个单词，查找 300 维单词嵌入。
3.  取这些单词嵌入的平均值作为文档质心。

为了计算每组名字的简历重心，我采取了以下步骤:

1.  为组中的每个名称创建了一个简历文本。
2.  处理每份简历的文本，为每份简历创建一个单词列表，删除标点符号和停用词。
3.  对于单词列表中的每个单词，查找手套 300 维单词嵌入。
4.  将这些单词嵌入的平均值作为该组中每个简历的简历质心。
5.  计算一个组中所有简历质心的平均简历质心。
6.  计算该组的平均简历质心和 Google 职位发布质心之间的余弦和欧几里德距离。

![](img/cee32aa94bf3180c1af5ea22f36427b4.png)

*我的流程的高级图表，用于比较姓名组和职位发布的距离。*

**初步结果**

当使用手套向量时，我发现使用欧几里德距离或余弦距离时，有男性欧裔美国人名字的简历与谷歌软件工程简历最接近。下一个最接近的群体是欧裔美国人的名字，其次是全男性的名字。我查看了欧几里德距离和余弦距离，以及每个组与所有组到招聘信息的平均距离的标准差。

![](img/c041605cddcc3ddd4e493693f9023812.png)

*结果图表。请查看我的完整笔记本* [*这里*](https://github.com/keck343/embedding_biases/blob/master/Groups_of_Names_Resume_Experiment.ipynb) *。*

这表明了这些词嵌入中编码的偏见的非故意但却是负面的影响。虽然所有组都在平均值的 2 个标准偏差内，但是当观察组之间的欧几里德或余弦距离的分布时，在组的顺序中有一个清晰的模式。男性欧裔美国人的简历比男性非裔美国人的简历更接近谷歌的招聘信息，大约有 3 个标准差。种族歧视和性别歧视都会影响这些简历的排名。这些简历除了上面的名字之外都是一样的，而且都应该和招聘信息一样接近。

![](img/c36dcc7b27c3abf4b351791027849cea.png)

这些图表直观地显示了与职位发布的平均距离的标准偏差。*每组简历都标在 x 轴上，y 轴上是它们与职位发布的标准差。具有负值的组意味着其组的平均简历质心比所有组的平均值更接近职务发布质心。相反，具有正值的组表示他们组的平均恢复质心比所有组的平均值更远。*

**结论和后续步骤**

目前，解决单词嵌入中的偏见是一个活跃的研究领域。在有更强有力的解决方案来解决偏见之前，我认为任何使用推荐系统的人都应该从系统排名系统使用的简历文本中删除姓名。暗示候选人性别的职称和经历也可能影响结果，但去除起来更复杂。这些结果只是初步的，这类实验应该用不同的名字、招聘信息等重复进行。在生产中的简历推荐系统中对这些相同的简历进行 A/B 测试也会提供有价值的见解。

在一个公平公正的世界里，每个人的简历都有可能被任何算法推荐，不管他们的名字或身份如何。这只是单词嵌入和机器学习模型中的偏见有害的许多情况之一，在任何研究领域，我们都需要所有我们可以获得的声音和想法。我鼓励你学习更多，与他人合作，如果你所在的公司在你的推荐系统的任何部分使用这种措辞，调查编码的偏见并努力解决它。

特别感谢雷切尔·托马斯和詹姆士·威尔森在这一努力中给予的支持。更多详情请查看[我的 GitHub](https://github.com/keck343/embedding_biases/blob/master/Groups_of_Names_Resume_Experiment.ipynb) 。我很乐意听到你的想法和建议，以及进一步研究的想法！

我邀请反馈，可以通过 gmail.com>的 keck.quinn <联系到我。感谢阅读！

**来源及延伸阅读**

Google 相册文章

[https://www . USA today . com/story/tech/2015/07/01/Google-道歉-拍照后-识别-黑人-大猩猩/29567465/](https://www.usatoday.com/story/tech/2015/07/01/google-apologizes-after-photos-identify-black-people-as-gorillas/29567465/)

亚马逊使用简历推荐系统的不幸

[https://www . Reuters . com/article/us-Amazon-com-jobs-automation-insight/Amazon-scraps-secret-ai-recruiting-tool-that-show-bias-against-women-iduskcn 1 MK 08g](https://www.reuters.com/article/us-amazon-com-jobs-automation-insight/amazon-scraps-secret-ai-recruiting-tool-that-showed-bias-against-women-idUSKCN1MK08G)

简历算法审计

[https://qz . com/1427621/companies-is-on-the-hook-if-they-hiring-algorithms-is-biased/](https://qz.com/1427621/companies-are-on-the-hook-if-their-hiring-algorithms-are-biased/)

手套制品

[https://medium . com/sci force/word-vectors-in-natural-language-processing-global-vectors-glove-51339 db 89639](https://medium.com/sciforce/word-vectors-in-natural-language-processing-global-vectors-glove-51339db89639)

手套项目

[https://nlp.stanford.edu/projects/glove/](https://nlp.stanford.edu/projects/glove/)

Doc2Vec 文章

[https://medium . com/@ zafaralibagh 6/a-simple-word 2 vec-tutorial-61e 64 e 38 a6 a 1](https://medium.com/@zafaralibagh6/a-simple-word2vec-tutorial-61e64e38a6a1)

Word2Vec 文章

[https://medium . com/@ zafaralibagh 6/a-simple-word 2 vec-tutorial-61e 64 e 38 a6 a 1](https://medium.com/@zafaralibagh6/a-simple-word2vec-tutorial-61e64e38a6a1)

从语言语料库中自动获得的语义包含类人偏见

作者:艾林·卡利斯坎、乔安娜·j·布赖森、阿尔温德·纳拉亚南[https://science . science mag . org/content/sci/Suppl/2017/04/12/356.6334 . 183 . dc1/CALIS Kan-sm . pdf](https://science.sciencemag.org/content/sci/suppl/2017/04/12/356.6334.183.DC1/Caliskan-SM.pdf)

纸张:

【https://science.sciencemag.org/content/356/6334/183? URL _ ver = z 39.88-2003&RFT _ id = doi % 3a 10.1126/science . AAL 4230&col wiz-ref = wbdp(需要 AAAS 登录才能访问)

*历时词语嵌入揭示语义变化的统计规律*

作者:林子幸·汉密尔顿，朱尔·莱斯科维奇，丹·茹拉夫斯基

[https://nlp.stanford.edu/projects/histwords/](https://nlp.stanford.edu/projects/histwords/)

男人对于电脑程序员就像女人对于家庭主妇一样？去偏置词嵌入

作者:、张开伟、詹姆斯·邹、文卡特什·萨利格拉玛、亚当·卡莱

[https://arxiv.org/pdf/1607.06520.pdf](https://arxiv.org/pdf/1607.06520.pdf)

*猪身上的口红:去偏见方法掩盖了单词嵌入中的系统性性别偏见，但并没有消除它们*

希拉·戈宁，约夫·戈德堡

[https://arxiv.org/abs/1903.03862](https://arxiv.org/abs/1903.03862)

*理解机器学习意外结果的框架*

约翰·古塔格·哈里尼·苏雷什

[https://arxiv.org/abs/1901.10002](https://arxiv.org/abs/1901.10002)

如果公司的招聘算法有偏差，他们就有麻烦了

戴夫·格什格恩

[https://qz . com/1427621/companies-is-on-the-hook-if-they-hiring-algorithms-is-biased/](https://qz.com/1427621/companies-are-on-the-hook-if-their-hiring-algorithms-are-biased/)

*自然语言处理的数据陈述:减少系统偏差和实现更好的科学*

艾米莉·本德，巴特亚·弗里德曼

[https://www.aclweb.org/anthology/Q18-1041](https://www.aclweb.org/anthology/Q18-1041)

代码链接:[https://github . com/keck 343/embedding _ bias/blob/master/Groups _ of _ Names _ Resume _ experiment . ipynb](https://github.com/keck343/embedding_biases/blob/master/Groups_of_Names_Resume_Experiment.ipynb)