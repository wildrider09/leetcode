---
comments: true
difficulty: Medium
edit_url: https://github.com/doocs/leetcode/edit/main/lcci/04.06.Successor/README_EN.md
---

<!-- problem:start -->

# [04.06. Successor](https://leetcode.cn/problems/successor-lcci)

[Chinese Version](/lcci/04.06.Successor/README.md)

## Description

<!-- description:start -->

<p>Write an algorithm to find the &quot;next&quot; node (i.e., in-order successor) of a given node in a binary search tree.</p>

<p>Return <code>null</code> if there&#39;s no &quot;next&quot; node for the given node.</p>

<p><strong>Example 1:</strong></p>

<pre>

<strong>Input:</strong> root = <code>[2,1,3], p = 1

  2

 / \

1   3

</code>

<strong>Output:</strong> 2</pre>

<p><strong>Example 2:</strong></p>

<pre>

<strong>Input:</strong> root = <code>[5,3,6,2,4,null,null,1], p = 6

      5

     / \

    3   6

   / \

  2   4

 /   

1

</code>

<strong>Output:</strong> null</pre>

<!-- description:end -->

## Solutions

<!-- solution:start -->

### Solution 1: Binary Search

The in-order traversal of a binary search tree is an ascending sequence, so we can use the binary search method.

The in-order successor node of a binary search tree node $p$ satisfies:

1. The value of the in-order successor node is greater than the value of node $p$.
2. The in-order successor is the node with the smallest value among all nodes greater than $p$.

Therefore, for the current node $root$, if $root.val > p.val$, then $root$ could be the in-order successor of $p$. We record $root$ as $ans$ and then search the left subtree, i.e., $root = root.left$. If $root.val \leq p.val$, then $root$ cannot be the in-order successor of $p$, and we search the right subtree, i.e., $root = root.right$.

The time complexity is $O(h)$, where $h$ is the height of the binary search tree. The space complexity is $O(1)$.

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
    public TreeNode inorderSuccessor(TreeNode root, TreeNode p) {
        TreeNode ans = null;
        while (root != null) {
            if (root.val > p.val) {
                ans = root;
                root = root.left;
            } else {
                root = root.right;
            }
        }
        return ans;
    }
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
