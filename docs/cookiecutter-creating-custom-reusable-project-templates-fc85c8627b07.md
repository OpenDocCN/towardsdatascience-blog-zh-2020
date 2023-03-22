# Cookiecutter 创建自定义可重用项目模板

> 原文：<https://towardsdatascience.com/cookiecutter-creating-custom-reusable-project-templates-fc85c8627b07?source=collection_archive---------14----------------------->

## 一个简短的教程，学习如何修改现有的 cookiecutter 模板或从头创建自己的模板。

![](img/164f4374a30618f3d01098d394e82feb.png)

在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上由 [Neven Krcmarek](https://unsplash.com/@nevenkrcmarek?utm_source=medium&utm_medium=referral) 拍摄的照片

你过去和未来的项目可能在某些方面都是独一无二的。也就是说，如果你是(并且你应该是)适度组织的，所有的底层结构在项目之间不会有太大的变化:一个文件夹用于原始数据，另一个用于处理过的数据；一个存放源代码的文件夹，一个存放报告的文件夹等等。拥有一个共享的结构不仅在短期内有助于找到和使用与项目相关的所有资源，而且从长远来看，有助于各个项目之间的一致性和可复制性。

如果做得好的话，良好的项目结构可以节省时间，但是如果你刚开始一个新项目的时候，还在创建自述文件和。gitignore 的而不是导入 matplotlib。人们总是可以简单地复制一个旧的项目，删除/修改旧的文件和文件夹，但这并不会比从头开始更有趣或更快。尤其是如果你必须改变每个文档中的项目标题，每个文件的每个联系人部分的作者姓名等等。这个问题真正的最佳解决方案是使用灵活的模板工具。事实上，有太多的模板工具，然而，以其简单、灵活和直观而著称的是 [cookiecutter](https://cookiecutter.readthedocs.io/en/1.7.0/README.html) 。

如果你还没有听说过它，或者你还没有花时间使用它来优化你的模板，在这篇文章中，我将向你展示如何快速开始使用现有的在线 cookiecutter 模板，如何定制它们，以及如何在没有代码或复杂设置的情况下从头开始制作你自己的模板。

# 安装 Cookiecutter

安装 cookiecutter 就像在 python 中安装任何其他包一样简单:

```
pip *install* cookiecutter
```

或者，如果您使用的是 conda:

```
conda *install* -c conda-forge cookiecutter
```

我个人更喜欢拥有一个包含 cookiecutter(以及其他常用/快速使用的工具)的通用 conda 环境，然后为特定的项目依赖项创建一个新环境。但我把这部分留给你。

# 使用预定义的 Cookiecutter 模板

与开源天堂中的许多资源一样，您可以通过使用您喜欢的 *maaaaaany* 公共模板之一来开始使用 cookiecutter 模板。

我特别喜欢的一个是[cookiecutter-data-science](https://github.com/drivendata/cookiecutter-data-science)模板。我强烈推荐你访问这个链接，看看整个模板结构。它不仅是一个很好的文件目录树，还可以帮助您组织一般数据相关项目的概念流程。如果需要，我们将在下一节学习如何定制它。如果你想知道对于不同类型的项目还有什么其他的模板(Django 模板，数据新闻模板等等。)你应该看看[官方文件](https://cookiecutter.readthedocs.io/en/1.7.0/README.html)上的详细列表。

不过现在，我们将使用*烹饪数据科学*。假设您希望将项目保存在文件夹 **AllProjects** 中，通常您会将所有项目保存在该文件夹中。只需进入该目录，在命令/Anaconda 提示符下激活您虚拟/conda 环境(如果您还没有进入该目录),然后键入:

```
cookiecutter <repo url>
```

如果你确实在使用*cookiecutter-data-science*，这就变成了:

```
cookiecutter https://github.com/drivendata/cookiecutter-data-science
```

或者，如果你想保存一些字符，你可以使用 GitHub 地址的快捷键:

```
cookiecutter gh:drivendata/cookiecutter-data-science
```

如果是第一次使用，它会先把模板下载到本地。一旦完成，cookiecutter 动作就开始了。根据您选择的模板，系统会提示您回答不同的问题，例如项目名称是什么、作者姓名、项目描述、许可证等。这些都是您可以采用的不同定制选项(或者简单地接受默认选项)。填写所需答案或从提供的选项中选择(默认值出现在方括号中)。

我将假设(如果您被询问项目的名称，这通常是应该的)您选择了名称 **ElProject** 作为项目的根目录。完成所有问题后，您现在应该会在 **AllProjects** 目录中看到新的项目文件夹，其中包含模板提供的所有内容:

```
AllProjects
└── ElProject
    └── <all the template's subfolders and files>
```

就这么简单！

根据模板的不同，您可能会看到它不仅包括一个好的目录树，还包括一些有用的文件，如。gitignore(从版本控制中排除文件)，一个预先编写的自述文件，安装和需求文件以及更多好东西。

如果你想开始一个新的项目，比如说 ***超级机密项目*** ，再次运行命令，在你的 ***AllProjects*** 目录下，回答提示，你应该会看到目录下添加的项目文件夹:

```
AllProjects
└── ElProject
    └── <all the project's subfolders and files>
└── super secret project
    └── <all the project's subfolders and files>
```

如果您需要任何额外的定制，您可以随时添加/删除/更改项目目录中的任何内容。但是，如果您一次又一次地进行相同的定制，那会怎么样呢？也许您更喜欢为您所做的每个项目创建 LaTex 报告，或者 PowerPoint 演示文稿，并且您厌倦了创建文件，从以前的项目中复制相同的结构，然后使用当前项目的信息来纠正它们。如果这是您的情况，那么请继续阅读下一节，我将介绍如何创建定制模板。

# 创建和自定义模板

# 从头开始创建模板

创建您自己的 cookiecutter 模板的第一步是用您希望我们的模板拥有的名称创建一个文件夹。您应该始终选择一个易于记忆并且能够描述使用该模板的项目类型的名称。不过对于本教程，我们将做完全相反的事情，并将我们的新模板命名为 ***dasTemplate*** 。

这个文件夹可以放在你电脑的任何地方，但是我建议你把所有的模板放在一起，比如放在文件夹 ***AllTemplates*** (如果你把所有的模板保存在一个容易访问的远程存储库中，比如*Github，你会得到额外的分数)。现在，我们有以下目录:*

```
AllTemplates
└── dasTemplate
```

现在，进入新的空文件夹 ***dasTemplate*** 开始创建你的模板。

我们希望在模板中重新创建的最基本的功能是能够创建一个以我们的项目命名的空文件夹。为此，into***das template***添加下面两个完全相同的内容(包括花括号):

1.  答。json 文件名为***cooki cutter . JSON****(如果第一次，只需创建一个文本文件，但另存为。json 而不是。txt)*
2.  名为***{ { cookiecutter . project _ name } }***的文件夹

因此我们的模板目录现在看起来像这样:

```
AllTemplates
└── dasTemplate
     ├── cookiecutter.json
     └── {{cookiecutter.project_name}}
```

在使用模板之前，您需要打开 json 文件，并在其中编写以下内容:

```
{
"project_name": "default_name"
}
```

在解释它们之前，我们可以看看使用这个模板会给我们带来什么。

为此，转到您希望创建项目的位置(例如目录 **AllProjects** )，并传递与我们使用*cookiecutter-data-science*模板时相同的命令。然而，这一次不是提供存储库的 url，而是简单地传递模板的路径(除非您也想从 GitHub 使用模板，然后传递存储库的 url…):

```
cookiecutter path/to/template
```

例如(如果你像我一样运行 windows):

```
cookiecutter C:\Users\USer1\Documents\AllTemplates\dasTemplate
```

运行此命令时，您将得到以下提示:

```
project_name [default_name]:
```

你可能会认出这是我们在。json 文件。现在，只需输入你的项目名称*，例如* ***ElProject*** 。之后，您应该会看到新的项目文件夹被添加到目录中:

```
AllProjects
└── ElProject
```

那么，我们在这里做了什么？这个问题的答案最好一分为二。首先，可以将 cookiecutter.json 看作是我们希望用户在创建新模板时收到的提示文档。正如我暗示的那样。json pair 只是在您没有对提示提供任何答案时使用的默认值。反过来，可以在模板中的任何位置使用这些参数，语法为{{cookiecutter.parameter}}。从这个意义上来说，当我们创建文件夹{{cookiecutter.project_name}}时，我们告诉 cookiecutter 寻找提示 *project_name* 的答案，并在创建新的单个项目时使用它作为文件夹的名称。

这意味着当我早些时候告诉你你必须完美地复制基本模板时，这并不完全正确。在。json 文件，您实际上可以编写如下内容:

```
{
"bestCheese": "Cambozola"
}
```

和模板结构:

```
AllTemplates
└── dasTemplate
     ├── cookiecutter.json
     └── {{cookiecutter.bestCheese}}
```

只要您在模板中用双花括号(*例如* {{cookiecutter.parameter}})写的内容与 json 文件中的参数匹配，那么 cookiecutter 就会为您完成剩下的工作。

# 添加到模板

我们现在可以通过选择我们希望模板在项目的文件夹中重新创建的文件来开始慢慢增加我们的模板。在 cookiecutter 中这样做是非常直观的:

> *您放置在模板中的任何文件和文件夹将按原样出现在使用该模板启动的单个项目中。不仅文件和文件夹会在那里，而且它们的内容也会在那里。也就是说，任何写在里面的东西，每一个图像，所有的东西！*

假设您希望您的项目从根目录下的 *README.md* 开始，并且可能有一个 *aboutme.txt* 文件用于简要描述您和一些联系信息，以及一个 *reports* 文件夹，其中有一个 LaTex 报告的模板:

```
AllTemplates
└── dasTemplate
     ├── cookiecutter.json
     └── {{cookiecutter.project_name}}
          ├── README.md
          ├── aboutme.txt
          └── reports
              └── monthlyReport.tex
```

在您希望创建项目的目录中运行命令，您应该看到:

```
AllProjects
└── ElProject
    ├── README.md
    ├── aboutme.txt
    └── reports
        └── monthlyReport.tex
```

超级容易，但是有点无聊。让我们通过为文件名及其内容使用一些新的可变参数来使它更加个性化。让我们要求将项目的简短描述添加到不同项目的 README.md 中，aboutme.txt 以单个项目的作者命名，并显示项目的名称和描述。tex 文件。我们将首先修改。json 文件来请求所有这些新信息:

```
{
"project_name": "default_name",
"description": "short description",
"author": "Dr. Myfull Name",
"email": "reachme@mail.com"
}
```

现在让我们用花括号来修改文件和文件夹。新模板目录应该如下所示:

```
AllTemplates
└── dasTemplate
     ├── cookiecutter.json
     └── {{cookiecutter.project_name}}
          ├── README.md
          ├── {{cookiecutter.author}}.txt
          └── reports
              └── monthlyReport.tex
```

README.md 可能如下所示:

```
# {{cookiecutter.project_name}}
This is a great README file by {{cookiecutter.author}} ({{cookiecutter.email}})

The project is about:
{{cookiecutter.description}}
```

对于 latex，语法可能会有一些变化(这就是我选择它作为例子的原因)，因为 LaTex 在自己的语法中使用了花括号。请注意下面的月度报告示例:

```
\documentclass{article}%%%%% Packages %%%%%
\usepackage{graphicx}
\usepackage{glossaries}%%%%% Graphics %%%%%
{% raw %}
\graphicspath{{../figures/}}
{% endraw %}%%%%% Glossary %%%%%
\makeglossaries
\loadglsentries{glossary}%%%%% Project details %%%%%
\title{ {{cookiecutter.project_name}} }
\author{ {{cookiecutter.author_name}} }
\date{\today}
```

我想让你注意的第一件事是*项目细节*部分。对于标题和作者，双花括号与包围它们的花括号之间用空格隔开。这不是纯粹的审美。在某些情况下，如果你写的时候没有空格，cookiecutter 将不能正确地创建你的文件夹。因此，我建议当您遇到这种情况时，无论您是在 Latex 中工作还是在其他地方工作，都要一步一步地尝试您的模板。

第二个要注意的是*图形*部分。也许你已经问过自己“如果我需要在文件中使用双花括号怎么办？”。如果这是您的情况，并且您要正常进行，cookiecutter(或者更恰当地说，它所基于的语言)很可能会向您抛出一个错误，认为您正在尝试使用一个很可能不存在的变量或命令。您可以简单地通过用{% raw %}和标记{% endraw %}(一个用于打开，一个用于关闭)将它包围起来，从而避开这个问题。在 *raw-endraw* 之间的所有文本将出现在单个项目上，就像它在模板中输入的一样；花括号什么的。

这不是我们能用 cookiecutter 开发的唯一额外功能。我们将在下一节看到更多有用的命令。

# 利用 Jinja 语法增强你的模板

到目前为止，我们看到了如何用我称之为“花括号”的语法创建可复制的模板。这个语法实际上属于模板语言 [Jinja](https://jinja.palletsprojects.com/en/2.11.x/) ，它在幕后运行 cookicutter 程序。如果你和 Django 或者 [Liquid](https://shopify.github.io/liquid/) 一起工作过，你可能会熟悉这种语言的范围和应用。

一个人可以用 Jinja 做的所有事情的详细解释已经超出了这篇文章的范围。相反，我将只提到一个特别有用的基本技巧，它可以帮助提升你的模板。

假设，您通常在三个不同的团队中工作，并且您希望您的模板指定单个项目实际上属于哪个团队，以及每个团队的名称列表。您可以简单地在您的。json 调用“project_team”并在每次使用模板时填写团队名称。但是，您可能会拼错名称，这将导致意想不到的和/或恼人的错误。最重要的是，如果你每次都简单地写下团队名称，你将如何更新你的文件作为你答案的一个功能？

您可以做的是设置。json 提供了一个选项选择，而不是一个完全开放的字段，并使用良好的 ol' if 语句告诉模板根据您的答案打印哪个团队的列表。为此，首先修改。json 文件，只需简单地将所有选项添加到括号中通常会放置默认答案的位置(现在默认情况下，将采用括号中的第一个选项):

```
{
"project_name": "default_name",
"description": "short description",
"author": "Dr. Myfull Name",
"email": "reachme@mail.com",
"project_team": ["Great Team", "Team of Legends", "The Other Team"]
}
```

现在，让我们根据对提示的回答，将团队成员列表添加到 README.md 中:

```
# {{cookiecutter.project_name}}
This is a great README file by {{cookiecutter.author}} ({{cookiecutter.email}})

The project is about:
{{cookiecutter.description}}

## Team:
{% if cookiecutter.project_team == 'Great Team' %}
* Max E.Mumm
* Jack Pott

{% elif cookiecutter.project_team == 'Team of Legends' %}
* Armando E. Quito
* Al E.Gater

{% elif cookiecutter.project_team == 'The Other Team' %}
* May B.Dunn
* Justin Thyme
{% endif %}
```

如果您运行该命令来创建一个新项目，您应该会看到根据您所选择的团队，README.md 的团队成员列表会有所不同。例如，如果我与“伟大的团队”一起工作，项目文件夹中的自述文件将如下所示:

```
# ElProject
This is a great README file by Dr. Myfull Name (reachme@mail.com)

The project is about:
short description

## Team:

* Max E.Mumm
* Jack Pott
```

有了这个新增的功能，您现在应该能够创建非常灵活的模板，允许您使用相同的共享结构快速启动新项目，而无需更改所有的小细节。

如果您一直往下读，希望您现在已经对如何使用 cookiecutter 模板有了一个很好的想法。你现在可以使用预制的，根据你的需要定制或者创建你自己的。如果您希望我介绍更强大的 Jinja 命令来进一步提高您的工作效率，或者有任何评论/问题，请随时在社交媒体上留下评论或加入我。模板快乐，我现在要去吃些饼干了…

*原载于*[https://matic derini . github . io/blog/tutorial/2020/04/13/cookiecutter . html](https://maticalderini.github.io/blog/tutorial/2020/04/13/cookiecutter.html)*2020 年 4 月 13 日。*