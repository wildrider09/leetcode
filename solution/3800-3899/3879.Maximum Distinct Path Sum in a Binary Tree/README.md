---
comments: true
difficulty: Medium
edit_url: https://github.com/doocs/leetcode/edit/main/solution/3800-3899/3879.Maximum%20Distinct%20Path%20Sum%20in%20a%20Binary%20Tree/README_EN.md
tags:
    - Tree
    - Depth-First Search
    - Hash Table
    - Binary Tree
---

<!-- problem:start -->

# [3879. Maximum Distinct Path Sum in a Binary Tree 🔒](https://leetcode.com/problems/maximum-distinct-path-sum-in-a-binary-tree)

[Chinese Version](/solution/3800-3899/3879.Maximum%20Distinct%20Path%20Sum%20in%20a%20Binary%20Tree/README.md)

## Description

<!-- description:start -->

<p>You are given the <code>root</code> of a <strong>binary tree</strong>, where each node contains an integer value.</p>

<p>A <strong>valid path</strong> in the tree is a sequence of <strong>connected</strong> nodes such that:</p>

<ul>
	<li>The path can start and end at <strong>any node</strong> in the tree.</li>
	<li>The path does <strong>not</strong> need to pass through the root.</li>
	<li>All node values along the path are <strong>distinct</strong>.</li>
</ul>

<p>Return an integer denoting the <strong>maximum</strong> possible sum of node values among all valid paths.</p>

<p>&nbsp;</p>
<p><strong class="example">Example 1:</strong></p>

<div class="example-block">
<p><img src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/3800-3899/3879.Maximum%20Distinct%20Path%20Sum%20in%20a%20Binary%20Tree/images/screenshot-2026-01-29-at-12940am.png" style="width: 200px; height: 175px;" /></p>

<p><strong>Input:</strong> <span class="example-io">root = [2,2,1]</span></p>

<p><strong>Output:</strong> <span class="example-io">3</span></p>

<p><strong>Explanation:</strong></p>

<ul>
	<li>The path <code>2 &rarr; 2</code> is invalid because the value 2 is not distinct.</li>
	<li>The maximum-sum valid path is <code>2 &rarr; 1</code>, with a sum = <code>2 + 1 = 3</code>.</li>
</ul>
</div>

<p><strong class="example">Example 2:</strong></p>

<div class="example-block">
<p><img src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/3800-3899/3879.Maximum%20Distinct%20Path%20Sum%20in%20a%20Binary%20Tree/images/screenshot-2026-01-29-at-15149am.png" style="width: 200px; height: 204px;" /></p>

<p><strong>Input:</strong> <span class="example-io">root = [1,-2,5,null,null,3,5]</span></p>

<p><strong>Output:</strong> <span class="example-io">9</span></p>

<p><strong>Explanation:</strong></p>

<ul>
	<li>The path <code>3 &rarr; 5 &rarr; 5</code> is invalid due to duplicate value 5.</li>
	<li>The maximum-sum valid path is <code>1 &rarr; 5 &rarr; 3</code>, with a sum = <code>1 + 5 + 3 = 9</code>.</li>
</ul>
</div>

<p><strong class="example">Example 3:</strong></p>

<p><img src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/3800-3899/3879.Maximum%20Distinct%20Path%20Sum%20in%20a%20Binary%20Tree/images/screenshot-2026-01-29-at-15555am.png" style="width: 180px; height: 217px;" />​​​​​​​</p>

<div class="example-block">
<p><strong>Input:</strong> <span class="example-io">root = [4,6,6,null,null,null,9]</span></p>

<p><strong>Output:</strong> <span class="example-io">19</span></p>

<p><strong>Explanation:</strong></p>

<ul>
	<li>The path <code>6 &rarr; 4 &rarr; 6 &rarr; 9</code> is invalid because the value 6 appears more than once.</li>
	<li>The maximum-sum valid path is <code>4 &rarr; 6 &rarr; 9</code>, with a sum = <code>4 + 6 + 9 = 19</code>.</li>
</ul>
</div>

<p>&nbsp;</p>
<p><strong>Constraints:</strong></p>

<ul>
	<li>The number of nodes in the tree is in the range <code>[1, 1000]</code>.</li>
	<li><code>-1000 &lt;= Node.val &lt;= 1000​​​​​​​</code></li>
</ul>

<!-- description:end -->

## Solutions

<!-- solution:start -->

### Solution 1: DFS + Hash Table

We can treat the tree as an undirected graph, using a hash table $g$ to store the adjacent nodes of each node, where $g[node]$ contains the parent node, left child node, and right child node of $node$.

We use depth-first search to traverse the tree and build the hash table $g$. For each node, we add its parent node, left child node, and right child node to $g[node]$.

Next, we use another depth-first search to compute the maximum path sum starting from each node. During this process, we use a hash set $vis$ to record the node values already visited on the current path, ensuring all node values along the path are distinct. For each node, we first check whether it is already in $vis$; if so, we return $0$. Otherwise, we add the node value to $vis$ and compute the path sum starting from that node. We traverse the adjacent nodes in $g[node]$, recursively compute the path sum starting from each adjacent node, and update the current best. Finally, we remove the current node value from $vis$ and return the current node value plus the best path sum.

We perform the above computation for every node in the tree and track the maximum path sum. The final answer is the maximum path sum.

The time complexity is $O(n^2)$, and the space complexity is $O(n)$, where $n$ is the number of nodes in the tree.

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
    Map<TreeNode, List<TreeNode>> g = new HashMap<>();
    Set<Integer> vis = new HashSet<>();

    public int maxSum(TreeNode root) {
        dfs(root, null);

        int ans = Integer.MIN_VALUE;
        for (TreeNode node : g.keySet()) {
            ans = Math.max(ans, dfs2(node));
            vis.clear();
        }
        return ans;
    }

    private void dfs(TreeNode node, TreeNode p) {
        if (node == null) {
            return;
        }
        g.computeIfAbsent(node, k -> new ArrayList<>());
        g.get(node).add(p);
        g.get(node).add(node.left);
        g.get(node).add(node.right);

        dfs(node.left, node);
        dfs(node.right, node);
    }

    private int dfs2(TreeNode node) {
        if (node == null || vis.contains(node.val)) {
            return 0;
        }
        vis.add(node.val);
        int res = node.val;
        int best = 0;
        for (TreeNode nxt : g.getOrDefault(node, Collections.emptyList())) {
            best = Math.max(best, dfs2(nxt));
        }
        vis.remove(node.val);
        return res + best;
    }
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
