# 混合 Pytest Fixture 和 unittest。硒测试的测试用例

> 原文：<https://towardsdatascience.com/mixing-pytest-fixture-and-unittest-testcase-for-selenium-test-9162218e8c8e?source=collection_archive---------33----------------------->

## 登录到一个网站，通过密码和测试无处不在

![](img/21749f04906fb43e5bdb90c6ebff0569.png)

Joshua Sortino 在 [Unsplash](https://unsplash.com/s/photos/automation?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) 上拍摄的照片

在我早期的文章[中，我介绍了使用 pytest fixture 来传递 Selenium 测试函数的参数。但是有一个问题。我们必须在每个测试函数中编写登录函数。但是我们想要的是一次登录，到处测试。](/pass-arguments-to-selenium-test-functions-in-pytest-b6c76a320ecd)

# 问题是

如果要测试`play.py`中的几个函数，就要在每个测试函数中写登录函数。代码将变得非常多余，如下例所示。

```
def test_foo(params):
    # login
    driver = webdriver.Chrome()
    ...

    # test foo function
    ...def test_bar(params):
    # login
    driver = webdriver.Chrome()
    ...

    # test bar function
    ...
```

我们想登录一次，到处测试，就像下面的例子。

```
class TestFtseLinks(TestCase):
  def setUp(self):
　　self.driver = webdriver.Chrome()

　def test_foo(self):
　　self.driver.find('xxxxx')

　def test_bar(self):
　　self.driver.find('xxx')
```

在我以前的帖子中，我尝试混合使用 unittest。TestCase 和 pytest fixture，但失败。

不过，这次我来介绍一下解决方案。

# 解决方法:在`conftest.py`中写一个驱动类

目录树:

```
test
├── conftest.py
└── test_play.py
```

在`conftest.py`中，我们必须将每个选项编写为 pytest.fixture 函数。我们编写一个`dirver_class`函数来登录我们的目标网站。

在`test_play.py`中，我们可以直接用`self.login.driver`驱动。

测试输出:

**查看我的其他帖子** [**中等**](https://medium.com/@bramblexu) **同** [**一分类查看**](https://bramblexu.com/posts/eb7bd472/) **！
GitHub:** [**荆棘徐**](https://github.com/BrambleXu) **领英:** [**徐亮**](https://www.linkedin.com/in/xu-liang-99356891/) **博客:**[](https://bramblexu.com)

# **参考**

*   **使用标记 [](https://docs.pytest.org/en/latest/unittest.html#mixing-pytest-fixtures-into-unittest-testcase-subclasses-using-marks) 将 pytest 夹具混合到`unittest.TestCase`子类中**
*   **[https://stack overflow . com/questions/54030132/how-to-use-pytest-to-pass-a-value-in-your-test-using-command-line-args](https://stackoverflow.com/questions/54030132/how-to-use-pytest-to-pass-a-value-in-your-test-using-command-line-args)**
*   **[https://stack overflow . com/questions/50132703/pytest-fixture-for-a-class-through-self-not-as-method-argument/50135020 # 50135020](https://stackoverflow.com/questions/50132703/pytest-fixture-for-a-class-through-self-not-as-method-argument/50135020#50135020)**
*   **[https://docs . pytest . org/en/latest/unittest . html # using-autouse-fixtures-and-access-other-fixtures](https://docs.pytest.org/en/latest/unittest.html#using-autouse-fixtures-and-accessing-other-fixtures)**