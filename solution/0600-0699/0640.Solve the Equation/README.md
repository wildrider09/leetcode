---
comments: true
difficulty: Medium
edit_url: https://github.com/doocs/leetcode/edit/main/solution/0600-0699/0640.Solve%20the%20Equation/README_EN.md
tags:
    - Math
    - String
    - Simulation
---

<!-- problem:start -->

# [640. Solve the Equation](https://leetcode.com/problems/solve-the-equation)

[Chinese Version](/solution/0600-0699/0640.Solve%20the%20Equation/README.md)

## Description

<!-- description:start -->

<p>Solve a given equation and return the value of <code>&#39;x&#39;</code> in the form of a string <code>&quot;x=#value&quot;</code>. The equation contains only <code>&#39;+&#39;</code>, <code>&#39;-&#39;</code> operation, the variable <code>&#39;x&#39;</code> and its coefficient. You should return <code>&quot;No solution&quot;</code> if there is no solution for the equation, or <code>&quot;Infinite solutions&quot;</code> if there are infinite solutions for the equation.</p>

<p>If there is exactly one solution for the equation, we ensure that the value of <code>&#39;x&#39;</code> is an integer.</p>

<p>&nbsp;</p>
<p><strong class="example">Example 1:</strong></p>

<pre>
<strong>Input:</strong> equation = &quot;x+5-3+x=6+x-2&quot;
<strong>Output:</strong> &quot;x=2&quot;
</pre>

<p><strong class="example">Example 2:</strong></p>

<pre>
<strong>Input:</strong> equation = &quot;x=x&quot;
<strong>Output:</strong> &quot;Infinite solutions&quot;
</pre>

<p><strong class="example">Example 3:</strong></p>

<pre>
<strong>Input:</strong> equation = &quot;2x=x&quot;
<strong>Output:</strong> &quot;x=0&quot;
</pre>

<p>&nbsp;</p>
<p><strong>Constraints:</strong></p>

<ul>
	<li><code>3 &lt;= equation.length &lt;= 1000</code></li>
	<li><code>equation</code> has exactly one <code>&#39;=&#39;</code>.</li>
	<li><code>equation</code> consists of integers with an absolute value in the range <code>[0, 100]</code> without any leading zeros, and the variable <code>&#39;x&#39;</code>.</li>
	<li>The input is generated that if there is a single solution, it will be an integer.</li>
</ul>

<!-- description:end -->

## Solutions

<!-- solution:start -->

### Solution 1: Mathematics

We split the $equation$ by the equal sign `"="` into left and right expressions, and compute the coefficient of `"x"` (denoted $x_i$) and the constant value (denoted $y_i$) for each side.

The equation is then transformed into: $x_1 \times x + y_1 = x_2 \times x + y_2$.

- When $x_1 = x_2$: if $y_1 \neq y_2$, there is no solution; if $y_1 = y_2$, there are infinite solutions.
- When $x_1 \neq x_2$: there is a unique solution $x = \frac{y_2 - y_1}{x_1 - x_2}$.

Similar problems:

- [592. Fraction Addition and Subtraction](https://github.com/doocs/leetcode/blob/main/solution/0500-0599/0592.Fraction%20Addition%20and%20Subtraction/README_EN.md)

<!-- tabs:start -->

#### Java

```java
class Solution {
    public String solveEquation(String equation) {
        String[] es = equation.split("=");
        int[] a = f(es[0]), b = f(es[1]);
        int x1 = a[0], y1 = a[1];
        int x2 = b[0], y2 = b[1];
        if (x1 == x2) {
            return y1 == y2 ? "Infinite solutions" : "No solution";
        }
        return "x=" + (y2 - y1) / (x1 - x2);
    }

    private int[] f(String s) {
        int x = 0, y = 0;
        if (s.charAt(0) != '-') {
            s = "+" + s;
        }
        int i = 0, n = s.length();
        while (i < n) {
            int sign = s.charAt(i) == '+' ? 1 : -1;
            ++i;
            int j = i;
            while (j < n && s.charAt(j) != '+' && s.charAt(j) != '-') {
                ++j;
            }
            String v = s.substring(i, j);
            if (s.charAt(j - 1) == 'x') {
                x += sign * (v.length() > 1 ? Integer.parseInt(v.substring(0, v.length() - 1)) : 1);
            } else {
                y += sign * Integer.parseInt(v);
            }
            i = j;
        }
        return new int[] {x, y};
    }
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
