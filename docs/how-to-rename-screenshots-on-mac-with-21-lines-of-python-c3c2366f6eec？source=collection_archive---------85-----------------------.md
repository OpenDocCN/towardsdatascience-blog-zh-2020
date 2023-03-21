# 如何用 21 行 Python 在 Mac 上重命名截图

> 原文：<https://towardsdatascience.com/how-to-rename-screenshots-on-mac-with-21-lines-of-python-c3c2366f6eec?source=collection_archive---------85----------------------->

## 大蟒

## 使用操作系统和日期时间模块，通常重复的任务现在只需点击一下。

![](img/b857c2a4e33f308eee4b17a79b91feba.png)

小姐姐为这个项目创作的原创图片

在 Mac 上截屏很简单，只需点击 **Command-Shift-3** 进行全屏截图，或者点击 **Command-Shift-4** 进行部分屏幕截图。你也可以用终端为你的截图设置一个自定义标签，但是有时候你需要更多的自定义。

作为一个为不同项目拍摄批量截图的人，我经常需要重命名截图以适应这种格式:

project name _ date created _ screenshotcount . png

只有 21 行 Python 代码，现在我只需点击桌面上的一个可执行文件，所有的截图都会被重命名以匹配上面的格式。

![](img/930d37e8b007798bb918608d37ace84c.png)

使用 Python 脚本在 Mac 上重命名屏幕截图

每当我需要重命名一批 20-30 个截图时，这可以节省我大约一分钟的时间。但是随着时间的推移，这种情况会越来越多，所以我决定使用 Python 来自动化这些枯燥的东西！

下面看看这个脚本是如何工作的，以及如何让它在 Mac 上可执行。

# 使用 Python 的 os 和 datetime 模块在 Mac 上重命名屏幕截图

在写这个脚本之前，我没有改变任何预先构建的截图设置。默认情况下，我的截图如下:

![](img/e3387b10275311a77a8bf6a94239907c.png)

我电脑上的截图示例

因为我为不同的项目拍摄了一批截图，所以我希望每个文件名都以项目标签开头。我还想保留截图拍摄的原始日期，但我不需要时间。最后，为了确保没有重复，每个截图的文件名后面都应该有一个数字。

## 配置您的截图标签和当前工作目录

首先，我们需要导入必要的模块:

```
import os
from datetime import datetime
```

**操作系统模块**允许我们通过 Python 与您的操作系统交互。使用这个模块，我们可以读写文件、操作路径等等。 **datetime 模块**也内置在 Python 中，它为我们提供了在脚本中操作日期和时间的类。

接下来，我们将添加一个项目名称提示，用于标记每个截图的文件名:

```
project = input('Enter the project name: ')
```

然后，我们需要启用我们的脚本来找到截图。为此，我们将当前工作目录更改为我们系统的桌面。我们可以手动找到个人桌面的路径，并将其硬编码到脚本中，但为了避免这样做，我们可以使用这两行代码:

```
desktop = os.path.join(os.environ['HOME'], 'Desktop')
os.chdir(desktop)
```

我们来分析一下。

“ **os.environ** ”将返回我们的“环境变量”的字典，这些变量是我们的 Python 脚本运行所在的系统的特性。写出" **os.environ['HOME']** "将返回字典的一个子集，其中只有我们的主目录的路径名。在 Mac 上，根据您配置机器名称的方式，您将得到类似于以下内容的内容:

```
/Users/firstname.lastname
```

要获得桌面的路径，我们需要在上面的字符串中添加“/Desktop”。现在，我们可以把两个字符串加在一起，但是一旦你开始把两个以上的字符串加在一起，记住在哪里放反斜线就变得复杂了。

为了避免硬编码反斜杠，我们使用了" **os.path.join()** "函数。这将我们提供给函数的路径合并成一个文件路径，而不必手动添加必要的反斜杠。我们提供的两个参数是“os.environ['Home']”，它将给出您的系统的主目录，以及字符串“Desktop”。

运行 os.path.join()函数将得到以下结果:

```
/Users/firstname.lastname/Desktop
```

接下来，我们需要用“ **os.chdir()** ”函数改变我们的工作目录，提供“桌面”变量，该变量包含我们机器的桌面路径。这将允许我们的脚本在桌面上搜索截图，我们现在就这样做。

## 在桌面上查找和重命名屏幕截图

首先，为了避免重复的文件名，我们将把“count”变量赋给 0。在下面的 for 循环中，我们将在每次找到并重命名屏幕截图时给这个变量加 1。

```
count = 0
```

现在，这里是 for 循环:

```
for original_file in os.listdir():
    if 'Screen Shot' in original_file:
        count += 1
        birth_time = os.stat(original_file).st_birthtime
        birth_time = datetime.fromtimestamp(birth_time)
        birth_time = birth_time.strftime("%d-%m-%Y")
        new_file = project + "_" + birth_time + "_" + str(count) + ".png"
        os.rename(original_file, new_file)
```

让我们把这个循环分解成几个部分。

" **os.listdir()** "输出给定目录中所有条目的文件名列表。默认情况下，它将在我们当前的工作目录中查找所有条目，我们之前将该目录指定为桌面。

例如，如果我们的桌面上有一个 Excel 文件和一个文本文件，os.listdir()会给出:

```
['samplefile.xlsx', 'samplefile2.txt']
```

for 循环的目的是遍历每个条目，看看它们的名称中是否有“Screen Shot”。这是因为在 Mac 上，截屏文件名的默认命名约定是总是“截屏”，然后是截屏拍摄的日期和时间。为此，我们只需检查“截图”是否在我们从“os.listdir()”中找到的文件名中，如下所示:

```
if 'Screen Shot' in original_file:
```

如果满足该条件，脚本将开始重命名该文件的过程。在 if 语句下的第一行，我们写“count+=1 ”,将前面初始化的计数器变量加 1。这将在以后添加到文件名中。

然后，为了获取截屏拍摄的日期，我们使用" **os.stat()** "来获取文件的一些元数据，并作为参数传递。

```
birth_time = os.stat(original_file).st_birthtime
```

这里，我们传入了“original_file”变量，该变量只包含符合“屏幕截图”条件的每个文件的名称。我们对“st_birthtime”结果特别感兴趣，它返回文件创建时的时间戳。

为了将时间戳转换成更容易阅读的文件名，我们将使用“**fromtimestamp()**”datetime 类方法将其转换成 datetime 对象。

```
birth_time = datetime.fromtimestamp(birth_time)
```

接下来，我们将把我们的 datetime 对象重新格式化为我们希望它打印在屏幕截图的文件名上的样子。为此，我们使用“ **strftime** ”或“来自时间的字符串”，其中我们获取一个 datetime 对象并解析它以获得我们需要的部分。在本例中，我希望将类似“2019 年 12 月 1 日”的日期显示为“2019 年 1 月 12 日”。为此，我们使用以下 strftime 运算符:

```
birth_time = birth_time.strftime("%d-%m-%Y")
```

现在我们有了文件名的所有组成部分。为了把它们放在一起，我们要用这条线把所有的变量结合起来:

```
new_file = project + "_" + birth_time + "_" + str(count) + ".png"
```

这里，我们有从一开始的输入中获得的“项目”标签，截图的“出生时间”格式的字符串，以及转换为字符串的“计数”变量。为了使名称看起来更好，我们还在文件名部分之间添加了下划线。然后，我们加上”。png”作为图像扩展名。

为了实际重命名文件，我们使用“ **os.rename()** ”，它有两个参数:原始文件名和替换文件名。

```
os.rename(original_file, new_file)
```

仅此而已！

这个循环将遍历我们桌面上的每个文件，并根据我们刚刚遍历的过程编辑每个名称中带有“Screen Shot”的文件。

# 使 Python 文件可执行

我们可以从我们的终端运行该脚本，但是要让我们只需单击桌面上的一个文件来重命名我们的屏幕截图，只需遵循以下步骤:

1.  **把这个添加到你的 Python 脚本的第一行:**

```
#!/usr/bin/env python3
```

这被称为“shebang ”,它允许执行 python 脚本，而不必在终端的文件名前键入“Python”。

**2。将 Python 脚本的文件扩展名更改为“”。命令”。**

在我的例子中，Python 文件被称为“screenshots.py”，所以我将其重命名为“screenshots.command”。

**3。在终端上，运行下面两行来创建您的"。命令“文件可执行:**

```
cd Desktop
chmod +x filename.command
```

这假定您已经放置了您的"。命令”文件放在桌面上。如果您将它存储在不同的文件夹中，请将该文件夹的文件路径放在“cd”之后。

现在，您只需点击文件，您的 Python 脚本就会运行！

完整的代码可以在这里看到:

屏幕截图重命名脚本

我发现这是一种快速简单的方法，可以练习使用 os 模块来检查系统上的文件，同时集成 datetime 来定制屏幕截图名称。有很多方法可以进一步缩短这个脚本，比如删除第 7 行和第 8 行，将桌面的路径直接传递给 os.listdir()。为了清楚起见，我按照本文中的解释保留了它，但是当您在自己的任务中使用它时，可以随意调整脚本。

您也可以使用相同的步骤模板来转动一个”。py“将文件转换成可执行文件”。command”文件，每当你需要一个可点击的脚本。

祝你截图好运！