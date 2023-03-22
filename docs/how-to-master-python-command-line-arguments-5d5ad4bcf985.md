# 如何掌握 Python 命令行参数

> 原文：<https://towardsdatascience.com/how-to-master-python-command-line-arguments-5d5ad4bcf985?source=collection_archive---------11----------------------->

## 使用命令行参数创建自己的 Python 脚本的简单指南

![](img/b250854533c4c24de6e8cdc096d4b187.png)

照片由[émile Perron](https://unsplash.com/@emilep?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)在 [Unsplash](https://unsplash.com/s/photos/python-programming?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) 上拍摄

我相信我们大多数人都运行过这个命令行来执行您的 python 脚本。

```
$ python main.py
```

我们能不能在这个脚本中定义我们自己的论点？答案肯定是肯定的！

```
$ python main.py arg1 arg2
```

我们将使用 Python[**arg parse**](https://docs.python.org/3.3/library/argparse.html)模块来配置命令行参数和选项。argparse 模块使得编写用户友好的 [**命令行界面**](https://en.wikipedia.org/wiki/Command-line_interface) 变得容易。该程序定义了它需要什么参数，并且 [argparse](https://docs.python.org/3.3/library/argparse.html#module-argparse) 将计算出如何解析来自 [sys.argv](https://docs.python.org/3.3/library/sys.html#sys.argv) 的参数。当用户给程序无效的参数时， [argparse](https://docs.python.org/3.3/library/argparse.html#module-argparse) 模块也会自动生成帮助和用法消息，并发出错误。

# Argparse 入门

## 正在安装 Argparse

像往常一样，我们需要做的第一件事是安装这个 Python 模块。

```
conda install argparse
```

## 定义位置参数和可选参数

用这个脚本的描述用`ArgumentParser`创建一个`parser`对象。`Positional Arguments`和`Optional Arguments`用`add_argument`函数定义。用`help`添加了对该参数所做工作的简要描述。

> **位置自变量**是需要包含在适当的**位置**或顺序中的**自变量**。
> **可选参数**是用关键字和等号输入的**关键字** **参数**，可选。

*   让我们尝试使用 help 参数`-h`运行这个脚本。

```
$ python employee.py -h               
usage: employee.py [-h] [--address ADDRESS] name titleThis script is going to create an employee profile.positional arguments:
  name               Name of Employee
  title              Job Title of Employeeoptional arguments:
  -h, --help         show this help message and exit
  --address ADDRESS  Address of Employee
```

`-h`和`--help`在`argparse`中默认定义。它将显示我们在脚本中定义的描述，以帮助用户使用脚本。

*   我们试着输入`name`和`title`。

```
$ python employee.py Alex Manager
Name : Alex
Job Title : Manager
Address : None
```

由于缺少地址参数，在这个脚本中，`NoneType`被传递给`Address`。为了打印它，我们必须把它转换成字符串。

*   让我们只用`name`试试

```
$ python employee.py Alex                                    
usage: employee.py [-h] [--address ADDRESS] name title
employee.py: error: the following arguments are required: title
```

因为 title 也是`Positional Argument`，所以在这个脚本中是需要的。

*   这次我们用`name`、`title`和`address`试试。

```
$ python employee.py Alex Manager --address 123 Baker Street
usage: employee.py [-h] [--address ADDRESS] name title
employee.py: error: unrecognized arguments: Baker Street
```

因为`123 Baker Street`包含空格，所以脚本会将`Baker Street`视为其他参数。在这种情况下，我们需要双引号。

```
$ python employee.py Alex Manager --address "123 Baker Street"
Name : Alex
Job Title : Manager
Address : 123 Baker Street
```

> `name`和`title`如果名称或标题不止一个单词，则需要双引号。

## 定义布尔参数

让我们将上述代码添加到现有的脚本中。我们将使用`default=True`定义一个可选参数。意思是即使我们从来没有在这个参数中输入任何东西，默认情况下也等于`True`。

这里使用`type=strtobool`来确保输入被转换成**布尔**数据类型。否则，当脚本在输入中传递时，它将是**字符串**数据类型。如果我们需要一个整数参数，我们也可以将其定义为`type=int`。

`%(default)s)`在帮助中是检索参数中的默认值。这是为了确保描述不是硬编码的，并且随着缺省值的变化是灵活的。

*   让我们用`name`、`title`和`address`再试一次

```
$ python employee.py Alex Manager --address "123 Baker Street"                   
Name : Alex
Job Title : Manager
Address : 123 Baker Street
Alex is a full time employee.
```

默认情况下，`isFullTime`是`True`，所以如果我们不向`isFullTime`输入任何参数，Alex 就是全职员工。

*   让我们用一个`False`输入试试。

```
$ python employee.py Alex Manager --address "123 Baker Street" --isFullTime False
Name : Alex
Job Title : Manager
Address : 123 Baker Street
Alex is not a full time employee.
```

行动。亚历克斯现在不是全职员工。

## 定义参数中的选择

我们还可以用`choices`参数限制输入参数的可能值。这有助于防止用户输入无效值。例如，我们可以使用这个`choices=[“Singapore”, “United States”, “Malaysia”]`将国家值限制为新加坡、美国和马来西亚。

*   让我们看看如果我们不在选项中输入国家值会发生什么。

```
$ python employee.py Alex Manager --country Japan    
usage: employee.py [-h] [--address ADDRESS]
                   [--country {Singapore,United States,Malaysia}]
                   [--isFullTime ISFULLTIME]
                   name title
employee.py: error: argument --country: invalid choice: 'Japan' (choose from 'Singapore', 'United States', 'Malaysia')
```

我们将遇到一个无效选择的错误。这些选择在`--help`总是可用的。

现在你已经学会了如何创建带有自定义参数的 **Python 命令行**。希望这篇文章对你有用。如果我犯了任何错误或错别字，请给我留言。

可以在我的 [**Github**](https://github.com/chingjunetao/medium-article/blob/master/python-command-line/employee.py) 中查看完整的脚本。干杯！

**如果你喜欢读这篇文章，你可能也会喜欢这些:**

[](/manage-your-python-virtual-environment-with-conda-a0d2934d5195) [## 使用 Conda 管理您的 Python 虚拟环境

### 轻松在 Python 2 和 Python 3 环境之间切换

towardsdatascience.com](/manage-your-python-virtual-environment-with-conda-a0d2934d5195) [](/access-google-search-console-data-on-your-site-by-using-python-3c8c8079d6f8) [## 使用 Python 访问您站点上的 Google 搜索控制台数据

### 谷歌搜索控制台(以前的谷歌网站管理员工具)是由谷歌提供的网络服务，帮助您监控和…

towardsdatascience.com](/access-google-search-console-data-on-your-site-by-using-python-3c8c8079d6f8) 

**你可以在 Medium 上找到我其他作品的链接，关注我** [**这里**](https://medium.com/@chingjunetao) **。感谢阅读！**