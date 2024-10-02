---
layout: post
usemathjax: true
title: StackAndQueue
date: 2023-06-13
tags: algorithm csharp
---

<!-- <span style="color: blue;"> </span> -->
# 栈和队列

## <span style="color: blue;"> Outline </span>

本文从三方面讲述C#中栈和队列是什么，怎么用：
- 理论基础
- 底层实现
- 使用总结
<!--more-->

## <span style="color: blue;">Basic 理论基础</span>

- 栈：LIFO last in first out
- 队列：FIFO first in first out

![stackandqueue]({{site.baseurl}}/assets/img/stackandqueue.png)

### <span style="color: blue;">栈 Stack</span>

简单来说，栈是一个操作受限的线性表，只能在栈头处进行插入和删除操作，先进后出：
- Push: 入栈
- Pop: 出栈

### <span style="color: blue;">队列 Queue</span>

同样，队列也是一个操作受限的线性表，只能在队头处进行插入，队尾处进行删除操作，先进先出：
- Enqueue: 入队
- Dequeue: 出队

## <span style="color: blue;">C#中的底层实现</span>

说实话，Stack 和 Queue 的底层实现大差不差，主要是操作方式有些许区别，数据存储基本一致

### <span style="color: blue;">栈 Stack</span>

栈实现的两种方式:
1. 顺序表存储，类似数组，大小固定，栈内元素类型也固定，扩容成本大
2. 链式存储，类似链表，大小因链式结构可以不固定，栈内元素类型也固定，扩容成本低

> 对于栈内元素类型问题，可以使用`泛型Generic`增加灵活性，不管是顺序表还是链式存储，入栈出栈操作空间时间复杂度都为`O(1)`

> C#中自带的Stack类:

```c#
1. Stack Class( System.Collections ) 
2. Stack<T> Class( System.Collections.Generic ) 

```
**Remarks**: 
- 对于上面两种实现，官方推荐使用泛型类，即 `class Stack<T>`
- `Stack<T>` 底层实现是`array`，即类似数组的<mark>顺序栈</mark>，当空间不足时需要扩容
- C# 实现了多线程安全的栈和队列分别是：

```csharp
using System.Collections.Concurrent;
   
1. Class ConcurrentStack<T>
2. Class ConcurrentQueue<T>
```

> 那么C#中如何实现的Stack呢？

C#中定义了一个泛型数组`_array`用于存储数据，同时构造函数实例化时自动初始化大小为0，并在入栈函数中实现了自动扩容，先检测是否有空闲容量，如果超出数组大小，自动扩容成原来大小的两倍（0的时候直接变为4），因此初始化的时候不用定义栈的初始大小

```csharp
// 存储数据的泛型数组
private T[] _array;

// 构造函数
public Stack()
{
    _array = _emptyArray;
    _size = 0;
    _version = 0;
}

// 入栈添加元素，并检测大小是否超出，超出则扩容
public void Push(T item)
{
    if (_size == _array.Length)
    {
        T[] array = new T[(_array.Length == 0) ? 4 : (2 * _array.Length)];
        Array.Copy(_array, 0, array, 0, _size);
        _array = array;
    }
    _array[_size++] = item;
    _version++;
}
```


### <span style="color: blue;">队列 Queue</span>

> 队列的实现方式：

1. 顺序表实现，即数组
2. 链式结构实现，链式队列

各自的优缺点在上面栈的部分也说明过了

> C#中的实现

队列其实跟栈差不多，只不过操作不同罢了，FIFO先入先出，底层实现的时候也是利用顺序表存储，即数组存储，利用两个变量标志头部和尾部实现队列

**Remarks**
1. Objects stored in a `Queue<T>` are inserted at one end and removed from the other
2. The class `Queue<T>` implements a generic queue as a **circular array** 利用数组模仿循环列表来实现队列
3. `Enqueue(T item)` 方法向队列中加入一个元素，并在其中实现了自动扩容

> 具体源码

C# 中`Queue`相比`Stack`多定义了一个成员来标志队尾，其他原理和栈差不多：
```csharp
// 队中元素，头和尾标志符，大小，version按我的理解是指队列扩容或更新的次数
private T[] _array;
private int _head;       // First valid element in the queue
private int _tail;       // Last valid element in the queue
private int _size;       // Number of elements.
private int _version;

// 默认的无参构造函数
public Queue() {
    _array = _emptyArray;            
}

// 指定容量的有参构造函数
// Creates a queue with room for capacity objects. The default grow factor
// is used.
//
/// <include file='doc\Queue.uex' path='docs/doc[@for="Queue.Queue1"]/*' />
public Queue(int capacity) {
    if (capacity < 0)
        ThrowHelper.ThrowArgumentOutOfRangeException(ExceptionArgument.capacity, ExceptionResource.ArgumentOutOfRange_NeedNonNegNumRequired);
    
    _array = new T[capacity];
    _head = 0;
    _tail = 0;
    _size = 0;
}

// Enqueue 方法，添加元素，实现自动扩容
// Adds item to the tail of the queue.
//
/// <include file='doc\Queue.uex' path='docs/doc[@for="Queue.Enqueue"]/*' />
public void Enqueue(T item) {
    if (_size == _array.Length) {
        int newcapacity = (int)((long)_array.Length * (long)_GrowFactor / 100);
        if (newcapacity < _array.Length + _MinimumGrow) {
            newcapacity = _array.Length + _MinimumGrow;
        }
        SetCapacity(newcapacity);
    }

    _array[_tail] = item;
    _tail = (_tail + 1) % _array.Length;
    _size++;
    _version++;
}
```

