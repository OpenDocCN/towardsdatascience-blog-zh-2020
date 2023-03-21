# 为机器学习(图像识别)预处理图像

> 原文：<https://towardsdatascience.com/preprocessing-your-images-for-machine-learning-image-recognition-277fb7aad5b2?source=collection_archive---------61----------------------->

![](img/988aa555b814176744b94849896fa8d0.png)

照片由 Jye B 在 Unsplash 上拍摄—【https://unsplash.com/photos/RuTMP0iI_ek 

我在 JKU 学习期间，有一个为机器学习项目预处理图像的任务。在学习算法中使用原始图像之前，有必要对其进行清理，因此我们创建了一个预处理函数。我认为这对其他人也很有用，所以我想分享一点我的方法。该文件是以一种易于理解的方式构建的，并且应该具有类似教程的效果。

# 目录

*   [预处理错误](https://github.com/Createdd/Writing/blob/master/2020/articles/preProcessingImages.md#the-pre-processing-errors)
*   [加工文件](https://github.com/Createdd/Writing/blob/master/2020/articles/preProcessingImages.md#the-processing-file)
*   [Python 代码](https://github.com/Createdd/Writing/blob/master/2020/articles/preProcessingImages.md#python-code)
*   [关于](https://github.com/Createdd/Writing/blob/master/2020/articles/preProcessingImages.md#about)

# 预处理错误

在为机器学习任务处理图像时，经常会遇到一些错误。这些是:

1.  正确的文件扩展名，代表图像文件(如 jpg)
2.  特定的文件大小
3.  该文件可以作为图像读取(取决于用于进一步处理的库)
4.  图像数据具有可用信息(不止一个值)
5.  图像有一定的宽度和高度
6.  没有重复的图像

# 加工文件

该函数有 3 个参数:

1.  包含图像文件的输入目录
2.  输出目录，有效图像将被复制到该目录
3.  包含错误的日志文件

我的解决方案递归搜索允许的图像文件。这是通过`get_files`功能完成的。

`check_files`函数获取文件并返回带有相应错误代码的图像列表。

`validate_file`函数检查图像中的各种常见错误，并返回错误编号。如果没有发生错误，错误代码将为 0。

`separate_files`功能将区分有效图像和无效图像。

`copy_valid_files`功能会将有效文件复制到定义的“输出目录”文件夹中。

函数在指定的日志文件路径中创建一个日志文件，包含所有有错误的文件。

整个`pre_processing`函数最终返回有效文件的数量。

# Python 代码

python 文件可以在我的数据科学收藏库中找到:[https://github.com/Createdd/DataScienceCollection](https://github.com/Createdd/DataScienceCollection)

在这里，您可以通过 Github Gist 检查代码:

# 关于

丹尼尔是一名企业家、软件开发人员和律师。他的知识和兴趣围绕商业法和编程机器学习应用发展。从本质上说，他认为自己是复杂环境的问题解决者，这在他的各种项目中都有所体现。如果你有想法、项目或问题，不要犹豫，立即联系我们。

![](img/20f3d929b1125504c0086c77be8e2920.png)

连接到:

*   [领英](https://www.linkedin.com/in/createdd)
*   [Github](https://github.com/Createdd)
*   [中等](https://medium.com/@createdd)
*   [推特](https://twitter.com/_createdd)
*   [Instagram](https://www.instagram.com/create.dd/)