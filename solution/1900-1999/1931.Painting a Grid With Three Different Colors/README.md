---
comments: true
difficulty: Hard
edit_url: https://github.com/doocs/leetcode/edit/main/solution/1900-1999/1931.Painting%20a%20Grid%20With%20Three%20Different%20Colors/README_EN.md
rating: 2170
source: Weekly Contest 249 Q3
tags:
    - Dynamic Programming
---

<!-- problem:start -->

# [1931. Painting a Grid With Three Different Colors](https://leetcode.com/problems/painting-a-grid-with-three-different-colors)

[Chinese Version](/solution/1900-1999/1931.Painting%20a%20Grid%20With%20Three%20Different%20Colors/README.md)

## Description

<!-- description:start -->

<p>You are given two integers <code>m</code> and <code>n</code>. Consider an <code>m x n</code> grid where each cell is initially white. You can paint each cell <strong>red</strong>, <strong>green</strong>, or <strong>blue</strong>. All cells <strong>must</strong> be painted.</p>

<p>Return<em> the number of ways to color the grid with <strong>no two adjacent cells having the same color</strong></em>. Since the answer can be very large, return it <strong>modulo</strong> <code>10<sup>9</sup> + 7</code>.</p>

<p>&nbsp;</p>
<p><strong class="example">Example 1:</strong></p>
<img alt="" src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/1900-1999/1931.Painting%20a%20Grid%20With%20Three%20Different%20Colors/images/colorthegrid.png" style="width: 200px; height: 50px;" />
<pre>
<strong>Input:</strong> m = 1, n = 1
<strong>Output:</strong> 3
<strong>Explanation:</strong> The three possible colorings are shown in the image above.
</pre>

<p><strong class="example">Example 2:</strong></p>
<img alt="" src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/1900-1999/1931.Painting%20a%20Grid%20With%20Three%20Different%20Colors/images/copy-of-colorthegrid.png" style="width: 321px; height: 121px;" />
<pre>
<strong>Input:</strong> m = 1, n = 2
<strong>Output:</strong> 6
<strong>Explanation:</strong> The six possible colorings are shown in the image above.
</pre>

<p><strong class="example">Example 3:</strong></p>

<pre>
<strong>Input:</strong> m = 5, n = 5
<strong>Output:</strong> 580986
</pre>

<p>&nbsp;</p>
<p><strong>Constraints:</strong></p>

<ul>
	<li><code>1 &lt;= m &lt;= 5</code></li>
	<li><code>1 &lt;= n &lt;= 1000</code></li>
</ul>

<!-- description:end -->

## Solutions

<!-- solution:start -->

### Solution 1: State Compression + Dynamic Programming

We notice that the number of rows in the grid does not exceed $5$, so there are at most $3^5=243$ different color schemes in a column.

Therefore, we define $f[i][j]$ to represent the number of schemes in the first $i$ columns, where the coloring state of the $i$th column is $j$. The state $f[i][j]$ is transferred from $f[i - 1][k]$, where $k$ is the coloring state of the $i - 1$th column, and $k$ and $j$ meet the requirement of different colors being adjacent. That is:

$$
f[i][j] = \sum_{k \in \textit{valid}(j)} f[i - 1][k]
$$

where $\textit{valid}(j)$ represents all legal predecessor states of state $j$.

The final answer is the sum of $f[n][j]$, where $j$ is any legal state.

We notice that $f[i][j]$ is only related to $f[i - 1][k]$, so we can use a rolling array to optimize the space complexity.

The time complexity is $O((m + n) \times 3^{2m})$, and the space complexity is $O(3^m)$. Here, $m$ and $n$ are the number of rows and columns of the grid, respectively.

<!-- tabs:start -->

#### Java

```java
class Solution {
    private int m;

    public int colorTheGrid(int m, int n) {
        this.m = m;
        final int mod = (int) 1e9 + 7;
        int mx = (int) Math.pow(3, m);
        Set<Integer> valid = new HashSet<>();
        int[] f = new int[mx];
        for (int i = 0; i < mx; ++i) {
            if (f1(i)) {
                valid.add(i);
                f[i] = 1;
            }
        }
        Map<Integer, List<Integer>> d = new HashMap<>();
        for (int i : valid) {
            for (int j : valid) {
                if (f2(i, j)) {
                    d.computeIfAbsent(i, k -> new ArrayList<>()).add(j);
                }
            }
        }
        for (int k = 1; k < n; ++k) {
            int[] g = new int[mx];
            for (int i : valid) {
                for (int j : d.getOrDefault(i, List.of())) {
                    g[i] = (g[i] + f[j]) % mod;
                }
            }
            f = g;
        }
        int ans = 0;
        for (int x : f) {
            ans = (ans + x) % mod;
        }
        return ans;
    }

    private boolean f1(int x) {
        int last = -1;
        for (int i = 0; i < m; ++i) {
            if (x % 3 == last) {
                return false;
            }
            last = x % 3;
            x /= 3;
        }
        return true;
    }

    private boolean f2(int x, int y) {
        for (int i = 0; i < m; ++i) {
            if (x % 3 == y % 3) {
                return false;
            }
            x /= 3;
            y /= 3;
        }
        return true;
    }
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
