---
layout: post
title: LeetCode_Stack_Queue_Problem
date: 2023-06-15
tags: leetcode algorithm csharp learning
---

This blog is the learning note of solving leetcode stack&queue related problems

<!--more-->
<!-- <span style="color: blue;"> </span> -->

# <span style="color: blue;"> 栈和队列 Stack & Queue </span>

## <span style="color: blue;"> 232. Implement queue using stacks 利用栈实现队列 </span>

### <span style="color: blue;"> Problem Description 问题描述 </span>

> 利用栈来实现队列的功能，可以使用的方法均为栈方法：

1. `void push(int x)` 
2. `int pop()`
3. `int peek()`
4. `boolean empty()`

### <span style="color: blue;"> 例子 </span>

```
Input
["MyQueue", "push", "push", "peek", "pop", "empty"]
[[], [1], [2], [], [], []]
Output
[null, null, null, 1, 1, false]

Explanation
MyQueue myQueue = new MyQueue();
myQueue.push(1); // queue is: [1]
myQueue.push(2); // queue is: [1, 2] (leftmost is front of the queue)
myQueue.peek(); // return 1
myQueue.pop(); // return 1, queue is [2]
myQueue.empty(); // return false

```
> 思考：如何让其变为 O(1) 的时间复杂度?

### <span style="color: blue;"> 解题思路 </span>

我们可以准备两个栈，所谓输入栈，输出栈，基本思想就是利用入栈元素顺序和出栈元素顺序相反的规则，利用两个栈进行两次顺序反转使得结果类似队列，如下图所示：

![232]({{site.baseurl}}/assets/img/232.gif)


