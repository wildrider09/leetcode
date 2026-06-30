---
comments: true
difficulty: Easy
edit_url: https://github.com/doocs/leetcode/edit/main/lcci/01.09.String%20Rotation/README_EN.md
---

<!-- problem:start -->

# [01.09. String Rotation](https://leetcode.cn/problems/string-rotation-lcci)

[Chinese Version](/lcci/01.09.String%20Rotation/README.md)

## Description

<!-- description:start -->

<p>Given two strings, <code>s1</code>&nbsp;and <code>s2</code>, write code to check if <code>s2</code> is a rotation of <code>s1</code> (e.g.,&quot;waterbottle&quot; is a rotation of&quot;erbottlewat&quot;).&nbsp;Can you use&nbsp;only one call to the method that&nbsp;checks if one word is a substring of another?</p>

<p><strong>Example 1:</strong></p>

<pre>

<strong>Input: </strong>s1 = &quot;waterbottle&quot;, s2 = &quot;erbottlewat&quot;

<strong>Output: </strong>True

</pre>

<p><strong>Example 2:</strong></p>

<pre>

<strong>Input: </strong>s1 = &quot;aa&quot;, &quot;aba&quot;

<strong>Output: </strong>False

</pre>

<p>&nbsp;</p>

<p><strong>Note:</strong></p>

<ol>
	<li><code><font face="monospace">0 &lt;= s1.length, s1.length &lt;=&nbsp;</font>100000</code></li>
</ol>

<!-- description:end -->

## Solutions

<!-- solution:start -->

### Solution 1: String Matching

First, if the lengths of strings $s1$ and $s2$ are not equal, they are definitely not rotation strings of each other.

Second, if the lengths of strings $s1$ and $s2$ are equal, then by concatenating two $s1$ together, the resulting string $s1 + s1$ will definitely include all rotation cases of $s1$. At this point, we just need to check whether $s2$ is a substring of $s1 + s1$.

```bash
# True
s1 = "aba"
s2 = "baa"
s1 + s1 = "abaaba"
            ^^^

# False
s1 = "aba"
s2 = "bab"
s1 + s1 = "abaaba"
```

The time complexity is $O(n)$, and the space complexity is $O(n)$. Here, $n$ is the length of string $s1$.

<!-- tabs:start -->

#### Java

```java
class Solution {
    public boolean isFlipedString(String s1, String s2) {
        return s1.length() == s2.length() && (s1 + s1).contains(s2);
    }
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
