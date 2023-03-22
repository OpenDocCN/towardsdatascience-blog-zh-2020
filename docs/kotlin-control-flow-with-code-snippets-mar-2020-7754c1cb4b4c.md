# kot Lin——用代码片段控制流程(2020 年 3 月)

> 原文：<https://towardsdatascience.com/kotlin-control-flow-with-code-snippets-mar-2020-7754c1cb4b4c?source=collection_archive---------30----------------------->

## 初学者的 kot Lin—# 2:控制流

## 如果，当，为，而，当，中断并继续…

![](img/f3b36766c6ca47f39cdfe93c5f8b9136.png)

约翰·洛克伍德在 [Unsplash](https://unsplash.com/s/photos/road?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) 上拍摄的照片

C控制流运算符可用于在计算中从两条或多条可能的路径中选择一条合适的路径。

在本文中，我们将介绍 Kotlin 中的以下控制流:

1.  `if`表情
2.  `when`表情
3.  `for`循环
4.  `while`和`do...while`循环
5.  `break`操作员
6.  `continue`操作员

# `if`表情

1.  `if`语句的传统用法:

```
val num1 = 10
val num2 = 20

**if** (num1 > num2) {
    print("num1 > num2)
} **else if** (num1 < num2) {
    print("num1 < num2")
} **else** {
    print("num1 == num2")
}// Output: num1 < num2
```

2.而不是三元运算符(在 Java 中— condition？val1 : val2):

```
val num = 0
val isZero = **if** (num == 0) true **else** false
```

> 仅供参考:kotlin 中没有三元运算符。

3.在 Kotlin 中，`if`也可以返回值(我们在上面一点中已经看到)。因此，它也可以用在函数的返回语句中。

```
fun getMinValue(a: Int, b: Int): Int {
    return if (a <= b) {
        a
    } else {
        b
    }
}
```

> 参考:当使用`if`操作符返回值时，`if`和`else`块的最后一行应该是你想要返回的结果值。

# `when`表情

1.  `when`是 switch case 的替代品(java 中)。你可以说，`when`表情是有超能力的开关格。`when`表达式可以用来返回值，类似于`if`表达式。
2.  `when`的传统用法(类似于开关盒):

```
val color = "RED"
when(color) {
    "RED" -> print("Color is RED")
    "GREEN" -> print("Color is GREEN")
    "BLUE" -> print("Color is BLUE")
    else -> print("Color is Unknown")
}// Output: Color is RED
```

3.`when`表情的超能力能力:

```
fun getValue(input: Any): String {
    return when (input) {
        **is String ->** "Input is a String"
        **in 'A'..'Z' ->** "Input is between A to Z"
        **0 ->** "Input is 0"
        **1, 2 ->** "Input is either 1 or 2"
        **in 3..10 ->** "Input is between 3 to 10"
        **!in 11..100 ->** "Input is not between 11 to 100"
        **else ->** "Inside else case"
    }
}
```

*   `getValue("Any String")`:输出将是`Input is a String`。
*   `getValue('D')`:输出为`input is between A to Z`。
*   `getValue(0)`:输出将为`Input is 0`。
*   `getValue(5)`:输出将为`Input is between 3 to 10`。
*   `getValue(256)`:输出将为`Input is not between 11 to 100`。
*   `getValue(55)`:输出将是`Inside else case`。

> 参考消息:`is`或`!is`可用于检查输入的类型。`in`或`!in`将用于检查范围。

# `for`循环

> `*for*` *循环迭代*可迭代*的任何东西(具有提供* `*Iterator*` *对象的* `*iterator()*` *函数的任何东西)，或者本身是* `*Iterator*` *的任何东西。*

1.  *`in` —最简单的 for 循环，遍历列表中的每个元素*

```
*val fruits = listOf("Apple", "Banana", "Cherries", "Guava")
for (fruit **in** fruits) {
    println(fruit)
}// Output: Apple, Banana, Cherries, Guava*
```

*2.`..` —迭代 0(含)到 5(含)之间的范围*

```
*for (number in 0..5) {
    println(number)
}// Output: 0 through 5*
```

*3.`withIndex()` —使用当前项目的索引遍历列表。*

```
*val fruits = listOf("Apple", "Banana", "Cherries", "Guava")
for ((index, fruit) in fruits.**withIndex**()) {
    println("$index - $fruit")
}// Output: 0 - Apple, 1 - Banana, 2 - Cherries, 3 - Guava*
```

> ***注:**查看关于在 Kotlin 中使用 for 循环的 [8 种不同方法的详细文章。](https://medium.com/quick-code/all-you-need-to-know-about-kotlin-for-loop-144dc950271d)如果你想知道如何使用`for`循环来迭代`Map`，那么别忘了看看那篇文章的第 8 点。*

# *`while`和 do…while 循环*

1.  *类似于`while`循环的传统用例(基于它将循环通过语句块的条件):*

```
*var i = 5 
while (i > 0) {
    println(i)
    i--
}// Ouput: 5 through 1*
```

*2.在`do...while`中，首先执行程序块，然后检查条件:*

```
*var i = 5
do {
    println(i)
    i--
} while (i > 0)// Ouput: 5 through 1*
```

# *`break`操作员*

1.  *`break`运算符(默认，即无任何标签)用于终止最近的封闭循环:*

```
*for (y in 0..5) {
   if (y > 5) break
   println("y: $y")
}// Output: 
y: 0 
y: 1 
y: 2 
y: 3*
```

*2.一个循环在另一个循环中的例子，其中`break`操作符仅终止最近的循环:*

```
*for (x in 0..2) {
    for (y in 0..2) {
        if (y > 0) break
        println("x:$x - y:$y")
    }
}// Output:
x:0 - y:0 
x:1 - y:0 
x:2 - y:0*
```

*3.`break`带有[标签](https://kotlinlang.org/docs/reference/returns.html#break-and-continue-labels)的操作器不仅可用于终止最近的回路，也可用于终止外部回路:*

```
*outer@ for (x in 0..2) {
    for (y in 0..2) {
        if (y > 0) break@outer
        println("x:$x - y:$y")
    }
}// Output:
x:0 - y:0*
```

# *`continue`操作员*

1.  *`continue`运算符终止以下语句的执行，并跳转到封闭循环中的第一条语句:*

```
*for (i in 0..10) {
    if (i%2 == 0) continue
    print("$i ")
}// Output: 1 3 5 7 9*
```

*2.`continue`带[标签的操作员](https://kotlinlang.org/docs/reference/returns.html#break-and-continue-labels):*

```
*outer@ for (x in 0..3) {
    for (y in 0..3) {
        if (y > 1) continue@outer
        println("x:$x - y:$y")
    }
}// Ouput:
x:0 - y:0 
x:0 - y:1 
x:1 - y:0 
x:1 - y:1 
x:2 - y:0 
x:2 - y:1 
x:3 - y:0 
x:3 - y:1*
```

> ***注:**如果你想要一篇关于`return, break and continue`(即 Kotlin 中的跳转表达式)的带有[标签](https://kotlinlang.org/docs/reference/returns.html#break-and-continue-labels)和代码片段的详细文章，那么请在评论中提及**“是的，我感兴趣……”**。当我发布相同的文章时，我会特别回复你。*

# *您的机会…*

*加入我的部落后，获得我的个人 Java 收藏清单作为免费的欢迎礼物。 [*马上获取！*](https://mailchi.mp/809aa58bc248/anandkparmar)*

# *关于作者*

*Anand K Parmar 是一名软件工程师，热爱设计和开发移动应用程序。他是一名作家，发表关于计算机科学、编程和个人理财的文章。在 [LinkedIn](https://www.linkedin.com/in/anandkparmar/) 或 [Twitter](https://twitter.com/anandkparmar_) 上与他联系。下面是他的最新文章。*

*[](https://medium.com/quick-code/all-you-need-to-know-about-kotlin-for-loop-144dc950271d) [## 科特林——2 分钟“for”循环

### 关于 Kotlin 中的“for”循环，您只需要知道

medium.com](https://medium.com/quick-code/all-you-need-to-know-about-kotlin-for-loop-144dc950271d) [](https://medium.com/swlh/differences-between-data-structures-and-algorithms-eed2c1872cfc) [## 每个初学者都应该知道数据结构和算法之间的区别

### 理解计算机科学基础的简单指南

medium.com](https://medium.com/swlh/differences-between-data-structures-and-algorithms-eed2c1872cfc)*