# Python 正在慢慢失去它的魅力

> 原文：<https://towardsdatascience.com/python-is-slowly-losing-its-charm-9ca652726492?source=collection_archive---------0----------------------->

## 意见

## 编程语言的瑞士军刀有它的问题，可能会被其他更适合这项任务的语言所取代

![](img/4fff9afa4e45d171fbad976823bcdd49.png)

塔玛拉·戈尔在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上的照片

自从 Python 在 20 世纪 90 年代早期发布以来，它已经产生了很多宣传。当然，编程社区至少花了 20 年才意识到它的存在，但从那以后，它的受欢迎程度已经远远超过了 C、C#、Java 甚至 Javascript。

虽然 Python 主导了数据科学和机器学习领域，并且在某种程度上主导了科学和数学计算领域，但与 Julia、Swift 和 Java 等新语言相比，它也有自己的缺点。

# 是什么让 Python 如此受欢迎？

Python 的飞速发展背后的一个主要驱动点是它的易学性和易用性，这使得它对初学者非常有吸引力，甚至对那些因为像 C/C++这样的语言的艰深、陌生的语法而回避编程的人也是如此。

这种语言的核心是广泛强调*代码的可读性。凭借其简洁而富于表现力的语法，它允许开发人员表达想法和概念，而无需编写大量代码(如 C 或 Java 等低级语言)。鉴于其简单性，Python 可以与其他编程语言无缝集成(比如将 CPU 密集型任务卸载到 C/C++ ),使其成为多语言开发人员的额外收获。*

Python 通用性的另一个原因是它被企业(包括 FAANG)和无数小型企业大量使用。今天，你会发现你能想到的几乎任何东西的 Python 包——对于科学计算，你有用于机器学习的 [Numpy](https://pypi.org/project/numpy) 、 [Sklearn](https://pypi.org/project/scikit-learn/) 和用于计算机视觉的 [Caer](https://github.com/jasmcaus/caer) (我的计算机视觉包)。

[](https://github.com/jasmcaus/caer) [## 贾斯考斯/卡尔

### 用于高性能人工智能研究的轻量级计算机视觉库。Caer 包含强大的图像和视频处理操作…

github.com](https://github.com/jasmcaus/caer) 

# Python 有弱点

# 很慢，非常慢

![](img/678fad5f98594da290957e587b213b05.png)

尼克·艾布拉姆斯在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄的照片

这可能是显而易见的。速度通常被认为是开发人员的主要关注点之一，并且可能会持续一段不可预见的时间。

Python“慢”的主要原因有两个——Python 被*解释为*而不是编译，最终导致执行时间变慢；以及它是*动态类型化*的事实(变量的数据类型由 Python 在执行过程中自动推断)。

事实上，这种认为“Python 很慢”的观点在初学者中有很大的影响。是的，是真的。但只是部分地。

以 TensorFlow 为例，这是一个用 Python 编写的机器学习库。这些库实际上是用 C++编写的，并在 Python 中可用，在某种程度上形成了围绕 C++实现的 Python“包装器”。Numpy 也是如此，在某种程度上，甚至 Caer 也是如此。

# 它有一个 GIL

Python 缓慢的主要原因之一是 GIL(全局解释器锁)的存在，它一次只允许一个线程执行。虽然这提高了单线程的性能，但它限制了并行性，开发人员不得不实施多处理程序而不是多线程程序来提高速度。

# 不是内存密集型任务的最佳选择

当对象超出范围时，Python 会自动进行垃圾收集。它旨在消除 C 和 C++在内存管理方面的复杂性。由于指定数据类型的灵活性(或缺乏灵活性), Python 消耗的内存量可能会迅速爆炸。

此外，Python 可能没有注意到的一些 bug 可能会在运行时突然出现，最终会在很大程度上减慢开发过程。

# 在移动计算领域表现不佳

![](img/793eccbeb30f746c2bcbe55e9121f961.png)

照片由 [Yura Fresh](https://unsplash.com/@mr_fresh?utm_source=medium&utm_medium=referral) 在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄

随着从桌面到智能手机的大规模转移，显然需要更健壮的语言来为移动设备构建软件。虽然 Python 在桌面和服务器平台上有相当大的代表性，但由于缺乏强大的移动计算处理，它往往会在移动开发中失利。

近几年来，这个领域有了很大的进步，但是这些新增加的库甚至不能与他们的强劲竞争对手如 Kotlin、Swift 和 Java 相提并论。

# 其他语言的兴起

最近，像 Julia、Rust 和 Swift 这样的新语言已经出现在雷达上，从 Python、C/C++和 Java 中借用了许多好的设计概念— [**Rust**](https://www.rust-lang.org/) 非常好地保证了运行时的内存安全和并发性，并提供了与 WebAssembly 的一流互操作性； [**Swift**](https://developer.apple.com/swift/) 由于支持 LLVM 编译器工具链，几乎和 C 一样快，而[**Julia**](https://julialang.org/)**为 I/O 密集型任务提供异步 I/O，速度快得惊人。**

# **结论**

**Python 从来不是为了成为最好的编程语言而构建的。它从来就不是为了应对 C/C++和 Java 而构建的。它是一种通用编程语言，强调人类可读的、以英语为中心的语法，允许快速开发程序和应用程序。**

**Python 和其他语言一样，归根结底是一种工具。有时候，它是最好的工具。有时候不是。最常见的是“还行”。**

**那么，Python 作为一种编程语言，是不是正在走向灭亡？**

**我不这么认为。**

**它正在失去魅力吗？**

**啊，也许有一点。就一点点。**