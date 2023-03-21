# C++和 Python 中迭代器的简明介绍

> 原文：<https://towardsdatascience.com/a-gentle-introduction-to-iterators-in-c-and-python-dafece4b0f4f?source=collection_archive---------44----------------------->

## 第 1 部分:用 C++中的例子介绍迭代器

![](img/90fe16b24b6ed23722bdee6f28575f08.png)

来源:恰兰·库尼(用 Powerpoint 绘制)。

对于所有级别和学科的程序员，包括寻求实现高效数据处理管道的数据科学家，畸胎器是一个非常有用的编程范例。即使你没有显式地编写自己的代码来包含迭代器，它们也很可能已经在后台帮助你对容器中的元素进行操作了。简单地说，迭代器用于遍历容器中的元素，在执行操作时实现高效的处理。迭代器有助于处理无限序列，而不需要为所有可能的序列重新分配资源[1]。这有助于高效的资源配置和更干净的代码。

当然，不同编程语言的实现是不同的。这是关于这个主题的两篇文章中的第一篇，我将重点介绍 C++中迭代器的简单介绍。在第二篇文章中，我将看看迭代器是如何在 Python 中实现的。Python 附带了一个名为 **itertools** 的内置模块，它允许创建迭代器来增强循环性能，以及许多其他特性。如果您想了解更多关于 itertools 的信息，请阅读第 2 部分。

# 迭代器简介

D 不同的编程语言将订阅作为一种直接访问容器中元素的方法(如向量、数组)。例如，在 Python 中，可以从列表 *x = ['Today '，' is '，' Monday']* 中访问单词“Monday”，代码为 *x[2]* 。然而，一个更通用的方法，称为**迭代器**，给程序员**间接**访问元素(这是一个类似于 C++中指针的想法)【2】。基本上，迭代器抽象了数据结构中不同元素的地址，并且是一个非常强大的工具，允许数据结构与算法进行交互，而算法不一定知道它们正在操作的结构。

迭代器的主要功能之一是它能够处理容器的元素，同时将用户与数据的内部结构隔离开来[3]。迭代器在时间和内存性能方面提高了程序员的效率，尤其是在处理大型数据集时。它们还提供了便利，因为我们不需要如此关心列表或向量的大小，例如在 C++中，我们可以简单地使用 **end()** 方法。迭代器的使用还可以增强代码的可重用性，尤其是在我们改变所使用的容器类型的情况下。

当然，有些情况下迭代器可能不是最好的工具，或者根本不起作用。例如，以某种复杂的方式同时遍历两个独立的数据结构，特别是当其中一个数据决定了另一个数据的位置时，会导致迭代器出现问题[4]。然而，这些是更高级的案例，不属于本文的范围。

# 用 C++中的迭代器编码

在强类型语言 C++中，有迭代器的类型有返回迭代器的成员。这些类型有名为 **begin** 和 **end** 的成员，它们分别返回对应于第一个元素和“超过结尾的那个”的迭代器[5]。这对迭代器被定义为迭代器范围，表示容器中要操作的元素的范围(见上图)。

并非所有的迭代器都是相同的，它们可以根据功能分为五个子类型，其中最强大的是**随机访问迭代器**，顾名思义，它允许随机访问容器中的任何元素，而不是顺序访问[5]。在本教程中，我们将把重点放在随机访问迭代器上，因为我们将一些基本操作应用到字符串和向量上，但是关于其他类型迭代器的更多细节参见[5]。

您还应该知道迭代器类型是由其容器定义的。例如，如果你有一个 const 类型的 vector，那么它对应的迭代器将是 const_iterator 类型，可以从这个 vector 中读取元素，但不能向其中写入元素。从 C++11 开始，有两个新函数允许程序员特别请求 const_iterator 类型。这些是 **cbegin** 和 **cend** ，是上述**的常量版本。如果我们使用这些函数，返回的迭代器将是常量，即使底层容器不是。**

所以，让我们看一些代码…

在第一个例子中，我们只想使用 **toupper()** 函数将字符串中的第一个字母大写。在初始化字符串之后，我们检查它是否为空(if (s.begin()！= s.end()))。在 if 语句中，我们将第一个值的迭代器(s.begin())赋给变量名 *it* 。然后迭代器被解引用(*it)，以便将实际值' *c'* '传递给函数，并且在赋值的左侧被解引用，以便将大写字母赋给字符串， *s* 。**注意:解引用意味着我们从迭代器中提取实值。**

```
std::string s("ciaran cooney");
if (s.begin() != s.end()){
    auto it = s.begin();
    *it = toupper(*it);
    }
std::cout << s << std::endl;//Output: Ciaran cooney
```

这将返回首字母大写的字符串，即 **s = "Ciaran cooney"** 。如果我们想大写字符串中的每个元素，我们可以使用++操作符遍历它。这里，我们简单地使用迭代器依次遍历每个字符，大写该字符，然后递增迭代器。这个 for 循环中的条件检查迭代器在退出循环之前何时到达字符串的末尾(s.end())。

```
for (auto it = s.begin(); it != s.end(); it++){
     *it = toupper(*it);
    }
std::cout << s << std::endl;//Output: CIARAN COONEY
```

这一次，迭代器用于遍历字符串中的每个元素，然后将其大写:“CIARAN COONEY”。

对于数据科学家和机器学习工程师来说，自然语言编程显然是一个很大的领域，尽管 python 是 NLP 事实上的家，但看看其他语言如何处理数据总是好的。让我们看几个使用 NLP 中常见的一些非常基本的过程的例子。

这里的想法是取一个字符串(这里是名和姓),只将两个名字的首字母大写。为了帮助实现这一点，我使用了一个名为 **split_string()** 的函数，它根据一个分隔符分割字符串，并在一个向量中返回各个名称。首先，我们创建一个名为 **cont** 的**向量<字符串>，它是一个用于返回分隔字符串的容器，以及一个分隔符( **dlim** )，它用于确定用来实例化拆分的字符。然后使用**范围**运算符 **:** 依次从 cont 向量中选择每个单词。接下来，我们在第一个元素的位置创建迭代器 *it* 之前做一个检查，确保向量不为空。和前面的例子一样，toupper()函数用于执行首字母的大写，产生输出“Ciaran 库尼”。**

```
std::vector<std::string> cont; //container for split strings
char dlim = ' '; //where to split the inputsplit_string(s, cont, dlim);for (std::string &str : cont){
    if (str.begin() != str.end()){
        auto it = str.begin();
        *it = toupper(*it);
        }
    }
std::string strUp = cont[0] + ' ' + cont[1];
std::cout << strUp << std::endl;//Output: Ciaran Cooney
```

在最后一个考虑字符串的例子中，让我们想象我们读入一个文本文件，并把它放在一个向量中，用空格隔开。然后，我们可以使用迭代器遍历向量，打印它发现的每个单独的元素(或执行一些其他操作),然后在到达迭代器末尾时停止，或者发现一个空元素。

在这个例子中，我们递增迭代器，直到到达末尾或者发现一个空元素。它->empty())。在每次递增时，我们简单地取消对迭代器的引用，打印出相应的文本字符串。

```
std::vector<std::string> text = {"Hello", "there,", "my", "name", "is", "Ciaran"};for (auto it = text.cbegin(); it != text.cend() && !it->empty(); it++){
    std::cout << *it << " ";
     }//Output: Hello there, my name is Ciaran
```

结果输出是“你好，我的名字是 Ciaran”。

现在我将通过几个例子，使用包含整数的向量。在第一个示例中，我们希望比较两个向量在每个索引处的元素，当元素匹配时返回包含 1 的向量，当元素不匹配时返回包含 0 的向量。

这里，我们从被比较的两个向量中创建了两个迭代器， *itA* 和 *ItB* 。然后实例化一个 while 循环，并保持为真，直到迭代器 *itA* 到达向量 *vecA 的末尾。*在每次迭代中，通过解引用 *itA* 和 *ItB，*对两个迭代器中的元素进行比较，当这些元素相等时，将 1 附加到结果中。++操作符再次用于递增两个迭代器，直到到达序列的末尾。

```
std::vector<int> vecA = {0,1,0,0,0,1,0,1,1,1};
std::vector<int> vecB = {0,1,1,1,0,0,0,1,0,0};
std::vector<int> result;
auto itA = vecA.begin();
auto itB = vecB.begin();while (itA != vecA.end()){
    if (*itA == *itB){
        result.push_back(1);
        }
    else{
        result.push_back(0);
    }
    itA++;
    itB++;
}
print_results(result); //function for printing contents of vectors//Output: 1       1       0       0       1       0       1       1       0       0
```

在与迭代器交互时，C++中有许多方法可以促进额外的功能。这些范围从初学者到专家水平，但在这里我将演示一些更基本的例子。 **advance** 方法用于将迭代器向前移动一定的位置。 **next** 方法执行类似的任务，除了它接受一个迭代器作为它的一个参数，并返回另一个已经前进了许多步的迭代器。正如您可能猜到的那样， **prev** 方法是 next 方法的逆方法，它返回一个已经后退了许多步的迭代器。下面的代码是这些迭代器方法的简单演示。

```
std::vector<int> nums = {1,2,3,4};std::vector<int>::iterator it = nums.begin();
advance(it, 2); //increments an iterator
std::cout << *it << "\n"; //Output: 3 auto it2 = next(it, 1); //returns an iterator
std::cout << *it2 << std::endl; //Output: 4auto it3 = prev(it2, 1);
std::cout << *it3 << std::endl; //Output: 3
```

让我们看看 C++中迭代器的最后一个介绍性例子。这一个稍微高级一点，因为它试图使用迭代器通过 **advance** 和 **inserter** 方法将一些值插入到现有的向量中。插入器是一个迭代器适配器，它接受一个容器并产生一个迭代器，将元素添加到指定的容器中[2]。

对于这个玩具示例，我们有两个向量:一个包含整数序列，另一个包含该序列中缺失的整数。在迭代器 *it* 创建之后，从第一个序列开始，我们将迭代器向前推进 3 步，到达我们想要放置新序列的位置。我们希望复制的序列范围是用 begin()和 end()方法建立的。这允许我们使用迭代器 *it* 将数字 3、4 和 5 复制到第一个序列中。

```
std::vector<int> sequence1 = {1,2,3,7,8,9,10};
std::vector<int> sequence2 = {4,5,6};
auto it = sequence1.begin();advance(it, 3);
copy(sequence2.begin(), sequence2.end(), inserter(sequence1, it));for (int &i : sequence1){
     std::cout << i << ", ";}//Output: 1, 2, 3, 4, 5, 6, 7, 8, 9, 10
```

现在，第一个序列包含缺失的值，并打印为:1，2，3，4，5，6，7，8，9，10。

这就是我们对 C++中迭代器的浅显探讨的结束，但我希望它能让你们中的一些人开始研究所有提供的功能。必须指出的是，迭代器是一个抽象概念，一开始不容易完全理解，所以不要担心，我们仍然不完全清楚它们是如何操作的。我的建议是先尝试一些编码，然后你会开始看到好处。

我用过的所有 C++例子和附加函数都在这里:[https://github.com/cfcooney/medium_posts](https://github.com/cfcooney/medium_posts)

# 来源

[1] S. Jaiswal，《Python 迭代器教程》， *DataCamp* ，2018。【在线】。可用:[https://www . data camp . com/community/tutorials/python-iterator-tutorial UTM _ source = AdWords _ PPC&UTM _ campaignid = 898687156&UTM _ adgroupid = 48947256715&UTM _ device = c&UTM _ keyword =&UTM _ match type = b&UTM _ network = g&UTM _ ADT](https://www.datacamp.com/community/tutorials/python-iterator-tutorial?utm_source=adwords_ppc&utm_campaignid=898687156&utm_adgroupid=48947256715&utm_device=c&utm_keyword=&utm_matchtype=b&utm_network=g&utm_adpostion=&utm_creative=332602034343&utm_targetid=aud-299261629574:dsa-429603003980&utm_loc_interest_ms=&utm_loc_physical_ms=1007287&gclid=CjwKCAjw8pH3BRAXEiwA1pvMsXql1oSBz6jNEo9AjQPxD-DT6dNkrNp78jD2lyznqnNy8GIUmNue5hoCYp0QAvD_BwE.)

[2] S. B. Lippman，J. Lajoie，和 B. E. Moo， *C++初级读本*，第 5 版。皮尔森教育。

[3]《迭代器》，2020。【在线】。可用:[https://de.wikipedia.org/wiki/Iterator.](https://de.wikipedia.org/wiki/Iterator.)

[4] S. Gardner，“Scott Gartner 对‘编程语言中迭代器的一些缺点/不足是什么？，' " *Quora* ，2015。【在线】。可用:[https://www . quora . com/What-is-some-visibilities-defect-of-iterators-in-programming-languages。](https://www.quora.com/What-are-some-disadvantages-shortcoming-of-iterators-in-programming-languages.)

[5] M. Singh，《C++中迭代器的介绍》， *GeeksforGeeks* ，2019。【在线】。可用:[https://www.geeksforgeeks.org/introduction-iterators-c/.](https://www.geeksforgeeks.org/introduction-iterators-c/.)