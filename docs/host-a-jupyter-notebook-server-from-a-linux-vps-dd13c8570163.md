# 从 Linux VPS 托管 Jupyter 笔记本服务器

> 原文：<https://towardsdatascience.com/host-a-jupyter-notebook-server-from-a-linux-vps-dd13c8570163?source=collection_archive---------46----------------------->

## 如何从您的 Linux 服务器托管 Jupyter 笔记本服务器。

![](img/b114da5facfc08c05daf1c4dfe542873.png)

(jupyter logo src = http://jupyter.org/)

Jupyter 笔记本是行业标准工具，使统计计算编程更加容易。这是因为在编写统计数据时，您可能经常想要读取返回，并且能够编辑和重新运行代码。此外，拥有一个虚拟内核来做这件事也非常有用。虽然 Jupyter 笔记本很适合在本地机器上使用，但很难忽略它们是在 HTTP 服务器上运行的。鉴于 Jupyter Notebooks 是通过网络浏览器访问的，这一点尤其正确。

因此，由于 Jupyter Notebooks 是在 web 浏览器中运行的，并且具有成熟的非开发服务器，甚至具有密码和写保护，因此很容易理解为什么您可能希望在虚拟专用服务器(VPS)上运行 Jupyter。)近年来，服务器的租金已经便宜了很多。此外，你可以很容易地租到硬件比你的计算机好得多的服务器。最重要的是，这是一种在大型团队中共享文件，甚至是文本文件的好方法，因为您甚至可以将 Jupyter 用作文件浏览 GUI、bash 终端和文本编辑器。

首先，要在您的 VPS 上建立并运行一个 web 服务器，您当然需要通过 SSH 连接到该服务器。

```
ssh emmett@my-ip-is-hidden
```

# 负载平衡器/网络服务器

为了在 Linux 上为 wsgi 建立一个 web 服务器，我们需要一个 web 服务器。我喜欢使用 NGINX，但是也有几个选择，比如 Apache，它也经常被使用。然而，如果你是新手，我强烈建议你使用 NGINX。NGINX 比 Apache 快，在本文中，我将向您展示如何设置它来托管本地服务器。

要使用 NGINX(顺便说一下，发音是 engine-ex，)我们首先需要安装它。我很确定 NGINX 在所有的包管理器中都是可用的，但是如果你使用的是 Suse 服务器，我当然不能考虑 man。

```
sudo dnf install nginx
```

一旦我们安装了 NGINX，我们就可以继续配置我们的 web 服务器了。首先，cd 转到/etc/nginx/conf.d，这是存储 nginx 配置文件的地方。

```
cd /etc/nginx/conf.d
```

现在打开一个新的。conf 文件，请确保您以 root 用户身份运行，因为您可能没有权限在此处保存文件:

```
sudo nano mysite.conf
```

接下来，我们将创建一个配置文件。如果您的 VPS 上没有运行任何服务器，并且您可以使用端口 8000，那么复制我的就没有任何问题:

```
**server** {
    **listen** 80;
    **server_name** your.ip.here;

    **location** / {
        **proxy_pass** http://127.0.0.1:8000;
        **proxy_set_header** Host $host;
        **proxy_set_header** X-Forwarded-For $proxy_add_x_forwarded_for;
    }
}
```

现在只需 ctrl+o 然后按 y 保存文本缓冲区，并使用 ctrl+x 关闭 Nano。接下来，我们必须取消默认 NGINX 配置文件的链接。

```
sudo unlink /etc/nginx/sites-enabled/default
```

现在，您可以使用以下命令重新加载配置:

```
nginx -s reload
```

要测试您的配置，您可以安装并运行 jupyter-notebook，然后转到 NGINX 配置文件中提供的域或 IP。

```
sudo dnf install jupyter-notebook && jupyter-notebook
```

现在，如果你在浏览器中浏览你的域名或 IP 地址，你应该会到达 Jupyter。

# 监督者

最后一步是设置一个管理员来运行 jupyter-notebook 命令。根据您的发行版，您可能需要安装以下任一软件

```
sudo apt-get install supervisor
sudo dnf install supervisord
```

现在，我们将 cd 转到我们主管的配置目录。

```
/etc/supervisor/conf.d/
```

现在，正如您可能已经预料到的，我们需要更新另一个配置文件。

> 欢迎来到开发运营的世界。

这是我的，你可以用它作为你的基础。

```
[program:flask_app]
directory=/
command=jupyter-notebook
autostart=true
autorestart=true
stopasgroup=true
killasgroup=true
stderr_logfile=/var/log/appname/lognameerr.log
stdout_logfile=/var/log/appname/lognamestdout.log
```

> 很简单，对吧？

现在只需重新加载主管，

```
sudo service supervisor reload
```

现在，您的笔记本电脑服务器将启动，您可以随时与其他人一起访问和使用它！

# 结论

我认为拥有自己的远程 Jupyter 笔记本服务器非常酷！这是我在团队中使用的最好的方法之一，因为你的整个团队可以同时访问和修改同一目录中的文件。此外，您将可以使用 Bash、笔记本和一个图形文本编辑器。虽然我确实喜欢 Nano，但我可以理解为什么它可能会让那些不像我这样的终端狂热者感到有些畏惧。您还可以将它用作网络附加存储的本地解决方案，尤其是如果您打算将所有数据科学项目都放在那里的话。这是一个很好的方法，通过把旧电脑变成服务器来利用它。总而言之，很难不发现 Jupyter 服务器的巨大用途，无论是在本地、VPS、NAS 上，还是在 2000 年代初的 2GB RAM 笔记本电脑上运行。