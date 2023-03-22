# COCO 数据集入门

> 原文：<https://towardsdatascience.com/getting-started-with-coco-dataset-82def99fa0b8?source=collection_archive---------6----------------------->

## 理解计算机视觉常用数据集的格式

# 介绍

COCO ( [官网](https://cocodataset.org/#home) ) dataset，意为“上下文中的常见对象”，是一组具有挑战性的高质量数据集，用于计算机视觉，大多是最先进的神经网络。该名称也用于命名这些数据集使用的格式。

引用 COCO creators:

> COCO 是一个大规模的对象检测、分割和字幕数据集。COCO 有几个特点:
> 
> -对象分割
> 
> -在上下文中识别
> 
> -超像素素材分割
> 
> - 33 万张图片(超过 20 万张)
> 
> -150 万个对象实例
> 
> - 80 个对象类别

该数据集的格式由高级神经网络库自动理解，例如脸书检测器 2 ( [链接](https://github.com/facebookresearch/detectron2))。甚至还有专门为 COCO 格式的数据集构建的工具，例如 [COCO-annotator](https://github.com/jsbroks/coco-annotator) 和 [COCOapi](https://github.com/cocodataset/cocoapi) 。了解数据集的表示方式将有助于使用和修改现有数据集，也有助于创建自定义数据集。具体来说，我们对**注释**文件感兴趣，因为完整的数据集由图像目录和注释文件组成，提供了机器学习算法使用的元数据。

# 你能拿可可怎么办？

实际上有多个 COCO 数据集，每个数据集都是为特定的机器学习任务而制作的，带有额外的数据。3 个最受欢迎的任务是:

*   [对象检测](https://cocodataset.org/#detection-2020) —模型应该得到对象的包围盒，即返回对象类的列表及其周围矩形的坐标；对象(也称为“东西”)是离散的、独立的对象，通常由部分组成，如人和汽车；该任务的官方数据集还包含用于对象分割的附加数据(见下文)

![](img/273198b6c42078b61bd47d5ca1626bd9.png)

[来源](https://en.wikipedia.org/wiki/Object_detection)

*   [对象/实例分割](https://cocodataset.org/#detection-2020) —模型不仅应获得对象(实例/“事物”)的边界框，还应获得分割遮罩，即紧密围绕对象的多边形坐标

![](img/af0438f1a83f8297a5cb5fe65b88a6ae.png)

[来源](https://cocodataset.org/#detection-2020)

*   [素材分割](https://cocodataset.org/#stuff-2019) —模型应该进行对象分割，但不是在单独的对象(“事物”)上，而是在背景连续模式上，如草地或天空

![](img/ced53e78a45d90e195050849c8b2728f.png)

[来源](https://cocodataset.org/#stuff-2019)

在计算机视觉中，这些任务有着巨大的用途，例如自动驾驶车辆(检测人和其他车辆)、基于人工智能的安全(人体检测和/或分割)和对象重新识别(对象分割或使用填充分割去除背景有助于检查对象身份)。

# COCO 数据集格式

## 基本结构和共同要素

COCO annotations 使用的文件格式是 JSON，它以 dictionary(大括号内的键值对，`{…}`)作为顶值。它还可以有列表(括号内的有序项目集合，`[…]`)或嵌套的字典。

基本结构如下:

```
{
  "info": {…},
  "licenses": […],
  "images": […],
  "categories": […],
  "annotations": […]
}
```

让我们仔细看看其中的每一个值。

## “信息”部分

该字典包含关于数据集的元数据。对于官方 COCO 数据集，如下所示:

```
{
  "description": "COCO 2017 Dataset",
  "url": "http://cocodataset.org",
  "version": "1.0",
  "year": 2017,
  "contributor": "COCO Consortium",
  "date_created": "2017/09/01"
}
```

如你所见，它只包含基本信息，带有指向数据集官方网站的`"url"`值(例如 UCI 存储库页面或在一个单独的域中)。在机器学习数据集中，这是一种常见的事情，指向他们的网站以获得额外的信息，例如，数据是如何和何时获得的。

## **“许可证”部分**

以下是数据集中图像许可证的链接，例如具有以下结构的知识共享许可证:

```
[
  {
    "url": "http://creativecommons.org/licenses/by-nc-sa/2.0/", 
    "id": 1, 
    "name": "Attribution-NonCommercial-ShareAlike License"
  },
  {
    "url": "http://creativecommons.org/licenses/by-nc/2.0/", 
    "id": 2, 
    "name": "Attribution-NonCommercial License"
  },
  …
]
```

这里需要注意的重要一点是`"id"`字段——`"images"`字典中的每个图像都应该指定其许可证的“id”。

当使用图片时，确保你没有违反它的许可——全文可以在 URL 下找到。

如果您决定创建自己的数据集，请为每个图像分配适当的许可-如果您不确定，最好不要使用该图像。

## **“图像”部分**

可以说是第二重要的，这个字典包含了关于图像的元数据:

```
{
  "license": 3,
  "file_name": "000000391895.jpg",
  "coco_url": "http://images.cocodataset.org/train2017/000000391895.jpg",
  "height": 360,
  "width": 640,
  "date_captured": "2013–11–14 11:18:45",
  "flickr_url": "http://farm9.staticflickr.com/8186/8119368305_4e622c8349_z.jpg",
  "id": 391895
}
```

让我们一个接一个地浏览这些字段:

*   `"license"`:来自`"licenses"` 部分的图像许可证的 ID
*   `"file_name"`:图像目录下的文件名
*   `"coco_url"`、`"flickr_url"`:在线托管映像副本的 URL
*   `"height"`，`"width"`:图像的大小，在 C 语言这样的低级语言中非常方便，因为在 C 语言中获取矩阵的大小是不可能的或者很困难的
*   照片拍摄的时间

最重要的字段是`"id"`字段。这是在`"annotations"`中用来识别图像的编号，所以如果你想识别给定图像文件的注释，你必须在`"images"`中检查`"id"`中合适的图像文件，然后在`"annotations"`中交叉引用它。

在官方 COCO 数据集中，`"id"`与`"file_name"`相同(去掉前导零之后)。请注意，自定义 COCO 数据集可能不一定如此！这不是一个强制规则，例如，由私人照片制作的数据集可能具有与`"id"`毫无共同之处的原始照片名称。

## **“类别”部分**

这个部分对于对象检测和分割任务和对于填充分割任务有点不同。

**物体检测/物体分割:**

```
[
  {"supercategory": "person", "id": 1, "name": "person"},
  {"supercategory": "vehicle", "id": 2, "name": "bicycle"},
  {"supercategory": "vehicle", "id": 3, "name": "car"},
  …
  {"supercategory": "indoor", "id": 90, "name": "toothbrush"}
]
```

这些是可以在图像上检测到的对象的类别(COCO 中的`"categories"`是类别的另一个名称，您可以从有监督的机器学习中知道)。

每个类别都有一个唯一的`"id"`，它们应该在范围[1，类别数]内。类别也分组在“超级类别”中，你可以在你的程序中使用，例如，当你不在乎是自行车、汽车还是卡车时，检测一般的车辆。

**素材分割:**

```
[
  {"supercategory": "textile", "id": 92, "name": "banner"},
  {"supercategory": "textile", "id": 93, "name": "blanket"},
  …
  {"supercategory": "other", "id": 183, "name": "other"}
]
```

类别数开始很高，以避免与对象分割的冲突，因为有时这些任务可以一起执行(所谓的 [*全景分割*](https://cocodataset.org/#panoptic-2020) 任务，也具有非常具有挑战性的 COCO 数据集)。ID 从 92 到 182 是实际的背景材料，而 ID 183 代表所有其他没有单独类别的背景纹理。

**“注释”部分**

这是数据集最重要的部分，包含特定 COCO 数据集每个任务的重要信息。

```
{
  "segmentation":
  [[
    239.97,
    260.24,
    222.04,
    …
  ]],
  "area": 2765.1486500000005,
  "iscrowd": 0,
  "image_id": 558840,
  "bbox":
  [
    199.84,
    200.46,
    77.71,
    70.88
  ],
  "category_id": 58,
  "id": 156
}
```

*   `"segmentation"`:分割掩模像素的列表；这是一个扁平的对列表，所以你应该取第一个和第二个值(图中的 x 和 y)，然后是第三个和第四个值，依此类推。得到坐标；注意，这些不是图像索引，因为它们是浮点数——它们是由 COCO-annotator 等工具从原始像素坐标创建和压缩的
*   `"area"`:分割掩模内的像素数
*   `"iscrowd"`:标注是针对单个对象(值 0)，还是针对彼此靠近的多个对象(值 1)；对于材料分段，该字段始终为 0 并被忽略
*   `"image_id"`:“图像”字典中的“id”字段；**警告:**该值应该用于与其他字典交叉引用图像，而不是`"id"`字段！
*   `"bbox"`:边框，即物体周围矩形的坐标(左上 x，左上 y，宽度，高度)；从图像中提取单个对象非常有用，因为在很多语言中，比如 Python，可以通过访问图像数组来实现，比如`cropped_object = image[bbox[0]:bbox[0] + bbox[2], bbox[1]:bbox[1] + bbox[3]]`
*   `"category_id"`:对象的类别，对应于`"categories"`中的`"id"`字段
*   `"id"`:标注的唯一标识符；**警告:**这只是注释 ID，并不指向其他字典中的特定图像！

在处理群组图像(`"iscrowd": 1`)时，`"segmentation"`部分可能略有不同:

```
"segmentation":
{
  "counts": [179,27,392,41,…,55,20],
  "size": [426,640]
}
```

这是因为对于许多像素来说，明确列出所有像素来创建分段掩码将占用大量空间。相反，COCO 使用自定义游程编码(RLE)压缩，这是非常有效的，因为分段掩码是二进制的，只有 0 和 1 的 RLE 可以多次减小大小。

# **总结**

我们已经探索了 COCO 数据集格式，用于最流行的任务:对象检测、对象分割和材料分割。官方 COCO 数据集质量高，规模大，适合初学者项目、生产环境和最先进的研究。我希望这篇文章能帮助您理解如何解释这种格式，并在您的 ML 应用程序中使用它。