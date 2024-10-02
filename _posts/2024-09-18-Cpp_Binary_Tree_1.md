---
layout: post
usemathjax: true
title: Binary Tree in C++(1. Recusive Traversal)
date: 2024-09-18
tags: learning cpp algorithm
---

<!--# <span style="color: blue;"></span>-->
## <span style="color: blue;">Recursively traverse a Binary Tree</span>
In last post [BinaryTree_0]({% link _posts/2024-09-08-Cpp_Binary_Tree_0.md %}), we have discussed the DFT and BFT for traversing a BT. For DFT, recursive way and iterative way can both fulfill our purpose.

In this post, we will briefly introduce the recursive way for DFT.
<!--more-->

## <span style="color: blue;">Methodology For Tree Traversal</span>

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

> Inorder Traversal

So as the inorder traversal:
```cpp
class Solution{

    public:
        Solution() {}
        ~Solution() {}
        void inorderT(vector<int> &res, TreeNode *root){
            if (root == nullptr)
            {
                return ;
            }else{
                inorderT(res, root->left);
                res.push_back(root->val);
                inorderT(res, root->right);
            }
        }
        vector<int> inorderTraversal(TreeNode *root) {
            vector<int> res;
            inorderT(res, root);

            return res;
        }
};
```