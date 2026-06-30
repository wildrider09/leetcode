---
comments: true
difficulty: Easy
edit_url: https://github.com/doocs/leetcode/edit/main/lcci/05.07.Exchange/README_EN.md
---

<!-- problem:start -->

# [05.07. Exchange](https://leetcode.cn/problems/exchange-lcci)

[Chinese Version](/lcci/05.07.Exchange/README.md)

## Description

<!-- description:start -->

<p>Write a program to swap odd and even bits in an integer with as few instructions as possible (e.g., bit 0 and bit 1 are swapped, bit 2 and bit 3 are swapped, and so on).</p>

<p><strong>Example1:</strong></p>

<pre>

<strong> Input</strong>: num = 2（0b10）

<strong> Output</strong> 1 (0b01)

</pre>

<p><strong>Example2:</strong></p>

<pre>

<strong> Input</strong>: num = 3

<strong> Output</strong>: 3

</pre>

<p><strong>Note:</strong></p>

<ol>
	<li><code>0 &lt;= num &lt;=</code>&nbsp;2^30 - 1</li>
	<li>The result integer fits into 32-bit integer.</li>
</ol>

<!-- description:end -->

## Solutions

<!-- solution:start -->

### Solution 1: Bit Manipulation

We can perform a bitwise AND operation between `num` and `0x55555555` to get the even bits of `num`, and then shift them one bit to the left. Then, we perform a bitwise AND operation between `num` and `0xaaaaaaaa` to get the odd bits of `num`, and then shift them one bit to the right. Finally, we perform a bitwise OR operation on the two results to get the answer.

The time complexity is $O(1)$, and the space complexity is $O(1)$.

<!-- tabs:start -->

#### Java

```java
class Solution {
    public int exchangeBits(int num) {
        return ((num & 0x55555555) << 1) | ((num & 0xaaaaaaaa)) >> 1;
    }
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
