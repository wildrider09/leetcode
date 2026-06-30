---
comments: true
difficulty: Medium
edit_url: https://github.com/doocs/leetcode/edit/main/lcci/16.26.Calculator/README_EN.md
---

<!-- problem:start -->

# [16.26. Calculator](https://leetcode.cn/problems/calculator-lcci)

[Chinese Version](/lcci/16.26.Calculator/README.md)

## Description

<!-- description:start -->

<p>Given an arithmetic equation consisting of positive integers, +, -, * and / (no paren&shy;theses), compute the result.</p>
<p>The expression string contains only non-negative integers, +, -, *, / operators and empty spaces . The integer division should truncate toward zero.</p>
<p><strong>Example&nbsp;1:</strong></p>
<pre>

<strong>Input: </strong>&quot;3+2\*2&quot;

<strong>Output:</strong> 7

</pre>
<p><strong>Example 2:</strong></p>
<pre>

<strong>Input:</strong> &quot; 3/2 &quot;

<strong>Output:</strong> 1</pre>

<p><strong>Example 3:</strong></p>
<pre>

<strong>Input:</strong> &quot; 3+5 / 2 &quot;

<strong>Output:</strong> 5

</pre>
<p><strong>Note:</strong></p>
<ul>
	<li>You may assume that the given expression is always valid.</li>
	<li>Do not use the eval built-in library function.</li>
</ul>

<!-- description:end -->

## Solutions

<!-- solution:start -->

### Solution 1: Stack

We can use a stack to store numbers. Each time we encounter an operator, we push the number into the stack. For addition and subtraction, since their priority is the lowest, we can directly push the numbers into the stack. For multiplication and division, since their priority is higher, we need to take out the top element of the stack, perform multiplication or division with the current number, and then push the result back into the stack.

Finally, the sum of all elements in the stack is the answer.

The time complexity is $O(n)$, and the space complexity is $O(n)$. Where $n$ is the length of the string.

<!-- tabs:start -->

#### Java

```java
class Solution {
    public int calculate(String s) {
        int n = s.length();
        int x = 0;
        char sign = '+';
        Deque<Integer> stk = new ArrayDeque<>();
        for (int i = 0; i < n; ++i) {
            char c = s.charAt(i);
            if (Character.isDigit(c)) {
                x = x * 10 + (c - '0');
            }
            if (i == n - 1 || !Character.isDigit(c) && c != ' ') {
                switch (sign) {
                    case '+' -> stk.push(x);
                    case '-' -> stk.push(-x);
                    case '*' -> stk.push(stk.pop() * x);
                    case '/' -> stk.push(stk.pop() / x);
                }
                x = 0;
                sign = c;
            }
        }
        int ans = 0;
        while (!stk.isEmpty()) {
            ans += stk.pop();
        }
        return ans;
    }
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
