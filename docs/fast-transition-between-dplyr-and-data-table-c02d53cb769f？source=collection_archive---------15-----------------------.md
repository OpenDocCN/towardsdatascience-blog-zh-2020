# dplyr 和 data.table 之间的快速转换

> 原文：<https://towardsdatascience.com/fast-transition-between-dplyr-and-data-table-c02d53cb769f?source=collection_archive---------15----------------------->

## dplyr 和 data.table 的并行语法比较

![](img/ec85b4183de2ddf62db40daa2c82e5e4.png)

照片由[马体·米罗什尼琴科](https://www.pexels.com/@tima-miroshnichenko?utm_content=attributionCopyText&utm_medium=referral&utm_source=pexels)从[派克斯](https://www.pexels.com/photo/man-people-woman-laptop-5698381/?utm_content=attributionCopyText&utm_medium=referral&utm_source=pexels)拍摄

在这篇文章中，我比较了 R 的两个最强大的数据操作库的语法:dplyr 和 data.table。我在这里不是要提倡任何包。然而，在处理一个具有异常大的数据集的项目时，为了速度和内存效率，我首选的包变成了 data.table。我在 dplyr 中使用了一个遗留项目，并在 data.table 中重新创建了它。

> `***data table***`提供高性能版本的 base R 数据帧，增强了语法和功能，以提高易用性、便利性和编程速度根据数据表作者马特·道尔的说法
> 
> `***dplyr***` 是一种数据操作语法，提供一组一致的动词，帮助您解决最常见的数据操作挑战根据《dplyr》的作者 Hadley Wickham 所说

# **库和数据集**

我将使用 R 内置的汽车保险索赔的 AutoClaims 数据集。让我们安装和加载库，并为练习准备数据集。

```
#install.packages(“dplyr”)
#install.packages(“data.table”)
#install.packages(“insuranceData”)library(dplyr)
library(data.table)
library(insuranceData)data(“AutoClaims”) *#load the dataset*
data <- AutoClaims *#rename the dataset*
data <- data.table(data) *#convert it to data.table*
```

数据有 6773 行和 5 列。让我们看看数据是怎样的。

```
> head(data)
STATE CLASS GENDER AGE PAID
1 STATE 14 C6 M 97 1134.44
2 STATE 15 C6 M 96 3761.24
3 STATE 15 C11 M 95 7842.31
4 STATE 15 F6 F 95 2384.67
5 STATE 15 F6 M 95 650.00
6 STATE 15 F6 M 95 391.12
```

# **十大最常用的数据操作功能**

1.  **按行过滤**

让我们过滤 79 岁或更年轻的男性

```
data[AGE <= 79 & GENDER == “M”]*#datatable*data %>% filter(AGE <= 79 & GENDER == “M”) *#dplyr*
```

**2。按列选择**

让我们选择性别、年龄和薪酬列

```
data[, .(GENDER, AGE, PAID)] *#datatable*data %>% select(GENDER, AGE, PAID) *#dplyr*
```

**3。添加新列**

让我们用 0.85 美元/欧元的兑换率将美元兑换成欧元。产生的新变量被添加到数据集中

```
data[, PAID.IN.EURO := PAID * 0.85] *#datatable*data %>% mutate(PAID.IN.EURO = PAID * 0.85) *#dplyr*
```

**4。删除栏目**

让我们删除新创建的变量

```
data[, !c(“PAID.IN.EURO”)] *#datatable* data[, PAID.IN.EURO:= NULL] *#datatable (alternative way)*select(data, -PAID.IN.EURO) *#dplyr*
```

**5。创建新列**

现在创建一个新变量并删除现有的变量。因此，数据集将只有一个新变量

```
data[, .(PAID.IN.EURO = PAID * 0.85)] #datatabledata %>% transmute(PAID.IN.EURO = PAID * 0.85) #dplyr
```

**6。按列汇总**

平均赔付的保险理赔是多少？

以下代码聚合数据并返回名为 AVG 的单记录。有报酬的

```
data[, .(AVG.PAID = mean(PAID))] *#datatable*data %>% summarise(AVG.PAID = mean(PAID)) *#dplyr*
```

**7。重命名变量**

对于 data.table，我们使用 setnames 函数，它先引用旧名称，然后引用新名称。对于 dplyr，顺序正好相反

```
setnames(data, c(“GENDER”, “CLASS”), c(“gender”, “class”)) *#dt*data %>% rename(“gender” = “GENDER”, “class” = “CLASS”) *#dplyr*
```

**8。按升序或降序排列数据**

让我们使用 data.table 中的`setorder`函数和 dplyr 中的`arrange`函数对数据进行升序和降序排序。

```
setorder(data, PAID) *#datatable ascending order*
setorder(data, -PAID) *#datatable descending order*data %>% arrange(PAID) *#dplyr ascending* 
data %>% arrange(desc(PAID)) *#dplyr descending*
```

**9。按列分组**

单独对数据分组没有任何作用；您应该将它与另一个函数结合使用，例如，按州分组并汇总数据，或者筛选数据。

```
data[, .(mean.paid.by.state = mean(PAID)), by = "STATE"]data %>% group_by(STATE) %>%
  summarize(mean.paid.by.state = mean(PAID))
```

**10。计数观察值**

让我们通过对 dplyr 和使用 n()函数来计算每个类有多少个观察值。n 代表数据表

```
data[, .N, by= “class”] *#datatable*data %>% *#dplyr*
 group_by(class) %>%
 summarise(n())
```

现在让我们来看更多关于如何在 dplyr 和 data.table 中链接函数的例子。

# **链接功能**

在 dplyr 中，我们使用%>%管道操作符来链接函数。在 data.table 中，可以在一行代码中简洁地编写几个函数，或者使用方括号进行更复杂的链接。

*   让我们选择三个变量:STATE、AGE 和 payed，然后创建一个 20% payed 的新列，并根据列 AGE 对结果进行升序排序。

```
***#datatable***
data[, .(STATE, AGE, PAID)][, .paid.20 := PAID * 0.2, keyby = "AGE"]***#dplyr***
data %>% select(STATE, AGE, PAID) %>% 
  mutate(paid.20 = PAID * 0.2) %>%
  arrange(AGE)
```

# **结论**

今天我们并排比较了`dplyr`和`data.table`的语法，使用了 10 个常用函数和链式函数。使用这两个库，您可以做更多的事情。

**语法。** Data.table 使用比 dplyr 更短的语法，但通常更加细致和复杂。dplyr 使用管道操作符，对于初学者阅读和调试来说更直观。此外，许多其他库使用管道操作符，如 ggplot2 和 tidyr。虽然 data.table 和 dplyr 都在 R 社区中广泛使用，但 dplyr 的使用范围更广，提供了更多的协作机会。

**内存和速度**重要吗？在构建复杂的报告和仪表板时，尤其是在处理非常大的数据集时，data.table 具有明显的优势。

使用哪个库进行分析由您决定！

你可以在参考资料部分找到更多的资料。

感谢您的阅读！

# **参考文献**

[](https://rdatatable.gitlab.io/data.table/index.html) [## “数据帧”的扩展

### data.table 提供了 base R 的 data.frame 的高性能版本，并增强了语法和功能，以便于…

rdatatable.gitlab.io](https://rdatatable.gitlab.io/data.table/index.html) [](https://dplyr.tidyverse.org/) [## 数据操作的语法

### 一个快速、一致的工具，用于在内存中和内存外处理类似数据帧的对象。

dplyr.tidyverse.org](https://dplyr.tidyverse.org/)