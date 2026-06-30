---
comments: true
difficulty: Medium
edit_url: https://github.com/doocs/leetcode/edit/main/solution/3500-3599/3503.Longest%20Palindrome%20After%20Substring%20Concatenation%20I/README_EN.md
rating: 1548
source: Weekly Contest 443 Q2
tags:
    - Two Pointers
    - String
    - Dynamic Programming
    - Enumeration
---

<!-- problem:start -->

# [3503. Longest Palindrome After Substring Concatenation I](https://leetcode.com/problems/longest-palindrome-after-substring-concatenation-i)

[Chinese Version](/solution/3500-3599/3503.Longest%20Palindrome%20After%20Substring%20Concatenation%20I/README.md)

## Description

<!-- description:start -->

<p>You are given two strings, <code>s</code> and <code>t</code>.</p>

<p>You can create a new string by selecting a <span data-keyword="substring">substring</span> from <code>s</code> (possibly empty) and a substring from <code>t</code> (possibly empty), then concatenating them <strong>in order</strong>.</p>

<p>Return the length of the <strong>longest</strong> <span data-keyword="palindrome-string">palindrome</span> that can be formed this way.</p>

<p>&nbsp;</p>
<p><strong class="example">Example 1:</strong></p>

<div class="example-block">
<p><strong>Input:</strong> <span class="example-io">s = &quot;a&quot;, t = &quot;a&quot;</span></p>

<p><strong>Output:</strong> <span class="example-io">2</span></p>

<p><strong>Explanation:</strong></p>

<p>Concatenating <code>&quot;a&quot;</code> from <code>s</code> and <code>&quot;a&quot;</code> from <code>t</code> results in <code>&quot;aa&quot;</code>, which is a palindrome of length 2.</p>
</div>

<p><strong class="example">Example 2:</strong></p>

<div class="example-block">
<p><strong>Input:</strong> <span class="example-io">s = &quot;abc&quot;, t = &quot;def&quot;</span></p>

<p><strong>Output:</strong> <span class="example-io">1</span></p>

<p><strong>Explanation:</strong></p>

<p>Since all characters are different, the longest palindrome is any single character, so the answer is 1.</p>
</div>

<p><strong class="example">Example 3:</strong></p>

<div class="example-block">
<p><strong>Input:</strong> <span class="example-io">s = &quot;b&quot;, t = &quot;aaaa&quot;</span></p>

<p><strong>Output:</strong> 4</p>

<p><strong>Explanation:</strong></p>

<p>Selecting &quot;<code>aaaa</code>&quot; from <code>t</code> is the longest palindrome, so the answer is 4.</p>
</div>

<p><strong class="example">Example 4:</strong></p>

<div class="example-block">
<p><strong>Input:</strong> <span class="example-io">s = &quot;abcde&quot;, t = &quot;ecdba&quot;</span></p>

<p><strong>Output:</strong> 5</p>

<p><strong>Explanation:</strong></p>

<p>Concatenating <code>&quot;abc&quot;</code> from <code>s</code> and <code>&quot;ba&quot;</code> from <code>t</code> results in <code>&quot;abcba&quot;</code>, which is a palindrome of length 5.</p>
</div>

<p>&nbsp;</p>
<p><strong>Constraints:</strong></p>

<ul>
	<li><code>1 &lt;= s.length, t.length &lt;= 30</code></li>
	<li><code>s</code> and <code>t</code> consist of lowercase English letters.</li>
</ul>

<!-- description:end -->

## Solutions

<!-- solution:start -->

### Solution 1: Enumerate Palindrome Centers + Dynamic Programming

According to the problem description, the concatenated palindrome string can be composed entirely of string $s$, entirely of string $t$, or a combination of both strings $s$ and $t$. Additionally, there may be extra palindromic substrings in either string $s$ or $t$.

Therefore, we first reverse string $t$ and preprocess arrays $\textit{g1}$ and $\textit{g2}$, where $\textit{g1}[i]$ represents the length of the longest palindromic substring starting at index $i$ in string $s$, and $\textit{g2}[i]$ represents the length of the longest palindromic substring starting at index $i$ in string $t$.

We can initialize the answer $\textit{ans}$ as the maximum value in $\textit{g1}$ and $\textit{g2}$.

Next, we define $\textit{f}[i][j]$ as the length of the palindromic substring ending at the $i$-th character of string $s$ and the $j$-th character of string $t$.

For $\textit{f}[i][j]$, if $s[i - 1]$ equals $t[j - 1]$, then $\textit{f}[i][j] = \textit{f}[i - 1][j - 1] + 1$. We then update the answer:

$$
\textit{ans} = \max(\textit{ans}, \textit{f}[i][j] \times 2 + (0 \text{ if } i \geq m \text{ else } \textit{g1}[i])) \\
\textit{ans} = \max(\textit{ans}, \textit{f}[i][j] \times 2 + (0 \text{ if } j \geq n \text{ else } \textit{g2}[j]))
$$

Finally, we return the answer $\textit{ans}$.

The time complexity is $O(m \times (m + n))$, and the space complexity is $O(m \times n)$, where $m$ and $n$ are the lengths of strings $s$ and $t$, respectively.

<!-- tabs:start -->

#### Java

```java
class Solution {
    public int longestPalindrome(String S, String T) {
        char[] s = S.toCharArray();
        char[] t = new StringBuilder(T).reverse().toString().toCharArray();
        int m = s.length, n = t.length;
        int[] g1 = calc(s), g2 = calc(t);
        int ans = Math.max(Arrays.stream(g1).max().getAsInt(), Arrays.stream(g2).max().getAsInt());
        int[][] f = new int[m + 1][n + 1];
        for (int i = 1; i <= m; ++i) {
            for (int j = 1; j <= n; ++j) {
                if (s[i - 1] == t[j - 1]) {
                    f[i][j] = f[i - 1][j - 1] + 1;
                    ans = Math.max(ans, f[i][j] * 2 + (i < m ? g1[i] : 0));
                    ans = Math.max(ans, f[i][j] * 2 + (j < n ? g2[j] : 0));
                }
            }
        }
        return ans;
    }

    private void expand(char[] s, int[] g, int l, int r) {
        while (l >= 0 && r < s.length && s[l] == s[r]) {
            g[l] = Math.max(g[l], r - l + 1);
            --l;
            ++r;
        }
    }

    private int[] calc(char[] s) {
        int n = s.length;
        int[] g = new int[n];
        for (int i = 0; i < n; ++i) {
            expand(s, g, i, i);
            expand(s, g, i, i + 1);
        }
        return g;
    }
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
