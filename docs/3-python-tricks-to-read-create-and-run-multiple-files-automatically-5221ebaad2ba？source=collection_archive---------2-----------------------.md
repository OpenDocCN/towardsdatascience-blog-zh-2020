# 自动读取、创建和运行多个模型的 3 个 Python 技巧

> 原文：<https://towardsdatascience.com/3-python-tricks-to-read-create-and-run-multiple-files-automatically-5221ebaad2ba?source=collection_archive---------2----------------------->

## 用 Python 和 Bash For Loop 自动化枯燥的东西

![](img/9b58d1bcecf2bc9cd07984ed2dacd16b.png)

由[真诚媒体](https://unsplash.com/@sincerelymedia?utm_source=medium&utm_medium=referral)在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄

# 动机

将代码投入生产时，您很可能需要组织代码文件。读取、创建和运行许多数据文件非常耗时。本文将向您展示如何自动

*   遍历目录中的文件
*   如果嵌套文件不存在，则创建嵌套文件
*   使用 bash for 循环运行一个具有不同输入的文件

这些技巧让我在从事数据科学项目时节省了大量时间。我希望你也会发现它们很有用！

# 遍历目录中的文件

如果我们有多个数据要像这样读取和处理:

```
├── data
│   ├── data1.csv
│   ├── data2.csv
│   └── data3.csv
└── main.py
```

我们可以尝试一次手动读取一个文件

```
import pandas as pd def process_data(df):
   passdf = pd.read_csv(data1.csv)
process_data(df)df2 = pd.read_csv(data2.csv)
process_data(df2)df3 = pd.read_csv(data3.csv)
process_data(df3)
```

当我们有 3 个以上的数据时，这种方法是可行的，但效率不高。如果我们在上面的脚本中唯一改变的是数据，为什么不使用 for 循环来访问每个数据呢？

下面的脚本允许我们遍历指定目录中的文件

```
data/data3.csv
data/data2.csv
data/data1.csv
```

以下是对上述脚本的解释

*   `*for* filename *in* os.listdir(*directory*)`:遍历特定目录下的文件
*   `if filename.endswith(".csv")`:访问以'结尾的文件。csv '
*   `file_directory = os.path.join(*directory*, filename)`:连接父目录(‘数据’)和目录内的文件。

现在我们可以访问“数据”目录中的所有文件了！

# 如果嵌套文件不存在，则创建嵌套文件

有时我们可能想要创建嵌套文件来组织我们的代码或模型，这使得我们将来更容易找到它们。例如，我们可以使用“模型 1”来指定特定的特征工程。

在使用模型 1 时，我们可能希望使用不同类型的机器学习模型来训练我们的数据(“模型 1/XGBoost”)。

在使用每个机器学习模型时，由于模型使用的超参数的差异，我们甚至可能希望保存模型的不同版本。

因此，我们的模型目录可能看起来像下面这样复杂

```
model
├── model1
│   ├── NaiveBayes
│   └── XGBoost
│       ├── version_1
│       └── version_2
└── model2
    ├── NaiveBayes
    └── XGBoost
        ├── version_1
        └── version_2
```

为我们创建的每个模型手动创建一个嵌套文件可能会花费我们很多时间。有没有一种方法可以让这个过程自动化？是的，用`os.makedirs(datapath).`

运行上面的文件，您应该看到自动创建的嵌套文件“model/model2/XGBoost/version_2 ”!

现在您可以将您的模型或数据保存到新目录中了！

# Bash for 循环:用不同的参数运行一个文件

如果我们想用不同的参数运行一个文件呢？例如，我们可能希望使用相同的脚本来预测使用不同模型的数据。

如果一个脚本需要很长时间来运行，并且我们有多个模型要运行，那么等待脚本运行完毕然后运行下一个脚本将会非常耗时。有没有办法让电脑运行模型 1，2，3，..，10，然后去做别的事情。

是的，我们可以用 for bash for loop。首先，我们使用`sys.argv`来解析命令行参数。如果你想在命令行中覆盖你的配置文件，你也可以使用像 [hydra](https://hydra.cc/) 这样的工具。

```
>>> python train.py XGBoost 1
Loading model from model/model1/XGBoost/version_1 for training
```

太棒了。我们刚刚告诉我们的脚本使用模型 XGBoost，版本 1 来预测命令行上的数据。现在我们可以使用 bash for 循环遍历模型的不同版本。

如果你可以用 Python 做 for 循环，你也可以在终端上这样做，如下所示

```
$ for version in 2 3 4
> do
> python train.py XGBoost $version
> done
```

键入 Enter 来分隔各行

输出:

```
Loading model from model/model1/XGBoost/version_1 for training
Loading model from model/model1/XGBoost/version_2 for training
Loading model from model/model1/XGBoost/version_3 for training
Loading model from model/model1/XGBoost/version_4 for training
```

现在，您可以在让您的脚本使用不同的模型运行的同时做其他事情！多方便啊！

# 结论

恭喜你！您刚刚学习了如何一次自动读取和创建多个文件。您还学习了如何使用不同的参数运行一个文件。现在，您可以将手动读取、写入和运行文件的时间节省下来，用于更重要的任务。

如果你对文章中的某些部分感到困惑，我在这个[回购](https://github.com/khuyentran1401/Data-science/tree/master/python/python_tricks)中创建了具体的例子。

我喜欢写一些基本的数据科学概念，并尝试不同的算法和数据科学工具。你可以在 [LinkedIn](https://www.linkedin.com/in/khuyen-tran-1ab926151/) 和 [Twitter](https://twitter.com/KhuyenTran16) 上联系我。

如果你想查看我写的所有文章的代码，请点击这里。在 Medium 上关注我，了解我的最新数据科学文章，例如:

[](/how-to-get-a-notification-when-your-training-is-complete-with-python-2d39679d5f0f) [## 使用 Python 获得通知的 3 种方式

### 现在，您可以在等待培训完成的同时从事其他项目

towardsdatascience.com](/how-to-get-a-notification-when-your-training-is-complete-with-python-2d39679d5f0f) [](/how-to-create-reusable-command-line-f9a2bb356bc9) [## 如何创建可重用的命令行

### 你能把你的多个有用的命令行打包成一个文件以便快速执行吗？

towardsdatascience.com](/how-to-create-reusable-command-line-f9a2bb356bc9) [](/how-to-create-fake-data-with-faker-a835e5b7a9d9) [## 如何用 Faker 创建假数据

### 您可以收集数据或创建自己的数据

towardsdatascience.com](/how-to-create-fake-data-with-faker-a835e5b7a9d9) [](/cython-a-speed-up-tool-for-your-python-function-9bab64364bfd) [## cy thon——Python 函数的加速工具

### 当调整你的算法得到小的改进时，你可能想用 Cython 获得额外的速度，一个…

towardsdatascience.com](/cython-a-speed-up-tool-for-your-python-function-9bab64364bfd) [](/timing-the-performance-to-choose-the-right-python-object-for-your-data-science-project-670db6f11b8e) [## 高效 Python 代码的计时

### 如何比较列表、集合和其他方法的性能

towardsdatascience.com](/timing-the-performance-to-choose-the-right-python-object-for-your-data-science-project-670db6f11b8e)