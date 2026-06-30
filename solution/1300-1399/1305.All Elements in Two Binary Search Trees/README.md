---
comments: true
difficulty: Medium
edit_url: https://github.com/doocs/leetcode/edit/main/solution/1300-1399/1305.All%20Elements%20in%20Two%20Binary%20Search%20Trees/README_EN.md
rating: 1260
source: Weekly Contest 169 Q2
tags:
    - Tree
    - Depth-First Search
    - Binary Search Tree
    - Binary Tree
    - Sorting
---

<!-- problem:start -->

# [1305. All Elements in Two Binary Search Trees](https://leetcode.com/problems/all-elements-in-two-binary-search-trees)

[Chinese Version](/solution/1300-1399/1305.All%20Elements%20in%20Two%20Binary%20Search%20Trees/README.md)

## Description

<!-- description:start -->

<p>Given two binary search trees <code>root1</code> and <code>root2</code>, return <em>a list containing all the integers from both trees sorted in <strong>ascending</strong> order</em>.</p>

<p>&nbsp;</p>
<p><strong class="example">Example 1:</strong></p>
<img alt="" src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/1300-1399/1305.All%20Elements%20in%20Two%20Binary%20Search%20Trees/images/q2-e1.png" style="width: 457px; height: 207px;" />
<pre>
<strong>Input:</strong> root1 = [2,1,4], root2 = [1,0,3]
<strong>Output:</strong> [0,1,1,2,3,4]
</pre>

<p><strong class="example">Example 2:</strong></p>
<img alt="" src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/1300-1399/1305.All%20Elements%20in%20Two%20Binary%20Search%20Trees/images/q2-e5-.png" style="width: 352px; height: 197px;" />
<pre>
<strong>Input:</strong> root1 = [1,null,8], root2 = [8,1]
<strong>Output:</strong> [1,1,8,8]
</pre>

<p>&nbsp;</p>
<p><strong>Constraints:</strong></p>

<ul>
	<li>The number of nodes in each tree is in the range <code>[0, 5000]</code>.</li>
	<li><code>-10<sup>5</sup> &lt;= Node.val &lt;= 10<sup>5</sup></code></li>
</ul>

<!-- description:end -->

## Solutions

<!-- solution:start -->

### Solution 1: DFS + Merge

Since both trees are binary search trees, we can obtain the node value sequences $\textit{a}$ and $\textit{b}$ of the two trees through in-order traversal. Then, we use two pointers to merge the two sorted arrays to get the final answer.

The time complexity is $O(n+m)$, and the space complexity is $O(n+m)$. Here, $n$ and $m$ are the number of nodes in the two trees, respectively.

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
    public List<Integer> getAllElements(TreeNode root1, TreeNode root2) {
        List<Integer> a = new ArrayList<>();
        List<Integer> b = new ArrayList<>();
        dfs(root1, a);
        dfs(root2, b);
        int m = a.size(), n = b.size();
        int i = 0, j = 0;
        List<Integer> ans = new ArrayList<>();
        while (i < m && j < n) {
            if (a.get(i) <= b.get(j)) {
                ans.add(a.get(i++));
            } else {
                ans.add(b.get(j++));
            }
        }
        while (i < m) {
            ans.add(a.get(i++));
        }
        while (j < n) {
            ans.add(b.get(j++));
        }
        return ans;
    }

    private void dfs(TreeNode root, List<Integer> nums) {
        if (root == null) {
            return;
        }
        dfs(root.left, nums);
        nums.add(root.val);
        dfs(root.right, nums);
    }
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
