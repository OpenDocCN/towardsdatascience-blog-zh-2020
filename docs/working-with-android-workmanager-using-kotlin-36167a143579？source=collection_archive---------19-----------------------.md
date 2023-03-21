# 使用 Kotlin 使用 Android WorkManager

> 原文：<https://towardsdatascience.com/working-with-android-workmanager-using-kotlin-36167a143579?source=collection_archive---------19----------------------->

## 向后兼容的调度未来任务的理想方式

![](img/fa7e8f75bc84305d11ba49d8d1b490dd.png)

Fernando Jorge 在 [Unsplash](https://unsplash.com/s/photos/under-water?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) 上拍摄的照片

**工作管理器**是一个 API，它可以调度你未来的异步任务，并且可以在后台运行它们。即使用户在应用程序之外或应用程序关闭，分配给工作管理器的任务也会执行。WorkManager 只能运行您的任务一次，也可以多次运行或定期运行。

# 工作管理器的功能

*   它提供了高达 API 级别 14 的向后兼容性
*   您可以添加一个或多个约束条件，如仅在手机充电或重启时执行任务等。
*   您可以计划一次性任务或定期任务
*   您还可以链接多个任务。例如，任务 **(B)** 应该只在任务 **(A)** 完成时执行。
*   它可以帮助您在特定事件上执行任务。

**注意:**工作管理器不适用于在 app 进程结束时可以安全终止的正在进行的后台工作，也不适用于需要立即执行的任务。

# 上层社会

**工作人员:**这里定义了需要完成的工作。

**WorkRequest:** 它决定将要执行哪个 worker 类。这是一个抽象类，所以我们将使用它的直接类，它们是 **OneTimeWorkRequest** 和 **PeriodWorkRequest** 。

工作管理器:它对工作请求进行排队和管理。

WorkInfo: 它给我们关于工作的信息，不管是成功的、运行的还是失败的。

让我们现在开始编码吧…

# 我们会创造什么？

我们将在后台创建一个通知，这个通知只能创建一次，因为我们使用的是 **OneTimeWorkRequest** 类。稍后，我们将使用一些约束来基于事件生成通知。

首先，添加以下依赖项。

```
implementation **"androidx.work:work-runtime-ktx:2.3.4"**
```

我们将首先通过扩展`***Worker***`类来创建我们的 worker 类，并覆盖它的`***doWork()***`方法用于后台处理。当工作管理器调用`***doWork()***`方法时，它调用用户定义的方法`***createNotification()***`。

在我们的`***MainActivity.kt***`类中，我创建了一个按钮，当用户点击按钮时，就会立即生成通知。

在这里，我创建了 **OneTimeWorkRequest** 的对象，并传递了我们的`***MyWork***`类的类名。在现实世界中，我们可以有很多 worker 类，所以应该执行哪个类是由这个 request 对象决定的。

```
**val** request = *OneTimeWorkRequestBuilder*<MyWork>().build()
```

当用户单击按钮时，WorkManager 将请求排队。

```
WorkManager.getInstance(**this**).enqueue(request)
```

在这里，我们创建一个 toast 来显示我们任务的状态，无论是**运行**、**成功**还是**失败**。`***getWorkInfoByIdLiveData***`方法获取请求 id 并给出关于任务的信息。

```
WorkManager.getInstance(**this**).getWorkInfoByIdLiveData(request.*id*)
            .observe(**this**, *Observer* **{

                val** status: String = **it**.*state*.**name** Toast.makeText(**this**,status, Toast.*LENGTH_SHORT*).show()
            **}**)

}
```

现在运行你的应用程序，点击按钮，你会看到一个通知。

现在，我们将了解如何添加约束，以便仅在手机充电时创建通知。

添加以下代码行以创建约束并修改您的请求对象。在请求对象中，我只是设置约束，仅此而已。现在，只有当满足这个特定标准时，才会生成通知。

```
**val** constraints = Constraints.Builder()
        .setRequiresCharging(**true**)
        .build()**var** request = *OneTimeWorkRequestBuilder*<MyWork>()
        .setConstraints(constraints)
        .build()
```

**注意:**当指定了多个约束时，您的任务只有在所有约束都满足时才会运行。

如果你点击按钮，而你的手机没有在充电，那么你会看到一个状态显示**“排队”，**这意味着你的请求已经被放入队列，只有当你的手机正在充电时才会执行。

如果你在运行代码时遇到任何问题，那么你可以从我的 [**Github**](https://github.com/himanshujbd/WorkManager) 账户下载这个项目。

我希望你喜欢读这篇文章，你也可以访问我的 [**网站**](http://thehimanshuverma.com/) ，在那里我会定期发布文章。

[**订阅**](https://mailchi.mp/b08da935e5d9/himanshuverma) 我的邮件列表，以便在您的收件箱中直接获得我的文章，并且不要忘记关注我自己在 Medium [**【代码怪兽】**](https://medium.com/the-code-monster) 上发表的文章，以丰富您的技术知识。

# 结论

我们已经看到了如何使用 WorkManager 类来执行一些后台处理。在本文中，我在后台创建了一个通知。也看到了我们如何根据未来将要发生的事件来安排未来的任务。