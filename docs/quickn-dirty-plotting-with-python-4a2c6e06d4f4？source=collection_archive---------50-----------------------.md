# 用 Python 快速而不下流地绘图

> 原文：<https://towardsdatascience.com/quickn-dirty-plotting-with-python-4a2c6e06d4f4?source=collection_archive---------50----------------------->

![](img/10f44a3b75597f981f3897b69748c5e5.png)

照片(背景)由[米切尔罗](https://unsplash.com/@mitchel3uo?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)在 [Unsplash](https://unsplash.com/?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)

使用 Excel 最常见的借口之一是什么:“它有利于快速' n '肮脏的阴谋”。如果您对 Excel 可视化数据感到满意，那就没问题。然而，这篇文章是关于用 Python 绘制数据的一种更快更脏的方法:)

绘制 x 与 y 数据的简单代码如下:

![](img/af9112450e2132fe66c699a9eb87cced.png)

这个片段中的数据文件是一个两列文本文件。

这相对简单:我们导入相关模块，加载数据并绘制它。当我们定制细节时，代码的行数会增加。但现在这不重要，因为这是关于快速' n '肮脏！虽然上面的代码并不复杂，但是许多人会发现每次都要输入代码是一件痛苦的事情。

将来你可以做两件事来节省时间和精力:

*   保留一份拷贝。将文件复制到某个特殊的地方，并在每次需要的时候复制它，这样你只需要改变文件名。
*   制作你自己的绘图库，这样你就可以为特定的数据文件提供绘图功能。这将需要一些时间和经验来建立，但从长远来看会使事情更有效率。

这两种方法中的第二种更适合快速绘制，但是你们中的一些人仍然会觉得为每个新的绘图输入任何东西很烦人。然而，还有[的另一种](https://stackoverflow.com/questions/8570288/run-python-script-on-selected-file) [方法](https://stackoverflow.com/questions/9316907/right-click-command-pass-argument-to-python-script)，它需要以下步骤(在 Windows 机器上):

*   创建一个包含以下代码的 python 文件，并将其保存在您喜欢的任何地方(但最好是容易记住的地方)。

![](img/9ee68ad093e695531c1f07394e89f09f.png)

找出与前面代码的不同之处

*   在包含以下代码的同一文件夹中创建一个. cmd 文件。确保编辑步骤 1 中 python 文件的路径(橙色)。

![](img/2a320a979a515f771a6923c3a50530a1.png)

*   创建一个快捷方式。cmd 文件。右键单击快捷方式，选择属性，并在运行选项中选择最小化([这将在每次运行](https://www.winhelponline.com/blog/run-bat-files-invisibly-without-displaying-command-prompt/)时最小化终端)。
*   打开一个文件资源管理器窗口，输入“shell:sendto”或只输入“sendto”。通过快捷方式复制。cmd 文件复制到“SendTo”文件夹。

全部完成！现在，当您右键单击数据文件时，可以将鼠标悬停在上下文菜单中的“发送到”上，然后单击。cmd 文件。在这个例子中，数据文件只有两列 x，y 数据。您可能需要对忽略文本进行一些调整，例如[skip prows 参数](https://numpy.org/doc/stable/reference/generated/numpy.loadtxt.html)。

现在，您有了一个非常好的快速而不麻烦的方法来可视化数据。使用 matplotlib 绘制它允许您放大特定区域进行检查，并且您可以快速保存一个看起来不错的图以供共享。您可以通过执行简单的“for 循环”来编辑用于绘制多个文件的代码。从这里，您可以随心所欲地绘制带有图例的多个轴，以跟踪您打开了哪些文件。

![](img/09ddba390af072e43705610a1b48c199.png)

您可以针对特定的数据文件格式对代码进行进一步的调整。熊猫图书馆非常适合加载和检查。csv 文件。您还可以在“发送到”文件夹中为不同的文件类型或绘图方法设置一些不同的选项。

现在，在这一点上，你可能会想，我们正在通过快速' n '脏，进入缓慢' n '干净…但你需要记住，你只需要创建这些代码一次。在这之后，一切都是点击！像这样的工具一开始可能需要一点时间来制作，但是从长远来看，它们将会为您节省很多时间，并使可视化数据变得更加容易和灵活。尽情享受吧！