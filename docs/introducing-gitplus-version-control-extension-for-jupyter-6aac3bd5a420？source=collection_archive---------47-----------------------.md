# GitPlus 简介:Jupyter 的版本控制扩展

> 原文：<https://towardsdatascience.com/introducing-gitplus-version-control-extension-for-jupyter-6aac3bd5a420?source=collection_archive---------47----------------------->

![](img/75b5a3a407296a406e70304b62e8f948.png)

[图片与 undraw.io 的开放许可证一起使用]

没有简单的方法可以从 Jupyter UI 控制笔记本的版本。当然，你可以使用命令行& [来学习一些 git 命令](https://blog.reviewnb.com/github-jupyter-notebook/)来控制你的笔记本版本。但是并不是每个使用 Jupyter 的人都精通 git。因此我构建了 [GitPlus](https://github.com/ReviewNB/jupyterlab-gitplus) ，一个 JupyterLab 扩展，它提供了直接从 JupyterLab UI 提交笔记本&创建 GitHub pull 请求的能力。

# 如何控制 Jupyter 笔记本的版本

安装 GitPlus 扩展时，它在 JupyterLab UI 中提供了一个新的菜单项`Git-Plus`。从那里，您可以提交笔记本文件或创建 GitHub pull 请求，如下面的演示视频所示。

**从 Jupyter 创建 GitHub Pull 请求**

**从 Jupyter 创建&推送 GitHub 提交**

# 它是如何工作的？

*   扩展的[服务器组件](https://pypi.org/project/jupyterlab-gitplus/0.1.13/)使用[git python](https://github.com/gitpython-developers/GitPython)&GitHub API 来推送提交和创建拉取请求。
*   扩展的客户端组件[提供 UI 并调用服务器扩展上的适当端点。](https://www.npmjs.com/package/@reviewnb/jupyterlab_gitplus)
*   客户端组件查看 JupyterLab 中所有打开的文件，并确定哪些文件在 GitHub 存储库中。
*   它允许您选择要向其推送提交或创建拉取请求的存储库。
*   它捕获要提交的文件列表、提交消息，并将更改作为提交推送到远程存储库。
*   在 pull 请求的情况下，它会创建一个新的分支并将更改推送到那里。这个新创建的分支与您的存储库的默认分支(通常是`master`)进行比较。
*   在分叉存储库的情况下，在父存储库上创建拉请求。

# 装置

以下是[安装](https://github.com/ReviewNB/jupyterlab-gitplus#install)说明。

# 限制

截至目前，GitPlus 只与 JupyterLab 合作。如果您有兴趣将其与经典 Jupyter UI 配合使用，请投票支持此[功能请求](https://github.com/ReviewNB/jupyterlab-gitplus/issues/2)。只有当有足够的兴趣时，我才愿意研究经典的 Jupyter 支持。

# 路标

将来 GitPlus 将能够，

*   从 GitHub 提取更改
*   在本地切换/创建 git 分支
*   解决笔记本合并冲突(不影响底层 JSON)

# 结论

如果您是 Git 的新手，可能需要一些时间来适应所有的命令。即使您对 git 很熟悉，notebook JSON 也会产生意想不到的副作用。GitPlus 提供了易于使用的接口，可以从 JupyterLab UI 完成大多数常见的 git 任务。可以结合 [ReviewNB](https://www.reviewnb.com?utm_source=reviewnb_blog) 看视觉差异&写笔记修改意见。

黑客快乐！

*原载于 2020 年 4 月 11 日*[*【https://blog.reviewnb.com】*](https://blog.reviewnb.com/introducing-gitplus/)*。*