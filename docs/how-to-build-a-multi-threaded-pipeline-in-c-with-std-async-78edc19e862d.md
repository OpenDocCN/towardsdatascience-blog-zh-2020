# 如何用 std::async 在 C++中构建多线程管道

> 原文：<https://towardsdatascience.com/how-to-build-a-multi-threaded-pipeline-in-c-with-std-async-78edc19e862d?source=collection_archive---------15----------------------->

![](img/6e0c674cb20cd2076fabb4e5db04e67d.png)

由[凯文·Ku](https://unsplash.com/@ikukevk?utm_source=medium&utm_medium=referral)在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄

今天我们要弄清楚如何在 C++11 中创建一个能够处理流数据的多线程应用程序。更重要的是，我们不会自己创建任何 std::thread-s，相反，我们将采用未来和异步调用的新功能范式。

本教程的代码可以在 Github 上找到:

[](https://github.com/Obs01ete/async_pipeline) [## OBS 01 远程/异步管道

### 此时您不能执行该操作。您已使用另一个标签页或窗口登录。您已在另一个选项卡中注销，或者…

github.com](https://github.com/Obs01ete/async_pipeline) 

这项工作的动机是，当您想要在 CPU 上执行一些繁重的计算时，您不可避免地想要在多线程中运行它。否则，数据研磨的带宽将被限制在一个线程内。例如，如果你有一个来自网络摄像头的图像流，你想对它应用几个过滤器:颜色调整，调整大小，甚至可能是一些人工智能，比如人脸检测，运行所有这些处理步骤可能需要数百毫秒。当源帧速率为 30 fps 时，您可能会在可视化窗口中得到令人失望的 5–10 fps。我们通过 <future>header 和 stdlib 的内容来看看如何处理。</future>

然后让我们创建我们的处理函数`func1`和`func2`。在本教程中，我们不会运行真正的计算，因此作为概念验证，让我们设置模拟线程繁忙的睡眠。

最后，我们希望可视化处理的结果，比如调用 OpenCV 的 cv::showImage()。为了简单起见，我们在这里放置了一个简单的 std::cout 打印输出。

现在我们准备好准备我们的主要应用程序。

是时候推出我们的应用程序，看看我们有什么。

```
Enqueued sample: 0
Enqueued sample: 1
Sample 0 output: ‘input_string_0 func1 func2’ finished at 1851
Sample 1 output: ‘input_string_1 func1 func2’ finished at 2851
Enqueued sample: 2
Sample 2 output: ‘input_string_2 func1 func2’ finished at 3851
Enqueued sample: 3
Sample 3 output: ‘input_string_3 func1 func2’ finished at 4851
Enqueued sample: 4
Enqueued sample: 5
Sample 4 output: ‘input_string_4 func1 func2’ finished at 5851
Sample 5 output: ‘input_string_5 func1 func2’ finished at 6851
Enqueued sample: 6
...
Enqueued sample: 98
Sample 98 output: 'input_string_98 func1 func2' finished at 99862
Enqueued sample: 99
Waiting to finish...
Sample 99 output: 'input_string_99 func1 func2' finished at 100862
Finished!
```

太神奇了，管道成功了！请注意样本可视化时刻之间的时间差:为 1000 毫秒。如果没有多线程管道，则为 900+950=1850 毫秒。

std::async 的一个优点是它在幕后管理一个线程池。所以不用担心每次我们调用 std::async 时都会启动一个新线程。后续调用会重用线程，因此操作系统不会因为启动和终止大量线程而过载。

人们可能会注意到代码中的一些缺陷:

*   std::cout 不受互斥体保护。
*   异常不由处理函数处理。
*   我们正在向`func#`，尤其是`visualize`传递大量的争论。值得考虑将代码组织成一个类。

这些我将在下一篇文章中讨论。

谢谢你看我的教程，下期再见！