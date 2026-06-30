---
comments: true
difficulty: Hard
edit_url: https://github.com/doocs/leetcode/edit/main/solution/0400-0499/0471.Encode%20String%20with%20Shortest%20Length/README_EN.md
tags:
    - String
    - Dynamic Programming
---

<!-- problem:start -->

# [471. Encode String with Shortest Length 🔒](https://leetcode.com/problems/encode-string-with-shortest-length)

[Chinese Version](/solution/0400-0499/0471.Encode%20String%20with%20Shortest%20Length/README.md)

## Description

<!-- description:start -->

<p>Given a string <code>s</code>, encode the string such that its encoded length is the shortest.</p>

<p>The encoding rule is: <code>k[encoded_string]</code>, where the <code>encoded_string</code> inside the square brackets is being repeated exactly <code>k</code> times. <code>k</code> should be a positive integer.</p>

<p>If an encoding process does not make the string shorter, then do not encode it. If there are several solutions, return <strong>any of them</strong>.</p>

<p>&nbsp;</p>
<p><strong class="example">Example 1:</strong></p>

<pre>
<strong>Input:</strong> s = &quot;aaa&quot;
<strong>Output:</strong> &quot;aaa&quot;
<strong>Explanation:</strong> There is no way to encode it such that it is shorter than the input string, so we do not encode it.
</pre>

<p><strong class="example">Example 2:</strong></p>

<pre>
<strong>Input:</strong> s = &quot;aaaaa&quot;
<strong>Output:</strong> &quot;5[a]&quot;
<strong>Explanation:</strong> &quot;5[a]&quot; is shorter than &quot;aaaaa&quot; by 1 character.
</pre>

<p><strong class="example">Example 3:</strong></p>

<pre>
<strong>Input:</strong> s = &quot;aaaaaaaaaa&quot;
<strong>Output:</strong> &quot;10[a]&quot;
<strong>Explanation:</strong> &quot;a9[a]&quot; or &quot;9[a]a&quot; are also valid solutions, both of them have the same length = 5, which is the same as &quot;10[a]&quot;.
</pre>

<p>&nbsp;</p>
<p><strong>Constraints:</strong></p>

<ul>
	<li><code>1 &lt;= s.length &lt;= 150</code></li>
	<li><code>s</code> consists of only lowercase English letters.</li>
</ul>

<!-- description:end -->

## Solutions

<!-- solution:start -->

### Solution 1

<!-- tabs:start -->

#### Java

```java
class Solution {
    private String s;
    private String[][] f;

    public String encode(String s) {
        this.s = s;
        int n = s.length();
        f = new String[n][n];
        for (int i = n - 1; i >= 0; --i) {
            for (int j = i; j < n; ++j) {
                f[i][j] = g(i, j);
                if (j - i + 1 > 4) {
                    for (int k = i; k < j; ++k) {
                        String t = f[i][k] + f[k + 1][j];
                        if (f[i][j].length() > t.length()) {
                            f[i][j] = t;
                        }
                    }
                }
            }
        }
        return f[0][n - 1];
    }

    private String g(int i, int j) {
        String t = s.substring(i, j + 1);
        if (t.length() < 5) {
            return t;
        }
        int k = (t + t).indexOf(t, 1);
        if (k < t.length()) {
            int cnt = t.length() / k;
            return String.format("%d[%s]", cnt, f[i][i + k - 1]);
        }
        return t;
    }
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
