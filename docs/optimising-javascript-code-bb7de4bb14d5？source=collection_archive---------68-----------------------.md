# 优化 Javascript 代码

> 原文：<https://towardsdatascience.com/optimising-javascript-code-bb7de4bb14d5?source=collection_archive---------68----------------------->

## 有些 web 应用程序需要一段时间才能加载，这种情况并不少见。这可能是因为计算量过大或文件过大。这篇文章着眼于代码优化的两种主要方法，以及如何在谷歌的闭包编译器的帮助下完成这些。

![](img/002177af37ef77aa3888acf32300e59f.png)

照片由[蒂尔扎·范·迪克](https://unsplash.com/@tirzavandijk?utm_source=medium&utm_medium=referral)在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄

# 状态

在 web 应用程序中，javascript 通常有两个可以克服的瓶颈——小型化和代码优化。

## 缩小

在处理网站时，文件大小对应用程序的性能起着至关重要的作用。这是因为任何代码首先需要下载，然后运行。缩小的行为去除了源代码中任何多余的字符，从而产生一个更紧凑的(尽管可读性通常更差)文件。这方面的一个例子如下:

```
var array = [];for (var i = 0; i < 20; i++) {var j = Math.random()*i/100
  array.push(j)}
```

用[https://jscompress.com](https://jscompress.com)(它使用[丑化 JS 3](https://github.com/mishoo/UglifyJS2) 和[巴别塔缩小](https://github.com/babel/minify))缩小它，我们得到 32%的压缩，节省了 0.03 kb 和以下更紧凑的代码:

```
for(var j,array=[],i=0;20>i;i++)j=Math.random()*i/100,array.push(j);
```

缩小器的工作原理是从代码中删除不必要的字符和空白，但除此之外一般不会改变太多内容。

## 自我语言翻译

在编译语言中，优化通常处理复杂循环的展开、缓存阻塞或选择正确的列/行顺序来索引矩阵等过程。然而，这种技术计算方式不一定是 JS 工作流程的一部分。

相反，我们希望简化复杂的方程(例如 python`sympy`符号引擎可以帮助将一个方程重写为一个计算强度较低的方程)，或者我们可以将长变量重命名为较短的变量，以帮助简化过程。

这就是 google closure 编译器发挥作用的地方。

# Google 闭包编译器

这是一个 javascript 编译器，它以一种紧凑和更有效的方式重写和精简现有代码。与传统编译不同，闭包编译器不会将 javascript 转换成机器代码，而是转换成更好的版本。

## 装置

它的安装很简单，可以通过 node.js 完成。

```
npm i google-closure-compiler
```

## 运转

同样，它的运行也相当直观。Javascript 文件带有`--js`标志(在本例中，我使用了`*`通配符来选择所有的`.js`文件)，这些文件被处理成一个单独的输出文件(`compiled.js)`，可以在它们的位置导入。

```
npx google-closure-compiler --js=*.js --js_output_file=compiled.js 
```

*注意:如果文件顺序是必需的，那么你可以通过使用多个* `*--js=*` *标志顺序加载所有文件来解决这个问题。*

## 那么闭包编译器做什么呢？

正如你所期待的，谷歌提供了许多工具和技巧来改进代码。最简单的方法是查看长变量名并将其缩短:

```
function hello(longName) {
  alert('Hello, ' + longName);
}
hello('New User');
```

已更改为:

```
function hello(a){alert("Hello, "+a)}hello("New User");
```

**更高级的选项**可以进一步简化代码，它解析代码以删除死的(未使用的)函数并检查语法。在[文档](http://developers.google.com/closure/compiler/docs/)中给出了一个例子，其中下列功能:

```
function unusedFunction(note) {
  alert(note['text']);
}

function displayNoteTitle(note) {
  alert(note['title']);
}

var flowerNote = {};
flowerNote['title'] = "Flowers";
displayNoteTitle(flowerNote);
```

简化为:

```
function unusedFunction(a){alert(a.text)}function displayNoteTitle(a){alert(a.title)}var flowerNote={};flowerNote.title="Flowers";displayNoteTitle(flowerNote);
```

然后简化为:

```
var a={};a.title="Flowers";alert(a.title);
```

# 结论

Googles 的 closure compiler 不仅缩减了 javascript 代码，还降低了它的复杂性。静态方程如`var x = 17 + 2` 被重写(`var x=42`)以避免不必要的运行时计算，ECMAScript 6 代码可以被转换成 ECMAScript 3——这样它就可以在旧的浏览器上运行。“编译器”使用简单，大大减少了浏览器需要做的工作。

## 有用的链接

*   [https://github.com/google/closure-compiler](https://github.com/google/closure-compiler)
*   [https://developers.google.com/closure/compiler/](https://developers.google.com/closure/compiler/)
*   [https://www.npmjs.com/package/google-closure-compiler](https://www.npmjs.com/package/google-closure-compiler)