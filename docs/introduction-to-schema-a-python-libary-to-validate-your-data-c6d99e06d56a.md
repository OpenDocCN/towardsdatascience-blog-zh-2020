# 模式介绍:验证数据的 Python 库

> 原文：<https://towardsdatascience.com/introduction-to-schema-a-python-libary-to-validate-your-data-c6d99e06d56a?source=collection_archive---------3----------------------->

## 验证您的数据变得更加复杂！

![](img/5aafff85aeceb745bfd1e9a1a418e272.png)

照片由[非飞溅视觉效果](https://unsplash.com/@nonsapvisuals?utm_source=medium&utm_medium=referral)在[非飞溅](https://unsplash.com?utm_source=medium&utm_medium=referral)上拍摄

# 动机

您的脚本可以处理训练数据，但是当您将该脚本用于一个新的但应该相似的数据时，就会遇到错误。这是怎么回事？这可能是因为您的数据结构不像您预期的那样。

但是您可能很难查看新数据的每一行来找出问题所在。每次使用新数据时，手动分析您的数据也可能**耗时**。

如果您的代码没有抛出任何错误，但是数据发生了变化，那就更糟了。因此，您的型号的**性能可能会变得**更差**，因为数据与您的预期不同。**

如果我们可以用 Pytest 之类的工具编写函数测试，那么有没有一种方法也可以编写数据测试呢？

我们可以用图式做到这一点。本文将向您展示如何在各种场景中使用模式。

# 什么是图式？

[**Schema**](https://github.com/keleshev/schema) 是用于验证 Python 数据结构的库。

安装架构时使用

```
pip install schema
```

我们将使用 faker 来创建字典列表数据。Faker 是一个 Python 库，使我们能够轻松地创建假数据。我在这里写了如何使用 faker。

想象一下，这些数据展示了你朋友的信息。

```
[{'name': 'Norma Fisher',
  'city': 'South Richard',
  'closeness (1-5)': 4,
  'extrovert': True,
  'favorite_temperature': -45.74},
 {'name': 'Colleen Taylor',
  'city': 'North Laurenshire',
  'closeness (1-5)': 4,
  'extrovert': False,
  'favorite_temperature': 93.9},
 {'name': 'Melinda Kennedy',
  'city': 'South Cherylside',
  'closeness (1-5)': 1,
  'extrovert': True,
  'favorite_temperature': 66.33}]
```

# 模式入门

## 验证数据类型

我们可以使用模式来验证数据类型，如下所示。

```
[{'name': 'Norma Fisher',
  'city': 'South Richard',
  'closeness (1-5)': 4,
  'extrovert': True,
  'favorite_temperature': -45.74},
 {'name': 'Colleen Taylor',
  'city': 'North Laurenshire',
  'closeness (1-5)': 4,
  'extrovert': False,
  'favorite_temperature': 93.9},
 {'name': 'Melinda Kennedy',
  'city': 'South Cherylside',
  'closeness (1-5)': 1,
  'extrovert': True,
  'favorite_temperature': 66.33}]We want to make sure that ‘name’, ‘city’ columns are string type, ‘closeness (1–5)’ are
```

因为 schema 返回输出时没有抛出任何错误，所以我们知道我们的数据是有效的。

让我们看看如果数据类型不像我们期望的那样会发生什么

```
SchemaError: Or({'name': <class 'int'>, 'city': <class 'str'>, 'closeness (1-5)': <class 'int'>, 'extrovert': <class 'bool'>, 'favorite_temperature': <class 'float'>}) did not validate {'name': 'Norma Fisher', 'city': 'South Richard', 'closeness (1-5)': 3, 'extrovert': True, 'favorite_temperature': -45.74}Key 'name' error:
'Norma Fisher' should be instance of 'int'
```

从错误中，我们确切地知道数据的哪一列和值与我们期望的不同。因此，我们可以返回数据来修复或删除该值。

如果您只关心数据是否有效，请使用

```
schema.is_valid(data)
```

如果数据符合预期，这将返回`True`，否则返回`False`。

## 验证某些列的数据类型，而忽略其余列

但是，如果我们不关心所有列的数据类型，而只关心某些列的值，那该怎么办呢？我们可以用`str: object`来说明

```
Output: True
```

如您所见，我们尝试验证“名称”、“城市”和“favorite_temperature”的数据类型，而忽略数据中其余要素的数据类型。

数据是有效的，因为指定的 3 个要素的数据类型是正确的。

## 用函数验证

如果我们想确定列中的数据是否满足与数据类型(如列中值的范围)无关的特定条件，该怎么办？

Schema 允许您使用函数来指定数据的条件。

如果我们想检查“亲密度”列中的值是否在 1 到 5 之间，我们可以使用如下的`lambda`

```
Output: True
```

正如你所看到的，我们指定`n,`列中每一行的值为‘亲密度’，在 1 到 5 之间，用`lambda n: 1 <= n <=5.`表示整洁！

## 验证几个模式

***和***

如果你想确保你的‘接近度’列在 1 和 5 之间**并且**数据类型是一个整数呢？

这时候`And`就派上用场了

```
Output: False
```

虽然所有值都在 1 和 5 之间，但数据类型不是浮点型。因为不满足其中一个条件，所以数据无效

***或***

如果我们想在满足任一条件的情况下使列的数据有效，我们可以使用`Or`

例如，如果我们希望城市名称包含 1 个或 2 个单词，我们可以使用

***与和或的组合***

如果我们希望“城市”的数据类型是字符串，但长度可以是 1 或 2，该怎么办？幸运的是，这可以通过组合`And`和`Or`来轻松处理

```
Output: True
```

## 可选择的

如果我们没有你朋友的详细信息怎么办？

```
[{'name': 'Norma Fisher',
  'city': 'South Richard',
  'closeness (1-5)': 4,
  'detailed_info': {'favorite_color': 'Pink',
   'phone number': '7593824219489'}},
 {'name': 'Emily Blair',
  'city': 'Suttonview',
  'closeness (1-5)': 4,
  'detailed_info': {'favorite_color': 'Chartreuse',
   'phone number': '9387784080160'}},
 {'name': 'Samantha Cook', 'city': 'Janeton', 'closeness (1-5)': 3}]
```

因为 Samantha Cook 的“detailed_info”并不是对您所有的朋友都可用，所以我们想将此列设为可选。模式允许我们用`Optional`设置条件

```
Output: True
```

## 被禁止的

有时，我们可能还想确保某种类型的数据不在我们的数据中，比如私人信息。我们可以用`Forbidden`指定禁止哪个列

```
Forbidden key encountered: 'detailed_info' in {'name': 'Norma Fisher', 'city': 'South Richard', 'closeness (1-5)': 4, 'detailed_info': {'favorite_color': 'Pink', 'phone number': '7593824219489'}}
```

现在，每当 schema 抛出一个错误时，我们都知道了被禁止的列的存在！

# 嵌套词典

到目前为止，schema 已经使我们能够在几行代码中执行许多复杂的验证。但是在现实生活中，我们可能会处理比上面的例子更复杂的数据结构。

我们能把它用于更复杂结构的数据吗？比如字典中的字典？是的，我们可以

我们创建另一个包含嵌套字典的数据

```
>>> data[{'name': 'Norma Fisher',
  'city': 'South Richard',
  'closeness (1-5)': 4,
  'detailed_info': {'favorite_color': 'Pink',
   'phone number': '7593824219489'}},
 {'name': 'Emily Blair',
  'city': 'Suttonview',
  'closeness (1-5)': 4,
  'detailed_info': {'favorite_color': 'Chartreuse',
   'phone number': '9387784080160'}}]
```

现在我们用嵌套字典进行验证

语法非常简单！我们只需要在字典中编写另一个字典，并为每个键指定数据类型。

# 转换数据类型

schema 不仅可以用来验证数据，还可以用来转换数据类型，如果数据类型与我们预期的不一样的话！

例如，我们可以用`Use(int)`将字符串‘123’转换成整数 123

```
>>> Schema(Use(int)).validate('123')
123
```

# 结论

恭喜你！您刚刚学习了如何使用模式来验证和转换数据结构。如果你希望你的代码是可复制的，不仅需要测试代码，还需要测试你的数据。如果你正在寻找一种方法来验证你的数据，试试这个工具。它不仅有用，而且易于使用。

更多模式示例的源代码可以在[这里](https://github.com/khuyentran1401/Data-science/blob/master/data_science_tools/schema.ipynb)找到。

如果您想找到更多验证数据的方法，请查看 schema 的[文档](https://github.com/keleshev/schema)。

我喜欢写一些基本的数据科学概念，并尝试不同的算法和数据科学工具。你可以在 LinkedIn 和 Twitter 上与我联系。

如果你想查看我写的所有文章的代码，请点击这里。在 Medium 上关注我，了解我的最新数据科学文章，例如:

[](/how-to-boost-your-efficiency-with-customized-code-snippets-on-vscode-8127781788d7) [## 如何在 VSCode 上使用定制的代码片段来提高效率

### 与其为同一段代码复制，为什么不把它保存起来以备将来使用呢？

towardsdatascience.com](/how-to-boost-your-efficiency-with-customized-code-snippets-on-vscode-8127781788d7) [](/how-to-create-fake-data-with-faker-a835e5b7a9d9) [## 如何用 Faker 创建假数据

### 您可以收集数据或创建自己的数据

towardsdatascience.com](/how-to-create-fake-data-with-faker-a835e5b7a9d9) [](/how-to-create-and-view-interactive-cheatsheets-on-the-command-line-6578641039ff) [## 如何在命令行上创建和查看交互式备忘单

### 停止搜索命令行。用作弊来节省时间

towardsdatascience.com](/how-to-create-and-view-interactive-cheatsheets-on-the-command-line-6578641039ff) [](/introduction-to-hydra-cc-a-powerful-framework-to-configure-your-data-science-projects-ed65713a53c6) [## Hydra.cc 简介:配置数据科学项目的强大框架

### 尝试不同的参数和模型，而无需花费数小时来修改代码！

towardsdatascience.com](/introduction-to-hydra-cc-a-powerful-framework-to-configure-your-data-science-projects-ed65713a53c6) [](/introduction-to-ibm-federated-learning-a-collaborative-approach-to-train-ml-models-on-private-data-2b4221c3839) [## IBM 联邦学习简介:一种在私有数据上训练 ML 模型的协作方法

### 在训练来自不同来源的数据时，如何保证数据的安全？

towardsdatascience.com](/introduction-to-ibm-federated-learning-a-collaborative-approach-to-train-ml-models-on-private-data-2b4221c3839)