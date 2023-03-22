# 用 Python 格式化字符串

> 原文：<https://towardsdatascience.com/formatting-strings-in-python-6e014bdd1663?source=collection_archive---------30----------------------->

## 如何在 Python 中使用 format()方法和 f 字符串

![](img/734db9f32b42a3dc05f8c27d7517a842.png)

凯文·Ku 在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上的照片

## 介绍

python 中有多种格式化字符串的方法。我们将讨论 format()方法和 f 字符串，以创建以下内容:

我们将使用 first_name、last_name 和 age 变量来创建一个字符串，该字符串包含某人的名字、姓氏和年龄，格式如下:

> '名字姓氏年龄岁'

一个例句是:“约翰·多伊今年 43 岁”。

## 使用 format()方法

实现这一点的一种方法是使用 format 方法。format()方法是一个字符串方法。一旦你从一个字符串中调用它，它会用括号中传递的变量值替换字符串中所有的花括号，按照它们被传递的顺序。因此，花括号充当变量的占位符。

例如，要使用上述示例的 format 方法，我们将使用以下代码:

```
first_name = 'John'
last_name = 'Doe'
age = 43sentence = '{} {} is {} years old'.format(first_name, last_name, age)print(sentence)
# 'John Doe is 43 years old'
```

> **注意:**format 方法将按照变量在括号中出现的顺序用变量的值替换花括号。

如果我们不想担心以正确的顺序传递变量，我们可以在每个花括号中使用键，然后为该特定键分配变量:

```
first_name = 'John'
last_name = 'Doe'
age = 43sentence = '{first} {last} is {age_yrs} years old'.format(last=last_name, age_yrs=age, first=first_name)print(sentence)
# 'John Doe is 43 years old'
```

注意，如果我们为每个占位符使用特定的键，变量就不必以正确的顺序传递。

虽然 format()方法完成了这项工作，但是还有一种更好的格式化字符串的方法，那就是使用 f 字符串。

[](/looping-in-python-5289a99a116e) [## Python 中的循环

### 如何在 python 中使用 enumerate()函数

towardsdatascience.com](/looping-in-python-5289a99a116e) 

## f 弦

f 字符串是 Python 3.6 及更高版本中格式化字符串的一种新方法。对于大多数人来说，它们是格式化字符串的首选方式，因为它们易于阅读，因此更加直观。

> f'{var_1} {var_2}是{var_3}岁'

要指定我们想要使用 f 字符串，或者格式化字符串，我们只需在字符串前面放一个 f。然后，我们可以直接将变量添加到我们的花括号中，在我们希望它们出现的位置。因此，我们不需要像使用 format 方法那样在字符串末尾使用任何方法或传递任何变量。这是一种更直观的格式化字符串的方式，因为我们不必确保所有的变量都与占位符的顺序相同，也不必确保它们被添加到正确的位置。

因此，为了用 f 字符串完成上述任务，我们将使用以下代码:

```
first_name = 'John'
last_name = 'Doe'
age = 43sentence = f'{first_name} {last_name} is {age} years old'print(sentence)
# 'John Doe is 43 years old'
```

与 format 方法类似，我们也可以在 f 字符串中运行方法或函数。例如，如果我们希望名字大写，姓氏小写，我们可以在 f 字符串中使用相应的字符串方法:

```
first_name = 'John'
last_name = 'Doe'
age = 43sentence = f'{first_name.upper()} {last_name.lower()} is {age} years old'print(sentence)
# 'JOHN doe is 43 years old'
```

如果你喜欢阅读这样的故事，并想支持我成为一名作家，考虑注册成为一名媒体会员。每月 5 美元，你可以无限制地阅读媒体上的故事。如果你用我的 [***链接***](https://lmatalka90.medium.com/membership) *注册，我会赚一小笔佣金。*

[](https://lmatalka90.medium.com/membership) [## 通过我的推荐链接加入媒体——卢艾·马塔尔卡

### 阅读卢艾·马塔尔卡的每一个故事(以及媒体上成千上万的其他作家)。您的会员费直接支持…

lmatalka90.medium.com](https://lmatalka90.medium.com/membership) 

## 结论

在本教程中，我们快速了解了在 python 中格式化字符串的方法。我们首先看了 format()方法，然后看了更优雅和直观的 f 字符串。然后我们看到了如何在 f 字符串中使用方法或函数。