# 用 Python 读取 NetCDF 数据

> 原文：<https://towardsdatascience.com/read-netcdf-data-with-python-901f7ff61648?source=collection_archive---------0----------------------->

## 访问一种有点混乱但功能强大的数据格式

![](img/41a3b2ccd12c98350a6ff0d814c7a3df.png)

由 [Waldemar Brandt](https://unsplash.com/@waldemarbrandt67w?utm_source=medium&utm_medium=referral) 在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄的照片

网络通用数据格式(NetCDF)通常用于存储多维地理数据。这些数据的一些例子是温度、降雨量和风速。存储在 NetCDF 中的变量经常在大面积(大陆)地区每天被测量多次。由于每天进行多次测量，数据值会快速积累，变得难以处理。当每个值还被分配给一个地理位置时，数据管理会变得更加复杂。NetCDF 为这些挑战提供了解决方案。本文将带您开始使用 Python 从 NetCDF 文件中读取数据。

## 装置

NetCDF 文件可以用一些不同的 Python 模块读取。最受欢迎的是`netCDF4`和`gdal`。对于这篇文章，我们将严格集中在`netCDF4`上，因为这是我个人的偏好。

> 有关如何使用 xarray 和 rioxarray 在 Python 中读取和绘制 NetCDF 数据的信息，请查看本文。

安装很简单。我通常建议使用 anaconda Python 发行版来消除依赖关系和版本控制带来的混乱。要安装 anaconda (conda)，只需输入`conda install netCDF4`。或者，你可以用`pip`安装。

为了确保您的`netCDF4`模块安装正确，在终端中启动一个交互式会话(键入`python`并按下‘Enter’)。然后`import netCDF4 as nc`。

## 加载 NetCDF 数据集

加载数据集很简单，只需将一个 NetCDF 文件路径传递给`netCDF4.Dataset()`。对于这篇文章，我使用了一个包含来自 [Daymet](https://daymet.ornl.gov/) 的气候数据的文件。

```
import netCDF4 as ncfn = '/path/to/file.nc4'
ds = nc.Dataset(fn)
```

## 通用文件结构

NetCDF 文件有三个基本部分:元数据、维度和变量。变量包含元数据和数据。`netCDF4`允许我们访问与 NetCDF 文件相关的元数据和数据。

## 访问元数据

打印数据集`ds`，为我们提供文件中包含的变量及其尺寸的信息。

```
print(ds)
```

和输出。。。

```
<class 'netCDF4._netCDF4.Dataset'>root group (NETCDF4_CLASSIC data model, file format HDF5):start_year: 1980source: Daymet Software Version 3.0Version_software: Daymet Software Version 3.0Version_data: Daymet Data Version 3.0Conventions: CF-1.6citation: Please see [http://daymet.ornl.gov/](http://daymet.ornl.gov/) for current Daymet data citation informationreferences: Please see [http://daymet.ornl.gov/](http://daymet.ornl.gov/) for current information on Daymet referencesdimensions(sizes): time(1), nv(2), y(8075), x(7814)variables(dimensions): float32 time_bnds(time,nv), int16 lambert_conformal_conic(), float32 lat(y,x), float32 lon(y,x), float32 prcp(time,y,x), float32 time(time), float32 x(x), float32 y(y)groups:
```

上面您可以看到文件格式、数据源、数据版本、引用、维度和变量的信息。我们感兴趣的变量是`lat`、`lon`、`time`和`prcp`(降水量)。有了这些变量，我们就能找到给定时间、给定地点的降雨量。该文件仅包含一个时间步长(时间维度为 1)。

元数据也可以作为 Python 字典访问，这(在我看来)更有用。

```
print(ds.__dict__)OrderedDict([('start_year', 1980), ('source', 'Daymet Software Version 3.0'), ('Version_software', 'Daymet Software Version 3.0'), ('Version_data', 'Daymet Data Version 3.0'), ('Conventions', 'CF-1.6'), ('citation', 'Please see [http://daymet.ornl.gov/](http://daymet.ornl.gov/) for current Daymet data citation information'), ('references', 'Please see [http://daymet.ornl.gov/](http://daymet.ornl.gov/) for current information on Daymet references')])
```

那么任何元数据项都可以用它的键来访问。例如:

```
print(ds.__dict__['start_year']1980
```

## 规模

对维度的访问类似于文件元数据。每个维度都存储为包含相关信息的维度类。可以通过遍历所有可用维度来访问所有维度的元数据，如下所示。

```
for dim in ds.dimensions.values():
    print(dim)<class 'netCDF4._netCDF4.Dimension'> (unlimited): name = 'time', size = 1<class 'netCDF4._netCDF4.Dimension'>: name = 'nv', size = 2<class 'netCDF4._netCDF4.Dimension'>: name = 'y', size = 8075<class 'netCDF4._netCDF4.Dimension'>: name = 'x', size = 7814
```

单个维度是这样访问的:`ds.dimensions['x']`。

## 可变元数据

访问变量元数据的方式与访问维度的方式相同。下面的代码展示了这是如何做到的。我放弃了输出，因为它很长。

```
for var in ds.variables.values():
    print(var)
```

下面为`prcp`(降水)演示了访问特定变量信息的程序。

```
print(ds['prcp'])
```

和输出。。。

```
<class 'netCDF4._netCDF4.Variable'>float32 prcp(time, y, x)_FillValue: -9999.0coordinates: lat longrid_mapping: lambert_conformal_conicmissing_value: -9999.0cell_methods: area: mean time: sum within days time: sum over daysunits: mmlong_name: annual total precipitationunlimited dimensions: timecurrent shape = (1, 8075, 7814)
```

## 访问数据值

通过数组索引访问实际的降水数据值，并返回一个`numpy`数组。所有变量数据返回如下:

```
prcp = ds['prcp'][:]
```

或者可以返回一个子集。下面的代码返回一个 2D 子集。

```
prcp = ds['prcp'][0, 4000:4005, 4000:4005]
```

这是 2D 子集的结果。

```
[[341.0 347.0 336.0 329.0 353.0][336.0 339.0 341.0 332.0 349.0][337.0 340.0 334.0 336.0 348.0][342.0 344.0 332.0 338.0 350.0][350.0 351.0 342.0 346.0 348.0]]
```

## 结论

NetCDF 文件通常用于地理时间序列数据。最初，由于包含大量数据，并且与最常用的 csv 和光栅文件的格式不同，使用它们可能有点吓人。NetCDF 是记录地理数据的好方法，因为它内置了文档和元数据。这使得最终用户可以毫不含糊地准确理解数据所代表的内容。NetCDF 数据以 numpy 数组的形式访问，这为分析和整合到现有工具和工作流中提供了许多可能性。