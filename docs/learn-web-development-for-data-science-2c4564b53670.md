# 学习数据科学的 Web 开发

> 原文：<https://towardsdatascience.com/learn-web-development-for-data-science-2c4564b53670?source=collection_archive---------43----------------------->

## 如何从数据的海洋中拓宽我们的视野

![](img/85595d3c36007d46ef8117b0b2d8c4d8.png)

在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上由 [Atikh Bana](https://unsplash.com/@tikh?utm_source=medium&utm_medium=referral) 拍摄的照片

数据专家的生活通常相当舒适。我们坐在一个受控、安全的环境中，以特定的模式移动手指，让电子信号做一些事情——为此，我们得到了丰厚的回报。

这是一种便利的、有特权的存在。但在这种情况下，我们应该总是向前看，并问自己，**下一步是什么？**

这个行业的下一步是什么？为了我们的事业？应该把注意力放在哪里？

我不知道答案，但我非常相信学习做更多的事情。就我个人而言，我喜欢创造东西，我知道这是许多人的共同倾向。

对我们大多数人来说，我们可以让数据做事情。但是当涉及到允许其他人(没有 Python)使用我们的工具时，就不那么容易了。

在这些情况下，没有比我们在 web 开发中学习的技能更好的技能了。

因此，在这篇文章中，我将分享我发现的最好的资源，以开始掌握并快速产生实践 web 开发的切实好处——重点关注:

```
**> tensorflow.js****> Angular****> The Cloud**
 - Google Cloud
 - Azure
```

# TensorFlow.js

很明显，tensorflow.js 是 JavaScript 中的 tensorflow。这是一个非常酷的项目，对于我们这些目前专注于 Python 和 ML 的人来说，这是进入 web 开发的一个极好的切入点。

对于那些熟悉 TensorFlow(或任何 ML 框架)的人来说，学习 tensorflow.js 相当简单，因为我们构建的是相同的工作流，但语法略有不同。

例如，Python 中一个简单的多类分类器如下所示:

```
model = tf.keras.Sequential()model.add(tf.keras.layers.Dense(
    input_shape=(1, 10),
    activation="sigmoid", units=5
))model.add(tf.keras.layers.Dense(3, activation="softmax"))
```

用 tensorflow.js 重写，我们得到:

```
const model = tf.sequential();model.add(tf.layers.dense({
    inputShape: [10],
    activation: "sigmoid", units: 5
}))model.add(tf.layers.dense({activation: "softmax", units: 3})
```

一切都很熟悉，但是使用 JavaScript 语法。一旦一个模型被构建或导入到 JS 中，我们就开始构建简单的 HTML、CSS 和 JS 页面来支持与模型的交互— [,我将在这里介绍](/how-to-deploy-tensorflow-models-to-the-web-81da150f87f7)。

所有这些都意味着我们逐渐构建了支持我们所使用的工具所需的许多要素。

学习 tensorflow.js 最容易的地方是 Coursera 上的 deeplearning.ai:

[](https://www.coursera.org/learn/browser-based-models-tensorflow) [## 基于浏览器的模型和 TensorFlow.js

### 将机器学习模型带入现实世界涉及的不仅仅是…

www.coursera.org](https://www.coursera.org/learn/browser-based-models-tensorflow) 

# 有角的

我们已经构建了一个包含我们模型的简单 web 应用程序。那很好，但是有很多限制。

仅仅用 HTML、CSS 和 JS 来构建一个真正交互式的、响应迅速的、漂亮的 web 应用程序，有点像期望使用普通花园棚中可用的工具和材料来构建一架 F-35。

![](img/76ca47741259d3b3de8e5cf328376b14.png)

布里特·盖瑟在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄的照片

棱角分明不是一般的花园棚子。这是一个非常强大的现代框架，专为构建网络应用而设计。

与大多数其他框架相比，它相对容易掌握。谷歌支持这个项目，Angular 社区非常庞大。

所有这些因素导致了一个蓬勃发展的生态系统，为一个已经强大的工具提供了信息。有很多学习框架的资源，但是我发现有三个资源特别特别。

## 1.繁忙的开发人员的角度速成班

本课程由 Mosh Hamedani 教授，他是一位非常受欢迎的教师，教授一系列的 web 开发课程。

对我来说，这是对 Angular 的完美介绍——它简短而甜美，没有任何不必要的绒毛。此外，有一整节专门介绍 TypeScript(编程语言)，这对初学者非常有用。

[](https://click.linksynergy.com/link?id=I8HIg4MxNog&offerid=507388.719002&type=2&murl=https%3A%2F%2Fwww.udemy.com%2Fcourse%2Fangular-crash-course%2F) [## 繁忙的开发人员的角度速成班

### 你可能听说过，如今有棱角的开发人员很吃香。而你在这里学角快…

www.udemy.com](https://click.linksynergy.com/link?id=I8HIg4MxNog&offerid=507388.719002&type=2&murl=https%3A%2F%2Fwww.udemy.com%2Fcourse%2Fangular-crash-course%2F) 

## 2.Angular —完整指南(2020 版)

接下来是 Maximilian Schwarzüller 的课程。我认为这家伙真的每天都花一整天来创造有角度的课程。他创作了数量惊人的电影——但他真的 ***真的*** 擅长这个。

这是一个更长的课程，涵盖了 Mosh 课程的大部分内容(不包括打字稿介绍)，但更深入。

尽管如此，如果你有选择，我无疑会选择 Mosh 的课程，然后是这个。但如果非此即彼——选择这个！

[](https://click.linksynergy.com/link?id=I8HIg4MxNog&offerid=507388.756150&type=2&murl=https%3A%2F%2Fwww.udemy.com%2Fcourse%2Fthe-complete-guide-to-angular-2%2F) [## Angular —完整指南(2020 版)

### 掌握 Angular 10(以前的“Angular 2”)并使用 Angular.js 的继任者构建出色的反应式 web 应用程序

www.udemy.com](https://click.linksynergy.com/link?id=I8HIg4MxNog&offerid=507388.756150&type=2&murl=https%3A%2F%2Fwww.udemy.com%2Fcourse%2Fthe-complete-guide-to-angular-2%2F) 

## 3.角形(全应用),带角形材料、Angularfire 和 NgRx

正如我说过的，Maximilian Schwarzüller 设计了很多有角度的课程，这个课程也是由 Max 设计的。

这门课程采取了不同于其他课程的方法。在这里，我们通过构建一个全功能的 web 应用程序来学习 Angular！

这是一种不同的方法，我建议在学习本课程之前先熟悉 Angular 但它涵盖了许多很酷的功能。特别是:

*   **棱角分明的材料**，谷歌的设计框架——它很漂亮，反应灵敏，非常有用。
*   Angularfire 是一个整合 Angular 应用程序和谷歌云的框架，这是许多现代网络应用程序的关键因素。

[](https://click.linksynergy.com/link?id=I8HIg4MxNog&offerid=507388.1512962&type=2&murl=https%3A%2F%2Fwww.udemy.com%2Fcourse%2Fangular-full-app-with-angular-material-angularfire-ngrx%2F) [## 角形(全应用),带角形材料、Angularfire 和 NgRx

### 使用角，角材料，Angularfire(带 Firestore 的+ Firebase)和 NgRx 建立一个真正的角应用程序…

www.udemy.com](https://click.linksynergy.com/link?id=I8HIg4MxNog&offerid=507388.1512962&type=2&murl=https%3A%2F%2Fwww.udemy.com%2Fcourse%2Fangular-full-app-with-angular-material-angularfire-ngrx%2F) 

# 云

我们在 Angular 课程中接触了几次云，老实说，这已经足够让我们开始学习了。

然而，云已经存在，开发我们的云技能伴随着适度确定的未来需求。所以我会考虑在 Angular 或之后继续深造。

## 谷歌云

到目前为止，最轻松的基于云的设置是 Google Firebase，这是一个为我们管理所有基础设施的平台——我们只需上传代码。

我推荐通过[第三角课程](https://click.linksynergy.com/link?id=I8HIg4MxNog&offerid=507388.1512962&type=2&murl=https%3A%2F%2Fwww.udemy.com%2Fcourse%2Fangular-full-app-with-angular-material-angularfire-ngrx%2F)学习 Firebase。然而，我注意到 Udemy 上有一些其他的 Firebase 特定课程——我从来没有上过，所以我不知道它们是否好。

尽管如此，谷歌云提供的不仅仅是 Firebase。如果你对更广阔的云平台感兴趣——谷歌自己在 Coursera 上提供了很多非常优秀的课程。

[](https://www.coursera.org/learn/gcp-fundamentals) [## 谷歌云平台基础:核心基础设施

### 由谷歌云提供。本课程向您介绍使用谷歌云的重要概念和术语…

www.coursera.org](https://www.coursera.org/learn/gcp-fundamentals) 

## 蔚蓝的

我最后推荐的是 Azure。对我来说，这更像是环境的结合——但是很明显 Azure 确实提供了一个很棒的服务。

现在，毫无疑问，掌握 Azure 的最佳过程是从 Azure 基础认证开始，在学习的同时使用该服务。

该课程提供了 Azure cloud 作为一个整体的广泛视角，允许您通过他们的进一步认证学习更专业的知识。

我不会在这里深入讨论 Azure 但是如果你对它感兴趣，我在这里写了更多关于准备基础认证考试的内容[。](/how-to-fail-the-azure-fundamentals-certification-e37a650a251e)

# 快乐学习

Web 开发是一项技能，比其他任何技能都更能让你开始与世界其他地方分享你的工作。

我们可以把这些模型、管道和分析放在一个只有我们有钥匙的满是灰尘的柜子里，而不是在它们上面系上几条丝带，并把它们发布给世界其他地方。

我希望这些资源能对你有所帮助。它们是迄今为止我上过的最好的课，我怎么推荐都不为过。

我希望你喜欢这篇文章！如果你有任何问题或想法——让我在 [Twitter](https://twitter.com/jamescalam) 或下面的评论中知道。

感谢阅读！

如果你对学习数据科学的 web 开发感兴趣，但还没有准备好参加任何付费课程——试试我的关于 ML web-apps with tensorflow.js 的介绍文章:

[](/how-to-deploy-tensorflow-models-to-the-web-81da150f87f7) [## 如何将 TensorFlow 模型部署到 Web

### 使用基本 JavaScript 在浏览器中运行 Python 构建的模型

towardsdatascience.com](/how-to-deploy-tensorflow-models-to-the-web-81da150f87f7)