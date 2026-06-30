---
comments: true
difficulty: Medium
edit_url: https://github.com/doocs/leetcode/edit/main/solution/1000-1099/1026.Maximum%20Difference%20Between%20Node%20and%20Ancestor/README_EN.md
rating: 1446
source: Weekly Contest 132 Q2
tags:
    - Tree
    - Depth-First Search
    - Binary Tree
---

<!-- problem:start -->

# [1026. Maximum Difference Between Node and Ancestor](https://leetcode.com/problems/maximum-difference-between-node-and-ancestor)

[Chinese Version](/solution/1000-1099/1026.Maximum%20Difference%20Between%20Node%20and%20Ancestor/README.md)

## Description

<!-- description:start -->

<p>Given the <code>root</code> of a binary tree, find the maximum value <code>v</code> for which there exist <strong>different</strong> nodes <code>a</code> and <code>b</code> where <code>v = |a.val - b.val|</code> and <code>a</code> is an ancestor of <code>b</code>.</p>

<p>A node <code>a</code> is an ancestor of <code>b</code> if either: any child of <code>a</code> is equal to <code>b</code>&nbsp;or any child of <code>a</code> is an ancestor of <code>b</code>.</p>

<p>&nbsp;</p>
<p><strong class="example">Example 1:</strong></p>
<img alt="" src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/1000-1099/1026.Maximum%20Difference%20Between%20Node%20and%20Ancestor/images/tmp-tree.jpg" style="width: 400px; height: 390px;" />
<pre>
<strong>Input:</strong> root = [8,3,10,1,6,null,14,null,null,4,7,13]
<strong>Output:</strong> 7
<strong>Explanation: </strong>We have various ancestor-node differences, some of which are given below :
|8 - 3| = 5
|3 - 7| = 4
|8 - 1| = 7
|10 - 13| = 3
Among all possible differences, the maximum value of 7 is obtained by |8 - 1| = 7.</pre>

<p><strong class="example">Example 2:</strong></p>
<img alt="" src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/1000-1099/1026.Maximum%20Difference%20Between%20Node%20and%20Ancestor/images/tmp-tree-1.jpg" style="width: 250px; height: 349px;" />
<pre>
<strong>Input:</strong> root = [1,null,2,null,0,3]
<strong>Output:</strong> 3
</pre>

<p>&nbsp;</p>
<p><strong>Constraints:</strong></p>

<ul>
	<li>The number of nodes in the tree is in the range <code>[2, 5000]</code>.</li>
	<li><code>0 &lt;= Node.val &lt;= 10<sup>5</sup></code></li>
</ul>

<!-- description:end -->

## Solutions

<!-- solution:start -->

### Solution 1: DFS

For each node, to find the maximum difference with its ancestor nodes, we only need to find the difference between the maximum and minimum values of the ancestor nodes. The maximum difference among all nodes and their ancestor nodes is the answer.

Therefore, we design a function $dfs(root, mi, mx)$, where the current node being searched is $root$, the maximum value of its ancestor nodes is $mx$, and the minimum value is $mi$. The function updates the maximum difference $ans$.

The logic of the function $dfs(root, mi, mx)$ is as follows:

- If $root$ is null, return directly.
- Otherwise, we update $ans = max(ans, |mi - root.val|, |mx - root.val|)$.
- Then update $mi = min(mi, root.val)$, $mx = max(mx, root.val)$, and recursively search the left and right subtrees.

In the main function, we call $dfs(root, root.val, root.val)$, and finally return $ans$.

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
    private int ans;

    public int maxAncestorDiff(TreeNode root) {
        dfs(root, root.val, root.val);
        return ans;
    }

    private void dfs(TreeNode root, int mi, int mx) {
        if (root == null) {
            return;
        }
        int x = Math.max(Math.abs(mi - root.val), Math.abs(mx - root.val));
        ans = Math.max(ans, x);
        mi = Math.min(mi, root.val);
        mx = Math.max(mx, root.val);
        dfs(root.left, mi, mx);
        dfs(root.right, mi, mx);
    }
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
