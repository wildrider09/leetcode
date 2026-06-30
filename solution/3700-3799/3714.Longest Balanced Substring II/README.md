---
comments: true
difficulty: Medium
edit_url: https://github.com/doocs/leetcode/edit/main/solution/3700-3799/3714.Longest%20Balanced%20Substring%20II/README_EN.md
rating: 2201
source: Weekly Contest 471 Q3
tags:
    - Hash Table
    - String
    - Prefix Sum
---

<!-- problem:start -->

# [3714. Longest Balanced Substring II](https://leetcode.com/problems/longest-balanced-substring-ii)

[Chinese Version](/solution/3700-3799/3714.Longest%20Balanced%20Substring%20II/README.md)

## Description

<!-- description:start -->

<p>You are given a string <code>s</code> consisting only of the characters <code>&#39;a&#39;</code>, <code>&#39;b&#39;</code>, and <code>&#39;c&#39;</code>.</p>

<p>A <strong><span data-keyword="substring-nonempty">substring</span></strong> of <code>s</code> is called <strong>balanced</strong> if all <strong>distinct</strong> characters in the <strong>substring</strong> appear the <strong>same</strong> number of times.</p>

<p>Return the <strong>length of the longest balanced substring</strong> of <code>s</code>.</p>

<p>&nbsp;</p>
<p><strong class="example">Example 1:</strong></p>

<div class="example-block">
<p><strong>Input:</strong> <span class="example-io">s = &quot;abbac&quot;</span></p>

<p><strong>Output:</strong> <span class="example-io">4</span></p>

<p><strong>Explanation:</strong></p>

<p>The longest balanced substring is <code>&quot;abba&quot;</code> because both distinct characters <code>&#39;a&#39;</code> and <code>&#39;b&#39;</code> each appear exactly 2 times.</p>
</div>

<p><strong class="example">Example 2:</strong></p>

<div class="example-block">
<p><strong>Input:</strong> <span class="example-io">s = &quot;aabcc&quot;</span></p>

<p><strong>Output:</strong> <span class="example-io">3</span></p>

<p><strong>Explanation:</strong></p>

<p>The longest balanced substring is <code>&quot;abc&quot;</code> because all distinct characters <code>&#39;a&#39;</code>, <code>&#39;b&#39;</code> and <code>&#39;c&#39;</code> each appear exactly 1 time.</p>
</div>

<p><strong class="example">Example 3:</strong></p>

<div class="example-block">
<p><strong>Input:</strong> <span class="example-io">s = &quot;aba&quot;</span></p>

<p><strong>Output:</strong> <span class="example-io">2</span></p>

<p><strong>Explanation:</strong></p>

<p>One of the longest balanced substrings is <code>&quot;ab&quot;</code> because both distinct characters <code>&#39;a&#39;</code> and <code>&#39;b&#39;</code> each appear exactly 1 time. Another longest balanced substring is <code>&quot;ba&quot;</code>.</p>
</div>

<p>&nbsp;</p>
<p><strong>Constraints:</strong></p>

<ul>
	<li><code>1 &lt;= s.length &lt;= 10<sup>5</sup></code></li>
	<li><code>s</code> contains only the characters <code>&#39;a&#39;</code>, <code>&#39;b&#39;</code>, and <code>&#39;c&#39;</code>.</li>
</ul>

<!-- description:end -->

## Solutions

<!-- solution:start -->

### Solution 1: Enumeration + Prefix Sum + Hash Table

The answer is divided into the following three cases:

1. Balanced substring with only one character, such as `"aaa"`.
2. Balanced substring with two characters, such as `"aabb"`.
3. Balanced substring with three characters, such as `"abc"`.

We define three functions $\text{calc1}(s)$, $\text{calc2}(s, a, b)$, and $\text{calc3}(s)$ to calculate the longest balanced substring length for the above three cases respectively, and finally return the maximum of the three.

For $\text{calc1}(s)$, we only need to traverse the string $s$, count the length of each consecutive character, and take the maximum value.

For $\text{calc2}(s, a, b)$, we can use prefix sum and hash table to calculate the longest balanced substring length. Specifically, we maintain a variable $d$ to represent the number of character $a$ minus the number of character $b$ in the current substring, and use a hash table to record the first occurrence position of each $d$ value. When we encounter the same $d$ value again, it means that the number of character $a$ and character $b$ in the substring from the last occurrence position to the current position are equal, i.e., the substring is balanced, and we update the answer.

For $\text{calc3}(s)$, we also use prefix sum and hash table to calculate the longest balanced substring length. We define an array $\textit{cnt}$ to record the counts of characters $a$, $b$, and $c$, and use a hash table to record the first occurrence position of each $(\textit{cnt}[a] - \textit{cnt}[b], \textit{cnt}[b] - \textit{cnt}[c])$ value. When we encounter the same value again, it means that the counts of characters $a$, $b$, and $c$ in the substring from the last occurrence position to the current position are equal, i.e., the substring is balanced, and we update the answer.

Finally, we calculate the values of $\text{calc1}(s)$, $\text{calc2}(s, 'a', 'b')$, $\text{calc2}(s, 'b', 'c')$, $\text{calc2}(s, 'a', 'c')$, and $\text{calc3}(s)$ respectively, and return their maximum value.

The time complexity is $O(n)$ and the space complexity is $O(n)$, where $n$ is the length of the string $s$.

<!-- tabs:start -->

#### Java

```java
class Solution {
    public int longestBalanced(String s) {
        char[] cs = s.toCharArray();
        int x = calc1(cs);
        int y = Math.max(calc2(cs, 'a', 'b'), Math.max(calc2(cs, 'b', 'c'), calc2(cs, 'a', 'c')));
        int z = calc3(cs);
        return Math.max(x, Math.max(y, z));
    }

    private int calc1(char[] s) {
        int res = 0;
        int i = 0, n = s.length;
        while (i < n) {
            int j = i + 1;
            while (j < n && s[j] == s[i]) {
                j++;
            }
            res = Math.max(res, j - i);
            i = j;
        }
        return res;
    }

    private int calc2(char[] s, char a, char b) {
        int res = 0;
        int i = 0, n = s.length;
        while (i < n) {
            while (i < n && s[i] != a && s[i] != b) {
                i++;
            }
            Map<Integer, Integer> pos = new HashMap<>();
            pos.put(0, i - 1);
            int d = 0;
            while (i < n && (s[i] == a || s[i] == b)) {
                d += (s[i] == a) ? 1 : -1;
                Integer prev = pos.get(d);
                if (prev != null) {
                    res = Math.max(res, i - prev);
                } else {
                    pos.put(d, i);
                }
                i++;
            }
        }
        return res;
    }

    private int calc3(char[] s) {
        Map<Long, Integer> pos = new HashMap<>();
        pos.put(f(0, 0), -1);

        int[] cnt = new int[3];
        int res = 0;

        for (int i = 0; i < s.length; i++) {
            char c = s[i];
            ++cnt[c - 'a'];
            int x = cnt[0] - cnt[1];
            int y = cnt[1] - cnt[2];
            long k = f(x, y);

            Integer prev = pos.get(k);
            if (prev != null) {
                res = Math.max(res, i - prev);
            } else {
                pos.put(k, i);
            }
        }
        return res;
    }

    private long f(int x, int y) {
        return (x + 100000) << 20 | (y + 100000);
    }
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
