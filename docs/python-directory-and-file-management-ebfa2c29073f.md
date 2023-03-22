# Python 目录和文件管理

> 原文：<https://towardsdatascience.com/python-directory-and-file-management-ebfa2c29073f?source=collection_archive---------36----------------------->

## 了解如何用 Python 处理目录和文件操作

![](img/99168ff94dbbaefd38397a2b7118b4a8.png)

伊利亚·巴甫洛夫在 [Unsplash](https://unsplash.com/s/photos/computer-files?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) 上的照片

***在这篇文章中，你将学习到用 Python 处理文件和目录的各种方法，比如***

*   *创建、更改、删除和重命名目录*
*   *获取当前目录或列出文件，同时统计目录中的文件数量*
*   *读取和提取 zip 文件*
*   *复制文件并将文件从一个目录移动到另一个目录*

***为什么要学习目录和文件管理？***

*   在机器学习和深度学习中，您将处理作为数据集一部分存储在目录中的大量文件，或者您将在文件中存储模型的训练权重。因此，学习文件和目录管理将会很有帮助
*   目录和文件管理还将有助于在一致的目录结构中组织训练、测试和验证数据集，用于所有类型的数据，如数字数据、图像数据或文本数据。

我们将导入所需的库 ***os*** 和 ***shutil*** ，它们将提供处理操作系统相关功能的方法。

## 创建目录:mkdir(路径)

要创建一个目录，我们将使用 ***mkdir()*** ，我们需要向其传递要创建的目录的路径。

如果目录已经存在，则引发' ***文件存在错误'***

```
***import os
try:
    os.mkdir('c:\\test_1')
except FileExistsError:
    print("Directory already exists")***
```

如果目录 test_1 不存在，那么将创建一个新的目录 test_1。如果目录 test_1 已经存在，则出现错误，'***filexistserror '***

## 重命名文件或目录:重命名(源，目标)

要重命名文件或目录，我们需要指定源，即您要重命名的文件，以及目标，即该目录下文件的新名称。

如果源文件或目录 *test_1* 不存在，那么我们得到一个“***FileNotFoundError***”错误

如果目标 *test_2* 目录已经存在，那么我们得到“***filexistserror***”错误

```
***try:
    os.rename('c:\\test_1\\', 'c:\\test_2')
except :
    print(sys.exc_info())***
```

## 删除目录:rmdir(路径)

删除指定为参数的目录。

如果要删除的目录不存在，那么会得到一个'***FileNotFoundError***'错误。

如果该目录包含文件，并且您正试图删除该目录，那么您将得到一个“ ***OSError*** ”消息“*该目录不是空的”。*’

```
***try:
    os.rmdir('c:\test_2\\abc')
except:
    print(sys.exc_info())***
```

## 更改目录:chdir(路径)

将当前工作目录更改为指定路径。

如果目录 test_1 不存在，那么你会得到一个' ***FileNotFoundError '*** 如果指定文件名而不是目录，则会出现“**OSError”**消息“*文件名、目录名或卷标语法不正确。*'

```
***try:
    os.chdir('c:\\test_1')
except:
     print(sys.exc_info())***
```

## 打印当前工作目录:getcwd()

***os.getcwd()*** 以字符串形式返回当前工作目录

```
**print(os.getcwd())**
```

## 列出目录中的文件:listdir(路径)

列出指定目录路径中的文件。文件列表在列表中以任意顺序返回。

如果没有指定路径，那么 listdir()返回当前工作目录下的文件列表

```
***os.listdir('c:\\test_2\\test')***
```

![](img/65e7bc8435bce4a34ed424ac2dda3cd1.png)

目录***' c:\ test _ 2 \ test '***中的文件列表

计算目录中文件的数量

```
***len(os.listdir('c:\\test_2\\test'))***
```

您将得到输出 4，因为我们在如上所示的目录中有四个文件

## 从 zip 文件中读取和提取文件

您将接收或下载压缩在 zip 文件中的数据，因此知道如何从 zip 文件中读取和提取所有文件将是一个非常有用的工具。

你需要导入 ***zipfile*** 库。

我有从 [Kaggle](https://www.kaggle.com/c/dogs-vs-cats) 下载的狗和猫的数据集，放在一个 zip 文件中。读取 zip 文件，然后使用 ***extractall*** ()方法提取指定的目标文件夹中的所有数据

```
***import zipfile
zipfile_path='c:\\Datasets\\CV\\dogs-vs-cats.zip'******zip_f= zipfile.ZipFile(zipfile_path, 'r')
zip_f.extractall('c:\\Datasets\\CV\\dogs-vs-cats')******zip_f.close()***
```

## 复制文件:复制文件(源，目标)

使用 shutil copyfile()方法将文件从源文件复制到目标文件。目的地应该是完整的目标文件名。

下面的代码一次复制一个文件，从 ***base_dir*** 到 ***dest_dir*** 。我们用***OS . path . join()***来加入目录和文件名。

```
***from  shutil import copyfile***
***try:
    base_dir='c:\\Datasets\\CV\\dogs-vs-cats\\test1'
    dest_dir='c:\\test_2\\'
    for file in os.listdir(base_dir):
        copyfile(os.path.join(base_dir, file), os.path.join(dest_dir, file))
except:
    print(sys.exc_info())***
```

## 移动文件:移动(源，目标)

shutil 库中的 Move()方法可用于移动文件。在源参数和目标参数中都包含文件名。

```
***import  shutil******base_file='c:\\Datasets\\CV\\dogs-vs-cats\\test1\\1.jpg'
dest_file='c:\\test_2\\test\\1.jpg'
shutil.move(base_file, dest_file)***
```

如果目标文件名被更改，文件将被重命名和移动

```
***import  shutil******base_file='c:\\Datasets\\CV\\dogs-vs-cats\\test1\\100.jpg'
dest_file='c:\\test_2\\test\\dog_2.jpg'
shutil.move(base_file, dest_file)***
```

## 结论:

库 ***os*** 和 ***shutil*** 用于目录和文件管理，帮助在一致的目录结构中组织文件，以便使用 ML/DL 算法训练、测试和验证数据集

## 参考资料:

 [## os —其他操作系统接口— Python 3.8.3rc1 文档

### 该模块提供了一种使用操作系统相关功能的可移植方式。如果你只是想阅读或…

docs.python.org](https://docs.python.org/3/library/os.html) 

[https://docs.python.org/3/library/shutil.html](https://docs.python.org/3/library/shutil.html)