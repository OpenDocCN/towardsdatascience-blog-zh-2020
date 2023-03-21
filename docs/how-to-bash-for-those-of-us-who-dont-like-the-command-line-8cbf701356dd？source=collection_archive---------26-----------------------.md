# 如何抨击:对于我们这些不喜欢命令行的人来说

> 原文：<https://towardsdatascience.com/how-to-bash-for-those-of-us-who-dont-like-the-command-line-8cbf701356dd?source=collection_archive---------26----------------------->

## 80 年代的技术让你成为更快的程序员

![](img/20c3cee7ab32eecfbfc717219bcfd73d.png)

凯文·霍尔瓦特在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄的照片

詹姆斯塔克。PyCaret。CI/CD 自动化。软件开发前沿的所有性感的前沿技术。

然后是巴什。伯恩又一个外壳，Unix 用户的默认外壳——Linux 和 Mac。对于在 Windows 上工作的软件开发人员来说是绝对必要的。(如果你没有在 Windows 上使用 WSL2，停止阅读这篇文章，进入`Windows Features`，打开 Linux 的 Windows 子系统…现在！)

Bash 可能看起来是一个不值得您花费时间的遗迹。而且肯定不会诱发寻找最新东西的开发者流口水。但是它*是*所有运行终端或者 Windows 终端的人都用的(或者应该是)。

运行上面提到的所有性感程序？巴什。

我也用鼠标，所以这不是命令行纯粹主义者的说教。然而，这个外壳的用途不仅仅是你现在使用的。在文件系统中导航。设置项目。运行 Python / Node / whatever 包管理器的命令。

在本文中，我想分享一些 Bash 的基本知识。如何从 Bash shell 中获得更多的东西，而不至于让自己疯狂地试图掌握它的许多怪癖。

最后，我将在我的 Windows 笔记本电脑上共享`.bashrc`文件。这个文件连同。`bash_aliases`，每天为我节省了大量的按键和认知压力。

# `.bashrc`文件

`.bashrc`文件位于您的主目录的根目录下。大多数终端和文件 ui 默认不显示“点文件”；它们是隐藏文件。您可以通过输入以下内容来查看您的:

```
$ ls -a ~/.bashrc
/c/Users/nicho/.bashrc*
```

`ls`命令上的`-a`开关告诉程序打印所有文件，甚至是点文件。

shell 程序启动时需要运行的任何东西的 Home base。

*   环境变量
*   功能
*   别名
*   设置
*   提示
*   自动完成

这是所有好东西的去处。我保持简单。虽然没有必要，但我的别名放在一个单独的文件中，`.bash_aliases`。Bash 将在您的`.bashrc`中加载这些代码。

```
if [ -f ~/.bash_aliases ]; then
    . ~/.bash_aliases
fi
```

# Bash 别名

使用`alias`命令查看 shell 环境中可用的别名。

```
$ alias
alias avm='ssh -i ~/.ssh/digitalocean root@XXX.XX.XXX.XXX'
alias az='az.cmd'
alias ba='vim ~/.bash_aliases'
alias chrome='/c/Program\ Files\ \(x86\)/Google/Chrome/Application/chrome.exe'
alias clpr='cd /c/projects/CLIENTPROJECTS'
alias cls='clear'
alias cn='code -n .'
alias d='cd /c/Users/nicho/Downloads/'
alias day='touch ~/Downloads/$(date +%m%d%Y).txt; code ~/Downloads/$(date +%m%d%Y).txt'
alias dcu='docker-compose up -d'
alias default='export AWS_PROFILE=default && echo Using my default AWS account'
alias docs='cd /c/Users/nicho/OneDrive/Documents'
alias dpa='docker ps -a'
alias e='explorer .'
alias ee='exit'
alias es='/c/tools/elasticsearch/bin/elasticsearch.bat
...
```

别名只受你的想象力的限制。通过不接受参数。

```
# alias test="echo Hi, $1 $2"
$ test Jane Doe
Hi, Jane Doe
​
# alias test="echo Hi, $1 $2\. We are using aliases!"
$ test Jane Doe
Hi, . We are using aliases! Jane Doe
```

最好对带参数的命令使用 Bash 函数。

# 完成工作的基本功能

Bash 函数有足够简单的语法来声明:

```
function_name () {
  # logic here
}
```

示例:

```
test () {
  if [ -z "$1" ]; then # no argument
    echo "Hi, Anonymous!";
  else
    echo "Hi, $1 $2\. We are using aliases!";
  fi
}
```

编写 Bash 脚本的一个好处是，您可以直接从终端程序运行它们(在 Windows 上，当运行 [Git BASH](https://gitforwindows.org/) 时)。Bash 也是一种编程语言:

```
$ test () {
>   if [ -z "$1" ]; then # no argument
>     echo "Hi, Anonymous!";
>   else
>     echo "Hi, $1 $2\. We are using Bash functions!";
>   fi
> }
$ test
Hi, Anonymous!
$ test Nick B
Hi, Nick B. We are using Bash functions!
```

…但这也是我对 Bash 的 shell 脚本一无所知的地方。对于任何比上面的例子更复杂的东西——模板、大量变量、嵌套命令——请使用 Python。Python 彻底击败了 Bash 编写的易于理解的脚本。

# 快捷键

# 我的`.bashrc`

请击鼓！正如承诺的，这是我的`.bashrc`设置。在电脑上，没有比`.bashrc`更私人的文件了。(不，我没那么无辜。我们在这里谈论编程。)

你会发现这些对你没用。有些，我真心希望你能做到。拿一些你认为可以使用的部分，复制它们，然后自己动手做。自行添加。

请记住，您需要运行`.bashrc`文件来查看 shell 中的更改是否生效。

```
$ source ~/.bashrc
$ # or
$ . ~/.bashrc
```

我的`.bashrc`档案:

```
#!/bin/bash
​
export PATH="/c/scripts/:$PATH"
​
# virtualenvwrapper
export WORKON_HOME=$HOME/Envs
​
# Postgres
export PGCLIENTENCODING=UTF8
​
PATH=~/.local/bin:$PATH
​
if [ -f ~/.bash_aliases ]; then
. ~/.bash_aliases
fi
​
# command completion for AWS CLI
complete -C '/c/Python36/Scripts/aws_completer' aws
​
# function to go to the designated projects folder
p() {
    cd /c/projects/$1
}
​
# function to cd to the scripts folder, then subfolder
s() {
    cd /c/scripts/$1
}
​
# function to cd to the startup folder
startup() {
    cd /c/Users/nicho/AppData/Roaming/Microsoft/Windows/Start\ Menu/Programs/Startup
}
​
# random number generator
rand() {
  if [ -z "$1" ] # no argument
    then
      range=4
    else
      range=$1 # will error if argument is not a number
  fi
  local num
  for ((i=0;i<$range;i++))
    do
    num=$num$((RANDOM%9))
    done
  echo $num
  printf "%s" $num | clip.exe
  echo "Number is on the clipboard."
}
​
# alias for cd change directory
a() {
  if [ -z "$1" ] # no argument
    then
      cd -
    else
      cd $1
  fi
}
​
# alias for choco install --confirm
ci() {
  if [ -z "$1" ] # no argument
    then
      echo "choco install needs a package name argument to run."
    else
      choco install --confirm $1
  fi
}
​
# get an AWS elasticsearch domain's endpoint to the clipboard
esdomain() {
  if [ -z "$1" ] # no argument
    then
      echo "need a domain name as an argument for this function"
    else
      ENDPOINT=$(aws es describe-elasticsearch-domain --domain-name $1 | jq '.DomainStatus.Endpoint' | tr -d \")
      printf "%s" $ENDPOINT | clip.exe
      echo "The endpoint for $1 is on the clipboard."
  fi
}
​
# open my date log files
week() {
  today=$(date +"%Y-%m-%d")
  lastSunday=$(date -d "$today -$(date -d $today +"%u") days" +"%Y-%m-%d")
  for ((i=1; i<=7; i++)); do
    code ~/OneDrive/Documents/$(date +"%m%d%Y" -d "$lastSunday +$i days").txt
  done
}
​
toobig() {
  if [ -z "$1"] # no argument
    then
      du -h --threshold=5000000 # show files 5MB or greater
    else
      du -h --threshold=$1
  fi
}
```

# 结论

Bash 并不新鲜。但正因为如此，它是您的软件开发环境的基础。如果像 Bash 别名或某个函数这样的新技巧节省了您通常在终端中导航所花费的一半时间，那么这意味着您只需要实现一两个新东西，就可以将命令行的工作效率提高一倍！

我很想知道你在系统中使用了哪些 Bash 特性。请对本文发表评论。谁知道呢？也许你发现的东西也能帮助我们其他人！