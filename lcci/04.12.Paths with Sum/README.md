---
comments: true
difficulty: Medium
edit_url: https://github.com/doocs/leetcode/edit/main/lcci/04.12.Paths%20with%20Sum/README_EN.md
---

<!-- problem:start -->

# [04.12. Paths with Sum](https://leetcode.cn/problems/paths-with-sum-lcci)

[Chinese Version](/lcci/04.12.Paths%20with%20Sum/README.md)

## Description

<!-- description:start -->

<p>You are given a binary tree in which each node contains an integer value (which might be positive or negative). Design an algorithm to count the number of paths that sum to a given value. The path does not need to start or end at the root or a leaf, but it must go downwards (traveling only from parent nodes to child nodes).</p>

<p><strong>Example:</strong><br />

Given the following tree and &nbsp;<code>sum = 22,</code></p>

<pre>

              5

             / \

            4   8

           /   / \

          11  13  4

         /  \    / \

        7    2  5   1

</pre>

<p>Output:</p>

<pre>

3

<strong>Explanation: </strong>Paths that have sum 22 are: [5,4,11,2], [5,8,4,5], [4,11,7]</pre>

<p>Note:</p>

<ul>
	<li><code>node number &lt;= 10000</code></li>
</ul>

<!-- description:end -->

## Solutions

<!-- solution:start -->

### Solution 1: Hash Table + Prefix Sum + Recursion

We can use the idea of prefix sum to recursively traverse the binary tree, and use a hash table $cnt$ to count the occurrence of each prefix sum on the path from the root node to the current node.

We design a recursive function $dfs(node, s)$, where the current node being traversed is $node$, and the prefix sum on the path from the root node to the current node is $s$. The return value of the function is the number of paths with the path sum equal to $sum$ and the path ends at the $node$ node or its subtree nodes. Therefore, the answer is $dfs(root, 0)$.

The recursive process of the function $dfs(node, s)$ is as follows:

- If the current node $node$ is null, return $0$.
- Calculate the prefix sum $s$ on the path from the root node to the current node.
- Use $cnt[s - sum]$ to represent the number of paths with the path sum equal to $sum$ and the path ends at the current node, where $cnt[s - sum]$ is the count of the prefix sum equal to $s - sum$ in $cnt$.
- Add the count of the prefix sum $s$ by $1$, i.e., $cnt[s] = cnt[s] + 1$.
- Recursively traverse the left and right child nodes of the current node, i.e., call the functions $dfs(node.left, s)$ and $dfs(node.right, s)$, and add their return values.
- After the return value is calculated, subtract the count of the prefix sum $s$ of the current node by $1$, i.e., execute $cnt[s] = cnt[s] - 1$.
- Finally, return the answer.

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
    private Map<Long, Integer> cnt = new HashMap<>();
    private int target;

    public int pathSum(TreeNode root, int sum) {
        cnt.put(0L, 1);
        target = sum;
        return dfs(root, 0);
    }

    private int dfs(TreeNode root, long s) {
        if (root == null) {
            return 0;
        }
        s += root.val;
        int ans = cnt.getOrDefault(s - target, 0);
        cnt.merge(s, 1, Integer::sum);
        ans += dfs(root.left, s);
        ans += dfs(root.right, s);
        cnt.merge(s, -1, Integer::sum);
        return ans;
    }
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
