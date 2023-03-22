# 强大的终极二分搜索法模板和许多 LeetCode 问题

> 原文：<https://towardsdatascience.com/powerful-ultimate-binary-search-template-and-many-leetcode-problems-1f850ef95651?source=collection_archive---------10----------------------->

## 在几分钟内编写无 python 解决方案的伟大工具

![](img/2a53bccc02e23917a63b0853c12950a6.png)

[李·坎贝尔](https://unsplash.com/@leecampbell)在 [Unsplash](https://unsplash.com/photos/DtDlVpy-vvQ) 上的照片

# 介绍

二分搜索法在概念上很容易理解。基本上，它将搜索空间分成两半，只保留可能有搜索目标的一半，而丢弃可能没有答案的另一半。这样，我们在每一步都将搜索空间缩小一半，直到找到目标。二分搜索法帮助我们将搜索时间从线性 O(n)减少到对数 O(log n)。但是说到实现，要在几分钟内写出一个没有错误的代码是相当困难的。一些最常见的问题包括:

*   何时退出循环？应该用`left < right`还是`left <= right`作为 while 循环条件？
*   如何初始化边界变量`left`和`right`？
*   如何更新边界？如何从`left = mid`、`left = mid + 1`和`right = mid`、`right = mid — 1`中选择合适的组合？

对二分搜索法的一个相当普遍的误解是，人们通常认为这种技术只能用在简单的场景中，比如“给定一个排序的数组，从中找出一个特定的值”。事实上，它可以应用于更复杂的情况。

在 LeetCode 中做了大量的练习后，我制作了一个强大的二分搜索法模板，并通过稍微扭曲这个模板解决了许多难题。我会在这篇文章中与你们分享这个模板。**我不想只是炫耀代码就离开。最重要的是，我想分享逻辑思维:如何将这个通用模板应用于各种问题。希望看完这篇文章后，人们不会再因为听到“神圣的 sh*t！这个问题可以用二分搜索法来解决！我以前怎么没想到呢！”**

# 最广义的二分搜索法

假设我们有一个搜索空间。它可以是数组、范围等。通常是按升序排序的。对于大多数任务，我们可以将需求转换为以下通用形式:

**最小化 k，s.t .条件(k)为真**

以下代码是最通用的二分搜索法模板:

这个模板真正的好处是，对于大多数二分搜索法问题，我们只需要在复制粘贴这个模板后修改三个部分，再也不用担心代码中的死角和 bug 了:

*   正确初始化边界变量`left`和`right`。只有一个规则:设置边界使**包含所有可能的元素**；
*   决定返回值。是`return left`还是`return left — 1`？记住这个:**退出 while 循环后，** `**left**` **是满足** `**condition**` **函数**的最小 k；
*   设计`condition`功能。这是最难也是最美的部分。需要大量的练习。

下面我将向你们展示如何将这个强大的模板应用于许多 LeetCode 问题。

# 施基肥

[](https://leetcode.com/problems/first-bad-version/) [## 第一个错误版本- LeetCode

### 你是一名产品经理，目前正带领一个团队开发新产品。不幸的是，最新版本的…

leetcode.com](https://leetcode.com/problems/first-bad-version/) 

首先，我们初始化`left = 1`和`right = n`以包含所有可能的值。然后我们注意到我们甚至不需要设计`condition`函数。它已经由`isBadVersion` API 给出了。找到第一个坏版本就相当于找到满足`isBadVersion(k) is True`的最小 k。我们的模板非常适合:

[](https://leetcode.com/problems/sqrtx/) [## Sqrt(x) - LeetCode

### 实现 int sqrt(int x)。计算并返回 x 的平方根，其中 x 保证为非负整数…

leetcode.com](https://leetcode.com/problems/sqrtx/) 

相当简单的问题。我们需要搜索满足`k^2 <= x`的最大 k，因此我们可以很容易地得出解决方案:

有一件事我想指出来。记得我说过我们通常寻找满足特定条件的最小 k 值吗？但是在这个问题中，我们寻找的是最大的 k 值。感到困惑？不要这样。实际上，满足`isBadVersion(k) is False`的最大 k 正好等于满足`isBadVersion(k) is True`的最小 k 减一。这就是为什么我之前提到我们需要决定返回哪个值，`left`还是`left — 1`。

[](https://leetcode.com/problems/search-insert-position/) [## 搜索插入位置- LeetCode

### 给定一个排序数组和一个目标值，如果找到目标，则返回索引。如果没有，返回索引…

leetcode.com](https://leetcode.com/problems/search-insert-position/) 

非常经典的二分搜索法应用。我们正在寻找满足`nums[k] ≥ target`的最小 k，我们可以复制粘贴我们的模板。注意，不管输入数组`nums`是否有重复，我们的解决方案都是正确的。还要注意，输入`target`可能比`nums`中的所有元素都大，因此需要放在数组的末尾。这就是为什么我们应该初始化`right = len(nums)`而不是`right = len(nums) — 1`。

# 高级应用

上述问题相当容易解决，因为它们已经给了我们要搜索的数组。乍一看，我们就知道应该用二分搜索法来解决这些问题。然而，更常见的情况是搜索空间和搜索目标不那么容易获得。有时我们甚至不会意识到问题应该用二分搜索法来解决——我们可能只是求助于动态编程或 DFS，然后陷入很长一段时间。

至于“什么时候可以用二分搜索法”这个问题，我的回答是，**如果我们能发现某种单调性，例如，如果** `**condition(k) is True**` **那么** `**condition(k + 1) is True**` **，那么我们可以考虑二分搜索法**。

[](https://leetcode.com/problems/capacity-to-ship-packages-within-d-days/) [## 在 D 天内运送包裹的能力- LeetCode

### 传送带上的包裹必须在 D 天内从一个港口运送到另一个港口。第 I 个包裹在…

leetcode.com](https://leetcode.com/problems/capacity-to-ship-packages-within-d-days/) 

当我们第一次遇到这个问题时，可能不会想到二分搜索法。我们可能会自动将`weights`视为搜索空间，然后意识到在浪费了大量时间后，我们进入了一个死胡同。事实上，我们正在寻找所有可行容量中最小的一个。我们挖掘出这个问题的单调性:如果我们能在`D`天内用容量`m`成功装运所有包裹，那么我们就一定能把任何大于`m`的容量全部装运。现在我们可以设计一个`condition`函数，姑且称之为`feasible`，给定一个输入`capacity`，它返回是否有可能在`D`天内装运所有包裹。这可能以一种贪婪的方式运行:如果当前的包裹还有空间，我们将这个包裹放在传送带上，否则我们等待第二天放置这个包裹。如果需要的总天数超过了`D`，我们返回`False`，否则我们返回`True`。

接下来，我们需要正确初始化我们的边界。显然`capacity`至少应该是`max(weights)`，否则传送带运不出最重的包裹。另一方面，`capacity`不需要比`sum(weights)`多，因为这样我们就可以在一天之内把所有的包裹都发货了。

现在，我们已经获得了应用二分搜索法模板所需的所有信息:

[](https://leetcode.com/problems/split-array-largest-sum/) [## 分裂阵列最大和- LeetCode

### 给定一个由非负整数和一个整数组成的数组，你可以把数组分成非空的…

leetcode.com](https://leetcode.com/problems/split-array-largest-sum/) 

如果你仔细观察，你可能会发现这个问题与上面的 LC 1011 有多么相似。类似地，我们可以设计一个`feasible`函数:给定一个输入`threshold`，然后决定是否可以将数组分成几个子数组，使得每个子数组的和小于或等于`threshold`。这样我们就发现了问题的单调性:如果`feasible(m)`是`True`，那么所有大于`m`的输入都能满足`feasible`函数。你可以看到解决方案代码与 LC 1011 完全相同。

但是我们很可能会有疑问:我们的解返回的`left`确实是满足`feasible`的最小值，但是我们怎么知道我们可以把原数组拆分为**实际得到这个子数组和**？比如说`nums = [7,2,5,10,8]`和`m = 2`。我们有 4 种不同的方法来分裂阵列，从而相应地得到 4 个不同的最大子阵列和:`25:[[7], [2,5,10,8]]`、`23:[[7,2], [5,10,8]]`、`18:[[7,2,5], [10,8]]`、`24:[[7,2,5,10], [8]]`。只有 4 个值。但是我们的搜索空间`[max(nums),sum(nums)]=[10,32]`不仅仅只有 4 个值。也就是说，无论我们如何分割输入数组，我们都无法获得搜索空间中的大多数值。

假设`k`是满足`feasible`函数的最小值。我们可以用反证法证明我们解决方案的正确性。假设没有子阵列的和等于`k`，即每个子阵列的和小于`k`。`feasible`函数中的变量`total`记录当前负载的总重量。如果我们的假设是正确的，那么`total`将总是小于`k`。因此，`feasible(k-1)`必须是`True`，因为`total`最多等于`k-1`并且永远不会触发 if 子句`if total > threshold`，因此`feasible(k-1)`必须具有与`feasible(k)`相同的输出，即`True`。但是我们已经知道`k`是满足`feasible`函数的最小值，那么`feasible(k-1)`就得是`False`，这是一个矛盾。所以我们的假设是不正确的。现在我们已经证明了我们的算法是正确的。

[](https://leetcode.com/problems/koko-eating-bananas/) [## 科科吃香蕉- LeetCode

### 科科喜欢吃香蕉。有 N 堆香蕉，第 I 堆有成堆的香蕉。警卫已经走了，而且…

leetcode.com](https://leetcode.com/problems/koko-eating-bananas/) 

非常类似上面提到的 LC 1011 和 LC 410。让我们设计一个`feasible`函数，给定一个输入`speed`，确定科科是否能以每小时的进食速度`speed`在`H`小时内吃完所有香蕉。显然，搜索空间的下界是 1，上界是`max(piles)`，因为科科每小时只能选择一堆香蕉吃。

[](https://leetcode.com/problems/minimum-number-of-days-to-make-m-bouquets/) [## 制作 m 束鲜花的最少天数- LeetCode

### 给定一个整数数组 bloomDay，一个整数 m 和一个整数 k。我们需要做 m 个花束。要制作花束，你需要…

leetcode.com](https://leetcode.com/problems/minimum-number-of-days-to-make-m-bouquets/) 

既然我们已经解决了上面的三个高级问题，这一个应该很容易做到。这个问题的单调性非常明显:如果我们能在等待`d`天后制作`m`花束，那么如果我们等待超过`d`天，我们肯定也能完成。

[](https://leetcode.com/problems/kth-smallest-number-in-multiplication-table/description/) [## 乘法表中第 k 个最小的数- LeetCode

### 几乎每个人都用过乘法表。但是你能从…中快速找出第 k 个最小的数字吗？

leetcode.com](https://leetcode.com/problems/kth-smallest-number-in-multiplication-table/description/) 

对于像这样的最小问题，我们首先想到的是堆。通常，我们可以维护一个最小堆，并只弹出堆的顶部 k 次。然而，这在这个问题中是行不通的。我们没有整个乘法表中的每个数字，相反，我们只有表的高度和长度。如果我们要应用堆方法，我们需要显式地计算这些`m*n`值并将它们保存到堆中。这个过程的时间复杂度和空间复杂度都是 O(mn)，效率相当低。这是二分搜索法进来的时候。还记得我们说设计`condition`功能是最难的部分吗？为了找到表中第 k 个最小值，我们可以设计一个`enough`函数，给定一个输入`num`，判断是否至少有 k 个值小于或等于`num`。**满足** `**enough**` **函数的最小 num 就是我们要找的答案**。回想一下，二分搜索法的关键是发现单调性。在这个问题中，如果`num`满足`enough`，那么当然任何大于`num`的值都可以满足。这种单调性是我们的二分搜索法算法的基础。

让我们考虑搜索空间。显然下界应该是 1，上界应该是乘法表中的最大值，也就是`m * n`，那么我们就有了搜索空间`[1, m * n]`。与堆解决方案相比，二分搜索法解决方案的压倒性优势在于，它不需要显式计算表中的所有数字，它只需要从搜索空间中选取一个值，并将`enough`函数应用于该值，以确定我们应该保留搜索空间的左半部分还是右半部分。这样，二分搜索法方案只需要恒定的空间复杂度，比堆方案好得多。

接下来让我们考虑如何实现`enough`功能。可以观察到，乘法表中的每一行都只是其索引的倍数。例如，第三行`[3,6,9,12,15...]`中的所有数字都是 3 的倍数。因此，我们可以逐行计数小于或等于输入`num`的条目总数。以下是完整的解决方案。

在上面的 LC 410 中，我们怀疑“二分搜索法的结果实际上是子阵列和吗？”。这里我们有一个类似的疑问:“**二分搜索法的结果真的在乘法表里吗？**”。答案是肯定的，我们也可以用矛盾来证明。将`num`表示为满足`enough`功能的最小输入。我们假设`num`不在表中，也就是说`num`不能被`[1, m]`中的任何一个`val`整除，也就是说`num % val > 0`。因此，将输入从`num`更改为`num - 1`对表达式`add = min(num // val, n)`没有任何影响。所以`enough(num)`也会返回`True`，就像`enough(num)`一样。但是我们已经知道`num`是满足`enough`函数的最小输入，所以`enough(num - 1)`必须是`False`。矛盾！与我们最初的假设相反的是正确的:`num`实际上在表中。

[](https://leetcode.com/problems/find-k-th-smallest-pair-distance/) [## 找到第 K 个最小的配对距离- LeetCode

### 提高你的编码技能，迅速找到工作。这是扩展你的知识和做好准备的最好地方…

leetcode.com](https://leetcode.com/problems/find-k-th-smallest-pair-distance/) 

非常类似于上面的 LC 668，两者都是关于寻找第 k 个最小的。就像 LC 668 一样，我们可以设计一个`enough`函数，给定一个输入`distance`，确定是否至少有 k 对的距离小于或等于`distance`。我们可以对输入数组进行排序，并用两个指针(快指针和慢指针，指向一对)对其进行扫描。两个指针都从最左端开始。如果当前指向的指针对的距离小于或等于`distance`，则这些指针之间的所有指针对都是有效的(因为数组已经排序)，我们将快速指针向前移动。否则，我们将慢速指针向前移动。当两个指针都到达最右端时，我们完成扫描，并查看总计数是否超过 k。实现如下:

显然，我们的搜索空间应该是`[0, max(nums) - min(nums)]`。现在我们准备复制粘贴我们的模板:

[](https://leetcode.com/problems/ugly-number-iii/) [## 丑陋的数字 III - LeetCode

### 提高你的编码技能，迅速找到工作。这是扩展你的知识和做好准备的最好地方…

leetcode.com](https://leetcode.com/problems/ugly-number-iii/) 

没什么特别的。仍然在寻找最小的。我们需要设计一个`enough`函数，给定一个输入`num`，确定是否有至少 n 个丑数小于或等于`num`。由于`a`可能是`b`或`c`的倍数，或者反过来，我们需要最大公约数的帮助来避免计算重复数。

[](https://leetcode.com/problems/find-the-smallest-divisor-given-a-threshold/) [## 给定一个阈值，找出最小的除数

### 给定一个整数数组 num 和一个整数阈值，我们将选择一个正整数除数，并将所有的

leetcode.com](https://leetcode.com/problems/find-the-smallest-divisor-given-a-threshold/) 

上面介绍了这么多问题，这个应该是小菜一碟。我们甚至不需要费心去设计一个`condition`函数，因为问题已经明确告诉我们需要满足什么条件。

# 结束

哇，非常感谢你坚持到最后，真的很感激。从上面的 python 代码中可以看出，它们看起来都非常相似。那是因为我一直在复制粘贴我的模板。没有例外。这是我的模板强大的有力证明。我相信每个人都可以获得这个二分搜索法模板来解决许多问题。我们所需要的只是更多的练习来建立我们发现问题单调性的能力，并设计一个漂亮的`condition`函数。

希望这有所帮助。

**参考**

*   [【c++/快速/非常清晰的解释/干净的代码】贪婪算法和二分搜索法的解决方案](https://leetcode.com/problems/split-array-largest-sum/discuss/89819/C%2B%2B-Fast-Very-clear-explanation-Clean-Code-Solution-with-Greedy-Algorithm-and-Binary-Search)
*   [使用“试错”算法解决问题](https://leetcode.com/problems/find-k-th-smallest-pair-distance/discuss/109082/Approach-the-problem-using-the-%22trial-and-error%22-algorithm)
*   [二分搜索法 101 终极二进制搜索手册——leet code](https://leetcode.com/problems/binary-search/discuss/423162/Binary-Search-101-The-Ultimate-Binary-Search-Handbook)
*   [丑女三号二分搜索法配图&二分搜索法模板— LeetCode](https://leetcode.com/problems/ugly-number-iii/discuss/387539/cpp-Binary-Search-with-picture-and-Binary-Search-Template)