# 将你的大脑从 Python 重新连接到 Java

> 原文：<https://towardsdatascience.com/rewiring-your-brain-from-python-to-java-383960580098?source=collection_archive---------4----------------------->

## 学习一门新的编程语言时，你可能会遇到的七个概念障碍

自白:我个人的经历几乎和这篇文章的题目完全相反。实际上，我在大学里从 C++开始，转到 Java 来教授 AP 计算机科学 A，然后进入 Python 领域，使用所有可用的时髦数据科学库。现在我又在做一个 Java 项目(一个[神经网络包](https://github.com/danhales/java-neural-network)，它真的把我对底层数学的理解推向了极限)，我真的很注意这两种语言工作方式的细微差别。特别是，我一直在记录标准 Python 习惯在 Java 中会成为巨大障碍的地方。

![](img/ed609cd68b09664d2c17ad8a4ddcad2c.png)

印度尼西亚东爪哇|[Waranont(Joe)](https://unsplash.com/@tricell1991?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)on[Unsplash](https://unsplash.com/s/photos/java-island?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)

这篇文章是针对走在相反道路上的 Python 程序员(尤其是数据科学家)的；我将详细介绍在学习将 Python 本能应用于 Java 领域时可能会遇到的一些最初的概念障碍。特别是，我将避免表面上的差异，比如`snake_case`和`camelCase`或者 Java 要求你用分号结束一个语句，而 Python 却让它可选。我也将避免在 OOP 和函数式编程中陷得太深。这篇文章的重点是 Java 要求你以不同的方式思考如何解决你正在处理的任何问题。

> 尽管这看起来令人望而生畏，但请记住，在你之前，许多程序员已经成功地学习了 Java，所以这绝对是可以做到的。

## **1。Java 的开销比你习惯的要多。**

在 Python 中，您可以用一行代码编写一个完整的带有控制台输出的“Hello World”程序:

```
print('Oh hi there, globe')
```

要在 Java 中完成这一点，您需要创建一个完整的*类*，并用一个`main`方法作为入口点。

```
public class Room {
    public static void main(String[] args) {
        System.out.println("Oh hi there, globe");
   }
}
```

本质上，每个 Java 程序本质上都是某个类的`main`方法，它处理变量并告诉其他类做什么(有时，这个`main`方法隐藏在自动化文件的深处，就像 Android 项目一样)。尽管有一些方法可以在一个完整的类之外运行 Java 在教学时，我是 j grasp[j grasp](https://www.jgrasp.org/)中的`interactions`选项卡的忠实粉丝——但是要实现这一点还是有一些困难的。

简而言之，虽然 Python 允许您将一些命令直接输入解释器来测试算法，但是使用 Java 需要您将代码放在上下文中。

然后还有上面`Room`中的大象:`System.out.println()`。这看起来比仅仅产生控制台输出要多得多，对吗？这是因为我们对我们想要的输出显示方式非常非常挑剔；在我们可以放置该文本的所有不同位置中，我们希望它进入*控制台*，而不是日志文件、弹出窗口或以太网中的服务器。Python 默认*假设*到`print`你希望文本显示在控制台上，但是 Java 一般不做这样的假设。

这就引出了我们的下一点…

## 2.Java 要求你比 Python 更具体。

Python 让你逃脱了很多麻烦。想想你写过多少次这样的代码:

```
that_value_i_need = some_object.mystery_method()
```

却不得不跟进:

```
type(that_value_i_need)
```

为了弄清楚，确切地说，`mystery_method()`正在返回。

这不一定是处理事情的最佳方式；显然，医生应该解释你将从`mystery_method()`中得到什么，所以绝对有可能*你在调用之前就已经知道你将得到什么。但是 Python 让您逃脱了这一点。您可以为`that_value_i_need`创建一个变量，并且知道它可以处理`mystery_method()`抛出的任何内容。*

此外，您可以非常自信地完成像这样的走钢丝行为，甚至不必考虑所涉及的数据类型:

```
important_value = some_method(a, bunch, of, arguments)
important_value = unrelated_class.another_method()
```

毕竟`important_value`只是一个变量，可以保存它需要的任何数据类型……对吧？

在 Java 中，绝对不是这样。当声明一个变量时，需要从一开始就指定它的数据类型，然后在它的整个生命周期中被锁定。

虽然这一开始听起来令人生畏且有局限性，但我发现在 Java 中阅读别人的代码要容易得多。我可以自信地指着一个变量说，“`x`是一个`int`，因为它在第 72 行被声明为一个`int`。如果它保存的是由`someUsefulMethod`返回的值，这告诉我`someUsefulMethod`返回一个`int`(或者可以提升为`int`的东西，比如`short`)。

尽管当您尝试运行自己的代码和`error: cannot find symbol`消息时，这最初会让您感觉像是一堵不可逾越的保留字墙，浮动在其他人的代码和`error: cannot find symbol`消息中，但 Java 要求您如此具体的事实导致代码比 Python 明显更加自文档化、可预测和明确。尽管我在编写 Java 代码时需要更加细致，但我对理解它的工作方式有了明显的信心。

![](img/91c3c97a5846100f42f16e7fef2ae205.png)

我对自己的 Python 代码的理解的物理表现| [Nathan Dumlao](https://unsplash.com/@nate_dumlao?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) 在 [Unsplash](https://unsplash.com/s/photos/building-blocks?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) 上

说到文档,`javadoc`是一个生成干净文档的强大工具——即使您没有明确地写下注释(您应该这样做)。`javadoc`将读取您的代码并生成有组织的 html 文件，这些文件准确地指出您需要什么数据类型来传递一个方法，以及您可以得到什么值作为回报。大多数 ide 甚至有一个按钮可以帮你做到这一点。

为了让事情尽可能不含糊…

## 3.Java 比 Python 更严格。

让我们重温一下 Python 中的 Hello World 程序。我们可以有多少种不同的写法？

```
# version 1 - single quotes
print('Oh hi there, globe')# version 2 - double quotes
print("Oh hi there, globe")# version 3 - triple single quotes
print('''Oh hi there, globe''')# version 4 - uh, triple double quotes, because why not?
print("""Oh hi there, globe""")
```

Python 为完成同样的任务提供了很多选择——在上面的例子中，我们可以用四种不同的语法来封装我们的字符串。当我学习 Python 的时候，这让我陷入了一个循环；我怎么知道何时使用哪一个？有没有一个优雅的经验法则需要记住？

如果你是一个 Python 程序员，即使经验很少，你也应该知道接下来会发生什么:“大多数人只会用单引号。除非字符串中有撇号，在这种情况下，可以用双引号括起来。除非字符串中既有撇号又有双引号，在这种情况下，您可以用三个单引号将其括起来。我猜，如果字符串中有单引号、双引号、三单引号和三双引号，那么你总是可以回到单引号，并在任何需要的地方使用转义字符。”

因为 Java 是如此的具体和严格，所以学习使用`String`要简单得多:总是用双引号将它们括起来。如果在你的`String`中有一个双引号，在它前面放一个转义符`\`:`\”`。如果你的`String`里面有一个`\`，在它前面加一个转义符:`\\`。如果你看到单引号，比如`'a'`，那意味着你在处理一个`char`，而不是一个`String`。

如果你已经习惯于将你能想到的每一个值——从`int`到`KitchenSink`——放入 Python 中的一个列表，你会发现使用 Java 数组需要一种非常不同的思维方式。Java 数组中的所有元素必须具有相同的数据类型，类似于`ndarray`或`Series`，但是通过仔细的设计，您可以通过多态引用绕过这个限制。

![](img/c35b351198bcc9418204917647daa18a.png)

一个 Python 列表可以容纳所有这些。Java 数组可能不能。|[Scott Umstattd](https://unsplash.com/@scott_umstattd?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)on[Unsplash](https://unsplash.com/s/photos/kitchen-sink?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)

尽管这种特殊性可能令人讨厌，但它使您的代码明显更具有可预测性，这是我的可读性黄金标准。块是用花括号而不是缩进组合在一起的，所以你可以很容易地告诉他们从哪里开始和结束。虽然这在您的计算机上不是一个大问题，但我发现在印刷的 Python 书籍中按缩进分组(其中代码被分成多页)会给初学该语言的人带来问题。

此外，这种严格性意味着变量作用域在 Java 中比在 Python 中更容易理解，如果你尊重花括号，它通常可以归结为“在花括号中定义？只存在于花括号中。实际上，您必须在 Java 中声明对所有类方法都可用的`global`变量，而在 Python 中，如果您曾经循环使用变量名，您可能会意外地从程序的完全不同的部分使用语法上有效(但完全不合适)的值。最重要的是，你可以在你的类中拥有`private`字段，而不是希望每个人都尊重你的下划线变量名。

如果你犯了错呢？与其让你的代码运行 90%的运行时间，不如在遇到第一个错误时就中止…

## 4.Java 是编译的，所以很多因误解前面几点而产生的错误在运行前就被捕捉到了。

显然，Python 会在运行代码之前解析代码，如果它不能理解您的意图，就会生成一个`SyntaxError`,但是我敢肯定，当您的代码在完成了一半您希望它做的事情之后，因为一个简单的错误而突然停止时，您所有人(尤其是数据科学家)都会感到恼火。

通常(无论如何，根据我的经验)，这种情况发生是因为您试图对错误类型的对象执行某种操作——比如调用方法。不是语法错误(你记住了`.`！)，所以解析器没有捕捉到它，回溯会带您进入核心库的一个兔子洞，所以您可以确切地看到 Python 假设您想要对您给它的神秘对象做什么。

在 Java 中，这一步发生在运行代码之前。因为在声明变量时，您已经将变量锁定为特定的数据类型，所以 Java 编译器可以验证您试图对该对象执行的每个操作在运行时之前实际存在*，如果编译器检测到不是为该数据类型定义的操作，您的程序将根本无法编译和运行。*

这并不是说你永远不会让你的 Java 程序意外终止——这种语言有一个动态的方面，所以你会和`NullPointerException`和`ArrayIndexOutOfBoundsException`成为非常好的朋友——但是这种额外的验证层，结合 Java 严格和特定的特性，往往会将这些运行时错误引入可预测的小巷。

## 5.设计 Java 方法几乎与设计 Python 函数完全不同，因此您需要以不同的方式考虑您的问题。

这种严格性的缺点是 Java 方法调用可能很快变得难以控制。我非常喜欢 Python 的功能性，不仅开始依赖无序的关键字参数，还开始传递函数并为可选参数提供默认值。

这是完全有效的 Python 代码…

```
def multiply(a, b, c=1):
    return a * b * cmultiply('hello', 3)
```

…在 Java 中无法运行。因为 Java 是静态类型的，所以在声明变量时就锁定了数据类型。这意味着您必须以与方法签名中出现的顺序完全相同的顺序传递具有该数据类型的值。虽然这降低了灵活性，但也减少了对代码的误用。

虽然您可以让同名的方法处理不同的参数列表，但 Java 通过*重载*来处理这一点，这需要您编写不同版本的方法来处理您希望作为参数列表支持的每个数据类型序列。Java 检查您传递的数据类型，查看是否定义了处理数据类型序列的方法，并调用该方法——具有相同名称和不同参数的方法实际上没有任何其他方式的联系。

如果你想为你的一个参数提供一个默认值呢？您必须重载该方法并从列表中删除默认值参数。

事实上，签名是由参数列表中的*数据类型*定义的，而不是由变量的*名称*定义的，当您习惯于用 Python 思考时，这也会产生问题。例如，如果我们想为此方法的重载版本提供默认参数:

```
public static double getArea(double length, double width){
    return length * width;
}
```

如果在参数列表中包含两个具有相同数据类型的版本，我们会遇到问题:

```
public static double getArea(double length) {
    return length * 10;
}public static double getArea(double width) {
    return width * 5;
}
```

这两个方法都被称为`getArea`并且都期望一个`double`，所以当你调用`getArea(12.3);`时，Java 不知道该走哪条路。

除此之外，虽然 Java 确实对 lambda 表达式有一些支持，但它们与函数式编程相差甚远，试图用面向对象的语言来思考函数式解决方案无异于自找灾难。

哦，还有返回类型？Java 将您限制为一种——并且您必须在编写方法时指定数据类型，这要求您提前很好地知道您想要返回什么。你可能习惯于认为 Python 能够返回多个值…

```
def get_stuff():
   return 1, 2, 3x, y, z = get_stuff()
```

…但它实际上返回一个对象——捆绑到一个元组中的所有值:

```
>>> type(get_stuff())
<class 'tuple'>
```

在 Python 中，你可以在不知道这个事实的情况下惊人地高效，但是在 Java 中，你总是必须知道你的方法返回什么。

## 6.除了数据类型之外，Java 还要求你考虑更低层次的概念，比如内存管理。

这可能有点草率(特别是对于来自没有自动垃圾收集功能的 C 语言的人来说)，但是 Java 的某些部分需要您考虑在内存方面发生了什么。我说的是参考变量！

例如，当您尝试在 Python 中运行以下代码时会发生什么？

```
my_list = [1, 2, 3]
print(my_list)
```

如果您来自纯 Python 背景，您将会看到以下输出:

```
[1, 2, 3]
```

另一方面，您从下面的 Java 代码中得到了什么？

```
public class JavaFeatures {
    public static void main(String[] args) {
      int[] myList = {1, 2, 3};
      System.out.println(myList);
  }
}
```

没错！它将打印内存地址。这里是我的电脑存储的地方:`[I@6d06d69c`

同样，Python 假设当您打印引用列表的变量时，您希望看到列表的*内容*，而 Java 在默认情况下，打印的正是引用变量中存储的内容:数组在内存中的位置。要显示内容，您需要遍历它们:

```
for (int i = 0; i < myList.length; i++) {
    System.out.println(myList[i]);
}
```

虽然这需要一点点努力来让你的大脑理解，但它迫使你思考当你抛出引用变量(该对象的内存地址)时，实际上传递的是什么*，这让你更深入地了解你的计算机实际上是如何执行其任务的。*

*虽然 Java 不像其他语言那样低级，但是从 Java 引用变量跳到 C 指针比从 Python 引用跳到 C 指针要容易得多，因为您已经习惯了遇到上面这种迫使您考虑内存地址的问题。*

*但是 Python 缺乏“设置好就忘了”的心态并不局限于引用变量。*

## *7.Java 并没有真正意义上的“T1”或“T2”。*

*当我考虑是什么让 Python 成为一门有用的语言时，我通常不会考虑语言特性或语法。我想到了 Python *生态系统*。通过快速浏览命令行，您可以使用类似于*

```
*pip install snazzy_new_library*
```

*下载一个`snazzy_new_library`以便随时使用。*

*尽管你会在 message 上发现有人坚持认为像`maven`或`gradle`这样的打包工具和`pip`或`conda`一样强大，但事实是，虽然这些工具确实*强大*，但它们并不真正等同于*。* Java 程序的结构通常是不同的——不是让一个包对该环境中的任何程序全局可用，而是将包直接与需要它们的应用程序捆绑在一起，并且通常需要通过将一个 jar 文件放到正确的位置来手动添加，或者包含在一个管理你的依赖性的工具中(这就是像`maven`和`gradle`这样的工具实际上是*擅长的)。**

*这对 Python 程序员来说可能很不方便，因为它要求您跳过额外的关卡，在您的项目中包含一个有用的现有库，结果项目文件的大小可能会变得很大。另一方面，将包含的库与项目捆绑在一起可以确保必要的代码在需要时确实可用。*

## *结论:你可以学习 Java，它会让你成为更好的程序员。*

*不得不调整自己的问题解决技能以适应新编程语言的怪癖可能是一项可怕的任务，但学会一门新语言确实会扩大你工具箱中的工具库——即使是在你的第一语言中。c 指针让我困惑，直到我习惯了使用 Java 引用变量，Java 中的 lambda 表达式让我困惑，直到我看到它们是如何在 Python 的函数式编程中使用的……这反过来又解释了为什么我们在 c 中需要函数指针。*

*尽管这看起来令人望而生畏，但请记住，在你之前，许多程序员已经成功地学习了 Java，所以这绝对是可以做到的。*