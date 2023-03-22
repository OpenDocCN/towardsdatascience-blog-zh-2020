# 使用 NLTK ft 构建文本规范化器。POS 标签

> 原文：<https://towardsdatascience.com/building-a-text-normalizer-using-nltk-ft-pos-tagger-e713e611db8?source=collection_archive---------19----------------------->

标记化和文本规范化是自然语言处理技术的两个最基本的步骤。文本规范化是将单词转换成其词根或基本形式以标准化文本表示的技术。请注意，基本形式不一定是有意义的词，我们将在本博客结束时看到这一点。这些基本形式被称为词干，这个过程被称为**词干**。一种专门的获取词干的方法叫做**词汇化**，它根据单词所属的词类(POS)家族使用规则。

![](img/2251f3023358fd743b21460b9ba536ea.png)

照片由 [Jelleke Vanooteghem](https://unsplash.com/@ilumire?utm_source=medium&utm_medium=referral) 在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 拍摄

# **为什么文本规范化很重要？**

文本规范化对于信息检索(IR)系统、数据或文本挖掘应用以及 NLP 管道是至关重要的。为了促进更快和更有效的信息检索系统，索引和搜索算法需要不同的单词形式——派生的[或屈折的](https://en.wikipedia.org/wiki/Morphological_derivation)[——简化为它们的规范化形式。此外，它还有助于节省磁盘空间和语际文本匹配。](https://en.wikipedia.org/wiki/Inflection)

# 词干化与词汇化

推导*词条*的过程处理单词所属的语义、形态和词性(POS ),而*词干*指的是一种粗略的启发式过程，它砍掉单词的词尾，希望在大多数时候都能正确实现这一目标，并且通常包括去除派生词缀。这就是为什么在大多数应用程序中，词汇化比词干化有更好的性能，尽管缩小到一个决策需要更大的视野。

在 Python 的 NLTK 库中， [nltk.stem](https://www.nltk.org/api/nltk.stem.html) 包同时实现了词干分析器和词汇分析器。

> 够了！现在让我们深入研究一些文本规范化代码…
> 
> 会出什么问题呢？？！

*注:* [*这里的*](https://github.com/royn5618/Medium_Blog_Codes/blob/master/Text_Normalization_ft_POS_Tagger.ipynb) *是 GitHub 上完整的 jupyter 笔记本。*

![](img/f69d7a2dab68a8700e2e3033eff71507.png)

在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上由 [Max Duzij](https://unsplash.com/@max_duz?utm_source=medium&utm_medium=referral) 拍照

**第一步:标记化**

```
**import** nltk
**import** stringhermione_said = '''Books! And cleverness! There are more important things - friendship and bravery and - oh Harry - be careful!'''*## Tokenization***from** nltk **import** sent_tokenize, word_tokenize
sequences = sent_tokenize(hermione_said)
seq_tokens = [word_tokenize(seq) for seq in sequences]*## Remove punctuation*no_punct_seq_tokens = []
**for** seq_token **in** seq_tokens:
    no_punct_seq_tokens.append([token for token in seq_token if token not in string.punctuation])print(no_punct_seq_tokens)**Output:**
[['Books'],
 ['And', 'cleverness'],
 ['There',
  'are',
  'more',
  'important',
  'things',
  'friendship',
  'and',
  'bravery',
  'and',
  'oh',
  'Harry',
  'be',
  'careful']]
```

> 太棒了。

**第二步:标准化技术**

我们从词干开始:

```
# Using Porter Stemmer implementation in nltk
**from** nltk.stem **import** PorterStemmer
stemmer = PorterStemmer()
stemmed_tokens = [stemmer.stem(token) **for** seq **in** no_punct_seq_tokens **for** token **in** seq]
print(stemmed_tokens)**Output:**
['book',
 'and',
 'clever',
 'there',
 'are',
 'more',
 '**import**',
 'thing',
 'friendship',
 'and',
 '**braveri**',
 'and',
 'oh',
 '**harri**',
 'be',
 'care']
```

> ***于是我们有了第一个问题——***在 16 个令牌中，高亮显示的 3 个并不好看！

所有这三个都是粗略的启发式过程的产物，但是“Harry”到“harri”是误导性的，尤其是对于 NER 应用程序，并且对于“import”来说“重要的”是信息丢失。“勇敢”到“勇敢”是一个词干的例子，它没有任何意义，但仍然可以在信息检索系统中用于索引。一个更好的例子是——争论，争论，争论——所有这些都变成了“争论”,保留了原始单词的意思，但它本身在英语词典中没有任何意义。

现在，让我们来假设一下:

```
**from** nltk.stem.wordnet **import** WordNetLemmatizer
**from** nltk.corpus **import** wordnetlm = WordNetLemmatizer()
lemmatized_tokens = [lm.lemmatize(token) **for** seq **in** no_punct_seq_tokens **for** token **in** seq]
print(lemmatized_tokens)Output:
['**Books**',
 'And',
 'cleverness',
 'There',
 'are',
 'more',
 '***important***',
 'thing',
 'friendship',
 'and',
 '***bravery***',
 'and',
 'oh',
 '***Harry***',
 'be',
 'careful']
```

这里，前面三个词干不正确的单词看起来更好。但是，“书”应该像“物”之于“物”一样成为“书”。

> ***这是我们的第二个问题。***

现在，WordNetLemmatizer.lemma()接受一个参数 **pos** 来理解单词/令牌的 pos 标签，因为单词形式可能相同，但上下文或语义不同。例如:

```
print(lm.lemmatize("Books", pos="n"))
Output: 'Books'print(lm.lemmatize("books", pos="v"))
Output: 'book'
```

> 因此，从 POS tagger 获得帮助似乎是一个方便的选择，我们将继续使用 POS tagger 来解决这个问题。

但在此之前，我们先来看看用哪个——stemmer？lemmatizer？两者都有？要回答这个问题，我们需要后退一步，回答如下问题:

*   问题陈述是什么？
*   哪些功能对解决问题陈述很重要？
*   这个特性会增加计算和基础设施的开销吗？

上面的例子是 NLP 从业者在处理数百万代币的基本形式时面临的更大挑战的一个简单尝试。此外，在处理庞大的语料库时保持精度，并进行额外的检查，如词类标记(在这种情况下)、NER 标记、匹配单词包中的标记(BOW)以及拼写纠正，这些都是计算开销很大的。

**步骤 3: POS 标签员进行救援**

让我们对已经词干化和词汇化的标记应用 POS tagger 来检查它们的行为。

```
**from** nltk.tag **import** StanfordPOSTagger

st = StanfordPOSTagger(path_to_model, path_to_jar, encoding=’utf8')## Tagging Lemmatized Tokenstext_tags_lemmatized_tokens = st.tag(lemmatized_tokens)
print(text_tags_lemmatized_tokens)Output:
[('Books', '***NNS***'),
 ('And', 'CC'),
 ('cleverness', 'NN'),
 ('There', 'EX'),
 ('are', 'VBP'),
 ('more', 'RBR'),
 ('important', 'JJ'),
 ('thing', 'NN'),
 ('friendship', 'NN'),
 ('and', 'CC'),
 ('bravery', 'NN'),
 ('and', 'CC'),
 ('oh', 'UH'),
 ('Harry', '***NNP***'),
 ('be', 'VB'),
 ('careful', 'JJ')] ## Tagging Stemmed Tokenstext_tags_stemmed_tokens = st.tag(stemmed_tokens)
print(text_tags_stemmed_tokens)Output:
[('book', '***NN***'),
 ('and', 'CC'),
 ('clever', 'JJ'),
 ('there', 'EX'),
 ('are', 'VBP'),
 ('more', 'JJR'),
 ('import', 'NN'),
 ('thing', 'NN'),
 ('friendship', 'NN'),
 ('and', 'CC'),
 ('braveri', 'NN'),
 ('and', 'CC'),
 ('oh', 'UH'),
 ('harri', '***NNS***'),
 ('be', 'VB'),
 ('care', 'NN')]
```

> 发现突出的差异？

*   第一个标记——**书**——带词干的标记是 NN(名词)，带词干的标记是 NNS(复数名词)，比较具体。
*   第二， **Harry —** 错误地源于 *harri* ，结果词性标注者未能正确地将其识别为专有名词。另一方面，词汇化的标记正确地将*哈利*分类，并且词性标注者明确地将其识别为专有名词。
*   最后， **harri** 和**braveri**——尽管这些词在英语词汇词典中没有任何位置，但它们应该被归类为 FW。这个我现在没有解释。

**步骤 4:为令牌标记构建 POS 映射器**

我首先创建了如下的 POS 映射器。该映射器用于根据树库位置标签代码到 wordnet 的参数。

```
**from** nltk.corpus.reader.wordnet **import** VERB, NOUN, ADJ, ADVdict_pos_map = {
    # Look for NN in the POS tag because all nouns begin with NN
    'NN': NOUN,
    # Look for VB in the POS tag because all nouns begin with VB
    'VB':VERB,
    # Look for JJ in the POS tag because all nouns begin with JJ
    'JJ' : ADJ,
    # Look for RB in the POS tag because all nouns begin with RB
    'RB':ADV  
}
```

**步骤 5:在解决问题的同时构建规格化器**

1.  识别专有名词，跳过处理并保留大写字母
2.  识别单词的词性标签所属的词性家族——NN、VB、JJ、RB，并传递正确的变元化参数
3.  获取词汇化记号的词干。

这是最后的代码。我使用 st.tag_sents()来保持序列的顺序(逐句嵌套的标记)

**带词干**

```
normalized_sequence = []
**for** each_seq **in** st.tag_sents(sentences=no_punct_seq_tokens):
    normalized_tokens = []
    **for** tuples **in** each_seq:
        temp = tuples[0]
        **if** tuples[1] == "NNP" **or** tuples[1] == "NNPS":
            **continue**
        **if** tuples[1][:2] **in** dict_pos_map.keys():
            temp = lm.lemmatize(tuples[0].lower(), 
                                pos=dict_pos_map[tuples[1][:2]])
        temp = stemmer.stem(temp)
        normalized_tokens.append(temp)
    normalized_sequence.append(normalized_tokens)
normalized_sequence**Output:**
[['book'],
 ['and', 'clever'],
 ['there',
  'be',
  'more',
  'import',
  'thing',
  'friendship',
  'and',
  'braveri',
  'and',
  'oh',
  'be',
  'care']]
```

**不带词干**

```
normalized_sequence = []
for each_seq in st.tag_sents(sentences=no_punct_seq_tokens):
    normalized_tokens = []
    for tuples in each_seq:
        temp = tuples[0]
        if tuples[1] == "NNP" or tuples[1] == "NNPS":
            continue
        if tuples[1][:2] in dict_pos_map.keys():
            temp = lm.lemmatize(tuples[0].lower(), 
                                pos=dict_pos_map[tuples[1][:2]])
        normalized_tokens.append(temp)
    normalized_sequence.append(normalized_tokens)
normalized_sequence**Output:**[['book'],
 ['And', 'cleverness'],
 ['There',
  'be',
  'more',
  'important',
  'thing',
  'friendship',
  'and',
  'bravery',
  'and',
  'oh',
  'be',
  'careful']]
```

在这个过程中，我还有两个重要的不同之处，一是源于进口，二是源于勇敢。虽然“braveri”相当明确，但“import”却是一个挑战，因为它可能意味着不同的东西。这就是为什么，一般来说，词干化比词元化具有更差的性能，因为它增加了搜索词的歧义性，因此检测到实际上并不相似的相似性。

*注:* [*这里的*](https://github.com/royn5618/Medium_Blog_Codes/blob/master/Text_Normalization_ft_POS_Tagger.ipynb) *是 GitHub 上完整的 jupyter 笔记本。*

[1][https://NLP . Stanford . edu/IR-book/html/html edition/stemming-and-lemma tization-1 . html](https://nlp.stanford.edu/IR-book/html/htmledition/stemming-and-lemmatization-1.html)