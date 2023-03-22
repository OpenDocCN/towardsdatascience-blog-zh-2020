# 3 Python 中数据科学与软件工程的算法面试问题

> 原文：<https://towardsdatascience.com/3-algorithm-interview-questions-for-data-science-and-software-engineering-in-python-29fc86a07a6f?source=collection_archive---------14----------------------->

## 熟悉常见的算法问题，为下一次面试做好准备

![](img/c80894b9cfbf0f3ab10f862d42e74b8c.png)

图片来自 [Pixabay](https://pixabay.com/?utm_source=link-attribution&utm_medium=referral&utm_campaign=image&utm_content=3373119) 的 [Tumisu](https://pixabay.com/users/Tumisu-148124/?utm_source=link-attribution&utm_medium=referral&utm_campaign=image&utm_content=3373119)

我上一次数据科学面试 90%都是 python 算法问题。

虽然你应该准备好解释一个 p 值，你也应该准备好回答传统的软件工程问题。

算法问题是一种可以学习的技能，公司用它来淘汰没有准备好的候选人。

下面是 3 个常见的算法问题和答案，在难度谱的容易端。

# 问题 1:检查两个字符串是否是字谜

变位词是通过重新排列另一个字符串中的字符而创建的字符串。

## 说明:

1.  给定两个字符串，`s1`和`s2`，如果它们互为变位词，则返回`True`。
2.  必须使用所有字符。

## 代码:

*把这个复制到本地的一个代码编辑器里，写一个函数解决这个问题。*

```
def anagram(s1, s2):
  #
  # your code here
  #
```

## 检查您的答案:

*运行此程序以确认您的功能按预期运行。*

```
assert anagram('word', 'wodr')
assert not anagram('dog', 'dogg')
assert anagram('racecar', 'carrace')
```

## 解决方案:

这是一个解决方案，但不是唯一的解决方案。

```
def anagram(s1, s2):
    c1 = {}
    c2 = {}

    def count_chars(s):
        h = {}
        for char in s:
            if char in h:
                h[char] += 1
            else:
                h[char] = 1
        return hreturn count_chars(s1) == count_chars(s2)
```

上面，我们用每个字符串中的字符数创建了字典，然后比较字典的相等性。

时间复杂度是 O(n) ,因为字符串迭代和字典查找依赖于输入字符串的长度。

# 问题 2:找出添加到句子中的单词

## 说明:

1.  给定一个句子，`sent1`，以及带有附加词的同一个句子，`sent2`，返回附加词。
2.  每一句话，`s1`，至少会有 1 个单词。

## 代码:

```
def extra_word(sent1, sent2):
  #
  # your code here
  #
```

## 检查您的答案:

```
assert extra_word('This is a dog', 'This is a fast dog') == 'fast'
assert extra_word('answer', 'The answer') == 'The'
assert extra_word('Can you solve algorithm questions', 'Can you solve hard algorithm questions') == 'hard'
```

## 解决方案:

```
def extra_word(sent1, sent2):
    words = {}

    for word in sent1.split(' '):
        if word in words:
            words[word] += 1
        else:
            words[word] = 1

    for word in sent2.split(' '):
        if word in words:
            words[word] -= 1
        else:
            return word
```

上面，我们通过字典统计了第一句话的单词。然后从同一本字典中减去第二句话中的单词。不在字典中的单词是要返回的单词。

时间复杂度是 **O(n)** 因为我们对每个句子迭代一次。

# 问题 3:嘶嘶的嗡嗡声

这是经典的 [fizzbuzz 面试问题](https://news.ycombinator.com/item?id=20778669)。

## 说明:

1.  给定`n`，返回从`1`到`n`的每个整数的值列表。
2.  如果一个数能被 3 整除，则返回`'Fizz'`代替它。
3.  如果这个数能被 5 整除，则返回`'Buzz'`代替它。
4.  如果数字能被 3 和 5 整除，则返回`'FizzBuzz'`代替它。

## 代码:

```
def fizzbuzz(n):
  #
  # your code here
  #
```

## 检查您的答案:

```
output = fizzbuzz(1000)assert output[8] == 'Fizz'
assert output[99] == 'Buzz'
assert output[198] == 199
assert output[510] == 511
assert output[998] == 'Fizz'
```

## 解决办法

```
def fizzbuzz(n):
    answer = []
    for i in range(1,n+1):
        if i % 3 == 0 and i % 5 == 0:
            answer.append("FizzBuzz")
        elif i % 3 == 0:
            answer.append("Fizz")
        elif i % 5 == 0:
            answer.append("Buzz")
        else:
            answer.append(i)
    return answer
```

上面，我们创建了一个给定值列表`n`。然后遍历每个值，并将值 Fizz、Buzz 或 FizzBuzz 添加到列表中。

时间复杂度是 **O(n)** 因为我们遍历列表一次。

# 结论

*您是否对每周 5 个算法问题和答案的系列感兴趣？请在评论中告诉我。*

至于算法问题，这些都很简单，并且都可以在 O(n)时间复杂度内解决。但是如果你不熟悉这类问题，最好从基础开始。

在可预见的未来，算法问题将成为数据科学和软件工程面试的一部分。掌握这项技能的最好方法是每周做几道题。这样，如果你需要申请新的工作，你就可以随时准备好。