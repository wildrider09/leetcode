---
comments: true
difficulty: Easy
edit_url: https://github.com/doocs/leetcode/edit/main/lcci/04.04.Check%20Balance/README_EN.md
---

<!-- problem:start -->

# [04.04. Check Balance](https://leetcode.cn/problems/check-balance-lcci)

[Chinese Version](/lcci/04.04.Check%20Balance/README.md)

## Description

<!-- description:start -->

<p>Implement a function to check if a binary tree is balanced. For the purposes of this question, a balanced tree is defined to be a tree such that the heights of the two subtrees of any node never differ by more than one.</p>

<p><br />

<strong>Example 1:</strong></p>

<pre>

Given tree [3,9,20,null,null,15,7]

    3

   / \

  9  20

    /  \

   15   7

return true.</pre>

<p><strong>Example 2:</strong></p>

<pre>

Given [1,2,2,3,3,null,null,4,4]

      1

     / \

    2   2

   / \

  3   3

 / \

4   4

return&nbsp;false.</pre>

<p>&nbsp;</p>

<!-- description:end -->

## Solutions

<!-- solution:start -->

### Solution 1: Recursion (Post-order Traversal)

We design a function $dfs(root)$, which returns the height of the tree with $root$ as the root node. If the tree with $root$ as the root node is balanced, it returns the height of the tree, otherwise, it returns $-1$.

The execution logic of the function $dfs(root)$ is as follows:

- If $root$ is null, then return $0$.
- Otherwise, we recursively call $dfs(root.left)$ and $dfs(root.right)$, and check whether the return values of $dfs(root.left)$ and $dfs(root.right)$ are $-1$. If not, we check whether $abs(dfs(root.left) - dfs(root.right)) \leq 1$ holds. If it holds, then return $max(dfs(root.left), dfs(root.right)) + 1$, otherwise return $-1$.

In the main function, we only need to call $dfs(root)$, and check whether its return value is $-1$. If not, return `true`, otherwise return `false`.

The time complexity is $O(n)$, and the space complexity is $O(n)$, where $n$ is the number of nodes in the binary tree.

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
    public boolean isBalanced(TreeNode root) {
        return dfs(root) >= 0;
    }

    private int dfs(TreeNode root) {
        if (root == null) {
            return 0;
        }
        int l = dfs(root.left);
        int r = dfs(root.right);
        if (l < 0 || r < 0 || Math.abs(l - r) > 1) {
            return -1;
        }
        return Math.max(l, r) + 1;
    }
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
