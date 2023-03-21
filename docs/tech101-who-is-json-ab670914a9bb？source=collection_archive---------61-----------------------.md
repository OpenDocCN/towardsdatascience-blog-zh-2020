# { tech 101:Json 是谁？}

> 原文：<https://towardsdatascience.com/tech101-who-is-json-ab670914a9bb?source=collection_archive---------61----------------------->

## JSON 文件介绍。

虽然沃尔赫斯、斯泰瑟姆和德鲁洛各有所长，但我认为最好的 Json 应该是机器可读的文件格式。Json，或者更确切地说，JSON，是科技界一个重要的缩写词。JSON 代表 JavaScript 对象符号。数据专业人员、软件工程师和 It 人员经常使用它。如果你从来没有写过一段代码，这对你来说就像是一门外语。你猜怎么着？确实是。

我们来分解一下。

# { JS: JavaScript }

JavaScript 是一种脚本或编程语言*，允许你在网页上实现复杂的功能(根据 [Mozilla](https://developer.mozilla.org/en-US/docs/Learn/JavaScript/First_steps/What_is_JavaScript) )。这是用于构建网站的最流行的编程语言之一。无论你是在访问 Zillow.com 的[还是 Gap.com 的](http://zillow.com)或[或者几乎任何网站，在某种程度上，这种构建都很有可能依赖于 JavaScript。我怎么知道他们用的是 JavaScript？](http://gap.com)*

*让我们来看看。*

1.  *打开一个新的标签，并前往[Gap.com](http://gap.com/)(见下图)。*
2.  *右键单击页面上的任意位置，根据您的浏览器选择“检查元素”或“检查”。这将从窗口的下半部分打开一个新面板。*
3.  *在这个新面板中，确认您在“元素”或“检查器”选项卡上。向上或向下滚动，直到看到单词“text/javascript”或以“”结尾的文件名。js”。*

*![](img/ac2ab2baae65d943293fe92829cf121c.png)**![](img/848471604d2b002c1a88bab59fdf9d07.png)*

*去 Gap.com。右键单击并选择“检查”或“检查元素”。*

*![](img/a7e26f3c6257a13afc2fed03931ab6da.png)**![](img/a99958ef90561325397cfa0bc0ffd91f.png)*

*在“Elements”或“Inspector”选项卡下，您可以查找提到“javascript”或“Inspector”的内容。js”文件。*

*代码的一个例子是这样一行:*

```
*<script type=”text/javascript” src=”/static_content/onesitecategory/components/mfe/sitewide-app/69.5e08e990.js? defer=”defer”.></script>*
```

*上面的行调用一个特定的 JavaScript 文件来运行一组代码或指令。这些指令(即 JavaScript)改变了您与 Gap 主页交互时的外观和感觉。*

# *{ ON:对象符号}*

*韦氏词典将符号定义为艺术、科学、数学或逻辑中用来表达技术事实或数量的字符、符号或缩写表达的系统。一种常见的符号是用$x$来表示数学中的未知值或变量。然而，在这种情况下，人类将 JSON 设计为一种机器可以轻松解释的特定符号。对于易于人们阅读的符号来说，一个恰当的比较是一个购物清单。下图显示了一个典型的杂货清单从普通人类符号到 JSON 格式的转换。*

*![](img/e8454fd7a89b332e8f92159cb31be8b4.png)*

*在此图中，对象是列表的名称、杂货项目的类别以及各种项目本身。每个对象都用花括号括起来，而其中的项目则写成“key”:“value”对。你可以试试用 JSON 格式写[这里](https://www.w3schools.com/Js/js_json_objects.asp)。*

# *{如何解读 JSON？}*

*因为 JSON 文件对人类来说很难理解，程序员最终需要将输出转换成可读性更好的东西。如果你没有任何编程知识，需要解读一个 JSON 文件，网上有 JSON 查看器。我们可以用上图中 JSON 格式的购物清单测试一下。*

1.  *为这个 JSON [查看器](http://jsonviewer.stack.hu/)打开一个新标签(见下图)。*
2.  *而在“文本”选项卡中，粘贴下面的 JSON 代码:
    {“杂货清单”:{“食物”:{“面包”:“1 个面包”，“薯条”:“2 袋”}，“饮料”:{“苏打水”:“2 个 12 包”，“水”:“1 箱”}，“其他”:{“纸巾”:“4 卷”，“杯子”:“30 个塑料杯”}}}*
3.  *单击“查看器”选项卡*
4.  *单击加号继续打开不同的对象。*

*![](img/d1383d7cf3d1ab21dde443c58de2e655.png)*

*从上面粘贴 JSON 字符串。*

*![](img/ef7f18628d7550fd3ae177ec6ee5a5c1.png)*

*在“查看器”选项卡中，单击加号打开对象。*

# *{ JSON 为什么重要？}*

*想象一下，尝试阅读一本 JSON 格式的哈利波特小说。读一整章可能要花上几个小时，而且会让你头疼。这与机器阅读和解释我们以段落形式写的小说的斗争是可比的，只是我不认为计算机会头痛。无论如何，机器读取 JSON 格式的文件要比读取人类可读格式的数据或文本快得多。程序员把速度称为性能。程序员喜欢他们的代码具有高性能(即执行速度快)。*

*![](img/441fbf9e4110a98cef927b0b7b92c1c3.png)*

*来源: [Unsplash](https://unsplash.com/photos/-2vD8lIhdnw)*

*下次当你听到有人提到 JSON 时，它将不再像是一门外语。没有必要因为沮丧而咬你的铅笔。回想一下你的购物清单。JSON 所做的只是将信息转换成不同的格式。新的格式使机器能更快地读取信息。我相信你也会表现得很好。*

*~ [数据通才](http://thedatageneralist.com/)*