# 41 个问题来测试你对 Python 字符串的了解

> 原文：<https://towardsdatascience.com/41-questions-to-test-your-knowledge-of-python-strings-9eb473aa8fe8?source=collection_archive---------0----------------------->

## 如何通过掌握字符串基础来碾压算法题

![](img/99d0b0c746a4414a5a692a1f0e074432.png)

照片由[本工程 RAEng](https://unsplash.com/@thisisengineering?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) 在 [Unsplash](https://unsplash.com/?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) 上拍摄

我在 LeetCode 和 HackerRank 上做算法题的时候已经开始跟踪最常用的函数了。

成为一名优秀的工程师并不是要记住一门语言的功能，但这并不意味着它没有帮助。尤其是在面试中。

这是我的字符串 cheatsheet 转换成一个问题列表来测试自己。虽然这些不是面试问题，但是掌握这些将帮助您更轻松地解决现场编码问题。

你对 Python 字符串了解多少？

## 1.如何确认两个字符串具有相同的身份？

如果两个名字指向内存中的同一个位置,`is`操作符返回`True`。这就是我们谈论身份时所指的。

不要把`is`和`==,`混淆，后者只测试平等性。

```
animals           = ['python','gopher']
more_animals      = animalsprint(animals == more_animals) #=> True
print(animals is more_animals) #=> Trueeven_more_animals = ['python','gopher']print(animals == even_more_animals) #=> True
print(animals is even_more_animals) #=> False
```

请注意上面的`animals`和`even_more_animals`虽然相等，但身份却不同。

此外，`id()`函数返回与名称相关联的内存地址的`id`。具有相同身份的两个对象将返回相同的`id`。

```
name = 'object'
id(name)
#=> 4408718312
```

## 2.如何检查字符串中的每个单词是否以大写字母开头？

`istitle()`函数检查每个单词是否大写。

```
print( 'The Hilton'.istitle() ) #=> True
print( 'The dog'.istitle() ) #=> False
print( 'sticky rice'.istitle() ) #=> False
```

## 3.检查字符串是否包含特定的子字符串

如果一个字符串包含子串，`in`操作符将返回`True`。

```
print( 'plane' in 'The worlds fastest plane' ) #=> True
print( 'car' in 'The worlds fastest plane' ) #=> False
```

## 4.在字符串中查找子字符串第一次出现的索引

有两个不同的函数将返回起始索引，`find()`和`index()`。他们的行为略有不同。

如果没有找到子串，则`find()`返回`-1`。

```
'The worlds fastest plane'.find('plane') #=> 19
'The worlds fastest plane'.find('car') #=> -1
```

`index()`会抛出一个`ValueError`。

```
'The worlds fastest plane'.index('plane') #=> 19
'The worlds fastest plane'.index('car') #=> ValueError: substring not found
```

## 5.计算字符串中的字符总数

`len()`将返回一个字符串的长度。

```
len('The first president of the organization..') #=> 19
```

## 6.计算字符串中特定字符的个数

`count()`将返回特定字符出现的次数。

```
'The first president of the organization..'.count('o') #=> 3
```

## 7.将字符串的第一个字符大写

使用`capitalize()`功能来完成此操作。

```
'florida dolphins'.capitalize() #=> 'Florida dolphins'
```

## 8.什么是 f 弦，你如何使用它？

f 字符串是 python 3.6 中的新特性，它使得字符串插值变得非常容易。使用 f 弦类似于使用`format()`。

f 弦在开盘价前用一个`f`表示。

```
name = 'Chris'
food = 'creme brulee'f'Hello. My name is {name} and I like {food}.'
#=> 'Hello. My name is Chris and I like creme brulee'
```

## 9.在字符串的特定部分搜索子字符串

`index()`还可以提供可选的开始和结束索引，用于在更大的字符串中进行搜索。

```
'the happiest person in the whole wide world.'.index('the',10,44)
#=> 23
```

注意上面是如何返回`23`而不是`0`。

```
'the happiest person in the whole wide world.'.index('the')
#=> 0
```

## 10.使用 format()将变量插入字符串

`format()`类似于使用 f 弦。虽然在我看来，它对用户不太友好，因为变量都是在字符串的末尾传入的。

```
difficulty = 'easy'
thing = 'exam''That {} was {}!'.format(thing, difficulty)
#=> 'That exam was easy!'
```

## 11.检查字符串是否只包含数字

如果所有字符都是数字，则`isnumeric()`返回`True`。

```
'80000'.isnumeric() #=> True
```

注意标点符号不是数字。

```
'1.0'.isnumeric() #=> False
```

## 12.拆分特定字符上的字符串

`split()`函数将在给定的一个或多个字符上拆分一个字符串。

```
'This is great'.split(' ')
#=> ['This', 'is', 'great']'not--so--great'.split('--')
#=> ['not', 'so', 'great']
```

## 13.检查字符串是否全部由小写字符组成

仅当字符串中的所有字符都是小写时，`islower()`才返回`True`。

```
'all lower case'.islower() #=> True
'not aLL lowercase'.islower() # False
```

## 14.检查字符串中的第一个字符是否小写

这可以通过在字符串的第一个索引上调用前面提到的函数来完成。

```
'aPPLE'[0].islower() #=> True
```

## 15.Python 中的字符串可以加整数吗？

在一些语言中这是可以做到的，但是 python 会抛出一个`TypeError`。

```
'Ten' + 10 #=> TypeError
```

## 16.反转字符串“hello world”

我们可以将字符串拆分成一个字符列表，反转列表，然后重新组合成一个字符串。

```
''.join(reversed("hello world"))
#=> 'dlrow olleh'
```

## 17.将一系列字符串合并成一个字符串，用连字符分隔

Python 的`join()`函数可以连接列表中的字符，在每个元素之间插入一个给定的字符。

```
'-'.join(['a','b','c'])
#=> 'a-b-c'
```

## 18.检查字符串中的所有字符是否都符合 ASCII

如果字符串中的所有字符都包含在 ASCII 码中，`isascii()`函数返回`True`。

```
print( 'Â'.isascii() ) #=> False
print( 'A'.isascii() ) #=> True
```

## 19.整个字符串大写或小写

`upper()`和`lower()`返回所有大写和小写的字符串。

```
sentence = 'The Cat in the Hat'sentence.upper() #=> 'THE CAT IN THE HAT'
sentence.lower() #=> 'the cat in the hat'
```

## 20.字符串的第一个和最后一个大写字符

像在过去的例子中一样，我们将针对字符串的特定索引。Python 中的字符串是不可变的，所以我们将构建一个全新的字符串。

```
animal = 'fish'animal[0].upper() + animal[1:-1] + animal[-1].upper()
#=> 'FisH'
```

## 21.检查字符串是否全部大写

类似于`islower()`，`isupper()`仅在整个字符串大写时返回`True`。

```
'Toronto'.isupper() #=> False
'TORONTO'.isupper() #= True
```

## 22.什么时候使用 splitlines()？

`splitlines()`在换行符处拆分字符串。

```
sentence = "It was a stormy night\nThe house creeked\nThe wind blew."sentence.splitlines()
#=> ['It was a stormy night', 'The house creeked', 'The wind blew.']
```

## 23.举一个字符串切片的例子

分割一个字符串需要 3 个参数，`string[start_index:end_index:step]`。

`step`是字符应该返回的间隔。所以步长 3 将在每第三个索引处返回字符。

```
string = 'I like to eat apples'string[:6] #=> 'I like'
string[7:13] #=> 'to eat'
string[0:-1:2] #=> 'Ilk oetape' (every 2nd character)
```

## 24.将整数转换为字符串

为此使用字符串构造函数`str()`。

```
str(5) #=> '5'
```

## 25.检查字符串是否只包含字母表中的字符

如果所有字符都是字母，则`isalpha()`返回`True`。

```
'One1'.isalpha()
'One'.isalpha()
```

## 26.替换字符串中子字符串的所有实例

不导入正则表达式模块，可以使用`replace()`。

```
sentence = 'Sally sells sea shells by the sea shore'sentence.replace('sea', 'mountain')
#=> 'Sally sells mountain shells by the mountain shore'
```

## 27.返回字符串中的最小字符

大写字符和字母表中较早的字符具有较低的索引。`min()`将返回索引最低的字符。

```
min('strings') #=> 'g'
```

## 28.检查字符串中的所有字符是否都是字母数字

字母数字值包括字母和整数。

```
'Ten10'.isalnum() #=> True
'Ten10.'.isalnum() #=> False
```

## 29.移除字符串左侧、右侧或两侧的空白

`lstrip()`、`rstrip()`和`strip()`删除字符串末尾的空格。

```
string = '  string of whitespace    '
string.lstrip() #=> 'string of whitespace    '
string.rstrip() #=> '  string of whitespace'
string.strip() #=> 'string of whitespace'
```

## 30.检查字符串是以特定字符开头还是结尾？

`startswith()`和`endswith()`检查字符串是否以特定的子字符串开始和结束。

```
city = 'New York'city.startswith('New') #=> True
city.endswith('N') #=> False
```

## 31.将给定的字符串编码为 ASCII

`encode()`用给定的编码对字符串进行编码。默认为`utf-8`。如果一个字符不能被编码，那么抛出一个`UnicodeEncodeError`。

```
'Fresh Tuna'.encode('ascii')
#=> b'Fresh Tuna''Fresh Tuna Â'.encode('ascii')
#=> UnicodeEncodeError: 'ascii' codec can't encode character '\xc2' in position 11: ordinal not in range(128)
```

## 32.检查所有字符是否都是空白字符

`isspace()`仅当字符串完全由空格组成时才返回`True`。

```
''.isspace() #=> False
' '.isspace() #=> True
'   '.isspace() #=> True
' the '.isspace() #=> False
```

## 33.一个字符串乘以 3 是什么效果？

该字符串被连接在一起 3 次。

```
'dog' * 3
# 'dogdogdog'
```

## 34.将字符串中每个单词的第一个字符大写

`title()`将字符串中的每个单词大写。

```
'once upon a time'.title()
```

## 35.连接两个字符串

附加运算符可用于连接字符串。

```
'string one' + ' ' + 'string two' 
#=> 'string one string two'
```

## 36.举一个使用 partition()函数的例子

`partition()`在子字符串的第一个实例上拆分字符串。返回拆分字符串的元组，但不移除子字符串。

```
sentence = "If you want to be a ninja"print(sentence.partition(' want '))
#=> ('If you', ' want ', 'to be a ninja')
```

## 37.Python 中字符串不可变是什么意思？

string 对象一旦创建，就不能更改。“修改”该字符串会在内存中创建一个全新的对象。

我们可以用`id()`函数来证明。

```
proverb = 'Rise each day before the sun'
print( id(proverb) )
#=> 4441962336proverb_two = 'Rise each day before the sun' + ' if its a weekday'
print( id(proverb_two) )
#=> 4442287440
```

串联`‘ if its a weekday’`在内存中用新的`id`创建一个新的对象。如果对象实际上被修改了，那么它会有相同的`id`。

## 38.定义一个字符串两次(与两个不同的变量名相关联)会在内存中创建一个还是两个对象？

比如写`animal = 'dog'`和`pet = 'dog'`。

它只会创造一个。我第一次遇到它时就发现这不直观。但这有助于 python 在处理大字符串时节省内存。

我们将用`id()`来证明这一点。注意两者有相同的`id`。

```
animal = 'dog'
print( id(animal) )
#=> 4441985688pet = 'dog'
print( id(pet) )
#=> 4441985688
```

## 39.举一个使用 maketrans()和 translate()的例子

`maketrans()`创建从字符到其他字符的映射。然后应用这个映射来翻译字符串。

```
# create mapping
mapping = str.maketrans("abcs", "123S")# translate string
"abc are the first three letters".translate(mapping)
#=> '123 1re the firSt three letterS'
```

注意上面我们是如何改变字符串中每个`a`、`b`、`c`和`s`的值的。

## 40.从字符串中删除元音

一种选择是通过列表理解来迭代字符串中的字符。如果它们与一个元音不匹配，那么将它们重新组合成一个字符串。

```
string = 'Hello 1 World 2'vowels = ('a','e','i','o','u')''.join([c for c in string if c not in vowels])
#=> 'Hll 1 Wrld 2'
```

## 41.什么时候使用 rfind()？

`rfind()`类似于`find()`，但是它从字符串的右边开始搜索，并返回第一个匹配的子字符串。

```
story = 'The price is right said Bob. The price is right.'
story.rfind('is')
#=> 39
```

# 结论

正如我经常向一位老产品经理解释的那样，工程师不是存储方法的字典。但是有时候少点谷歌可以让编码更加无缝和有趣。

我希望你粉碎了这个。

如果你觉得太容易了，你可能会对我的另一篇文章感兴趣， [54 个 Python 面试问题](/53-python-interview-questions-and-answers-91fa311eec3f)。