---
title: Leetcode 623 - Add One Row to Tree
date: 2022-10-05 12:41:11 +0800
categories: [Leetcode, Medium]
tags: [tree, recursion]     # TAG names should always be lowercase
---

Okay, [it](https://leetcode.com/problems/add-one-row-to-tree/) is binary tree again. Taking recursion as the first approach, it is very easy to override the original function signature by adding a parameter `side` defining whether the current node should be extended in left or right (i.e., 0 => left, 1 => right). The rest is trivial.

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

function addOneRow(root: TreeNode | null, val: number, depth: number, side: number = 0): TreeNode | null {
  if ( depth === 1 ) {
    if ( side === 0 ) {
      return new TreeNode( val, root, null )
    } else {
      return new TreeNode( val, null, root )
    }
  } else if ( depth < 1) {
    return root
  }
  if ( root === null ) return null;
  return new TreeNode(
    root.val,
    addOneRow( root.left, val, depth - 1, 0 ),
    addOneRow( root.right, val, depth - 1, 1 )
  )
};
```

