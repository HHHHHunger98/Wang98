---
layout: post
usemathjax: true
title: Cpp_Map_Unordered_Map
date: 2023-09-13
tags: learning cpp algorithm
---

<!--# <span style="color: blue;"></span>-->
## <span style="color: blue;">C++ Learning: Map Container</span>

### <span style="color: blue;">What is a map, and what is an unordered map</span>

maps and unordered maps are both associative container that can store the key-value pairs. However, their underlying implementation are different, which supports their different use for different scenarios.

`map`和`unordered_map`都是存储键值对的数据结构，但是因为他们的底层实现不一样，相应的应用场景也有所不同
| |map| unordered_map|
|--|----|----|
|Ordering|increasing order of keys(by default)|no ordering|
|Implementation|Self balancing BST like Red-Black Tree|Hash Table|  
|search time|log(n)| O(1) -> Average, O(n) -> Worst Case|
|Insertion time  | log(n) + Rebalance  | Same as search|
|Deletion time   | log(n) + Rebalance  | Same as search|

### <span style="color: blue;">Map的接口</span>

## <span style="color: blue;">总结</span>

