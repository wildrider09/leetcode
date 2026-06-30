---
comments: true
difficulty: Medium
edit_url: https://github.com/doocs/leetcode/edit/main/solution/0500-0599/0524.Longest%20Word%20in%20Dictionary%20through%20Deleting/README_EN.md
tags:
    - Array
    - Two Pointers
    - String
    - Sorting
---

<!-- problem:start -->

# [524. Longest Word in Dictionary through Deleting](https://leetcode.com/problems/longest-word-in-dictionary-through-deleting)

[Chinese Version](/solution/0500-0599/0524.Longest%20Word%20in%20Dictionary%20through%20Deleting/README.md)

## Description

<!-- description:start -->

<p>Given a string <code>s</code> and a string array <code>dictionary</code>, return <em>the longest string in the dictionary that can be formed by deleting some of the given string characters</em>. If there is more than one possible result, return the longest word with the smallest lexicographical order. If there is no possible result, return the empty string.</p>

<p>&nbsp;</p>
<p><strong class="example">Example 1:</strong></p>

<pre>
<strong>Input:</strong> s = &quot;abpcplea&quot;, dictionary = [&quot;ale&quot;,&quot;apple&quot;,&quot;monkey&quot;,&quot;plea&quot;]
<strong>Output:</strong> &quot;apple&quot;
</pre>

<p><strong class="example">Example 2:</strong></p>

<pre>
<strong>Input:</strong> s = &quot;abpcplea&quot;, dictionary = [&quot;a&quot;,&quot;b&quot;,&quot;c&quot;]
<strong>Output:</strong> &quot;a&quot;
</pre>

<p>&nbsp;</p>
<p><strong>Constraints:</strong></p>

<ul>
	<li><code>1 &lt;= s.length &lt;= 1000</code></li>
	<li><code>1 &lt;= dictionary.length &lt;= 1000</code></li>
	<li><code>1 &lt;= dictionary[i].length &lt;= 1000</code></li>
	<li><code>s</code> and <code>dictionary[i]</code> consist of lowercase English letters.</li>
</ul>

<!-- description:end -->

## Solutions

<!-- solution:start -->

### Solution 1: Subsequence Judgment

We define a function $check(s, t)$ to determine whether string $s$ is a subsequence of string $t$. We can use a two-pointer approach, initializing two pointers $i$ and $j$ to point to the beginning of strings $s$ and $t$ respectively, then continuously move pointer $j$. If $s[i]$ equals $t[j]$, then move pointer $i$. Finally, check if $i$ equals the length of $s$. If $i$ equals the length of $s$, it means $s$ is a subsequence of $t$.

We initialize the answer string $ans$ as an empty string. Then, we iterate through each string $t$ in the array $dictionary$. If $t$ is a subsequence of $s$, and the length of $t$ is greater than the length of $ans$, or the length of $t$ is equal to the length of $ans$ but $t$ is lexicographically smaller than $ans$, then we update $ans$ to $t$.

The time complexity is $O(d \times (m + n))$, where $d$ is the length of the string list, and $m$ and $n$ are the lengths of string $s$ and the average length of strings in the list, respectively. The space complexity is $O(1)$.

<!-- tabs:start -->

#### Java

```java
class Solution {
    public String findLongestWord(String s, List<String> dictionary) {
        String ans = "";
        for (String t : dictionary) {
            int a = ans.length(), b = t.length();
            if (check(t, s) && (a < b || (a == b && t.compareTo(ans) < 0))) {
                ans = t;
            }
        }
        return ans;
    }

    private boolean check(String s, String t) {
        int m = s.length(), n = t.length();
        int i = 0;
        for (int j = 0; i < m && j < n; ++j) {
            if (s.charAt(i) == t.charAt(j)) {
                ++i;
            }
        }
        return i == m;
    }
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
