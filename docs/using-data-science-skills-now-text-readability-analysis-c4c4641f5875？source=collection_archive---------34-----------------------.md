# 现在使用数据科学技能:文本可读性分析

> 原文：<https://towardsdatascience.com/using-data-science-skills-now-text-readability-analysis-c4c4641f5875?source=collection_archive---------34----------------------->

## 如何使用 python 识别阅读水平分数

![](img/1a3c2bb49c3d08b8ed15a40d6c887ef1.png)

图片来自[皮克斯拜](https://pixabay.com/?utm_source=link-attribution&utm_medium=referral&utm_campaign=image&utm_content=3048651)

当营销效果分析被开发时，内容阅读水平经常被忽视。这应该是分析的一部分还是你的模型的一部分？如果你认为是，你会如何轻松地将不同的文档或创意与当前的阅读水平联系起来？今天我将回顾如何使用 python 进行阅读水平分析并创建报告。我们将把英语作为主要语言。同样的技术可以用来确定你的其他内容的可读性，比如中型博客。

## 为什么可读性很重要

无论你是想让你的客户了解你的新服务的好处，还是想让他们了解你的医疗实践，你都需要你的受众阅读并理解这些内容。太难了，你的读者就放弃你了。太简单了，读者会觉得你在居高临下地和他们说话。你不仅想让你的读者舒服地阅读你的文本，这也是搜索引擎优化的一个关键考虑因素。

## 你的读者是谁？

了解和研究你的典型读者是很重要的。如果你需要专注于一个领域开始，考虑教育水平。估计你的读者受教育的年数，然后减去三到五年来估计平均阅读水平。

如果你不能完全确定听众的教育水平，该怎么办？一些行业经验法则会有所帮助。

得分在 4 到 6 年级的书面文本通常被认为是“容易阅读的”如果你试图简单地为大众解释复杂的概念，这是你的范围。想想一般的博客和教育材料。

得分为 7 到 9 分的书面材料被认为是一般难度。这些读者期待一些更复杂的词汇。他们认为他们可能需要重读一段文字才能完全理解。操作指南文章可能属于这一范围。

任何十年级及以上的课文都被认为是非常难的。这应该留给白皮书、技术写作或文学作品，当你确定你的读者“准备好”和/或期待它的时候。你的读者在阅读和吸收你的内容上花费了相当多的精力。你知道当你读一本让你精疲力尽的书时？这是那个范围。

## 有哪些局限性？

当心标题、列表和其他“非句子”这些会影响你的分数。此外，根据您使用的指标，等级级别可能会有所不同。这些分数作为分析文本的一般起点。您可以确定哪些内容需要进一步检查。

## 代码

许多商业产品会扫描你的内容，并提供可读性指标。当这些产品对您不可用或者有成百上千的文档时，您将需要创建一个脚本来自动化这个过程。

我这里有一个脚本，它执行常见的自动化任务。它读入一个文件夹中的所有文本文档，然后给它们打分。有关如何在多个文件夹和数据源中抓取文本的更多信息，请参见这篇相关文章。

[](/using-data-science-skills-now-text-scraping-8847ca4825fd) [## 现在使用数据科学技能:文本抓取

### 有一个繁琐的文档搜索任务？用 python 通过 5 个步骤实现自动化。

towardsdatascience.com](/using-data-science-skills-now-text-scraping-8847ca4825fd) 

您可以在分析中使用几个软件包，包括 [textstat](https://pypi.org/project/textstat/) 和[可读性](https://pypi.org/project/readability/)。在这个例子中，我将使用 textstat。使用起来很简单。

```
import textstat  # [https://pypi.org/project/textstat/](https://pypi.org/project/textstat/)
import os
import glob       # using to read in many files
import docx2txt   # because I'm reading in word docs# where is the folder with your content?
folderPath = '<path to your top folder>\\'# I want to search in all of the folders, so I set recursive=True
docs=[]
docs = glob.glob(folderPath + '/**/*.txt',recursive=True)
docs.extend(glob.glob(folderPath + '/**/*.docx',recursive=True))
#... keep adding whatever types you needprint(docs)# the language is English by default so no need to set the language# Loop through my docs
for doc in docs:
    if os.path.isfile(doc):
        text = docx2txt.process(os.path.join(folderPath,doc))

        print('Document:                     ' + doc)
        print('Flesch Reading Ease:          ' + str(textstat.flesch_reading_ease(text)))
        print('Smog Index:                   ' + str(textstat.smog_index(text)))
        print('Flesch Kincaid Grade:         ' + str(textstat.flesch_kincaid_grade(text)))
        print('Coleman Liau Index:           ' + str(textstat.coleman_liau_index(text)))
        print('Automated Readability Index:  ' + str(textstat.automated_readability_index(text)))
        print('Dale Chall Readability Score: ' + str(textstat.dale_chall_readability_score(text)))
        print('Difficult Words:              ' + str(textstat.difficult_words(text)))
        print('Linsear Write Formula:        ' + str(textstat.linsear_write_formula(text)))
        print('Gunning Fog:                  ' + str(textstat.gunning_fog(text)))
        print('Text Standard:                ' + str(textstat.text_standard(text)))
        print('*********************************************************************************')"""Flesch-Kincaid Grade Level = This is a grade formula in that a score of 9.3 means that a ninth grader would be able to read the document""""""Gunning Fog = This is a grade formula in that a score of 9.3 means that a ninth grader would be able to read the document.""""""SMOG - for 30 sentences or more  =This is a grade formula in that a score of 9.3 means that a ninth grader would be able to read the document.""""""Automated Readability Index = Returns the ARI (Automated Readability Index) which outputs a number that approximates the grade level needed to comprehend the text."""""" Coleman Liau Index = Returns the grade level of the text using the Coleman-Liau Formula.""""""Linsear = Returns the grade level using the Linsear Write Formula."""""" Dale Chall = Different from other tests, since it uses a lookup table of the most commonly used 3000 English words. Thus it returns the grade level using the New Dale-Chall Formula."""
```

我的四个文档的结果。首先是这个博客。第二个是这个博客，去掉了链接。第三篇是 USAToday 的文章，第四篇是去掉了页眉和照片的 USAToday 文章。

```
Document:                     C:...readability\sampleblog.docx
Flesch Reading Ease:          56.59  (Fairly Difficult)
Smog Index:                   13.8
Flesch Kincaid Grade:         11.1
Coleman Liau Index:           11.9
Automated Readability Index:  14.1
Dale Chall Readability Score: 7.71 (average 9th or 10th-grade student)
Difficult Words:              124
Linsear Write Formula:        10.833333333333334
Gunning Fog:                  12.86
Text Standard:                10th and 11th grade
*********************************************************************************
Document:                     C:...readability\sampleblognolinks.docx
Flesch Reading Ease:          58.52 (Fairly Difficult)
Smog Index:                   12.9
Flesch Kincaid Grade:         10.3
Coleman Liau Index:           10.5
Automated Readability Index:  12.2
Dale Chall Readability Score: 7.48
Difficult Words:              101
Linsear Write Formula:        10.833333333333334
Gunning Fog:                  11.95
Text Standard:                10th and 11th grade
*********************************************************************************
Document:                     C:...readability\usatoday article no header photos.docx
Flesch Reading Ease:          21.47 (Very Confusing)
Smog Index:                   19.8
Flesch Kincaid Grade:         24.6
Coleman Liau Index:           13.19
Automated Readability Index:  32.2
Dale Chall Readability Score: 9.49
Difficult Words:              317
Linsear Write Formula:        16.25
Gunning Fog:                  27.01
Text Standard:                24th and 25th grade
*********************************************************************************
Document:                     C:...\readability\usatoday article.docx
Flesch Reading Ease:          21.47 (Very Confusing)
Smog Index:                   19.8
Flesch Kincaid Grade:         24.6
Coleman Liau Index:           13.19
Automated Readability Index:  32.2
Dale Chall Readability Score: 9.49
Difficult Words:              317
Linsear Write Formula:        16.25
Gunning Fog:                  27.01
Text Standard:                24th and 25th grade
*********************************************************************************
```

对于这个博客，分数受到移除链接的影响。USAToday 的文章没有受到移除标题和照片的影响。

## 参考资料和资源

*   **全国成人识字调查。**全国成人识字调查(NALS)显示了不同年龄、种族/民族和健康状况的人的识字水平。如果你知道你的读者的人口统计数据，你可以利用这次调查的数据来估计平均阅读水平。
*   社论:[避免稿件可读性差的循证指南](http://www.msera.org/docs/RITS_20_1_Readability.pdf)
*   卡格尔:

2019 年有一个有趣的比赛，我应用可读性分数来改善洛杉矶市的招聘信息。你可以看到不同的竞争者使用不同的技术。

[](https://www.kaggle.com/silverfoxdss/city-of-la-readability-and-promotion-nudges) [## 洛杉矶——可读性和推广

### 使用 Kaggle 笔记本探索和运行机器学习代码|使用来自多个数据源的数据

www.kaggle.com](https://www.kaggle.com/silverfoxdss/city-of-la-readability-and-promotion-nudges) 

## 结论

文本可读性是一个非常有趣的主题。了解谁是你的受众以及他们如何消费你的内容是非常重要的。忽略这一信息，你可能不会击中目标。