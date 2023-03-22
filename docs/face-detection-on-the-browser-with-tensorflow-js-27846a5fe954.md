# 使用 Tensorflow.js 优化浏览器上的人脸检测

> 原文：<https://towardsdatascience.com/face-detection-on-the-browser-with-tensorflow-js-27846a5fe954?source=collection_archive---------19----------------------->

## [现实世界中的数据科学](https://towardsdatascience.com/data-science-in-the-real-world/home?gi=f64157fe3425)

## 基于浏览器的人脸检测器正是你所需要的

![](img/edce88e64ec0b20b7cd525bab7d79b8e.png)

我的健壮的基于浏览器的人脸检测结果

> *让我们开始讨论最重要的问题。*

## 为什么是这个探测器？

你可能遇到过很多人脸检测教程和项目。不过，这个不同。它部署在浏览器上，可以在任何地方使用，从监督在线考试到许多不同的街机游戏，再到使用它来实时检测人脸面具、模糊或提高人脸的分辨率(因为获得了感兴趣的区域)，同时还可以以相当高的速度执行。网络上的机器学习是这个时代的要求，不应该局限于本地系统。另一种方法是 face-api.js，它使用多任务 CNN，但我们的检测器具有更高的准确性。许多其他项目将基于部署在 Flask 应用程序上的面部检测模型，相比之下，这些应用程序非常慢。

Tensorflow 服务使用 gRPC 和 Protobuf，而 Flask 应用使用 REST 和 JSON。JSON 依赖于 HTTP 1.1，而 gRPC 使用 HTTP/2。HTTP 1.1 存在延迟问题。每个单独的请求都需要 TCP 握手，大量的请求会大大增加加载页面所需的时间。HTTP 1.1 也受到行首阻塞的影响，它限制了同一个域的连接数。使用 HTTP 管道，您可以在等待前一个请求的响应时发送一个请求，从而有效地创建一个队列。但这也带来了其他问题。如果你的请求被一个缓慢的请求卡住了，那么你的响应时间将会受到影响。HTTP /2 保持了 HTTP 的基本前提和范例，但是去掉了 HTTP 1.1 的可选部分。REST 只支持 HTTP 1.x 中可用的请求-响应模型，但是 gRPC 充分利用了 HTTP/2 的功能，并允许您不断地传输信息。Protobuf 是一种用于序列化数据的二进制格式，比 JSON 更高效。Tensorflow 服务可以向同一模型批量发送请求，这更适合使用硬件(GPU)。Tensorflow 服务的性能相当于用 C/C++编写的代码。

此外，Flask 应用程序是用 Python 编写的，而 Tensorflow.js 将 Node.js 或 Chrome V8 引擎作为其服务器端节点。在这个项目中，我们使用 Chrome 的 V8 Javascript 引擎，这是一个开源的高性能 Javascript 引擎。

要获得更多关于 Chrome V8 引擎的直觉，请阅读这个令人敬畏的博客

[](https://www.freecodecamp.org/news/understanding-the-core-of-nodejs-the-powerful-chrome-v8-engine-79e7eb8af964/) [## 了解 Chrome V8 引擎如何将 JavaScript 翻译成机器码

### 通过 Mayank Tripathi 了解 Chrome V8 引擎如何在深入研究之前将 JavaScript 翻译成机器码…

www.freecodecamp.org](https://www.freecodecamp.org/news/understanding-the-core-of-nodejs-the-powerful-chrome-v8-engine-79e7eb8af964/) 

Tensorflow.js 提供了使用 Web GL 层所需的正确类型的硬件加速，它可以被视为浏览器端 GPU。WebGL 是一个 JavaScript API，用于在任何兼容的 web 浏览器中渲染交互式 2D 和 3D 图形，而无需使用插件。WebGL 与其他 web 标准完全集成，允许 GPU 加速使用物理和图像处理以及作为网页画布一部分的效果。

因此，这个客户端服务人脸检测器被证明比以前的 flask 人脸检测应用程序更快。

## **Tensorflow.js**

Tensorflow.js 是 Javascript 中的机器学习库。它是一个开源的硬件加速 JavaScript 库，用于训练和部署机器学习模型。

它可以用于在浏览器中开发 ML，通过使用灵活直观的 API，使用低级 JavaScript 线性代数库或高级 layers API 从头构建模型。也可以通过在 Node.js 运行时下用相同的 TensorFlow.js API 运行 native TensorFlow 在 Node.js 中开发 ML。TensorFlow.js 模型转换器可以在浏览器中使用预训练的 Tensorflow 或 Keras 模型。Tensorflow.js 还可以使用传感器数据重新训练预先存在的模型-连接到浏览器。

想了解更多关于 Tensorflow.js 的内容，可以查看官方文档。

[](https://www.tensorflow.org/js) [## 面向 Javascript 开发人员的机器学习

### 在浏览器、Node.js 或 Google 云平台中训练和部署模型。TensorFlow.js 是开源的 ML 平台…

www.tensorflow.org](https://www.tensorflow.org/js) 

要获得预训练的模型，您可以克隆以下官方 tfjs GitHub 库

[](https://github.com/tensorflow/tfjs-models) [## 张量流/tfjs-模型

### 该存储库托管一组已移植到 TensorFlow.js 的预训练模型。这些模型托管在 NPM 上…

github.com](https://github.com/tensorflow/tfjs-models) 

# 先决条件

*   **一个文本编辑器(可选)(Sublime，括号，Visual Studio 代码等)。)**

您可以从下载**括号**编辑器

[](http://brackets.io/) [## 一个现代的、理解网页设计的开源代码编辑器

### 括号是一个轻量级的，但功能强大的，现代的文本编辑器。我们将可视化工具融入到编辑器中，因此您可以获得正确的…

括号. io](http://brackets.io/) 

*   **谷歌浏览器**

[](https://www.google.com/chrome/) [## 谷歌浏览器-从谷歌下载快速、安全的浏览器

### 现在比以往任何时候都更简单、更安全、更快速——内置了 Google 的智能。

www.google.com](https://www.google.com/chrome/) 

# 履行

要使用基于浏览器的人脸检测器，请查看我的 GitHub 库

[](https://github.com/sid0312/tfjs-face_detection) [## sid0312/tfjs-face_detection

### 使用 tensor flow . js-sid 0312/tfjs-face _ detection 在浏览器上使用网络摄像头进行人脸检测

github.co](https://github.com/sid0312/tfjs-face_detection) 

# 复制步骤

> 如果你想亲自动手做一些事情，这里有一个指南

*   **创建一个起始 HTML 文件**

*   **将以下几行添加到 html 文件 tensorflow.js 头中，以在 head 标签中导入 tfjs 模型**

```
<script src=”https://cdn.jsdelivr.net/npm/@tensorflow/tfjs"></script><script src=”https://cdn.jsdelivr.net/npm/@tensorflow-models/blazeface">
</script>
```

*   样式(可选):创建用于格式化的 div 并添加样式表链接。添加视频元素和画布
*   **添加加载模型的 JavaScript 文件链接**
*   将你的文件重命名为 index.html

您的 index.html 文件应该如下所示

*   为一些可选的样式创建一个 main.css 文件。向其中添加以下代码。方便时更换

您可以从以下网址获得一些可爱的 CSS

[](https://codepen.io/) [## 密码笔

### 一个在线代码编辑器、学习环境和社区，用于使用 HTML、CSS 和 JavaScript 的前端 web 开发…

codepen.io](https://codepen.io/) 

*注意:main.css 加载一个 GIF，这是用于初始加载的，包含在存储库中。请随意使用自己的 gif。*

*   **创建一个 blaze_pred.js 文件。向其中添加以下代码。**

*   **解释**

我们创建了一个自我调用函数，在这个函数中，我们通过它们的 id 来获取画布和视频元素。为了获得画布的上下文，我们使用 getContext()方法。

使用 navigator.getUserMedia 方法接收视频源，并将流添加到视频源对象。

video.play()用于播放视频，但是由于我们已经在 index.html 文件的 video 元素中将 visibility 设置为 false，因此无法实际查看视频提要。显示的提要是画布。使用画布上下文来绘制面部预测。

为了同步视频提要和 canvas 元素，我们添加了一个事件侦听器，并调用一个函数 draw 来绘制预测。

*   **创建一个异步绘制函数，参数为视频馈送、上下文、画布的宽度和高度。**

在上面绘制视频馈送的当前帧。使用 await blazeface.load()加载模型。将 **returnTensors** 参数设置为 false 时，获取模型 await model.estimateFaces()上的预测。使用预测张量在画布上下文上绘制边界框。使用预测张量获得置信度得分，并将文本添加到相对于边界框的画布上下文中。要重复调用 draw 函数，请将超时设置为 250 ms。

*   **将工作文件(index.html，main.css，blaze_pred.js)添加到文件夹中。将文件夹命名为 tfjs-face_detection。你也可以给任何其他的名字。**

# 克隆存储库

如果您还没有完成复制步骤，请在您的 shell/终端/命令提示符下键入以下内容

```
git clone https://github.com/sid0312/tfjs-face_detection
```

# 使用检测器

**对于 Linux 和 Mac 用户，**

```
user@username:~$ cd tfjs-face_detection
user@username:~/tfjs-face_detection$ google-chrome index.html
```

**对于 Windows 用户，**

```
C:\Users\username> cd tfjs-face_detection
C:\Users\username\tfjs-face_detection> index.html
```

**允许访问网络摄像头**

![](img/9d0574ca15609ea62df2c5e94485486a.png)

等待几秒钟，一切都准备好了！

# **结论**

我们已经使用 Tensorflow.js 在浏览器上成功检测到我们的人脸，您可以创建自己的深度学习人脸模型，并使用 tfjs 模型转换器将其转换为 tfjs 模型，以增加其鲁棒性。

![](img/a4a3bcd4fb09b03d73785e59e67161f4.png)

伊恩·斯托弗在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄的照片

# 作者

![](img/83c8c123945041f29e2c4633366ac297.png)

悉达多·巴尔多塔

耶！视频里的那个是我！查看我的其他文章:

[](/segmentation-of-optical-coherence-tomography-images-with-diabetic-macular-edema-the-gluon-82093cfd8e24) [## 基于 U-网的 OCT 图像分割

### 使用 OCT 报告诊断糖尿病性黄斑水肿的深度学习方法。使用 Apache Mxnet 和…

towardsdatascience.com](/segmentation-of-optical-coherence-tomography-images-with-diabetic-macular-edema-the-gluon-82093cfd8e24) [](/counting-people-on-your-webcam-using-gluoncv-and-mxnet-d1a9f05c427d) [## 使用 GluonCV 和 MxNet 计算网络摄像头上的人数

### 介绍

towardsdatascience.com](/counting-people-on-your-webcam-using-gluoncv-and-mxnet-d1a9f05c427d) 

> 快乐的计算机视觉，快乐的深度学习，快乐的部署。下次见！