# Pickletools 的力量

> 原文：<https://towardsdatascience.com/the-power-of-pickletools-handling-large-model-pickle-files-7f9037b9086b?source=collection_archive---------22----------------------->

## 处理大型 ML 模型 pickle 文件

数据量正在增加。数据越多，我们就越能利用它来解决不同的问题。假设您为某个解决方案训练了一个机器学习模型，并希望保存它以供以后预测。下面来介绍一些序列化-反序列化的方法:**Pickle**(Pickle，cPickle，Joblib，JsonPickle，Dill，Mlflow)，保存为 **PMML** 格式为管道(sklearn2pmml)，保存为 **JSON** 格式(sklearn _ json，sklearn _ export，msgpack，JsonPickle)。

您还可以使用 **m2cgen** 库将您的模型导出到 python/java/c++代码，或者编写您自己的代码来序列化和反序列化您的模型。

当你建立一个机器学习模型，通过使用大量数据来解决一些问题时，这个模型也将是巨大的！因此，当您试图将这种巨大的编码保存到您的系统时，问题就出现了(我在这里将讨论我试图处理的大型 RandomForestClassifier 模型/pickle 文件)。

# 解决方案 1:

更多的数据意味着大尺寸的模型。尽量减少你的数据，又不丢失数据中你有价值的信息。去除**重复或近似重复**、**分层欠采样**可以在这里拯救你。

# 解决方案 2:

尝试通过调整模型参数来缩小模型尺寸，而不影响精度。**在这种情况下，超参数调整**将对您有所帮助。我的基本型号 *RandomForestClassifier* 有很大的 60 GB，但是超参数调优设法把它降到了 8 GB。还是大号？让我们看看下一种疗法。

# 解决方案 3:

现在，您已经尽一切努力使您的模型更轻，而没有牺牲太多的预测能力，并且仍然获得 8 GB 的 pickle 文件。哦，亲爱的，生活太不公平了。

坚持住，还有一些希望。Pickletools 前来救援！

该库帮助您减小 pickle 文件的大小，并使 pickle 文件作为 RF 对象的加载变得更加容易和快速。

虽然 Joblib 和使用压缩方法如 **zlib、gzip** 使我的 pickle 文件缩小到 1 GB，但是将该文件作为随机森林分类器对象加载回来是一件令人头痛的事情。加载我的 pickle (1 GB 大小)和反序列化 RF 对象需要 16 GB 以上的 RAM，这将导致 *MemoryError。*

使用 Pickletools 解决了缩小 pickle 文件大小和更快加载 pickle 文件的问题，而不会占用超过系统处理能力的内存，如下所示。

要序列化:

```
clf.fit(X_train, y_train) #your classifier/regressor modelimport gzip, pickle, pickletoolsfilepath = "random_forest.pkl"
with gzip.open(filepath, "wb") as f:
    pickled = pickle.dumps(clf)
    optimized_pickle = pickletools.optimize(pickled)
    f.write(optimized_pickle)
```

要反序列化/加载回:

```
with gzip.open(filepath, 'rb') as f:
    p = pickle.Unpickler(f)
    clf = p.load()
```

呜哇！！虽然 gzip 有助于减小 pickle 的大小(到 1 GB)，但 pickletools 有助于 pickle 文件加载更快，而不会消耗太多内存(这次占用了 7 GB RAM)

为此，您还可以设置 pickle 属性 fast = True，但将来可能会被弃用，因为文档中是这么说的:

```
import picklefilepath = "random_forest.pkl"with open(filepath, 'wb') as f:
    p = pickle.Pickler(f)
    p.fast = True
    p.dump(clf)with open(filepath, 'rb') as f:
    p = pickle.Unpickler(f)
    clf = p.load()
```

# 额外解决方案:

如果您可以通过让 CPU 的每个核心同时工作来以任何方式并行化整个过程，那么在某种程度上，它也可以拯救您！

好了，该你自己试试了。愿原力与你同在！

***延伸阅读及参考文献:***

1.  【https://docs.python.org/3/library/pickle.html#pickle. Pickler.fast
2.  [https://docs . python . org/3/library/pickle tools . html # pickle tools . optimize](https://docs.python.org/3/library/pickletools.html#pickletools.optimize)
3.  [https://wiki.python.org/moin/ParallelProcessing](https://wiki.python.org/moin/ParallelProcessing)
4.  [https://stack overflow . com/questions/23916413/celery-parallel-distributed-task with multi-processing](https://stackoverflow.com/questions/23916413/celery-parallel-distributed-task-with-multiprocessing)
5.  [https://docs.python.org/3/library/multiprocessing.html](https://docs.python.org/3/library/multiprocessing.html)