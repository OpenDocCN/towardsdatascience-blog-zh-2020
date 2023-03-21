# 简历—总结和匹配

> 原文：<https://towardsdatascience.com/resume-summarizing-and-matching-165840cf9f75?source=collection_archive---------18----------------------->

![](img/d64a1836f70a999103544babd0ff177a.png)

艾玛·马修斯数字内容制作在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 拍摄的照片

仅仅在17 周内，将近 5100 万美国人申请失业保险——这比大萧条期间申请失业保险的人数还要多。随后，找到一份符合自己兴趣的合适工作并不容易。因为企业已经将业务转移到网上，从而造成了机会的匮乏。

显而易见，如果你在这里，你正在努力增加获得梦想工作的机会。然而，在 indeed.com、LinkedIn.com 或其他招聘网站上寻找新工作却是一项艰巨的任务。这种寻找需要没完没了地阅读职位描述，不断更新简历以适应感兴趣的组织需求。

我向你提交这个，如果有一种方法可以让机器总结并匹配你的简历和职位描述，会怎么样？而且，它给你提供了一个方向，你需要在这个方向上付出额外的努力。

让我们看看这是可能的还是只是一个想法。

# 总结

“说说你自己吧？”——这是一个臭名昭著的问题，但在招聘人员中很流行。一位招聘人员要求(或者至少已经要求我)更多地了解候选人的兴趣，并评估你是否适合这个角色。在这种情况下，一个人想要总结他们的简历，而不是按时间顺序背诵你的简历。

让我们在 Jupyter 笔记本中导入一些库。

```
# Import summarize from gensim
from gensim.summarization.summarizer import summarizefrom gensim.summarization import keywords# Import the library# to convert MSword doc to txt for processing.import docx2txt
```

现在让我们阅读和理解你和你的有趣的立场。

```
# Store the resume in a variableresume = docx2txt.process(“babandeep.docx”)text_resume = str(resume)#Summarize the text with ratio 0.1 (10% of the total words.)summarize(text_resume, ratio=0.1)
```

> 丰富的有监督和无监督机器学习模型经验-回归:线性，逻辑，套索，岭，分类器:SVM，K-NN，决策树，感知器，随机梯度下降生成:朴素贝叶斯，LDA，集成:随机森林，XgBoost，GBM，无监督:K-means，DBSCAN，分层聚类，深度学习:CNN，RNN (LSTM，比尔斯特姆)。\ n 利用 python 中的 SVM、决策树、k-NN、逻辑回归(使用 t 检验、卡方检验、单变量和双变量分析)\ n 利用朴素贝叶斯分类器、集成(随机森林)、回归(决策树、逻辑)和分类器(SVM、k-NN)，在 r 中具有 87%的准确性

听起来很有希望:它很好地概括了我的简历的要旨和我所具备的重要的解决问题的能力。

这里我用了 [*机器学习工程师*](https://www.indeed.com/viewjob?cmp=Triplebyte&t=Machine+Learning+Engineer&jk=5000f9501cecac0f&sjdu=bxvQ0S_06ENLQJvr5P3hPmNES9oJadh6b32NSH8Zvx29wif2DwTXqVMXtLgHPOBqu10ictL4zh2Lbh06uriY0AvDvxrYtEe2-CJ-rSqpGeXSMS2xqiIP_Tj8LTLJVrTeNDZv0iXfX1kgLDBUip1yGw&tk=1edu08ci6353a000&adid=355142144&pub=4a1b367933fd867b19b072952f68dceb&vjs=3) 来试试这个。

```
text = input(“ Enter Job description : “) # Prompt for the Job description.# Convert text to string formattext = str(text)#Summarize the text with ratio 0.1 (10% of the total words.)summarize(text, ratio=0.1)
```

输出:

> “Triplebyte 每月筛选和评估数千名工程师，为我们的合作伙伴公司寻找最佳候选人。\作为一名机器学习工程师，您将负责设计和运行实验的端到端流程，以便为大规模生产模型提供服务。\ n 我们的一些管道使用现成的组件，但我们也在实施来自最新研究论文的自定义模型和技术。\ n 我们的最终目标是收集最大的数据集，并以此建立世界上最好的技术招聘流程。”

有趣的是，这是我正在调查的事情，只需一秒钟，我就可以了解这家公司以及他们的期望。

# 相称的

所以，现在我知道这家公司在期待什么，这就引出了下一个令人困惑的问题:我符合他们对理想候选人的描述吗？

```
# recycle the text variable from summarizing# creating A list of texttext_list = [text_resume, text]from sklearn.feature_extraction.text import CountVectorizercv = CountVectorizer()count_matrix = cv.fit_transform(text_list)
```

> CountVectorizer:将一组文本文档转换成一个令牌计数矩阵。该实现使用 scipy.sparse.csr_matrix 生成计数的稀疏表示。如果您不提供先验词典，并且不使用进行某种特征选择的分析器，那么特征的数量将等于通过分析数据找到的词汇大小。

# 余弦相似度是什么？

> *余弦相似性是一种度量标准，用于衡量文档的相似程度，而不考虑其大小。在数学上，它测量的是在多维空间中投影的两个向量之间的角度余弦。余弦相似性是有利的，因为即使两个相似的文档相距欧几里德距离很远(由于文档的大小)，它们仍有可能更靠近在一起。角度越小，余弦相似度越高。*

```
from sklearn.metrics.pairwise import cosine_similarity# get the match percentage
matchPercentage = cosine_similarity(count_matrix)[0][1] * 100matchPercentage = round(matchPercentage, 2) # round to two decimalprint(“Your resume matches about “+ str(matchPercentage)+ “% of the job description.”)# outputYour resume matches about 33.33% of the job description.#keywordsprint(keywords(text, ratio=0.25)) 
# gives you the keywords of the job description
```

哦！我不是很匹配这份工作(33.33%或只有 1/3)，即使我有所有需要的库和调制的经验。我最初认为这是成为一名机器学习工程师的核心要求，然而，现在我知道我必须专注于什么才能在 TripleByte 获得面试机会。

# 这种情况下我能做什么？

到目前为止，我想到了两种方法:

1.  要么用职位描述的总结来更新我的简历。
2.  使用关键字并将其纳入我的简历中，使我的相似度得分达到 80%以上。

后面的部分为我节省了更多的时间，这也是本文的主旨。

如果这种方法在任何方面对你有帮助，或者如果你有任何我们可以从中得出的见解，请随意留下评论。

在接下来的任务中，我计划将复制粘贴职位描述的过程自动化为爬行并节省更多时间，并且只显示最匹配的职位描述。

# 参考资料:

*   [https://sci kit-learn . org/stable/modules/generated/sk learn . feature _ extraction . text . count vectorizer . html](https://scikit-learn.org/stable/modules/generated/sklearn.feature_extraction.text.CountVectorizer.html)
*   [https://www . machine learning plus . com/NLP/Cosine-similarity/#:~:text = Cosine % 20 similarity % 20 is % 20a % 20 metric，in % 20a % 20 multi % 2d dimensional % 20 space。](https://www.machinelearningplus.com/nlp/cosine-similarity/#:~:text=Cosine%20similarity%20is%20a%20metric,in%20a%20multi%2Ddimensional%20space.)