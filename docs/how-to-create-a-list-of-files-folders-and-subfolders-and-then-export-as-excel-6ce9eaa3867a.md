# 如何创建文件、文件夹和子文件夹的列表，然后导出为 Excel

> 原文：<https://towardsdatascience.com/how-to-create-a-list-of-files-folders-and-subfolders-and-then-export-as-excel-6ce9eaa3867a?source=collection_archive---------26----------------------->

## 一个生产力工具玛丽近藤你的文件夹

![](img/3e057c34cb98366715005448295550e3.png)

照片由 [Pranav Madhu](https://unsplash.com/@pranavmadhu?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) 在 [Unsplash](https://unsplash.com/s/photos/file-list?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) 上拍摄

> 你能为我们的项目文件夹创建一个文件、文件夹和子文件夹的列表吗？

这个请求看起来似乎很简单，但是你已经可以想象一旦这个列表的第一个版本完成，可能会出现的其他修改。

> 你还能列出文件类型并给文件添加超链接吗？或许还可以增加文件的大小？

根据文件夹的大小、文件数量及其嵌套程度，您可以选择手动完成此任务，因为添加新字段可能很简单。现在，让我们考虑另一个请求:

> 我重新整理了一些文件夹，重命名了一些文件，你能更新一下列表吗？

别担心，蟒蛇来救你了！

`os.walk()`是主函数，我们将使用它来创建一个脚本来遍历主文件夹并列出所有子文件夹和文件。python 文档的链接是[这里是](https://docs.python.org/3.8/library/os.html#os.walk)。下面是代码的主要部分:

```
df = pd.DataFrame(columns=['File','File Type',
                           'Folder Location','Link', 'Path'])for root, dir, files in os.walk(path):
    files = [f for f in files if not f.startswith('~') and f!='Thumbs.db']
    paths = [os.path.join(root, f) for f in files]
    exts = [os.path.splitext(f)[1][1:] for f in files]
    filetypes = [ext_desc(ext) for ext in exts]
    file_links = ['=HYPERLINK("{}","link")'.format(p) if len(p) < 256 else '' for p in paths]
    folders = [os.path.dirname(p) for p in paths]
    df1 = pd.DataFrame({'File': files,
                        'File Type': filetypes,
                        'Folder Location': folders,
                        'Link': file_links,
                        'Path': paths})
    df = df.append(df1)
```

完整的代码可在故事的结尾。

# 代码解释:

## 1.创建一个空的数据框架，其中所需的列按所需的顺序排列:

```
df = pd.DataFrame(columns=['File','File Type',
                           'Folder Location','Link', 'Path'])
```

## 2.启动`for`循环:

```
for root, dir, files in os.walk(path):
```

`path`是指我们感兴趣的主文件夹路径。`os.walk`返回一个元组`root`、`dir`和`files`。为此我们只需要`root`和`files`。

## 3.用列表理解来构建每个专栏

```
# 'Files' columns
files = [f for f in files if not f.startswith('~') and f!='Thumbs.db']
```

临时文件以波浪号`~`开始，包含这些可能没有意义，因此 list comprehension 有一个`if`条件来排除这样的文件。类似地，`Thumbs.db`只是一个缩略图文件，并被明确排除。

```
# 'Path' column
paths = [os.path.join(root, f) for f in files]
```

`paths`是指向每个子文件夹中特定文件的路径列表。

文档推荐使用`os.path.join`而不是字符串连接，这里有一个 stackoverflow [回答](https://stackoverflow.com/questions/13944387/why-use-os-path-join-over-string-concatenation)为什么推荐这样做。

```
# 'File Type' column
exts = [os.path.splitext(f)[1][1:].lower() for f in files]
filetypes = [ext_desc(ext) for ext in exts]
```

`exts`是通过使用`os.path.splitext()`形成的扩展名列表，它返回一个没有扩展名和扩展名*的*文件名元组。**

```
>>> os.path.split('some_filename.pdf')
('some_filename', '.pdf')
```

[编辑]以前，我通过右分裂`rsplit`文件名得到扩展名。`exts = [f.rsplit('.',1)[-1].lower() for f in files]`这不是一个好的做法。

`filetypes`是文件类型列表，`ext_desc`是将每个扩展名映射到适当名称的函数。

```
def ext_desc(ext):

    d_ext_desc = {'xlsx':'Microsoft Excel File',
                  'docx':'Microsoft Word Doc'}
    # additional extensions and descriptions can be added to d_ext_desc try:
        desc = d_ext_desc[ext]
    except KeyError:
        desc = ''     # Any file extensions not mapped will be empty.
    else:
        pass
    return desc
```

添加文件链接的方式略有不同。

```
file_links = ['=HYPERLINK("{}","link")'.format(p) if len(p) < 256 else '' for p in paths]
```

`=HYPERLINK()`是一个 Excel 公式，在给定链接位置(可以是网页或文件位置)时创建超链接。第二个参数`link`是可选的，用于缩短显示的文本。`if`条件`len(p)<256`是 Excel 中的一个限制，超过 255 个字符的链接无效。

```
# Folder column
folders = [os.path.dirname(p) for p in paths]
```

`folders`是一个文件夹列表，告诉我们每个文件的位置。这是通过使用`os.path.dirname()`方法获取路径的目录名获得的。

[已编辑]之前，我使用`folders = [p.rsplit('\\',1)[0] for path in paths]`来获取文件夹。同样，这不是一个好的做法。

## 4.附加到原始数据帧

```
df1 = pd.DataFrame({'File': files,
                    'File Type': filetypes,
                    'Folder Location': folders,
                    'Link': file_links,
                    'Path': paths})
df = df.append(df1)
```

由于每个列表理解都是一列，剩下的工作就是构建一个数据帧并附加到原始数据帧上。使用`df.to_excel('some_filename.xlsx')`可以将最终的数据帧写入 Excel

## 5.将其推广以备将来使用

*   如果文件夹包含大量文件，生成完整的文件索引需要时间，因此当记录数量超过 500 时，我在`for`循环中添加了一个关键字参数。这在添加新列或修改最终输出的外观时特别有用，因为您不会希望遍历成千上万个文件才意识到最后几行代码中存在输入错误。
*   在脚本中修改路径可能会很麻烦，因此我添加了一个`tkinter.filedialog`方法，如果没有提供路径，它会提示用户选择文件夹。