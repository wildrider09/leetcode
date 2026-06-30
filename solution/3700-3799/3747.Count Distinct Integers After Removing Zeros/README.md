---
comments: true
difficulty: Medium
edit_url: https://github.com/doocs/leetcode/edit/main/solution/3700-3799/3747.Count%20Distinct%20Integers%20After%20Removing%20Zeros/README_EN.md
rating: 1848
source: Weekly Contest 476 Q3
tags:
    - Math
    - Dynamic Programming
---

<!-- problem:start -->

# [3747. Count Distinct Integers After Removing Zeros](https://leetcode.com/problems/count-distinct-integers-after-removing-zeros)

[Chinese Version](/solution/3700-3799/3747.Count%20Distinct%20Integers%20After%20Removing%20Zeros/README.md)

## Description

<!-- description:start -->

<p>You are given a <strong>positive</strong> integer <code>n</code>.</p>

<p>For every integer <code>x</code> from 1 to <code>n</code>, we write down the integer obtained by removing all zeros from the decimal representation of <code>x</code>.</p>

<p>Return an integer denoting the number of <strong>distinct</strong> integers written down.</p>

<p>&nbsp;</p>
<p><strong class="example">Example 1:</strong></p>

<div class="example-block">
<p><strong>Input:</strong> <span class="example-io">n = 10</span></p>

<p><strong>Output:</strong> <span class="example-io">9</span></p>

<p><strong>Explanation:</strong></p>

<p>The integers we wrote down are 1, 2, 3, 4, 5, 6, 7, 8, 9, 1. There are 9 distinct integers (1, 2, 3, 4, 5, 6, 7, 8, 9).</p>
</div>

<p><strong class="example">Example 2:</strong></p>

<div class="example-block">
<p><strong>Input:</strong> <span class="example-io">n = 3</span></p>

<p><strong>Output:</strong> <span class="example-io">3</span></p>

<p><strong>Explanation:</strong></p>

<p>The integers we wrote down are 1, 2, 3. There are 3 distinct integers (1, 2, 3).</p>
</div>

<p>&nbsp;</p>
<p><strong>Constraints:</strong></p>

<ul>
	<li><code>1 &lt;= n &lt;= 10<sup>15</sup></code></li>
</ul>

<!-- description:end -->

## Solutions

<!-- solution:start -->

### Solution 1: Digit DP

The problem essentially asks us to count the number of integers in the range $[1, n]$ that do not contain the digit 0. We can solve this problem using digit DP.

We design a function $\text{dfs}(i, \text{zero}, \text{lead}, \text{limit})$, which represents the number of valid solutions when we are currently processing the $i$-th digit of the number. We use $\text{zero}$ to indicate whether a non-zero digit has appeared in the current number, $\text{lead}$ to indicate whether we are still processing leading zeros, and $\text{limit}$ to indicate whether the current number is constrained by the upper bound. The answer is $\text{dfs}(0, 0, 1, 1)$.

In the function $\text{dfs}(i, \text{zero}, \text{lead}, \text{limit})$, if $i$ is greater than or equal to the length of the number, we can check $\text{zero}$ and $\text{lead}$. If $\text{zero}$ is false and $\text{lead}$ is false, it means the current number does not contain 0, so we return $1$; otherwise, we return $0$.

For $\text{dfs}(i, \text{zero}, \text{lead}, \text{limit})$, we can enumerate the value of the current digit $d$, then recursively calculate $\text{dfs}(i+1, \text{nxt\_zero}, \text{nxt\_lead}, \text{nxt\_limit})$, where $\text{nxt\_zero}$ indicates whether a non-zero digit has appeared in the current number, $\text{nxt\_lead}$ indicates whether we are still processing leading zeros, and $\text{nxt\_limit}$ indicates whether the current number is constrained by the upper bound. If $\text{limit}$ is true, then $up$ is the upper bound of the current digit; otherwise, $up$ is $9$.

The time complexity is $O(\log_{10} n \times D)$ and the space complexity is $O(\log_{10} n)$, where $D$ represents the count of digits from 0 to 9.

<!-- tabs:start -->

#### Java

```java
class Solution {
    private char[] s;
    private Long[][][][] f;

    public long countDistinct(long n) {
        s = String.valueOf(n).toCharArray();
        f = new Long[s.length][2][2][2];
        return dfs(0, 0, 1, 1);
    }

    private long dfs(int i, int zero, int lead, int limit) {
        if (i == s.length) {
            return (zero == 0 && lead == 0) ? 1 : 0;
        }

        if (limit == 0 && f[i][zero][lead][limit] != null) {
            return f[i][zero][lead][limit];
        }

        int up = limit == 1 ? s[i] - '0' : 9;
        long ans = 0;
        for (int d = 0; d <= up; d++) {
            int nxtZero = zero == 1 || (d == 0 && lead == 0) ? 1 : 0;
            int nxtLead = (lead == 1 && d == 0) ? 1 : 0;
            int nxtLimit = (limit == 1 && d == up) ? 1 : 0;
            ans += dfs(i + 1, nxtZero, nxtLead, nxtLimit);
        }

        if (limit == 0) {
            f[i][zero][lead][limit] = ans;
        }
        return ans;
    }
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
