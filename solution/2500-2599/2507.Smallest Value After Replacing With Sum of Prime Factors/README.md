---
comments: true
difficulty: Medium
edit_url: https://github.com/doocs/leetcode/edit/main/solution/2500-2599/2507.Smallest%20Value%20After%20Replacing%20With%20Sum%20of%20Prime%20Factors/README_EN.md
rating: 1499
source: Weekly Contest 324 Q2
tags:
    - Math
    - Number Theory
    - Simulation
---

<!-- problem:start -->

# [2507. Smallest Value After Replacing With Sum of Prime Factors](https://leetcode.com/problems/smallest-value-after-replacing-with-sum-of-prime-factors)

[Chinese Version](/solution/2500-2599/2507.Smallest%20Value%20After%20Replacing%20With%20Sum%20of%20Prime%20Factors/README.md)

## Description

<!-- description:start -->

<p>You are given a positive integer <code>n</code>.</p>

<p>Continuously replace <code>n</code> with the sum of its <strong>prime factors</strong>.</p>

<ul>
	<li>Note that if a prime factor divides <code>n</code> multiple times, it should be included in the sum as many times as it divides <code>n</code>.</li>
</ul>

<p>Return <em>the smallest value </em><code>n</code><em> will take on.</em></p>

<p>&nbsp;</p>
<p><strong class="example">Example 1:</strong></p>

<pre>
<strong>Input:</strong> n = 15
<strong>Output:</strong> 5
<strong>Explanation:</strong> Initially, n = 15.
15 = 3 * 5, so replace n with 3 + 5 = 8.
8 = 2 * 2 * 2, so replace n with 2 + 2 + 2 = 6.
6 = 2 * 3, so replace n with 2 + 3 = 5.
5 is the smallest value n will take on.
</pre>

<p><strong class="example">Example 2:</strong></p>

<pre>
<strong>Input:</strong> n = 3
<strong>Output:</strong> 3
<strong>Explanation:</strong> Initially, n = 3.
3 is the smallest value n will take on.
</pre>

<p>&nbsp;</p>
<p><strong>Constraints:</strong></p>

<ul>
	<li><code>2 &lt;= n &lt;= 10<sup>5</sup></code></li>
</ul>

<!-- description:end -->

## Solutions

<!-- solution:start -->

### Solution 1: Brute Force Simulation

According to the problem statement, we can perform a process of prime factorization, i.e., continuously decompose a number into its prime factors until it can no longer be decomposed. During the process, add the prime factors each time they are decomposed, and perform this recursively or iteratively.

The time complexity is $O(\sqrt{n})$.

<!-- tabs:start -->

#### Java

```java
class Solution {
    public int smallestValue(int n) {
        while (true) {
            int t = n, s = 0;
            for (int i = 2; i <= n / i; ++i) {
                while (n % i == 0) {
                    s += i;
                    n /= i;
                }
            }
            if (n > 1) {
                s += n;
            }
            if (s == t) {
                return s;
            }
            n = s;
        }
    }
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
