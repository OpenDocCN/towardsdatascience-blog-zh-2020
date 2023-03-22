# Apache Avro 和 Python 入门

> 原文：<https://towardsdatascience.com/getting-started-with-apache-avro-and-python-59239ae7f5e?source=collection_archive---------6----------------------->

## 了解如何创建和使用基于 Apache Avro 的数据，以便在 Python 应用程序中更好、更高效地传输数据

![](img/9fd66b41f4ef2eff9d45b6935cb7160e.png)

https://unsplash.com/photos/LqKhnDzSF-8

在这篇文章中，我将谈论 [Apache Avro](https://avro.apache.org/docs/current/) ，这是一个开源数据序列化系统，被 Spark、Kafka 和其他工具用于大数据处理。

# 什么是阿帕奇 Avro

根据维基百科:

> Avro 是在 Apache 的 Hadoop 项目中开发的面向行的远程过程调用和数据序列化框架。它使用 JSON 来定义数据类型和协议，并以紧凑的二进制格式序列化数据。它的主要用途是在 Apache Hadoop 中，可以为持久数据提供序列化格式，为 Hadoop 节点之间的通信以及从客户端程序到 Hadoop 服务的通信提供有线格式。Avro 使用一个模式来组织被编码的数据。它有两种不同类型的模式语言；一个用于人工编辑(Avro IDL ),另一个基于 JSON，更适合机器阅读。[3]

基本上，Avro 是由 Hadoop 之父 Doug Cutting 开发的独立于语言的数据序列化系统。在我进一步讨论 Avro 之前，请允许我简单讨论一下*数据序列化*及其优势。

# 什么是数据序列化和反序列化

数据序列化是将复杂对象(*数组、字典、列表、类对象、JSON 等*)转换成字节流的过程，这样它们就可以被存储或转移到其他机器上。在具有不同体系结构、硬件或操作系统的计算机之间传输数据时进行数据序列化的原因。一旦数据在另一端被接收到，它就可以被还原回原来的形式。这个过程被称为*反序列化*。

现在你知道它是关于什么的了，让我们深入研究并使用一些代码。

# 开发和安装

Python 应用程序中目前使用了两个库。一个简单地叫做`avro`，你可以在这里访问[。而另一个是](https://avro.apache.org/docs/1.10.0/gettingstartedpython.html) [FastAvro](https://github.com/fastavro/fastavro) 号称比上一个更快。两者的工作原理是一样的。因为我们正在做一个玩具例子，所以前面的库对我们来说已经足够了。因此，总是使用典型的 *pip* 工具来安装它:

`pip install avro`

# Avro 模式

```
{"namespace": "me.adnansiddiqi",
 "type": "record",
 "name": "User",
 "fields": [
     {"name": "name", "type": "string"},
     {"name": "age",  "type": ["int", "null"]},
     {"name": "gender", "type": ["string", "null"]}
 ]
}
```

Apache Avro 格式实际上是一种 JSON 结构。你可以说 Avro 格式实际上是一个 JSON 数据结构和一个用于验证目的的模式的组合。因此，在我们创建扩展名为`.avro`的 Avro 文件之前，我们将创建它的模式。

好了，我已经想出了一个模式，上面是一个 JSON 结构。我们将首先提到名称空间。它只是一个字符串。通常，它遵循 Java 打包命名约定使用的相同格式，这是域名的反码，但不是必需的。这里我提到了我的博客网址的反面。在那之后你提到了你的模式的类型，这里它是`record`类型。还有其他类型的也像`enum`、`arrays`等。之后，我们提到模式的名字，这里是`User`。下一个是`field`项，可以是一个或多个。它有必填字段`name`和`type`以及可选字段`doc`和`alias`。`doc`字段用于记录您的字段，而`alias`用于给字段一个不同于`name`中提到的名称。到目前为止一切顺利。我们创建的模式将保存在一个名为`users.avsc`的文件中。

现在，我们将编写从模式文件中读取模式的代码，然后在 Avro 文件中添加一些记录。稍后，我们将检索记录并显示它们。让我们写代码吧！

```
import avro.schema
from avro.datafile import DataFileReader, DataFileWriter
from avro.io import DatumReader, DatumWriterschema = avro.schema.parse(open("user.avsc").read())writer = DataFileWriter(open("users.avro", "wb"), DatumWriter(), schema)
writer.append({"name": "Alyssa", "age": 25,"gender":"female"})
writer.append({"name": "Ahmad", "age": 35,"gender":"male"})
writer.close()reader = DataFileReader(open("users.avro", "rb"), DatumReader())
for user in reader:
    print(user)
    print('===================')
reader.close()
```

导入必要的模块后，我要做的第一件事就是读取模式文件。`DatumWriter`负责将数据翻译成 Avro 格式 w.r.t 输入模式。`DataFileWriter`负责将数据写入文件。插入几条记录后，我们关闭编写器。`DataFileReader`的工作方式类似，唯一的区别是`DatumReader()`负责存储在`users.avro`中的数据的反序列化，然后使其可用于显示。当您运行代码时，它将显示如下:

```
python play.py
{'name': 'Alyssa', 'age': 25, 'gender': 'female'}
===================
{'name': 'Ahmad', 'age': 35, 'gender': 'male'}
```

到目前为止还好吗？现在让我们对模式做一点改变。我将`age`字段更改为`dob`，当我运行时，它显示以下错误:

```
python play.py
Traceback (most recent call last):
  File "play.py", line 8, in <module>
    writer.append({"name": "Alyssa", "age": 25,"gender":"female"})
  File "/Users/AdnanAhmad/Data/anaconda3/lib/python3.7/site-packages/avro/datafile.py", line 303, in append
    self.datum_writer.write(datum, self.buffer_encoder)
  File "/Users/AdnanAhmad/Data/anaconda3/lib/python3.7/site-packages/avro/io.py", line 771, in write
    raise AvroTypeException(self.writer_schema, datum)
avro.io.AvroTypeException: The datum {'name': 'Alyssa', 'age': 25, 'gender': 'female'} is not an example of the schema {
  "type": "record",
  "name": "User",
  "namespace": "me.adnansiddiqi",
  "fields": [
    {
      "type": "string",
      "name": "name"
    },
    {
      "type": [
        "int",
        "null"
      ],
      "name": "dob"
    },
    {
      "type": [
        "string",
        "null"
      ],
      "name": "gender"
    }
  ]
}
```

很酷，不是吗？同样，如果你改变了数据，它会尖叫着告诉你把事情弄好。

# 结论

我希望您已经了解了一些关于 Apache Avro 的知识，以及 Python 如何让您使用它跨设备和系统传输数据。我只是触及了它的表面，还有更多关于它的内容。Avro 也被阿帕奇 Spark T1 和 T2 Kafka T3 大量用于数据传输。

*原载于 2020 年 8 月 6 日*[*http://blog . adnansiddiqi . me*](http://blog.adnansiddiqi.me/getting-started-with-apache-avro-and-python/)*。*