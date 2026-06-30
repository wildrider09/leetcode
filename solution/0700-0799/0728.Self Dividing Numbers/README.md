---
comments: true
difficulty: Easy
edit_url: https://github.com/doocs/leetcode/edit/main/solution/0700-0799/0728.Self%20Dividing%20Numbers/README_EN.md
tags:
    - Math
---

<!-- problem:start -->

# [728. Self Dividing Numbers](https://leetcode.com/problems/self-dividing-numbers)

[Chinese Version](/solution/0700-0799/0728.Self%20Dividing%20Numbers/README.md)

## Description

<!-- description:start -->

<p>A <strong>self-dividing number</strong> is a number that is divisible by every digit it contains.</p>

<ul>
	<li>For example, <code>128</code> is <strong>a self-dividing number</strong> because <code>128 % 1 == 0</code>, <code>128 % 2 == 0</code>, and <code>128 % 8 == 0</code>.</li>
</ul>

<p>A <strong>self-dividing number</strong> is not allowed to contain the digit zero.</p>

<p>Given two integers <code>left</code> and <code>right</code>, return <em>a list of all the <strong>self-dividing numbers</strong> in the range</em> <code>[left, right]</code> (both <strong>inclusive</strong>).</p>

<p>&nbsp;</p>
<p><strong class="example">Example 1:</strong></p>
<pre><strong>Input:</strong> left = 1, right = 22
<strong>Output:</strong> [1,2,3,4,5,6,7,8,9,11,12,15,22]
</pre><p><strong class="example">Example 2:</strong></p>
<pre><strong>Input:</strong> left = 47, right = 85
<strong>Output:</strong> [48,55,66,77]
</pre>
<p>&nbsp;</p>
<p><strong>Constraints:</strong></p>

<ul>
	<li><code>1 &lt;= left &lt;= right &lt;= 10<sup>4</sup></code></li>
</ul>

<!-- description:end -->

## Solutions

<!-- solution:start -->

### Solution 1: Simulation

We define a function $\textit{check}(x)$ to determine whether $x$ is a self-dividing number. The implementation idea of the function is as follows:

We use $y$ to record the value of $x$, and then continuously divide $y$ by $10$ until $y$ is $0$. During this process, we check whether the last digit of $y$ is $0$, or whether $x$ cannot be divided by the last digit of $y$. If either of these conditions is met, then $x$ is not a self-dividing number, and we return $\text{false}$. Otherwise, after traversing all the digits, we return $\text{true}$.

Finally, we traverse all the numbers in the interval $[\textit{left}, \textit{right}]$, and for each number, we call $\textit{check}(x)$. If it returns $\text{true}$, we add this number to the answer array.

The time complexity is $O(n \times \log_{10} M)$, where $n$ is the number of elements in the interval $[\textit{left}, \textit{right}]$, and $M = \textit{right}$, which is the maximum value in the interval.

<!-- tabs:start -->

#### Java

```java
class Solution {
    public List<Integer> selfDividingNumbers(int left, int right) {
        List<Integer> ans = new ArrayList<>();
        for (int x = left; x <= right; ++x) {
            if (check(x)) {
                ans.add(x);
            }
        }
        return ans;
    }

    private boolean check(int x) {
        for (int y = x; y > 0; y /= 10) {
            if (y % 10 == 0 || x % (y % 10) != 0) {
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
