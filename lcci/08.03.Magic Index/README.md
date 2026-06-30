---
comments: true
difficulty: Easy
edit_url: https://github.com/doocs/leetcode/edit/main/lcci/08.03.Magic%20Index/README_EN.md
---

<!-- problem:start -->

# [08.03. Magic Index](https://leetcode.cn/problems/magic-index-lcci)

[Chinese Version](/lcci/08.03.Magic%20Index/README.md)

## Description

<!-- description:start -->

<p>A magic index in an array <code>A[0...n-1]</code> is defined to be an index such that <code>A[i] = i</code>. Given a sorted array of distinct integers, write a method to find a magic index, if one exists, in array A. If not, return -1. If there are more than one magic index, return the smallest one.</p>

<p><strong>Example1:</strong></p>

<pre>

<strong> Input</strong>: nums = [0, 2, 3, 4, 5]

<strong> Output</strong>: 0

</pre>

<p><strong>Example2:</strong></p>

<pre>

<strong> Input</strong>: nums = [1, 1, 1]

<strong> Output</strong>: 1

</pre>

<p><strong>Note:</strong></p>

<ol>
	<li><code>1 &lt;= nums.length &lt;= 1000000</code></li>
</ol>

<!-- description:end -->

## Solutions

<!-- solution:start -->

### Solution 1: Binary Search

We design a function $dfs(i, j)$ to find the magic index in the array $nums[i, j]$. If found, return the value of the magic index, otherwise return $-1$. So the answer is $dfs(0, n-1)$.

The implementation of the function $dfs(i, j)$ is as follows:

1. If $i > j$, return $-1$.
2. Otherwise, we take the middle position $mid = (i + j) / 2$, then recursively call $dfs(i, mid-1)$. If the return value is not $-1$, it means that the magic index is found in the left half, return it directly. Otherwise, if $nums[mid] = mid$, it means that the magic index is found, return it directly. Otherwise, recursively call $dfs(mid+1, j)$ and return.

In the worst case, the time complexity is $O(n)$, and the space complexity is $O(n)$. Where $n$ is the length of the array $nums$.

<!-- tabs:start -->

#### Java

```java
class Solution {
    public int findMagicIndex(int[] nums) {
        return dfs(nums, 0, nums.length - 1);
    }

    private int dfs(int[] nums, int i, int j) {
        if (i > j) {
            return -1;
        }
        int mid = (i + j) >> 1;
        int l = dfs(nums, i, mid - 1);
        if (l != -1) {
            return l;
        }
        if (nums[mid] == mid) {
            return mid;
        }
        return dfs(nums, mid + 1, j);
    }
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
