---
comments: true
difficulty: Medium
edit_url: https://github.com/doocs/leetcode/edit/main/solution/3200-3299/3291.Minimum%20Number%20of%20Valid%20Strings%20to%20Form%20Target%20I/README_EN.md
rating: 2081
source: Weekly Contest 415 Q3
tags:
    - Trie
    - Segment Tree
    - Array
    - String
    - Binary Search
    - Dynamic Programming
    - String Matching
    - Hash Function
    - Rolling Hash
---

<!-- problem:start -->

# [3291. Minimum Number of Valid Strings to Form Target I](https://leetcode.com/problems/minimum-number-of-valid-strings-to-form-target-i)

[Chinese Version](/solution/3200-3299/3291.Minimum%20Number%20of%20Valid%20Strings%20to%20Form%20Target%20I/README.md)

## Description

<!-- description:start -->

<p>You are given an array of strings <code>words</code> and a string <code>target</code>.</p>

<p>A string <code>x</code> is called <strong>valid</strong> if <code>x</code> is a <span data-keyword="string-prefix">prefix</span> of <strong>any</strong> string in <code>words</code>.</p>

<p>Return the <strong>minimum</strong> number of <strong>valid</strong> strings that can be <em>concatenated</em> to form <code>target</code>. If it is <strong>not</strong> possible to form <code>target</code>, return <code>-1</code>.</p>

<p>&nbsp;</p>
<p><strong class="example">Example 1:</strong></p>

<div class="example-block">
<p><strong>Input:</strong> <span class="example-io">words = [&quot;abc&quot;,&quot;aaaaa&quot;,&quot;bcdef&quot;], target = &quot;aabcdabc&quot;</span></p>

<p><strong>Output:</strong> <span class="example-io">3</span></p>

<p><strong>Explanation:</strong></p>

<p>The target string can be formed by concatenating:</p>

<ul>
	<li>Prefix of length 2 of <code>words[1]</code>, i.e. <code>&quot;aa&quot;</code>.</li>
	<li>Prefix of length 3 of <code>words[2]</code>, i.e. <code>&quot;bcd&quot;</code>.</li>
	<li>Prefix of length 3 of <code>words[0]</code>, i.e. <code>&quot;abc&quot;</code>.</li>
</ul>
</div>

<p><strong class="example">Example 2:</strong></p>

<div class="example-block">
<p><strong>Input:</strong> <span class="example-io">words = [&quot;abababab&quot;,&quot;ab&quot;], target = &quot;ababaababa&quot;</span></p>

<p><strong>Output:</strong> <span class="example-io">2</span></p>

<p><strong>Explanation:</strong></p>

<p>The target string can be formed by concatenating:</p>

<ul>
	<li>Prefix of length 5 of <code>words[0]</code>, i.e. <code>&quot;ababa&quot;</code>.</li>
	<li>Prefix of length 5 of <code>words[0]</code>, i.e. <code>&quot;ababa&quot;</code>.</li>
</ul>
</div>

<p><strong class="example">Example 3:</strong></p>

<div class="example-block">
<p><strong>Input:</strong> <span class="example-io">words = [&quot;abcdef&quot;], target = &quot;xyz&quot;</span></p>

<p><strong>Output:</strong> <span class="example-io">-1</span></p>
</div>

<p>&nbsp;</p>
<p><strong>Constraints:</strong></p>

<ul>
	<li><code>1 &lt;= words.length &lt;= 100</code></li>
	<li><code>1 &lt;= words[i].length &lt;= 5 * 10<sup>3</sup></code></li>
	<li>The input is generated such that <code>sum(words[i].length) &lt;= 10<sup>5</sup></code>.</li>
	<li><code>words[i]</code> consists only of lowercase English letters.</li>
	<li><code>1 &lt;= target.length &lt;= 5 * 10<sup>3</sup></code></li>
	<li><code>target</code> consists only of lowercase English letters.</li>
</ul>

<!-- description:end -->

## Solutions

<!-- solution:start -->

### Solution 1: Trie + Memoization

We can use a trie to store all valid strings and then use memoization to calculate the answer.

We design a function $\textit{dfs}(i)$, which represents the minimum number of strings needed to concatenate starting from the $i$-th character of the string $\textit{target}$. The answer is $\textit{dfs}(0)$.

The function $\textit{dfs}(i)$ is calculated as follows:

- If $i \geq n$, it means the string $\textit{target}$ has been completely traversed, so we return $0$;
- Otherwise, we can find valid strings in the trie that start with $\textit{target}[i]$, and then recursively calculate $\textit{dfs}(i + \text{len}(w))$, where $w$ is the valid string found. We take the minimum of these values and add $1$ as the return value of $\textit{dfs}(i)$.

To avoid redundant calculations, we use memoization.

The time complexity is $O(n^2 + L)$, and the space complexity is $O(n + L)$. Here, $n$ is the length of the string $\textit{target}$, and $L$ is the total length of all valid strings.

<!-- tabs:start -->

#### Java

```java
class Trie {
    Trie[] children = new Trie[26];

    void insert(String w) {
        Trie node = this;
        for (int i = 0; i < w.length(); ++i) {
            int j = w.charAt(i) - 'a';
            if (node.children[j] == null) {
                node.children[j] = new Trie();
            }
            node = node.children[j];
        }
    }
}

class Solution {
    private Integer[] f;
    private char[] s;
    private Trie trie;
    private final int inf = 1 << 30;

    public int minValidStrings(String[] words, String target) {
        trie = new Trie();
        for (String w : words) {
            trie.insert(w);
        }
        s = target.toCharArray();
        f = new Integer[s.length];
        int ans = dfs(0);
        return ans < inf ? ans : -1;
    }

    private int dfs(int i) {
        if (i >= s.length) {
            return 0;
        }
        if (f[i] != null) {
            return f[i];
        }
        Trie node = trie;
        f[i] = inf;
        for (int j = i; j < s.length; ++j) {
            int k = s[j] - 'a';
            if (node.children[k] == null) {
                break;
            }
            f[i] = Math.min(f[i], 1 + dfs(j + 1));
            node = node.children[k];
        }
        return f[i];
    }
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
