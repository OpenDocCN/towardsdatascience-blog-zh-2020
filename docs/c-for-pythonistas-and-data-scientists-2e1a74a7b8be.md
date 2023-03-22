# 面向 Pythonistas 和数据科学家的 C++

> 原文：<https://towardsdatascience.com/c-for-pythonistas-and-data-scientists-2e1a74a7b8be?source=collection_archive---------38----------------------->

## 《依附入门》有助于学习 C++

![](img/ef0174dadc9ee7d37f4e4d49399cdc1f.png)

图片取自[https://negativespace.co/](https://negativespace.co/)。

当我在大学学习数学的时候，我在一个统计模块中接触到了 Python 和 R。从那以后，我只坚持使用这两种语言，只在需要的时候涉足其他语言。

最近，我想提高我的编程基础，并了解更多关于我们数据科学家对 Python 和 R 习以为常的底层概念，同时也想找到一些方法来改进我的工作流。所以我接受了学习 C++的挑战，这样做让我发现了[依附](https://root.cern.ch/cling)。

# 坚持

Cling 是一个交互式的 C++解释器，有助于提供类似于用 Python 编码的体验。这给我的 C++学习经历带来了巨大的好处，我相信它也能帮助许多其他人。

# 你好，世界！

Hello，World 的基本 C++程序！对于 Python 用户来说可能是令人畏惧和不快的。下面是一个例子:

```
#include <iostream>int main() {
 std::cout << “Hello, World!” << std::endl;
 return 0;
}
```

一旦编写完成，程序就可以被编译(如果没有错误)，然后运行以查看输出。

下面是相同的，但是当使用附着解释器时:

```
#include <iostream>
std::cout << “Hello, World”! << std::endl;
```

不需要编译！

这是一个非常简单的例子，但是使用 Cling 可以让你以一种互动的方式探索 C++函数、向量等等，让你做更多的实验。

例如，为了试验向量，你可以使用解释器来帮助学习函数和语法。

```
#include <vector>
vector<int> data;data.push_back(1);
data.push_back(2) 
// return the vector to see what happened
data;
```

# Jupyter 笔记本

还有一个 Jupyter 内核，它可以使学习更加互动，同时还可以帮助您学习如何在数据科学流程中展示您的工作。

# 装置

安装非常简单，使用 [Conda](https://anaconda.org/anaconda/conda) 即可完成:

```
conda install -c conda-forge cling
```

要获得 Jupyter 内核，还需要安装“xeus-cling ”:

```
conda install xeus-cling -c conda-forge
```

## 设置 Jupyter 笔记本

正常运行`Jupyter Notebook`,然后选择一个 C++内核，如下图所示:

# 结束语

解释 C++并不是最终的解决方案，像这样使用 C++意味着你失去了这种语言的很多能力。但是，作为一种学习工具，依附是非常宝贵的，如果你已经有了一些其他语言的基础，它可以帮助你加快学习进程。

相关的代码和笔记本可以在我的 [GitHub](https://github.com/henriwoodcock/blog-post-codes) 上获得，这也包括代码启动代码来导入一个简单的单列 CSV。