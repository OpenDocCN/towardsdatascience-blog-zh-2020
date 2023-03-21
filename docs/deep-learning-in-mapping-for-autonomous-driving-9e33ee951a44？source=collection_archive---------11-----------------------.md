# 自动驾驶地图中的深度学习

> 原文：<https://towardsdatascience.com/deep-learning-in-mapping-for-autonomous-driving-9e33ee951a44?source=collection_archive---------11----------------------->

更新:自本文撰写以来，有几篇关于深度学习的高清地图构建的新论文。当我有机会的时候，我会更新这个帖子

*   [高清地图网](https://arxiv.org/abs/2107.06307)
*   [面向城市无地图驾驶的分层道路拓扑学习](https://arxiv.org/abs/2104.00084)

深度学习的应用已经在自动驾驶栈的各个组件中进行了探索，例如，在感知、预测和规划中。深度学习还可以用于**映射**，这是更高级自动驾驶的关键组件。

拥有准确的地图对于自动驾驶在路线选择、定位以及轻松感知方面的成功至关重要。可以通过订购商业地图服务来获得具有不同程度信息的地图。然而，在没有地图的地区，自动驾驶车辆需要依靠自己的地图构建能力来确保自动驾驶的功能性和安全性。

## 离线映射与在线映射

在离线绘图场景中，传感器数据被聚集在一个集中的位置。这些数据可能是卫星图像，也可能是相机或激光雷达等机载传感器收集的数据。它可能来自同一辆车在同一地点的多次通行，也可能来自一个车队的众包。地图的渲染是离线构建的，需要人工注释者在地图上注释语义结构并检查最终结果。传统的地图服务以这种离线方式工作，然后将经过注释和管理的地图提供给路上的车辆。

![](img/b7975319342a3c930cdaff963fa33729.png)

高清地图注释的用户界面示例([来源](https://arxiv.org/pdf/2005.03778.pdf))

在线地图是在车辆上进行的，因此不能让人类参与其中。典型的例子是用于同步定位和绘图的 [SLAM 系统](/monocular-dynamic-object-slam-in-autonomous-driving-f12249052bf1)。最近，**语义 SLAM** 专注于道路上的表面标记的几何和语义含义，被探索作为映射的轻量级解决方案。此外，**单目语义在线映射(monoSOM)** 是一个趋势性主题，其中使用神经网络将来自多个相机的单目图像的时间序列融合成语义鸟瞰图。这些话题超出了本文的范围，将在其他地方讨论。

## 标清地图与高清地图

根据输入分辨率的不同，深度学习在映射中的应用大致有两种类型。第一行工作集中于地图拓扑的发现，例如道路网络，并且通常不包含车道级别信息。他们只需要一张分辨率相对较低、精度大约为米级的图像。另一类应用侧重于提取车道级别信息，如车道线、路面箭头和其他语义标记。这需要更高分辨率的图像，具有**厘米级的精度**。相应地，这两种类型的地图在本文的其余部分将被统称为标清地图和高清地图。

![](img/1f589a0c4856bae72acc9a1c98bed231.png)

深度学习在 SD 地图上的应用(左， [**DeepRoadMapper**](https://openaccess.thecvf.com/content_ICCV_2017/papers/Mattyus_DeepRoadMapper_Extracting_Road_ICCV_2017_paper.pdf) )和 HD 地图上的应用(右， [**DAGMapper**](https://openaccess.thecvf.com/content_ICCV_2019/papers/Homayounfar_DAGMapper_Learning_to_Map_by_Discovering_Lane_Topology_ICCV_2019_paper.pdf) )

这篇文章主要关注高清地图的离线生成。请注意，其中一些方法也可以应用于在线制图，我们将专门针对 SD 制图的一些相关工作进行一个简短的回顾。

## 注释者友好的映射

在离线映射中，有一个人类评审员来评审深度学习的结果是负担得起的，也是必不可少的。因此，不仅整体的准确性很重要，而且结果的表示也很重要，因为结果应该很容易被人类注释者修改。

由于大多数运动规划者只能处理结构化的并表示正确拓扑的车道图，映射算法的输出通常是**一个** **结构化表示**(车道线的折线等)。幸运的是，这种表示有助于人工注释者的有效修改。

# SD 映射(道路拓扑发现)

深度学习在地图绘制上的早期应用侧重于从相对低分辨率的航空图像中提取道路级拓扑。深度学习为 SD 映射创建了一个大覆盖范围的负担得起的解决方案。SD 地图中生成的道路拓扑的主要用途在自动驾驶环境中相对有限，用于路线选择和导航。然而，其中一些研究中提出的方法与后来的高清地图工作高度相关，因此在此进行回顾。

[**deep road mapper**](https://openaccess.thecvf.com/content_ICCV_2017/papers/Mattyus_DeepRoadMapper_Extracting_Road_ICCV_2017_paper.pdf)(ICCV 2017)接收从卫星获得的航空图像，并创建结构化的道路网络。它首先执行语义分割，并在生成的道路图上运行细化和修剪算法。由于语义分割的不准确性(被树、建筑物等遮挡)，许多道路是断开的。为了解决这个问题，DeepRoadMapper 使用 [A*搜索算法](https://en.wikipedia.org/wiki/A*_search_algorithm)来生成连接假设以弥合差距。

[**road tracer**](https://openaccess.thecvf.com/content_cvpr_2018/papers/Bastani_RoadTracer_Automatic_Extraction_CVPR_2018_paper.pdf)(CVPR 2018)也注意到了不可靠的语义分割结果，将其作为中间表示进行了剔除。它使用迭代图构造来直接获得道路的拓扑。该算法需要做出决定，朝着某个方向前进一定的距离，类似于强化学习设置中的代理。

![](img/9961ffa1c622b948e440e7c570efaa1c.png)

“代理”在 [RoadTracer](https://openaccess.thecvf.com/content_cvpr_2018/papers/Bastani_RoadTracer_Automatic_Extraction_CVPR_2018_paper.pdf) 中提取道路拓扑

[**poly mapper**](https://openaccess.thecvf.com/content_ICCV_2019/papers/Li_Topological_Map_Extraction_From_Overhead_Images_ICCV_2019_paper.pdf)(ICCV 2019)可能受到 RoadTracer 的启发，它也消除了中间表示。它显式地统一了不同类型对象的形状表示，包括道路和街道建筑物，并将它们表述为封闭的多边形。公式非常巧妙和简洁，遵循[迷宫墙跟随器](https://en.wikipedia.org/wiki/Maze_solving_algorithm#Wall_follower)算法。

![](img/e9eda7254d673f1051aeea75781ce0b6.png)

在 [PolyMapper](https://openaccess.thecvf.com/content_ICCV_2019/papers/Li_Topological_Map_Extraction_From_Overhead_Images_ICCV_2019_paper.pdf) 中使用迷宫墙跟随器对道路拓扑进行顺序化

[PolyMapper](https://openaccess.thecvf.com/content_ICCV_2019/papers/Li_Topological_Map_Extraction_From_Overhead_Images_ICCV_2019_paper.pdf) 使用 Mask RCNN 架构提取建筑物和道路的边界遮罩。基于遮罩，它提取顶点，找到起始顶点，并使用 RNN 自回归迭代所有顶点以形成闭合多边形。

![](img/027fdfdd04be12c4d6291fe319dd9f1c.png)

[聚合地图](https://arxiv.org/abs/1812.01497)的网络架构

在 [RoadTracer](https://openaccess.thecvf.com/content_cvpr_2018/papers/Bastani_RoadTracer_Automatic_Extraction_CVPR_2018_paper.pdf) 和**[poly mapper](https://arxiv.org/abs/1812.01497)**中，地图结构化表示的自回归生成与高清制图中使用的非常相似。****

# ****高清制图(车道等级信息提取)****

****SD 地图缺乏自主汽车安全定位和运动规划所需的细节和准确性。带有车道等级信息的高清地图是自动驾驶所必需的。高清地图生成通常采用更高分辨率的鸟瞰(BEV)图像，通过拼接机载相机图像和/或激光雷达扫描生成。****

****[](https://openaccess.thecvf.com/content_cvpr_2018/papers/Homayounfar_Hierarchical_Recurrent_Attention_CVPR_2018_paper.pdf)****【结构化在线地图的分层递归注意网络，CVPR 2018】取道路的稀疏点云扫掠，输出包含车道边界实例的道路网络的结构化表示。它首先迭代地找到每条车道线的起点，然后对于每条车道线，沿着这条线迭代地绘制下一个顶点。这两个 rnn 以分层的方式组织起来，因此被命名为 HRAN——分层递归注意网络。********

******![](img/9a95efef15737bf32f453b158f460b76.png)******

******分层循环注意力网络([来源](https://openaccess.thecvf.com/content_cvpr_2018/papers/Homayounfar_Hierarchical_Recurrent_Attention_CVPR_2018_paper.pdf))******

******它提出了**折线损失**的思想，以鼓励神经网络输出结构化折线。折线损失测量地面真实折线的边与其预测值的偏差。这比顶点上的距离更合适，因为有许多方法可以绘制等效的折线。******

****![](img/0c5fae57fb768cf29f49a82fbe0e6d4d.png)****

****折线损失(又名倒角距离损失)关注的是形状而不是顶点位置([源](https://openaccess.thecvf.com/content_cvpr_2018/papers/Homayounfar_Hierarchical_Recurrent_Attention_CVPR_2018_paper.pdf))****

****HRAN 使用每像素 5 厘米的分辨率，并在 20 厘米的精度内实现 0.91 召回。主要的失效模式来自于缺失或额外的车道线。注意，100%的准确性不一定是最终目标，因为注释者仍然需要手动检查这些图像并修复它们。这些故障案例可以相对容易地修复。后期作品[深度边界提取器](http://openaccess.thecvf.com/content_CVPR_2019/html/Liang_Convolutional_Recurrent_Network_for_Road_Boundary_Extraction_CVPR_2019_paper.html)中使用的高度渐变图或许可以修复护栏被误认为车道线的 FP 情况。****

****![](img/994d368b115a9958127b09c9c63fe763.png)****

****来自 [HRAN](https://openaccess.thecvf.com/content_cvpr_2018/papers/Homayounfar_Hierarchical_Recurrent_Attention_CVPR_2018_paper.pdf) 的定性结果****

****[**深度结构化人行横道**](https://openaccess.thecvf.com/content_ECCV_2018/papers/Justin_Liang_End-to-End_Deep_Structured_ECCV_2018_paper.pdf) (绘制人行横道的端到端深度结构化模型，ECCV 2018)从 Lidar 点和相机图像(lidar + RGB = 4 通道)产生的 BEV 图像中提取结构化人行横道。该网络生成三个独特的特征地图——语义分割、轮廓检测以及根据直接监控定义人行横道方向的角度。****

> ****天真地说，等高线图和角度/对齐图仅在人行横道边界处具有非零值。这类似于一键编码，并且提供了过于稀疏的监督。为了解决这一问题，等值线图采用了反距离变换(DT)的形式，并且角度图还将非零区域扩展到扩大的等值线带。另一种方法是使用高斯模糊来处理几乎是一个热点的地面真相，就像在 [CenterNet](https://arxiv.org/abs/1904.07850) 中一样。****

****这项工作在某种意义上不完全是端到端的，因为管道的最终目标是生成人行横道的两个结构化的、定向的边界。为了获得结构化的边界，三个中间特征地图连同一个粗地图(OpenStreetMaps，其提供道路中心线和交叉多边形)被输入到能量最大化管道中，以找到人行横道的最佳边界和方向角。****

****![](img/73937048bd15e53e29665f83f921b1e4.png)****

****[深层结构人行横道](https://openaccess.thecvf.com/content_ECCV_2018/papers/Justin_Liang_End-to-End_Deep_Structured_ECCV_2018_paper.pdf)总体管线****

****BEV 输入分辨率为 4cm/像素，整体精度达到 0.96。主要故障模式是油漆质量差，导致不正确的距离变换和分割预测。****

> ****以上结果由几次行驶产生的输入**离线**得出。该模型还可以在同一位置单程运行**在线**，并可以实现类似的性能，在线生成图像质量差的附加故障模式。****

****![](img/19aa8418d6db3fb3e588903b219d794a.png)****

****来自[深层结构人行横道](https://openaccess.thecvf.com/content_ECCV_2018/papers/Justin_Liang_End-to-End_Deep_Structured_ECCV_2018_paper.pdf)的定性结果****

****[**深度边界提取器**](http://openaccess.thecvf.com/content_CVPR_2019/html/Liang_Convolutional_Recurrent_Network_for_Road_Boundary_Extraction_CVPR_2019_paper.html) (道路边界提取卷积递归网络，CVPR 2019)用折线提取道路边界。它受[深度结构化人行横道](https://openaccess.thecvf.com/content_ECCV_2018/papers/Justin_Liang_End-to-End_Deep_Structured_ECCV_2018_paper.pdf)的启发，使用卷积 RNN(卷积蛇，或 cSnake)以自回归方式进行预测。通过增加一个额外的激光雷达高度梯度通道，输入扩展了[深层结构人行横道](https://openaccess.thecvf.com/content_ECCV_2018/papers/Justin_Liang_End-to-End_Deep_Structured_ECCV_2018_paper.pdf)的输入，该通道通过获取 Sobel 滤波激光雷达 BEV 图的幅值生成。****

****cSnake 网络迭代地关注旋转的 ROI，并输出对应于道路边界的折线的顶点。它首先预测终点。基于每个端点，它裁剪和旋转以该端点为中心的特征地图，并定位下一个点的位置。上述过程自回归运行。****

****![](img/fe121348d9d0375d22257d97309459e7.png)****

****[深边界提取器](http://openaccess.thecvf.com/content_CVPR_2019/html/Liang_Convolutional_Recurrent_Network_for_Road_Boundary_Extraction_CVPR_2019_paper.html)整体管线****

****Deep Boundary Extractor 以 4 cm/pixel 的输入分辨率运行，并实现约 0.90 逐点 F1 分数和 0.993 拓扑精度。****

****![](img/9e73e92872060c7c76dcbd67311d63db.png)****

****来自[深边界提取器](http://openaccess.thecvf.com/content_CVPR_2019/html/Liang_Convolutional_Recurrent_Network_for_Road_Boundary_Extraction_CVPR_2019_paper.html)的定性结果****

****[**Dag mapper**](https://openaccess.thecvf.com/content_ICCV_2019/papers/Homayounfar_DAGMapper_Learning_to_Map_by_Discovering_Lane_Topology_ICCV_2019_paper.pdf)**(通过发现车道拓扑来学习地图，ICCV 2019)将 [HRAN](https://openaccess.thecvf.com/content_cvpr_2018/papers/Homayounfar_Hierarchical_Recurrent_Attention_CVPR_2018_paper.pdf) 的结构化车道线提取工作向前推进了一步，并专注于分叉和合并等更难的情况。它接收激光雷达强度图并输出 DAG(有向无环图)，而不是 HRAN 中的简单折线。******

******在 [DAGMapper](https://openaccess.thecvf.com/content_ICCV_2019/papers/Homayounfar_DAGMapper_Learning_to_Map_by_Discovering_Lane_Topology_ICCV_2019_paper.pdf) 的核心还有一个递归卷积头，它迭代地关注以最后预测点为中心的裁剪后的特征地图补丁，并预测下一个点的位置。变化在于它还预测点的状态是合并、分叉还是继续。******

******![](img/57277d46d0081035cf13b5315e1f5ae6.png)******

******[DAGMapper](https://openaccess.thecvf.com/content_ICCV_2019/papers/Homayounfar_DAGMapper_Learning_to_Map_by_Discovering_Lane_Topology_ICCV_2019_paper.pdf) 的整体管线******

******输入分辨率为 5 厘米/像素。精确度/召回率/F1 在 2 像素(10 厘米)阈值处约为 0.76，在 10 像素(50 厘米)处约为 0.96。拓扑精度约为 0.89。******

******![](img/d405808db585f884d388f69e63e89534.png)******

******来自 [DAGMapper](https://openaccess.thecvf.com/content_ICCV_2019/papers/Homayounfar_DAGMapper_Learning_to_Map_by_Discovering_Lane_Topology_ICCV_2019_paper.pdf) 的定性结果******

# ******外卖食品******

*   ******用于离线绘图的深度学习有一个人在循环中。深度学习的结果需要被结构化，以便能够被自动驾驶堆栈使用，并且能够被人类注释者容易地修改。******
*   ******当前的高清制图应用主要集中在道路边界、车道线(包括合并和分叉拓扑)和人行横道边界的提取上。******
*   ******所有 HD 制图研究的核心构建模块是递归卷积网络，它迭代地获取以当前注释点为中心的裁剪后的特征地图，并预测下一个注释点。******

# ******参考******

*   ******[**DeepRoadMapper** :从航拍影像中提取道路拓扑](https://openaccess.thecvf.com/content_ICCV_2017/papers/Mattyus_DeepRoadMapper_Extracting_Road_ICCV_2017_paper.pdf)，ICCV 2017******
*   ****[**RoadTracer** :航拍影像道路网自动提取](https://openaccess.thecvf.com/content_cvpr_2018/papers/Bastani_RoadTracer_Automatic_Extraction_CVPR_2018_paper.pdf)，CVPR 2018****
*   ******PolyMapper** : [从航拍图像中提取拓扑图](https://arxiv.org/abs/1812.01497)，ICCV 2019****
*   ******折线损失** : [结构化在线地图的分层递归注意网络](https://openaccess.thecvf.com/content_cvpr_2018/papers/Homayounfar_Hierarchical_Recurrent_Attention_CVPR_2018_paper.pdf)，CVPR 2018****
*   ******深度结构化人行横道** : [绘制人行横道的端到端深度结构化模型](https://openaccess.thecvf.com/content_ECCV_2018/papers/Justin_Liang_End-to-End_Deep_Structured_ECCV_2018_paper.pdf)，ECCV 2018****
*   ******深度边界提取器**:用于道路边界提取的[卷积递归网络](http://openaccess.thecvf.com/content_CVPR_2019/html/Liang_Convolutional_Recurrent_Network_for_Road_Boundary_Extraction_CVPR_2019_paper.html)，CVPR 2019****
*   ****[**DAGMapper** :通过发现车道拓扑学地图](https://openaccess.thecvf.com/content_ICCV_2019/papers/Homayounfar_DAGMapper_Learning_to_Map_by_Discovering_Lane_Topology_ICCV_2019_paper.pdf)，ICCV 2019****
*   ******稀疏高清地图** : [利用稀疏语义高清地图进行自动驾驶车辆定位](https://arxiv.org/abs/1908.03274)，IROS 2019 口述****