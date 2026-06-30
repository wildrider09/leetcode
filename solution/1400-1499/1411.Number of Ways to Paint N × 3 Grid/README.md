---
comments: true
difficulty: Hard
edit_url: https://github.com/doocs/leetcode/edit/main/solution/1400-1499/1411.Number%20of%20Ways%20to%20Paint%20N%20%C3%97%203%20Grid/README_EN.md
rating: 1844
source: Weekly Contest 184 Q4
tags:
    - Dynamic Programming
---

<!-- problem:start -->

# [1411. Number of Ways to Paint N × 3 Grid](https://leetcode.com/problems/number-of-ways-to-paint-n-3-grid)

[Chinese Version](/solution/1400-1499/1411.Number%20of%20Ways%20to%20Paint%20N%20%C3%97%203%20Grid/README.md)

## Description

<!-- description:start -->

<p>You have a <code>grid</code> of size <code>n x 3</code> and you want to paint each cell of the grid with exactly one of the three colors: <strong>Red</strong>, <strong>Yellow,</strong> or <strong>Green</strong> while making sure that no two adjacent cells have the same color (i.e., no two cells that share vertical or horizontal sides have the same color).</p>

<p>Given <code>n</code> the number of rows of the grid, return <em>the number of ways</em> you can paint this <code>grid</code>. As the answer may grow large, the answer <strong>must be</strong> computed modulo <code>10<sup>9</sup> + 7</code>.</p>

<p>&nbsp;</p>
<p><strong class="example">Example 1:</strong></p>
<img alt="" src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/1400-1499/1411.Number%20of%20Ways%20to%20Paint%20N%20%C3%97%203%20Grid/images/e1.png" style="width: 400px; height: 257px;" />
<pre>
<strong>Input:</strong> n = 1
<strong>Output:</strong> 12
<strong>Explanation:</strong> There are 12 possible way to paint the grid as shown.
</pre>

<p><strong class="example">Example 2:</strong></p>

<pre>
<strong>Input:</strong> n = 5000
<strong>Output:</strong> 30228214
</pre>

<p>&nbsp;</p>
<p><strong>Constraints:</strong></p>

<ul>
	<li><code>n == grid.length</code></li>
	<li><code>1 &lt;= n &lt;= 5000</code></li>
</ul>

<!-- description:end -->

## Solutions

<!-- solution:start -->

### Solution 1: Recursion

We classify all possible states for each row. According to the principle of symmetry, when a row only has $3$ elements, all legal states are classified as: $010$ type, $012$ type.

- When the state is $010$ type: The possible states for the next row are: $101$, $102$, $121$, $201$, $202$. These $5$ states can be summarized as $3$ $010$ types and $2$ $012$ types.
- When the state is $012$ type: The possible states for the next row are: $101$, $120$, $121$, $201$. These $4$ states can be summarized as $2$ $010$ types and $2$ $012$ types.

In summary, we can get: $newf0 = 3 \times f0 + 2 \times f1$, $newf1 = 2 \times f0 + 2 \times f1$.

The time complexity is $O(n)$, where $n$ is the number of rows in the grid. The space complexity is $O(1)$.

<!-- tabs:start -->

#### Java

```java
class Solution {
    public int numOfWays(int n) {
        int mod = (int) 1e9 + 7;
        long f0 = 6, f1 = 6;
        for (int i = 0; i < n - 1; ++i) {
            long g0 = (3 * f0 + 2 * f1) % mod;
            long g1 = (2 * f0 + 2 * f1) % mod;
            f0 = g0;
            f1 = g1;
        }
        return (int) (f0 + f1) % mod;
    }
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- solution:start -->

### Solution 2: State Compression + Dynamic Programming

We notice that the grid only has $3$ columns, so there are at most $3^3=27$ different coloring schemes in a row.

Therefore, we define $f[i][j]$ to represent the number of schemes in the first $i$ rows, where the coloring state of the $i$th row is $j$. The state $f[i][j]$ is transferred from $f[i - 1][k]$, where $k$ is the coloring state of the $i - 1$th row, and $k$ and $j$ meet the requirement of different colors being adjacent. That is:

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
    public int numOfWays(int n) {
        final int mod = (int) 1e9 + 7;
        int m = 27;
        Set<Integer> valid = new HashSet<>();
        int[] f = new int[m];
        for (int i = 0; i < m; ++i) {
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
            int[] g = new int[m];
            for (int i : valid) {
                for (int j : d.getOrDefault(i, List.of())) {
                    g[j] = (g[j] + f[i]) % mod;
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
        for (int i = 0; i < 3; ++i) {
            if (x % 3 == last) {
                return false;
            }
            last = x % 3;
            x /= 3;
        }
        return true;
    }

    private boolean f2(int x, int y) {
        for (int i = 0; i < 3; ++i) {
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
