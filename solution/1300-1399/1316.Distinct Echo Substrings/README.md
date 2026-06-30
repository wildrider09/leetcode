---
comments: true
difficulty: Hard
edit_url: https://github.com/doocs/leetcode/edit/main/solution/1300-1399/1316.Distinct%20Echo%20Substrings/README_EN.md
rating: 1836
source: Biweekly Contest 17 Q4
tags:
    - Trie
    - String
    - Hash Function
    - Rolling Hash
---

<!-- problem:start -->

# [1316. Distinct Echo Substrings](https://leetcode.com/problems/distinct-echo-substrings)

[Chinese Version](/solution/1300-1399/1316.Distinct%20Echo%20Substrings/README.md)

## Description

<!-- description:start -->

<p>Return the number of <strong>distinct</strong> non-empty substrings of <code>text</code>&nbsp;that can be written as the concatenation of some string with itself (i.e. it can be written as <code>a + a</code>&nbsp;where <code>a</code> is some string).</p>

<p>&nbsp;</p>
<p><strong class="example">Example 1:</strong></p>

<pre>
<strong>Input:</strong> text = &quot;abcabcabc&quot;
<strong>Output:</strong> 3
<b>Explanation: </b>The 3 substrings are &quot;abcabc&quot;, &quot;bcabca&quot; and &quot;cabcab&quot;.
</pre>

<p><strong class="example">Example 2:</strong></p>

<pre>
<strong>Input:</strong> text = &quot;leetcodeleetcode&quot;
<strong>Output:</strong> 2
<b>Explanation: </b>The 2 substrings are &quot;ee&quot; and &quot;leetcodeleetcode&quot;.
</pre>

<p>&nbsp;</p>
<p><strong>Constraints:</strong></p>

<ul>
	<li><code>1 &lt;= text.length &lt;= 2000</code></li>
	<li><code>text</code>&nbsp;has only lowercase English letters.</li>
</ul>

<!-- description:end -->

## Solutions

<!-- solution:start -->

### Solution 1

<!-- tabs:start -->

#### Java

```java
class Solution {
    private long[] h;
    private long[] p;

    public int distinctEchoSubstrings(String text) {
        int n = text.length();
        int base = 131;
        h = new long[n + 10];
        p = new long[n + 10];
        p[0] = 1;
        for (int i = 0; i < n; ++i) {
            int t = text.charAt(i) - 'a' + 1;
            h[i + 1] = h[i] * base + t;
            p[i + 1] = p[i] * base;
        }
        Set<Long> vis = new HashSet<>();
        for (int i = 0; i < n - 1; ++i) {
            for (int j = i + 1; j < n; j += 2) {
                int k = (i + j) >> 1;
                long a = get(i + 1, k + 1);
                long b = get(k + 2, j + 1);
                if (a == b) {
                    vis.add(a);
                }
            }
        }
        return vis.size();
    }

    private long get(int i, int j) {
        return h[j] - h[i - 1] * p[j - i + 1];
    }
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
