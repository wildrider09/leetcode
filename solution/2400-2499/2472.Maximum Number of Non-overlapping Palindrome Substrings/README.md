---
comments: true
difficulty: Hard
edit_url: https://github.com/doocs/leetcode/edit/main/solution/2400-2499/2472.Maximum%20Number%20of%20Non-overlapping%20Palindrome%20Substrings/README_EN.md
rating: 2013
source: Weekly Contest 319 Q4
tags:
    - Greedy
    - Two Pointers
    - String
    - Dynamic Programming
---

<!-- problem:start -->

# [2472. Maximum Number of Non-overlapping Palindrome Substrings](https://leetcode.com/problems/maximum-number-of-non-overlapping-palindrome-substrings)

[Chinese Version](/solution/2400-2499/2472.Maximum%20Number%20of%20Non-overlapping%20Palindrome%20Substrings/README.md)

## Description

<!-- description:start -->

<p>You are given a string <code>s</code> and a <strong>positive</strong> integer <code>k</code>.</p>

<p>Select a set of <strong>non-overlapping</strong> substrings from the string <code>s</code> that satisfy the following conditions:</p>

<ul>
	<li>The <strong>length</strong> of each substring is <strong>at least</strong> <code>k</code>.</li>
	<li>Each substring is a <strong>palindrome</strong>.</li>
</ul>

<p>Return <em>the <strong>maximum</strong> number of substrings in an optimal selection</em>.</p>

<p>A <strong>substring</strong> is a contiguous sequence of characters within a string.</p>

<p>&nbsp;</p>
<p><strong class="example">Example 1:</strong></p>

<pre>
<strong>Input:</strong> s = &quot;abaccdbbd&quot;, k = 3
<strong>Output:</strong> 2
<strong>Explanation:</strong> We can select the substrings underlined in s = &quot;<u><strong>aba</strong></u>cc<u><strong>dbbd</strong></u>&quot;. Both &quot;aba&quot; and &quot;dbbd&quot; are palindromes and have a length of at least k = 3.
It can be shown that we cannot find a selection with more than two valid substrings.
</pre>

<p><strong class="example">Example 2:</strong></p>

<pre>
<strong>Input:</strong> s = &quot;adbcda&quot;, k = 2
<strong>Output:</strong> 0
<strong>Explanation:</strong> There is no palindrome substring of length at least 2 in the string.
</pre>

<p>&nbsp;</p>
<p><strong>Constraints:</strong></p>

<ul>
	<li><code>1 &lt;= k &lt;= s.length &lt;= 2000</code></li>
	<li><code>s</code> consists of lowercase English letters.</li>
</ul>

<!-- description:end -->

## Solutions

<!-- solution:start -->

### Solution 1: Preprocessing + Memoization Search

First, preprocess the string $s$ to get $dp[i][j]$, which represents whether the substring $s[i,..j]$ is a palindrome.

Then, define a function $dfs(i)$ to represent the maximum number of non-overlapping palindrome substrings that can be selected from the substring $s[i,..]$, i.e.,

$$
\begin{aligned}
dfs(i) &= \begin{cases}
0, & i \geq n \\
\max\{dfs(i + 1), \max_{j \geq i + k - 1} \{dfs(j + 1) + 1\}\}, & i < n
\end{cases}
\end{aligned}
$$

The time complexity is $O(n^2)$, and the space complexity is $O(n^2)$. Here, $n$ is the length of the string $s$.

<!-- tabs:start -->

#### Java

```java
class Solution {
    private boolean[][] dp;
    private int[] f;
    private String s;
    private int n;
    private int k;

    public int maxPalindromes(String s, int k) {
        n = s.length();
        f = new int[n];
        this.s = s;
        this.k = k;
        dp = new boolean[n][n];
        for (int i = 0; i < n; ++i) {
            Arrays.fill(dp[i], true);
            f[i] = -1;
        }
        for (int i = n - 1; i >= 0; --i) {
            for (int j = i + 1; j < n; ++j) {
                dp[i][j] = s.charAt(i) == s.charAt(j) && dp[i + 1][j - 1];
            }
        }
        return dfs(0);
    }

    private int dfs(int i) {
        if (i >= n) {
            return 0;
        }
        if (f[i] != -1) {
            return f[i];
        }
        int ans = dfs(i + 1);
        for (int j = i + k - 1; j < n; ++j) {
            if (dp[i][j]) {
                ans = Math.max(ans, 1 + dfs(j + 1));
            }
        }
        f[i] = ans;
        return ans;
    }
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
