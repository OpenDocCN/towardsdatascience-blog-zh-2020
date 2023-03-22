# 使用 JavaScript 检测人脸特征并应用滤镜

> 原文：<https://towardsdatascience.com/detecting-face-features-and-applying-filters-with-javascript-34a2081daebb?source=collection_archive---------67----------------------->

## 使用 JavaScript 构建类似 Snapchat 的过滤器

![](img/c5d95d30282a34dd14b809c1be7d5c81.png)

从应用程序中截取—背景照片由 [Théo rql](https://unsplash.com/@theorql?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) 在 [Unsplash](https://unsplash.com/?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) 上拍摄

几天前我发表了一篇关于“用 Python 检测面部特征”的文章，我在 Twitter[上收到了很多关于如何用 JavaScript 做这件事的问题。今天我们将回答这个问题，我们将添加一些额外的东西，比如用蜘蛛侠滤镜或者经典的狗滤镜来遮盖你的脸。参与这个项目非常有趣，我希望你会喜欢。](https://twitter.com/livecodestream)

本文将涵盖两个主要主题:

*   人脸特征识别
*   添加过滤器

# 如何检测面部特征？

类似于 DLib 的工作方式，对于 JavaScript，我们有一个名为 [clmtrackr](https://github.com/auduno/clmtrackr) 的库，它将完成检测人脸在图像上的位置的繁重工作，并且还将识别人脸特征，如鼻子、嘴、眼睛等。

该库提供了一些通用模型，这些模型已经过预训练，可以按照如下特征编号使用:

点地图

当我们用这个库处理一个图像时，它将为地图上的每个点返回一个数组，其中每个点由它在`x`和`y`轴上的位置来标识。当我们构建过滤器时，这将变得非常重要。你可能已经猜到了，如果我们想要画一些东西来代替人的鼻子，我们可以使用鼻子的中心点`62`。

不过，说够了，让我们开始研究一些很酷的东西吧！

# 我们在建造什么？

在本文中，我们将利用`clmtrackr`来识别视频流中的人脸(在我们的例子中是网络摄像头或摄像机),并应用可以通过屏幕上的下拉菜单选择的自定义过滤器。这是 codepen 上的应用程序演示(请确保您在浏览器中允许应用程序访问相机，否则它将无法工作):

厉害！它可能不完美，但看起来很神奇！

让我们分解代码并解释我们正在做什么。

# 基本代码结构

为了构建应用程序，我们使用了 [p5.js](https://p5js.org/) 库，这是一个 JavaScript 库，主要用于处理 canvas，非常适合我们的用例。P5JS 不是你传统的 UI 库，它使用事件来定义什么时候构建 UI，什么时候更新 UI。类似于一些游戏引擎。

我想介绍 p5 的 3 个主要事件:

*   `preload`:在库加载之后，构建任何 UI 或在屏幕上绘制任何东西之前执行。这使得加载资产变得非常完美。
*   `setup`:也是在`preload`之后执行一次，我们在这里准备一切并构建初始 UI
*   `draw`:循环调用的函数，每次系统需要渲染屏幕时执行。

# 事先装好

根据定义，我们将使用`preload`事件来加载我们将在后面的代码中使用的图像，如下所示:

```
function preload() {
    // Spiderman Mask Filter asset
    imgSpidermanMask = loadImage("https://i.ibb.co/9HB2sSv/spiderman-mask-1.png");

    // Dog Face Filter assets
    imgDogEarRight = loadImage("https://i.ibb.co/bFJf33z/dog-ear-right.png");
    imgDogEarLeft = loadImage("https://i.ibb.co/dggwZ1q/dog-ear-left.png");
    imgDogNose = loadImage("https://i.ibb.co/PWYGkw1/dog-nose.png");
}
```

很简单。如您所料，p5 中的函数`loadImage`将加载图像并使其作为 P5 图像对象可用。

# 设置

这里的事情变得更有趣了，因为它是我们加载 UI 的地方。我们将把在这个事件中执行的代码分成 4 个部分

# 创建画布

因为我们希望我们的代码具有响应性，所以我们的画布将具有动态大小，该大小将根据窗口大小并使用 4:3 的纵横比来计算。在代码中使用这样的纵横比并不理想，但是我们会做一些假设，以保持代码简洁，便于演示。在我们知道画布的尺寸后，我们可以用 P5 函数`createCanvas`创建一个，如下所示。

```
const maxWidth = Math.min(windowWidth, windowHeight);
pixelDensity(1);
outputWidth = maxWidth;
outputHeight = maxWidth * 0.75; // 4:3createCanvas(outputWidth, outputHeight);
```

# 捕获视频流

在我们的画布工作后，我们需要从网络摄像头或摄像机捕捉视频流，并将其放入画布，幸运的是 P5 通过`videoCapture`函数可以很容易地做到这一点。

```
// webcam capture
videoInput = createCapture(VIDEO);
videoInput.size(outputWidth, outputHeight);
videoInput.hide();
```

# 构建过滤器选择器

我们的应用程序很棒，可以为多个过滤器提供选项，所以我们需要建立一种方法来选择我们想要激活的过滤器。同样…我们可以在这里变得非常有趣，然而，为了简单起见，我们将使用一个简单的下拉菜单，我们可以使用 P5 `createSelect()`函数来创建它。

```
// select filter
const sel = createSelect();
const selectList = ['Spiderman Mask', 'Dog Filter']; // list of filters
sel.option('Select Filter', -1); // Default no filter
for (let i = 0; i < selectList.length; i++)
{
    sel.option(selectList[i], i);
}
sel.changed(applyFilter);
```

# 创建图像跟踪器

图像跟踪器是一个可以附加到视频馈送的对象，它将为每一帧识别所有的脸及其特征。对于给定的视频源，跟踪器需要设置一次。

```
// tracker
faceTracker = new clm.tracker();
faceTracker.init();
faceTracker.start(videoInput.elt);
```

# 绘制视频和滤镜

现在一切都设置好了，我们需要从 P5 更新我们的`draw`事件，将视频源输出到画布，并应用任何选中的过滤器。在我们的例子中,`draw`函数将非常简单，将复杂性推到每个过滤器定义中。

```
function draw() {
  image(videoInput, 0, 0, outputWidth, outputHeight); // render video from webcam // apply filter based on choice
  switch(selected)
  {
    case '-1': break;
    case '0': drawSpidermanMask(); break;
    case '1': drawDogFace(); break;
  }
}
```

# 建立蜘蛛侠面具过滤器

蜘蛛侠面具滤镜

构建过滤器可能是一项简单或非常复杂的任务。这将取决于过滤器应该做什么。对于蜘蛛侠的面具，我们只需要将蜘蛛侠的面具图像放在屏幕的中央。为此，我们首先通过使用`faceTraker.getCurrentPosition()`来确保我们的 faceTracker 对象确实检测到了人脸。

一旦我们检测到我们的面部，我们使用 P5 来使用面部点 62 渲染图像，面部点 62 是作为图像中心的鼻子的中心，并且具有如下表示面部大小的宽度和高度。

```
const positions = faceTracker.getCurrentPosition();
if (positions !== false)
{
    push();
    const wx = Math.abs(positions[13][0] - positions[1][0]) * 1.2; // The width is given by the face width, based on the geometry
    const wy = Math.abs(positions[7][1] - Math.min(positions[16][1], positions[20][1])) * 1.2; // The height is given by the distance from nose to chin, times 2
    translate(-wx/2, -wy/2);
    image(imgSpidermanMask, positions[62][0], positions[62][1], wx, wy); // Show the mask at the center of the face
    pop();
}
```

很酷吧？

现在狗过滤器以同样的方式工作，但是使用三张图片而不是一张，每只耳朵一张，鼻子一张。我不会用更多相同的代码来烦你，但是如果你想检查它，回顾一下 [codepen](https://codepen.io/livecodestream/pen/rNxrMzp) ，它包含了演示的完整代码。

# 结论

在 JavaScript 库的帮助下，识别面部特征并开始构建自己的过滤器是非常容易的。不过，还有一些我们在本教程中没有涉及到的注意事项。比如脸不直对着镜头会怎么样？我们如何扭曲我们的过滤器，使它们符合面部的曲率？或者，如果我想添加 3d 对象而不是 2d 滤镜，该怎么办？

我知道你们中的很多人会用它来做一些很酷的东西，我很想听听你们做了什么，如果你们也能和我分享你们的例子。你可以在推特上找到我。

感谢阅读！