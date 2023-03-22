# 如何在 Javascript 中使用 Generator 和 yield

> 原文：<https://towardsdatascience.com/how-to-use-generator-and-yield-in-javascript-d1771bf698cd?source=collection_archive---------49----------------------->

## 更好的编程

## 你知道 JavaScript 里有个叫“生成器”的东西吗？

![](img/c12e6177c5ecc94f42febc76fe34da1c.png)

由[Thomas re abourg](https://unsplash.com/@thomasreaubourg?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)在 [Unsplash](/s/photos/wind-turbine?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) 拍摄的照片

前段时间我写了一篇文章解释了[生成器的概念以及如何在 Python](https://medium.com/@bajcmartinez/how-to-use-generator-and-yield-in-python-c481cea097d7) 中使用它们，但是你知道 JavaScript 也有自己版本的生成器吗？这其实是一个很多开发 JavaScript 应用的人都不知道它存在的概念，所以今天我们要介绍的是 JavaScript 中的生成器。

# 什么是发电机？

在 ES6 中，我们引入了伟大的新功能，如[箭头函数](https://medium.com/@bajcmartinez/when-not-to-use-javascript-arrow-functions-a7340630d4a8)、[扩展操作符](https://medium.com/@bajcmartinez/how-to-use-the-spread-operator-in-javascript-3aff104adb71)和生成器等等，但是什么是生成器呢？生成器是一个与普通函数相反的函数，它允许函数退出并在以后重新进入，并在重新进入时保留其上下文(变量绑定)。

让我们把它分解开来，一步一步地研究发电机，这样我们都可以理解它们是如何工作的。当我们执行一个常规函数时，解释器会将所有代码运行到那个函数中，直到函数完成(或者抛出一个错误)。这就是所谓的**运行至完成**模型。

让我们以一个非常简单的函数为例:

```
function regularFunction() {
    console.log("I'm a regular function")
    console.log("Surprise surprice")
    console.log("This is the end")
}regularFunction()-----------------
Output
-----------------
I'm a regular function
Surprise surprice
This is the end
```

还没有什么新奇的东西，正如你所期望的，这是一个常规函数，它会一直执行到最后或者返回值。但是如果我们只想在任意点停止函数来返回值，然后继续呢？这时，发电机就出现了。

# 我的第一个生成器函数

```
function* generatorFunction() {
    yield "This is the first return"
    console.log("First log!")
    yield "This is the second return"
    console.log("Second log!")
    return "Done!"
}
```

在我们执行这个函数之前，你可能会想知道一些事情，首先什么是`function*`？这就是我们用来将函数声明为生成器的语法。而`yield`呢？与 return 不同的是，`yield`将通过保存函数的所有状态来暂停函数，并在随后的调用中从该点继续。在这两种情况下，表达式都将返回给调用方执行。

我们的功能到底发生了什么？让我们通过调用函数来找出答案:

```
generatorFunction()-----------------
Output
-----------------
generatorFunction {<suspended>} {
    __proto__: Generator
    [[GeneratorLocation]]: VM272:1
    [[GeneratorStatus]]: "suspended"
    [[GeneratorFunction]]: ƒ* generatorFunction()
    [[GeneratorReceiver]]: Window
    [[Scopes]]: Scopes[3]
}
```

等等，什么？当我们调用一个生成器函数时，这个函数不会被自动触发，而是返回一个迭代器对象。这个对象的特别之处在于，当调用方法 next()时，生成器函数的主体被执行，直到第一个`yield`或`return`表达式。让我们来看看它的实际应用:

```
const myGenerator = generatorFunction()
myGenerator.next()-----------------
Output
-----------------
{value: "This is the first return", done: false}
```

如前所述，生成器一直运行到第一条`yield`语句，并生成一个包含`value`属性和`done`属性的对象。

```
{ value: ..., done: ... }
```

*   `value`属性等于我们产生的值
*   `done`属性是一个布尔值，它只在生成器函数返回值时设置为`true`。(未屈服)

让我们再次调用`next()`，看看我们会得到什么

```
myGenerator.next()-----------------
Output
-----------------
First log!
{value: "This is the second return", done: false}
```

这次我们首先看到我们的生成器主体中的`console.log`被执行并打印`First log!`，以及第二个生成的对象。我们可以继续这样做:

```
myGenerator.next()-----------------
Output
-----------------
Second log!
{value: "Done!", done: true}
```

现在第二个`console.log`语句被执行，我们得到一个新的返回对象，但是这次属性`done`被设置为`true`。

属性的值不仅仅是一个标志，它是一个非常重要的标志，因为我们只能迭代一个生成器对象一次！。不相信我？尝试再次呼叫`next()`:

```
myGenerator.next()-----------------
Output
-----------------
{value: undefined, done: true}
```

还好它没有崩溃，但是我们只是得到了未定义的结果，因为`value`和`done`属性仍然设置为 true。

# 产生迭代器

在我们进入一些场景之前，yield 操作符还有一个特殊性，那就是`yield*`。让我们通过创建一个函数来解释它，这个函数允许我们迭代一个数组，我们可以天真地想到这样做:

```
function* yieldArray(arr) {
    yield arr
}const myArrayGenerator1 = yieldArray([1, 2, 3])
myArrayGenerator1.next()-----------------
Output
-----------------
{value: Array(3), done: false}
```

但这并不是我们想要的，我们想要产生数组中的每个元素，所以我们可以尝试这样做:

```
function* yieldArray(arr) {
    for (element of arr) {
        yield element
    }
}const myArrayGenerator2 = yieldArray([1, 2, 3])
myArrayGenerator2.next()
myArrayGenerator2.next()
myArrayGenerator2.next()-----------------
Output
-----------------
{value: 1, done: false}
{value: 2, done: false}
{value: 3, done: false}
```

现在我们得到了想要的结果，但是我们能做得更好吗？是的，我们可以:

```
function* yieldArray(arr) {
    yield* arr
}const myArrayGenerator3 = yieldArray([1, 2, 3])
myArrayGenerator3.next()
myArrayGenerator3.next()
myArrayGenerator3.next()-----------------
Output
-----------------
{value: 1, done: false}
{value: 2, done: false}
{value: 3, done: false}
```

太棒了，通过使用 yield* expression，我们可以迭代操作数，并产生它返回的每个值。这适用于其他生成器，数组，字符串，任何可迭代的对象。

既然您已经了解了 JavaScript 中的所有生成器，那么它们有什么用处呢？

# 发电机的使用

关于生成器的伟大之处在于它们是惰性计算的，这意味着在调用`next()`方法后返回的值，只有在我们明确请求后才被计算。这使得生成器成为解决多种场景(如下所示)的好选择。

# 生成一个无限序列

正如我们在 Python 文章中看到的，生成器适合生成无限序列，这可以是从质数到简单计数的任何东西:

```
function* infiniteSequence() {
    let num = 0
    while (true) {
        yield num
        num += 1
    }
}for(i of infiniteSequence()) {
    if (i >= 10) {
        break
    }
    console.log(i)
}-----------------
Output
-----------------
0
1
2
3
4
5
6
7
8
9
```

注意，在这种情况下，我在`i >= 10`时退出循环，否则，它将永远运行(或者直到手动停止)。

# 实现 iterables

当你需要实现一个迭代器时，你必须用一个`next()`方法手工创建一个对象。此外，您必须手动保存状态。

假设我们想创建一个只返回`I`、`am`、`iterable`的 iterable。如果不使用发电机，我们将不得不做这样的事情:

```
const iterableObj = {
  [Symbol.iterator]() {
    let step = 0;
    return {
      next() {
        step++;
        if (step === 1) {
          return { value: 'I', done: false};
        } else if (step === 2) {
          return { value: 'am', done: false};
        } else if (step === 3) {
          return { value: 'iterable.', done: false};
        }
        return { value: '', done: true };
      }
    }
  },
}
for (const val of iterableObj) {
  console.log(val);
}-----------------
Output
-----------------
I
am
iterable.
```

有了发电机，这就简单多了:

```
function* iterableObj() {
    yield 'I'
    yield 'am'
    yield 'iterable.'
}for (const val of iterableObj()) {
  console.log(val);
}-----------------
Output
-----------------
I
am
iterable.
```

# 更好的异步？

一些人认为生成器有助于改进承诺和回调的使用，尽管我更喜欢简单地使用 await/async。

# 警告

当我们使用发电机时，并不是所有的东西都闪闪发光。设计有一些限制，有两个非常重要的考虑因素:

*   生成器对象只能一次性访问。一旦用完，就不能再迭代了。为此，您必须创建一个新的生成器对象。
*   生成器对象尽可能不允许随机访问，例如数组。由于值是一个接一个生成的，您无法获得特定索引的值，您将不得不手动调用所有的`next()`函数，直到您到达所需的位置，但是之后，您将无法访问先前生成的元素。

# 结论

生成器函数对于优化我们的应用程序的性能非常有用，并且有助于简化构建迭代器所需的代码。

我希望您现在已经很好地理解了 JavaScript 中的生成器，并且可以在您的下一个项目中使用它们。

感谢阅读！