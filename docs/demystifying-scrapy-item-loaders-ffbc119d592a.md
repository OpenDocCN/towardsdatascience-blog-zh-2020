# 揭开杂乱物品装载器的神秘面纱

> 原文：<https://towardsdatascience.com/demystifying-scrapy-item-loaders-ffbc119d592a?source=collection_archive---------6----------------------->

## 权威指南

## 自动清理和扩展你的垃圾蜘蛛

![](img/6be3e37dd9ee2fb01e916c38f86eaa64.png)

来自 Unsplash 的 Nicolasintravel

# 项目和项目加载器

当从网站上抓取数据时，它可能是混乱和不完整的。现在 scrapy 上的大部分教程都引入了物品的概念。项目为报废的数据提供容器。但是物品装载器在这里起什么作用呢？它们提供了填充项目容器的机制。

项目加载器自动执行常见任务，如在项目容器之前解析数据。它还提供了一种更干净的方式来管理提取的数据，因为你扩大了你的蜘蛛。

要跟进，请阅读 scrapy 文档中的项目。有必要对这个有个了解再继续！

# 路线图

在本文中，我们将定义什么是与项目相比较的项目加载器。然后我们将讨论项目加载器如何通过处理器来完成这项工作。这些处理器是内置的或定制的。它们为我们提供了在填充项目字段之前解析数据的能力。然后，我们将向您展示项目加载器如何使数据提取更加干净。这是在传送到处理器之前。我们将解释如何让我们自己的处理器超越内置函数，按照我们认为合适的方式改变我们的数据。我们将介绍如何扩展 ItemLoader。这允许我们在网站改变时扩大我们的蜘蛛的功能。

## 清单

1.  定义项目装入者职责
2.  定义输入和输出处理器，这是项目装载器的发电站
3.  定义 ItemLoaders 方法。这就是 ItemLoaders 为处理器获取数据的方式
4.  定义项目装入程序上下文。这是处理器之间共享键和值的一种方式，非常有用
5.  定义内置处理器`Identity()`、`TakeFirst()`、`Join()`、`Compose()`、`MapCompose()`
6.  如何定义项目加载器所需的特定输入和输出处理器。这为您提供了灵活性。您可以为不同的项目字段选择不同的输入和输出处理器。
7.  定义 NestedLoader。当所有数据都在一个 HTML 块中时，这是一种清理我们编写项目加载器的方式。
8.  如何扩展 ItemLoader 和插件功能。对于可伸缩性和网站变化非常有用。

# 使用项目装入器填写项目

让我们来看看物品装载器的幕后。为了利用这一点，我们必须创建一个 ItemLoader。这是一个名为 Scrapy.Loader.Itemloader 的类，它有三个主要参数。

1.  Item:指定我们为蜘蛛创建的一个或多个条目，以便用 ItemLoader 填充。如果没有指定，scrapy 会创建一个名为 default_item_class 的新项目对象
2.  选择器:为 Itemloaders 方法使用选择器。如果没有指定，Scrapy 会创建一个新的选择器类。然后给变量 default_selector_class 赋值。
3.  用于使用 default_selector_class 构造选择器的响应

## 句法

`ItemLoader([item,selector,response,] **kwargs)`

为了开始使用 ItemLoader，我们将它导入到我们的 spider 中。然后，我们导入我们在 spiders items.py 中指定的项目

```
from scrapy.loader import ItemLoader
from myproject.items import Product
```

现在让我们看看如何启动 ItemLoader，注意我们在这里没有指定选择器。如果我们不定义默认选择器，Itemloader 实际上会填充它。

```
def parse(self,response):
    l = ItemLoader(Product(), response)
```

注意事项
1。我们从 parse 函数开始，这是 ItemLoader 实例形成的地方。

2.我们将变量 l 赋给 ItemLoader 类实例。我们在 spiders Items.py 和响应中定义了我们的`product()` items 实例。

使用 ItemLoader 类的下一步是 ItemLoader 方法。这让我们有能力从我们的网站获取数据，并允许我们操纵它。我们通过一种叫做处理器的东西来操纵它。

![](img/a0f2d57db90c5673852f208a06bc1655.png)

Unsplash 的哈勃望远镜

# 输入输出处理器

对于每个项目字段，项目加载器都有一个输入处理器和一个输出处理器。一旦收到提取的数据，它们就进行处理。在 ItemLoad 方法中定义处理器来解析数据(下一节)。

一旦我们用输入处理器处理了数据。然后，它会将提供给它的任何数据追加到 ItemLoader 中的一个内部列表中。因为这些数据被放入一个列表中，所以允许 ItemLoader 的一个条目字段有多个值。这个内部列表的结果然后被馈送到输出处理器。然后输出处理器的结果被分配给 Item 字段。

处理器是调用数据进行解析并返回解析值的对象。有了它，我们就可以将任何函数用作输入或输出处理器。唯一的条件是处理器函数的第一个参数必须是可迭代的。

Scrapy 还内置了我们将在本文中讨论的处理器。

我们现在可以讨论真正神奇的 ItemLoader 方法了！

![](img/6efdb80d0a77e8a8bba5e7c6a3441b96.png)

来自 Unsplash 的 Slejven Djurakovic

# 项目加载器方法

ItemLoader 类具有允许我们在项目字段中使用数据之前更改数据的方法。我们将在这里讨论一些重要的方法。有关详细信息，请参考文档。

如果我们看一下 ItemLoader 类，它有几个处理提取数据的函数。最常见的三种是`add_xpath()`、`add_css()`、`add_value()`。

它们都接受一个项目字段名，然后接受一个 xpath 选择器、css 选择器或字符串值。它还允许使用处理器来改变数据。

## 句法

`add_xpath(field_name, xpath,*processors,**kwargs)`

`add_css(field_name,css, processors,**kwargs)`

`add_value(field_name,value,processors,**kwargs)`

ItemLoader 的核心功能是通过输入处理器实现的。这就是为什么 ItemLoader 方法提供了指定它的选项。

您可以通过指定自己的输入处理器来更改提取的数据。现在，如果没有指定处理器，默认的处理器是。然后只返回提取数据的值。

现在输出处理器接受这个收集的内部列表。这可能会也可能不会被我们自己的输入处理器改变。这提供了要填充的新值。

为了用提取的数据填充条目字段，我们使用 load_item()方法。一旦通过处理程序，这将填充“项目”字段。

现在让我们通过一个简单的例子来看看这在实践中是如何工作的。

```
def parse(self,response):
    l = ItemLoader(Product(), response)
    l.add_xpath('Title', //*[@itemprop="name"][1]/text()')
    return l.load_item()
```

笔记

1.我们用我们定义的项目`product()`和响应来定义我们的项目加载器

2.我们使用`add_xpath`方法指定条目字段标题和标题的 xpath 选择器。

3.然后我们使用`load_item()`方法填充这个条目字段

![](img/d484978083a5ae7a8316935a5a2795ba.png)

来自 Unsplash 的 Markus Spiske

# 项目装入程序上下文

在进入内置处理器之前。讨论项目加载器上下文很重要。这些是可以共享的键和值。我们创建的 ItemLoader 将已定义的项目/选择器/响应分配给 loader_context。然后他们可以习惯于改变输入/输出处理器。

一些内置处理器可以使用它，了解这一点很重要。

处理器函数中的`loader_context`参数允许处理器共享数据。

一个简单的例子

```
def parse_length(text,loader_context):
    unit = loader_context.get('unit','m')
#length parsing code goes herereturn parse_length
```

现在，您可以通过多种方式更改项目加载器上下文值。

1.  修改当前活动的项目加载器上下文

```
loader = ItemLoader(product)
loader.context['unit'] = 'cm'
```

2.在项目加载器实例化时(作为关键字参数)

```
loader = ItemLoader(product, unit='cm')
```

3.关于输入/输出处理器的项目加载器声明，例如 MapCompose

```
class ProductLoader(ItemLoader):
    length_out = MapCompose(parse_length, unit='cm')
```

ItemLoader 上下文对于传递我们希望在处理器中使用的键和值非常有用。

# 内置处理器

现在我们已经确定了项目加载器的基础。让我们来看看 Scrapy 提供给我们的一些样板文件。

在 Scrapy 代码库中，内置处理器的类在一个名为 processes.py 的单独文件中。我们必须导入它们才能使用它们。

```
from scrapy.loader.processors import BUILTIN PROCESSORS
```

Scrapy 提供了`Identity()`、`TakeFirst()`、`Join()`、`Compose()`、`MapCompose()`、`SelectJmes()`六个内置处理器。我们将带你经历前五个。

## 身份()

`Identity()`是最简单的处理器，它返回值不变。

```
from scrapy.loader.processors import Identity
proc = Identity()
proc(['one','two','three'])
```

输出:

```
['one,'two','three']
```

这个内置处理器你已经不知不觉用上了。当在我们讨论的 ItemLoader 方法中没有指定处理器时，identity 类实例被赋予变量 default_input_processor 和 default_output_processor。这意味着如果我们不指定处理器，ItemLoader 返回的值不变。

## Takefirst()

`Takefirst()`从接收的值中返回第一个非空值。它被用作单值字段的输出处理器。

```
from scrapy.loader.processors import TakeFirst
proc = TakeFirst()
proc(['', 'One','Two','Three'])
```

输出:

```
'one'
```

## 加入()

`Join(separator=u’ ’)`返回连接在一起的值。分隔符可以用来在每一项之间放置一个表达式。默认为`u’’`。在下面的例子中，我们输入`<br>`。

```
from scrapy.loader.processors import Join
proc = Join() 
proc(['One, 'Two', 'Three'])
proc2 = Join('<br>')
proc2(['One', 'Two','Three'])
```

输出:

```
'one two three''one<br>two<br>three'
```

## 撰写()

`Compose()`获取一个输入值，并将其传递给参数中指定的函数。如果指定了另一个函数，结果会传递给该函数。这样一直持续下去，直到最后一个函数返回这个处理器的输出值。这个函数是我们用来改变输入值的。`Compose()`用作输入处理器。

语法是`compose(*functions,**default_loader_context)`

现在每个函数可以接收一个`loader_context`参数。这将把活动加载器上下文键和值传递给处理器。现在，如果没有指定`loader_context`，则指定一个`default_loader_context`变量。这什么都不能通过。

```
from scrapy.loader.processor import Compose
proc = Compose(Lambda v: v[0], str.upper)
proc(['hello','world])
```

输出:

```
'HELLO'
```

笔记

1.  组合内置处理器被调用
2.  Proc 由我们用函数指定的 compose 类定义。这种情况下是 lambda 函数和 string upper 方法。每个列表项从小写字母变成大写字母。

## 地图合成()

很像`compose()`，但处理方式不同。输入值被迭代，第一个函数应用于每个元素。结果被连接起来以构造一个新的 iterable。然后将它用于第二个函数。最后一个函数的输出值连接在一起，形成输出。

这为创建只处理单个值的函数提供了一种便捷的方式。当字符串对象从选择器中被提取出来时，它被用作输入处理器。

```
def filter_world(x):
    return None if x == 'world else xfrom scrapy.loader.processors import MapCompose
proc = MapCompose(filter_world, str.upper)
proc(['hello', 'world', 'this','is', 'scrapy'])
```

输出

```
['HELLO, 'THIS', 'IS', 'SCRAPY']
```

笔记。

1.我们创建了一个名为`filter_world`的简单函数

2.`Mapcompose`被导入

3.过程被分配`MapCompose`。两个函数`filter_world`和一个类似函数的 str 方法。

4.然后 Proc 被输入一个 iterable，每一项都被传递给`filter_world`,然后应用一个 string 方法。

5.注意查看第二个项目`‘world’`如何被输入到`filter_world`函数。这将返回 none，该函数的输出将被忽略。现在可以在下一个项目中进行进一步的处理。正是这一点给了我们这样的输出，项目“世界”被过滤掉了。

![](img/a6f0cb8f06bb03cfb50cb2a0e8fdf0e1.png)

来自 Unsplash 的大卫·拉托雷·罗梅罗

# 声明自定义项目装入器处理器

我们可以像声明项目一样声明项目加载器处理器。也就是使用类定义语法。

```
from scrapy.loader import ItemLoader
from scrapy.loader.processors import TakeFirst, MapCompose, Joinclass ProductLoader(ItemLoader):
    default_output_processor = Takefirst()
    name_in = MapCompose(unicode.title)
    name_out = Join()
    price_in = MapCompose(unicode.strip)
```

笔记。

1.  ProductLoader 类扩展了 ItemLoader 类
2.  `_in`后缀定义输入处理器，`_out`后缀声明输出处理器。我们将 Item 字段(在本例中为 name)放在后缀之前，以定义我们希望流程处理哪个字段。
3.  `name_in`分配一个`MapCompose`实例，并定义了函数 unicode.title。这将用于名称项字段。
4.  `name_out`被定义为`Join()`类实例
5.  `price_in`被定义为一个`Mapcompose`实例。为价格项目字段定义函数`unicode.strip`。

也可以在项目字段中声明处理器，让我们看看它是如何工作的。

```
import scrapy 
from scrapy.loader.processors import Join, MapCompose, Takefirst
from w3lib.html import remove_tagsdef filter_price(value):
   if value.isdigit():
       return valueclass Product(scrapy.Item):
    name = scrapy.Field(input_processor=MapCompose(remove_tags), outputprocessor=Join(),) price = scrapy.Field(input_processor=MapCompose(remove_tags,     filter_price), outprocessor=Takefirst(),)
```

笔记

1.  我们导入内置处理器和另一个移除标签的包。
2.  我们定义了`filter_price`函数，如果输入是一个数字，它将被返回。
3.  然后，我们通过扩展 Scrapy 来定义项目。项目分类。
4.  Name 被定义为 ItemField。我们指定应该使用哪些输入和输出处理器。
5.  价格被定义为 ItemField。我们指定应该使用哪些输入和输出处理器。
6.  价格有两个被调用的函数，即`remove_tags`和`filter_price`。`filter_price`是我们定义的函数。

现在，我们可以在蜘蛛的 data.py 文件中使用 ItemLoader 实例。注意，我们已经通过我们的 spider 中的 items.py 指定了我们的输入和输出处理器。

```
from scrapy.loader import ItemLoader

il = ItemLoader(item=Product())
il.add_value('name',[u'Welcome to my', u'<strong>website</strong>'])
il.add_value('price', [u'&euro;', u'<span>1000</span>'])
il.load_item()
```

输出:

```
{'name': u'Welcome to my website', 'price': u'1000'}
```

注释
1。我们现在导入 ItemLoader 类
2。我们用创建的项目字段定义项目加载器
3。我们使用`add_value`方法，定义名称项字段，并传递一个包含字符串的列表。
4。我们使用`add_value`方法来定义价格项目字段，并传递另一个包含两个项目的列表。
5。当我们添加第一个值字符串时，它的标签被移除，两个项目被连接在一起
6。当我们添加第二个值时，第一项被忽略，因为它不是一个字符串，我们将数字变为 1000。
7。`load_item()`方法给了我们一个条目字段名和修改后的数据的字典。这被输入到项目字段中。

现在，输入和输出处理器的优先顺序是从 1 号到 3 号。

1.项目加载器特定于字段的属性 field_in 和 field_out

2.字段元数据(输入处理器和输出处理器关键字参数)

3.项目加载器默认`itemLoad.default_input_processor()`和`itemLoad.default_output_processor()`

![](img/445d8bb9fa6d29bad977c76073aee375.png)

来自 Unsplash 的 Brian Kostiuk

# 嵌套加载程序

当你想要抓取的信息在一个大的块中时，我们可以使用一个嵌套的加载器来提高可读性。它允许我们通过创建相对路径来缩短 ItemLoader 方法的路径。这可以让你的零碎项目更容易阅读，但不要过度使用它！

```
Loader = ItemLoader(item=Item())
footer_loader = loader.nested_xpath('//footer/)
footer_loader.add_xpath('social','a[@class="social"]/@href')
loader.load_item()
```

笔记

1.  我们像前面一样实例化一个 Itemloader 类
2.  我们为嵌套的加载程序定义了一个变量。我们指定了`nested_xpath()`方法，它接受一个选择器。在这种情况下我们给它`//footer/`。
3.  我们使用 footer_loader 来访问 ItemLoader 方法。这意味着我们不必多次指定页脚选择器。在这种情况下，我们调用`add_xpath()`方法，定义项目字段和我们想要的相对 xpath 选择器。这样我们就不必一直编写 xpath 选择器的`//footer/`部分

4.我们像以前一样调用`load_item()`方法将它输入到我们的项目字段 social 中。

# 重用和扩展项目加载器

我们可以使用继承的属性来扩展我们的项目加载器。这提供了无需创建单独的 ItemLoader 的功能。当网站改变时，这变得特别重要。这就是项目加载器的力量，当网站改变时，我们可以按比例增加。

当需要不同的输入处理器时，我们倾向于扩展 ItemLoader 类。使用 Item.py 来定义输出处理器更加定制化。

```
from scrapy.loader.processors import MapCompose
from myproduct.ItemLoaders import ProductLoaderdef strip_dashes(x):
    return x.strip('-')class SiteSpecificLoader(ProductLoader):
    name_in = MapCompose(strip_dashes, ProductLoader.name_in)
```

笔记

1.  我们导入地图合成流程
2.  我们还导入了一个`ProductLoader`，这是我们定义的 ItemLoader

3.然后我们创建一个函数`strip_dashes`来删除破折号

4.然后我们在`SiteSpecificLoader`中扩展`ProductLoader`

5.我们将字段输入处理器的名称定义为`MapCompose`。我们传入`strip_dash`函数并调用 ProductLoader `name_in`方法。

然后我们可以在我们的`SiteSpecificLoader`中使用`name_in`输入处理器。与所有其他的`ProductLoader`样板。

Scrapy 让你能够为不同的站点或数据扩展和重用你的项目加载器。

# 摘要

在这里，您已经了解了项目和项目加载器之间的关系。我们已经展示了处理器如何习惯于能够解析我们提取的数据。ItemLoader 方法是我们从网站获取数据进行更改的方式。我们已经讨论了 scrapy 拥有的不同内置处理器。这也让我们可以为项目字段指定内置或新的处理器。

# 相关文章

[](/scrapy-this-is-how-to-successfully-login-with-ease-ea980e2c5901) [## Scrapy:这就是如何轻松成功登录

### 揭秘用 Scrapy 登录的过程。

towardsdatascience.com](/scrapy-this-is-how-to-successfully-login-with-ease-ea980e2c5901) [](/how-to-download-files-using-python-ffbca63beb5c) [## 如何使用 Python 下载文件

### 了解如何使用 Python 下载 web 抓取项目中的文件

towardsdatascience.com](/how-to-download-files-using-python-ffbca63beb5c) 

# 关于作者

我是一名医学博士，对教学、python、技术和医疗保健有浓厚的兴趣。我在英国，我教在线临床教育以及运行网站[www.coding-medics.com。](http://www.coding-medics.com./)

您可以通过 asmith53@ed.ac.uk 或 twitter [这里](https://twitter.com/AaronSm46722627)联系我，欢迎所有意见和建议！如果你想谈论任何项目或合作，这将是伟大的。

如需更多技术/编码相关内容，请在此注册我的简讯[。](https://aaronsmith.substack.com/p/coming-soon?r=6yuie&utm_campaign=post&utm_medium=web&utm_source=copy)