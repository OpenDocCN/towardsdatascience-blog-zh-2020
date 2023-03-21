# 在不发布的情况下构建 Python 包

> 原文：<https://towardsdatascience.com/building-a-python-package-without-publishing-e2d36c4686cd?source=collection_archive---------13----------------------->

## 轻松访问和组织您的 Python 模块

![](img/2934c5ab5667ed928913f5499cdea126.png)

在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上 [Leone Venter](https://unsplash.com/@fempreneurstyledstock?utm_source=medium&utm_medium=referral) 拍摄的照片

python 中的包允许无缝分发 Python 模块。当我们使用 pip install 时，我们经常从 [PyPI](https://pypi.org/) 下载一个公开可用的包。本地包对于代码组织和重用也很有用，允许您简单地导入一个模块，而不必导航到它的目录或重写代码。在本次演示中，我们将构建和访问一个基本的 Python 包。

让我们创建两个简单的模块并将它们打包。然后，我们将使用这个包编写一个简单的应用程序，提示用户从他们的计算机中选择一个. jpg 图像进行灰度化。

# 编写您的模块

*ui.py*

```
#ui.pyfrom tkinter import filedialog
from tkinter import Tkdef get_image_file():
    Tk().withdraw()
    filename =  filedialog.askopenfilename(title = "Select file",filetypes = [("jpeg files","*.jpg")])
    return filename
```

*image_edits.py*

```
#image_edits.pyfrom cv2 import imread, COLOR_RGB2GRAY, cvtColor, imwrite
import osdef write_grayscale_image(filepath):
    original_image = imread(filepath)
    grayed_image = cvtColor(original_image, COLOR_RGB2GRAY)
    grayed_filename=os.path.join(os.path.split(filepath)[0],'grayed_'+os.path.split(filepath)[1])
    print(grayed_filename)
    imwrite(grayed_filename, grayed_image) #export grayscaled image
    return grayed_filename
```

# 初始化和设置文件

创建一个空白的 *__init__。py* 文件，Python 将使用它来识别这个包。

```
$ touch __init__.py
```

此时，我们的文件结构应该如下所示:

```
dir/
    image_pkg/
              ui.py
              image_edits.py
              __init__.py
```

接下来，在您的包目录外创建一个 *setup.py* 文件

*setup.py*

```
import setuptoolssetuptools.setup(name='examplepackage',
version='0.1',
description='An example package',
url='#',
author='max',
install_requires=['opencv-python'],
author_email='',
packages=setuptools.find_packages(),
zip_safe=False)
```

注意，python 标准库中没有包含的包应该包含在 *install_requires* 中。我们的文件结构现在应该看起来像这样:

```
dir/
    image_pkg/
              ui.py
              image_edits.py
              __init__.py
    setup.py
```

# 构建并安装您的软件包

如果您正在使用[虚拟环境](https://docs.python.org/3/tutorial/venv.html)(Python 开发的一般良好实践)，创建并激活您的环境。

```
$ python3 -m venv myenv
$ source myenv/bin/activate
```

安装车轮。

```
$ pip install wheel
```

将软件包安装到您的环境中。

```
$ pip install .
```

我们的包已经创建好了，现在我们可以在任何地方使用它。让我们创建一个简单的应用程序，它包含了我们的包内容。

*main.py*

```
#main.pyfrom image_pkg.ui import get_image_file
from image_pkg.image_edits import write_grayscale_imagewrite_grayscale_image(get_image_file())
```

我们现在已经构建并实现了一个基本的 python 包！关于软件包分发、许可和安装的更多信息可以在[文档](https://packaging.python.org/tutorials/packaging-projects/#)中找到。

## 其他 Python 教程:

[](/solving-mazes-with-python-f7a412f2493f) [## 用 Python 解迷宫

### 使用 Dijkstra 的算法和 OpenCV

towardsdatascience.com](/solving-mazes-with-python-f7a412f2493f) [](/building-a-simple-ui-for-python-fd0e5f2a2d8b) [## 为 Python 构建一个简单的 UI

### Streamlit:一个基于浏览器的 Python UI，不需要 HTML/CSS/JS

towardsdatascience.co](/building-a-simple-ui-for-python-fd0e5f2a2d8b)