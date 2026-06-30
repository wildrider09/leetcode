---
comments: true
difficulty: Medium
edit_url: https://github.com/doocs/leetcode/edit/main/solution/0600-0699/0663.Equal%20Tree%20Partition/README_EN.md
tags:
    - Tree
    - Depth-First Search
    - Binary Tree
---

<!-- problem:start -->

# [663. Equal Tree Partition 🔒](https://leetcode.com/problems/equal-tree-partition)

[Chinese Version](/solution/0600-0699/0663.Equal%20Tree%20Partition/README.md)

## Description

<!-- description:start -->

<p>Given the <code>root</code> of a binary tree, return <code>true</code><em> if you can partition the tree into two trees with equal sums of values after removing exactly one edge on the original tree</em>.</p>

<p>&nbsp;</p>
<p><strong class="example">Example 1:</strong></p>
<img alt="" src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/0600-0699/0663.Equal%20Tree%20Partition/images/split1-tree.jpg" style="width: 500px; height: 204px;" />
<pre>
<strong>Input:</strong> root = [5,10,10,null,null,2,3]
<strong>Output:</strong> true
</pre>

<p><strong class="example">Example 2:</strong></p>
<img alt="" src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/0600-0699/0663.Equal%20Tree%20Partition/images/split2-tree.jpg" style="width: 277px; height: 302px;" />
<pre>
<strong>Input:</strong> root = [1,2,10,null,null,2,20]
<strong>Output:</strong> false
<strong>Explanation:</strong> You cannot split the tree into two trees with equal sums after removing exactly one edge on the tree.
</pre>

<p>&nbsp;</p>
<p><strong>Constraints:</strong></p>

<ul>
	<li>The number of nodes in the tree is in the range <code>[1, 10<sup>4</sup>]</code>.</li>
	<li><code>-10<sup>5</sup> &lt;= Node.val &lt;= 10<sup>5</sup></code></li>
</ul>

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
    private List<Integer> seen;

    public boolean checkEqualTree(TreeNode root) {
        seen = new ArrayList<>();
        int s = sum(root);
        if (s % 2 != 0) {
            return false;
        }
        seen.remove(seen.size() - 1);
        return seen.contains(s / 2);
    }

    private int sum(TreeNode root) {
        if (root == null) {
            return 0;
        }
        int l = sum(root.left);
        int r = sum(root.right);
        int s = l + r + root.val;
        seen.add(s);
        return s;
    }
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
