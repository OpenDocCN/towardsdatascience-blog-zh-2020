# 3 个简单的步骤来大幅改善你的代码

> 原文：<https://towardsdatascience.com/3-simple-steps-to-make-your-code-startlingly-better-f6505aa4e125?source=collection_archive---------44----------------------->

## 编写更干净代码的快速指南。

![](img/801cb8251bfaf89cce4f2611974671e1.png)

Mahmudul Hasan Shaon 在 [Unsplash](https://unsplash.com/s/photos/programming?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) 上拍摄的照片

*“当我在解决一个问题时，我从不考虑美。但当我完成后，如果解决方案不漂亮，我知道它是错的。”—* ***巴克明斯特富勒***

现在是下午晚些时候。你有一个你已经做了一整天的功能，你被一个你无法解决的 bug 卡住了——你知道这种 bug 会产生这样的想法:“也许我应该把整个电脑扔掉，去做桌子谋生”听起来是个好主意。但是你咬紧牙关，挺过去了。

你终于找到了解决办法。这是丑陋的地狱，你知道这一点，但它的工作…目前。

作为开发人员，我们都对此感到内疚。我们制定了一个可行的计划，但不幸的是，这是对我们工匠精神的嘲弄。

我将与你分享我获得的 5 个简单的技巧，它们将对你的代码质量产生巨大的影响。

# 命名你的函数来反映你的意图

写程序时，函数的命名是非常重要的。它会对理解代码的难易程度产生巨大的影响。

一个名副其实的函数可以传达你的意图，让你的代码看起来更像自然语言。而命名不当的函数会掩盖程序员的真实意图。

这里有一个简单的例子来说明我的观点。你能告诉我这个函数在做什么吗？

```
handleEdit = (e) => {
        // Prevent navigation to axios post.
        e.preventDefault();

        // Get user id from context
        const userId = this.context.user._id;

        // Get updated user info from state
        let user = {
            fname: this.state.fname,
            lname: this.state.lname
        }

        // Send PUT request to edit route using updated user
        axios.put(`http://localhost:3000/user/edit/${userId}`, user)
        .then( res => {
            if ( res.status === 200 ){
                this.setState({ successMessage : res.data });
                this.context.user.fname = res.data.fname;
                this.context.user.lname = res.data.lname;
                this.toggleInputs();
            }
        }).catch( err => {
            console.log(err);
        });
}
```

逐行阅读代码后，您可能会认为代码试图更新用户。标题 ***handleEdit*** 回避了“我们到底在编辑什么？”

现在来看看这个名字更新后的例子。

```
updateUser = (e) => {
    // Code here
}
```

你看到了吗？代码不需要确定这个函数在做什么。这是一个将会更新用户的功能。这个名字清楚地表明了我的意图。

这就引出了下一点。

# 命名您的变量以反映您的意图

命名变量以反映您的意图会对您的代码产生很好的影响。

考虑这个例子。这段代码旨在让用户确认他们想要停用他们的帐户。这是原始代码。

```
var deactivationResponse = window.confirm("Are you sure you would like to deactivate your account?"); if( deactivationResponse ){
    doSomething()
}
```

还不错。它确实传达了我们正在依赖某种回应。但是它可以自动阅读。这是更新的版本。

```
var userConfirmsDeactivation = window.confirm("Are you sure you would like to deactivate your account?");if( userConfirmsDeactivation ){
    doSomething()
}
```

这样好多了。你甚至不需要阅读原始变量来完全理解我们正在检查的条件。

# 去掉那些臭评论！！

![](img/e4777d7422ccc1dd31bc559a4a54fd30.png)

照片由 [Unsplash](https://unsplash.com/s/photos/delete?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) 上的 [u j e s h](https://unsplash.com/@ujesh?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) 拍摄

评论有多种形式。有些是有用的，提供了丰富的上下文和信息，这些信息可能是非常特定于领域的，并且可能难以置信地难以通过单独的代码进行交流。这些注释很重要，可以为您和其他必须编写代码的人节省宝贵的时间。

但是，大部分评论都是训练轮。当我们为变量和函数留下一串无意义的名字时，我们依靠注释来帮助我们找到一个有效的程序。

注释帮助我们对写得不好的代码感觉更好，或者更糟的是，它们重述了显而易见的事实。你应该尽可能地写好你的代码，因为当你的代码读起来像自然语言的时候，注释就会碍事。

软件开发既是一门科学，也是一门艺术。作为开发人员，我们最崇高的目标是编写不仅仅是工作的代码——而是编写我们可以毫无压力地阅读的代码，易于维护的代码，以及我们可以引以为豪的代码。

我想以对罗伯特·塞西尔·马丁(Robert Cecil Martin)的快速点头作为结束，他是*《干净的代码:敏捷软件工艺手册》*的作者，这篇文章的灵感很大程度上来自于测试他的作品中分享的设计原则对我来说是多么有效。如果你觉得这篇文章有用，我强烈建议你把这本书加入你的阅读清单！