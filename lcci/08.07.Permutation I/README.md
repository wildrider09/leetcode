---
comments: true
difficulty: Medium
edit_url: https://github.com/doocs/leetcode/edit/main/lcci/08.07.Permutation%20I/README_EN.md
---

<!-- problem:start -->

# [08.07. Permutation I](https://leetcode.cn/problems/permutation-i-lcci)

[Chinese Version](/lcci/08.07.Permutation%20I/README.md)

## Description

<!-- description:start -->

<p>Write a method to compute all permutations of a string of unique characters.</p>

<p><strong>Example1:</strong></p>

<pre>

<strong> Input</strong>: S = &quot;qwe&quot;

<strong> Output</strong>: [&quot;qwe&quot;, &quot;qew&quot;, &quot;wqe&quot;, &quot;weq&quot;, &quot;ewq&quot;, &quot;eqw&quot;]

</pre>

<p><strong>Example2:</strong></p>

<pre>

<strong> Input</strong>: S = &quot;ab&quot;

<strong> Output</strong>: [&quot;ab&quot;, &quot;ba&quot;]

</pre>

<p><strong>Note:</strong></p>

<ol>
	<li>All characters are English letters.</li>
	<li><code>1 &lt;= S.length &lt;= 9</code></li>
</ol>

<!-- description:end -->

## Solutions

<!-- solution:start -->

### Solution 1: DFS (Backtracking)

We design a function $\textit{dfs}(i)$ to represent that the first $i$ positions have been filled, and now we need to fill the $(i+1)$-th position. Enumerate all possible characters, and if the character has not been used, fill in this character and continue to fill the next position until all positions are filled.

The time complexity is $O(n \times n!)$, where $n$ is the length of the string. There are $n!$ permutations in total, and each permutation takes $O(n)$ time to construct.

<!-- tabs:start -->

#### Java

```java
class Solution {
    private char[] s;
    private char[] t;
    private boolean[] vis;
    private List<String> ans = new ArrayList<>();

    public String[] permutation(String S) {
        s = S.toCharArray();
        int n = s.length;
        vis = new boolean[n];
        t = new char[n];
        dfs(0);
        return ans.toArray(new String[0]);
    }

    private void dfs(int i) {
        if (i >= s.length) {
            ans.add(new String(t));
            return;
        }
        for (int j = 0; j < s.length; ++j) {
            if (!vis[j]) {
                vis[j] = true;
                t[i] = s[j];
                dfs(i + 1);
                vis[j] = false;
            }
        }
    }
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
