---
layout: post
title: "LeetCode 543. Diameter of Binary Tree"
date: 2026-04-22 00:00:00 +0900
author: kang
categories: [CODE, CODE - Practice]
tags: [CODE]
pin: false
math: true
mermaid: true
---

## <b> LeetCode 543. Diameter of Binary Tree </b>
**LINK**: https://leetcode.com/problems/diameter-of-binary-tree/

#### <b>Matter</b>

You are given the root of a binary tree.

Return the length of the longest path between any two nodes in the tree.  
The path does not necessarily pass through the root.

The length is measured by the number of edges between nodes.

#### <b>Example.</b>

[1]  
Input:  
`root = [1,2,3,4,5]`

Output:  
`3`

Explanation:  
The longest path can be `[4,2,1,3]` or `[5,2,1,3]`.

[2]  
Input:  
`root = [1,2]`

Output:  
`1`

####  <b>Constraints </b>

1 <= number of nodes <= $10^4$  

-$100$ <= Node.val <= $100$

## <b>Problem analysis</b>

The diameter of a tree is the longest path between any two nodes.

For each node, the longest path passing through that node is:

```cpp
left_height + right_height
```

## <b>Solution</b>

```cpp
struct TreeNode 
{
    int val;
    TreeNode *left;
    TreeNode *right;
    TreeNode() : val(0), left(nullptr), right(nullptr) {}
    TreeNode(int x) : val(x), left(nullptr), right(nullptr) {}
    TreeNode(int x, TreeNode *left, TreeNode *right) : val(x), left(left), right(right) {}
};

class Solution {
public:
    int diameterOfBinaryTree(TreeNode* root) 
    {
        int i32Diameter = 0;
        Repeat(root, i32Diameter);
        return i32Diameter;
    }
private:
    int Repeat(TreeNode* root, int& i32Diameter)
    {
        int i32Height = 0;

        do
        {
            if(!root)
                break;

            int i32Left = Repeat(root->left, i32Diameter);
            int i32Right = Repeat(root->right, i32Diameter);
            i32Diameter = std::max(i32Diameter, i32Left + i32Right);
            i32Height = std::max(i32Left, i32Right) + 1;
        } 
        while (false);
        
        return i32Height;
    }
};
```