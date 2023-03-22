# Python:布尔混淆

> 原文：<https://towardsdatascience.com/python-the-boolean-confusion-f7fc5288f0ce?source=collection_archive---------7----------------------->

`if val`和`if val is not None`不一样！

![](img/5e18887ff31720ae648112ebc2bcd846.png)

照片由 [Hitesh Choudhary](https://unsplash.com/@hiteshchoudhary?utm_source=medium&utm_medium=referral) 在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄

## 哇，什么？？？Python(不)疯狂。

当你做`if val is None`时，你调用操作员`is`，它检查`x`的身份。即`if val is value`在这里，**是**运算符检查两个操作数是否指向同一个对象。

> `*None*`在 Python 中是单例的，所有的`*None*`值也是完全相同的实例。

## 但是…

你说`if val`，python 的表现就不一样了。**如果**期望一个布尔值，并且假设`val`不是布尔值，Python 自动调用`val`的`__bool__`方法。

`if val`实际上是作为`if val.__bool__`执行的

令人困惑的是，`bool(None)`返回`False`，所以如果`val`为 None。这按预期工作，但还有其他值被评估为`False.`

最重要的例子是空列表。`bool([])`同样返回`False`。通常情况下，空单和`None`有不同的含义；None 表示没有值，而空列表表示零值。语义上，他们是不同的。

## 例子

为了更好地理解，这里有一些例子。

我们将针对不同的值执行以下条件块:

```
**if** val:
    print(**'if val'**)**if not** val:
    print(**'if not val'**)**if** val **is not None**:
    print(**'if val is not None'**)**if** val **is None**:
    print(**'if val is None'**)
```

**1。无**

```
val = Noneif not val
if val is None
```

**2。【**(空单)

```
val = []if not val
if val is not None
```

**3。[27，37]** (非空列表)

```
val = [27, 37]if val
if val is not None
```

**4。0** (数字—零)

```
val = 0if not val
if val is not None
```

**5。1** (数字—1/非零)

```
val = 1if val
if val is not None
```

**6。一个物体**

```
val = object()if val
if val is not None
```

很有趣，对吧？试试看！