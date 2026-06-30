---
comments: true
difficulty: Hard
edit_url: https://github.com/doocs/leetcode/edit/main/solution/0600-0699/0689.Maximum%20Sum%20of%203%20Non-Overlapping%20Subarrays/README_EN.md
tags:
    - Array
    - Dynamic Programming
    - Prefix Sum
    - Sliding Window
---

<!-- problem:start -->

# [689. Maximum Sum of 3 Non-Overlapping Subarrays](https://leetcode.com/problems/maximum-sum-of-3-non-overlapping-subarrays)

[Chinese Version](/solution/0600-0699/0689.Maximum%20Sum%20of%203%20Non-Overlapping%20Subarrays/README.md)

## Description

<!-- description:start -->

<p>Given an integer array <code>nums</code> and an integer <code>k</code>, find three non-overlapping subarrays of length <code>k</code> with maximum sum and return them.</p>

<p>Return the result as a list of indices representing the starting position of each interval (<strong>0-indexed</strong>). If there are multiple answers, return the lexicographically smallest one.</p>

<p>&nbsp;</p>
<p><strong class="example">Example 1:</strong></p>

<pre>
<strong>Input:</strong> nums = [1,2,1,2,6,7,5,1], k = 2
<strong>Output:</strong> [0,3,5]
<strong>Explanation:</strong> Subarrays [1, 2], [2, 6], [7, 5] correspond to the starting indices [0, 3, 5].
We could have also taken [2, 1], but an answer of [1, 3, 5] would be lexicographically larger.
</pre>

<p><strong class="example">Example 2:</strong></p>

<pre>
<strong>Input:</strong> nums = [1,2,1,2,1,2,1,2,1], k = 2
<strong>Output:</strong> [0,2,4]
</pre>

<p>&nbsp;</p>
<p><strong>Constraints:</strong></p>

<ul>
	<li><code>1 &lt;= nums.length &lt;= 2 * 10<sup>4</sup></code></li>
	<li><code>1 &lt;= nums[i] &lt;&nbsp;2<sup>16</sup></code></li>
	<li><code>1 &lt;= k &lt;= floor(nums.length / 3)</code></li>
</ul>

<!-- description:end -->

## Solutions

<!-- solution:start -->

### Solution 1: Sliding Window

We use a sliding window to enumerate the position of the third subarray, while maintaining the maximum sum and its position of the first two non-overlapping subarrays.

The time complexity is $O(n)$, where $n$ is the length of the array $nums$. The space complexity is $O(1)$.

<!-- tabs:start -->

#### Java

```java
class Solution {
    public int[] maxSumOfThreeSubarrays(int[] nums, int k) {
        int[] ans = new int[3];
        int s = 0, s1 = 0, s2 = 0, s3 = 0;
        int mx1 = 0, mx12 = 0;
        int idx1 = 0, idx121 = 0, idx122 = 0;
        for (int i = k * 2; i < nums.length; ++i) {
            s1 += nums[i - k * 2];
            s2 += nums[i - k];
            s3 += nums[i];
            if (i >= k * 3 - 1) {
                if (s1 > mx1) {
                    mx1 = s1;
                    idx1 = i - k * 3 + 1;
                }
                if (mx1 + s2 > mx12) {
                    mx12 = mx1 + s2;
                    idx121 = idx1;
                    idx122 = i - k * 2 + 1;
                }
                if (mx12 + s3 > s) {
                    s = mx12 + s3;
                    ans = new int[] {idx121, idx122, i - k + 1};
                }
                s1 -= nums[i - k * 3 + 1];
                s2 -= nums[i - k * 2 + 1];
                s3 -= nums[i - k + 1];
            }
        }
        return ans;
    }
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- solution:start -->

### Solution 2: Preprocessing Prefix and Suffix + Enumerating Middle Subarray

We can preprocess to get the prefix sum array $s$ of the array $nums$, where $s[i] = \sum_{j=0}^{i-1} nums[j]$. Then for any $i$, $j$, $s[j] - s[i]$ is the sum of the subarray $[i, j)$.

Next, we use dynamic programming to maintain two arrays $pre$ and $suf$ of length $n$, where $pre[i]$ represents the maximum sum and its starting position of the subarray of length $k$ within the range $[0, i]$, and $suf[i]$ represents the maximum sum and its starting position of the subarray of length $k$ within the range $[i, n)$.

Then, we enumerate the starting position $i$ of the middle subarray. The sum of the three subarrays is $pre[i-1][0] + suf[i+k][0] + (s[i+k] - s[i])$, where $pre[i-1][0]$ represents the maximum sum of the subarray of length $k$ within the range $[0, i-1]$, $suf[i+k][0]$ represents the maximum sum of the subarray of length $k$ within the range $[i+k, n)$, and $(s[i+k] - s[i])$ represents the sum of the subarray of length $k$ within the range $[i, i+k)$. We find the starting positions of the three subarrays corresponding to the maximum sum.

The time complexity is $O(n)$, and the space complexity is $O(n)$. Here, $n$ is the length of the array $nums$.

<!-- tabs:start -->

#### Java

```java
class Solution {
    public int[] maxSumOfThreeSubarrays(int[] nums, int k) {
        int n = nums.length;
        int[] s = new int[n + 1];
        for (int i = 0; i < n; ++i) {
            s[i + 1] = s[i] + nums[i];
        }
        int[][] pre = new int[n][0];
        int[][] suf = new int[n][0];
        for (int i = 0, t = 0, idx = 0; i < n - k + 1; ++i) {
            int cur = s[i + k] - s[i];
            if (cur > t) {
                pre[i + k - 1] = new int[] {cur, i};
                t = cur;
                idx = i;
            } else {
                pre[i + k - 1] = new int[] {t, idx};
            }
        }
        for (int i = n - k, t = 0, idx = 0; i >= 0; --i) {
            int cur = s[i + k] - s[i];
            if (cur >= t) {
                suf[i] = new int[] {cur, i};
                t = cur;
                idx = i;
            } else {
                suf[i] = new int[] {t, idx};
            }
        }
        int[] ans = new int[0];
        for (int i = k, t = 0; i < n - 2 * k + 1; ++i) {
            int cur = s[i + k] - s[i] + pre[i - 1][0] + suf[i + k][0];
            if (cur > t) {
                ans = new int[] {pre[i - 1][1], i, suf[i + k][1]};
                t = cur;
            }
        }
        return ans;
    }
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
