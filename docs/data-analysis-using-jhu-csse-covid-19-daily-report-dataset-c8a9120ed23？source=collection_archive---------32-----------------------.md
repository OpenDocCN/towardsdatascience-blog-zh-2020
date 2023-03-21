# 使用 JHU CSSE 新冠肺炎日报数据集进行数据分析

> 原文：<https://towardsdatascience.com/data-analysis-using-jhu-csse-covid-19-daily-report-dataset-c8a9120ed23?source=collection_archive---------32----------------------->

![](img/7aaca3ac520d3f7e69c84d423adbe70f.png)

数据分析使用 JHU CSSE 新冠肺炎数据。照片由 [Pexels](https://www.pexels.com/photo/people-holding-a-poster-asking-about-facts-on-coronavirus-3952215/?utm_content=attributionCopyText&utm_medium=referral&utm_source=pexels) 的 [cottonbro](https://www.pexels.com/@cottonbro?utm_content=attributionCopyText&utm_medium=referral&utm_source=pexels) 拍摄

## 数据科学实践指南

## 数据科学方法论中的疫情报表数据集查询

*免责声明:本文中使用的数据集由约翰霍普金斯大学代表其工程系统科学中心在知识共享署名 4.0 国际版(CC BY 4.0)下授权。版权所有约翰·霍普金斯大学 2020 年*

从头开始构建数据框可能有点乏味。因此，您通常要做的不是从一些数据序列创建数据框，而是导入数据。在本文中，您将通过从**约翰霍普金斯大学 Github** 页面导入新冠肺炎每日报告案例数据框来分析带有**熊猫**的数据集。

![](img/6e80f6ed269bdfe81ea3cfb333aa724b.png)

图 1:从 JHU CSSE 新冠肺炎数据导入 CSV 文件。图片作者[作者](https://medium.com/@wiekiang)

如图 1 所示，我们调用`import pandas as pd`来导入`pandas`库。接下来，您可以用`read_csv`导入 CSV 文件，并将其赋给一个变量。在这种情况下，我们使用`covid_daily_report`作为变量。

令人印象深刻的是，因为它现在在熊猫数据框中，我们可以利用熊猫提供的所有功能，并操纵查看和更改这些数据。

导入数据后，您可能希望修改并导出数据。让我们看看如何导出数据框。导入后，我们没有对数据框进行任何更改，但我们还是要将其导出。

![](img/8f7b465b34a73bb6dfa723c34c93cfe8.png)

图 2:将数据帧导出到本地磁盘。图片作者[作者](https://medium.com/@wiekiang)

如何导出数据帧的过程如图 2 所示。您将调用这个函数，`covid_daily_report.to_csv`，然后传递一个字符串名称作为备份文件。接下来，您应该通过创建一个新变量并调用`read_csv`函数来调用导出的 CSV 文件，从而尝试导入备份文件。

将数据导入 pandas 的典型方式是读取一个 CSV 文件，然后一旦您对其进行了更改或操作，您就可以将它作为一个修改过的数据集导出为一个 CSV 文件。

## 描述数据

为了描述数据框中的数据类型，我们可以使用`dtypes`属性。你会注意到，有些函数调用后有括号，有些没有。不带括号的是属性，比如`dtypes`，而带括号的是函数，比如`covid_daily_report.to_csv()`。

![](img/dc549c36d9dc56eda617cf4b7af631ee.png)

图 3: dtypes 属性。图片作者[作者](https://medium.com/@wiekiang)

属性和函数的区别在于，函数将执行一些步骤，而属性`dtypes`只是存储的关于`covid_daily_report`数据帧的一些元信息。关于属性和函数之间的区别，这是需要记住的两件主要事情。

属性会告诉你一些关于基于列的数据的信息。这个属性将告诉我们每一列的类型(图 3)。让我们看看我们可以利用该数据框做些什么。`covid_daily_report.columns`，它是一个属性，因为它没有括号。这将告诉我们列名，并以列表的形式返回。

![](img/04a023c83d855b01e1a0c2204b7b0937.png)

图 4:列属性。图片作者[作者](https://medium.com/@wiekiang)

您还可以将该列表存储到一个新的变量中，如图 4 所示，您可以对该列表执行一些操作，然后在以后使用它来操作您的数据框。这只是演示了如何从数据框中访问两种主要类型的属性。索引呢？可以叫`covid_daily_report.index`。这将返回索引的范围，从 0 开始，到 58 结束。

接下来，您将使用 describe 函数`covid_daily_report.describe()`从数据框中返回一些统计信息。您会注意到这个函数将只返回数字列，这意味着它只返回 float 和 int 数据类型。

![](img/6c6c1e9e2a8768b43c466f043ec1879e.png)

图 5:索引和描述。图片作者[作者](https://medium.com/@wiekiang)

重要的是要记住，这些类型的描述函数和`dtypes`属性是您在开始探索一组新数据时可能会遇到的。

也许我们想从数据框中获取更多信息。您可以通过调用函数`covid_daily_report.info()`来完成此操作。它有点像结合了数据类型的索引。你必须开始运行像 info 和 describe 这样的功能，同时探索你的数据并获得一些相关信息。

![](img/f346fa47bd9e181a00239514802ed075.png)

图 6:数据帧的详细信息。图片由[作者](https://medium.com/@wiekiang)

您可以对您的数据框调用另一种统计分析，例如均值。`covid_daily_report.mean()`，它会给你你的数值列的平均值。

![](img/66db3b63ec3043113567ab3c6e747b85.png)

图 7:熊猫的平均功能。图片作者[作者](https://medium.com/@wiekiang)

接下来，`covid_daily_report.sum()`。它将对所有不同列的值求和。这实际上是数据框列中内容的组合。在整个数据框上调用它是没有用的，所以也许我们在单个列上这样做，例如:

```
covid_daily_report["Confirmed"].sum()
covid_daily_report["Deaths"].sum()
covid_daily_report["Recovered"].sum()
covid_daily_report["Active"].sum()
```

![](img/b04bda81f543e178dd86c45fa28c283a.png)![](img/e19a79cff8752bd8a4110161f941253e.png)

图 8:熊猫的求和函数。图片作者[作者](https://medium.com/@wiekiang)

最后，如果您想要关于数据框长度的最后一点信息，您可以通过将它传递给 len 来计算出您当前正在处理的数据框的长度，我们可以看到我们的数据框中有 58 行。

![](img/39845dc034450af4201545779b427772.png)

图 9:数据帧的长度。图片由[作者](https://medium.com/@wiekiang)

## 查看和选择数据

您已经了解了描述我们导入到数据框中的数据的几种不同方式。让我们来看看查看和选择数据的一些不同方法。

![](img/1a739bea780ca4edd4502c1652c8e0da.png)

图 10:熊猫的头部功能。图片作者[作者](https://medium.com/@wiekiang)

您要做的第一件事是查看数据框的头部。你可以调用`covid_daily_report.head()`，记住这个后面有括号，所以它是一个函数。此函数将返回数据框前五行中的第一行。实际上，您可能会以某种方式操作数据框，然后经常调用 head。我们的数据框仅包含 58 行。因此，调用整个事情不是太糟糕，但想象一下，如果你有成千上万的这些。

head 所做的是在一个很小的空间内给你一个快速的快照，显示你的数据框所包含的内容。如果您在这里或那里进行了一些快速的更改，您可能想要查看前五行，如果五行不够呢？也许你想看看前八名。head 的美妙之处在于，你可以在这里输入一个数字，它会返回那么多行。

![](img/d135391c4d53d4cea557f1f6523cd104.png)

图 11:熊猫的尾巴功能。图片作者[作者](https://medium.com/@wiekiang)

出于某种原因，如果您需要返回数据框的底部，您可以使用`.tail()`，它将返回底部的行。如果您在数据框的底部而不是顶部进行修改，这将非常方便。变化只会出现在底部。

接下来我们要看的两个函数是`loc`和`iloc`。我们将在这里创建一个系列，以便详细演示`loc`和`iloc`之间的区别。我们要做一个叫`cars`的系列，在这里设置一些索引。默认情况下，如果我们创建了一个序列，索引将从 0 开始递增。然而，我们不会在这里这样做，因为我们想看看`loc`和`iloc`之间的区别。

![](img/22d2751361515ecac8273243ead56cc0.png)

图 12:pandas 系列中的 loc 和 iloc 示例。图片作者[作者](https://medium.com/@wiekiang)

我们这里有 7 个随机索引列表。如果我们调用`cars.loc[6]`，它将返回两个列表，奔驰和福特，因为`loc`引用了一个索引号，并且两者的索引号都是 6。另一方面，如果我们调用`cars.iloc[6]`，它只会返回马自达，因为它占据了 6 号位置。`iloc`指零分度法的位置。

![](img/2e316aee738d96a972fc4f2b585ea8b3.png)

图 13:数据帧中的 loc 和 iloc。图片由[作者](https://medium.com/@wiekiang)

关于`loc`和`iloc`有两个要点你要记住，`iloc`是指位置，`loc`是指索引。这件事的美妙之处在于，使用`loc`和`iloc`你可以使用切片。如果您曾经使用过 Python 列表，您可能对切片很熟悉。这和调用 head 函数是一样的。

![](img/92bf097528218fd1d4cd31548a6a022b.png)

图 14:在 loc 中切片，和熊猫的头部功能一样。图片作者[作者](https://medium.com/@wiekiang)

我们之前已经看到了这一点，但是让我重复一下关于列选择的信息。假设您想要选择一个特定的列。选择列的方法是在数据框名称旁边的方括号中输入其名称。您可能会看到两种方式，我希望您熟悉这两种选择列的方式，括号符号和点符号。

![](img/583c9f7328cd6ac20050f7f6350441de.png)

图 15:pandas 中带括号符号的列选择。图片由[作者](https://medium.com/@wiekiang)

![](img/6175bcb2d661c8051cdb8205cfad0066.png)

图 16:pandas 中带点符号的列选择。图片由[作者](https://medium.com/@wiekiang)

关于点符号，你应该知道的一件事是，如果你的列名中有一个空格，点符号将不起作用。因此，您可以考虑使用括号符号。

![](img/1da03bd00f310009c2379dd36fbcd5fe.png)

图 17:列选择中的过滤器。图片作者[作者](https://medium.com/@wiekiang)

另一个有趣的部分是，您可以在列选择中放置一点过滤器，如图 17 所示。

![](img/6a1b3389c152741a4b01912984dc2998.png)

图 18:熊猫视觉化的例子。图片作者[作者](https://medium.com/@wiekiang)

我们将在这里看一些可视化。你要打电话给`covid_daily_report[“Active”].plot()`。这将返回城市的活动案例的情节。熊猫的美妙之处在于，你可以通过给定的数据快速获得分析。获得可视化是快速理解正在发生的事情的好方法。

你想看的另一件事是另一种图，叫做直方图。这是观察分布的好方法。分布是一个关于数据传播的奇特词汇，您可以比较图 18 中的可视化。

## 操纵数据

作为数据科学家或机器学习工程师，研究的一部分是学习如何提出不同的问题。想想我如何利用这些数据，然后试着找出答案。话虽如此，我们还是来看看这里，如何用熊猫操纵数据。

一个真实的数据集通常在某一行包含缺失数据，称为`NaN`。只是说明那里没有价值。

> 如果你想用一些东西来填补这些缺失的值呢？

![](img/1ef35ed95d7e3d4bd178ad2ab86aca3d.png)

图 19:重新分配方法和就地参数方法。图片由[作者](https://medium.com/@wiekiang)

有一个函数叫做`fillna`，它用值填充这些，并且可以传递一些值并填充它。例如，您想用 0 填充“已确认”列中的一些缺失值。在 pandas 中有两种方法做同样的事情，重新赋值方法和就地参数。您可以在图 19 中看到重新分配和就地分配之间的区别。

![](img/2cc6a675c70e475e67c3ba485a290604.png)

图 20:熊猫的 dropna 功能。图片作者[作者](https://medium.com/@wiekiang)

例如，我们很高兴填写了“Confirmed”列，但是我们想忽略另一行的缺失值，因为缺失值对我们的数据分析没有帮助。事实上，它们可能是有用的，但是我们想知道如何摆脱它们。有一个功能叫`dropna`。您可以看到行数从 58 减少到 34 的结果(图 20)。

**如果我们想要重新访问丢失的值，该怎么办？**

由于我们在这里创建了一个新变量`covid_daily_report_drop_missing`
，您仍然可以通过调用`covid_daily_report`变量来访问原始数据帧。如果您弄乱了真实数据框，也可以重新导入 CSV 文件。向上滚动到文章的开头，查看如何重新导入图 1 中的 CSV。

![](img/7f657c09fbf7c4da57f69df9815e24ff.png)

图 21:从现有的列创建一个新的列。图片作者[作者](https://medium.com/@wiekiang)

接下来，我们如何从现有的列创建数据？Pandas 有几种不同的方法来生成数据，比如构建一个新列，或者用计算操作另一列中的现有数据。例如，您想计算出每个城市的恢复案例百分比。您可以创建一个名为“Recovered_Percentage”的新列，并从“Recovered”列进行一些计算，以获得百分比率，如图 21 所示。

![](img/b14ec30e7f4127a2d4f0254f8fe1386d.png)

图 22:在 pandas 中放置一列。图片由[作者](https://medium.com/@wiekiang)

也许你意识到你不再需要“Recovered_Percentage”列了。如果你想删除其中一列，你可以使用`drop`功能。它需要轴和原地参数。Axis 等于 1，因为 1 反映列，0 反映行。在这种情况下，您希望消除列，而不是行，所以我们在轴上放置 1。

## 随机抽样数据

当我们开始将机器学习算法应用于我们的数据时，他们最重要的步骤之一是创建一个训练验证和测试集。做到这一点的一个步骤是随机排列数据的顺序。因此，用一个`sample`函数来混合索引是一个很好的方法。

![](img/c189debdbc914ceb5ca36adf7b3c62e8.png)

图 23:pandas 中的示例函数。图片作者[作者](https://medium.com/@wiekiang)

`sample`函数从数据帧中抽取一个样本，其中包含一个参数`frac`。参数是整数形式。例如，为了获得 50%的数据，我们将 0.5 放入参数中。因此，如果你想混洗所有东西，100%的数据样本，我们在参数中传递 1。这将返回数据的混排索引。

![](img/605de92435aa86fcf84352a81e776e67.png)

图 24:从数据帧中只选择 20%的样本。图片作者[作者](https://medium.com/@wiekiang)

实际上，您可能只需要 20%的数据。在`frac`参数中，可以传入 0.2 的值。这将从数据帧中返回混洗的 20%。这是您将来在项目中处理大量数据时必须考虑的问题。

一个问题是，我们如何让这些索引恢复秩序？有一个函数叫做`reset_index`，后面跟着两个参数`drop`和`inplace`，如图 25 所示。

![](img/77bc497a6ec1a1fbfff9c1373cb0e2fe.png)

图 25:重置混洗索引。图片由[作者](https://medium.com/@wiekiang)

我们要看的最后一件事是如何对列应用函数。pandas 中的 Apply function 将允许您对特定的列应用一些函数，无论是 NumPy 还是 lambda。在图 26 中，您可以看到我们将 lambda 函数应用于“Active”列。这将把“活动”列的值与“死亡”列的值相加，并将其重新分配到“活动”列。

![](img/04e9e60ac0a72f3c07c7e41a1967954b.png)

图 26:对列应用一个函数。图片作者[作者](https://medium.com/@wiekiang)

## 结论

您已经看到了 pandas 可以做的一些事情，特别是使用 Python 从某个数据集进行数据分析。是时候通过访问 Github 中的 analysis Jupyter 笔记本文件来练习您的技能了。

## 关于作者

Wie Kiang 是一名研究人员，负责收集、组织和分析意见和数据，以解决问题、探索问题和预测趋势。

他几乎在机器学习和深度学习的每个领域工作。他正在一系列领域进行实验和研究，包括卷积神经网络、自然语言处理和递归神经网络。

*连接上*[*LinkedIn*](https://linkedin.com/in/wiekiang)

```
**Citation**Dong E, Du H, Gardner L. An interactive web-based dashboard to track COVID-19 in real time. Lancet Inf Dis. 20(5):533-534\. doi: 10.1016/S1473-3099(20)30120-1**References**#1 [COVID-19 Data Repository by the Center for Systems Science and Engineering (CSSE) at Johns Hopkins University](https://github.com/CSSEGISandData/COVID-19/tree/master/csse_covid_19_data/csse_covid_19_daily_reports_us)#2 [Github repositories for the analysis](https://github.com/wiekiang/covid-jhu-csse)
```

***编者注:*** [*迈向数据科学*](http://towardsdatascience.com/) *是一份以数据科学和机器学习研究为主的中型刊物。我们不是健康专家或流行病学家，本文的观点不应被解释为专业建议。想了解更多关于疫情冠状病毒的信息，可以点击* [*这里*](https://www.who.int/emergencies/diseases/novel-coronavirus-2019/situation-reports) *。*