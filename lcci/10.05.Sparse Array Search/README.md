---
comments: true
difficulty: Easy
edit_url: https://github.com/doocs/leetcode/edit/main/lcci/10.05.Sparse%20Array%20Search/README_EN.md
---

<!-- problem:start -->

# [10.05. Sparse Array Search](https://leetcode.cn/problems/sparse-array-search-lcci)

## Description

<!-- description:start -->

<p>Given a sorted array of strings that is interspersed with empty strings, write a method to find the location of a given string.</p>

<p><strong>Example1:</strong></p>

<pre>

<strong> Input</strong>: words = [&quot;at&quot;, &quot;&quot;, &quot;&quot;, &quot;&quot;, &quot;ball&quot;, &quot;&quot;, &quot;&quot;, &quot;car&quot;, &quot;&quot;, &quot;&quot;,&quot;dad&quot;, &quot;&quot;, &quot;&quot;], s = &quot;ta&quot;

<strong> Output</strong>: -1

<strong> Explanation</strong>: Return -1 if <code>s</code> is not in <code>words</code>.

</pre>

<p><strong>Example2:</strong></p>

<pre>

<strong> Input</strong>: words = [&quot;at&quot;, &quot;&quot;, &quot;&quot;, &quot;&quot;, &quot;ball&quot;, &quot;&quot;, &quot;&quot;, &quot;car&quot;, &quot;&quot;, &quot;&quot;,&quot;dad&quot;, &quot;&quot;, &quot;&quot;], s = &quot;ball&quot;

<strong> Output</strong>: 4

</pre>

<p><strong>Note:</strong></p>

<ol>
	<li><code>1 &lt;= words.length &lt;= 1000000</code></li>
</ol>

<!-- description:end -->

## Solutions

<!-- solution:start -->

### Solution 1: Binary Search

We design a function $dfs(i, j)$ to find the target string in the array $nums[i, j]$. If found, return the index of the target string, otherwise return $-1$. So the answer is $dfs(0, n-1)$.

The implementation of the function $dfs(i, j)$ is as follows:

1. If $i > j$, return $-1$.
2. Otherwise, we take the middle position $mid = (i + j) / 2$, then recursively call $dfs(i, mid-1)$. If the return value is not $-1$, it means that the target string is found in the left half, return it directly. Otherwise, if $words[mid] = s$, it means that the target string is found, return it directly. Otherwise, recursively call $dfs(mid+1, j)$ and return.

In the worst case, the time complexity is $O(n \times m)$, and the space complexity is $O(n)$. Where $n$ and $m$ are the length of the string array and the length of the string $s$, respectively.

<!-- tabs:start -->

#### Java

```java
class Solution {
    public int findString(String[] words, String s) {
        return dfs(words, s, 0, words.length - 1);
    }

    private int dfs(String[] words, String s, int i, int j) {
        if (i > j) {
            return -1;
        }
        int mid = (i + j) >> 1;
        int l = dfs(words, s, i, mid - 1);
        if (l != -1) {
            return l;
        }
        if (words[mid].equals(s)) {
            return mid;
        }
        return dfs(words, s, mid + 1, j);
    }
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
