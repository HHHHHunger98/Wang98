---
layout: post
title: Cpp_Binary_Tree
date: 2024-09-08
tags: learning cpp algorithm
---

<!--# <span style="color: blue;"></span>-->
## <span style="color: blue;">C++: Binary Tree</span>
> Focus of binary tree problems

1. Tree traversal
2. Tree properties
3. Tree modifications and operations
4. Search tree properties
5. Common ancestor
6. Search tree modifications and operations

In this blog, the problems related to binary tree will be discussed.
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
    ![BTstorage2]({{site.baseurl}}/assets/img/btstorge2.png)

## <span style="color: blue;">Tree Traversal</span>

Two main traversals:
1.  Depth First Traversal(DFT): Explore the node as far as possible along each branch.
2.  Breadth First Traversal(BFT): Always attempt to visit the node at the same depth prior to next depth level.

Two ways for achieving traversal: **Recursion** and **Iteration**
- Recursion: let the function call itself, to decompositing the problem.
- Iteration: execute the code in a loop, to solve the problem by repeating operation.
  
The visit order is processed recursively.
1. Pre-order: visit the root first, then go to left child and right child respecitively.
2. Post-order: fist visit recursively the current node's left child, and then go to the right child, at the end visit the current node.
3. In-order: first visit the left child, and then go to the current node, at the end visit the left child.

### Methodology For Tree Traversal

#### Recursion

> Three elements for recursively traverse the tree:
1. Parameters and the return value
   What parameters should be handled, what tpye of value should be returned to previous recursion.
2. Loop exit conditon
    Sometimes we may encounter problems like stack overflow, normally it's because of an inappropriate stop condition.
3. Operation to be executed in one recursion
    Specify the operation for each recursion.

> Preorder Traversal

```cpp
// as we want to print out all the value of the node we visited in a preorder form, the result should be added into vector<int>.
void preTree(vector<int> &res, TreeNode *root) {
    // exit condition, when the current node is null, then we return.
    if (root == NULL)
    {
        return;
    }else {
        // operation for each recursion.
        res.push_back(root->val);
        preTree(res, root->left);
        preTree(res, root->right);
    }
}

vector<int> preorderTraversal(TreeNode* root) {
    vector<int> res;
    preTree(res, root);
    return res;
}
```

> Postorder Traversal

Postorder traversal is just a little bit different from the preorder. We traverse first the whole tree util the leaf node. And then record the value.

```cpp
...
        postTree(cur->left, res);
        postTree(cur->right, res);
        res.push_back(cur->val);
...

```