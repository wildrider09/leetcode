---
comments: true
difficulty: Medium
edit_url: https://github.com/doocs/leetcode/edit/main/solution/0400-0499/0416.Partition%20Equal%20Subset%20Sum/README_EN.md
tags:
    - Array
    - Dynamic Programming
---

<!-- problem:start -->

# [416. Partition Equal Subset Sum](https://leetcode.com/problems/partition-equal-subset-sum)

[Chinese Version](/solution/0400-0499/0416.Partition%20Equal%20Subset%20Sum/README.md)

## Description

<!-- description:start -->

<p>Given an integer array <code>nums</code>, return <code>true</code> <em>if you can partition the array into two subsets such that the sum of the elements in both subsets is equal or </em><code>false</code><em> otherwise</em>.</p>

<p>&nbsp;</p>
<p><strong class="example">Example 1:</strong></p>

<pre>
<strong>Input:</strong> nums = [1,5,11,5]
<strong>Output:</strong> true
<strong>Explanation:</strong> The array can be partitioned as [1, 5, 5] and [11].
</pre>

<p><strong class="example">Example 2:</strong></p>

<pre>
<strong>Input:</strong> nums = [1,2,3,5]
<strong>Output:</strong> false
<strong>Explanation:</strong> The array cannot be partitioned into equal sum subsets.
</pre>

<p>&nbsp;</p>
<p><strong>Constraints:</strong></p>

<ul>
	<li><code>1 &lt;= nums.length &lt;= 200</code></li>
	<li><code>1 &lt;= nums[i] &lt;= 100</code></li>
</ul>

<!-- description:end -->

## Solutions

<!-- solution:start -->

### Solution 1: Dynamic Programming

First, we calculate the total sum $s$ of the array. If the total sum is odd, it cannot be divided into two subsets with equal sums, so we directly return `false`. If the total sum is even, we set the target subset sum to $m = \frac{s}{2}$. The problem is then transformed into: does there exist a subset whose element sum is $m$?

We define $f[i][j]$ to represent whether it is possible to select several numbers from the first $i$ numbers so that their sum is exactly $j$. Initially, $f[0][0] = true$ and the rest $f[i][j] = false$. The answer is $f[n][m]$.

Considering $f[i][j]$, if we select the $i$-th number $x$, then $f[i][j] = f[i - 1][j - x]$. If we do not select the $i$-th number $x$, then $f[i][j] = f[i - 1][j]$. Therefore, the state transition equation is:

$$
f[i][j] = f[i - 1][j] \textit{ or } f[i - 1][j - x] \textit{ if } j \geq x
$$

The final answer is $f[n][m]$.

The time complexity is $O(m \times n)$, and the space complexity is $O(m \times n)$. Where $m$ and $n$ are half of the total sum of the array and the length of the array, respectively.

<!-- tabs:start -->

#### Java

```java
class Solution {
    public boolean canPartition(int[] nums) {
        // int s = Arrays.stream(nums).sum();
        int s = 0;
        for (int x : nums) {
            s += x;
        }
        if (s % 2 == 1) {
            return false;
        }
        int n = nums.length;
        int m = s >> 1;
        boolean[][] f = new boolean[n + 1][m + 1];
        f[0][0] = true;
        for (int i = 1; i <= n; ++i) {
            int x = nums[i - 1];
            for (int j = 0; j <= m; ++j) {
                f[i][j] = f[i - 1][j] || (j >= x && f[i - 1][j - x]);
            }
        }
        return f[n][m];
    }
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- solution:start -->

### Solution 2: Dynamic Programming (Space Optimization)

We notice that in Solution 1, $f[i][j]$ is only related to $f[i - 1][\cdot]$. Therefore, we can compress the two-dimensional array into a one-dimensional array.

The time complexity is $O(n \times m)$, and the space complexity is $O(m)$. Where $n$ is the length of the array, and $m$ is half of the total sum of the array.

<!-- tabs:start -->

#### Java

```java
class Solution {
    public boolean canPartition(int[] nums) {
        // int s = Arrays.stream(nums).sum();
        int s = 0;
        for (int x : nums) {
            s += x;
        }
        if (s % 2 == 1) {
            return false;
        }
        int m = s >> 1;
        boolean[] f = new boolean[m + 1];
        f[0] = true;
        for (int x : nums) {
            for (int j = m; j >= x; --j) {
                f[j] |= f[j - x];
            }
        }
        return f[m];
    }
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
