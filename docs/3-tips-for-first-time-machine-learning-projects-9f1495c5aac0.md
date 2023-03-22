# 首次机器学习项目的 3 个技巧

> 原文：<https://towardsdatascience.com/3-tips-for-first-time-machine-learning-projects-9f1495c5aac0?source=collection_archive---------46----------------------->

## 帮助构建成功顶点项目的实用工具和技术。

![](img/149b8c03cf02bf05c71b9fa09c901a3e.png)

[来源](https://unsplash.com/photos/icrhAD-qidc)

自从我的顶点团队完成我们的森林火灾管理机器学习项目以来，已经过去了几个月。这是一次很棒的经历，最终让我们赢得了 2020 年安大略省软件工程顶点竞赛！随着学校开学，学生们开始新的项目，我认为分享一些在这个项目中帮助我的技巧是有用的。

总的来说，我从涉及机器学习的顶点工作中获得的最大收获是，90%的工作都是让数据屈服(或者与数据成为朋友，这取决于你的观点)。

理解干净数据、探索性数据分析、数据争论的重要性对我们项目的成功绝对至关重要。我认为这一点在课程工作中经常被忽视，这会导致人们对这样一个项目中所涉及的工作有一种扭曲的看法。因此，这里有一些工具和技术(有例子)帮助我的团队与数据保持健康的关系，并使项目更加愉快。

# **1。与 Google Colab 的合作**

Google Colab 是一款来自 Google Drive 的用于编码和运行 Jupyter 笔记本(常用于机器学习项目)的工具。与本地 Python 脚本甚至本地 Jupyter 笔记本相比，使用 Google Colab 有两大优势。

首先，Colab 让你可以使用免费的云图形处理器，这可以真正加快你的工作流程。第二，使用 Colab 允许在 Google Drive 上存储数据(每个帐户最多有 15GB)，这使得在数据争论上的合作更加容易。为了利用这些功能，我们的团队在 Google Drive 上使用了一个包含我们的 Colab 笔记本的共享文件夹，以及一个包含 gzipped csv 文件的共享数据文件夹，这些文件由一些笔记本输出，由其他笔记本用作输入。由于 Google Drive 文件夹是共享的，所以每个人都可以很容易地访问团队其他成员产生的最新数据。

下面是一个使用 Google Colab 笔记本从共享文件夹导入 gzipped csv 的示例:

如果你正在使用大的 csv 文件，我建议使用 gzip 压缩来存储它们，因为 pandas 包可以在一行代码中快速地解压缩和读入文件。

在我们的项目中，我们有 15 个活动的笔记本文件，它们有不同的相互依赖关系，因此有一个明确的位置来存储数据是非常有帮助的。

# **2。使用 aiohttp 加速数据采集**

温度、风和湿度都对森林火灾是否会发生以及是否会蔓延有重大影响。由于这些值在一天中变化很大，我们必须从加拿大各地的气象站获得几年来的每小时数据。

对我们来说幸运的是，加拿大政府(加拿大环境和气候变化部)拥有加拿大数百个气象站的[历史每小时天气数据](https://climate.weather.gc.ca/)，有些数据甚至可以追溯到 1866 年！他们甚至提供了完全免费的 [API](https://drive.google.com/drive/folders/1WJCDEU34c60IfOnG4rv5EPZ4IhhW9vZH) 来批量下载历史天气数据。

下载每小时数据的 API 要求用户指定站 ID、年份和月份。因此，如果我们想要过去 10 年中 150 个站点的数据，这将意味着 150x10x12 = 18，000 个 API 调用。按顺序执行这些调用需要很长时间，所以我们使用 aiohttp 包并发地执行 API 调用。

上面的代码可以用来获取任何类型的 csv 数据。

# **3。矢量化**

在特征工程中，我们的团队假设绝对湿度可以提供额外的信息，有助于森林火灾的预测。绝对湿度可以从温度和相对湿度中导出，使用一个特殊的公式导出[这里是](https://carnotcycle.wordpress.com/2012/08/04/how-to-convert-relative-humidity-to-absolute-humidity/)。

下面是代码中的公式:

有几种方法可以将它应用于熊猫数据框中每小时的天气数据。我们可以遍历数据帧中的每一行，计算绝对湿度:

```
**%%timeit -n 3** abs_humidity_list = []
for index, row in df.iterrows():
    temperature = row['Temp (°C)']
    rel_humidity = row['Rel Hum (%)']
    abs_humidity = calculate_abs_humidity(temperature, rel_humidity)
    abs_humidity_list.append(abs_humidity)
```

**结果:** 3 次循环，3 次最佳:每次循环 8.49 秒

虽然上面的代码完成了这项工作，但是它为每一行计算一次函数。为了更快获得结果，可以使用熊猫应用功能:

```
**%%timeit -n 3** abs_humidity_series = df.apply(lambda row:
    calculate_abs_humidity(row['Temp (°C)'], row['Rel Hum (%)']),
    axis=1)
```

**结果:** 3 次循环，3 次最佳:每次循环 1.9 秒

即使这样更快，pandas apply 函数基本上仍然是一个循环。为了真正实现快速计算，我们希望将***create _ ABS _ weather***函数应用于同时**的所有温度和相对湿度对**。在我们的大多数项目笔记本中，我们的团队为此使用了 numpy 的 ***矢量化*** 功能:

```
import numpy as np**%%timeit -n 3** abs_humidity_np = np.vectorize(calculate_abs_humidity)(
    df[‘Temp (°C)’],
    df[‘Rel Hum (%)’])
```

**结果:** 3 次循环，3 次最佳:每次循环 34.2 毫秒

不错的进步。然而，在这篇文章的写作过程中，我发现使用 numpy 的*矢量化仍然不是[真正的矢量化](https://stackoverflow.com/questions/52673285/performance-of-pandas-apply-vs-np-vectorize-to-create-new-column-from-existing-c)。事实上，我们无法使用***calculate _ ABS _ weather***进行真正的矢量化，因为在该函数中，我们使用了 ***math.exp*** ，它一次只能对一个数字进行操作，而不能对一长串数字(向量)进行操作。*

*幸运的是，numpy 有一个等效的指数函数，它作用于向量， ***np.exp，*** 因此，如果我们对原来的***calculate _ ABS _ weather***函数做一点小调整，去掉指数函数，我们会得到:*

*突然间，我们有了一个可以接收整个熊猫数据帧列的函数:*

```
***%%timeit -n 3**
calculate_abs_humidity_np(df[‘Temp (°C)’], df[‘Rel Hum (%)’])*
```

***结果:** 3 次循环，3 次最佳:每次循环 5.64 毫秒*

*真正的矢量化！*

*你可能会说，“矢量化真的那么重要吗？节省 8 秒钟似乎没什么大不了的。”对于小数据集，我同意它不会产生巨大的差异。作为参考，上面的示例使用了过去 10 年来自单个气象站的每小时数据，结果为 78，888 行(仍然相对较小)。*

*但是，如果我们想计算同一时期 100 个气象站数据的绝对湿度呢？现在，如果您正在遍历每一行，您将不得不等待 13 分钟，这就是矢量化开始产生重大影响的地方。矢量化还有一个额外的好处，就是可以让代码更加简洁易读，所以我建议尽可能养成使用它的习惯。*

# ***最终注释***

*顶点项目中使用的最终森林火灾预测模型是在包含超过 1600 万行的数据集上训练的(每行包含过去 10 年中某一天 20x20 公里网格的信息)。组装该数据集被证明是项目的关键部分，通过使用以下工具，整个过程变得更加简单:*

*   ***Google Colab**——方便分享和协作数据*
*   *用于快速获取大量数据*
*   ***向量化函数** —用于对具有数百万行的数据集执行高效操作*

*我已经将所有示例代码包含在一个笔记本文件中:[https://github . com/ivanzvonkov/examples-3-tips-for-ml-projects/blob/master/example-notebook . ipynb](https://github.com/ivanzvonkov/examples-3-tips-for-ml-projects/blob/master/example-notebook.ipynb)。*

*我希望您有机会在您的下一个项目中尝试这些工具！*

*如果你想查看实际的顶点工程库，你可以在这里找到:[https://github.com/ivanzvonkov/forestcasting](https://github.com/ivanzvonkov/forestcasting)。*

*感谢阅读，感谢所有反馈！*