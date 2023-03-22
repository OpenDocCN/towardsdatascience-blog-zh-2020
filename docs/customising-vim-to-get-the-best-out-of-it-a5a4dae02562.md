# 定制 Vim 以充分利用它

> 原文：<https://towardsdatascience.com/customising-vim-to-get-the-best-out-of-it-a5a4dae02562?source=collection_archive---------14----------------------->

## 在`.vimrc`中设置默认值以提高效率

![](img/db825ce29567f2c06b4cccfd34a78a92.png)

照片由 [Marius Niveri](https://unsplash.com/@m4r1vs?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) 在 [Unsplash](https://unsplash.com/s/photos/code?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) 上拍摄

Vim 的命令是这样的，它的全部功能仅使用键盘就能实现。但是这并不容易。需要一段时间来适应。毕竟是各种程序员都信誓旦旦的编辑器。

作为刚接触 Vim 的程序员，我们经常在终端中偶然发现它，完全被新的编辑界面所迷惑，感觉完全不知所措，敲打键盘键以弄清楚如何管理 Vim。难怪“如何退出 Vim”一直是堆栈溢出过去最受欢迎的问题之一。

当开始使用 Vim 时，真正重要的是首先创造一个让人们使用 Vim 感到舒适的环境。如果没有配置任何默认设置，Vim 看起来一点也不吸引人，您也不太可能使用 Vim。这意味着您最终会错过这个极其强大的模态编辑器，这对程序员来说是一个巨大的损失。

Vim 是高度可定制的，它应该可以帮助你编辑；而不是相反。因此，让我们进入一些设置，让 Vim 有宾至如归的感觉。

*   `syntax on`:开启语法高亮显示。毫无疑问，需要打开这个设置。
*   `colorscheme desert`:该设置使用“沙漠”配色方案突出显示语法。我个人很喜欢。
    获取所有可用配色方案的列表:
    `ls -l /usr/share/vim/vim*/colors/`
    有很多选项可供选择！我在另一篇博客文章[这里](/vim-the-ubiquitous-text-editor-b4e09374df57)中更详细地讨论了语法突出显示(在标题 ***语法突出显示*** 下)。
*   `set showmatch`:当文本指示器在匹配的大括号上时，显示匹配的大括号
*   `set number`:显示行号
*   `set relativenumber`:在`number`和`relativenumber`都设置的情况下，当前行显示该行的实际编号，其上下的行相对于当前行进行编号。这有助于了解在行与行之间上下跳动的确切数字。
*   `set ignorecase`:该设置使搜索不区分大小写
*   `set smartcase`:该设置使搜索不区分大小写，但如果搜索包含任何大写字母，则区分大小写。
*   `set incsearch`:此设置允许在您键入时搜索，而不是只在您按回车键时搜索
*   这是一个非常烦人的设置，但是对于 Vim 新手来说非常有用(是的，我打开了这个设置)。它不鼓励使用箭头键在 Vim 中浏览文本，并提示您使用`h`、`j`、`k`、`l`键。一开始你会行动缓慢，但慢慢地这将成为你的第二天性。

```
**" comments in vimrc start with "
" in normal mode**
nnoremap <Left>  : echoe "Use h" <CR>
nnoremap <Right> : echoe "Use l" <CR>
nnoremap <Up>    : echoe "Use k" <CR>
nnoremap <Down>  : echoe "Use j" <CR>**" in insert mode**
inoremap <Left>  : echoe "Use h" <CR>
inoremap <Right> : echoe "Use l" <CR>
inoremap <Up>    : echoe "Use k" <CR>
inoremap <Down>  : echoe "Use j" <CR>
```

*   编辑:另一个必须在`.vimrc`中设置的是缩进行。我花了很长时间试图找到一个允许缩进的选项，类似于 Visual Studio 或 Sublime，但还没有找到一个有效的。同时，`set autoindent`肯定会使在 Vim 中编辑比以前容易得多。

将这些设置添加到您的`.vimrc`文件中，并注意不同之处！您肯定会发现现在在 Vim 中编辑文本更有吸引力了！

点击 查看第 2 部分 [**中一些非常需要的补充内容！**](/customising-vim-to-get-the-best-out-of-it-part-2-931246996458)

## 参考

*   [https://missing.csail.mit.edu/2020/editors/](https://missing.csail.mit.edu/2020/editors/)