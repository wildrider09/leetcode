---
comments: true
difficulty: Medium
edit_url: https://github.com/doocs/leetcode/edit/main/solution/2100-2199/2168.Unique%20Substrings%20With%20Equal%20Digit%20Frequency/README_EN.md
tags:
    - Hash Table
    - String
    - Counting
    - Hash Function
    - Rolling Hash
---

<!-- problem:start -->

# [2168. Unique Substrings With Equal Digit Frequency 🔒](https://leetcode.com/problems/unique-substrings-with-equal-digit-frequency)

[Chinese Version](/solution/2100-2199/2168.Unique%20Substrings%20With%20Equal%20Digit%20Frequency/README.md)

## Description

<!-- description:start -->

Given a digit string <code>s</code>, return <em>the number of <strong>unique substrings </strong>of </em><code>s</code><em> where every digit appears the same number of times.</em>

<p>&nbsp;</p>
<p><strong class="example">Example 1:</strong></p>

<pre>
<strong>Input:</strong> s = &quot;1212&quot;
<strong>Output:</strong> 5
<strong>Explanation:</strong> The substrings that meet the requirements are &quot;1&quot;, &quot;2&quot;, &quot;12&quot;, &quot;21&quot;, &quot;1212&quot;.
Note that although the substring &quot;12&quot; appears twice, it is only counted once.
</pre>

<p><strong class="example">Example 2:</strong></p>

<pre>
<strong>Input:</strong> s = &quot;12321&quot;
<strong>Output:</strong> 9
<strong>Explanation:</strong> The substrings that meet the requirements are &quot;1&quot;, &quot;2&quot;, &quot;3&quot;, &quot;12&quot;, &quot;23&quot;, &quot;32&quot;, &quot;21&quot;, &quot;123&quot;, &quot;321&quot;.
</pre>

<p>&nbsp;</p>
<p><strong>Constraints:</strong></p>

<ul>
	<li><code>1 &lt;= s.length &lt;= 1000</code></li>
	<li><code>s</code> consists of digits.</li>
</ul>

<!-- description:end -->

## Solutions

<!-- solution:start -->

### Solution 1

<!-- tabs:start -->

#### Java

```java
class Solution {
    public int equalDigitFrequency(String s) {
        int n = s.length();
        int[][] presum = new int[n + 1][10];
        for (int i = 0; i < n; ++i) {
            ++presum[i + 1][s.charAt(i) - '0'];
            for (int j = 0; j < 10; ++j) {
                presum[i + 1][j] += presum[i][j];
            }
        }
        Set<String> vis = new HashSet<>();
        for (int i = 0; i < n; ++i) {
            for (int j = i; j < n; ++j) {
                if (check(i, j, presum)) {
                    vis.add(s.substring(i, j + 1));
                }
            }
        }
        return vis.size();
    }

    private boolean check(int i, int j, int[][] presum) {
        Set<Integer> v = new HashSet<>();
        for (int k = 0; k < 10; ++k) {
            int cnt = presum[j + 1][k] - presum[i][k];
            if (cnt > 0) {
                v.add(cnt);
            }
            if (v.size() > 1) {
                return false;
            }
        }
        return true;
    }
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
