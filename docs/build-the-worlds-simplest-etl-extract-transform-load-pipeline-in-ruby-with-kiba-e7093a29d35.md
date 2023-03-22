# 用 Kiba 在 Ruby 中构建世界上最简单的 ETL(提取、转换、加载)管道

> 原文：<https://towardsdatascience.com/build-the-worlds-simplest-etl-extract-transform-load-pipeline-in-ruby-with-kiba-e7093a29d35?source=collection_archive---------34----------------------->

## 数据预处理

## 通过构建管道修改 CSV 中的文本来学习 ETL

![](img/1761859ec61b3f194456b7d3d400682e.png)

照片由[摄影记者](https://www.pexels.com/@skitterphoto?utm_content=attributionCopyText&utm_medium=referral&utm_source=pexels)从[像素](https://www.pexels.com/photo/gray-gold-and-red-industrial-machine-675987/?utm_content=attributionCopyText&utm_medium=referral&utm_source=pexels)拍摄

您多久遍历一次数据库表中的所有记录，修改每条记录，然后重新保存一次？

对我来说，很多。

这种模式被称为 [ETL](https://en.wikipedia.org/wiki/Extract,_transform,_load) (提取、转换、加载)。

我在网络应用、分析平台和机器学习管道中看到了这一点。

你可以自己开发，但是有很多包可以让 ETL 的编写变得干净、模块化和可测试。

我们将使用 [Kiba](https://www.kiba-etl.org/) 在 Ruby 中浏览一个例子。

# 什么是 ETL？

ETL 代表“提取、转换、加载”，但除非你来自数据挖掘背景，否则这个名字会误导人。

更好的名称可能是“加载、修改、保存”。

**提取** =从一个源加载数据(例如:数据库、CSV、XML、JSON、…)

**转换** =修改/增加每个数据点(例如:添加地理位置数据、修改标点符号、计算一些东西等等)

**加载** =存储更新(即:数据库、CSV、数据仓库……)

# 我们的目标

我们将:

1.  将电话号码的 CSV 作为输入
2.  删除非数字字符和不是 10 位数的电话号码
3.  保存到另一个 CSV

# 代码

我们将把它打包到一个小型的 ruby 项目中。

## 创建我们的目录

```
$ mkdir kiba-etl && cd kiba-etl/
```

## 添加源 CSV

用`touch phone.csv`创建一个 CSV 文件并粘贴到下面。

```
id,number
1,123.456.7891
2,222
3,303-030-3030
4,444-444-4444
5,900-000-00001
6,#1000000000
7,#9898989898
8,800-000-00000
9,999.999.9999
10,1.1.1.1.1.1.1.1.1.1
11,(112)233-4455
12,(121)212-0000
```

*在真实情况下，你可以使用 Twilio 这样的服务来检测它们是否是真实的电话号码。*

## 创建 Gemfile

运行`touch Gemfile`并将以下内容添加到文件中。

```
source '[https://rubygems.org'](https://rubygems.org')gem 'kiba'
gem 'rake', '~> 11.2', '>= 11.2.2'
```

用`bundle install`安装这些包。

## 创建用于提取的类

创建一个文件来存储我们的 ETL 类。我们称之为`common.rb`。

写我们在`common.rb`的第一堂课。我们可以给这个类取任何名字，但是我选择了`SourceCSV`。

```
require 'csv'class SourceCSV 
  def initialize(filename:)
    [@filename](http://twitter.com/filename) = filename
  end # Iterates over each line in the CSV so we can pass them one at a
  # time to transform in the next step
  def each
    csv = CSV.open([@filename](http://twitter.com/filename), headers:true) csv.each do |row|
      yield(row.to_hash)
    end csv.close
  end
end
```

## 创建一个耙式文件

我们将使用耙子任务来启动 Kiba 作业。

创建一个名为`Rakefile`(没有扩展名)的文件，并添加以下内容。我们稍后会添加更多的逻辑。

```
require 'kiba'
require_relative 'common'task :kiba_run do
  puts "The job is running..."

  Kiba.run(
    Kiba.parse do
      ##############################################################
      source SourceCSV, filename: 'numbers.csv'
      ##############################################################
    end
  )
  puts "...The job is finished"
end
```

注意，我们的 rake 任务被命名为`kiba_run`。让我们从控制台运行我们的任务。

```
$ rake kiba_run
```

输出应该是:

```
The job is running...
...The job is finished
```

如果我们看到这一点，到目前为止一切正常。

## 为加载创建一个类

现在我们将创建一个在数据被修改后保存数据的类。显然我们还没有添加任何修改逻辑。

在`common.rb`中，添加。

```
class DestinationCSV
  def initialize(filename:)
    [@csv](http://twitter.com/csv) = CSV.open(filename, 'w')
    [@headers_written](http://twitter.com/headers_written) = false
  end def write(row)
    if !@headers_written
      # Add headers on the first row 
      # after which, don't add headers
      [@headers_written](http://twitter.com/headers_written) = true
      [@csv](http://twitter.com/csv) << row.keys
    end
    [@csv](http://twitter.com/csv) << row.values
  end def close
    [@csv](http://twitter.com/csv).close
  end
end
```

这将我们加载的数据存储到一个新的 CSV 中。或者，您可以将这些数据转储到像 postgres 这样的数据库中。

将加载步骤添加到 rake 文件。

```
...
  ##############################################################
  source SourceCSV, filename: 'numbers.csv' destination DestinationCSV, filename: 'numbers-cleaned.csv'
  ##############################################################
...
```

用`rake kiba_run`再试一次。

现在应该有一个名为`numbers-cleaned.csv`的新文件，看起来和`numbers.csv`一模一样。

恭喜你！您已经完成了 ETL 中的“E”和“L”步骤。

## **为转换创建类**

现在是有趣的部分。我们将创建两个与 transform 相关的类，每个类做不同的事情。

第一个从我们的数字中删除标点符号，并计算字符数。将此类添加到`common.rb`。

```
class TransformClean
  def initialize(field:)
    [@field](http://twitter.com/field) = field
  end def process(row)
    number = row[[@field](http://twitter.com/field)] # remove non-numeric characters
    row[:number_cleaned] = number.tr('^0-9', '') # count number of characters
    row[:digit_count] = row[:number_cleaned].length row
  end
end
```

并更新`Rakefile`。

```
...
  #################################################################
  source SourceCSV, filename: 'numbers.csv' transform TransformClean, field: 'number' destination DestinationCSV, filename: 'numbers-cleaned.csv'
  #################################################################
```

第二个删除所有不是 10 位数的数字。将第二个转换类添加到`common.rb`。

```
class TransformDropFake
  def initialize(field:)
    [@field](http://twitter.com/field) = field
  enddef process(row)
    number = row[[@field](http://twitter.com/field)]
    row[:digit_count] == 10 ? row :nil
  end
end
```

更新我们的耙子任务。

```
...
  ##################################################################
  source SourceCSV, filename: 'numbers.csv' transform TransformClean, field: 'number'
  transform TransformDropFake, field: 'number' destination DestinationCSV, filename: 'numbers-cleaned.csv'
  ##################################################################
...
```

现在再次运行我们的 rake 任务，看看输出的文件。应该是这样的。

```
id,number,number_cleaned,digit_count
1,123.456.7891,1234567891,10
3,303-030-3030,3030303030,10
4,444-444-4444,4444444444,10
6,#1000000000,1000000000,10
7,#9898989898,9898989898,10
9,999.999.9999,9999999999,10
10,1.1.1.1.1.1.1.1.1.1,1111111111,10
11,(112)233-4455,1122334455,10
12,(121)212-0000,1212120000,10
```

厉害！

如果我们把输出的 CSV 打印出来，看起来会像这样。试试:`cat numbers-cleaned.csv | column -t -s, | less -S`。

```
id  number               number_cleaned  digit_count
1   123.456.7891         1234567891      10
3   303-030-3030         3030303030      10
4   444-444-4444         4444444444      10
6   #1000000000          1000000000      10
7   #9898989898          9898989898      10
9   999.999.9999         9999999999      10
10  1.1.1.1.1.1.1.1.1.1  1111111111      10
11  (112)233-4455        1122334455      10
12  (121)212-0000        1212120000      10
```

我们已经在 CSV 上实现了 ETL！

# 结论

*这个* [*youtube 视频*](https://www.youtube.com/watch?v=6hmNuXXIKf4) *和这个* [*博文*](https://www.timtilberg.com/ruby/etl/2019/09/26/kiba-tips.html) *都有助于搞清楚这一点。*

如果你把 ETL 模式放在心里，你会惊讶于使用它的频率。

但是 Python 里那么多 ETL 包(即:Airflow，Luigi，Bonobo，…)，为什么会在 Ruby 里用 Kiba 呢？

您的 web 应用程序是 Ruby on Rails，您的团队中没有 Python 专家，或者您只是想要一个简单的 ETL 解决方案。

我说，在生产环境中，只要有可能，就使用你所知道的，并最小化复杂性。