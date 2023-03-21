# 计算两个地理空间坐标之间的方位角

> 原文：<https://towardsdatascience.com/calculating-the-bearing-between-two-geospatial-coordinates-66203f57e4b4?source=collection_archive---------8----------------------->

当以编程方式调整相机以遵循特定路径时——无论是在谷歌街景还是地理信息系统动画上，程序员可能需要指定两点之间的方位或经纬度坐标。在这篇文章中，我解释了这是如何实现的。

![](img/36b78d4636652ced1994469e98ec5a5b.png)

照片由[丹尼斯·詹斯](https://unsplash.com/@dmjdenise?utm_source=medium&utm_medium=referral)在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 拍摄

# 数学

数学上，点 *a* 和点 *b* 之间的方位角是通过取 X 和 Y 的反正切函数来计算的

```
bearing  = arctan(X,Y)
```

其中 X 和 Y 定义如下:

```
X = cos θb * sin ∆LY = cos θa * sin θb – sin θa * cos θb * cos ∆L
```

这里 *a* 和 *b* 代表两个坐标，它们的前缀由下式给出:

```
L     = Longitude
theta = Latitudeand ∆L is the difference between the Longitudal values of the two points
```

# 程序上

为了将这一逻辑应用于计算机程序，我们从我们的观点开始:

```
a = {'lat': <some value>, 'lon': <some value>}
b = {'lat': <some value>, 'lon': <some value>}
```

接下来，我们通过从 *b* 中减去 *a* 的纵向值得到`∆L`

```
dL = b.lon-a.lon
```

现在我们可以计算我们的 X 和 Y 值:

```
X = cos(b.lat)* sin(dL)
Y = cos(a.lat)*sin(b.lat) - sin(a.lat)*cos(b.lat)* cos(dL)
```

最后，我们可以使用反正切并获得我们的方位:

```
bearing = **arctan2**(X,Y)
```

*注意:该值以弧度为单位！*

## Python 导入

如果使用 Python，您可以在下面的 numpy 导入中重用上面的代码。

```
from numpy import arctan2,random,sin,cos,degrees
```

要将弧度转换成角度，只需在末端应用“角度”功能- `degrees(bearing)`。在我的例子中，我想要北方的学位，因此使用了`((degrees(bearing)+360) % 360)`。

# 结论

这里我们有一个简单的方法来计算两点之间的方位。