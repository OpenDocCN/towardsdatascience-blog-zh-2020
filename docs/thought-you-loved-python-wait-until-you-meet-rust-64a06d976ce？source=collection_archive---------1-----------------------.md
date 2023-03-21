# 以为你爱 Python？等到你遇到铁锈

> 原文：<https://towardsdatascience.com/thought-you-loved-python-wait-until-you-meet-rust-64a06d976ce?source=collection_archive---------1----------------------->

## 意见

## 小众现象如何连续第五年成为 StackOverflow 最受欢迎的语言

![](img/cc05c5da419ab48ebc0346254e0a1218.png)

有时候旧东西比你想象的更令人向往。妮可·盖里在 [Unsplash](https://unsplash.com/s/photos/rusty?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) 上的照片

![“T](img/342a2c9be12278887a431eff4c084f0d.png) “过去的技术拯救未来。”这是《铁锈》的创作者格雷顿·霍尔(Graydon Hoare)对他想要实现的目标的描述。

这是 Rust 的关键标志之一:使用学术界熟知但在当代编程语言中很少实现的技术。古老、可靠、有时被遗忘的技术。但最重要的是，它工作得非常好。

这些技术主要用于一件事:安全。

听起来很无聊？如果你问社区，它不是。根据今年的 StackOverflow 开发者调查，多达 86.1%的 Rust 开发者喜欢这种语言，这使它成为自 2016 年以来最受欢迎的语言。

你会认为软件开发人员是这个星球上最具创新精神的人。然而，Rust 与*“快速移动并打破东西”*咒语正好相反。然而，Rust 开发人员几乎肯定会学到他们以前从未听说过的概念。

从一些开发人员对代数数据类型的系统编程的新奇感到 Rust 自己的内存安全方法:每个开发人员都可以找到新的、非常有用的东西来学习。还有更多理由让[爱上](https://stackoverflow.blog/2020/01/20/what-is-rust-and-why-is-it-so-popular/)铁锈。

[](/you-want-to-learn-rust-but-you-dont-know-where-to-start-fc826402d5ba) [## 你想学 Rust 但是不知道从哪里开始

### Rust 初学者的完整资源

towardsdatascience.com](/you-want-to-learn-rust-but-you-dont-know-where-to-start-fc826402d5ba) 

# 更高的内存安全性，无需垃圾收集

每种编程语言都面临着一个挑战，那就是如何安全有效地管理计算机内存。例如，Python 有一个垃圾收集器，在程序运行时不断寻找不再使用的内存并清理它。

在其他语言中，如 C 和 C++，程序员必须显式地分配和释放内存。因为所有与内存相关的问题都在程序运行前被清除了，所以这种方法对优化性能更好。

另一方面，内存是开发人员需要一直考虑的另一件事。这就是为什么用 C 写一个程序要比用 Python 长得多的原因之一，即使它在一天结束时做同样的事情。

Rust 走的是另一条路:内存是在编译时通过所有权系统分配的。这是一个干净利落的方法，可以确保不用的数据被清理掉，而不用强迫程序员一直考虑分配和释放内存。

![](img/ecb88ed45b92a930234e7697fdc91aa5.png)

Rust 使用旧技术进行有效的内存管理。安迪·福肯纳在 [Unsplash](https://unsplash.com/s/photos/rusty?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) 上的照片

基本上，所有权是三个[规则](https://doc.rust-lang.org/book/ch04-01-what-is-ownership.html)的集合:

1.  Rust 中的每个值都有一个名为 owner 的变量。
2.  一次只能有一个所有者。
3.  当所有者超出范围时，该值将被丢弃，从而释放内存。

一个简单的例子是在 Rust 中指定一个向量:

```
fn main() {     
    let a = vec![1, 2, 3];     
    let b = a;                 
    println!("a: {:?}", b); 
}
```

在第二行中，创建了拥有者为`a`的向量`[1, 2, 3]`。此后，`b`成为了载体的主人。因为在 print 语句中调用了正确的所有者，所以该程序在执行时会编译并返回预期的结果:

```
a: [1, 2, 3]
```

另一方面，您可以尝试用它以前的所有者`a`来调用 vector，就像这样:

```
fn main() {
    let a = vec![1, 2, 3];
    let b = a;
    println!("a: {:?}", b, a);
}
```

在这种情况下，编译器抛出一个错误，因为第三行中已经删除了`a`。这个主题还有很多更深入的内容，但这是基本的想法。

相比之下，Python 会在第二种情况下运行。它的垃圾收集器只有在最后一次被调用后才会丢弃`a`，这对开发人员来说很好，但从内存空间的角度来看就不那么好了。

在 C 语言中，事情会稍微复杂一点:你必须为`a`分配内存空间，然后将它指向 vector，然后为`b`分配更多的内存空间，将`b`指向`a`，最后当你完成时，释放由`a`和`b`占用的空间。

从这个意义上说，Rust 对内存的处理方式是开发速度和性能之间的折衷。虽然它不像 Python 那么容易编写，但是一旦你理解了所有权的概念，它也不会像 C 语言那样笨拙。

另一方面，效率是相当惊人的:例如，开发团队 [Tilde](https://www.tilde.io) 在 Rust 中重写了一些 JavaHTTP 片段后，设法减少了 90%的内存使用量[。](https://www.rust-lang.org/static/pdfs/Rust-Tilde-Whitepaper.pdf)

![](img/de944fead90eb8bf1000b8de15c003e4.png)

谁说铁锈不能吸引人？安娜斯塔西娅·塔拉索娃在 [Unsplash](https://unsplash.com/s/photos/rusty?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) 上拍摄的照片

# 静态打字不难看

这几乎是动态类型化和静态类型化之间的宗教战争。虽然用动态类型的语言来开发软件要容易得多，但是代码很快就会变得不可维护。这就是为什么 Python 代码与 C 语言相比很难维护的原因之一。

另一方面，必须声明每个变量的类型 C-style 会变得相当烦人。如果你曾经试图在 C 语言中使用一个返回`float`类型的函数中使用一个`double`，你就会明白我的意思。

Rust 走的是中间路线:它是一个静态类型系统，但它只需要程序员指定顶级类型，如函数参数和常数。在函数体内，允许 Python 风格的类型推断。

Rust 的一个特别[有用的特性](https://learning-rust.github.io/docs/e3.option_and_result.html)是它也有一个`None`类型。这允许您在编译时处理异常，从而保证程序在最终用户处平稳运行。考虑这个例子，我们可以得到一个人的全名，而不管他是否有中间名:

```
fn get_full_name(fname: &str, mname: Option<&str>, lname: &str) -> String { 
    match mname {
        Some(n) => format!("{} {} {}", fname, n, lname),
        None => format!("{} {}", fname, lname),
    } 
}fn main() {
    println!("{}", get_full_name("Ronald", None, "McDonald"));
    println!("{}", get_full_name("Dwight", Some("P."), "Eisenhower"));
}
```

虽然其他语言中也存在各种版本的`None`变通办法，但它以一种简洁的方式展示了 Rust 的雄心:不使事情变得太难编写，同时尽可能保持代码的持久性和可维护性。

# 一种巧妙的系统编程方法

虽然 Python 是一种通用编程语言，但 Rust 和 C 一样，绝对适合系统编程。虽然 Rust 不是为最终用户开发应用程序的理想语言，但它非常适合构建为其他软件提供服务的软件。

因此，效率是核心问题。零成本抽象就是最好的证明，它在解释代码的同时将内存使用保持在最低水平。正如 C++的发明者比雅尼·斯特劳斯特鲁普，:*“你不用的东西，不用付钱。更进一步:你所使用的，你不可能比手工编码更好了。”*

例如，考虑在 Python 中将 1000 以内的所有整数相加:

```
sum(range(1000))
```

每次代码运行时，它都会进行 1000 次迭代和添加——您可以想象这会降低代码的速度。相比之下，在 Rust 中考虑同样的事情:

```
(0..1000).sum()
```

这编译成常数`499500`。实际上，内存使用已经减少了 1，000 倍。

虽然这些抽象也存在于 C 语言中，但是 Rust 大量使用了它们——事实上，一个目标是尽可能多地向语言中添加零成本的抽象。从这个意义上说，Rust 有点像 next-level C。

c 已经存在了 40 多年，Rust 的目标也是如此。Rust 非常强调向后兼容性，以至于今天你仍然可以在 Rust 1.0 中运行代码。同样，如果你今天编写 Rust 代码，二十年后你仍然可以运行它。锈也不会生锈！

[](/rust-powered-command-line-utilities-to-increase-your-productivity-eea03a4cf83a) [## Rust-Powered 命令行实用程序可提高您的工作效率

### 您腰带下的现代快速工具

towardsdatascience.com](/rust-powered-command-line-utilities-to-increase-your-productivity-eea03a4cf83a) 

# 一个小而不可思议的社区

Dropbox 强调安全性和可持续性，所有漂亮的细节都说明了这一点，难怪 Dropbox 用 Rust 重写了许多核心结构。Rust 的第一个大赞助商 Mozilla 在里面写了 Firefox 的重要部分。微软认为 C 和 C++ [对于关键任务软件不再安全](https://thenewstack.io/microsoft-rust-is-the-industrys-best-chance-at-safe-systems-programming/#)，并在 Rust 上投入了越来越多的资金。

不仅仅是大公司，对 Rust 的热爱也转化到了个人程序员身上。尽管到目前为止，StackOverflow 的调查对象中只有 5%的人使用 Rust，但是这些开发者对这种语言非常感兴趣。

这是有原因的。不仅语言规范和编译器都考虑得很周全。有`[rustup](https://rustup.rs)`来安装和管理工具链。Cargo 是一个命令行工具，每个 Rust 安装都附带它，可以帮助管理依赖关系、运行测试和生成文档。

有 [crates.io](https://crates.io) 供用户分享和发现图书馆，还有 [docs.rs](https://docs.rs) 供用户记录。有来自 [Clippy](https://github.com/rust-lang/rust-clippy) 的编译器 lints 和来自 [rustfmt](https://github.com/rust-lang/rustfmt) 的自动格式化。

除此之外，还有官方和非官方的聊天，子编辑，用户论坛，StackOverflow 问题，以及世界各地的会议。对于一个把友好放在一切之上的社区，还有什么可要求的呢？

# 缺点是:在你会走之前需要先跑

Rust 令人沮丧的一点是高昂的启动成本。虽然在大多数语言中，你需要一到两天的时间来提高效率，但在 Rust 中，这更像是一两周的时间。

这是由于许多其他语言没有使用的新概念，以及编译时通常会有许多错误的事实。你需要在第一天就处理所有的异常，而不能像在 Python 中那样，只是写一段临时代码，然后运行并添加异常。

此外，由于 Rust 仍然很新，并不是所有你想要的库都已经存在了。除了官方文档和关于 StackOverflow 的各种问题，也没有那么多教程。

好消息是，一旦你学会了这些概念并编译好了你的程序，它就会像魔法一样运行。另外，考虑到向后兼容性，它应该还能工作 20 年。

考虑到你的代码的可持续性，以及 Rust 得到许多大公司支持的事实，尽管有缺点，一两周的前期学习可能是值得的。

![](img/5745f1dd8d900abb0725ecfb225e92f4.png)

这种铁锈不会使你的船沉没。马特·拉默斯在 [Unsplash](https://unsplash.com/s/photos/rusty?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) 上拍摄的照片

# 底线:无所畏惧地黑客

生锈是[多于安全](https://steveklabnik.com/writing/rust-is-more-than-safety)。但很难否认，它的许多核心概念旨在消除内存泄漏和其他安全问题。在这个软件决定一切的时代，安全是必须的。

每种即将到来的语言可能都有一席之地: [Go](/one-in-two-pythonistas-should-learn-golang-now-ba8dacaf06e8) 正在越来越多地占据 Python 和 Java 的空间， [Julia](/bye-bye-python-hello-julia-9230bff0df62) 正在数据科学领域追赶 Python，Rust 正在 Python 和 C++领域成长。让 Rust 与众不同的是它不可思议的社区，它的创新特性，以及它被设计成可以在未来几十年内工作的事实。

还有很多工作要做，而 Rust 只能完成其中的一小部分。今天的新语言很有可能会存在一段时间，尽管其他语言也会在未来几年出现。但是如果我必须把我的牌放在一种语言上，Rust 将是一个安全的赌注。

*编辑:正如*[*Ketut Artayasa*](https://medium.com/u/6a6a5a2d9af0?source=post_page-----64a06d976ce--------------------------------)*和推特用户*[*don dish*](https://twitter.com/dondishdev)*曾* *指出，比雅尼·斯特劳斯特鲁普是 C++的发明者，而不是 C，这在这个故事的最初版本中是错误的。此外，当 C++的意思是 C#已经被提及。这一点也已得到纠正。*