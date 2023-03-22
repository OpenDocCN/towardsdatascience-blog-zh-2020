# 如何在任何赛车游戏中实现你的首次车道检测

> 原文：<https://towardsdatascience.com/how-i-learned-lane-detection-using-asphalt-8-airborne-bae4d0982134?source=collection_archive---------38----------------------->

## 在 Python 中使用这 5 个简单的编码步骤

![](img/1d23baba9c03c783f5b7fc17b29968d9.png)

图片来自 flickr 上的 bagogames

如今，ane 检测和控制已经成为许多车辆的常见功能。此外，这也是任何人朝着自动驾驶方向前进的基本起点。但对于大多数不从事自动驾驶或计算机视觉工作的人来说，从它开始可能看起来比实际情况更令人生畏。

在进行实际的车道检测时，需要进行大量的技术研究。在这种情况下，我们看到了一个实用的视角，因此任何感兴趣的人都可以毫不费力地尝试一下。该理论的详细链接仍在好奇者的相关章节中:那些不满足于实际应用并喜欢深入主题的人。

以下是您开始工作所需的一切:

1.  沥青 8 空降:如果你是 Windows 10 用户，可以到 Windows 商店免费下载:[https://www . Microsoft . com/en-us/p/asphalt-8-空降/9wzdncrfj12h？activetab=pivot:overviewtab](https://www.microsoft.com/en-us/p/asphalt-8-airborne/9wzdncrfj12h?activetab=pivot:overviewtab)
2.  Python 3.7:您可以安装最新版本的 Anaconda，大多数必需的包都已经捆绑好，随时可以使用。这里是下载最新版本的链接:【https://www.anaconda.com/products/individual
3.  OpenCV:这是一个主要针对实时计算机视觉的库。你可以在这里找到关于如何安装和使用它的文档:[https://www.learnopencv.com/install-opencv3-on-windows/](https://www.learnopencv.com/install-opencv3-on-windows/)

既然我们需要的都有了，那就让我们切入正题吧！

**第一步:找到进入游戏屏幕的方法**

这个很简单。我做了一个快速的谷歌搜索，查看可以用来访问屏幕的 python 代码。这是一个非常棒的教程，我使用了其中的基本代码，并针对这个案例进行了修改:

[](https://holypython.com/how-to-use-imagegrab-of-cv2/) [## 如何用 Python-HolyPython.com 捕捉你的屏幕

### 本 Python 教程解释了数字图像的基本原理。您将了解像素如何表示为…

holypython.com](https://holypython.com/how-to-use-imagegrab-of-cv2/) 

如果我们直接运行代码，你会看到一个类似的结果，如下图所示。你会注意到颜色有点不同，屏幕速率导致一些滞后(这对于我们的目的来说是可以的)。

![](img/bed7a54bc08678e636b5b22fa08038b3.png)

让我们在 OpenCV 文档的帮助下纠正颜色部分。cv2 中有一个参数使屏幕记录看起来像实际的颜色(或至少在我的视觉允许的范围内)，即 COLOR_BGR2RGB，这是我在这里用于校正的颜色。还有，时间函数是用来得到屏幕速率的，大概 10 fps 左右，一点都不差！修改后的代码如下所示:

```
# import libraries
from PIL import ImageGrab
import cv2
import numpy as np
import time# for timing
last_time = time.time()# screen capture loop
while True:

    screen = np.array(ImageGrab.grab(bbox=(0,40,800,700)))

    print(f'the screen rate is {(1/(time.time()-last_time))}')
    last_time = time.time()

    cv2.imshow('Python Window', cv2.cvtColor(screen, \
               cv2.COLOR_BGR2RGB)) if cv2.waitKey(25) & 0xFF == ord('q'):
        cv2.destroyAllWindows()
        break
```

为了让上面的代码正确地捕获屏幕，我将游戏窗口最小化到主显示器的左上角，尺寸为 800 x 700。你可以在你的代码中适当地调整它(通过改变上面的 *ImageGrab* 中的 *bbox* )，特别是当你使用多个监视器的时候。

一旦运行此命令，您将获得如下屏幕截图:

![](img/9fdf2e9e3e43ac4a5d8169816a60b853.png)

这种截屏方法非常方便，特别是当我们需要为机器学习用例生成图像时。

现在我们可以捕捉游戏窗口，我们可以进入下一步。

**步骤 2:用于边缘检测的图像处理**

由于车道检测基本上是从图像中识别边缘，所以您必须处理图像以实际获得主要轮廓。计算视觉处理中最常用的方法之一是用于边缘检测的 [Canny 算法](https://en.wikipedia.org/wiki/Canny_edge_detector)。它基本上从图像中提取并保留有用的结构信息，同时大大减小了它的大小。

Open CV 有一个简洁的 [Canny 边缘检测](https://opencv-python-tutroals.readthedocs.io/en/latest/py_tutorials/py_imgproc/py_canny/py_canny.html)的小实现。这进一步降低了图像的复杂性，我们用三行代码就可以得到图像的边缘。

让我们使用 cv2 中的 Canny 实现创建一个小函数来处理图像。您可以通过查看文档来试验各种阈值，代码如下所示:

```
# canny image processing for detecting edgesdef edgeprocessed(image):

    gray_image = cv2.cvtColor(image, cv2.COLOR_BGR2GRAY)
    edgeprocessed_img = cv2.Canny(gray_image,               \
                        threshold1 = 200, threshold2 = 300) return edgeprocessed_img
```

我们必须在图像捕捉循环中再次运行处理过的图像，因此将该循环定义为一个函数以考虑作为输入的处理更有意义。我在这里有点超前了，将它修改为以列表形式输入的通用函数，这在分析中会更有意义。

```
def screen_capture(processing = None):

    # takes in a list for type of processing
    # [processing_function, masking_function]

    last_time = time.time()

    while True:

        screen = np.array(ImageGrab.grab(bbox=(0,40,800,700)))
        count = 0

        if processing is not None:

            for i in processing:
                count += 1

            processed_screen = processing[0](screen) if count > 1:                
                    masked_screen = processing[1] \
                                    (processed_screen,[vertices]) screen_1 = masked_screen

                else:                
                    screen_1 = processed_screen

        else:            
            screen_1 = screen                

        cv2.imshow('Python Window 2', screen_1)
        cv2.imshow('Python Window', cv2.cvtColor(screen, \
                    cv2.COLOR_BGR2RGB))                

        print(f'the screen rate is {(1/(time.time()-last_time))}')
        last_time = time.time()

        if cv2.waitKey(25) & 0xFF == ord('q'):
            cv2.destroyAllWindows()
            break
```

现在使用该功能会好得多。在下一节中，您将会看到我为什么这样构造函数。到那时，让我们检查边缘处理的结果。

```
screen_capture([edgeprocessed])
```

这是你将会看到的:

![](img/c70a716b1781b853d3775d6cef294289.png)

我们几乎可以从游戏屏幕上获得物体的边缘。这是我在游戏中的测试跑道上尝试时的样子:

![](img/fd44d817f01a07a52f0b8a11fb179837.png)

正如您所注意到的，图像中仍然有太多不需要的信息。树木、岩石、电线，基本上是道路地平线以上的一切。更不用说游戏本身的覆盖控件显示了。如果我们想把注意力集中在车道上，我们必须找到一种方法，以某种方式过滤掉或掩盖其余的图像线。

**步骤 3:从图像中屏蔽附加信息**

为此，让我们定义一个掩蔽函数。我们需要的是定义一个多边形来屏蔽图像中的所有信息，除了让我们关注车道的区域。这是一个非常基本的方法，在这种情况下，我用一点试错法定义了我需要的精确区域的顶点，如下图所示:

![](img/8ce740cd6a49c292210b1c984262cedd.png)

所需的区域函数使用一个遮罩帧和一个带顶点的多边形。蒙版和多边形的交集只给了我们所需的图像部分。函数和顶点(如上定义)如下所示:

```
def required_region(image, vertices):

    mask = np.zeros_like(image) # black image        
    cv2.fillPoly(mask, vertices, 255) # fill within vertices
    masked = cv2.bitwise_and(image, mask) # intersection

    return maskedvertices = np.array([[120,500],[120,400],[150,330],[650,330], \
                     [680,400],[680,500],[260,500],[260,450], \ 
                     [325,370],[475,370],[540,450][540,500]], \
                      np.int32)
```

现在，我们也可以用这些输入运行屏幕捕获功能。

```
screen_capture([edgeprocessed, required_region])
```

并且，结果如下:

![](img/1994c92a04b01186b70406bcee50e1d7.png)

正如你现在注意到的，我已经去掉了所有不必要的信息，除了车道。对于一个基本级别的代码，我要说这是非常整洁的！

但是我们还没有完全实现。

**第四步:用霍夫线寻找线条**

为了找到实际的几何线，我们需要对边缘处理后的图像进行某种变换。在这种情况下，我在 Open CV 中使用了[霍夫变换](https://docs.opencv.org/3.4/d9/db0/tutorial_hough_lines.html)实现。为了正常工作，图像在作为 Hough 变换的输入之前必须被模糊一点。在这种情况下，我使用了来自 Open CV 的[高斯模糊](https://opencv-python-tutroals.readthedocs.io/en/latest/py_tutorials/py_imgproc/py_filtering/py_filtering.html)。

此外，需要在图像帧上绘制检测到的线。所以我又定义了一个函数来覆盖帧上检测到的线。

修改的覆盖线和边缘处理代码是:

```
def overlay_lines(image, lines):

    for line in lines:
        coordinates = line[0]
        cv2.line(image,(coordinates[0],coordinates[1]), \
                (coordinates[2],coordinates[3]),[255,255,255],3)def edgeprocessed(image):

    gray_image = cv2.cvtColor(image, cv2.COLOR_BGR2GRAY)    

    edgeprocessed_img = cv2.Canny(gray_image, threshold1 = 200,\
                                  threshold2 = 300)    

    edgeprocessed_img = cv2.GaussianBlur(edgeprocessed_img,(5,5),0)

    lines = cv2.HoughLinesP(edgeprocessed_img, 1, np.pi/180, \
                            180, np.array([]), 100, 5)

    overlay_lines(edgeprocessed_img, lines)

    return edgeprocessed_img
```

![](img/a471d8031c5762baecbbd3960b6173f1.png)

如您所见，这一点也不差，而且只需要几行代码！我们快到了。

**第五步:获取车道**

在前面的步骤中检测到了各种线条。我们必须找到一种方法，以某种方式合并它们，这样我们就可以得到实际的车道。这有点复杂，我在网上查阅了一些资源，看看是否有更简单的方法。我在 Github 项目[这里](https://codynicholson.github.io/Finding_Lane_Lines_Project/)中发现了一个很棒的*绘制车道*函数，我在这里直接使用了这个函数(代码太长，所以没有在下面添加，但是你可以参考我在 Github 中的项目笔记本，链接在最后)。所有需要做的就是修改边缘处理函数如下:

```
def edgeprocessed(image):

    original_image = image

    gray_image = cv2.cvtColor(original_image, cv2.COLOR_BGR2GRAY)
    edgeprocessed_img = cv2.Canny(gray_image, threshold1 = 200, \
                                  threshold2 = 300)    
    edgeprocessed_img = cv2.GaussianBlur(edgeprocessed_img,(5,5),0)
    edgeprocessed_img = required_region(edgeprocessed_img, \
                                        [vertices])    
    lines = cv2.HoughLinesP(edgeprocessed_img, 1, np.pi/180, 180, \
                            np.array([]), 100, 5)
    #overlay_lines(edgeprocessed_img, lines)
    m1 = 0
    m2 = 0

    try:
        l1, l2, m1, m2 = draw_lines(original_image,lines)

        cv2.line(original_image, (l1[0], l1[1]), \
                 (l1[2], l1[3]), [0,255,0], 30)

        cv2.line(original_image, (l2[0], l2[1]), \
                 (l2[2], l2[3]), [0,255,0], 30)

    except Exception as e:
        pass try:
        for coords in lines:
            coords = coords[0]
            try:
                cv2.line(edgeprocessed_img, 
                         (coords[0], coords[1]), 
                         (coords[2], coords[3]), [255,0,0], 2)

            except Exception as e:
                print(str(e))
    except Exception as e:
        passreturn edgeprocessed_img,original_image, m1, m2
```

并相应地对捕获功能进行了一些调整:

```
def screen_capture(processing = None):

    # takes in a list for type of processing
    # [processing_function, masking_function]

    last_time = time.time()

    while True:

        screen = np.array(ImageGrab.grab(bbox=(0,40,800,700)))
        count = 0

        if processing is not None:

            for i in processing:
                count += 1

            processed_screen ,original_image, m1, m2 = \
                                           processing[0](screen) if count > 1:                
                    masked_screen = processing[1] \
                                    (processed_screen,[vertices]) screen_1 = masked_screen

                else:                
                    screen_1 = processed_screen

        else:            
            screen_1 = screen                

        cv2.imshow('Python Window 2', screen_1)
        cv2.imshow('Python Window', cv2.cvtColor(screen, \
                    cv2.COLOR_BGR2RGB))                

        print(f'the screen rate is {(1/(time.time()-last_time))}')
        last_time = time.time()

        if cv2.waitKey(25) & 0xFF == ord('q'):
            cv2.destroyAllWindows()
            break
```

瞧啊。

```
screen_capture([edgeprocessed])
```

![](img/44cc26e278958a1c2b0a5c8154a9a4b1.png)

正如你所看到的，仍然有更多的调整和微调可以做。但作为入门学习项目，这是一个很棒的应用程序！

有趣的是，整个事情现场工作！您可以在运行代码的同时玩游戏和检测车道。这给了我们很多可能性，比如为 ML 保存图像，游戏控制等等。我将在以后的文章中探讨这个问题。

就这样，伙计们！我希望你在这 5 个简单的步骤中实现了车道检测。Jupyter 笔记本可以在这个 [***链接***](https://github.com/enKRypted15/Asphalt-8-Lane-Detection.git) ***中找到。***

请随意使用和修改代码，如果您有任何有趣的调整、结果或想法，请告诉我。快乐学习！