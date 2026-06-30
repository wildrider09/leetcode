---
comments: true
difficulty: Medium
edit_url: https://github.com/doocs/leetcode/edit/main/lcci/04.10.Check%20SubTree/README_EN.md
---

<!-- problem:start -->

# [04.10. Check SubTree](https://leetcode.cn/problems/check-subtree-lcci)

[Chinese Version](/lcci/04.10.Check%20SubTree/README.md)

## Description

<!-- description:start -->

<p>T1&nbsp;and T2 are two very large binary trees, with T1&nbsp;much bigger than T2. Create an algorithm to determine if T2 is a subtree of T1.</p>

<p>A tree T2 is a subtree of T1&nbsp;if there exists a node n in T1&nbsp;such that the subtree of n is identical to T2. That is, if you cut off the tree at node n, the two trees would be identical.</p>

<p><strong>Example1:</strong></p>

<pre>

<strong> Input</strong>: t1 = [1, 2, 3], t2 = [2]

<strong> Output</strong>: true

</pre>

<p><strong>Example2:</strong></p>

<pre>

<strong> Input</strong>: t1 = [1, null, 2, 4], t2 = [3, 2]

<strong> Output</strong>: false

</pre>

<p><strong>Note: </strong></p>

<ol>
	<li>The node numbers of both tree are in [0, 20000].</li>
</ol>

<!-- description:end -->

## Solutions

<!-- solution:start -->

### Solution 1: Recursion

First, we check if $t_2$ is null. If it is, then $t_2$ is definitely a subtree of $t_1$, so we return `true`.

Otherwise, we check if $t_1$ is null. If it is, then $t_2$ is definitely not a subtree of $t_1$, so we return `false`.

Next, we check if $t_1$ and $t_2$ are equal. If they are, then $t_2$ is a subtree of $t_1$, so we return `true`. Otherwise, we recursively check if $t_1$'s left subtree is equal to $t_2$, and if $t_1$'s right subtree is equal to $t_2$. If either is `true`, then $t_2$ is a subtree of $t_1$, so we return `true`.

The time complexity is $O(n^2)$, and the space complexity is $O(n)$. Where $n$ is the number of nodes in $t_1$.

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
    public boolean checkSubTree(TreeNode t1, TreeNode t2) {
        if (t2 == null) {
            return true;
        }
        if (t1 == null) {
            return false;
        }
        if (dfs(t1, t2)) {
            return true;
        }
        return checkSubTree(t1.left, t2) || checkSubTree(t1.right, t2);
    }

    private boolean dfs(TreeNode t1, TreeNode t2) {
        if (t2 == null) {
            return t1 == null;
        }
        if (t1 == null || t1.val != t2.val) {
            return false;
        }
        return dfs(t1.left, t2.left) && dfs(t1.right, t2.right);
    }
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
