---
layout: post
title: Cpp_Stack_Queue
date: 2023-09-03
tags: learning cpp algorithm
---

<!--# <span style="color: blue;"></span>-->
## <span style="color: blue;">C++ Learning: Stack & Queue</span>
 
First thing to know, C++ is technical and hard to understand. :>

In STL, stack and queue are implemented using container adapters

- Container: Containers are data types from STL that can contain data.
- Adapter: Adapters are data types from STL that adapt a container to provide specific interface.

<!--more-->
## <span style="color: blue;">Stack的底层</span>

因为上述说到，C++中的栈是Container Adapter (**容器适配器**)，意思就是栈是建立在底层容器的基础上实现的，对外提供相应的接口，比如push/pop，底层容器是可插拔的（也就是说我们可以控制使用哪种容器来实现栈的功能）

> STL 中栈是用什么容器实现的？

The standard container classes vector, deque and list fulfill these requirements. By default, if no container class is specified for a particular stack class instantiation, the standard container deque is used.

栈的底层实现可以是vector，deque，list 都是可以的， 主要就是数组和链表的底层实现

常用的SGI版本的STL，如果没有指定底层实现的话，默认是以deque为缺省情况下栈的底层结构，同时**stack栈不提供走访功能，也不提供迭代器(iterator)**

```cpp
// constructing stacks
#include <iostream>       // std::cout
#include <stack>          // std::stack
#include <vector>         // std::vector
#include <deque>          // std::deque

int main ()
{
  std::deque<int> mydeque (3,100);          // deque with 3 elements
  std::vector<int> myvector (2,200);        // vector with 2 elements

  std::stack<int> first;                    // empty stack
  std::stack<int> second (mydeque);         // stack initialized to copy of deque

  std::stack<int,std::vector<int> > third;  // empty stack using vector
  std::stack<int,std::vector<int> > fourth (myvector);

  std::cout << "size of first: " << first.size() << '\n';
  std::cout << "size of second: " << second.size() << '\n';
  std::cout << "size of third: " << third.size() << '\n';
  std::cout << "size of fourth: " << fourth.size() << '\n';

  return 0;
}

/* Output:
size of first: 0
size of second: 3
size of third: 0
size of fourth: 2
*/
```

### <span style="color: blue;">Stack的接口</span>

|member fucntion|Description|
|--|--|
|(constructor)|Construct stack 构造函数，可指定底层实现结构|
|empty|Test whether the stack is empty 返回值为bool类型|
|size|Returns the number of elements in the stack 返回值为无符号整型unsigned integral|
|top|Returns a reference to the top element in the stack 返回一个栈顶元素的引用|
|push|return val is none, insert an element to the top of stack|
|pop|return val is none, remove the top element to from the stack|
|emplace|return val is none, 跟push差不多，the new element is constructed in place passing args as the arguments for its constructor|
|swap|交换两个同类型的栈，swap the underlying containers|

## <span style="color: blue;">Queue的底层</span>

FIFO queue 先入先出，同样是容器适配器 (Container Adapter)，即基于底层容器实现的数据结构，对外提供相应的接口，push/pop

The standard container classes deque and list fulfill these requirements. By default, if no container class is specified for a particular queue class instantiation, the standard container deque is used.

同样不允许有遍历行为，不提供迭代器

```cpp
// constructing queues
#include <iostream>       // std::cout
#include <deque>          // std::deque
#include <list>           // std::list
#include <queue>          // std::queue

int main ()
{
  std::deque<int> mydeck (3,100);        // deque with 3 elements
  std::list<int> mylist (2,200);         // list with 2 elements

  std::queue<int> first;                 // empty queue
  std::queue<int> second (mydeck);       // queue initialized to copy of deque

  std::queue<int,std::list<int> > third; // empty queue with list as underlying container
  std::queue<int,std::list<int> > fourth (mylist);

  std::cout << "size of first: " << first.size() << '\n';
  std::cout << "size of second: " << second.size() << '\n';
  std::cout << "size of third: " << third.size() << '\n';
  std::cout << "size of fourth: " << fourth.size() << '\n';

  return 0;
}

/* Output:
size of first: 0
size of second: 3
size of third: 0
size of fourth: 2
*/
```

### <span style="color: blue;">Stack的接口</span>

|member fucntion|Description|
|--|--|
|front|Returns a reference to the next element in the queue. 返回队首元素|
|back|Returns a reference to the last element in the queue. 返回队尾元素|

## <span style="color: blue;">总结</span>

简单总结就是，STL提供了队列和栈的实现，queue和stack，但只能使用特定接口，不能遍历，且没有迭代器，同时the underlying container can be specified. 想要理解他们，就要先学好底层的容器，比如 list, deque 和 vector 等

