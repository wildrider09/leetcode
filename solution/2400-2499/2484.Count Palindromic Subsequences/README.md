---
comments: true
difficulty: Hard
edit_url: https://github.com/doocs/leetcode/edit/main/solution/2400-2499/2484.Count%20Palindromic%20Subsequences/README_EN.md
rating: 2223
source: Biweekly Contest 92 Q4
tags:
    - String
    - Dynamic Programming
---

<!-- problem:start -->

# [2484. Count Palindromic Subsequences](https://leetcode.com/problems/count-palindromic-subsequences)

[Chinese Version](/solution/2400-2499/2484.Count%20Palindromic%20Subsequences/README.md)

## Description

<!-- description:start -->

<p>Given a string of digits <code>s</code>, return <em>the number of <strong>palindromic subsequences</strong> of</em> <code>s</code><em> having length </em><code>5</code>. Since the answer may be very large, return it <strong>modulo</strong> <code>10<sup>9</sup> + 7</code>.</p>

<p><strong>Note:</strong></p>

<ul>
	<li>A string is <strong>palindromic</strong> if it reads the same forward and backward.</li>
	<li>A <strong>subsequence</strong> is a string that can be derived from another string by deleting some or no characters without changing the order of the remaining characters.</li>
</ul>

<p>&nbsp;</p>
<p><strong class="example">Example 1:</strong></p>

<pre>
<strong>Input:</strong> s = &quot;103301&quot;
<strong>Output:</strong> 2
<strong>Explanation:</strong> 
There are 6 possible subsequences of length 5: &quot;10330&quot;,&quot;10331&quot;,&quot;10301&quot;,&quot;10301&quot;,&quot;13301&quot;,&quot;03301&quot;. 
Two of them (both equal to &quot;10301&quot;) are palindromic.
</pre>

<p><strong class="example">Example 2:</strong></p>

<pre>
<strong>Input:</strong> s = &quot;0000000&quot;
<strong>Output:</strong> 21
<strong>Explanation:</strong> All 21 subsequences are &quot;00000&quot;, which is palindromic.
</pre>

<p><strong class="example">Example 3:</strong></p>

<pre>
<strong>Input:</strong> s = &quot;9999900000&quot;
<strong>Output:</strong> 2
<strong>Explanation:</strong> The only two palindromic subsequences are &quot;99999&quot; and &quot;00000&quot;.
</pre>

<p>&nbsp;</p>
<p><strong>Constraints:</strong></p>

<ul>
	<li><code>1 &lt;= s.length &lt;= 10<sup>4</sup></code></li>
	<li><code>s</code> consists of digits.</li>
</ul>

<!-- description:end -->

## Solutions

<!-- solution:start -->

### Solution 1: Enumeration + Counting

The time complexity is $O(100 \times n)$, and the space complexity is $O(100 \times n)$. Where $n$ is the length of the string $s$.

<!-- tabs:start -->

#### Java

```java
class Solution {
    private static final int MOD = (int) 1e9 + 7;

    public int countPalindromes(String s) {
        int n = s.length();
        int[][][] pre = new int[n + 2][10][10];
        int[][][] suf = new int[n + 2][10][10];
        int[] t = new int[n];
        for (int i = 0; i < n; ++i) {
            t[i] = s.charAt(i) - '0';
        }
        int[] c = new int[10];
        for (int i = 1; i <= n; ++i) {
            int v = t[i - 1];
            for (int j = 0; j < 10; ++j) {
                for (int k = 0; k < 10; ++k) {
                    pre[i][j][k] = pre[i - 1][j][k];
                }
            }
            for (int j = 0; j < 10; ++j) {
                pre[i][j][v] += c[j];
            }
            c[v]++;
        }
        c = new int[10];
        for (int i = n; i > 0; --i) {
            int v = t[i - 1];
            for (int j = 0; j < 10; ++j) {
                for (int k = 0; k < 10; ++k) {
                    suf[i][j][k] = suf[i + 1][j][k];
                }
            }
            for (int j = 0; j < 10; ++j) {
                suf[i][j][v] += c[j];
            }
            c[v]++;
        }
        long ans = 0;
        for (int i = 1; i <= n; ++i) {
            for (int j = 0; j < 10; ++j) {
                for (int k = 0; k < 10; ++k) {
                    ans += (long) pre[i - 1][j][k] * suf[i + 1][j][k];
                    ans %= MOD;
                }
            }
        }
        return (int) ans;
    }
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
