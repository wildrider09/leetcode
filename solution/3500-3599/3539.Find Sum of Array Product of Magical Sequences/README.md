---
comments: true
difficulty: Hard
edit_url: https://github.com/doocs/leetcode/edit/main/solution/3500-3599/3539.Find%20Sum%20of%20Array%20Product%20of%20Magical%20Sequences/README_EN.md
rating: 2693
source: Weekly Contest 448 Q4
tags:
    - Bit Manipulation
    - Array
    - Math
    - Dynamic Programming
    - Bitmask
    - Combinatorics
---

<!-- problem:start -->

# [3539. Find Sum of Array Product of Magical Sequences](https://leetcode.com/problems/find-sum-of-array-product-of-magical-sequences)

[Chinese Version](/solution/3500-3599/3539.Find%20Sum%20of%20Array%20Product%20of%20Magical%20Sequences/README.md)

## Description

<!-- description:start -->

<p>You are given two integers, <code>m</code> and <code>k</code>, and an integer array <code>nums</code>.</p>
A sequence of integers <code>seq</code> is called <strong>magical</strong> if:

<ul>
	<li><code>seq</code> has a size of <code>m</code>.</li>
	<li><code>0 &lt;= seq[i] &lt; nums.length</code></li>
	<li>The <strong>binary representation</strong> of <code>2<sup>seq[0]</sup> + 2<sup>seq[1]</sup> + ... + 2<sup>seq[m - 1]</sup></code> has <code>k</code> <strong>set bits</strong>.</li>
</ul>

<p>The <strong>array product</strong> of this sequence is defined as <code>prod(seq) = (nums[seq[0]] * nums[seq[1]] * ... * nums[seq[m - 1]])</code>.</p>

<p>Return the <strong>sum</strong> of the <strong>array products</strong> for all valid <strong>magical</strong> sequences.</p>

<p>Since the answer may be large, return it <strong>modulo</strong> <code>10<sup>9</sup> + 7</code>.</p>

<p>A <strong>set bit</strong> refers to a bit in the binary representation of a number that has a value of 1.</p>

<p>&nbsp;</p>
<p><strong class="example">Example 1:</strong></p>

<div class="example-block">
<p><strong>Input:</strong> <span class="example-io">m = 5, k = 5, nums = [1,10,100,10000,1000000]</span></p>

<p><strong>Output:</strong> <span class="example-io">991600007</span></p>

<p><strong>Explanation:</strong></p>

<p>All permutations of <code>[0, 1, 2, 3, 4]</code> are magical sequences, each with an array product of 10<sup>13</sup>.</p>
</div>

<p><strong class="example">Example 2:</strong></p>

<div class="example-block">
<p><strong>Input:</strong> <span class="example-io">m = 2, k = 2, nums = [5,4,3,2,1]</span></p>

<p><strong>Output:</strong> <span class="example-io">170</span></p>

<p><strong>Explanation:</strong></p>

<p>The magical sequences are <code>[0, 1]</code>, <code>[0, 2]</code>, <code>[0, 3]</code>, <code>[0, 4]</code>, <code>[1, 0]</code>, <code>[1, 2]</code>, <code>[1, 3]</code>, <code>[1, 4]</code>, <code>[2, 0]</code>, <code>[2, 1]</code>, <code>[2, 3]</code>, <code>[2, 4]</code>, <code>[3, 0]</code>, <code>[3, 1]</code>, <code>[3, 2]</code>, <code>[3, 4]</code>, <code>[4, 0]</code>, <code>[4, 1]</code>, <code>[4, 2]</code>, and <code>[4, 3]</code>.</p>
</div>

<p><strong class="example">Example 3:</strong></p>

<div class="example-block">
<p><strong>Input:</strong> <span class="example-io">m = 1, k = 1, nums = [28]</span></p>

<p><strong>Output:</strong> <span class="example-io">28</span></p>

<p><strong>Explanation:</strong></p>

<p>The only magical sequence is <code>[0]</code>.</p>
</div>

<p>&nbsp;</p>
<p><strong>Constraints:</strong></p>

<ul>
	<li><code>1 &lt;= k &lt;= m &lt;= 30</code></li>
	<li><code>1 &lt;= nums.length &lt;= 50</code></li>
	<li><code>1 &lt;= nums[i] &lt;= 10<sup>8</sup></code></li>
</ul>

<!-- description:end -->

## Solutions

<!-- solution:start -->

### Solution 1: Combinatorics + Memoized Search

We design a function $\text{dfs}(i, j, k, st)$, which represents the number of ways when we are currently processing the $i$-th element of array $\textit{nums}$, still need to select numbers from the remaining $j$ positions to fill into the magical sequence, still need to satisfy having $k$ set bits in binary form, and the current carry from the previous bit is $st$. Then the answer is $\text{dfs}(0, m, k, 0)$.

The execution process of function $\text{dfs}(i, j, k, st)$ is as follows:

If $k < 0$ or $i = n$ and $j > 0$, it means the current solution is not feasible, return $0$.

If $i = n$, it means we have finished processing array $\textit{nums}$. We need to check if there are still set bits in the current carry $st$, and if so, we need to decrease $k$. If $k = 0$ at this point, it means the current solution is feasible, return $1$, otherwise return $0$.

Otherwise, we enumerate selecting $t$ numbers at position $i$ to fill into the magical sequence ($0 \leq t \leq j$). The number of ways to fill $t$ numbers into the magical sequence is $\binom{j}{t}$, the array product is $\textit{nums}[i]^t$, the updated carry is $(t + st) >> 1$, the updated number of required set bits is $k - ((t + st) \& 1)$, and we recursively call $\text{dfs}(i + 1, j - t, k - ((t + st) \& 1), (t + st) >> 1)$. The sum of all $t$ solutions is $\text{dfs}(i, j, k, st)$.

To efficiently compute the binomial coefficient $\binom{m}{n}$, we preprocess the factorial array $f$ and the inverse factorial array $g$, where $f[i] = i! \mod (10^9 + 7)$ and $g[i] = (i!)^{-1} \mod (10^9 + 7)$. Then $\binom{m}{n} = f[m] \cdot g[n] \cdot g[m - n] \mod (10^9 + 7)$.

The time complexity is $O(n \cdot m^3 \cdot k)$ and the space complexity is $O(n \cdot m^2 \cdot k)$, where $n$ is the length of array $\textit{nums}$, and $m$ and $k$ are the parameters in the problem.

<!-- tabs:start -->

#### Java

```java
class Solution {
    static final int N = 31;
    static final long MOD = 1_000_000_007L;
    private static final long[] f = new long[N];
    private static final long[] g = new long[N];
    private Long[][][][] dp;

    static {
        f[0] = 1;
        g[0] = 1;
        for (int i = 1; i < N; ++i) {
            f[i] = f[i - 1] * i % MOD;
            g[i] = qpow(f[i], MOD - 2);
        }
    }

    public static long qpow(long a, long k) {
        long res = 1;
        while (k != 0) {
            if ((k & 1) == 1) {
                res = res * a % MOD;
            }
            a = a * a % MOD;
            k >>= 1;
        }
        return res;
    }

    public static long comb(int m, int n) {
        return f[m] * g[n] % MOD * g[m - n] % MOD;
    }

    public int magicalSum(int m, int k, int[] nums) {
        int n = nums.length;
        dp = new Long[n + 1][m + 1][k + 1][N];
        long ans = dfs(0, m, k, 0, nums);
        return (int) ans;
    }

    private long dfs(int i, int j, int k, int st, int[] nums) {
        if (k < 0 || (i == nums.length && j > 0)) {
            return 0;
        }
        if (i == nums.length) {
            while (st > 0) {
                k -= (st & 1);
                st >>= 1;
            }
            return k == 0 ? 1 : 0;
        }

        if (dp[i][j][k][st] != null) {
            return dp[i][j][k][st];
        }

        long res = 0;
        for (int t = 0; t <= j; t++) {
            int nt = t + st;
            int nk = k - (nt & 1);
            long p = qpow(nums[i], t);
            long tmp = comb(j, t) * p % MOD * dfs(i + 1, j - t, nk, nt >> 1, nums) % MOD;
            res = (res + tmp) % MOD;
        }

        return dp[i][j][k][st] = res;
    }
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
