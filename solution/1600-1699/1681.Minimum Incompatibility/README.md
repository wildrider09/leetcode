---
comments: true
difficulty: Hard
edit_url: https://github.com/doocs/leetcode/edit/main/solution/1600-1699/1681.Minimum%20Incompatibility/README_EN.md
rating: 2389
source: Weekly Contest 218 Q4
tags:
    - Bit Manipulation
    - Array
    - Hash Table
    - Dynamic Programming
    - Bitmask
---

<!-- problem:start -->

# [1681. Minimum Incompatibility](https://leetcode.com/problems/minimum-incompatibility)

[Chinese Version](/solution/1600-1699/1681.Minimum%20Incompatibility/README.md)

## Description

<!-- description:start -->

<p>You are given an integer array <code>nums</code>​​​ and an integer <code>k</code>. You are asked to distribute this array into <code>k</code> subsets of <strong>equal size</strong> such that there are no two equal elements in the same subset.</p>

<p>A subset&#39;s <strong>incompatibility</strong> is the difference between the maximum and minimum elements in that array.</p>

<p>Return <em>the <strong>minimum possible sum of incompatibilities</strong> of the </em><code>k</code> <em>subsets after distributing the array optimally, or return </em><code>-1</code><em> if it is not possible.</em></p>

<p>A subset is a group integers that appear in the array with no particular order.</p>

<p>&nbsp;</p>
<p><strong class="example">Example 1:</strong></p>

<pre>
<strong>Input:</strong> nums = [1,2,1,4], k = 2
<strong>Output:</strong> 4
<strong>Explanation:</strong> The optimal distribution of subsets is [1,2] and [1,4].
The incompatibility is (2-1) + (4-1) = 4.
Note that [1,1] and [2,4] would result in a smaller sum, but the first subset contains 2 equal elements.</pre>

<p><strong class="example">Example 2:</strong></p>

<pre>
<strong>Input:</strong> nums = [6,3,8,1,3,1,2,2], k = 4
<strong>Output:</strong> 6
<strong>Explanation:</strong> The optimal distribution of subsets is [1,2], [2,3], [6,8], and [1,3].
The incompatibility is (2-1) + (3-2) + (8-6) + (3-1) = 6.
</pre>

<p><strong class="example">Example 3:</strong></p>

<pre>
<strong>Input:</strong> nums = [5,3,3,6,3,3], k = 3
<strong>Output:</strong> -1
<strong>Explanation:</strong> It is impossible to distribute nums into 3 subsets where no two elements are equal in the same subset.
</pre>

<p>&nbsp;</p>
<p><strong>Constraints:</strong></p>

<ul>
	<li><code>1 &lt;= k &lt;= nums.length &lt;= 16</code></li>
	<li><code>nums.length</code> is divisible by <code>k</code></li>
	<li><code>1 &lt;= nums[i] &lt;= nums.length</code></li>
</ul>

<!-- description:end -->

## Solutions

<!-- solution:start -->

### Solution 1: Preprocessing + State Compression + Dynamic Programming

Let's assume that the size of each subset after partitioning is $m$, so $m=\frac{n}{k}$, where $n$ is the length of the array.

We can enumerate all subsets $i$, where $i \in [0, 2^n)$, if the binary representation of subset $i$ has $m$ ones, and the elements in subset $i$ are not repeated, then we can calculate the incompatibility of subset $i$, denoted as $g[i]$, i.e., $g[i]=\max_{j \in i} \{nums[j]\} - \min_{j \in i} \{nums[j]\}$.

Next, we can use dynamic programming to solve.

We define $f[i]$ as the minimum sum of incompatibilities when the current partitioned subset state is $i$. Initially, $f[0]=0$, which means no elements are partitioned into the subset, and the rest $f[i]=+\infty$.

For state $i$, we find all undivided and non-repeated elements, represented by a state $mask$. If the number of elements in state $mask$ is greater than or equal to $m$, then we enumerate all subsets $j$ of $mask$, and satisfy $j \subset mask$, then $f[i \cup j]=\min \{f[i \cup j], f[i]+g[j]\}$.

Finally, if $f[2^n-1]=+\infty$, it means that it cannot be partitioned into $k$ subsets, return $-1$, otherwise return $f[2^n-1]$.

The time complexity is $O(3^n)$, and the space complexity is $O(2^n)$. Here, $n$ is the length of the array.

<!-- tabs:start -->

#### Java

```java
class Solution {
    public int minimumIncompatibility(int[] nums, int k) {
        int n = nums.length;
        int m = n / k;
        int[] g = new int[1 << n];
        Arrays.fill(g, -1);
        for (int i = 1; i < 1 << n; ++i) {
            if (Integer.bitCount(i) != m) {
                continue;
            }
            Set<Integer> s = new HashSet<>();
            int mi = 20, mx = 0;
            for (int j = 0; j < n; ++j) {
                if ((i >> j & 1) == 1) {
                    if (!s.add(nums[j])) {
                        break;
                    }
                    mi = Math.min(mi, nums[j]);
                    mx = Math.max(mx, nums[j]);
                }
            }
            if (s.size() == m) {
                g[i] = mx - mi;
            }
        }
        int[] f = new int[1 << n];
        final int inf = 1 << 30;
        Arrays.fill(f, inf);
        f[0] = 0;
        for (int i = 0; i < 1 << n; ++i) {
            if (f[i] == inf) {
                continue;
            }
            Set<Integer> s = new HashSet<>();
            int mask = 0;
            for (int j = 0; j < n; ++j) {
                if ((i >> j & 1) == 0 && !s.contains(nums[j])) {
                    s.add(nums[j]);
                    mask |= 1 << j;
                }
            }
            if (s.size() < m) {
                continue;
            }
            for (int j = mask; j > 0; j = (j - 1) & mask) {
                if (g[j] != -1) {
                    f[i | j] = Math.min(f[i | j], f[i] + g[j]);
                }
            }
        }
        return f[(1 << n) - 1] == inf ? -1 : f[(1 << n) - 1];
    }
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
