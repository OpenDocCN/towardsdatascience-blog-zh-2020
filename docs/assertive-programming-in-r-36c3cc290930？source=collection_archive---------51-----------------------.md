# R 语言中的断言编程

> 原文：<https://towardsdatascience.com/assertive-programming-in-r-36c3cc290930?source=collection_archive---------51----------------------->

## 您的代码应该按预期工作，否则会立即失败

您可能熟悉使用 *testthat* 包的单元测试。单元测试的目标是检查你的功能是否被正确开发。断言检查你的函数是否被正确使用。

> 单元测试是为**开发人员**准备的，并根据命令执行。
> 断言是针对**用户**的，并且在每次函数调用时执行。

![](img/a291efef7ab83faff4a81028f28dbf9e.png)

照片由 chuttersnap 在 Unsplash 上拍摄

# 快速失败

运行时测试，通常称为断言，是一小段代码，用于检查条件，如果条件不满足，测试就会失败。断言使你的代码能够快速失败——这是一件好事。当某个东西出错时，我们不希望我们的程序继续运行，直到错误复合并暴露出来。相反，我们希望程序在错误的源头立即失败，并给出清晰准确的错误消息。

本文的目标是让你的 R 函数快速失效。假设你的函数没有错误或者副作用，错误进入你的函数的唯一途径就是通过它的输入。在本文中，您将学习如何使用断言来识别不良输入并警告用户(包括未来的您)。

输入有两种不好的方式:

1.  **功能输入包含错误**。R 发现了一些错误:从不可能或丢失的值到十进制数中逗号和点的不一致使用。其他错误更加隐蔽。想想无效的信用卡号或不存在的邮政编码和糟糕的电子邮件格式。
2.  你在开发你的功能时考虑了一个特殊的用例。你无法想象用户试图使用你的代码的所有方式。他或她可能**以技术上正确的输入使用你的功能，但不是以预期的方式。你试过用`sort.list(list(2, 1, 3))`排序列表吗？**

# 为什么 R 包“自信”

r 有几个用于编写断言的包。这篇文章提倡 R 包*自信*有三个原因:

1.  该软件包为许多不同的情况提供了大量的功能
2.  它的功能非常容易阅读

`assert_all_are_positive(c(1, -2, 3))`

3.并提供信息丰富的错误消息

```
Error: is_positive : c(1, -2, 3) contains non-positive values.
There was 1 failure:
 Position Value Cause
1 2 -2 too low
```

直接从[起重机](https://cran.r-project.org/)安装软件包，并将其加载到您的 R 会话中:

`install.packages("assertive")`

`library(assertive)`

# 写你的第一个断言

假设您编写了一个对数值向量的元素求和的函数:

```
add_numbers <- function(numbers) {
  result <- sum(numbers)
  return(result)
}
```

向量的值可以是任何数字。让我们在函数的开头写一个断言来检查我们的输入向量:

```
add_numbers <- function(numbers) {
 **assert_is_numeric(numbers)**
  result <- sum(numbers)
  return(result)
}add_numbers(c(1, 2, 2, 3)) # pass
```

注意断言是写在函数内部的*。这意味着它总是在那里，等待每次调用函数时被执行。检查一个断言只需要几毫秒的时间，这对于除了对性能要求最高的应用程序之外的所有应用程序来说都没问题。*

# 检查多个条件

假设您修改了函数，期望得到一个唯一的*值的向量。现在你需要两个断言:*

1.  一个用于检查向量是否为数字
2.  另一个检查向量没有重复值

您可以通过用来自*magritter*包的[转发管道](https://magrittr.tidyverse.org/) `%>%`链接两个或更多断言来保持代码的可读性:

```
library(magrittr)add_unique_numbers <- function(input_vector) {
 **input_vector %>% 
    assert_is_numeric %>%
    assert_has_no_duplicates** result <- sum(input_vector)
  return(result)
}add_unique_numbers(c(1.5, 2, 2, 3)) # fail
```

有很多*断言*函数，适用于各种情况。全部背下来不太实际。只需在 CRAN 上搜索[包文档](https://cran.r-project.org/web/packages/assertive/assertive.pdf)就能找到适合你特定需求的功能。

让我们用一个练习来试试这个。下面的函数需要百分比值的向量，例如`percentages <- c(64, 55, 97, 85)`，并计算平均百分比。你能在 *assertive* 包中找到合适的断言吗？

```
calculate_mean_percentage <- function(percentages) {
 **# ... your assertion goes here**    result <- mean(percentages)
    return(result)
}
```

*提示:在你的 R 控制台中编写* `ls("package:assertive", pattern = "percent")` *来搜索 assertive 中名称与“百分比”匹配的函数。*

# 断言和谓词

您可能会在这里看到一种模式。所有断言都由三部分组成:

1.  他们总是以`assert_`开头
2.  后跟谓语`is_`或`has_`
3.  并以期望结束，例如`numeric`

当检查对象的*单个元素*而不是对象本身时，谓词`is_`在中变为`all_are_`或`any_are_`。例如，下面的断言检查传递给函数的所有数字是否都是整数:

```
add_whole_numbers <- function(whole_numbers) {
 **assert_all_are_whole_numbers(whole_numbers)**
  result <- sum(whole_numbers)
  return(result)
}add_unique_numbers(c(1.5, 2, 2, 3)) # fail
```

# 除了核对数字

到目前为止，我们只处理了数字。但是*断言*包包含了各种情况下的断言。为了说明这一点，我们将用一些检查文本、日期甚至主机操作系统的例子来结束我们的讨论。

*   你的函数需要一个人名向量吗？
    检查缺失或空字符串:

`assert_all_are_non_missing_nor_empty_character()`

*   有一个只在 Windows 上工作的功能？然后确保用户正在运行 Windows:

`assert_is_windows()`

*   你的函数期望一个人的出生日期吗？
    确定事情已经过去:

`assert_is_in_past()`

# **结论**

在本文中，您学习了如何使用 *assertive* 包编写断言。多亏了断言，你的函数将做它们应该做的事情，或者快速失败，并给出一个清晰的错误消息。

## *参考文献*

棉花理查德。(2017).*测试 R 代码(第一版)。*查普曼&霍尔。

亨特安德鲁。托马斯戴夫。(1999).*《实用程序员》(第 25 版，2010 年 2 月)*。爱迪生韦斯利朗曼公司。