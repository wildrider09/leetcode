---
comments: true
difficulty: Easy
edit_url: https://github.com/doocs/leetcode/edit/main/lcci/04.02.Minimum%20Height%20Tree/README_EN.md
---

<!-- problem:start -->

# [04.02. Minimum Height Tree](https://leetcode.cn/problems/minimum-height-tree-lcci)

[Chinese Version](/lcci/04.02.Minimum%20Height%20Tree/README.md)

## Description

<!-- description:start -->

<p>Given a sorted (increasing order) array with unique integer elements, write an algo&shy;rithm to create a binary search tree with minimal height.</p>

<p><strong>Example:</strong></p>

<pre>

Given sorted array: [-10,-3,0,5,9],

One possible answer is: [0,-3,9,-10,null,5]，which represents the following tree:

          0

         / \

       -3   9

       /   /

     -10  5

</pre>

<!-- description:end -->

## Solutions

<!-- solution:start -->

### Solution 1: Recursion

We design a function `dfs(l, r)`, which constructs a subtree from `l` to `r`. Therefore, the answer is `dfs(0, len(nums) - 1)`.

The execution process of the function `dfs(l, r)` is as follows:

1. If `l > r`, return `None`.
2. Otherwise, we calculate the middle position `mid = (l + r) / 2`, then construct the root node, the left subtree is `dfs(l, mid - 1)`, and the right subtree is `dfs(mid + 1, r)`.
3. Finally, return the root node.

The time complexity is $O(n)$, and the space complexity is $O(n)$, where $n$ is the length of the array.

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
    private int[] nums;

    public TreeNode sortedArrayToBST(int[] nums) {
        this.nums = nums;
        return dfs(0, nums.length - 1);
    }

    private TreeNode dfs(int l, int r) {
        if (l > r) {
            return null;
        }
        int mid = (l + r) >> 1;
        return new TreeNode(nums[mid], dfs(l, mid - 1), dfs(mid + 1, r));
    }
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
