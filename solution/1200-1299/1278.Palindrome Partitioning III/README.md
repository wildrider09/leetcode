---
comments: true
difficulty: Hard
edit_url: https://github.com/doocs/leetcode/edit/main/solution/1200-1299/1278.Palindrome%20Partitioning%20III/README_EN.md
rating: 1979
source: Weekly Contest 165 Q4
tags:
    - String
    - Dynamic Programming
---

<!-- problem:start -->

# [1278. Palindrome Partitioning III](https://leetcode.com/problems/palindrome-partitioning-iii)

[Chinese Version](/solution/1200-1299/1278.Palindrome%20Partitioning%20III/README.md)

## Description

<!-- description:start -->

<p>You are given a string <code>s</code> containing lowercase letters and an integer <code>k</code>. You need to :</p>

<ul>
	<li>First, change some characters of <code>s</code> to other lowercase English letters.</li>
	<li>Then divide <code>s</code> into <code>k</code> non-empty disjoint substrings such that each substring is a palindrome.</li>
</ul>

<p>Return <em>the minimal number of characters that you need to change to divide the string</em>.</p>

<p>&nbsp;</p>
<p><strong class="example">Example 1:</strong></p>

<pre>
<strong>Input:</strong> s = &quot;abc&quot;, k = 2
<strong>Output:</strong> 1
<strong>Explanation:</strong>&nbsp;You can split the string into &quot;ab&quot; and &quot;c&quot;, and change 1 character in &quot;ab&quot; to make it palindrome.
</pre>

<p><strong class="example">Example 2:</strong></p>

<pre>
<strong>Input:</strong> s = &quot;aabbc&quot;, k = 3
<strong>Output:</strong> 0
<strong>Explanation:</strong>&nbsp;You can split the string into &quot;aa&quot;, &quot;bb&quot; and &quot;c&quot;, all of them are palindrome.</pre>

<p><strong class="example">Example 3:</strong></p>

<pre>
<strong>Input:</strong> s = &quot;leetcode&quot;, k = 8
<strong>Output:</strong> 0
</pre>

<p>&nbsp;</p>
<p><strong>Constraints:</strong></p>

<ul>
	<li><code>1 &lt;= k &lt;= s.length &lt;= 100</code>.</li>
	<li><code>s</code> only contains lowercase English letters.</li>
</ul>

<!-- description:end -->

## Solutions

<!-- solution:start -->

### Solution 1: Dynamic Programming

We define $f[i][j]$ to represent the minimum number of changes needed to partition the first $i$ characters of the string $s$ into $j$ palindromic substrings. We assume the index $i$ starts from 1, and the answer is $f[n][k]$.

For $f[i][j]$, we can enumerate the position $h$ of the last character of the $(j-1)$-th palindromic substring. Then $f[i][j]$ is equal to the minimum value of $f[h][j-1] + g[h][i-1]$, where $g[h][i-1]$ represents the minimum number of changes needed to turn the substring $s[h..i-1]$ into a palindrome (this part can be preprocessed with a time complexity of $O(n^2)$).

The time complexity is $O(n^2 \times k)$, and the space complexity is $O(n \times (n + k))$. Where $n$ is the length of the string $s$.

<!-- tabs:start -->

#### Java

```java
class Solution {
    public int palindromePartition(String s, int k) {
        int n = s.length();
        int[][] g = new int[n][n];
        for (int i = n - 1; i >= 0; --i) {
            for (int j = i; j < n; ++j) {
                g[i][j] = s.charAt(i) != s.charAt(j) ? 1 : 0;
                if (i + 1 < j) {
                    g[i][j] += g[i + 1][j - 1];
                }
            }
        }
        int[][] f = new int[n + 1][k + 1];
        for (int i = 1; i <= n; ++i) {
            for (int j = 1; j <= Math.min(i, k); ++j) {
                if (j == 1) {
                    f[i][j] = g[0][i - 1];
                } else {
                    f[i][j] = 10000;
                    for (int h = j - 1; h < i; ++h) {
                        f[i][j] = Math.min(f[i][j], f[h][j - 1] + g[h][i - 1]);
                    }
                }
            }
        }
        return f[n][k];
    }
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
