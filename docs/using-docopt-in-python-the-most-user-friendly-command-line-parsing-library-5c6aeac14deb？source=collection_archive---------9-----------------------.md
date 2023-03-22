# 在 python 中使用 Docopt，这是最用户友好的命令行解析库

> 原文：<https://towardsdatascience.com/using-docopt-in-python-the-most-user-friendly-command-line-parsing-library-5c6aeac14deb?source=collection_archive---------9----------------------->

![](img/50c51cb4c70cb5e5be5c6864290a8b30.png)

来源:[普里西拉·杜·普里兹](https://unsplash.com/@priscilladupreez)， [Unsplash](https://unsplash.com/photos/bZQJLStVYWs)

## 在每个项目的运行文件中，将参数解析成 python 文件是必不可少的。本文向您展示了 Docopt 的实践

当你处理一个有很多参数的文件时，总是在 python 文件中改变这些参数是很费时间的，而且对用户也不友好。已经实现了一些库来解决这个问题，称为命令行解析库。在 python 中，它们是三种不同的解决方案: [Docopt](http://docopt.org/) 、 [Argparse](https://docs.python.org/3/library/argparse.html) 和 [Click](https://click.palletsprojects.com/en/7.x/) 。

在本文中，我将深入探讨 Docopt 的使用，而不是 Argparse 或 Click。我做出这个选择是因为在使用了所有这三种方法之后，我意识到 Docopt 是迄今为止将参数解析到文件中最用户友好的方式。

我展示 Docopt 库的方式是向您展示一个带有 **setup.py** 和 **requirements.txt** 文件的 **docopt.py** 文件示例。

```
### docopt.py file '''
Usage:docopt.py square [options] [operation] [square-option]docopt.py rectangle [options] [operation] [triangle-option]operation:
   --area=<bool>       Calculate the area if the argument==True
   --perimeter=<bool>  Calculate the perimeter if the argument==Truesquare-option:
   --edge=<float>      Edge of the square. [default: 2]rectangle-option:
   --height=<float>    Height of the rectangle 
   --width=<float>     Width of the rectangle '''
from docopt import docoptdef area(args):
   """
   This method computes the area of a square or a rectangle 
   """   
   if bool(args['square']):
      if args['--edge']:
         area = float(args['edge']) ** 2
      else:
         print("You must specify the edge")
   elif bool(args['rectangle']):
      if args['--height'] and args['--width']:
         area = float(args['--height']) * float(args['--width'])
      else:
         print("You have forgotten to specify either the height or       the width or both")
   return area def perimeter(args):
   """
   This method computes the perimeter of a square or a rectangle 
   """
   if bool(args['square']):
      if args['--edge']:
         perimeter = float(args['edge']) * 2
      else:
         print("You must specify the edge")
   elif bool(args['rectangle']):
      if args['--height'] and args['--width']:
         perimeter = float(args['--height']) + float(args['--width'])
      else:
         print("You have forgotten to specify either the height or       the width or both")
   return perimeterdef main():
   args = docopt(__doc__) if args['--area']:
      area(args)
   elif args['--perimeter']:
      perimeter(args)
   else:
      print("command not found")if __name__=='__main__':
   main()
```

它们是 **docopt.py** 文件中的两部分:

*   文档字符串中参数的定义
*   您想要/需要调用的不同方法

# 参数定义

在用法部分，你写下你的命令行在终端中的调用格式。因此，如果我想在边长为 5 的正方形上调用面积运算，我将执行以下操作。：

```
python -m docopt.py square --area=True --edge=5 
```

如您所见，在“**用法**”部分，我们可以定义默认值，但是如果我们像使用“ **— edge** 参数那样在命令行中指定值，这些值将被覆盖。

所以让我们分解命令行:

*   **docopt.py** :是您想要运行的 python 文件的名称
*   **正方形**或**矩形**是我们需要在命令行中给出的两个必要参数，以正确运行它
*   [ **options** ]:是一个必要的括号参数，声明下面的元素将是可选参数
*   【**运算**】:由两个不同的参数组成，分别是“—面积”和“—周长”。对于这两个参数，我们需要在命令行中输入的值是一个布尔值。' = < bool >'是可选的，但是给出了用户需要输入哪种元素的提示。
*   【**方形选项**】:由一个参数组成，默认值为

# 方法

在这一部分，我将通过'**区**'和'**周边**'的方法。但是在这样做之前，您注意到您需要导入 docopt 包，并且在此之前您需要安装它。然而，暂时忘记安装，我会给你一个干净的方法来做。

您会注意到，在这两种方法中，当我需要来自 docopt 的参数时，我需要将其转换为正确的类型。例如 args['square']是一个布尔值，但我需要显式地将其转换为布尔值。

然后，计算面积和周长的方法是非常基本的方法，但是是可选的方法。

然而，在文件的末尾有两行是正确运行该文件所必需的:

```
if __name__=='__main__':
   main()
```

现在我们已经创建了我们的 **docopt.py** 文件，我们可以创建一个 **setup.py** 文件，只要一个 requirements.txt 文件，使我们的项目更加用户友好。

一个 [**setup.py**](https://docs.python.org/3/distutils/setupscript.html) 文件是一个 python 文件，在其中你描述了你的模块向 Distutils 的分布，以便在你的模块上操作的各种命令做正确的事情。在这个文件中，您可以指定要运行项目中的哪些文件，以及运行项目所需的哪些包。

在进入 **setup.py** 文件之前，您的项目中需要有以下架构:

```
/docopt-demo
   docopt.py
   requirements.txt
   setup.py
```

这里有一个关于这个项目的 **setup.py** 文件的例子:

```
### setup.py file from setuptools import setupwith open("requirements.txt", "r") as fh:
    requirements = fh.readlines()setup(
   name = 'docopt-demo',
   author = 'JonathanL',
   description = 'Example of the setup file for the docopt demo',
   version = '0.1.0',
   packages = ['docopt-demo'],
   install_requires = [req for req in requirements if req[:2] != "# "],
   include_package_data=True,
   entry_points = {
      'console_scripts': [
         'docopt = docopt-demo.docopt:main'
      ]
   }
)
```

这是 requirements.txt 文件:

```
# Argument parser packagesdocopt==0.6.2
```

# 解释 setup.py 文件

你必须做的第一个元素是从 **setuptools** 中**导入设置**，但是这个包是 python 自带的，所以如果你的电脑上有 python，它就会存在。

然后，您需要创建一个所有必需包的列表，您将在安装模块的 **install_requires** 参数中解析这些包。

在设置模块中，您需要了解一些要素:

*   **包**:应该和你所在的文件夹同名，所以对我们来说是‘doc opt-demo’
*   在' **console_script** 中，您可以为特定文件指定快捷方式。在我们的例子中，不是在终端中键入:

```
python -m docopt square --area=True --edge=5
```

我们只需要输入:

```
docopt square --area=True --edge=5
```

事实上，我们将单词' **docopt** '与操作'**doc opt-demo . doc opt:main**'相关联，该操作启动 docopt.py 文件中的主函数。

# 解释要求. txt 文件

在这个文件中，您指定了正确运行项目所需安装的所有包。

您有两种选择来指定版本:

*   要么你告诉确切的版本，你想使用，你会使用' == '符号
*   或者您也可以使用像' > = '这样的操作符来指定包的旧版本

# 结论

在本文中，我将展示如何使用 Docopt python 包，这是一种将参数解析到文件中的非常好的方法。

我希望你在这篇文章中找到了你来这里的目的，并在接下来的图像识别之旅中与我在一起！

如果你喜欢阅读这样的故事，并想支持我成为一名作家，考虑注册成为一名灵媒成员。每月 5 美元，你可以无限制地阅读媒体上的故事。如果你使用[我的链接](http://If you enjoy reading stories like these and want to support me as a writer consider signing up to become a Medium member. It's $5 a month, giving you unlimited access to stories on Medium. If you sign up using my link, I will earn a small commission and you will still pay $5\. Thank you !! https://medium.com/@jonathan_leban/membership)注册，我将赚取一小笔佣金，你仍需支付 5 美元。谢谢大家！！

[](https://medium.com/@jonathan_leban/membership) [## 通过我的推荐链接加入媒体-乔纳森·莱班

### 阅读乔纳森·莱班的每一个故事(以及媒体上成千上万的其他作家)。您的会员费直接支持…

medium.com](https://medium.com/@jonathan_leban/membership) 

*PS:我目前是 Unity Technologies 的机器学习工程师，刚刚从加州大学伯克利分校的工程硕士毕业，如果你想讨论这个话题，请随时联系我。* [*这里的*](http://jonathan_leban@berkeley.edu/) *是我的邮箱。*