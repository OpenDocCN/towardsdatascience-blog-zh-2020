# 将参数传递给 Pytest 中的 Selenium 测试函数

> 原文：<https://towardsdatascience.com/pass-arguments-to-selenium-test-functions-in-pytest-b6c76a320ecd?source=collection_archive---------25----------------------->

## 从命令行传递用户名和密码进行 Selenium 测试

![](img/33b6f84fa24f244e47cc2105bb713cc0.png)

照片由 [NeONBRAND](https://unsplash.com/@neonbrand?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) 在 [Unsplash](https://unsplash.com/s/photos/automation?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) 上拍摄

测试硒的功能并不容易。尤其是当我们必须传递参数时，比如用户名和密码。我解决这些问题有些困难。所以我写了这篇文章，希望能节省你的时间。

# 使用 pytest fixture 传递参数

下面是一个简单的例子来说明如何传递参数。首先，我们需要编写一个`conftest.py`文件来允许 pytest 在执行命令行时接受参数。延伸阅读:[conftest . py 文件有什么用？](https://stackoverflow.com/questions/34466027/in-pytest-what-is-the-use-of-conftest-py-files)

测试结果:

好的，输出看起来没问题。我们重写了`play.py`并在下面展示了两个硒的例子。

# 一个简单的硒的例子

我们添加硒:

测试结果:

# 一个复杂的硒的例子

我们在向 TestCase 类传递参数时会出错(原因在**失败尝试**部分)，所以我使用一个`test_ftse_links()`函数来测试`ftse_links()`中的所有函数。

测试结果:

我们在`conftest.py`中使用了`pytest.skip()`。如果我们没有传递参数，相关的测试将被跳过。

# 失败的尝试

以下是一些失败的尝试。如果你知道为什么和如何解决它们，请留下评论，我会更新帖子。

## 无法将 pytest fixture 传递给 TestCase 类

下面是一些错误案例，我试图将参数传递给 TestCase 类，但失败了。

错误案例 1:

测试结果:

错误情况 2:

测试结果:

# `sys.argv`不支持 pytest

使用 Python 运行这个文件没有错误。

但是使用 Pytest 会导致一个问题。

测试结果:

**查看我的其他帖子上** [**中**](https://medium.com/@bramblexu) **与** [**一个分类查看**](https://bramblexu.com/posts/eb7bd472/) **！
GitHub:** [**荆棘徐**](https://github.com/BrambleXu) **领英:** [**徐亮**](https://www.linkedin.com/in/xu-liang-99356891/) **博客:**[](https://bramblexu.com)

# **参考**

*   **pytest 注释行:[https://docs . py test . org/en/latest/example/simple . html # pass-different-values-to-a-test-function-depending-on-command-line-options](https://docs.pytest.org/en/latest/example/simple.html#pass-different-values-to-a-test-function-depending-on-command-line-options)**
*   **py test skip:[https://docs . py test . org/en/latest/example/simple . html # control-skip-of-tests-by-command-line-option](https://docs.pytest.org/en/latest/example/simple.html#control-skipping-of-tests-according-to-command-line-option)**
*   **pytest 命令行:[https://stack overflow . com/questions/40880259/how-to-pass-arguments-in-pytest-by-command-line/42145604](https://stackoverflow.com/questions/40880259/how-to-pass-arguments-in-pytest-by-command-line/42145604)**
*   **conf test . py:[https://stack overflow . com/questions/34466027/in-py test-what-the-use-of-conf test-py-files](https://stackoverflow.com/questions/34466027/in-pytest-what-is-the-use-of-conftest-py-files)**