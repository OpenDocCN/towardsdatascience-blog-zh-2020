# 嵌套数据结构和嵌套循环

> 原文：<https://towardsdatascience.com/nested-data-structures-and-nested-loops-247eb5fa016d?source=collection_archive---------35----------------------->

## 循环中的循环…循环中的循环。

![](img/a89d0d62412c662da68159df8e879d4f.png)

照片由[卡伦·乔卡](https://unsplash.com/@kciocca?utm_source=medium&utm_medium=referral)在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 拍摄

我在熨斗学校参加兼职数据科学项目的第二周，到目前为止一切都很顺利！在考虑了什么样的职业道路最适合我之后，我决定从事数据科学方面的职业。我拥有心理学和哲学的双学士学位，但直到最近，我还没有在这些领域中找到我特别热爱的领域。多亏了新冠肺炎疫情和几个月的隔离，我终于能够在熨斗公司追求我在技术、统计和编码方面的兴趣。我喜欢我的老师和我的友好的同龄人，我发现到目前为止我对材料有很好的理解。

不过，有一件事让我陷入了一个循环(绝对是双关语):Python 中的**嵌套循环**。

在这个项目开始的时候，我们被鼓励在博客上写下我们可能会发现具有挑战性的材料的各个方面，以便帮助我们自己更深入地理解它，同时也帮助其他人学习。起初，嵌套循环对我来说很棘手，因为它们是其他循环中的循环。我发现，在构建嵌套循环结构的过程中，分解它并一次只关注一个循环会有很大帮助。

我写这篇文章所做的工作还包括理解 Python 中的字典和列表理解，这两个主题一开始也有点难。和大多数障碍一样，我越努力克服它，它就变得越容易。考虑到这一点，下面是使用嵌套循环在 Python 中处理嵌套数据的一些工作。

我先说一些代表我家的数据。

```
my_family = [ { "family_name": "Tunnicliffe", "num_people": 4, "local": True, "city": "Bethpage, NY", "date_established": 2014, "names": ["Diane", "Steve", "Dylan", "Landon"], "number_of_children": 2, "children": [ { "name": "Dylan", "age": 5, "favorite_color": "black", "nickname": "Dillybeans", "loves": "Super Mario", }, { "name": "Landon", "age": 2, "favorite_color": "blue", "nickname": "Landybean", "loves": "trucks", } ] }, { "family_name": "Agulnick", "num_people": 5, "local": False, "city": "Newton, MA", "date_established": 1987, "names": ["Ellen", "Mark", "Diane", "Joshua", "Allison"], "number_of_children": 3, "children": [ { "name": "Diane", "age": 31, "favorite_color": "pink", "nickname": "Dini", "loves": "unicorns", }, { "name": "Joshua", "age": 28, "favorite_color": "red", "nickname": "Joshie", "loves": "trains", }, { "name": "Allison", "age": 26, "favorite_color": "purple", "nickname": "Alli", "loves": "candy", } ] }]
```

上面，我使用了变量`my_family`来存储一个包含两个字典的列表:一个用于我居住的家庭单元(我自己、我丈夫和我们的两个孩子)，另一个用于我出生的家庭单元(我的父母、我自己和我的兄弟姐妹)。Python 中的**字典**是以键:值对的形式存储数据的对象。所以在第一个字典中，对于`"family name"`的键，我们有`"Tunnicliffe"`的值。这在使用键和值来访问信息时变得很有用，尤其是在嵌套循环中。

![](img/77726410a5167602af470ae4b9291847.png)

由 [Marco Secchi](https://unsplash.com/@marcosecchi?utm_source=medium&utm_medium=referral) 在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄的照片

现在，假设我想访问我居住的城市。为了获得这些数据，我将调用第一个字典并寻找与键`"city"`相关联的值:

```
print(my_family[0]['city'])
```

作为回应，我会得到:

```
Bethpage, NY
```

这样做是因为我知道代表我居住的家庭的数据存储在第一个列表对象中，即一个字典，索引为`my_family[0]`。由于`‘city’`是该词典的一个属性，因此很容易理解，例如:

```
print (f"I live in {my_family[0]['city']}.")
```

我们会有:

```
I live in Bethpage, NY.
```

好的，这很有意义，也很简单。现在，如果我想创建一个两个家庭中每个人的名字的列表，作为一个组合列表，该怎么办呢？为此，我可以使用嵌套的 For 循环:

```
names = []for unit in my_family: for name in unit['names']: names.append(name)print(names)
```

就在那里。循环中的循环。外部循环由`for unit in my_family:`指定，它访问 my_family 中包含的我的列表中的两个字典。而`for name in unit['names']`是内部循环，它访问两个字典的键`names`的所有值。最后，我们的 append 方法意味着，对于列表`my_family`中的每个家庭单元字典，以及该家庭单元的名称列表中的每个名称，将这些名称添加到名为`names`的列表中。因此，作为回应，我们得到:

```
['Diane', 'Steve', 'Dylan', 'Landon', 'Ellen', 'Mark', 'Diane', 'Joshua', 'Allison']
```

酷！现在我们有进展了。值得注意的是，在我写`unit`和`name`的地方，你可以放任何你想要表示元素的变量。例如，我本来可以写`for blob in my_family`或`for narwhal in unit['names'].`，但是这里的目标是让事情更有意义，而不是更少，所以我选择了更符合逻辑(尽管不那么有趣)的变量名。让我们更进一步。现在我要一份孩子的名单。我可以这样得到:

```
children = []for unit in my_family: for child in unit['children']: children.append(child['name'])print(children)
```

我会:

```
['Dylan', 'Landon', 'Diane', 'Joshua', 'Allison']
```

或者如果我想要他们的昵称列表:

```
children = []for unit in my_family: for child in unit['children']: children.append(child['nickname'])print(children)
```

我会看到这个列表:

```
['Dillybeans', 'Landybean', 'Dini', 'Joshie', 'Alli']
```

请注意，我在这里使用的“儿童”一词相当宽松，因为我把我自己和我的兄弟姐妹列为儿童，而我们在技术上都是成年人。(虽然我们可能感觉不像成年人，我们的兴趣，在我们的数据集中由关键字`'loves':`指定，当然说明了这一点。)如果我想找到我出生的家庭单元中的孩子的年龄，记住我们将这个单元指定为非本地单元(从地理上来说)会有所帮助。因此，要访问这些信息，我可以这样写:

```
child_ages = []for unit in my_family: if unit['local'] == False: for child in unit['children']: name_and_age = child['name'], child['age'] child_ages.append(name_and_age)print (child_ages)
```

我们会看到非本地家庭单元中儿童的姓名和年龄列表:

```
[('Diane', 31), ('Joshua', 28), ('Allison', 26)]
```

事实上，我和我的兄弟姐妹都是成年人了。

为了加深我们对这一切是如何工作的理解，让我们再做几个。假设我想找出哪个孩子对超级玛丽真正感兴趣。我可以这样做:

```
loves_Mario = Nonefor unit in my_family: for child in unit['children']: if child['loves'] == 'Super Mario': loves_Mario = child['name']print (f"{loves_Mario} really loves Super Mario.")
```

![](img/467c89d5cdba604d058433bdf124196d.png)

照片由 [Cláudio Luiz Castro](https://unsplash.com/@claudiolcastro?utm_source=medium&utm_medium=referral) 在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄

我们的书面回复是:

```
Dylan really loves Super Mario.
```

是真的！我的儿子迪伦对超级马里奥非常着迷。当我写这篇文章的时候，他穿着超级马里奥睡衣和布瑟袜子。注意，这里使用了`==`操作符来证明与键`child[‘loves']`关联的值和与`"Super Mario"`关联的值是相同的。与上面使用的类似类型的代码公式可以用于访问任何信息，从兴趣到喜欢的颜色。例如:

```
loves_trains = Noneloves_trains_age = Nonefor unit in my_family: for child in unit['children']:

        if child['loves'] == 'trains': loves_trains = child['nickname'] loves_trains_age = child['age']print (f"{loves_trains} is still very much into trains, even at the age of {loves_trains_age}.")
```

我们有:

```
Joshie is still very much into trains, even at the age of 28.
```

或者:

```
likes_blue = Nonefor unit in my_family: for child in unit['children']: if child['favorite_color'] == 'blue': likes_blue = child['name']print (f"{likes_blue}'s favorite color is blue.")
```

我们的答案是:

```
Landon's favorite color is blue.
```

一旦掌握了访问嵌套信息的诀窍，它就变得像访问 Python 中的任何其他字典项一样合乎逻辑。最后，让我们看看能否找到`my_family`最大和最小的孩子。为此，我将使用嵌套循环和排序，这个任务在之前的实验中让我非常头疼。但是，我现在已经多练习了一点，所以让我们试一试。

```
oldest_child = Noneyoungest_child = Nonechildren = []for unit in my_family: for child in unit['children']: children.append(child)sorted_children = (sorted(children, key = lambda child: child['age'], reverse = True))oldest_child = sorted_children[0]['name']youngest_child = sorted_children[-1]['name']print(f"The oldest child is {oldest_child}. The youngest child is {youngest_child}.")
```

请击鼓…

```
The oldest child is Diane. The youngest child is Landon.
```

![](img/627670658e3fb64042f1d8c3b2c8e088.png)

我自己，我丈夫和我的孩子。我的(本地)家庭，在上面的数据中引用为 my_family[0]。

代码不会说谎。我是数据集中最大的孩子，我最小的儿子兰登是最小的。考虑到这整个嵌套循环的概念在几天前对我来说是多么令人沮丧，我不得不说写这个实际上比我预期的更有趣。(我不知道我的兄弟姐妹们会对这次冒险有什么感觉，所以我们不要告诉他们火车、糖果以及在网上列出他们的年龄。)

我认为到目前为止我学到的最重要的事情是，当谈到 Python 时，任何问题都可以通过结合信息(无论是来自课堂材料、教师和同龄人，还是 Google)和大量实践来解决。我现在可以看到一段代码，并且通常可以计算出它的输出是什么。当涉及到嵌套数据时，它可以被一行一行地分解成小块，以准确地计算出您试图访问的信息。