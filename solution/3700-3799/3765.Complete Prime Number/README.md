---
comments: true
difficulty: Medium
edit_url: https://github.com/doocs/leetcode/edit/main/solution/3700-3799/3765.Complete%20Prime%20Number/README_EN.md
rating: 1378
source: Biweekly Contest 171 Q1
tags:
    - Math
    - Enumeration
    - Number Theory
---

<!-- problem:start -->

# [3765. Complete Prime Number](https://leetcode.com/problems/complete-prime-number)

[Chinese Version](/solution/3700-3799/3765.Complete%20Prime%20Number/README.md)

## Description

<!-- description:start -->

<p>You are given an integer <code>num</code>.</p>

<p>A number <code>num</code> is called a <strong>Complete <span data-keyword="prime-number">Prime Number</span></strong> if every <strong>prefix</strong> and every <strong>suffix</strong> of <code>num</code> is <strong>prime</strong>.</p>

<p>Return <code>true</code> if <code>num</code> is a Complete Prime Number, otherwise return <code>false</code>.</p>

<p><strong>Note</strong>:</p>

<ul>
	<li>A <strong>prefix</strong> of a number is formed by the <strong>first</strong> <code>k</code> digits of the number.</li>
	<li>A <strong>suffix</strong> of a number is formed by the <strong>last</strong> <code>k</code> digits of the number.</li>
	<li>Single-digit numbers are considered Complete Prime Numbers only if they are <strong>prime</strong>.</li>
</ul>

<p>&nbsp;</p>
<p><strong class="example">Example 1:</strong></p>

<div class="example-block">
<p><strong>Input:</strong> <span class="example-io">num = 23</span></p>

<p><strong>Output:</strong> <span class="example-io">true</span></p>

<p><strong>Explanation:</strong></p>

<ul>
	<li><strong>​​​​​​​</strong>Prefixes of <code>num = 23</code> are 2 and 23, both are prime.</li>
	<li>Suffixes of <code>num = 23</code> are 3 and 23, both are prime.</li>
	<li>All prefixes and suffixes are prime, so 23 is a Complete Prime Number and the answer is <code>true</code>.</li>
</ul>
</div>

<p><strong class="example">Example 2:</strong></p>

<div class="example-block">
<p><strong>Input:</strong> <span class="example-io">num = 39</span></p>

<p><strong>Output:</strong> <span class="example-io">false</span></p>

<p><strong>Explanation:</strong></p>

<ul>
	<li>Prefixes of <code>num = 39</code> are 3 and 39. 3 is prime, but 39 is not prime.</li>
	<li>Suffixes of <code>num = 39</code> are 9 and 39. Both 9 and 39 are not prime.</li>
	<li>At least one prefix or suffix is not prime, so 39 is not a Complete Prime Number and the answer is <code>false</code>.</li>
</ul>
</div>

<p><strong class="example">Example 3:</strong></p>

<div class="example-block">
<p><strong>Input:</strong> <span class="example-io">num = 7</span></p>

<p><strong>Output:</strong> <span class="example-io">true</span></p>

<p><strong>Explanation:</strong></p>

<ul>
	<li>7 is prime, so all its prefixes and suffixes are prime and the answer is <code>true</code>.</li>
</ul>
</div>

<p>&nbsp;</p>
<p><strong>Constraints:</strong></p>

<ul>
	<li><code>1 &lt;= num &lt;= 10<sup>9</sup></code></li>
</ul>

<!-- description:end -->

## Solutions

<!-- solution:start -->

### Solution 1: Mathematics

We define a function $\text{is\_prime}(x)$ to determine whether a number $x$ is prime. Specifically, if $x < 2$, then $x$ is not prime; otherwise, we check all integers $i$ from $2$ to $\sqrt{x}$. If there exists some $i$ that divides $x$, then $x$ is not prime; otherwise, $x$ is prime.

Next, we convert the integer $\textit{num}$ to a string $s$, and sequentially check whether the integer corresponding to each prefix and suffix of $s$ is prime. For prefixes, we construct the integer $x$ from left to right; for suffixes, we construct the integer $x$ from right to left. If during the checking process we find that the integer corresponding to some prefix or suffix is not prime, we return $\text{false}$; if all integers corresponding to prefixes and suffixes are prime, we return $\text{true}$.

The time complexity is $O(\sqrt{n} \times \log n)$, and the space complexity is $O(\log n)$, where $n$ is the value of the integer $\textit{num}$.

<!-- tabs:start -->

#### Java

```java
class Solution {
    public boolean completePrime(int num) {
        char[] s = String.valueOf(num).toCharArray();
        int x = 0;
        for (int i = 0; i < s.length; i++) {
            x = x * 10 + (s[i] - '0');
            if (!isPrime(x)) {
                return false;
            }
        }
        x = 0;
        int p = 1;
        for (int i = s.length - 1; i >= 0; i--) {
            x = p * (s[i] - '0') + x;
            p *= 10;
            if (!isPrime(x)) {
                return false;
            }
        }
        return true;
    }

    private boolean isPrime(int x) {
        if (x < 2) {
            return false;
        }
        for (int i = 2; i * i <= x; i++) {
            if (x % i == 0) {
                return false;
            }
        }
        return true;
    }
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
