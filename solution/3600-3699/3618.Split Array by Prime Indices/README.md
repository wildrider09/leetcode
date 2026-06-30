---
comments: true
difficulty: Medium
edit_url: https://github.com/doocs/leetcode/edit/main/solution/3600-3699/3618.Split%20Array%20by%20Prime%20Indices/README_EN.md
rating: 1227
source: Biweekly Contest 161 Q1
tags:
    - Array
    - Math
    - Number Theory
---

<!-- problem:start -->

# [3618. Split Array by Prime Indices](https://leetcode.com/problems/split-array-by-prime-indices)

[Chinese Version](/solution/3600-3699/3618.Split%20Array%20by%20Prime%20Indices/README.md)

## Description

<!-- description:start -->

<p>You are given an integer array <code>nums</code>.</p>

<p>Split <code>nums</code> into two arrays <code>A</code> and <code>B</code> using the following rule:</p>

<ul>
	<li>Elements at <strong><span data-keyword="prime-number">prime</span></strong> indices in <code>nums</code> must go into array <code>A</code>.</li>
	<li>All other elements must go into array <code>B</code>.</li>
</ul>

<p>Return the <strong>absolute</strong> difference between the sums of the two arrays: <code>|sum(A) - sum(B)|</code>.</p>

<p><strong>Note:</strong> An empty array has a sum of 0.</p>

<p>&nbsp;</p>
<p><strong class="example">Example 1:</strong></p>

<div class="example-block">
<p><strong>Input:</strong> <span class="example-io">nums = [2,3,4]</span></p>

<p><strong>Output:</strong> <span class="example-io">1</span></p>

<p><strong>Explanation:</strong></p>

<ul>
	<li>The only prime index in the array is 2, so <code>nums[2] = 4</code> is placed in array <code>A</code>.</li>
	<li>The remaining elements, <code>nums[0] = 2</code> and <code>nums[1] = 3</code> are placed in array <code>B</code>.</li>
	<li><code>sum(A) = 4</code>, <code>sum(B) = 2 + 3 = 5</code>.</li>
	<li>The absolute difference is <code>|4 - 5| = 1</code>.</li>
</ul>
</div>

<p><strong class="example">Example 2:</strong></p>

<div class="example-block">
<p><strong>Input:</strong> <span class="example-io">nums = [-1,5,7,0]</span></p>

<p><strong>Output:</strong> <span class="example-io">3</span></p>

<p><strong>Explanation:</strong></p>

<ul>
	<li>The prime indices in the array are 2 and 3, so <code>nums[2] = 7</code> and <code>nums[3] = 0</code> are placed in array <code>A</code>.</li>
	<li>The remaining elements, <code>nums[0] = -1</code> and <code>nums[1] = 5</code> are placed in array <code>B</code>.</li>
	<li><code>sum(A) = 7 + 0 = 7</code>, <code>sum(B) = -1 + 5 = 4</code>.</li>
	<li>The absolute difference is <code>|7 - 4| = 3</code>.</li>
</ul>
</div>

<p>&nbsp;</p>
<p><strong>Constraints:</strong></p>

<ul>
	<li><code>1 &lt;= nums.length &lt;= 10<sup>5</sup></code></li>
	<li><code>-10<sup>9</sup> &lt;= nums[i] &lt;= 10<sup>9</sup></code></li>
</ul>

<!-- description:end -->

## Solutions

<!-- solution:start -->

### Solution 1: Sieve of Eratosthenes + Simulation

We can use the Sieve of Eratosthenes to preprocess all prime numbers in the range $[0, 10^5]$. Then we iterate through the array $\textit{nums}$. For $\textit{nums}[i]$, if $i$ is a prime number, we add $\textit{nums}[i]$ to the answer; otherwise, we add $-\textit{nums}[i]$ to the answer. Finally, we return the absolute value of the answer.

Ignoring the preprocessing time and space, the time complexity is $O(n)$, where $n$ is the length of the array $\textit{nums}$, and the space complexity is $O(1)$.

<!-- tabs:start -->

#### Java

```java
class Solution {
    private static final int M = 100000 + 10;
    private static boolean[] primes = new boolean[M];

    static {
        for (int i = 0; i < M; i++) {
            primes[i] = true;
        }
        primes[0] = primes[1] = false;

        for (int i = 2; i < M; i++) {
            if (primes[i]) {
                for (int j = i + i; j < M; j += i) {
                    primes[j] = false;
                }
            }
        }
    }

    public long splitArray(int[] nums) {
        long ans = 0;
        for (int i = 0; i < nums.length; ++i) {
            ans += primes[i] ? nums[i] : -nums[i];
        }
        return Math.abs(ans);
    }
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
