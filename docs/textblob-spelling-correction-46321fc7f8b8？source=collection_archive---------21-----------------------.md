# TextBlob 拼写更正

> 原文：<https://towardsdatascience.com/textblob-spelling-correction-46321fc7f8b8?source=collection_archive---------21----------------------->

## 本文解释了如何使用 TextBlob 模块进行拼写纠正。

![](img/d4f5b77aeec903d7b52706e69964f834.png)

[真诚媒体](https://unsplash.com/@sincerelymedia?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)在 [Unsplash](https://unsplash.com/s/photos/text?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) 上拍摄的照片

## 什么是 TextBlob？

T extBlob 是一个用于处理文本数据的 Python 库。它提供了一个一致的 API，用于处理常见的自然语言处理(NLP)任务，如词性标注、名词短语提取、情感分析等。

## 为什么是 TextBlob？

> *NLU 是 NLP 的一个子集，其中非结构化数据或句子被转换成结构化形式，以在处理端到端交互方面执行 NLP。关系提取、语义解析、情感分析、名词短语提取是 NLU 的几个例子，它本身是 NLP 的子集。现在，在这些领域中，TextBlob 扮演了一个重要的角色，而 NLTK 并没有有效地做到这一点。*

## 使用 TextBlob 进行拼写更正

![](img/a8ed6014e53f0178a05d47f1dc5d22d2.png)

由 [Romain Vignes](https://unsplash.com/@rvignes?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) 在 [Unsplash](https://unsplash.com/s/photos/text?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) 上拍摄的照片

**步骤:1 →安装 TextBlob**

有时推文、评论或任何博客数据可能包含打字错误，因此首先我们需要纠正这些数据，以减少代表相同意思的相同单词的多个副本。

在你的电脑上安装 TextBlob 非常简单。你只需要使用 pip 安装它。

**步骤 2 →加载预处理输入**

我们必须从基础开始给计算机喂食，这样它才能在自然语言理解和处理方面得到很好的训练。

接下来，加载您需要正确拼写的输入文本(如 *docx* ),我们将要处理的文本是*多伦多的移民。*

这里，我们需要使用 regex 清理输入文本，因为我们不需要任何数字字符。

这里，清理后的文本输入

```
People have travelled through and inhabited the Toronto area, located on a broad sloping plateau interspersed with rivers, deep ravines, and urban forest, for more than , years. After the broadly disputed Torronto Purchase, when the Mississauga surrendered the area to the British Crown, the British established the town of York in and later designeted it as the capital of Upper Canada. During the War of , the town was the site of the Battle of York and suffered heavy damage by American troops. York was renamed and incorporated in as the city of Toronto. It was designated as the capitel of the province of Ontario in during Canadian Confederation. The city proper has since expanded past its original borders through both annexation and amalgamation to its current area of . km . sq mi . The diverse population of Tornto reflects its current and historical role as an important destination for immigrants to Canada. More than percent of residants belong to a visible minority population group, and over distinct ethnic origins are represented among its inhabitats. While the majority of Torontonians speak English as their premary language, over languages are spoken in the city. Toront is a prominent center for music, theatre, motion picture production, and tilevision production, and is home to the headquarters of Canada s major notional broadcast networks and media outlets. Its varied caltural institutions, which include numerous museums and gelleries, festivals and public events, entertaiment districts, national historic sites, and sports actevities, attract over million touriets each year. Torunto is known for its many skysvrapers and high rise buildinds, in particalar the tallest free standind structure in the Western Hemisphere, the CN Tower.
```

步骤 3 →识别拼错的单词

首先，识别拼写错误的标记，以纠正它们的拼写。为此，我们可以使用*拼写检查器*模块中的*拼写检查器*功能。

输出:

```
{'skysvrapers', 'entertaiment', 'standind', 'residants', 'rivers,', 'forest,', 'torronto', 'torunto', 'actevities,', 'touriets', 'toront', 'particalar', 'gelleries,', 'production,', 'designeted', 'purchase,', 'hemisphere,', 'area,', 'capitel', 'ravines,', 'premary', 'inhabitats.', 'ecents,', 'tilevision', 'toronto.', 'tornto', 'buildinds,', 'confederation.', 'sites,', 'caltural', 'theatre,', 'institutons,', 'language,', 'music,', 'troups.', 'torontonians', 'mississauga'}
```

**步骤 4 →纠错过程**

用 *TextBlob* 对象存储更新后的文本。

使用`[**correct()**](https://textblob.readthedocs.io/en/dev/api_reference.html#textblob.blob.TextBlob.correct)`方法尝试拼写纠正。

输出:

```
People have travelled through and inhabited the Toronto area, located on a broad sloping plateau interspersed with rivers, deep ravines, and urban forest, for more than , years. After the broadly disputed Torronto Purchase, when the Mississauga surrendered the area to the British Grown, the British established the town of Work in and later designed it as the capital of Upper Canada. During the War of , the town was the site of the Battle of Work and suffered heavy damage by American troops. Work was renamed and incorporated in as the city of Toronto. It was designate as the capital of the province of Ontario in during Canadian Confederation. The city proper has since expanded past its original borders through both annexation and amalgamation to its current area of . km . sq mi . The diverse population of Onto reflect its current and historical role as an important destination for immigrants to Canada. More than percent of residents belong to a visible minority population group, and over distinct ethnic origins are represented among its inhabitants. While the majority of Torontonians speak English as their primary language, over languages are spoken in the city. Front is a prominent center for music, theatre, motion picture production, and television production, and is home to the headquarters of Canada s major national broadcast network and media outlets. Its varied cultural institutions, which include numerous museums and galleries, festival and public events, entertainment districts, national historic sites, and sports activities, attract over million tories each year. Torunto is known for its many skysvrapers and high rise buildings, in particular the tables free standing structure in the Western Hemisphere, the of Power.
```

完整代码可在 [GitHub](https://github.com/Nishk23/TextBlob-Spelling-Correction) 获得

# 结论

我希望您现在已经理解了如何使用 TextBlob 模块进行拼写纠正。感谢阅读！