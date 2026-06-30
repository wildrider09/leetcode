---
comments: true
difficulty: Hard
edit_url: https://github.com/doocs/leetcode/edit/main/solution/1200-1299/1223.Dice%20Roll%20Simulation/README_EN.md
rating: 2008
source: Weekly Contest 158 Q3
tags:
    - Array
    - Dynamic Programming
---

<!-- problem:start -->

# [1223. Dice Roll Simulation](https://leetcode.com/problems/dice-roll-simulation)

[Chinese Version](/solution/1200-1299/1223.Dice%20Roll%20Simulation/README.md)

## Description

<!-- description:start -->

<p>A die simulator generates a random number from <code>1</code> to <code>6</code> for each roll. You introduced a constraint to the generator such that it cannot roll the number <code>i</code> more than <code>rollMax[i]</code> (<strong>1-indexed</strong>) consecutive times.</p>

<p>Given an array of integers <code>rollMax</code> and an integer <code>n</code>, return <em>the number of distinct sequences that can be obtained with exact </em><code>n</code><em> rolls</em>. Since the answer may be too large, return it <strong>modulo</strong> <code>10<sup>9</sup> + 7</code>.</p>

<p>Two sequences are considered different if at least one element differs from each other.</p>

<p>&nbsp;</p>
<p><strong class="example">Example 1:</strong></p>

<pre>
<strong>Input:</strong> n = 2, rollMax = [1,1,2,2,2,3]
<strong>Output:</strong> 34
<strong>Explanation:</strong> There will be 2 rolls of die, if there are no constraints on the die, there are 6 * 6 = 36 possible combinations. In this case, looking at rollMax array, the numbers 1 and 2 appear at most once consecutively, therefore sequences (1,1) and (2,2) cannot occur, so the final answer is 36-2 = 34.
</pre>

<p><strong class="example">Example 2:</strong></p>

<pre>
<strong>Input:</strong> n = 2, rollMax = [1,1,1,1,1,1]
<strong>Output:</strong> 30
</pre>

<p><strong class="example">Example 3:</strong></p>

<pre>
<strong>Input:</strong> n = 3, rollMax = [1,1,1,2,2,3]
<strong>Output:</strong> 181
</pre>

<p>&nbsp;</p>
<p><strong>Constraints:</strong></p>

<ul>
	<li><code>1 &lt;= n &lt;= 5000</code></li>
	<li><code>rollMax.length == 6</code></li>
	<li><code>1 &lt;= rollMax[i] &lt;= 15</code></li>
</ul>

<!-- description:end -->

## Solutions

<!-- solution:start -->

### Solution 1: Memoization Search

We can design a function $dfs(i, j, x)$ to represent the number of schemes starting from the $i$-th dice roll, with the current dice roll being $j$, and the number of consecutive times $j$ is rolled being $x$. The range of $j$ is $[1, 6]$, and the range of $x$ is $[1, rollMax[j - 1]]$. The answer is $dfs(0, 0, 0)$.

The calculation process of the function $dfs(i, j, x)$ is as follows:

- If $i \ge n$, it means that $n$ dice have been rolled, return $1$.
- Otherwise, we enumerate the number $k$ rolled next time. If $k \ne j$, we can directly roll $k$, and the number of consecutive times $j$ is rolled will be reset to $1$, so the number of schemes is $dfs(i + 1, k, 1)$. If $k = j$, we need to judge whether $x$ is less than $rollMax[j - 1]$. If it is less, we can continue to roll $j$, and the number of consecutive times $j$ is rolled will increase by $1$, so the number of schemes is $dfs(i + 1, j, x + 1)$. Finally, add all the scheme numbers to get the value of $dfs(i, j, x)$. Note that the answer may be very large, so we need to take the modulus of $10^9 + 7$.

During the process, we can use memoization search to avoid repeated calculations.

The time complexity is $O(n \times k^2 \times M)$, and the space complexity is $O(n \times k \times M)$. Here, $k$ is the range of dice points, and $M$ is the maximum number of times a certain point can be rolled consecutively.

<!-- tabs:start -->

#### Java

```java
class Solution {
    private Integer[][][] f;
    private int[] rollMax;

    public int dieSimulator(int n, int[] rollMax) {
        f = new Integer[n][7][16];
        this.rollMax = rollMax;
        return dfs(0, 0, 0);
    }

    private int dfs(int i, int j, int x) {
        if (i >= f.length) {
            return 1;
        }
        if (f[i][j][x] != null) {
            return f[i][j][x];
        }
        long ans = 0;
        for (int k = 1; k <= 6; ++k) {
            if (k != j) {
                ans += dfs(i + 1, k, 1);
            } else if (x < rollMax[j - 1]) {
                ans += dfs(i + 1, j, x + 1);
            }
        }
        ans %= 1000000007;
        return f[i][j][x] = (int) ans;
    }
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- solution:start -->

### Solution 2: Dynamic Programming

We can change the memoization search in Solution 1 to dynamic programming.

Define $f[i][j][x]$ as the number of schemes for the first $i$ dice rolls, with the $i$-th dice roll being $j$, and the number of consecutive times $j$ is rolled being $x$. Initially, $f[1][j][1] = 1$, where $1 \leq j \leq 6$. The answer is:

$$
\sum_{j=1}^6 \sum_{x=1}^{rollMax[j-1]} f[n][j][x]
$$

We enumerate the last dice roll as $j$, and the number of consecutive times $j$ is rolled as $x$. The current dice roll can be $1, 2, \cdots, 6$. If the current dice roll is $k$, there are two cases:

- If $k \neq j$, we can directly roll $k$, and the number of consecutive times $j$ is rolled will be reset to $1$. Therefore, the number of schemes $f[i][k][1]$ will increase by $f[i-1][j][x]$.
- If $k = j$, we need to judge whether $x+1$ is less than or equal to $rollMax[j-1]$. If it is less than or equal to, we can continue to roll $j$, and the number of consecutive times $j$ is rolled will increase by $1$. Therefore, the number of schemes $f[i][j][x+1]$ will increase by $f[i-1][j][x]$.

The final answer is the sum of all $f[n][j][x]$.

The time complexity is $O(n \times k^2 \times M)$, and the space complexity is $O(n \times k \times M)$. Here, $k$ is the range of dice points, and $M$ is the maximum number of times a certain point can be rolled consecutively.

<!-- tabs:start -->

#### Java

```java
class Solution {
    public int dieSimulator(int n, int[] rollMax) {
        int[][][] f = new int[n + 1][7][16];
        for (int j = 1; j <= 6; ++j) {
            f[1][j][1] = 1;
        }
        final int mod = (int) 1e9 + 7;
        for (int i = 2; i <= n; ++i) {
            for (int j = 1; j <= 6; ++j) {
                for (int x = 1; x <= rollMax[j - 1]; ++x) {
                    for (int k = 1; k <= 6; ++k) {
                        if (k != j) {
                            f[i][k][1] = (f[i][k][1] + f[i - 1][j][x]) % mod;
                        } else if (x + 1 <= rollMax[j - 1]) {
                            f[i][j][x + 1] = (f[i][j][x + 1] + f[i - 1][j][x]) % mod;
                        }
                    }
                }
            }
        }
        int ans = 0;
        for (int j = 1; j <= 6; ++j) {
            for (int x = 1; x <= rollMax[j - 1]; ++x) {
                ans = (ans + f[n][j][x]) % mod;
            }
        }
        return ans;
    }
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
