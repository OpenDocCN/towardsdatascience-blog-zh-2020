# C++中的数据结构—第 1 部分

> 原文：<https://towardsdatascience.com/data-structures-in-c-part-1-b64613b0138d?source=collection_archive---------1----------------------->

## 在 C++中实现通用数据结构

C++是支持创建类的 C 编程语言的扩展，因此被称为“ *C with classes* ”。它用于创建高性能应用程序，并为我们提供对计算资源的高级控制。在本文中，我将向您展示我在上一篇文章[每个程序员都必须知道的 8 种常见数据结构](/8-common-data-structures-every-programmer-must-know-171acf6a1a42)中讨论的 4 种数据结构的 C++实现。

[](/8-common-data-structures-every-programmer-must-know-171acf6a1a42) [## 每个程序员都必须知道的 8 种常见数据结构

### 数据结构是一种在计算机中组织和存储数据的专门方法，以这种方式我们可以执行…

towardsdatascience.com](/8-common-data-structures-every-programmer-must-know-171acf6a1a42) 

让我们深入研究代码。

![](img/5749be8dc9db4f357682eca64e9e7c2b.png)

由[麦克斯韦·纳尔逊](https://unsplash.com/@maxcodes?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)在 [Unsplash](https://unsplash.com/s/photos/c%2B%2B-programming?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) 上拍摄的照片

# 1.数组

一个**数组**是一个固定大小的结构，可以保存相同数据类型的项目。数组是有索引的，这意味着随机访问是可能的。在许多编程语言中，数组通常以本机数据结构的形式出现。然而，不要把`array`和像 python 这样的语言中的`list`这样的数据结构混淆。让我们看看数组是用 C++表示的；

```
// simple declaration
int array[] = {1, 2, 3, 4, 5 };
// in pointer form (refers to an object stored in heap)
int * array = new int[5]; 
```

然而，我们已经习惯了更友好的`vector<T>`数据结构，我们可以将数据推送到这种结构，而不用担心数据的大小。让我们看看如何实现一个我们自己的`list`数据结构，它可以自己调整大小。

```
using namespace std;

class DynamicArray
{
private:
    int size_;
    int max_;
    int *arrayholder_;

public:
    DynamicArray()
    {
        this->size_ = 0;
        this->max_ = 5;
        this->arrayholder_ = new int[5];
    }

    ~DynamicArray()
    {
        delete[] this->arrayholder_;
    }

    int size()
    {
        return this->size_;
    }

    int& operator[](int i) 
    {
        assert(i < this->size_);
        return this->arrayholder_[i];
    }

    void add(int n)
    {
        if (this->max_ < this->size_ + 1)
        {
            this->max_ *= 2;
            int *tmp_ = new int[this->max_];

            for (size_t i = 0; i < this->size_; i++)
            {
                tmp_[i] = this->arrayholder_[i];

            }
            delete[] this->arrayholder_;
            this->arrayholder_ = tmp_;
            this->arrayholder_[this->size_] = n;
            this->size_ += 1;
        }
        else 
        {
            this->arrayholder_[this->size_] = n;
            this->size_ += 1;
        }
    }
};

int main(int argc, char **argv)
{
    DynamicArray darray;
    vector<int> varray;

    for (size_t i = 0; i <= 15; i++)
    {
        darray.add(i);
    }
    return 0;
}
```

我们可以看到上面的`DynamicArray`的初始大小为 5。同样在`add`函数中，我们可以看到，如果我们已经达到了最大容量，数组大小将被加倍和复制。这就是`vector<T>`数据结构在现实中的工作方式。通常，基数在 30 左右。知道这一点可能会在面试中有所帮助。

还有一点需要注意的是，我们同时使用了**构造函数**和**析构函数**。这是因为我们有一个指针指向随着数组扩展而分配的内存。必须释放这些分配的内存以避免溢出。这个实现的美妙之处在于用户不需要知道任何关于丑陋指针的事情。重载`operator[]`允许像本地数组一样进行索引访问。有了这些知识，让我们继续下一个数据结构，**链表**。

# 2.链接列表

**链表**是一个顺序结构，由一系列相互链接的线性项目组成。您只需要知道遍历这个数据结构的链的一端。在大小频繁变化的情况下，使用数组可能没有优势，除非随机访问数据(除非实现收缩操作，否则扩展会导致更长的复制时间并使用更多内存)。因此，链表可以被认为是一种支持频繁大小变化和顺序访问的数据结构。

让我们看一下我们的实现。

```
using namespace std;

template <typename T>
class Node
{
    public:
    T value;
    Node *next;
    Node *previous;

    Node(T value)
    {
        this->value = value;
    }
};

template <typename T>
class LinkedList
{
    private:
    int size_;
    Node<T> *head_ = NULL;
    Node<T> *tail_ = NULL;
    Node<T> *itr_ = NULL;

    public:
    LinkedList()
    {
        this->size_ = 0;
    }

    void append(T value)
    {
        if (this->head_ == NULL)
        {
            this->head_ = new Node<T>(value);
            this->tail_ = this->head_;
        }
        else
        {
            this->tail_->next = new Node<T>(value);
            this->tail_->next->previous = this->tail_;
            this->tail_ = this->tail_->next;
        }
        this->size_ += 1;
    }

    void prepend(T value)
    {
        if (this->head_ == NULL)
        {
            this->head_ = new Node<T>(value);
            this->tail_ = this->head_;
        }
        else
        {
            this->head_->previous = new Node<T>(value);
            this->head_->previous->next = this->head_;
            this->head_ = this->head_->previous;
        }
        this->size_ += 1;
    }

    Node<T> * iterate()
    {
        if (this->itr_ == NULL) 
        {
            this->itr_ = this->head_;
        } 
        else 
        {
            this->itr_ = this->itr_->next;
        }
        return this->itr_;
    }

    T ptr()
    {
        return this->itr_->value;
    }

    void resetIterator()
    {
        this->tail_ = NULL;
    }
};

int main(int argc, char **argv)
{
    LinkedList<int> llist;
    llist.append(10);
    llist.append(12);
    llist.append(14);
    llist.append(16);
    llist.prepend(5);
    llist.prepend(4);
    llist.prepend(3);
    llist.prepend(2);
    llist.prepend(1);

    cout << "Printing Linked List" << endl;

    while(llist.iterate() != NULL)
    {
        cout << llist.ptr() << "\t";
    }
    cout << endl;

    return 0;
}
```

这里我们使用了一个叫做**节点**的支持结构。该**节点**将携带指向**下一个**项目和**上一个**项目的指针。通常有下一个项目**就足够了。但是同时拥有 **next** 和 **previous** 可以提高 append 和 prepend 的性能，因为我们可以对两端进行访问。 ***追加*** 意味着我们将更新 ***tail*** 指针的下一个元素，然后更新 tail 作为添加的项。反之， ***前置*** 会创建一个新元素，并将其**下一个**项设置为当前 ***头*** 。请注意，我没有包括内存清理操作，因为我们不处理显式删除，并使代码更简单。但是，必须将它们放在单独的析构函数中，以避免内存泄漏。此外，我们使用一个简单的迭代器来打印项目。**

# 3.大量

一个**堆栈**是一个 **LIFO** (后进先出——最后放置的元素可以首先访问)结构。尽管该数据结构具有不同的行为，但这可以被认为是仅具有**头**或对**顶**元素的访问的链表的派生。

```
#include <iostream>

using namespace std;

template <typename T>
class Node
{
public:
    T value;
    Node *next;

    Node(T value)
    {
        this->value = value;
    }
};

template <typename T>
class Stack
{
private:
    int size_;
    Node<T> *top_ = NULL;
    Node<T> *itr_ = NULL;

public:
    Stack()
    {
        this->size_ = 0;
    }

    void push(T value)
    {
        if (this->top_ == NULL)
        {
            this->top_ = new Node<T>(value);
        }
        else
        {
            Node<T> *tmp = new Node<T>(value);
            tmp->next = this->top_;
            this->top_ = tmp;
        }
        this->size_ += 1;
    }

    Node<T> *pop()
    {
        Node<T> *tmp = this->top_;

        this->top_ = this->top_->next;
        this->size_ -= 1; return tmp;
    }

    Node<T> *peek()
    {
        return this->top_;
    }

    int size()
    {
        return this->size_;
    }

    Node<T> *iterate()
    {
        if (this->itr_ == NULL)
        {
            this->itr_ = this->top_;
        }
        else
        {
            this->itr_ = this->itr_->next;
        }
        return this->itr_;
    }

    T ptr()
    {
        return this->itr_->value;
    }

    void resetIterator()
    {
        this->itr_ = NULL;
    }
};

int main(int argc, char **argv)
{
    Stack<int> stk1;
    stk1.push(10);
    stk1.push(12);
    stk1.push(14);
    stk1.push(16);
    stk1.push(5);
    stk1.push(4);
    stk1.push(3);
    stk1.push(2);
    stk1.push(1);

    cout << "Printing Stack" << endl;

    while (stk1.iterate() != NULL)
    {
        cout << stk1.ptr() << "\t";
    }
    cout << endl; return 0;
}
```

注意，**节点**只有对**下一个**项目的引用。添加一个条目会更新数据结构的顶部。此外，移除和取回也是从顶部进行的。为此，我们分别使用`pop()`和`top()`方法。

# 4.行列

一个**队列**是一个 **FIFO** (先进先出——放在最前面的元素可以最先被访问)结构。这可以被认为是堆栈的相反情况。简单来说，它是一个链表，我们从一端添加，从另一端读取。这模拟了车道上的真实世界。这里我们可以把一个**节点**结构想成如下。

```
template <typename T>
class Node
{
    public:
    T value;
    Node *next;
    Node *previous;

    Node(T value)
    {
        this->value = value;
    }
};
```

主要的数据结构是:

```
template <typename T>
class Queue
{
    private:
    int size_;
    Node<T> *head_ = NULL;
    Node<T> *tail_ = NULL;

    public:
    Queue()
    {
        this->size_ = 0;
    }

    void enqueue(T value)
    {
        if (this->head_ == NULL)
        {
            this->head_ = new Node<T>(value);
            this->tail_ = this->head_;
        }
        else
        {
            this->tail_->next = new Node<T>(value);
            this->tail_->next->previous = this->tail_;
            this->tail_ = this->tail_->next;
        }
        this->size_ += 1;
    }

    Node<T> dequeue()
    {
        Node<T> *tmp = this->tail_;

        this->tail_ = this->tail->previous;
        this->tail_->next = NULL; this->size_ -= 1; return tmp;
    }
};
```

# 最后的想法

我们已经使用 C++编程语言实现了 4 种常见的数据结构。我将在以后的文章中介绍其余的实现。

希望你们都觉得这些数组、链表、栈和队列的 C++实现很有用。你可以通过下面的链接查看我的其他关于数据结构的文章。

[](/8-useful-tree-data-structures-worth-knowing-8532c7231e8c) [## 值得了解的 8 种有用的树数据结构

### 8 种不同树形数据结构的概述

towardsdatascience.com](/8-useful-tree-data-structures-worth-knowing-8532c7231e8c) [](/self-balancing-binary-search-trees-101-fc4f51199e1d) [## 自平衡二分搜索法树 101

### 自平衡二分搜索法树简介

towardsdatascience.com](/self-balancing-binary-search-trees-101-fc4f51199e1d) 

感谢您的阅读！如果你觉得这篇文章有用，请在你的网络中分享。

干杯！