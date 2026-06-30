---
comments: true
difficulty: Medium
edit_url: https://github.com/doocs/leetcode/edit/main/lcci/04.08.First%20Common%20Ancestor/README_EN.md
---

<!-- problem:start -->

# [04.08. First Common Ancestor](https://leetcode.cn/problems/first-common-ancestor-lcci)

[Chinese Version](/lcci/04.08.First%20Common%20Ancestor/README.md)

## Description

<!-- description:start -->

<p>Design an algorithm and write code to find the first common ancestor of two nodes in a binary tree. Avoid storing additional nodes in a data structure. NOTE: This is not necessarily a binary search tree.</p>

<p>For example, Given the following tree: root = [3,5,1,6,2,0,8,null,null,7,4]</p>

<pre>

    3

   / \

  5   1

 / \ / \

6  2 0  8

  / \

 7   4

</pre>

<p><strong>Example 1:</strong></p>

<pre>

<strong>Input:</strong> root = [3,5,1,6,2,0,8,null,null,7,4], p = 5, q = 1

<strong>Input:</strong> 3

<strong>Explanation:</strong> The first common ancestor of node 5 and node 1 is node 3.</pre>

<p><strong>Example 2:</strong></p>

<pre>

<strong>Input:</strong> root = [3,5,1,6,2,0,8,null,null,7,4], p = 5, q = 4

<strong>Output:</strong> 5

<strong>Explanation:</strong> The first common ancestor of node 5 and node 4 is node 5.</pre>

<p><strong>Notes:</strong></p>

<ul>
	<li>All node values are pairwise distinct.</li>
	<li>p, q are different node and both can be found in the given tree.</li>
</ul>

<!-- description:end -->

## Solutions

<!-- solution:start -->

### Solution 1: Recursion

First, we check if the root node is null or if the root node is equal to $\textit{p}$ or $\textit{q}$. If so, we return the root node directly.

Then, we recursively search the left and right subtrees to get $\textit{left}$ and $\textit{right}$, respectively. If both $\textit{left}$ and $\textit{right}$ are not null, it means $\textit{p}$ and $\textit{q}$ are in the left and right subtrees, respectively, so the root node is the lowest common ancestor. Otherwise, if either $\textit{left}$ or $\textit{right}$ is null, it means both $\textit{p}$ and $\textit{q}$ are in the non-null subtree, so the root node of the non-null subtree is the lowest common ancestor.

The time complexity is $O(n)$, and the space complexity is $O(n)$. Here, $n$ is the number of nodes in the binary tree.

<!-- tabs:start -->

#### Java

```java
/**
 * Definition for a binary tree node.
 * public class TreeNode {
 *     int val;
 *     TreeNode left;
 *     TreeNode right;
 *     TreeNode(int x) { val = x; }
 * }
 */
class Solution {
    public TreeNode lowestCommonAncestor(TreeNode root, TreeNode p, TreeNode q) {
        if (root == null || root == p || root == q) {
            return root;
        }
        TreeNode left = lowestCommonAncestor(root.left, p, q);
        TreeNode right = lowestCommonAncestor(root.right, p, q);
        return left == null ? right : (right == null ? left : root);
    }
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
