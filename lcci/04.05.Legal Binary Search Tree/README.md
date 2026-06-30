---
comments: true
difficulty: Medium
edit_url: https://github.com/doocs/leetcode/edit/main/lcci/04.05.Legal%20Binary%20Search%20Tree/README_EN.md
---

<!-- problem:start -->

# [04.05. Legal Binary Search Tree](https://leetcode.cn/problems/legal-binary-search-tree-lcci)

[Chinese Version](/lcci/04.05.Legal%20Binary%20Search%20Tree/README.md)

## Description

<!-- description:start -->

<p>Implement a function to check if a binary tree is a binary search tree.</p>

<p><strong>Example&nbsp;1:</strong></p>

<pre>

<strong>Input:</strong>

    2

   / \

  1   3

<strong>Output:</strong> true

</pre>

<p><strong>Example&nbsp;2:</strong></p>

<pre>

<strong>Input:</strong>

    5

   / \

  1   4

&nbsp;    / \

&nbsp;   3   6

<strong>Output:</strong> false

<strong>Explanation:</strong> Input: [5,1,4,null,null,3,6].

&nbsp;    the value of root node is 5, but its right child has value 4.</pre>

<!-- description:end -->

## Solutions

<!-- solution:start -->

### Solution 1: Recursion

We can perform a recursive in-order traversal on the binary tree. If the result of the traversal is strictly ascending, then this tree is a binary search tree.

Therefore, we use a variable `prev` to save the last node we traversed. Initially, `prev = -∞`. Then we recursively traverse the left subtree. If the left subtree is not a binary search tree, we directly return `False`. Otherwise, we check whether the value of the current node is greater than `prev`. If not, we return `False`. Otherwise, we update `prev` to the value of the current node, and then recursively traverse the right subtree.

The time complexity is $O(n)$, and the space complexity is $O(n)$. Where $n$ is the number of nodes in the binary tree.

<!-- tabs:start -->

#### Java

```java
/**
 * Definition for a binary tree node.
 * public class TreeNode {
 *     int val;
 *     TreeNode left;
 *     TreeNode right;
 *     TreeNode() {}
 *     TreeNode(int val) { this.val = val; }
 *     TreeNode(int val, TreeNode left, TreeNode right) {
 *         this.val = val;
 *         this.left = left;
 *         this.right = right;
 *     }
 * }
 */
class Solution {
    private TreeNode prev;

    public boolean isValidBST(TreeNode root) {
        return dfs(root);
    }

    private boolean dfs(TreeNode root) {
        if (root == null) {
            return true;
        }
        if (!dfs(root.left)) {
            return false;
        }
        if (prev != null && prev.val >= root.val) {
            return false;
        }
        prev = root;
        return dfs(root.right);
    }
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
