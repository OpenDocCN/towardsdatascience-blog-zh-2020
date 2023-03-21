# 文本数据扩充

> 原文：<https://towardsdatascience.com/text-data-augmentation-f4143571ecd2?source=collection_archive---------26----------------------->

## [自然语言处理](https://towardsai.net/p/category/nlp)

## 用于文本数据扩充的 TextAttack 和 Googletrans 简介

![](img/8f7e5a6fc43926a0bc1be2c2570591be.png)

照片由[尤金尼奥·马佐尼](https://unsplash.com/@eugi1492)在 [Unsplash](https://unsplash.com/photos/6ywyo2qtaZ8) 上拍摄

`Data Augmentation`是使我们能够增加训练数据的大小而无需实际收集数据的过程。但是为什么我们需要更多的数据呢？答案很简单——我们拥有的数据越多，模型的性能就越好。

图像数据增加步骤，例如翻转、裁剪、旋转、模糊、缩放等。对计算机视觉有很大的帮助。此外，创建增强图像相对容易，但由于语言固有的复杂性，`Natural Language Processing (NLP)`就不一样了。(例如，我们不能用同义词替换每个单词，即使我们替换了它，句子的意思也可能完全改变)。根据我的发现/研究，我还没有看到像图像增强那样多的围绕文本数据增强的研究。

然而，在这篇文章中，我们将浏览两个库`TextAttack` & `Googletrans`，这是我最近在尝试扩充文本数据时遇到的。那么，我们开始吧。

我们将应用我们将要学习的增强技术来引用西蒙·西内克的话。

> “领导力需要两样东西:对尚不存在的世界的愿景，以及传达这种愿景的能力。”
> 
> “领导者的角色不是提出所有伟大的想法。领导者的角色是创造一个可以产生伟大想法的环境”
> 
> 西蒙·西内克

# 文本攻击

`[TextAttack](https://github.com/QData/TextAttack)`是一个 Python 框架，用于 NLP 中的对抗性攻击、对抗性训练和数据扩充。在本文中，我们将只关注数据扩充。

## **安装**

```
!pip install textattack
```

## **用途**

`textattack.Augmenter`类提供了六种数据扩充方法。你可以在[官方文档](https://github.com/QData/TextAttack#augmenting-text-textattack-augment)中找到这些方法的详细描述。

1.  wordnet 增强器
2.  嵌入增强器
3.  CharSwapAugmenter
4.  简易数据增强器
5.  检查表增强器
6.  克莱尔增强器

让我们看看使用这四种方法的数据增强结果。注意，默认情况下，`pct_words_to_swap=0.1`、`transformations_per_example=4`被传递给这些方法中的每一个。我们可以根据需要修改这些默认值。

我们可以将这些方法应用于真实世界的数据，以增加数据的大小。下面给出了示例代码。这里，原始的`train`数据帧被复制到`train_aug`数据帧，然后在`train_aug`上应用增强。最后，`train_aug`被附加到原始的`train`数据集。

```
train_aug = train.copy()from textattack.augmentation import EmbeddingAugmenter
aug = EmbeddingAugmenter()

train_aug['text'] = train_aug['text'].apply(lambda x: str(aug.augment(x)))

train = train.append(train_copy, ignore_index=True)
```

# Googletrans

`Googletrans`建立在谷歌翻译 API 之上。这使用 [Google Translate Ajax API](https://translate.google.com/) 进行语言检测和翻译。

## **安装**

```
!pip install googletrans
```

## 使用

`translate()`方法的关键参数是:

`src`:源语言。可选参数如`googletrans`将检测它。

`dest`:目的语言。强制参数。

`text`:要从源语言翻译成目标语言的文本。强制参数。

我们可以看到，给定的文本首先从`English`翻译到`Italian`，然后再翻译回`English`。在这个反向翻译中，我们可以看到，原文和回译文本之间的句子有轻微的变化，但句子的整体意思仍然保留。

我们可以将这种技术应用于真实世界的数据。下面给出了示例代码。这里，原始的`train`数据帧被复制到`tran_aug` 数据帧，然后`back-translation`被应用到`train_aug`数据帧。最后，`train_aug`被附加到原始的`train`数据帧上。请注意，我们将原文从英语翻译成意大利语，然后从意大利语翻译成英语。

```
train_aug = train.copy()

from googletrans import Translator
translator = Translator()

train_aug['text'] = train_aug['text'].apply(lambda x: translator.translate(translator.translate(x, dest='it').text, dest='en').text)

train = train.append(train_aug, ignore_index=True)
```

# 结论

现在您知道了如何在您的数据科学项目中使用`TextAttack`和`Googletrans`库来扩充文本数据。

*阅读更多关于 Python 和数据科学的此类有趣文章，* [***订阅***](https://pythonsimplified.com/home/) *到我的博客*[***【www.pythonsimplified.com】***](http://www.pythonsimplified.com/)***。*** 你也可以通过[**LinkedIn**](https://www.linkedin.com/in/chetanambi/)**联系我。**

# 参考

[](https://github.com/ssut/py-googletrans) [## ssut/py-googletrans

### Googletrans 是一个免费且无限制的 python 库，实现了 Google Translate API。这使用谷歌…

github.com](https://github.com/ssut/py-googletrans) [](https://github.com/QData/TextAttack) [## QData/TextAttack

### 为 NLP 模型生成对抗示例[text attack docs on ReadTheDocs]关于*设置*使用*设计…

github.com](https://github.com/QData/TextAttack) [](/data-augmentation-in-nlp-2801a34dfc28) [## 自然语言处理中的数据扩充

### 文本增强简介

towardsdatascience.com](/data-augmentation-in-nlp-2801a34dfc28)