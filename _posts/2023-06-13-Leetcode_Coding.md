---
layout: post
usemathjax: false
title: Leetcode_Coding
date: 2023-06-13
tags: algorithm csharp cpp
---

<!-- <span style="color: blue;"> </span> -->
This article is a learning diary of solving leetcode problems

<!--more-->

# Problem 142 Linked List Cycle II
## Description: 
> Given the **head** of a linked list, return the node where the cycle begins. If there is no cycle, return **null**.
## Input Examples：
![142.1]({{site.baseurl}}/assets/img/142.1.PNG)
## Solution:
> ① The first thing to think about is **whether there is a loop existing in the linked list?**
> 
> ② If there is a loop in the list, **how can I find the entrance of the loop?**

To answer question ① , we can use a classic method: **fast and slow pointer**

Fast and slow pointer:
![142.2]({{site.baseurl}}/assets/img/142.2.gif)
As the picture shows, the fast pointer moves faster and will get into the loop first. **The most important fact is  that the fast pointer and slow pointer will definitely encounter somewhere in the loop**. Base on this fact we can solve the problem ① easily:

1. Define two pointers: slow pointer and fast pointer.
2. Let them move in different step.
3. If (slow == fast) return true;
4. If (fast == null) return false;

To answer question ② , we can extend a little bit the fast and slow pointer technique.

Let's say the there are **x** nodes not in the loop, and **y** representing the number of the nodes from the entrance of the loop till the encounter point, while **z** is the number of the nodes from the encounter point to the loop entrance, as the following picture shows.
![142.3]({{site.baseurl}}/assets/img/142.3.png)

Assume each time the slow pointer will move 1 step forwards, while the fast pointer moves 2 steps.

The total moving length of slow pointer is: x + y

The total moving length of fast pointer is: x + y + n * (z + y), where n is the times the fast pointer go through the entire loop.

and as we assume that fast pointer move 1 more step each time. So we have 2 * (x + y) = x + y + n * (z + y). Then we have:

**x + y = n * (z + y)**, because **x** is all we want, we can finally have this equation:

**x = (n - 1) * (y + z) + z**, where n is greater than 1, cause the fast point should move at least one more loop step to catch the slow pointer.

That means **if we let one pointer A move from the head, and the other pointer B move from the encounter point, both of them move 1 step forwards each time, then they will finally encounter each other literally at the entrance point !!!**

---

# Hash Table Summary

## Basic

- **The most essential reason to use a hash table is we can quickly (in Big O(1) time) tell whether an element is in a set?**

- For the use of HashTable, there are two things to know about:
  1. **Hash Function**: the function which can map the key to an index of the HashTable (normally fixed size).
  2. **Hash Collision**: the situation that two elements have the same hash code (the keys are the same, meaning that they are mapped to the same index of HashTable). To use HashTable, we need to know how to handle such situations.

In C# there is actually an implementation of HashTable: class <mark>**Hashtable**</mark>, it stores key/value pairs where the key cannot be null and the value can be. Even though the class Hashtable exists, we are recommended to use the generic <mark>**Dictionary\<TKey,TValue\>**</mark> for [two big reasons](https://github.com/dotnet/platform-compat/blob/master/docs/DE0006.md)
1. Error tolerance
2. Better performant

Another hash-based data structure is the <mark>**HashSet\<T\>**</mark>, In simple terms, the **HashSet\<T\>** class can be thought of as a **Dictionary\<TKey,TValue\>** collection without values.

## Handle Hash Collision

**There are normally two ways to handle hash collisions**
1. 拉链法, Hash collisions are solved by make each bucket contain a linked list of elements with same hash code.
2. 线性探测法, It will start at the original hash value position and move sequentially through the hash table util the first empty slot is found.
   
