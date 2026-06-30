---
comments: true
difficulty: Medium
edit_url: https://github.com/doocs/leetcode/edit/main/lcci/08.05.Recursive%20Mulitply/README_EN.md
---

<!-- problem:start -->

# [08.05. Recursive Mulitply](https://leetcode.cn/problems/recursive-mulitply-lcci)

[Chinese Version](/lcci/08.05.Recursive%20Mulitply/README.md)

## Description

<!-- description:start -->

<p>Write a recursive function to multiply two positive integers without using the * operator. You can use addition, subtraction, and bit shifting, but you should minimize the number of those operations.</p>
<p><strong>Example 1:</strong></p>
<pre>

<strong> Input</strong>: A = 1, B = 10

<strong> Output</strong>: 10

</pre>
<p><strong>Example 2:</strong></p>
<pre>

<strong> Input</strong>: A = 3, B = 4

<strong> Output</strong>: 12

</pre>
<p><strong>Note:</strong></p>
<ol>
	<li>The result will not overflow.</li>
</ol>

<!-- description:end -->

## Solutions

<!-- solution:start -->

### Solution 1: Recursion + Bit Manipulation

First, we check if $B$ is $1$. If it is, we directly return $A$.

Otherwise, we check if $B$ is an odd number. If it is, we can right shift $B$ by one bit, then recursively call the function, and finally left shift the result by one bit and add $A$. If not, we can right shift $B$ by one bit, then recursively call the function, and finally left shift the result by one bit.

The time complexity is $O(\log n)$, and the space complexity is $O(\log n)$. Here, $n$ is the size of $B$.

<!-- tabs:start -->

#### Java

```java
class Solution {
    public int multiply(int A, int B) {
        if (B == 1) {
            return A;
        }
        if ((B & 1) == 1) {
            return (multiply(A, B >> 1) << 1) + A;
        }
        return multiply(A, B >> 1) << 1;
    }
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
