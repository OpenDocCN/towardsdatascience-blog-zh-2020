# 医疗保健中的深度学习— X 射线成像(第 2 部分—了解 X 射线图像)

> 原文：<https://towardsdatascience.com/deep-learning-in-healthcare-x-ray-imaging-part-2-understanding-x-ray-images-b8c6155cd51d?source=collection_archive---------27----------------------->

## 这是深度学习在 X 射线成像上的应用的第二部分。这里的重点是理解 X 射线图像，特别是胸部 X 射线。

# 解读胸部 x 光片:

![](img/3b9c93133ecf89aee1eb107fd102fb8e.png)

图一。胸部 x 光——1)肺部，2)右半膈肌，3)左半膈肌，4)右心房，5)左心房(作者迭戈·格雷兹——radiografía _ pulmonaes _ Francis ca _ lor ca . jpg，CC BY-SA 3.0，[https://commons.wikimedia.org/w/index.php?curid=10302947](https://commons.wikimedia.org/w/index.php?curid=10302947)。作者编辑)

x 射线图像是灰度图像，即图像具有一些暗的像素和一些亮的像素。在医学成像术语中，这些图像具有范围从 0 到 255 的值，其中 0 对应于完全暗的像素，255 对应于完全白的像素。

![](img/129293fe27613e723138f6d95e7b1bd3.png)

图二。灰度条

X 射线图像上的不同值对应不同的密度区域:

1.  深色-身体上充满空气的位置将显示为黑色。
2.  深灰色——皮下组织或脂肪
3.  浅灰色——像心脏和血管这样的软组织
4.  灰白色——像肋骨这样的骨头
5.  亮白色——存在金属物体，如起搏器或除颤器

医生解读图像的方式是通过观察不同密度之间的边界。如图 1 所示，肋骨呈灰白色，因为它们是致密的组织，但由于肺部充满空气，因此肺部呈黑色。类似地，肺下面是半膈，它也是软组织，因此呈现浅灰色。这有助于我们清楚地了解肺部的位置和范围。

因此，如果两个具有不同密度的物体彼此靠近，它们可以在 X 射线图像中被区分开来。

现在，如果肺部发生了什么事情，比如肺炎，那么，空气密度大的肺部会变成水密度大的肺部。这将导致分界线褪色，因为像素密度将开始接近灰度条。

进行胸部 X 射线检查时，通常要求患者站立，并从前到后(前-后)或从后到前(后-前)拍摄 X 射线。

## 使用胸部 x 光可以区分的各种解剖结构:

![](img/221a3ab782478b50c60b7ad0b4109238.png)

图二。解剖气道— 1)气管，2)右主支气管，3)左主支气管(作者 Diego Grez—radiografía _ pulmones _ Francis ca _ lor ca . jpg，CC BY-SA 3.0，【https://commons.wikimedia.org/w/index.php?curid=10302947】T2，作者编辑)

![](img/8ce16d6c4f9bfcc7520a31c87a47bffc.png)

图 3。解剖膈肌和胸膜——1)胸膜，2)右半膈肌，3)左半膈肌(作者迭戈·格雷兹(Diego Grez——radiografía _ pulme es _ Francis ca _ lor ca . jpg，CC BY-SA 3.0，[https://commons.wikimedia.org/w/index.php?curid=10302947](https://commons.wikimedia.org/w/index.php?curid=10302947)，作者编辑)

![](img/8c1b1f29a4e9dd1becc93425e3bd7bf3.png)

图 4。解剖骨骼——1)锁骨，2)后肋，3)前肋，4)椎体(作者 Diego Grez——radiografía _ pulme es _ Francis ca _ lor ca . jpg，CC BY-SA 3.0，【https://commons.wikimedia.org/w/index.php?curid=10302947】，作者编辑)

![](img/e22eb860063bac25a843282f8ebbca8e.png)

图 5。胸部 x 光片上的心脏解剖(作者 Mikael hgg strm，M.D.-作者信息-重复使用图片使用 ZooFari、Stillwaterising 和 Gray's Anatomy creators 的源图片-编辑文件:ZooFari 的 Heart diagram-en . SVG(attribute-Share like 3.0 un ported license)。文件:气管(透明)。png(来自《实习医生格蕾》,公共领域)文件:Chest _ x ray _ PA _ 3–8–2010 . png by Stillwaterising(公共领域)静脉系统的进一步概述:(2011)。"一篇绘画性文章:重症监护室的导管放射学"。印度放射学和成像杂志 21 (3): 182。DOI:10.4103/0971–3026.85365。ISSN 0971–3026。合成:米凯尔·海格斯特伦，CC BY-SA 3.0，【https://commons.wikimedia.org/w/index.php?curid=55155319】T4

以上图像的目的是显示对于一个没有受过训练的医生来说，从胸部 x 光图像中解释不同的解剖结构是多么困难，更不用说任何异常了。

这正是深度学习派上用场的地方。通过深度学习，即使是对身体的不同异常或解剖结构一无所知的人，也可以在预测各种异常时得出令人满意的结果。当然，如果没有庞大的数据集或来自训练有素的医生的地面真实数据，这些都是不可能的，但尽管如此，自我训练算法甚至可以得出这样的结果这一事实令人困惑。

在接下来的几个部分中，我们将使用 Python、卷积神经网络深入研究 X 射线图像可视化，并研究它们如何训练，并得出预测。

参考资料:

1.  Fred A. Mettler、Walter Huda、Terry T. Yoshizumi、Mahadevappa Mahesh:“放射学和诊断核医学中的有效剂量:目录”——放射学 2008 年；248:254–263
2.  [“胸部 X 线质量—投影”](http://radiologymasterclass.co.uk/tutorials/chest/chest_quality/chest_xray_quality_projection.html#top_forth_img)。*放射学大师级*。检索于 2016 年 1 月 27 日。
3.  [胸透](http://www.brooksidepress.org/Products/OBGYN_101/MyDocuments4/Xray/Chest/ChestXray.htm)，OB-GYN 101:产科入门&妇科。2003 年、2004 年、2005 年、2008 年 Brookside Associates 有限公司医学教育部门，2010 年 2 月 9 日检索。