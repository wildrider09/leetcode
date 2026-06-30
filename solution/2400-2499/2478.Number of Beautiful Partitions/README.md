---
comments: true
difficulty: Hard
edit_url: https://github.com/doocs/leetcode/edit/main/solution/2400-2499/2478.Number%20of%20Beautiful%20Partitions/README_EN.md
rating: 2344
source: Weekly Contest 320 Q4
tags:
    - String
    - Dynamic Programming
    - Prefix Sum
---

<!-- problem:start -->

# [2478. Number of Beautiful Partitions](https://leetcode.com/problems/number-of-beautiful-partitions)

[Chinese Version](/solution/2400-2499/2478.Number%20of%20Beautiful%20Partitions/README.md)

## Description

<!-- description:start -->

<p>You are given a string <code>s</code> that consists of the digits <code>&#39;1&#39;</code> to <code>&#39;9&#39;</code> and two integers <code>k</code> and <code>minLength</code>.</p>

<p>A partition of <code>s</code> is called <strong>beautiful</strong> if:</p>

<ul>
	<li><code>s</code> is partitioned into <code>k</code> non-intersecting substrings.</li>
	<li>Each substring has a length of <strong>at least</strong> <code>minLength</code>.</li>
	<li>Each substring starts with a <strong>prime</strong> digit and ends with a <strong>non-prime</strong> digit. Prime digits are <code>&#39;2&#39;</code>, <code>&#39;3&#39;</code>, <code>&#39;5&#39;</code>, and <code>&#39;7&#39;</code>, and the rest of the digits are non-prime.</li>
</ul>

<p>Return<em> the number of <strong>beautiful</strong> partitions of </em><code>s</code>. Since the answer may be very large, return it <strong>modulo</strong> <code>10<sup>9</sup> + 7</code>.</p>

<p>A <strong>substring</strong> is a contiguous sequence of characters within a string.</p>

<p>&nbsp;</p>
<p><strong class="example">Example 1:</strong></p>

<pre>
<strong>Input:</strong> s = &quot;23542185131&quot;, k = 3, minLength = 2
<strong>Output:</strong> 3
<strong>Explanation:</strong> There exists three ways to create a beautiful partition:
&quot;2354 | 218 | 5131&quot;
&quot;2354 | 21851 | 31&quot;
&quot;2354218 | 51 | 31&quot;
</pre>

<p><strong class="example">Example 2:</strong></p>

<pre>
<strong>Input:</strong> s = &quot;23542185131&quot;, k = 3, minLength = 3
<strong>Output:</strong> 1
<strong>Explanation:</strong> There exists one way to create a beautiful partition: &quot;2354 | 218 | 5131&quot;.
</pre>

<p><strong class="example">Example 3:</strong></p>

<pre>
<strong>Input:</strong> s = &quot;3312958&quot;, k = 3, minLength = 1
<strong>Output:</strong> 1
<strong>Explanation:</strong> There exists one way to create a beautiful partition: &quot;331 | 29 | 58&quot;.
</pre>

<p>&nbsp;</p>
<p><strong>Constraints:</strong></p>

<ul>
	<li><code>1 &lt;= k, minLength &lt;= s.length &lt;= 1000</code></li>
	<li><code>s</code> consists of the digits <code>&#39;1&#39;</code> to <code>&#39;9&#39;</code>.</li>
</ul>

<!-- description:end -->

## Solutions

<!-- solution:start -->

### Solution 1: Dynamic Programming

We define $f[i][j]$ as the number of schemes for dividing the first $i$ characters into $j$ sections. Initialize $f[0][0] = 1$, and the rest $f[i][j] = 0$.

First, we need to determine whether the $i$th character can be the last character of the $j$th section, it needs to meet the following conditions simultaneously:

1. The $i$th character is a non-prime number;
1. The $i+1$th character is a prime number, or the $i$th character is the last character of the entire string.

If the $i$th character cannot be the last character of the $j$th section, then $f[i][j]=0$. Otherwise, we have:

$$
f[i][j]=\sum_{t=0}^{i-minLength}f[t][j-1]
$$

That is to say, we need to enumerate which character is the end of the previous section. Here we use the prefix sum array $g[i][j] = \sum_{t=0}^{i}f[t][j]$ to optimize the time complexity of enumeration.

Then we have:

$$
f[i][j]=g[i-minLength][j-1]
$$

The time complexity is $O(n \times k)$, and the space complexity is $O(n \times k)$. Where $n$ and $k$ are the length of the string $s$ and the number of sections to be divided, respectively.

<!-- tabs:start -->

#### Java

```java
class Solution {
    public int beautifulPartitions(String s, int k, int minLength) {
        int n = s.length();
        if (!prime(s.charAt(0)) || prime(s.charAt(n - 1))) {
            return 0;
        }
        int[][] f = new int[n + 1][k + 1];
        int[][] g = new int[n + 1][k + 1];
        f[0][0] = 1;
        g[0][0] = 1;
        final int mod = (int) 1e9 + 7;
        for (int i = 1; i <= n; ++i) {
            if (i >= minLength && !prime(s.charAt(i - 1)) && (i == n || prime(s.charAt(i)))) {
                for (int j = 1; j <= k; ++j) {
                    f[i][j] = g[i - minLength][j - 1];
                }
            }
            for (int j = 0; j <= k; ++j) {
                g[i][j] = (g[i - 1][j] + f[i][j]) % mod;
            }
        }
        return f[n][k];
    }

    private boolean prime(char c) {
        return c == '2' || c == '3' || c == '5' || c == '7';
    }
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
