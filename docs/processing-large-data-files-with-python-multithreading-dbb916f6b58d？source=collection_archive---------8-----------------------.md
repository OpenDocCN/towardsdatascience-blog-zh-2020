# 用 Python 多线程处理大型数据文件

> 原文：<https://towardsdatascience.com/processing-large-data-files-with-python-multithreading-dbb916f6b58d?source=collection_archive---------8----------------------->

## 在左侧车道加速行驶。

![](img/d243aa1623ea16a77e1591f6694ab2dc.png)

2020 年 zapalote.com

我们花费大量时间等待一些数据准备任务完成——你可能会说，这是数据科学家的命运。我们可以加快速度。这里有两种技术会派上用场:**内存映射**文件和**多线程**。

# **数据**

最近，我不得不从 Google Books Ngram corpus[中提取术语和术语频率](http://commondatastorage.googleapis.com/books/syntactic-ngrams/index.html)，我发现自己在想是否有办法加快这项任务。语料库由 26 个文件组成，总数据量为 24GB。我感兴趣的每个文件都包含一个术语和其他元数据，用制表符分隔。将这些文件作为熊猫数据帧读取的暴力方法非常慢。因为我们只需要唯一的术语和它们的匹配计数，所以我想我会尽量让它更快:-)

# **内存映射文件**

这种技术并不新鲜。由来已久，起源于 Unix(Linux 之前！).简而言之，`mmap`通过将文件内容加载到内存页面中来绕过通常的 I/O 缓冲。这对于内存占用量大的计算机非常适用。对于今天的台式机和笔记本电脑来说，这基本上没问题，因为 32GB 的内存不再是一个深奥的问题。Python 库模仿了大多数 Unix 功能，并提供了一个方便的`readline()`函数来一次提取一行字节。

```
# map the entire file into memory
mm = mmap.mmap(fp.fileno(), 0)# iterate over the block, until next newline
for line in iter(mm.readline, b""):
    # convert the bytes to a utf-8 string and split the fields
    term = line.decode("utf-8").split("\t")
```

`fp` 是一个文件指针，之前用`r+b`访问属性打开过。这就对了，通过这个简单的调整，你已经使文件读取速度提高了一倍(好吧，确切的改进将取决于许多因素，如磁盘硬件等)。

# 多线程操作

下一个总是有助于提高速度的技术是增加并行性。在我们的例子中，任务是 I/O 绑定的。这非常适合于*扩展—* 即添加线程。你会发现关于什么时候在搜索引擎上横向扩展(多处理)更好的讨论。

Python3 有一个很棒的标准库，用于管理线程池并动态地给它们分配任务。所有这一切都通过一个极其简单的 API 实现。

```
# use as many threads as possible, default: os.cpu_count()+4
with ThreadPoolExecutor() as threads:
   t_res = threads.map(process_file, files)
```

`ThreadPoolExecutor`的`max_workers`默认值是每个 CPU 内核 5 个线程(从 Python v3.8 开始)。`map()` API 将接收一个应用于列表中每个成员的函数，并在线程可用时自动运行该函数。哇哦。就这么简单。在不到 50 分钟的时间里，我已经将 24GB 的输入转换成了一个方便的 75MB 的数据集，可以用 pandas 来分析——瞧。

完整的代码在 [GitHub](https://gist.github.com/zapalote/30aa2d7b432a08e6a7d95e536e672494) 上。随时欢迎评论和意见。

PS:我给每个线程加了一个带`tqdm`的进度条。我真的不知道他们是如何设法避免屏幕上的线条混乱的…它非常有效。

更新:两年后，[这个](https://hackernoon.com/crunching-large-datasets-made-fast-and-easy-the-polars-library)上来了:-)