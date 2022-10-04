---
title: Leetcode 112 - Path Sum
date: 2022-10-04 16:59:51 +0800
categories: [Leetcode, Easy]
tags: [tree, recursion]     # TAG names should always be lowercase
---

No wonder that recursion is a super fit for solving [question](https://leetcode.com/problems/path-sum/) of binary tree. Simply setting the base case of true and false, the rest is trivial.

```typescript
/**
 * Definition for a binary tree node.
 * class TreeNode {
 *     val: number
 *     left: TreeNode | null
 *     right: TreeNode | null
 *     constructor(val?: number, left?: TreeNode | null, right?: TreeNode | null) {
 *         this.val = (val===undefined ? 0 : val)
 *         this.left = (left===undefined ? null : left)
 *         this.right = (right===undefined ? null : right)
 *     }
 * }
 */

function hasPathSum(root: TreeNode | null, targetSum: number): boolean {
  if ( root === null ) return false
  if ( root.left === null && root.right === null && targetSum === root.val) return true
  return hasPathSum(root.left, targetSum - root.val) || hasPathSum(root.right, targetSum - root.val)
};
```

