# Oracle 中的约束

> 原文：<https://towardsdatascience.com/constraints-in-oracle-640fed815432?source=collection_archive---------17----------------------->

## ***保证数据完整性的方法***

![](img/36923cf24310bde1486c5e895a346558.png)

照片由[马丁·桑切斯](https://unsplash.com/@martinsanchez?utm_source=medium&utm_medium=referral)在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 拍摄

任何数据库管理系统的核心功能之一是确保数据在其生命周期中的完整性。数据完整性，简单来说，就是随着时间的推移，数据应该保持****【准确】*** 。不一致或损坏的数据对企业来说用处有限或毫无用处，在某些情况下甚至会造成威胁。损害数据完整性的方式有多种，例如在复制或传输过程中、来自多个来源的集成、人为数据输入错误等*

*在 Oracle 中，**“约束条件”**是一种实施规则的工具，以确保只有允许的数据值存储在数据库中。顾名思义，约束对可以存储在数据库表中的数据的类型或值进行限制/检查。Oracle 提供了以下约束来加强数据完整性:*

1.  ***NOT NULL:** 如果根据业务逻辑，表中的一列或一组列不允许空值，那么可以使用 NOT NULL 约束来防止这种情况。*

```
***alter table SALES modify (cust_id NOT NULL);***
```

*如果在销售表的 cust_id 列中输入空值，数据库将抛出错误*

***2。UNIQUE:** 如果根据业务逻辑，一个表中的一列或一组列需要存储唯一的值，那么可以使用 UNIQUE 约束来实施这个规则。*

```
***create table test (col1 number UNIQUE);***
```

> ***唯一约束允许存储空值。***

***3。主键:**主键约束是非空约束和唯一约束的组合。定义主键的一列或一组列只允许唯一值，不能为空值。*

```
***create table test (col1 number PRIMARY KEY);***
```

> ****一个 Oracle 表中只能有一个(且只能有一个)主键。****

***4。外键:**经常需要将一个表中的数据与另一个表中的数据进行比较来验证。(例如，如果您在订单表中添加一个新订单，您必须交叉检查对应于该订单的有效产品是否出现在您的产品表中)。为了实现这种数据完整性，使用了外键约束。这种类型的验证也被称为*。外键约束总是引用其他表的主键或唯一约束。定义了外键的表称为引用表的**。定义了主键或唯一约束的表称为**引用表**。****

```
****create table orders
(
 order_no number primary key,
 customer_name varchar2(10),
 constraint cons_prod_fk foreign key(prod_no) references product(prod_no)
);****
```

**如果使用' ON DELETE CASCADE '选项定义外键约束，则当从被引用表中删除任何行时，相应的行也将从引用表中删除。**

> ****外键列中允许空值。****

****5。CHECK:** Check 约束用于强制数据行的一个或多个检查条件。**

```
****alter table customers add constraint customer_credit_limit CHECK (credit_limit <= 1000)****
```

**这将确保 credit_limit 始终小于或等于 1000。否则，系统将引发错误**

**![](img/17d93ba1c1eec04630e2f886841c7e57.png)**

**[皮斯特亨](https://unsplash.com/@pisitheng?utm_source=medium&utm_medium=referral)在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上的照片**

**在 Oracle 中使用约束时，以下信息会非常有用:**

*   **约束名称存储在 ALL_CONSTRAINTS 表中。定义约束的列名可以在 ALL _ CONS _ 列中找到。**
*   **可以随时启用或禁用约束。创建、禁用或启用约束时，可以指定一些有关约束行为的其他信息。启用的约束可以有两个选项 VALIDATE 和 NOVALIDATE。VALIDATE 将验证表中的现有数据，而 NOVALIDATE 在启用约束后不会验证现有数据。**
*   **当您创建或启用主键或唯一约束时，Oracle 将在该约束的列上创建唯一索引。外键约束不强制自动创建索引。但是，在每个外键约束的列上建立索引是值得的。如果子表中的相应列没有索引，Oracle 在对父表执行删除操作时，将被迫对子表解除表锁。如果存在索引，Oracle 使用它来标识和锁定子代中的必要行，同时删除父代行。**

## **数据完整性不应与数据安全性混淆。数据安全性涉及保护数据免受未经授权的访问，而数据完整性指的是存储数据的准确性和一致性。**

**数据完整性是衡量数字信息健康程度的标准。在数据管道中尽可能早地实施这些规则或检查，将导致最少的损坏，并提高业务流程的效率。**

**下面这段话通常适用于数据库设计和生活。**

> **"诚信，在方便和正确之间的选择！"**