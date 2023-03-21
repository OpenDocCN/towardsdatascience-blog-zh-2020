# 简单 Python 中的 Sardinas-Patterson 算法

> 原文：<https://towardsdatascience.com/the-sardinas-patterson-algorithm-in-simple-python-9718242752c3?source=collection_archive---------30----------------------->

## 检查可变长度代码的唯一可解码性

![](img/400c146b29264e9c38bdc3053bce107e.png)

图片来自[皮克斯拜](https://pixabay.com/?utm_source=link-attribution&utm_medium=referral&utm_campaign=image&utm_content=4031973)

在关于数据科学的对话中，两个经常被冷落的领域是 [**信息论**](https://en.wikipedia.org/wiki/Information_theory) **，**研究信息的量化、存储和交流， [**编码理论**](https://en.wikipedia.org/wiki/Coding_theory) ，研究代码的属性及其各自对特定应用的适用性。加雷思·a·琼斯和 j·玛丽·琼斯的《信息和编码理论》是对这两个领域的精彩介绍。几年来，我一直把这本书列在我的待办事项清单上(我是在遇到作者的另一本书[初等数论](https://link.springer.com/book/10.1007/978-1-4471-0613-5)后添加的，在宾夕法尼亚州立大学数学 465 中)，最近决定打开它，看看它到底是关于什么的。

如题，这篇帖子讲的是[撒丁纳斯-帕特森算法](https://en.wikipedia.org/wiki/Sardinas%E2%80%93Patterson_algorithm) (书中定义为*撒丁纳斯-帕特森定理*)，可以用来判定一个变长码是否唯一可解码。

在我们深入研究算法之前，我们需要给出一些定义。出于可读性的考虑，我将在这里做一些演示，但更严格的定义和所有代码可以在我的 Github 上的[笔记本](https://github.com/danhales/blog-sardinas-patterson)中找到。

## 定义

出于我们的目的，我们可以把一个**代码** C 想象成一组独特的码字，这些码字由一些**源字母表**中的符号构成。在这篇文章中，我将把我的源字母表限制为{0，1，2}，只是为了保持简单，但是我们将看到的代码可以处理更多。**本例中的码字**将是由至少一个数字组成的字符串，如 012、1201021，甚至只是 0。

码字的**长度**是该码字中使用的符号序列的长度。例如，码字 0012 的长度是 4，因为它由序列(0，0，1，2)组成。

在许多频道上，没有指示一个码字在哪里结束，下一个码字在哪里开始——这是我们问题的核心。我们得到一串字母，只有代码本身的属性允许我们确定是否可以恢复编码的信息。例如，我们可能需要解释以下符号串:

1202120

如果从代码字构造的每个符号串(例如上面的串)可以被唯一地分解成恰好一个代码字序列，则代码是唯一可解码的。可以很容易地证明，所有字都具有固定长度的代码是唯一可解码的，但是我们对更困难的问题感兴趣——确定码字具有可变长度的代码是否是唯一可解码的。

让我们看几个可能在字符串 1202120 中产生的不同代码，来说明我们所说的“唯一可解码性”是什么意思

## **唯一可解码的代码**

首先，假设 C = {120，21，20}。

我们可以通过找到共享第一个字母的码字来开始解码消息 1202120，在这种情况下，120 也以 1 开始，因此我们可以将该消息“因式分解”为 120.2120。

接下来，我们移动到下一个符号 120。 **2** 120，并检查是否有任何码字以 2 开头——我们有两个码字，21 和 20，所以我们看看是否找到下一个符号的匹配:120。 **21** 20。

答对了。

21 是一个有效的代码字，幸运的是，接下来的两个符号 20 也是。我们可以将此消息因式分解为 120.21.20。

## **不可唯一解码的代码**

现在，假设 C = {02，12，120，21，20}。

使用与上面相同的过程，我们可以得到 1202120 的两种不同的因式分解:

12.02.120

120.21.20

因为我们的频道没有告诉我们一个单词在哪里结束，下一个单词在哪里开始，所以我们不知道该用哪个。在特定情况下，这可能是毁灭性的。例如，假设我们像这样编码重要的英语单词:

02 →不是

12 →还没有

120 →清除

21 →至

20 →引爆

我们的信息可以被解读为:

还不清楚

或者

准备引爆

我会让你的想象力来填补这个不幸的场景，既导致这种混乱，又在误译后展开。

![](img/1b4e9b7e23ae2c0e298eff225e42a2a2.png)

图片来自 [Pixabay](https://pixabay.com/?utm_source=link-attribution&utm_medium=referral&utm_campaign=image&utm_content=417894)

为了避免这样的歧义，我们需要确保我们使用的代码字集是唯一可解码的。为此，我们有**萨丁纳斯-帕特森算法**。

## 定义 C(n)

我将试图说明这个算法是最简单的，最不数学的英语，但是我们需要建立我们的方法来达到它。

首先，提醒一下:

C 是我们感兴趣测试的一组代码字。当我们对我们的信息进行编码时，这就是我们映射到的单词集。在我们上面的非唯一可解码的例子中，C 是{02，12，120，21，20}。我们让 **C(0)** 等于 **C** ，我们将归纳定义一个新集合的序列 C(1)，C(2)，…。请记住，C(n)并不代表 *C* 作为 *n，*的函数，而是下标 C_n 的索引。括号是为了以后的可读性。

> **C(1)** 是一组*后缀*，可以加到 C 中的单词上，以便得到 C 中的另一个单词

例如，0 可以加到 12 上得到 120，所以 0 在 C(1)中。事实上，这是唯一的*后缀*，它可以添加到 C 中的一个单词，以创建 C 中的另一个单词，因此 C(1) = {0}。下面的代码演示了这一点，它找到了所有可以附加到“a”以在集合{a，an，apple}中创建另一个单词的后缀。

```
c = set(['a', 'an', 'apple'])
c1 = set()for u in c:
    for v in c:
        if len(u) > len(v) and u.find(v) == 0:
            c1.add(u[len(v):])
c1
# {'n', 'pple'}
```

我们在外循环中遍历`c`中的单词，然后(在这种情况下)在内循环中再次遍历单词(当你到达那里时，与下面的例子进行比较)。在每一步，我们检查内部循环的单词是否是代码中的一个单词的开头，如果是，我们就给`c1`加上后缀。

现在我们已经定义了 C、C(0)和 C(1)，我们可以归纳定义 C(n)的更一般的定义:

C(n)是所有后缀的集合，这些后缀可以被添加到 C 中的原始码字之一以创建 C(n-1)中的后缀，或者可以被添加到 C(n-1)中的后缀以创建原始码字之一。

继续我们的 C = {02，12，120，21，20}和 C(1) = {0}的例子，C(2)是可以添加到 C 中的单词以创建“0”的所有后缀的集合(没有后缀)，或者是可以添加到 C(1)中的后缀以从 C 创建单词的后缀的集合——唯一的元素是“2”，它可以添加到“0”以创建 02。这意味着 C(2) = {2}。

诸如此类。我们可以将它包装在一个函数中，该函数接受 C 和 n，生成 C(n-1)，并执行与上面的函数相同的比较。因此，我们有`generate_cn(c, n)`，我们可以用它来计算 C(3)、C(4)、C(5)和 C(6):

```
def generate_cn(c, n):
    if n == 0:
        return set(c)
    else:
        # create a set to hold our new elements
        cn = set()

        # generate c_(n-1)
        cn_minus_1 = generate_cn(c, n-1)

        for u in c:
            for v in cn_minus_1:
                if (len(u) > len(v)) and u.find(v) == 0:
                    cn.add(u[len(v):])
        for u in cn_minus_1:
            for v in c:
                if len(u) > len(v) and u.find(v) == 0:
                    cn.add(u[len(v):])
        return cnfor i in [3, 4, 5, 6]:
    print(generate_cn(set(['02', '12', '120', '20', '21']), i))# {'0', '1'}
# {'20', '2'}
# {'0', '1'}
# {'20', '2'}
```

一般逻辑与我们检查的第一段代码相同，但是现在我们必须检查 c 和 c(n-1)中的前缀。

注意在输出中，对于我们的非唯一代码，我们最终会遇到一个输出循环；这是我们在构建撒丁岛-帕特森算法语句所需的最终集合时需要小心处理的事情。

在继续之前，还值得注意的是，如果 C(n)为空，那么 C(n+1)也将为空。考虑下面的代码，它构造了 C(0)、C(1)和 C(2)，对于代码{'0 '、' 1 '、' 21'}:

```
c = set(['0’, '1', '21'])for n in range(2):
    print(‘C({})’.format(n), ‘\t’, generate_cn(c, n))#C(0) {‘0’, ‘1’, ‘21’}
#C(1) set()
#C(2) set()
```

在构造 C(1)时，没有后缀可以附加到 C(0)中的任何单词上，以从 C(1)创建另一个单词。因此，在 C(2)中我们没有什么可追加的。可以想象，这意味着 C(3)，C(4)，等等也将是空的。

## 定义(C∞)

理解撒丁纳斯-帕特森算法的最后一块拼图如下:

**C(∞)** :所有 C(n)的并集，其中 n ≥1。

> C(∞)可以被认为是所有可能的后缀的集合，这些后缀从在解码中产生歧义的原始码字中导出。

我们将通过构造 C(1)、C(2)、C(3)等等来计算它，直到发生以下两种情况之一:

1.  C(n)为空，在这种情况下 C(n+1)将为空，在这种情况下 C(n+2)将为空…以此类推。
2.  我们遇到一个 C(n ),它等于先前计算的某个 C(m ),这表明一个类似上面看到的循环。

可以证明，其中一个最终会发生；直观来看，一个有限的源字母表，一个有限的 C，拆原字的方式也一定是有限的。

考虑到这些因素，我们有以下函数:

```
def generate_c_infinity(c):
    cs = []
    c_infinity = set()
    n = 1
    cn = generate_cn(c, n)
    print('c_{}'.format(n), cn)while len(cn) > 0:
        if cn in cs:
            print('Cycle detected. Halting algorithm.')
            break
        else:
            cs.append(cn)
            c_infinity = c_infinity.union(cn)
            n += 1
            cn = generate_cn(c, n)
            print('c_{}'.format(n), c_infinity)
    return c_infinity

c = set(['02', '12', '120', '20', '21'])
generate_c_infinity(c)
```

为了检测循环，我们将每个 C(n)存储在一个 Cs 列表中。如果我们遇到一个以前见过的 C(n ),我们就中断循环，如果我们遇到任何空的 C(n ),循环前提条件就失败。

考虑到这一点，加上我们工具包中的这些函数，我们有足够的拼图块来最终组合成唯一可解码性的必要和充分条件，这就是所谓的[萨丁纳斯-帕特森算法](https://en.wikipedia.org/wiki/Sardinas%E2%80%93Patterson_algorithm)。

![](img/e92f76a3dd95884019bae3aa96e47c83.png)

马库斯·温克勒在 [Unsplash](https://unsplash.com/s/photos/jigsaw?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) 上的照片

## 撒丁纳斯-帕特森算法

**一个码 C 是唯一可解码的当且仅当集合 C 和 C(∞)不相交。**

稍微解释一下，记住每个 C(n)给我们一个可能产生歧义的后缀，就像我们在 not . yet . clear/clear . to . explode 例子中看到的那样。这个定理的另一种表述方式是

> "在原始代码字集中没有产生歧义的后缀."

使用我最喜欢的 set 的属性[，将它实现为一个函数非常简单:](https://medium.com/swlh/a-python-style-set-in-simple-java-698977f1b5d0)

```
def sardinas_patterson_theorem(c):
    """
    Returns True if c is uniquely decodable
    """
    c_infinity = generate_c_infinity(c)
    return len(c.intersection(c_infinity)) == 0def check_decodability(c):
    if sardinas_patterson_theorem(c):
        print(c, 'is uniquely decodable')
    else:
        print(c, 'is not uniquely decodable')c = set(['02', '12', '120', '20', '21'])
check_decodability(c)
```

我们做到了！判定一组码字是否唯一可解码的一个充要条件。

我已经尽了最大努力让普通读者远离大部分数学符号，但是如果你有兴趣阅读更正式的(并从书中整理和稍微改写)定义，或者如果你有兴趣深入笔记本本身，你可以在 [my github](https://github.com/danhales/blog-sardinas-patterson) 上找到它们。