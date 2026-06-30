---
comments: true
difficulty: Medium
edit_url: https://github.com/doocs/leetcode/edit/main/solution/0600-0699/0625.Minimum%20Factorization/README_EN.md
tags:
    - Greedy
    - Math
---

<!-- problem:start -->

# [625. Minimum Factorization 🔒](https://leetcode.com/problems/minimum-factorization)

[Chinese Version](/solution/0600-0699/0625.Minimum%20Factorization/README.md)

## Description

<!-- description:start -->

<p>Given a positive integer num, return <em>the smallest positive integer </em><code>x</code><em> whose multiplication of each digit equals </em><code>num</code>. If there is no answer or the answer is not fit in <strong>32-bit</strong> signed integer, return <code>0</code>.</p>

<p>&nbsp;</p>
<p><strong class="example">Example 1:</strong></p>
<pre><strong>Input:</strong> num = 48
<strong>Output:</strong> 68
</pre><p><strong class="example">Example 2:</strong></p>
<pre><strong>Input:</strong> num = 15
<strong>Output:</strong> 35
</pre>
<p>&nbsp;</p>
<p><strong>Constraints:</strong></p>

<ul>
	<li><code>1 &lt;= num &lt;= 2<sup>31</sup> - 1</code></li>
</ul>

<!-- description:end -->

## Solutions

<!-- solution:start -->

### Solution 1

<!-- tabs:start -->

#### Java

```java
class Solution {
    public int smallestFactorization(int num) {
        if (num < 2) {
            return num;
        }
        long ans = 0, mul = 1;
        for (int i = 9; i >= 2; --i) {
            if (num % i == 0) {
                while (num % i == 0) {
                    num /= i;
                    ans = mul * i + ans;
                    mul *= 10;
                }
            }
        }
        return num < 2 && ans <= Integer.MAX_VALUE ? (int) ans : 0;
    }
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
