---
layout: post
usekatex: true
title: Binary Tree in C++(0. What you need to know about BT)
date: 2024-09-08
tags: learning cpp algorithm
---

<!--# <span style="color: blue;"></span>-->
## <span style="color: blue;">All you need to know about Binary Tree</span>
> Focus of binary tree problems

1. Tree traversal
2. Tree properties
3. Tree modifications and operations
4. Search tree properties
5. Common ancestor
6. Search tree modifications and operations

To know about BT, following points should be mentioned.
<!--more-->
## <span style="color: blue;">Binary Tree Types</span>

In general, there are two main types of binary tree:
1. **Complete binary tree**: Every non-leaf node must have two children, if the height of a complete tree is $k$, then there are $2^k - 1$ nodes in this tree.
2. **Full binary tree**: Every node must have 0 or 2 children.

> Note: The **priority queue** is actually a **heap**, which is a complete binary tree that ensures the order of parent and child nodes.

3. Binary Search Tree(BST): It is an ordered or sorted binary tree with the key of each internal node being less than the keys in its right subtree and greater than the ones in its left subtree. Shown in the following pic.

![BST]({{site.baseurl}}/assets/img/Binary_search_tree.png)

4. Balanced binary tree or AVL（Adelson-Velsky and Landis）tree: The height differ between two children subtrees is at most 1.

![AVLT]({{site.baseurl}}/assets/img/AVL.png)

> **The underlying implementations of ```map, set, multimap, multiset``` are all balanced binary search trees, so the complexity of add and delete operations is $logN$**

> **While the underlying implementations of ```unordered_map, unordered_set,``` are hashtables**

## <span style="color: blue;">Representations of Binary Tree</span>

BTs can be organized by using pointers or arrays.

1. Linked storage:
   - data
   - pointer to left child
   - pointer to right child 

    ![BTstorage1]({{site.baseurl}}/assets/img/btlinkstorage.png)

```cpp
struct TreeNode{
    int val;
    TreeNode* leftChild;
    TreeNode* rightChild;
    TreeNode(int x) : val(x), leftChild(NULL), rightChild(NULL) {}
};
```

2. Sequential storage: using the index of array to indicate the relation. If the index of a node is ```i```, then its children will be ```left: i*2 + 1, right: i*2 + 2```

    ![BTstorage1]({{site.baseurl}}/assets/img/btstorage1.png)

    ![BTstorage2]({{site.baseurl}}/assets/img/btstorage2.png)

## <span style="color: blue;">Tree Traversal</span>

Two main traversals:
1.  Depth First Traversal(DFT): Explore the node as far as possible along each branch.
2.  Breadth First Traversal(BFT): Always attempt to visit the node at the same depth prior to next depth level.

Two ways for achieving traversal: **Recursion** and **Iteration**
- Recursion: let the function call itself, to decompositing the problem.
- Iteration: execute the code in a loop, to solve the problem by repeating operation.

> For DFT, the often used visit orders are:

1. **Pre-order**: visit the root first, then go to left child and right child respecitively.
2. **Post-order**: fist visit recursively the current node's left child, and then go to the right child, at the end visit the current node.
3. **In-order**: first visit the left child, and then go to the current node, at the end visit the left child.

The above visiting orders can be implemented in a recursive way or in an iterative way.

> For BFT, the **Level Order Traversal** is often used: All nodes in the same level are visited before going to next level. Normally, the level order traversal is implemented in an iterative way.

## <span style="color: blue;">Summary</span>

This blog briefly discussed about:

1. the binary tree types
2. how to define a binary tree data structure in C++.
3. how to traverse a binary tree.

The practical implementation of BT traversal will be discussed in 

link is: {{ 2024-09-18-Cpp_Binary_Tree_1.md | prepend: site.baseurl }}