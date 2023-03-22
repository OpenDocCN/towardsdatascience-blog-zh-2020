# 用诗歌解决 Python 中的依赖管理

> 原文：<https://towardsdatascience.com/solving-dependency-management-in-python-with-poetry-165b92069e9d?source=collection_archive---------12----------------------->

## [现实世界中的数据科学](https://towardsdatascience.com/data-science-in-the-real-world/home)

## 为我们的 Python 工作流带来简单性和一致性

![](img/38a4cf36a7b17a3c594fcc6e0af9c87e.png)

一列火车拉着它在加利福尼亚州埃默里维尔的所有附属物

我的团队最近选择向我们的技术堆栈添加另一个工具，[poem](https://python-poetry.org/)。与每一个技术决策一样，这个决策是经过仔细考虑的，以避免不必要的复杂性。那么我们为什么决定用诗歌呢？

来自[一个建设网站的世界](https://medium.com/bbc-design-engineering/server-timing-in-the-wild-bfb34816322e)，我已经习惯了 [npm](https://www.npmjs.com/) 。有了 npm，JavaScript 世界将依赖性管理视为一个已解决的问题。在我们的 Python 项目中，我们只是使用 pip，我们可以遵循两个基本的工作流程:

*   手动管理顶层所需的依赖项
*   使用`pip freeze`收集所有的依赖项

# 定义你需要什么

在这种方法中，您创建一个需求文件，看起来像:

```
pandas==1.0.1
```

这样做的好处是更容易理解，因为你只是直接使用熊猫。然后使用 pip 安装这些依赖项及其依赖项:

```
pip install -r requirements.txt
```

这意味着您可以为任何开发依赖项创建一个单独的文件:

```
pip install -r dev_requirements.txt
```

这允许您将生产需求从开发需求中分离出来。仅安装您的生产需求有助于减少生产构建的规模。减少生产中的软件包数量还可以通过减少攻击面来提高安全性。

不幸的是，您必须维护这个文件，并且您对依赖关系没有什么控制权。每次运行`pip install`时，您的依赖项都有被安装不同版本的风险。

# 冻结所有依赖项

另一种方法是使用 pip 的更多功能来冻结您的依赖项。运行`pip install`，然后运行`pip freeze`，会给你一个所有已安装软件包的列表。例如，运行`pip install pandas`将导致`pip freeze`输出:

```
numpy==1.18.1
pandas==1.0.1
python-dateutil==2.8.1
pytz==2019.3
six==1.14.0
```

这很好，因为它可以让您将依赖项锁定到特定的版本，从而创建一个可重复的环境。不幸的是，如果你在 pip 运行的环境中安装了任何东西，它都会被添加到这个列表中。例如，您决定安装 numba，看看它是否能加快您的处理速度。这会导致以下冻结的依赖关系:

```
llvmlite==0.31.0
numba==0.48.0
numpy==1.18.1
pandas==1.0.1
python-dateutil==2.8.1
pytz==2019.3
six==1.14.0
```

但遗憾的是，numba 并没有你想要的性能提升。当您运行`pip uninstall numba`时，它确实会删除 numba，但不一定会删除 numba 安装的所有依赖项:

```
llvmlite==0.31.0
numpy==1.18.1
pandas==1.0.1
python-dateutil==2.8.1
pytz==2019.3
six==1.14.0
```

注意到你现在有一个额外的`llvmlite`包，你可能永远也不会摆脱它了吗？

值得注意的是，有一些工具可以解决这个问题并消除所有的依赖性。但是还有一点需要考虑。如果您安装了开发依赖项，它们也会导致这个问题。

# 两全其美

诗歌可以通过简单的界面做到以上两点。当您调用`poetry add`时，它会将包添加到一个`pyproject.toml`文件中，以跟踪顶层依赖关系(包括 Python 本身):

```
[tool.poetry.dependencies]
python = "^3.7"
pandas = "^1.0.1"
```

这与一个包含所有已安装软件包的`poetry.lock`文件成对出现，锁定到一个特定的版本。在本文中嵌入锁文件会占用很多空间，因为默认情况下它包含了散列以确保包的完整性。下面是一个片段:

```
[[package]]
category = "main"
description = "Python 2 and 3 compatibility utilities"
name = "six"
optional = false
python-versions = ">=2.7, !=3.0.*, !=3.1.*, !=3.2.*"
version = "1.14.0"[metadata]
content-hash = "5889192b2c2bef6b6ceae7457fc90225ba0c38a80ecd15bbbbf5871f91a08825"
python-versions = "^3.7"[metadata.files]
six = [
{file = "six-1.14.0-py2.py3-none-any.whl", hash = "sha256:8f3cd2e254d8f793e7f3d6d9df77b92252b52637291d0f0da013c76ea2724b6c"},
{file = "six-1.14.0.tar.gz", hash = "sha256:236bdbdce46e6e6a3d61a337c0f8b763ca1e8717c03b369e87a7ec7ce1319c0a"},
]
```

为了用 numba 重复我们的实验，我们使用了`poetry add numba`和`poetry remove numba`。这些命令还从环境和依赖锁定文件中删除了`llvmlite`。通过移除依赖性和清理，poem 使我们能够尝试更多的软件包，以一种简单的方式清理我们的虚拟环境。

最后，诗歌拆分出生产依赖和开发依赖。如果我们想写一些测试，我们可以使用`poetry add -D pytest`，结果是:

```
[tool.poetry.dependencies]
python = "^3.7"
pandas = "^1.0.1"[tool.poetry.dev-dependencies]
pytest = "^5.3.5"
```

当创建产品包时，您可以使用`poetry install --no-dev`忽略任何用于开发的东西。

# 使用虚拟环境

去年[我的一位同事写了一篇很棒的文章，讲述了我们为什么在数据科学中使用虚拟环境](https://medium.com/sainsburys-data-analytics/the-case-for-switching-from-conda-to-virtual-environments-in-python-de3211d8f6f)。诗歌在这里也有一些补充，为你的项目自动管理它们。所以不用跑了:

```
python -m venv .venv
. .venv/bin/activate
super-command
```

你只需输入:

```
poetry run super-command
```

poems 为您的项目创建一个虚拟环境，它与您指定的 Python 版本相匹配。默认情况下，这保存在项目根目录之外，以免造成混乱。我的团队出于其他原因选择将虚拟环境放回项目目录中，原因很简单:

```
poetry config --local virtualenvs.in-project true
```

这在相当标准的`.venv`文件夹中创建了虚拟环境。这有点麻烦，它创建了另一个`poetry.toml`文件，而不是使用主`pyproject.toml`。

# 这种事以前肯定有人做过吧？

还有其他在诗歌之前就已经存在的工具可以满足这些要求。诗歌也利用了很多 Python 自己的增强提议，比如 [PEP-508](https://www.python.org/dev/peps/pep-0508/) 、 [PEP-517](https://www.python.org/dev/peps/pep-0517/) 和 [PEP-518](https://www.python.org/dev/peps/pep-0518/) 。这些 PEP 致力于使用`pyproject.toml`作为配置项目的主要位置。当考虑诗歌的寿命时，遵循 pep 给了我们对我们正在使用的特性的未来支持的信心。

希望这解释了为什么我对我的团队选择使用诗歌而不是之前的替代品充满信心。如果你正在寻找更详细的历史介绍和更具体的诗歌命令，我会推荐 Todd Birchard 的[Package Python Projects the right Way](https://hackingandslacking.com/package-python-projects-the-proper-way-with-poetry-ca1e94a1727b)。对于其他与诗歌结合的工具，请看 Simon Hawe 的[如何为数据科学或其他东西建立一个令人敬畏的 Python 环境](/how-to-setup-an-awesome-python-environment-for-data-science-or-anything-else-35d358cc95d5)。

工程师和数据科学家合作翻译项目，这是一个非常简单的转变。构建新项目也变得更加简单，使我们能够试验不同的包，并在投入生产时获得特定的确定性构建。