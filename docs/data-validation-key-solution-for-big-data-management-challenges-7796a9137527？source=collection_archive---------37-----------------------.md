# 数据验证:应对大数据管理挑战的关键解决方案

> 原文：<https://towardsdatascience.com/data-validation-key-solution-for-big-data-management-challenges-7796a9137527?source=collection_archive---------37----------------------->

## 由于数据验证仍然是当今数据驱动型公司面临的最大挑战之一，这里有一个适用于全球技术团队的有效改进解决方案。

数据验证预计将是 2020 年电子商务网站可能面临的最大挑战之一。在本文中，我们将回顾一些关键统计数据，强调当前影响大数据公司的主要数据验证问题。这篇文章的最终目的是为技术团队提供一个质量改进的解决方案。

全球范围内，零售电子商务公司的销售额持续快速增长。2019 年，全球销售额达到 35，350 亿美元，据 [Statista](https://www.statista.com/statistics/379046/worldwide-retail-e-commerce-sales/) 统计，这些数字将在 2023 年翻近一番，达到 65，420 亿美元之多。

随着电子商务的持续增长，存储的数据也在增长。仅电子商务的数据量就将呈指数级增长。希捷的《2025 年数据时代报告》预测，到 2025 年，全球数据空间将达到 [175 兆字节。](https://www.seagate.com/gb/en/our-story/data-age-2025/)

但是，只有一小部分从竞争对手和公开来源收集的数据将用于分析。这是因为电子商店接收非标准数据，无论是来自第三方供应商还是通过他们自己构建的抓取基础设施。他们通常收集的数据是未格式化的，并且是原始格式，例如 HTML，因此很难阅读。

我们在 [Oxylabs](https://oxylabs.io/) 的客户也面临这些挑战，因为我们的大多数电子商务客户都处理大数据。本文旨在指出数据管理的主要难点，并为任何处理大数据的技术团队提供解决方案和捷径。

# 主要数据管理问题

## 体积增长

数据管理将是 2020 年大数据面临的最大挑战之一。不仅进入组织的信息量将难以存储和组织，而且非结构化数据流的涌入将减慢任何分析流程，中断工作流。

## 不同的数据格式

数据量增长并不是唯一的问题。不同的数据格式及其变体也在增长。举个例子，有些网站提供的日期是 *CSV* 格式，如 YYYY-MM-DD，而其他网站的标准是 *JSON* ，格式为 MM/DD/YYYY。在读取非标准化数据时，这种看似很小的差异可能会导致很大的错误。

## 数据有效性

当范围的增长速度超过人力资源的增长速度时，必须考虑使用自动化来处理这些任务。近 90%的公司表示他们已经实现了部分数据管理流程的自动化。这包括分析和报告形式的后端以及前端和数据过滤。这就是我们的数据管理解决方案**数据验证**发挥作用的地方。

# 什么是数据验证？

数据验证是一种检查和调整数据(通常在导入和处理之前)的准确性和质量的方法或功能。根据您使用的验证选项(我们将在本文后面介绍不同的 Cerberus 验证选项)，它可以成为一个自定义验证，确保您的数据是完整的，包含不同的值，并确保它们不会重复，并确保值的范围是一致的。

简而言之，数据验证允许您执行分析，知道您的结果将是准确的。那么，什么方法或库可以用于顺利的数据验证过程呢？

# Cerberus:强大的数据验证功能

[Cerberus](https://docs.python-cerberus.org/en/stable/) 提供强大而简单的轻量级数据验证功能。它被设计成易于扩展，允许自定义验证。简单来说，它是 Python 的一个验证库。

请注意，我们选择 Cerberus 作为例子，因为验证模式是语言不可知论者(JSON ),可以很好地与其他语言一起工作，使它更灵活地适应各种工作流。

## Cerberus 基本工作流程

要使用 Cerberus 验证您的数据，您需要做的就是定义规则。这是通过一个叫做*模式*的 *python* 字典对象来完成的。

假设您有 JSON 人员数据集:

```
```json**[**{**"name": "john snow",**"age": 23,**"fictional": True,**},**...**}```*
```

你希望确保它的有效性。您可以定义的最基本的*模式*如下所示:

```
```python**schema = {**"name": {"type": "string", "required": True},**"age": {"type": "int"},**"fictional": {"type": "bool"},**}```*
```

这里我们为个人数据字段定义了严格的类型:姓名必须是字符串，年龄必须是整数，虚构的必须是布尔值。此外，我们总是希望名字出现，因为没有名字的人的数据很少有用。一旦我们获得了数据集和*模式*，我们就可以在 *python* 中执行我们的验证:

```
```python**import json**from cerberus import Validator**​**dataset: dict**schema: dict**​**validator = Validator()**success = validator.validate(dataset, schema)**if not success:**print(validator.errors)**# error example:**# {'name': ["field 'name' is required"]}```
```

这里我们创建了一个 *Cerberus 验证器对象*。现在，您可以简单地为它提供一个数据集和模式，并运行 validate。如果验证失败，验证器将向您显示错误消息。很简单。现在让我们看看 Cerberus 为我们准备了什么样的验证选项！

# Cerberus 验证选项

**类型验证**

第一种也是最重要的一种验证是 __ 类型验证 _ _。

确保数据集字段是正确的类型是一种很好的做法，因为无效的类型可能会破坏整个数据工作流，甚至更糟的是，以静默方式执行无效的操作。例如，字段“年龄”最终是浮点数，而不是整数。这会在你不知不觉中打乱你所有的计算。

**值验证**

接下来，我们将验证价值覆盖和价值本身。例如，可以设置“必需”规则以确保该字段总是存在。“禁止”、“允许”和“包含”规则的混合可以确保严格的值选项。

为了举例，让我们验证一个**食谱**数据集:

```
```python**schema = {**# forbid some ingredients**"ingredients": {"forbidden": "pork"},**# restrict recipe type**"type": {"allowed": ["breakfast", "lunch", "dinner"]},**# all recipes posts should have recipes category**"categories": {"contains": "recipes"},**}```*
```

考虑到这一点，我们也可以将关系添加到一些关系期望中。例如，如果我们的**食谱**有素食版本，我们希望确保它包含素食成分:

```
```python**schema = {**"vegan_alternative": {"dependencies": "vegan_ingredients"}**}```*
```

最后，最灵活和最常见的值验证规则是正则表达式模式，Cerberus 也支持这种模式。以我们的**食谱**为例，我们可以进一步确保食谱的标题以大写字母开头:

```
```python**schema = {**"title": {"regex": "[A-Z].+"}**}```*
```

Cerberus 提供了广泛的内置值、覆盖范围和类型验证，并且可以通过自定义规则轻松扩展。让我们添加我们自己的 python 函数作为规则:

```
```python**def is_odd(field, value, error):**if not value & 1:**error(field, "Must be an odd number")**schema = {**"preparation_time": {"check_with": is_odd}**}```*
```

在这里，我们确保准备时间总是一个奇数(营销告诉我们奇数往往卖得更好)。

**密钥验证**

只剩下一件需要验证的事情了——密钥本身。这是一个重要的步骤，特别是如果您自己不做数据解释，而将数据提供给其他服务的话。例如，如果图形可视化工具不支持大写字母，您应该确保数据没有大写字母:

```
```python**schema = {**"keysrule": {**"type": "string",**"regex": "[a-z]+"**}**}```*
```

在这里，模式验证确保所有键都应该只包含小写字符。

正如我们所看到的，Cerberus 支持广泛的验证规则，通过使用它们的整体组合，我们确保我们的数据遵循严格的可预测格式，以进行无错误的数据处理。此外，地狱犬也可以帮助处理。

# Cerberus 附加处理

有时我们知道数据集已经有无效值。正是因为这个原因，Cerberus 提供了在验证之前处理数据的能力。

首先，我们可以规范化密钥格式:

```
​```python**v = Validator({}, allow_unknown={'rename_handler': [str, str.lower]})**v.normalized({'Cat': 'meow'})**# {'cat': 'meow'}```*
```

这里，我们确保所有的键都是小写字符串。

今后，我们还可以通过清除未知键来精简数据集，因为数据集通常带有元键和不重要的数据:

```
```python**v = Validator({'cat': {'type': 'string'}}, purge_unknown=True)**v.normalized({'cat': 'meow', 'dog': 'woof'})**# {'cat': 'meow'}**# cat people > dog people```
```

在这个例子中，我们去掉了不想要的动物类型。

我们也可以反其道而行之，通过为不存在的值设置默认值来扩展数据集:

```
```python**schema = {"dogs_or_cats": {**"type": "string",**"default": "cats"**}}```*
```

因此，如果“dogs_or_cats”字段没有值，我们可以直接假设该值是“cat”。

最后，我们可以通过强制将字符串转换为整数，以任何方式改变数据:

```
```python**schema = {'amount': {'type': 'integer', 'coerce': int}}**validator.validate({'amount': '1.0'})**validator.document**# {'amount': 1}```*
```

通过结合这两个 Cerberus 的特性，验证和处理，我们可以始终确保以一种非常实用的方式拥有一个可靠的数据流。您将拥有一个数据源模式文件，团队中的任何人都可以轻松地访问、编辑、维护和理解该文件。

# 包扎

随着不可靠结构化数据量的增加，管理和处理数据以使其可读的需求对大数据公司来说变得越来越重要。分析非结构化数据会变得非常令人头疼，并会降低您的工作流程。通过应用数据验证解决方案，如 Cerberus，您可以创建一个节省资源、资金和时间的重要公司生活帮。