# Java 15 的新特性

> 原文：<https://towardsdatascience.com/new-java-15-features-47dac2333aa8?source=collection_archive---------28----------------------->

## 本地记录和密封类在 Java 15 中找到了自己的路

![](img/bed5a24b741a25ba582bc3989cd4715c.png)

Photo by [五玄土 ORIENTO](https://unsplash.com/@oriento?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

保持六个月周期的传统，在 2020 年 3 月 17 日发布了 [Java 14](https://medium.com/better-programming/whats-new-java-14-features-6b5856c94aa4) 之后，我们现在有了 Java 15，下一个非 LTS 版本将于 2020 年 9 月 15 日推出。

下面快速浏览一下 Java 15 的一些特性:

*   密封类(预览)— JEP 360
*   `instanceof`(第二次预览)— JEP 375 的模式匹配
*   记录(第二次预览)— JEP 359
*   文本块(标准)— JEP 378
*   隐藏类— JEP 371
*   移除 Nashorn JavaScript 引擎— JEP 372
*   重新实现传统 DatagramSocket API — JEP 373
*   禁用和反对偏向锁定— JEP 374
*   谢南多厄:一个低停顿时间的垃圾收集器——JEP 379
*   删除 Solaris 和 SPARC 端口— JEP 381
*   外部存储器访问 API(第二个孵化器)— JEP 383
*   不赞成激活 RMI 进行删除— JEP 385

# Mac OS 上的 Java 15 安装设置

*   要开始使用 Java 15，请从这里的[下载 JDK。](http://jdk.java.net/15/)
*   复制并提取`/Library/Java/JavaVirtualMachines`中的 tar 文件，如下图所示:

```
$ cd /Library/Java/JavaVirtualMachines 
$ sudo cp ~/Downloads/openjdk-15_osx-x64_bin.tar.gz /Library/Java/JavaVirtualMachines$ sudo tar xzf openjdk-15_osx-x64_bin.tar.gz 
$ sudo rm openjdk-15_osx-x64_bin.tar.gz
```

一旦完成，使用任何文本编辑器打开`bash_profile`。我用的是`vim ~/.bash_profile`。将 Java15 的路径设置为 JAVA_HOME，保存更改并执行`source ~/.bash_profile`以反映更改。

```
export JAVA_HOME=/Library/Java/JavaVirtualMachines/jdk-15.jdk/Contents/Home
```

最后，您已经准备好使用 Java 15 编译和运行程序了。我们将使用 JShell，一个交互式 REPL 命令行工具，用于快速测试 Java 15 的新特性。

值得注意的是，Java 15 中发布的许多特性都在预览版中。这意味着尽管他们现在正在全力工作，但将来可能会有所改变。有些可能会成为标准，或者在下一个发布周期中被删除。为了测试预览功能，您需要在运行 JShell 或 Java 程序时显式设置`--enable-preview`，如下所示:

```
jshell --enable-preview javac --release 15 --enable-preview Author.java
```

在接下来的几节中，让我们讨论 Java 15 中的重大语言变化。

# 1.密封类(预览)

Kotlin 中的密封类已经存在一段时间了，Java 15 最终引入了这个特性，以便更好地控制继承。

顾名思义，密封类允许您将类层次结构限制或允许给某些类型。

这对于模式匹配非常有用，因为您有特定数量的类要在它们之间进行`switch`。

以下语法在 Java 15 中定义了一个密封类:

```
public sealed class Vehicle permits Car, Bike, Truck { ... }
```

所以，上面的代码意味着，只有在关键字`permits`之后定义的类才被允许扩展`Vehicle` `sealed`类。

如果您已经在与`Vehicle`相同的文件中定义了类`Car`、`Bike`和`Truck`，您可以省略关键字 permissions，编译器将隐式处理它，如下所示:

```
sealed class Vehicle {...}final class Car extends Vehicle {...} 
final class Bike extends Vehicle {...} 
final class Truck extends Vehicle {...}
```

正如你在上面看到的，我们已经定义了每个类的最终修饰符。现在，你需要记住密封类的一个重要规则:每个允许的类都必须设置一个显式修饰符。可以是`final`也可以是`sealed`或者`non-sealed`。

以下是每个修改器如何影响继承:

*   声明为`final`的允许子类不能进一步扩展。
*   声明为`sealed`的允许子类可以进一步扩展，但是只能由该子类允许的类来扩展。
*   一个被允许的子类可以被声明`non-sealed`可以被任何类进一步扩展。超类不能进一步限制这个类层次结构中的子类。

在 Java 15 之前，开发者只能使用`final`关键字或范围修饰符来控制继承。因此，在定义类层次结构时，密封类为 Java 开发人员带来了额外的灵活性。

Java 的反射 API 还获得了两种处理密封类的新方法:

```
java.lang.constant.ClassDesc[] getPermittedSubclasses();boolean isSealed()
```

# 2.记录(第二次预览)

记录是作为 Java 14 中的预览特性引入的，目的是在编写基于 POJO 的数据载体类时减少样板代码。这在 Kotlin 中以数据类的形式存在了很长时间。

现在，有了 Java 15，记录得到了第二次预览。虽然没有任何重大变化(只是一些小的增加)，但仍然有一些您应该知道的主要说明和限制:

*   在 Java 15 之前，人们可以在记录中声明原生方法(尽管这不是一个好主意)。现在，JEP 明确禁止在记录中声明本机方法。可以理解的是，定义一个`native`方法通过引入外部状态依赖偷走了 USP 记录。
*   对应于记录类的记录组件的隐式声明字段是`final`，现在不应该通过反射修改，因为它将抛出`IllegalAccessException`。

记录意味着数据载体类，您应该完全避免在其中定义本机方法。

## 密封类型的记录

我们知道记录是最终的，不能延长。令人高兴的是，记录可以实现接口。

因此，您可以定义一个密封的接口，并通过以下方式在您的记录中实现它们:

```
sealed interface Car permits BMW, Audi { ... } 
record BMW(int price) implements Car { ... } 
record Audi(int price, String model) implements Car { ... }
```

## 地方志

也可以在方法中定义记录来存储中间值。与本地类不同，本地记录是隐式静态的。这意味着它们不能访问变量和封闭方法的实例成员，这实际上很好，因为它防止了记录捕获值。

本地记录对于以前必须创建助手记录的 Java 开发人员来说是一大福音。

使用以下方法，了解本地记录的引入如何帮助在流 API 中执行值的计算:

```
List<Merchant> findTopMerchants(List<Merchant> merchants, int month){
// Local record
record MerchantSales(Merchant merchant, double sales) {}return merchants.stream().map(merchant -> new MerchantSales(merchant, computeSales(merchant, month)))
.sorted((m1, m2) -> Double.compare(m2.sales(), m1.sales()))
.map(MerchantSales::merchant)
.collect(toList());}
```

# 结论

虽然上面两个是 Java 15 中的两个主要语言特性，但我们在第二个预览版中也有模式匹配以获得用户反馈，文本块现在是一个标准特性，最重要的是一个新的隐藏类特性。

隐藏类是与框架开发者相关的 JVM 特性。它允许通过使用`Lookup::defineHiddenClass`定义类实现来使它们不可被发现。这样做的话，既不能使用`Class.forName`找到这样的类，也不能在字节码中引用它们。

这些是 Java 15 中引入的主要变化。