---
comments: true
difficulty: Easy
edit_url: https://github.com/doocs/leetcode/edit/main/lcci/01.01.Is%20Unique/README_EN.md
---

<!-- problem:start -->

# [01.01. Is Unique](https://leetcode.cn/problems/is-unique-lcci)

[Chinese Version](/lcci/01.01.Is%20Unique/README.md)

## Description

<!-- description:start -->

<p>Implement an algorithm to determine if a string has all unique characters. What if you cannot use additional data structures?</p>

<p><strong>Example 1:</strong></p>

<pre>

<strong>Input: </strong> = &quot;leetcode&quot;

<strong>Output: </strong>false

</pre>

<p><strong>Example 2:</strong></p>

<pre>

<strong>Input: </strong>s = &quot;abc&quot;

<strong>Output: </strong>true

</pre>

<p><strong>Note:</strong></p>

<ul>
	<li><code>0 &lt;= len(s) &lt;= 100 </code></li>
</ul>

<!-- description:end -->

## Solutions

<!-- solution:start -->

### Solution 1: Bit Manipulation

Based on the examples, we can assume that the string only contains lowercase letters (which is confirmed by actual verification).

Therefore, we can use each bit of a $32$-bit integer `mask` to represent whether each character in the string has appeared.

The time complexity is $O(n)$, where $n$ is the length of the string. The space complexity is $O(1)$.

<!-- tabs:start -->

#### Java

```java
class Solution {
    public boolean isUnique(String astr) {
        int mask = 0;
        for (char c : astr.toCharArray()) {
            int i = c - 'a';
            if (((mask >> i) & 1) == 1) {
                return false;
            }
            mask |= 1 << i;
        }
        return true;
    }
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
