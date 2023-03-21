# Linux 中的信号

> 原文：<https://towardsdatascience.com/signals-in-linux-b34cea8c5791?source=collection_archive---------16----------------------->

我写这篇文章是为了做笔记的参考。我希望分享这篇文章对那些有兴趣了解 Linux 的人有所帮助。如果你发现任何错误或错别字，评论他们，我会尽快纠正。谢谢，继续学习。

![](img/8792af71e38c8255d462059ce5376b31.png)

照片由[亚伦·伯顿](https://unsplash.com/@aaronburden?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)在 [Unsplash](https://unsplash.com/images/nature?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) 上拍摄

一旦你开始熟悉 Linux 系统，信号是你经常会遇到的基本事物之一。了解它们是如何工作的很重要，因为信号在过程中起着很大的作用。

像往常一样，我们将从一些基础开始，然后转移到一些高级主题。

信号是对流程的通知，表示事件已经发生。信号有时被称为软件中断，因为在大多数情况下，它们会中断程序的正常执行流程，并且它们的到来是不可预测的。

信号可以由内核或进程发送。

![](img/f21044e218ceb373c630516ed173247a.png)

接收信号的不同方式

在本文的其余部分，我们将关注内核发送的信号，而不是进程。

在下列情况下，内核可以向进程发送信号:

1.  当出现硬件异常并且需要将该异常通知给进程时。例如，试图除以零，或引用不可访问的内存部分。
2.  一些软件事件发生在过程的控制之外，但是影响了过程。例如，输入在文件描述符上变得可用，终端窗口被调整大小，进程的 CPU 时间限制被超过，等等。
3.  用户键入了一些终端特殊字符，如中断(Ctrl+C)或暂停字符(Ctrl+Z)。

内核发送的通知进程事件的信号称为传统信号。

## 什么是待定信号？

由于某些事件而生成信号后，它不会直接传递给进程，而是保持在一个称为挂起状态的中间状态。这在以下情况下是必需的:

*   进程现在没有安排 CPU。在这种情况下，一旦该进程下一次计划运行，就发送一个挂起信号。
*   从流程的角度来看，为了确保某个信号不会在某个关键部分执行期间到达，流程可以将信号添加到其流程的信号掩码中，信号掩码是一组当前被阻止传递的信号。进程的信号掩码是每个进程的属性。如果信号在被阻塞时生成，它将保持挂起状态，直到后来被解除阻塞。有各种系统调用允许一个进程从它的信号掩码中添加和删除信号。

## 当信号到达时会发生什么？

当一个信号即将被发送时，根据该信号发生以下*默认*动作之一:

1.  信号被忽略，也就是说，它被内核丢弃，对进程没有影响。(该过程仍然不知道该事件已经发生。)
2.  进程被终止，也称为异常进程终止，与程序使用 *exit()终止时发生的正常进程终止相反。*
3.  生成一个核心转储文件，并终止该过程。
4.  暂停或恢复进程的执行。

一个进程可以不接受特定信号的默认动作，而是通过改变信号传递时发生的动作来设置信号的处理。程序可以设置以下配置之一:

1.  *默认动作*应该发生。这有助于撤销之前对信号配置的更改，而不是将其更改为默认值。
2.  该信号被*忽略*，而不是终止该过程的默认动作。
3.  执行已建立的*信号处理器*。信号处理器是一个定制的功能，它执行*适当的*任务以响应信号的传递。当信号到达时通知内核应该调用一个处理函数，这被称为*建立*或*安装*一个信号处理程序。不可能将信号的配置设置为终止或转储核心，除非其中之一是信号的默认配置。

*信号 SIGKILL 和 SIGSTOP 不能被捕捉、阻止或忽略。*

## 怎么发信号？

可以使用 [kill](https://linux.die.net/man/2/kill) 系统调用或 [kill](https://linux.die.net/man/1/kill) 命令发送信号，并指定所需的进程 pid。

*我们使用术语‘kill’是因为大多数信号终止一个进程的默认动作。*

传递给 *kill* 的过程 pid 根据以下情况进行解释:

1.  如果 pid > 0，则信号被发送到具有指定 pid 的特定进程。
2.  如果 pid = 0，则信号被发送到同一进程组中的每个进程。
3.  如果 pid < -1, then the signal is sent to every process in the process group whose process group id is modulus of |pid| specified.
4.  If pid = -1, then the signal is sent to all the processes for which the calling process has permission to send a signal, except *init* 和调用过程本身。以这种方式发送的信号被称为*广播信号*。

还存在另一个函数 [raise()](https://linux.die.net/man/3/raise) ，它向调用进程本身发送信号。这样发送信号，使得该信号甚至在 raise 函数返回之前就被进程接收到。请将此视为 IP 网络的本地回环类比。

对于单线程实现，`int raise(int sig)`被实现为`kill(getpid(), sig)`。

对于多线程实现，`int raise(int sig)`被实现为`pthread_kill(pthread_self(), sig)`。

## 零信号

发送信号的一个有趣的用例是检查进程的存在。如果调用 *kill()* 系统调用时，信号参数为‘0’，也称为*空信号*，则不会发送任何信号，它只是执行错误检查，以查看是否可以向进程发送信号。

这意味着我们可以使用这种机制来检查进程的存在。当试图发送空信号时，可能会出现以下响应之一:

1.  如果出现错误 ESRCH，则意味着目标进程不存在。
2.  如果出现错误 EPERM，那么这意味着目标进程存在，但是您(调用方进程)没有足够的权限向该进程发送信号。
3.  如果调用成功，那么这意味着目标进程存在，并且调用者有权向它发送信号。

## 各种可用信号

通过对应一个特定的信号编号，我们可以使用 [strsignal](https://linux.die.net/man/3/strsignal) 函数来获得信号描述。此外，您可以查看[信号的手册页](https://linux.die.net/man/7/signal)。

下面列出了原始 POSIX . 1–1990 标准中描述的信号。

```
 Signal     Value     Action   Comment
   ────────────────────────────────────────────────────────────────────
   SIGHUP        1       Term    Hangup detected on controlling
                                 terminal
                                 or death of controlling process
   SIGINT        2       Term    Interrupt from keyboard
   SIGQUIT       3       Core    Quit from keyboard
   SIGILL        4       Core    Illegal Instruction
   SIGABRT       6       Core    Abort signal from abort(3)
   SIGFPE        8       Core    Floating point exception
   SIGKILL       9       Term    Kill signal
   SIGSEGV      11       Core    Invalid memory reference
   SIGPIPE      13       Term    Broken pipe: write to pipe with no
                                 readers
   SIGALRM      14       Term    Timer signal from alarm(2)
   SIGTERM      15       Term    Termination signal
   SIGUSR1   30,10,16    Term    User-defined signal 1
   SIGUSR2   31,12,17    Term    User-defined signal 2
   SIGCHLD   20,17,18    Ign     Child stopped or terminated
   SIGCONT   19,18,25    Cont    Continue if stopped
   SIGSTOP   17,19,23    Stop    Stop process
   SIGTSTP   18,20,24    Stop    Stop typed at terminal
   SIGTTIN   21,21,26    Stop    Terminal input for background 
                                 process
   SIGTTOU   22,22,27    Stop    Terminal output for background 
                                 process
```

使用信号集来表示一组信号。像 [sigaction()](https://linux.die.net/man/2/sigaction) 和 [sigprocmask()](https://linux.die.net/man/2/sigprocmask) 这样的 API 可以使用信号集，它们允许程序指定一组信号。

## 每个进程的挂起信号集

如果一个进程接收到一个当前正在阻塞的信号，那么它将被添加到该进程的挂起信号集中。为了确定哪些信号对于一个进程是挂起的，一个进程可以使用`int sigpending(sigset_t* set);`系统调用，它通过修改`set` 结构返回调用进程挂起的信号集。

标准信号没有排队，因此当一个信号多次到达时，它被添加到待定信号*集合*(当然，它只有一个副本)，并且当信号被解除阻塞时，它稍后仅被传送一次。

# 信号 API 概述

信号是对一个进程的事件已经发生的通知，可以由内核、另一个进程或它自己发送。信号的传递是异步的。您可以使用`kill`系统调用向进程发送信号。当信号到达时，可以通过使用`signal()`或`sigaction()`建立信号处理器来修改进程的行为。

标准信号不排队，也就是说，发送到一个进程的多个信号将只作为一个被检索。

信号集用于同时处理多个信号。许多系统调用接受信号集。

当一个信号到达时，一个进程可以选择阻塞它，并且在稍后的时间，可以使用`sigpending()`系统调用获取一组未决信号，这将给出一组处于未决状态的信号。使用`pause()`系统调用，进程可以暂停执行，直到信号到达。