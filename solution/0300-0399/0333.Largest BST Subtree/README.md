---
comments: true
difficulty: Medium
edit_url: https://github.com/doocs/leetcode/edit/main/solution/0300-0399/0333.Largest%20BST%20Subtree/README_EN.md
tags:
    - Tree
    - Depth-First Search
    - Binary Search Tree
    - Dynamic Programming
    - Binary Tree
---

<!-- problem:start -->

# [333. Largest BST Subtree 🔒](https://leetcode.com/problems/largest-bst-subtree)

[Chinese Version](/solution/0300-0399/0333.Largest%20BST%20Subtree/README.md)

## Description

<!-- description:start -->

<p>Given the root of a binary tree, find the largest <span data-keyword="subtree">subtree</span>, which is also a Binary Search Tree (BST), where the largest means subtree has the largest number of nodes.</p>

<p>A <strong>Binary Search Tree (BST)</strong> is a tree in which all the nodes follow the below-mentioned properties:</p>

<ul>
	<li>The left subtree values are less than the value of their parent (root) node&#39;s value.</li>
	<li>The right subtree values are greater than the value of their parent (root) node&#39;s value.</li>
</ul>

<p><strong>Note:</strong> A subtree must include all of its descendants.</p>

<p>&nbsp;</p>
<p><strong class="example">Example 1:</strong></p>

<p><strong><img alt="" src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/0300-0399/0333.Largest%20BST%20Subtree/images/tmp.jpg" style="width: 571px; height: 302px;" /></strong></p>

<pre>
<strong>Input:</strong> root = [10,5,15,1,8,null,7]
<strong>Output:</strong> 3
<strong>Explanation: </strong>The Largest BST Subtree in this case is the highlighted one. The return value is the subtree&#39;s size, which is 3.</pre>

<p><strong class="example">Example 2:</strong></p>

<pre>
<strong>Input:</strong> root = [4,2,7,2,3,5,null,2,null,null,null,null,null,1]
<strong>Output:</strong> 2
</pre>

<p>&nbsp;</p>
<p><strong>Constraints:</strong></p>

<ul>
	<li>The number of nodes in the tree is in the range <code>[0, 10<sup>4</sup>]</code>.</li>
	<li><code>-10<sup>4</sup> &lt;= Node.val &lt;= 10<sup>4</sup></code></li>
</ul>

<p>&nbsp;</p>
<p><strong>Follow up:</strong> Can you figure out ways to solve it with <code>O(n)</code> time complexity?</p>

<!-- description:end -->

## Solutions

<!-- solution:start -->

### Solution 1

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
    private int ans;

    public int largestBSTSubtree(TreeNode root) {
        ans = 0;
        dfs(root);
        return ans;
    }

    private int[] dfs(TreeNode root) {
        if (root == null) {
            return new int[] {Integer.MAX_VALUE, Integer.MIN_VALUE, 0};
        }
        int[] left = dfs(root.left);
        int[] right = dfs(root.right);
        if (left[1] < root.val && root.val < right[0]) {
            ans = Math.max(ans, left[2] + right[2] + 1);
            return new int[] {
                Math.min(root.val, left[0]), Math.max(root.val, right[1]), left[2] + right[2] + 1};
        }
        return new int[] {Integer.MIN_VALUE, Integer.MAX_VALUE, 0};
    }
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
