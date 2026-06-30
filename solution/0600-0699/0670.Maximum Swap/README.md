---
comments: true
difficulty: Medium
edit_url: https://github.com/doocs/leetcode/edit/main/solution/0600-0699/0670.Maximum%20Swap/README_EN.md
tags:
    - Greedy
    - Math
---

<!-- problem:start -->

# [670. Maximum Swap](https://leetcode.com/problems/maximum-swap)

[Chinese Version](/solution/0600-0699/0670.Maximum%20Swap/README.md)

## Description

<!-- description:start -->

<p>You are given an integer <code>num</code>. You can swap two digits at most once to get the maximum valued number.</p>

<p>Return <em>the maximum valued number you can get</em>.</p>

<p>&nbsp;</p>
<p><strong class="example">Example 1:</strong></p>

<pre>
<strong>Input:</strong> num = 2736
<strong>Output:</strong> 7236
<strong>Explanation:</strong> Swap the number 2 and the number 7.
</pre>

<p><strong class="example">Example 2:</strong></p>

<pre>
<strong>Input:</strong> num = 9973
<strong>Output:</strong> 9973
<strong>Explanation:</strong> No swap.
</pre>

<p>&nbsp;</p>
<p><strong>Constraints:</strong></p>

<ul>
	<li><code>0 &lt;= num &lt;= 10<sup>8</sup></code></li>
</ul>

<!-- description:end -->

## Solutions

<!-- solution:start -->

### Solution 1: Greedy Algorithm

First, we convert the number into a string $s$. Then, we traverse the string $s$ from right to left, using an array or hash table $d$ to record the position of the maximum number to the right of each number (it can be the position of the number itself).

Next, we traverse $d$ from left to right. If $s[i] < s[d[i]]$, we swap them and exit the traversal process.

Finally, we convert the string $s$ back into a number, which is the answer.

The time complexity is $O(\log M)$, and the space complexity is $O(\log M)$. Here, $M$ is the range of the number $num$.

<!-- tabs:start -->

#### Java

```java
class Solution {
    public int maximumSwap(int num) {
        char[] s = String.valueOf(num).toCharArray();
        int n = s.length;
        int[] d = new int[n];
        for (int i = 0; i < n; ++i) {
            d[i] = i;
        }
        for (int i = n - 2; i >= 0; --i) {
            if (s[i] <= s[d[i + 1]]) {
                d[i] = d[i + 1];
            }
        }
        for (int i = 0; i < n; ++i) {
            int j = d[i];
            if (s[i] < s[j]) {
                char t = s[i];
                s[i] = s[j];
                s[j] = t;
                break;
            }
        }
        return Integer.parseInt(String.valueOf(s));
    }
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- solution:start -->

### Solution 2: Space Optimized Greedy

<!-- solution:end -->

<!-- problem:end -->
