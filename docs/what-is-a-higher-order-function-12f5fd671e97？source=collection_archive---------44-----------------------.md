# 什么是高阶函数？

> 原文：<https://towardsdatascience.com/what-is-a-higher-order-function-12f5fd671e97?source=collection_archive---------44----------------------->

## 编程；编排

## 了解什么是高阶函数，如何创建它们以及如何使用它们

![](img/29e38ebb9ed3867db261ccc102fba581.png)

感谢 [Clément H](https://unsplash.com/@clemhlrdt) 分享他们在 [Unsplash](https://unsplash.com/photos/95YRwf6CNw8) 上的工作

**高阶函数**是将函数作为参数或返回函数的函数。这种类型的函数在许多编程语言中都有实现，包括 Go、JavaScript、Python 等；它们往往是面试中使用的一个问题。我多次与开发人员谈论这个概念，他们并不熟悉这个名称，尽管他们每天都在不知不觉中使用它，所以我决定用一篇帖子来讨论这个主题，这样我们就可以清楚地知道它们是什么以及它们如何有用。

由于这个主题在多种编程语言中被广泛使用，我将提供 JavaScript 和 Python 的代码示例。

# 一些简单的例子

让我们看一些高阶函数的简单示例，进入主题并使用代码，然后我们将进一步构建我们使用的一些常见函数，它们是高阶函数的示例。

# 将函数作为参数

首先，让我们构建一个名为`doOperation`的非常简单的函数，它有 3 个参数:

*   功能操作
*   数字 1
*   数字 2

此外，我们将创建一个名为`sumBothNumbers`的操作，它将简单地返回两个数的和。

Python:

```
def doOperation(operation, number1, number2):
    return operation(number1, number2)

def sumBothNumbers(number1, number2):
    return number1 + number2

doOperation(sumBothNumbers, 3, 5)------------
Output
------------
8
```

JavaScript:

```
function doOperation(operation, number1, number2) {
    return operation(number1, number2)
}function sumBothNumbers(number1, number2) {
    return number1 + number2
}doOperation(sumBothNumbers, 3, 5)------------
Output
------------
8
```

虽然在这种特殊情况下，拥有`doOperation`函数似乎是多余的，如果不是错误的话，但在某些情况下它可能是有用的，例如，`doOperation`函数可以是我们可以用自己的操作来扩展的库的一部分。

# 返回一个函数

接下来，我们将构建一个返回函数的高阶函数。我们的函数将被称为`multiplyBy`,它将接受一个数字作为参数，并返回一个将其输入乘以该数字的函数。

Python:

```
def multiplyBy(multiplier):
    def result(num):
        return num * multiplier
    return resultmultiplyByThree = multiplyBy(3)
multiplyByThree(4)------------
Output
------------
12
```

JavaScript:

```
function multiplyBy(multiplier) {
    return function result(num) {
        return num * multiplier
    }
}multiplyByThree = multiplyBy(3)
multiplyByThree(4)------------
Output
------------
12
```

# 构建过滤器()，映射()和减少()

让我们使用高阶函数(实际上是高阶函数)来构建一个简单版本的流行函数。

# 过滤器()又名过滤()

`filtering`函数将有两个参数，一个`array`和一个`test`函数，它将返回一个包含所有通过测试的元素的新数组。

Python:

```
def filtering(arr, test):
    passed = []
    for element in arr:
        if (test(element)):
            passed.append(element)
    return passeddef isSuperNumber(num):
    return num >= 10filtering([1, 5, 11, 3, 22], isSuperNumber)------------
Output
------------
[11, 22]
```

JavaScript:

```
function filtering(arr, test) {
    const passed = []
    for (let element of arr) {
        if (test(element)) {
            passed.push(element)
        }
    }
    return passed
}function isSuperNumber(num) {
    return num >= 10
}filtering([1, 5, 11, 3, 22], isSuperNumber)------------
Output
------------
> (2) [11, 22]
```

可以看到，我们的`filter()`函数非常容易编码和使用，例如从一个数组中获取所有的超级数😛。

# map()又名映射()

函数`mapping`将接受两个参数:一个`array`和一个`transform`函数，它将返回一个新的转换后的数组，其中每一项都是对原始数组的每个元素调用`transform`函数的结果。

Python:

```
def mapping(arr, transform):
    mapped = []
    for element in arr:
        mapped.append(transform(element))
    return mappeddef addTwo(num):
    return num+2mapping([1, 2, 3], addTwo)------------
Output
------------
[3, 4, 5]
```

JavaScript:

```
function mapping(arr, transform) {
    const mapped = []
    for (let element of arr) {
        mapped.push(transform(element))
    }
    return mapped
}function addTwo(num) {
    return num + 2
}mapping([1, 2, 3], addTwo)------------
Output
------------
> (3) [3, 4, 5]
```

# 减少()又名减少()

函数`reducing`将接受 3 个参数:一个`reducer`函数、一个用于累加器的`initial value`和一个`array`。对于数组中的每一项，都调用 reducer 函数，并向其传递累加器和当前数组元素。返回值被分配给累加器。当减少完列表中的所有项目后，将返回累计值。

Python:

```
def reducing(reducer, initial, arr):
    acc = initial
    for element in arr:
        acc = reducer(acc, element)
    return accdef accum(acc, curr):
    return acc + currreducing(accum, 0, [1, 2, 3])------------
Output
------------
6
```

JavaScript:

```
function reducing(reducer, initial, arr) {
    let acc = initial
    for (element of arr) {
        acc = reducer(acc, element)
    }
    return acc
}function accum(acc, curr) {
    return acc + curr
}reducing(accum, 0, [1, 2, 3])------------
Output
------------
6
```

# 结论

下一次，当你进行访问时，或者只是看到一个函数被返回或者作为参数的模式，你就会知道我们在处理高阶函数。

今天，我第一次介绍了一篇涵盖多种语言的文章，如果你觉得这是展示和比较它们的好方法，或者如果你认为这是一个糟糕的想法，请在评论中或通过 twitter 告诉我，我很乐意听到你的想法。

非常感谢你的阅读！