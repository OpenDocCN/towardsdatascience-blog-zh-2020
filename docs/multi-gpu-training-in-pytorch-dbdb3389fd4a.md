# Pytorch 中的多 GPU 培训

> 原文：<https://towardsdatascience.com/multi-gpu-training-in-pytorch-dbdb3389fd4a?source=collection_archive---------40----------------------->

## 数据和模型并行性

![](img/aa044c18e05530394d7f3b2cdc925c7d.png)

安娜·安彻的《收割机》。链接:[维基百科](https://en.wikipedia.org/wiki/File:Anna_Ancher_-_Harvesters_-_Google_Art_Project.jpg)。

这篇文章将概述 Pytorch 中的多 GPU 培训，包括:

*   在一个 GPU 上训练；
*   多 GPU 上的训练；
*   通过一次处理更多的例子，使用数据并行性来加速训练；
*   使用模型并行性来支持需要比一个 GPU 上可用的内存更多的内存的训练模型；
*   使用 num_workers > 0 的数据加载器来支持多进程数据加载；
*   仅在可用设备的子集上训练。

# **在一个 GPU 上训练**

假设您有 3 个可用的 GPU，并且您想在其中一个上训练一个模型。您可以通过指定设备来告诉 Pytorch 使用哪个 GPU:

```
device = torch.device('cuda:0') for GPU 0device = torch.device('cuda:1') for GPU 1device = torch.device('cuda:2') for GPU 2
```

# **在多个 GPU 上训练**

要允许 Pytorch“查看”所有可用的 GPU，请使用:

```
device = torch.device('cuda')
```

使用多个 GPU 有几种不同的方式，包括数据并行和模型并行。

# **数据并行度**

数据并行是指使用多个 GPU 来增加同时处理的实例数量。例如，如果 256 的批处理大小适合一个 GPU，您可以通过使用两个 GPU 使用数据并行性将批处理大小增加到 512，Pytorch 会自动将大约 256 个示例分配给一个 GPU，将大约 256 个示例分配给另一个 GPU。

使用数据并行可以通过数据并行轻松实现。例如，假设您有一个名为“custom_net”的模型，该模型当前初始化如下:

```
import torch, torch.nn as nnmodel = custom_net(**custom_net_args).to(device)
```

现在，使用数据并行性所要做的就是将 custom_net 包装在 DataParallel 中:

```
model = nn.DataParallel(custom_net(**custom_net_args)).to(device)
```

您还需要增加批量大小，以最大限度地利用所有可用设备。

有关数据并行性的更多信息，请参见本文。

# **模型并行度**

您可以使用模型并行性来训练需要比一个 GPU 上可用的内存更多的内存的模型。模型并行性允许您将模型的不同部分分布在不同的设备上。

使用模型并行有两个步骤。第一步是在模型定义中指定模型的哪些部分应该在哪个设备上运行。这里有一个来自 [Pytorch 文档](https://pytorch.org/tutorials/intermediate/model_parallel_tutorial.html)的例子:

```
**import** torch
**import** torch.nn **as** nn
**import** torch.optim **as** optim

**class** **ToyModel**(nn**.**Module):
    **def** __init__(self):
        super(ToyModel, self)**.**__init__()
        self**.**net1 **=** torch**.**nn**.**Linear(10, 10)**.**to('cuda:0')
        self**.**relu **=** torch**.**nn**.**ReLU()
        self**.**net2 **=** torch**.**nn**.**Linear(10, 5)**.**to('cuda:1')

    **def** **forward**(self, x):
        x **=** self**.**relu(self**.**net1(x**.**to('cuda:0')))
        **return** self**.**net2(x**.**to('cuda:1'))
```

第二步是确保在调用 loss 函数时，标签与模型的输出在同一个设备上。

例如，您可能希望从将标签移动到设备“cuda:1”并将数据移动到设备“cuda:0”开始。然后，您可以在“cuda:0”上用模型的一部分处理您的数据，然后将中间表示移动到“cuda:1”，并在“cuda:1”上生成最终预测。因为您的标签已经在“cuda:1”上，Pytorch 将能够计算损失并执行反向传播，而无需任何进一步的修改。

有关模型并行性的更多信息，请参见本文。

# **使用 Num_Workers 加快数据加载速度**

Pytorch 的数据加载器提供了一种自动加载和批处理数据的有效方法。您可以将它用于任何数据集，不管它有多复杂。您需要做的就是首先定义您自己的数据集，该数据集继承自 Pytorch 的 Dataset 类:

```
from torch.utils.data import DataLoaderclass MyComplicatedCustomDataset(Dataset): 
    def __init__(self, some_arg, some_other_arg):
        """Documentation"""
        self.some_arg = some_arg
        self.some_other_arg = some_other_arg

    # Pytorch Required Methods # — — — — — — — — — — — — — — —
    def __len__(self):
        """Return an integer representing the total number of 
        examples in your data set"""
        return len(self.my_list_of_examples)

    def __getitem__(self, idx):
        """Return a single sample at index <idx>. The sample is any 
        kind of Python object you want. It could be a numpy array. 
        It could be a dictionary with strings for keys and 
        numpy arrays for values. It could be a list — really 
        whatever you want."""
        return self._a_custom_method(self.my_list_of_examples[idx])

    # Whatever Custom Stuff You Want # — — — — — — — — — — — -
    def _a_custom_method(self, example_name):
        #processing step 1
        #processing step 2
        #etc.
        return processed_example
```

对数据集的唯一要求是它定义了 __len__ 和 __getitem__ 方法。

__len__ 方法必须返回数据集中示例的总数。

__getitem__ 方法必须返回基于整数索引的单个示例。

你实际上如何准备例子和例子是什么完全取决于你。

一旦创建了数据集，就需要将该数据集包装在 Pytorch 的数据加载器中，如下所示:

```
from torch.utils.data import Dataset, DataLoaderdataset_train = MyComplicatedCustomDataset(**dataset_args)train_dataloader = DataLoader(dataset_train, batch_size=256, shuffle=True, num_workers = 4)
```

为了获得批处理，您只需遍历数据加载器:

```
for batch_idx, batch in enumerate(train_dataloader): do stuff
```

如果希望加快数据加载，可以使用多个工作线程。请注意，在对 DataLoader 的调用中，您指定了一些工作线程:

```
train_dataloader = DataLoader(dataset_train, batch_size=256, shuffle=True, **num_workers = 4**)
```

默认情况下，num_workers 设置为 0。将 num_workers 设置为正整数将启用多进程数据加载，在这种情况下，将使用指定数量的加载器工作进程来加载数据。(注意，这并不是真正的多 GPU，因为这些加载器工作进程是 CPU 上的不同进程，但是因为它与加速模型训练有关，所以我决定将它放在同一篇文章中)。

请注意，工作进程越多并不总是越好。如果将 num_workers 设置得太高，实际上会降低数据加载速度。关于如何选择最佳工人数量，也没有很大的规则。网上有很多关于它的讨论(例如这里的)，但没有结论性的答案。关于如何选择工作人员的数量，没有什么很好的规则，原因是最佳的工作人员数量取决于您正在使用的机器类型、您正在使用的数据集类型以及您的数据需要多少即时预处理。

选择工人数量的一个好方法是在数据集上运行一些小实验，在这些实验中，您计算使用不同数量的工人加载固定数量的示例需要多长时间。随着 num_workers 从 0 开始增加，您将首先看到数据加载速度的增加，随后当您遇到“太多 workers”时，数据加载速度将会降低

有关更多信息，请参见本页的[中的“多进程数据加载”。](https://pytorch.org/docs/stable/data.html)

# **模型并行和数据并行同时进行**

如果您想同时使用模型并行和数据并行，那么数据并行必须以稍微不同的方式实现，使用 DistributedDataParallel 而不是 DataParallel。更多信息，请参见[“分布式数据并行入门”](https://pytorch.org/tutorials/intermediate/ddp_tutorial.html)

# **对可用设备子集的培训**

如果您想使用模型并行性或数据并行性，但不想占用单个模型的所有可用设备，该怎么办？在这种情况下，您可以限制 Pytorch 可以看到每个型号的哪些设备。在您的代码中，您将设置设备，好像您想要使用所有的 GPU(即使用 device = torch.device('cuda '))，但是当您运行代码时，您将限制哪些 GPU 可以被看到。

假设您有 6 个 GPU，您想在其中的 2 个上训练模型 A，在其中的 4 个上训练模型 B。您可以这样做:

```
CUDA_VISIBLE_DEVICES=0,1 python model_A.pyCUDA_VISIBLE_DEVICES=2,3,4,5 python model_B.py
```

或者，如果您有 3 个 GPU，并且希望在其中一个上训练模型 A，在其中两个上训练模型 B，您可以这样做:

```
CUDA_VISIBLE_DEVICES=1 python model_A.pyCUDA_VISIBLE_DEVICES=0,2 python model_B.py
```

多 GPU 训练快乐！

*原载于 2020 年 3 月 4 日 http://glassboxmedicine.com**的* [*。*](https://glassboxmedicine.com/2020/03/04/multi-gpu-training-in-pytorch-data-and-model-parallelism/)