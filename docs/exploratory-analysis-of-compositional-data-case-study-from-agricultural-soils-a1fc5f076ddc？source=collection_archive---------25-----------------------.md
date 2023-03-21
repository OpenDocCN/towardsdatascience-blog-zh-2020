# 成分数据的探索性分析:农业土壤的案例研究

> 原文：<https://towardsdatascience.com/exploratory-analysis-of-compositional-data-case-study-from-agricultural-soils-a1fc5f076ddc?source=collection_archive---------25----------------------->

## 组成数据是向量，其中所有的组成部分都是正数，并且受到闭包形式的约束，这意味着它们的总和是一个常数。在本文中，我使用土壤矿物部分作为一个案例研究的探索性分析这样的数据。

![](img/01b1d7e52e3fe3d5e97c3c6c96c9720c.png)

图片由 [Zbysiu Rodak](https://unsplash.com/@zbigniew?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) 提供

# 介绍

## 背景

农业行业的数据驱动决策在满足不断增长的食品、饲料和原材料需求方面发挥着关键作用，同时确保自然资源和环境的可持续利用。例如，精准农业涵盖了广泛的监测技术，包括数据科学、地质统计学和预测分析等跨学科领域，为农民、农学家、土壤科学家和所有其他利益相关者提供了大量信息，这些信息是我们从未见过的，可用于准确监测土壤和作物状况并分析处理方案。

![](img/d78513b55704ac1c1c3f1cd626194865.png)

图片由[若昂·马塞洛·马克斯](https://unsplash.com/@jmm17?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)拍摄

在这方面，在目前市场上可用的许多监测技术中，使用台式或手持设备的激光诱导击穿光谱(LIBS)越来越受欢迎，主要是因为其速度快、样品制备最少且成本相对较低。像在农业和食品工业中广泛使用的近红外光谱一样，LIBS 提供了对土壤化学成分的估计，可用于评估其肥力——即土壤中可用的矿物养分，这是为大田作物推荐肥料的基础。

然而，无论是在实验室还是在野外，测量准确度和精度对被分析的土壤类型非常敏感:细质地、中等质地和粗质地土壤。为了克服这些缺点，可以将土壤筛分成细粒或中等粒级，这样可以消除大颗粒对分析的任何影响。另一个选择是把大颗粒打碎。第三种选择是根据土壤质地调整设备参数并进行单独分析。后者是美国宇航局的 LIBS 仪器所采用的，名为 [ChemCam](/first-order-multivariate-calibration-in-laser-induced-breakdown-spectroscopy-ca5616dd5b38) ，它有一个机载相机来通知纹理，并配备了一个直径为 300-550 微米的聚焦激光束，可以对大颗粒和非常细颗粒大小的火星岩石和土壤进行元素分析。

![](img/dedbe8eee0546e75cfb27398b0ca23ec.png)

美国宇航局火星科学实验室任务的好奇号火星车，使用位于 LIBS 的 ChemCam 仪器分析火星岩石表面的化学成分。图片来源:美国宇航局/JPL 加州理工学院。

在地球上，仍然悬而未决的问题是这些选择将如何影响分析——例如，植物养分的可用性受到包括土壤颗粒大小在内的许多因素的影响——或者它们将如何影响 LIBS 吸引人的特征，如快速响应和低入侵性。在很大程度上，本文将搁置这些问题的细节。相反，主要主题是提供探索性分析的概述，显示土壤颗粒大小如何影响 LIBS 测量或任何基于激光的光谱测量，以便确定未来的挑战，并帮助确定最有效的解决方案。

## 土壤粗密度

对于任何从事农业的人来说，无论规模大小，了解土壤的质地都是非常重要的，因为它会极大地影响土壤养分和持水能力。土壤质地通常由沉降法确定——使用移液管或比重计——提供土壤矿物颗粒的相对比例，称为土壤颗粒大小分离:沙子(2.0-0.05 毫米)、淤泥(0.05-0.002 毫米)和粘土(<0.002 mm). Sandy soils are often referred to as coarse-textured soils whilst clayey soils are referred to as fine-textured soils. The term “coarse” is used because sandy soils are mostly made up of larger or coarse particles. Clayey soils are mainly composed of fine flaky-shape particles. The term “medium” is used for silty and loamy soils. The latter represents soil that does not have a dominant particle size fraction.

![](img/6aa473d35019fdae4857eb27e50ad940.png)

Image by [保罗·莫康](https://unsplash.com/@paulmocan?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)

## 成分数据

众所周知，土壤颗粒大小部分构成了所谓的成分数据，这对其统计分析有很大的影响。事实上，自 20 世纪 80 年代初，在约翰·艾奇逊的工作—[](https://www.jstor.org/stable/2345821?seq=1)**—*成分数据的统计分析之后，成分数据被地质学家和统计学家以及其他人熟知为向量，其中分量是作为整体的一部分的严格正数，这意味着每个分量仅携带相对部分的信息。这些数据通常被称为封闭数据，因为它们的总和是一个常数，例如 1%或 100%。从数学上讲，这意味着一个 *D* 部分的成分数据占据了一个有限的空间，在几何上用( *D* -1)单纯形表示，例如，成分只能从 0%变化到 100%，就像土壤矿物部分的情况一样。*

*因此， *D* 部分组成数据集**𝐗**表示为:*

*![](img/f7fa5ba70bf06b04828c828212fd09c9.png)*

*其中组件***x****j*∀*j*∈{ 1，2，…， *D* }受到以下约束:*

*![](img/53791ae91ca3a5634b03405fb5420ddd.png)*

*例如，对于 *D* = 2 或 1-单形，数据可以表示为一条线段，对于 *D* = 3 或 2-单形，数据可以表示为一个三角形(所谓的三元图，用于将土壤结构等级表示为沙子、淤泥和粘土百分比的函数)。*

# *探索性分析*

*在接下来的几节中，我将采取几个步骤来更好地了解数据集。这些步骤包括:(1)收集有关分析样本性质的信息，(2)计算土壤颗粒大小部分的描述性统计，(3)分析土壤质地对 LIBS 信号的影响，以及(4)可视化 PCA 双标图以发现模式。*

*其余软件包:*

```
*library(data.table)
library(tidyverse)
library(soiltexture)
library(ggtern)
library(compositions)
library(robCompositions)
library(FactoMineR)
library(factoextra)*
```

## *土壤样本*

*我们的数据集包含 LIBS 分析的 172 个土壤样本，其中包含土壤质地信息，即沙子、淤泥和粘土的百分比。*

```
*> soil.data <- fread(file="data.csv") %>% 
   unique() %>% 
   as_tibble() %>%
   str()

Classes ‘tbl_df’,‘tbl’ and 'data.frame': 172 obs. of 7156 variables:
 $ SampleID   : Factor w/172 levels "S18-0001",..: 1 2 3 4 5 ...
 $ Clay       : num  41 35.1 81 69.1 39.1 ...
 $ Silt       : num  30 41.9 12 23.9 41.9 ...
 $ Sand       : num  29 23 7 7 19 19 26.9 ...
 $ 199.3771616: num  659 659 664 663 661 ...
 $ 199.4644141: num  664 668 660 657 662 ...
 $ 199.5516666: num  674 669 660 661 659 ...
 $ 199.6389192: num  663 671 658 667 661 ...
 [list output truncated]*
```

*我们的第一步是收集有关分析样本的信息，以了解数据集的性质和分布。下图显示了一个三元图(或土壤结构三角形),它告诉我们分配给每个样品的结构类别以及它们在不同类别中的分布情况。*

*![](img/dcd96e1aecb6cb3daeb7977dcf864e52.png)*

*三元图用于报告土壤颗粒大小分离的相对比例。*

*然后，我们可以根据土壤样品的名称来确定它们的不同结构类别。根据美国农业部的分类系统，共有 12 种土壤质地类别，土壤质地类别符号见下表。*

```
*> TT.classes.tbl(class.sys="USDA-NCSS.TT") abbr   name              points                          
 [1,] "C"    "clay"            "24, 1, 5, 6, 2"                
 [2,] "SIC"  "silty clay"      "2, 6, 7"                       
 [3,] "SC"   "sandy clay"      "1, 3, 4, 5"                    
 [4,] "CL"   "clay loam"       "5, 4, 10, 11, 12, 6"           
 [5,] "SICL" "silty clay loam" "6, 12, 13, 7"                  
 [6,] "SCL"  "sandy clay loam" "3, 8, 9, 10, 4"                
 [7,] "L"    "loam"            "10, 9, 16, 17, 11"             
 [8,] "SIL"  "silty loam"      "11, 17, 22, 23, 18, 19, 13, 12"
 [9,] "SL"   "sandy loam"      "8, 14, 21, 22, 17, 16, 9"      
[10,] "SI"   "silt"            "18, 23, 26, 19"                
[11,] "LS"   "loamy sand"      "14, 15, 20, 21"                
[12,] "S"    "sand"            "15, 25, 20"*
```

*我们还可以检查分配给土壤样品的不同结构类别的数量。结果是我们有 11 种土壤质地——我们没有砂质粘土。如下图所示，最丰富的是粘壤土、壤土和砂壤土，其次是粉质粘壤土、粉质壤土和粘土。*

```
*soil.texture %>%
 ggplot() +
 geom_bar(aes(x=class, fill=class, weight=num.sample)) +
 geom_text(aes(x=class, y=num.sample, label=num.sample), nudge_x=-0.1, nudge_y=1) +
 scale_fill_hue() +
 labs(x=" ", y="Total number of soil samples") +
 theme_grey(base_size=14) +
 theme(legend.position="none")*
```

*![](img/f273e9449918476f4796316deec6f96a.png)*

*分配给每个质地类别的土壤样本总数。*

*每一类可以根据它们的纹理进行分组，例如粗糙、中等或精细，这对于主成分分析是有用的。*

*![](img/539a10982d8ffa9136f5c08f8fcaa257.png)*

*分为粗、中和细质地的土壤质地类别的数量。*

*此外，我们还可以从下面的三元图中看到，我们的大多数土壤样品具有大致相等比例的沙子、淤泥和粘土大小的颗粒，这解释了不同壤土类型结构在结构类别中的优势。*

```
*soil.data %>%
 ggtern(aes(x=Sand, y=Clay, z=Silt)) + 
    stat_density_tern(aes(fill = ..level.., alpha = ..level..), geom = "polygon") +
    scale_fill_gradient2(low="red", high="blue") +
    theme_bw(base_size=14) +
    guides(color="none", fill="none", alpha="none")*
```

*![](img/2d39edd9625c426be289c37afae0837a.png)*

*密度等值线的可视化。*

## *土壤颗粒级分*

*由于土壤颗粒的成分性质，土壤颗粒数据存在固有的依赖性。应用标准统计方法——例如算术平均值、标准偏差等——无法解释数据的这种受限属性。这就是为什么像探索性数据分析(EDA)一样，开发了一套称为成分数据分析(CoDA)的程序来分析这种数据。在 CoDA 中，对数比变换应用于成分数据，因此数据不受限制，可以在真实空间中取任何值，并且可以使用任何标准统计方法进行分析。不同类型的对数比变换包括:*

*加法对数比变换(alr)
居中对数比变换(clr)
等距对数比变换(ilr)*

*对于 *D* = 3 部分组成数据，加法、居中和等距对数比转换由下式给出*

*![](img/36400af70d1eca963bcdd1ec6be961ec.png)*

*因此，下文通过箱线图矩阵总结了土壤颗粒大小部分的描述性统计数据，其中每个箱线图都是成对的对数比。*

*![](img/028de7b6634ff84667fd3ba142d03cf7.png)*

*成对对数比的箱线图矩阵。*

*或者，我们可以计算一些描述性的统计数据。*

```
*> comp.data %>% mean()      # Compositional (geometric) mean
Clay      Silt      Sand 
0.2413    0.3579    0.4008> comp.data %>% variation() # Variation matrix
     Clay      Silt      Sand
Clay 0.0000    0.4174    1.2623
Silt 0.4174    0.0000    0.9059
Sand 1.2623    0.9059    0.0000> comp.data %>% var()       # Variance matrix of the clr-transform      
      Clay      Silt     Sand
Clay  0.3074    0.0927  -0.4001
Silt  0.0927    0.2371  -0.3298
Sand -0.4001   -0.3298   0.7298*
```

## *土壤质地对 LIBS 光谱的影响*

*在查看了我们的土壤样本及其结构数据后，我现在将考虑 LIBS 光谱数据。这里，要考虑的两个最重要的参数是光谱的整体强度及其基于重复测量的相对标准偏差——我们对每个分析样品进行了 24 次重复测量。*

*下图说明了从不同土壤质地类别获得的平均强度，并根据土壤质地类型进行分组。*

```
*spec.class %>%
  arrange(norm.int) %>%
  mutate(class=factor(class, levels=class)) %>%
  ggplot(aes(x=class, y=norm.int, group=texture, color=texture)) +
  geom_point() +
  geom_line() +
  labs(x=" ", y="Normalized averaged intensity") +
  theme_bw(base_size=14)*
```

*![](img/3eb9ccef11497be07f2250999044692d.png)*

*作为土壤质地函数的 LIBS 光谱的标准化平均强度。*

*类似地，比较信号的可变性。*

*![](img/a517b18bed0304c8606aa55bafc34618.png)*

*作为土壤质地函数的 LIBS 信号的相对标准偏差。*

## *主成分分析*

*对光谱数据进行 PCA，其清楚地显示了样品按结构类型聚类，尽管按结构类别聚类不太明显。*

```
*set.seed(1234)
pca.model <- soil.data[ ,-1] %>%
  select(Clay, Silt, Sand, colnames(spec.diff[selec.wave])) %>%
  PCA(scale.unit=TRUE, graph=FALSE)fviz_pca_biplot(pca.model,
                axes        = c(1,3),
                geom.ind    = "point",
                geom.var    = c("arrow", "text"),
                pointshape  = 21,
                pointsize   = 4,
                label       = "var",
                select.var  = list(name=c("Clay", "Silt", "Sand")),
                fill.ind    = pca.data$texture,
                col.ind     = "black",
                col         = "darkred",
                palette     = "jco",
                legend.title= "Texture",
                repel       = TRUE) +
  labs(x="t1 (69.3%)", y="t3 (5.8%)") +
  theme_bw(base_size=14)*
```

*![](img/17e05d727d7a845c38b12200b9a21b7c.png)*

*按土壤质地类型分组的光谱数据的 PCA 双标图。*

*![](img/e8d4c36a7f635de1f2d2cd953f3fef08.png)*

*按土壤结构类别分组的光谱数据的 PCA 双标图。*

# *摘要*

*我提供了一个快速概览，介绍了使用 LIBS 仪器对土壤样品进行元素成分分析的准确性如何受到不同类型土壤质地的影响。我们以前认为，特别注意土壤颗粒大小的不均匀性会产生更好的测量精度。LIBS 受欢迎的原因之一是其速度和多功能性，因此将样本接地是一个最佳的选择，但可能更具理论性。*

**更新:R 代码可从以下 GitHub 链接访问，*[*https://GitHub . com/ChristianGoueguel/EDA-of-composition-Variables*](https://github.com/ChristianGoueguel/EDA-of-Compositional-Variables)*