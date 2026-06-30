---
comments: true
difficulty: Medium
edit_url: https://github.com/doocs/leetcode/edit/main/lcci/01.05.One%20Away/README_EN.md
---

<!-- problem:start -->

# [01.05. One Away](https://leetcode.cn/problems/one-away-lcci)

[Chinese Version](/lcci/01.05.One%20Away/README.md)

## Description

<!-- description:start -->

<p>There are three types of edits that can be performed on strings: insert a character, remove a character, or replace a character. Given two strings, write a function to check if they are one edit (or zero edits) away.</p>

<p><strong>Example&nbsp;1:</strong></p>

<pre>

<strong>Input:</strong>

first = &quot;pale&quot;

second = &quot;ple&quot;

<strong>Output:</strong> True

</pre>

<p><strong>Example&nbsp;2:</strong></p>

<pre>

<strong>Input:</strong>

first = &quot;pales&quot;

second = &quot;pal&quot;

<strong>Output:</strong> False

</pre>

<!-- description:end -->

## Solutions

<!-- solution:start -->

### Solution 1: Case Analysis + Two Pointers

Let the lengths of the strings $\textit{first}$ and $\textit{second}$ be $m$ and $n$, respectively. Assume $m \geq n$.

Next, we discuss the following cases:

- When $m - n \gt 1$, $\textit{first}$ and $\textit{second}$ cannot be made equal with one edit, so return `false`;
- When $m = n$, $\textit{first}$ and $\textit{second}$ can be made equal with one edit only if there is exactly one different character;
- When $m - n = 1$, $\textit{first}$ and $\textit{second}$ can be made equal with one edit only if $\textit{second}$ is obtained by deleting one character from $\textit{first}$. We can use two pointers to achieve this.

The time complexity is $O(n)$, where $n$ is the length of the string. The space complexity is $O(1)$.

<!-- tabs:start -->

#### Java

```java
class Solution {
    public boolean oneEditAway(String first, String second) {
        int m = first.length(), n = second.length();
        if (m < n) {
            return oneEditAway(second, first);
        }
        if (m - n > 1) {
            return false;
        }
        int cnt = 0;
        if (m == n) {
            for (int i = 0; i < n; ++i) {
                if (first.charAt(i) != second.charAt(i)) {
                    if (++cnt > 1) {
                        return false;
                    }
                }
            }
            return true;
        }
        for (int i = 0, j = 0; i < m; ++i) {
            if (j == n || (j < n && first.charAt(i) != second.charAt(j))) {
                ++cnt;
            } else {
                ++j;
            }
        }
        return cnt < 2;
    }
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
