---
comments: true
difficulty: Medium
edit_url: https://github.com/doocs/leetcode/edit/main/solution/3100-3199/3176.Find%20the%20Maximum%20Length%20of%20a%20Good%20Subsequence%20I/README_EN.md
rating: 1849
source: Biweekly Contest 132 Q3
tags:
    - Array
    - Hash Table
    - Dynamic Programming
---

<!-- problem:start -->

# [3176. Find the Maximum Length of a Good Subsequence I](https://leetcode.com/problems/find-the-maximum-length-of-a-good-subsequence-i)

[Chinese Version](/solution/3100-3199/3176.Find%20the%20Maximum%20Length%20of%20a%20Good%20Subsequence%20I/README.md)

## Description

<!-- description:start -->

<p>You are given an integer array <code>nums</code> and a <strong>non-negative</strong> integer <code>k</code>. A sequence of integers <code>seq</code> is called <strong>good</strong> if there are <strong>at most</strong> <code>k</code> indices <code>i</code> in the range <code>[0, seq.length - 2]</code> such that <code>seq[i] != seq[i + 1]</code>.</p>

<p>Return the <strong>maximum</strong> possible length of a <strong>good</strong> <span data-keyword="subsequence-array">subsequence</span> of <code>nums</code>.</p>

<p>&nbsp;</p>
<p><strong class="example">Example 1:</strong></p>

<div class="example-block">
<p><strong>Input:</strong> <span class="example-io">nums = [1,2,1,1,3], k = 2</span></p>

<p><strong>Output:</strong> <span class="example-io">4</span></p>

<p><strong>Explanation:</strong></p>

<p>The maximum length subsequence is <code>[<u>1</u>,<u>2</u>,<u>1</u>,<u>1</u>,3]</code>.</p>
</div>

<p><strong class="example">Example 2:</strong></p>

<div class="example-block">
<p><strong>Input:</strong> <span class="example-io">nums = [1,2,3,4,5,1], k = 0</span></p>

<p><strong>Output:</strong> <span class="example-io">2</span></p>

<p><strong>Explanation:</strong></p>

<p>The maximum length subsequence is <code>[<u>1</u>,2,3,4,5,<u>1</u>]</code>.</p>
</div>

<p>&nbsp;</p>
<p><strong>Constraints:</strong></p>

<ul>
	<li><code>1 &lt;= nums.length &lt;= 500</code></li>
	<li><code>1 &lt;= nums[i] &lt;= 10<sup>9</sup></code></li>
	<li><code>0 &lt;= k &lt;= min(nums.length, 25)</code></li>
</ul>

<!-- description:end -->

## Solutions

<!-- solution:start -->

### Solution 1: Dynamic Programming

We define $f[i][h]$ as the length of the longest good subsequence ending with $nums[i]$ and having no more than $h$ indices satisfying the condition. Initially, $f[i][h] = 1$. The answer is $\max(f[i][k])$, where $0 \le i < n$.

We consider how to calculate $f[i][h]$. We can enumerate $0 \le j < i$, if $nums[i] = nums[j]$, then $f[i][h] = \max(f[i][h], f[j][h] + 1)$; otherwise, if $h > 0$, then $f[i][h] = \max(f[i][h], f[j][h - 1] + 1)$. That is:

$$
f[i][h]=
\begin{cases}
\max(f[i][h], f[j][h] + 1), & \textit{if } nums[i] = nums[j], \\
\max(f[i][h], f[j][h - 1] + 1), & \textit{if } h > 0.
\end{cases}
$$

The final answer is $\max(f[i][k])$, where $0 \le i < n$.

The time complexity is $O(n^2 \times k)$, and the space complexity is $O(n \times k)$. Where $n$ is the length of the array `nums`.

<!-- tabs:start -->

#### Java

```java
class Solution {
    public int maximumLength(int[] nums, int k) {
        int n = nums.length;
        int[][] f = new int[n][k + 1];
        int ans = 0;
        for (int i = 0; i < n; ++i) {
            for (int h = 0; h <= k; ++h) {
                for (int j = 0; j < i; ++j) {
                    if (nums[i] == nums[j]) {
                        f[i][h] = Math.max(f[i][h], f[j][h]);
                    } else if (h > 0) {
                        f[i][h] = Math.max(f[i][h], f[j][h - 1]);
                    }
                }
                ++f[i][h];
            }
            ans = Math.max(ans, f[i][k]);
        }
        return ans;
    }
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- solution:start -->

### Solution 2: Optimized Dynamic Programming

According to the state transition equation in Solution 1, if $nums[i] = nums[j]$, then we only need to get the maximum value of $f[j][h]$. We can maintain this with an array $mp$ of length $k + 1$. If $nums[i] \neq nums[j]$, we need to record the maximum value of $f[j][h - 1]$ corresponding to $nums[j]$, the maximum value and the second maximum value. We can maintain these with an array $g$ of length $k + 1$.

The time complexity is $O(n \times k)$, and the space complexity is $O(n \times k)$. Where $n$ is the length of the array `nums`.

<!-- tabs:start -->

#### Java

```java
class Solution {
    public int maximumLength(int[] nums, int k) {
        int n = nums.length;
        int[][] f = new int[n][k + 1];
        Map<Integer, Integer>[] mp = new HashMap[k + 1];
        Arrays.setAll(mp, i -> new HashMap<>());
        int[][] g = new int[k + 1][3];
        int ans = 0;
        for (int i = 0; i < n; ++i) {
            for (int h = 0; h <= k; ++h) {
                f[i][h] = mp[h].getOrDefault(nums[i], 0);
                if (h > 0) {
                    if (g[h - 1][0] != nums[i]) {
                        f[i][h] = Math.max(f[i][h], g[h - 1][1]);
                    } else {
                        f[i][h] = Math.max(f[i][h], g[h - 1][2]);
                    }
                }
                ++f[i][h];
                mp[h].merge(nums[i], f[i][h], Integer::max);
                if (g[h][0] != nums[i]) {
                    if (f[i][h] >= g[h][1]) {
                        g[h][2] = g[h][1];
                        g[h][1] = f[i][h];
                        g[h][0] = nums[i];
                    } else {
                        g[h][2] = Math.max(g[h][2], f[i][h]);
                    }
                } else {
                    g[h][1] = Math.max(g[h][1], f[i][h]);
                }
                ans = Math.max(ans, f[i][h]);
            }
        }
        return ans;
    }
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
