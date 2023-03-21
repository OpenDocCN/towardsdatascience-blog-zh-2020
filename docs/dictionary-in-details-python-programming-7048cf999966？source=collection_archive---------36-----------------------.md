# Python 字典详解:Python 编程

> 原文：<https://towardsdatascience.com/dictionary-in-details-python-programming-7048cf999966?source=collection_archive---------36----------------------->

## 举例学习在编程问题求解中使用字典

![](img/2623aefb0fd7d0ecbe72254b8765de6c.png)

来源:费尔南多·埃尔南德斯

Dictionary 是 Python 编程语言中的一种复合数据类型。在某种程度上，它类似于列表。列表是元素的集合。字典是键、值对的集合。借助字典，许多编程问题的解决方案可以变得更简单、更简洁。在这里我要解释一下字典里所有重要的方法和一些例题的解题。

让我们定义一个简单的字典。

```
d = {'a': 1, 'b':5, 'c': 3, 'd': 2, 'e': 8, 'f': 6}
```

1.  在此字典中添加新元素:

```
d['s'] = 12
print(d)
```

字典 d 现在是这样的:

```
{'a': 1, 'b': 5, 'c': 3, 'd': 2, 'f': 6, 's': 12}
```

2.从字典 d 中删除元素 e。

```
del d['e']
```

3.获取键 a 的值。

```
d['a']
#Output: 1
```

4.“a”的值看起来太小了。将元素 a 的值更新为 10。

```
d['a'] = 10
```

5.将元素 s 的值加 3。

```
d['s'] += d['s'] + 3
```

6.检查字典 d 的长度。如果它小于 9，则向它添加三个以上的元素。

```
if len(d) < 8:
    d.update({'t': 21, 'h': 9, 'p':14})
print(d)
'''
Output:
{'a': 10, 'b': 5, 'c': 3, 'd': 2, 'f': 6, 's': 12, 't': 21, 'h': 9, 'p': 14}
'''
```

7.列出所有钥匙的清单。

```
d.keys()##Output looks like this:dict_keys([‘a’, ‘b’, ‘c’, ‘d’, ‘f’, ‘s’, ‘t’, ‘h’, ‘p’])
```

8.列出所有的值。

```
d.values()
##Output looks like this:
dict_values([10, 5, 3, 2, 6, 27, 21, 9, 14])
```

9.找出哪个字母的价值最大。

```
max = 0
max_key = 'a'
for k, v in d.items():
    if v > max:
        max_key = k
        max = d[max_key]
print(max_key)
```

答案出来是‘t’。

10.按升序对字典 d 的键进行排序。

```
sorted(d, key=lambda x: d[x])
#Output:
['d', 'c', 'b', 'f', 'h', 'p', 't', 's']
```

11.找出每个单词在下列句子中出现的次数。

```
sentences = "I love my country. My country is the best in the world. We have the best athletes in the world."
```

让我们制作一本字典，其中的关键字是这些句子中的单词，值是这些单词出现的频率。

以下是解决这个问题的步骤:

a.初始化一个空字典' sen_map '

b.把句子都变成小写

c.重复句子中的每个单词

d.检查 sen_map 中是否存在一个单词

e.如果不是，则添加值为 1 的单词

f.否则，将该单词的值更新 1

```
sen_map = {}  
sentences = sentences.lower() 
for i in sentences.split(): 
    if i not in sen_map:   
        sen_map[i] = 1  
    sen_map[i] += 1  
sen_map'''Output
{'i': 2,  'love': 2,  'my': 3,  'country.': 2,  'country': 2,  'is': 2,  'the': 5,  'best': 3,  'in': 3,  'world.': 3,  'we': 2,  'have': 2,  'athletes': 2}'''
```

字典可以像列表一样嵌套。这里有一个例子:

```
Gold_medals = {'USA': {'Wrestling': 3, 'Long Jump': 3, 'Basketball': 5},
              'China': {'Wrestling': 1, 'Long Jump': 5, 'Basketball': 3},
              'England': {'Wrestling': 2, 'Long Jump': 7, 'Basketball': 0}}
```

12.美国在跳远比赛中获得了多少枚金牌？

```
Gold_medals['USA']['Long Jump']
```

正如我们在上面的字典中看到的，输出是 3。

字典中组织信息的另一种方式是列表。

这里有一个例子:

```
students = [{'Name': 'John Smith', 'Age': 12, 'Score': 90},
           {'Name': 'Laila Jones', 'Age': 11, 'Score': 82},
           {'Name': 'Omar Martinez', 'Age': 10, 'Score': 70},
           {'Name': 'Romana Raha', 'Age': 13, 'Score': 78},]
```

13.返回班上得分最高的学生的姓名。

让我们一步一步解决这个问题。首先按降序对“学生”进行排序。

```
sorted(students, key=lambda x: x['Score'], reverse=True)
#Output:
'''[{'Name': 'John Smith', 'Age': 12, 'Score': 90},  {'Name': 'Laila Jones', 'Age': 11, 'Score': 82},  {'Name': 'Romana Raha', 'Age': 13, 'Score': 78},  {'Name': 'Omar Martinez', 'Age': 10, 'Score': 70}]'''
```

获取列表“学生”中的第一个词典。

```
sorted(students, key=lambda x: x['Score'], reverse=True)[0]'''Output
{'Name': 'John Smith', 'Age': 12, 'Score': 90}'''
```

最后，得到学生的名字。

```
sorted(students, key=lambda x: x['Score'], reverse=True)[0]['Name']
```

这一行代码将返回学生的姓名。也就是‘约翰·史密斯’。

我试图先解释字典的方法，然后给出一些例子来说明如何使用字典。我希望这有所帮助。

下面是字典的视频教程: