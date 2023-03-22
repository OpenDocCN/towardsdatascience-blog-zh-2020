# 实体解析实用指南—第 4 部分

> 原文：<https://towardsdatascience.com/practical-guide-to-entity-resolution-part-4-299ac89b9415?source=collection_archive---------19----------------------->

## *候选配对生成和初始匹配评分*

![](img/e9fa2cb53df06adf255e5d20f44eb8a3.png)

照片由[艾莉娜·格鲁布尼亚](https://unsplash.com/@alinnnaaaa?utm_source=medium&utm_medium=referral)在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 拍摄

*这是关于实体解析的小型系列的第 4 部分。查看* [*第一部分*](https://yifei-huang.medium.com/practical-guide-to-entity-resolution-part-1-f7893402ea7e)*[*第二部分*](https://yifei-huang.medium.com/practical-guide-to-entity-resolution-part-2-ab6e42572405)*[*第三部分*](https://yifei-huang.medium.com/practical-guide-to-entity-resolution-part-3-1b2c262f50a7) *如果你错过了***

**候选对生成是 ER 的一个相当简单的部分，因为它本质上是阻塞键上的自连接。但是，为了确保实现的高效性和可伸缩性，需要注意一些实际问题:**

1.  **由阻塞连接产生的数据结构可以被建模为图，其中每个记录是节点，候选对连接是边。与更传统的表格结构相比，将它建模为图形对于下游计算来说更有性能，在传统的表格结构中，您将创建相同数据的更多副本。**
2.  **给单个块的大小设定一个上限是个好主意。这可以防止具有非常低特异性的阻塞键创建高度连接的节点，从而导致对性能产生负面影响的非常不对称的分区。阈值取决于特定的应用程序和数据集，但粗略的心理模型是应该解析为单个实体的合理记录数量。在这个产品目录的特殊例子中，我们选择了 100 个，因为很难想象会有超过 100 个不同的产品列表实际上指的是同一个物理产品。**
3.  **对块键执行自连接时，节点的顺序不会改变对，因此所有顺序相反的对都应进行重复数据消除。类似地，自配对也应该被移除，因为没有必要将节点与其自身进行比较。**

**下面是实现候选对生成的 PySpark 代码示例，请注意`GraphFrame`包的使用**

**候选对生成后的下一步是对候选对匹配可能性进行评分。这对于删除不匹配项和创建最终解析的实体至关重要。这一步也是相当开放的，人们可以对要实现的特定评分功能和特征非常有创意。这也是需要迭代来逐步提高特征和匹配精度的地方。通常，这看起来像创建一个简单的第一得分函数，并使用该函数的输出分布来通知迭代的方向。**

**对于示例用例，初始评分功能利用以下指标**

1.  **TF-IDF 和`name, description, manufacturer`的句子编码向量之间的点积或余弦距离**
2.  **`name, description, manufacturer`的规范化字符串之间的 Levenshtein 距离**
3.  **`prices`之间的相似性**
4.  **标记重叠作为标记化的较短字符串长度的一部分`name, description, manufacturer`**

**粗略的第一评分函数是简单地将这些单独的相似性度量相加。下面是实现这个的 PySpark。请注意，这只是起点，我们将详细讨论如何利用机器学习模型来迭代提高匹配精度。将产生的 PySpark 数据帧与单独的相似性度量保持一致是一个好主意，这样可以减少下游迭代的开销。**

**查看第 5 部分的[匹配分数迭代](https://yifei-huang.medium.com/practical-guide-to-entity-resolution-part-5-5ecdd0005470)**