---
comments: true
difficulty: Medium
edit_url: https://github.com/doocs/leetcode/edit/main/solution/0700-0799/0777.Swap%20Adjacent%20in%20LR%20String/README_EN.md
tags:
    - Two Pointers
    - String
---

<!-- problem:start -->

# [777. Swap Adjacent in LR String](https://leetcode.com/problems/swap-adjacent-in-lr-string)

[Chinese Version](/solution/0700-0799/0777.Swap%20Adjacent%20in%20LR%20String/README.md)

## Description

<!-- description:start -->

<p>In a string composed of <code>&#39;L&#39;</code>, <code>&#39;R&#39;</code>, and <code>&#39;X&#39;</code> characters, like <code>&quot;RXXLRXRXL&quot;</code>, a move consists of either replacing one occurrence of <code>&quot;XL&quot;</code> with <code>&quot;LX&quot;</code>, or replacing one occurrence of <code>&quot;RX&quot;</code> with <code>&quot;XR&quot;</code>. Given the starting string <code>start</code> and the ending string <code>result</code>, return <code>True</code> if and only if there exists a sequence of moves to transform <code>start</code> to <code>result</code>.</p>

<p>&nbsp;</p>
<p><strong class="example">Example 1:</strong></p>

<pre>
<strong>Input:</strong> start = &quot;RXXLRXRXL&quot;, result = &quot;XRLXXRRLX&quot;
<strong>Output:</strong> true
<strong>Explanation:</strong> We can transform start to result following these steps:
RXXLRXRXL -&gt;
XRXLRXRXL -&gt;
XRLXRXRXL -&gt;
XRLXXRRXL -&gt;
XRLXXRRLX
</pre>

<p><strong class="example">Example 2:</strong></p>

<pre>
<strong>Input:</strong> start = &quot;X&quot;, result = &quot;L&quot;
<strong>Output:</strong> false
</pre>

<p>&nbsp;</p>
<p><strong>Constraints:</strong></p>

<ul>
	<li><code>1 &lt;= start.length&nbsp;&lt;= 10<sup>4</sup></code></li>
	<li><code>start.length == result.length</code></li>
	<li>Both <code>start</code> and <code>result</code> will only consist of characters in <code>&#39;L&#39;</code>, <code>&#39;R&#39;</code>, and&nbsp;<code>&#39;X&#39;</code>.</li>
</ul>

<!-- description:end -->

## Solutions

<!-- solution:start -->

### Solution 1

<!-- tabs:start -->

#### Java

```java
class Solution {
    public boolean canTransform(String start, String end) {
        int n = start.length();
        int i = 0, j = 0;
        while (true) {
            while (i < n && start.charAt(i) == 'X') {
                ++i;
            }
            while (j < n && end.charAt(j) == 'X') {
                ++j;
            }
            if (i == n && j == n) {
                return true;
            }
            if (i == n || j == n || start.charAt(i) != end.charAt(j)) {
                return false;
            }
            if (start.charAt(i) == 'L' && i < j || start.charAt(i) == 'R' && i > j) {
                return false;
            }
            ++i;
            ++j;
        }
    }
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
