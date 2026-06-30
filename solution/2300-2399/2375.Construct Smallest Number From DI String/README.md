---
comments: true
difficulty: Medium
edit_url: https://github.com/doocs/leetcode/edit/main/solution/2300-2399/2375.Construct%20Smallest%20Number%20From%20DI%20String/README_EN.md
rating: 1641
source: Weekly Contest 306 Q3
tags:
    - Stack
    - Greedy
    - String
    - Backtracking
---

<!-- problem:start -->

# [2375. Construct Smallest Number From DI String](https://leetcode.com/problems/construct-smallest-number-from-di-string)

[Chinese Version](/solution/2300-2399/2375.Construct%20Smallest%20Number%20From%20DI%20String/README.md)

## Description

<!-- description:start -->

<p>You are given a <strong>0-indexed</strong> string <code>pattern</code> of length <code>n</code> consisting of the characters <code>&#39;I&#39;</code> meaning <strong>increasing</strong> and <code>&#39;D&#39;</code> meaning <strong>decreasing</strong>.</p>

<p>A <strong>0-indexed</strong> string <code>num</code> of length <code>n + 1</code> is created using the following conditions:</p>

<ul>
	<li><code>num</code> consists of the digits <code>&#39;1&#39;</code> to <code>&#39;9&#39;</code>, where each digit is used <strong>at most</strong> once.</li>
	<li>If <code>pattern[i] == &#39;I&#39;</code>, then <code>num[i] &lt; num[i + 1]</code>.</li>
	<li>If <code>pattern[i] == &#39;D&#39;</code>, then <code>num[i] &gt; num[i + 1]</code>.</li>
</ul>

<p>Return <em>the lexicographically <strong>smallest</strong> possible string </em><code>num</code><em> that meets the conditions.</em></p>

<p>&nbsp;</p>
<p><strong class="example">Example 1:</strong></p>

<pre>
<strong>Input:</strong> pattern = &quot;IIIDIDDD&quot;
<strong>Output:</strong> &quot;123549876&quot;
<strong>Explanation:
</strong>At indices 0, 1, 2, and 4 we must have that num[i] &lt; num[i+1].
At indices 3, 5, 6, and 7 we must have that num[i] &gt; num[i+1].
Some possible values of num are &quot;245639871&quot;, &quot;135749862&quot;, and &quot;123849765&quot;.
It can be proven that &quot;123549876&quot; is the smallest possible num that meets the conditions.
Note that &quot;123414321&quot; is not possible because the digit &#39;1&#39; is used more than once.</pre>

<p><strong class="example">Example 2:</strong></p>

<pre>
<strong>Input:</strong> pattern = &quot;DDD&quot;
<strong>Output:</strong> &quot;4321&quot;
<strong>Explanation:</strong>
Some possible values of num are &quot;9876&quot;, &quot;7321&quot;, and &quot;8742&quot;.
It can be proven that &quot;4321&quot; is the smallest possible num that meets the conditions.
</pre>

<p>&nbsp;</p>
<p><strong>Constraints:</strong></p>

<ul>
	<li><code>1 &lt;= pattern.length &lt;= 8</code></li>
	<li><code>pattern</code> consists of only the letters <code>&#39;I&#39;</code> and <code>&#39;D&#39;</code>.</li>
</ul>

<!-- description:end -->

## Solutions

<!-- solution:start -->

### Solution 1

<!-- tabs:start -->

#### Java

```java
class Solution {
    private boolean[] vis = new boolean[10];
    private StringBuilder t = new StringBuilder();
    private String p;
    private String ans;

    public String smallestNumber(String pattern) {
        p = pattern;
        dfs(0);
        return ans;
    }

    private void dfs(int u) {
        if (ans != null) {
            return;
        }
        if (u == p.length() + 1) {
            ans = t.toString();
            return;
        }
        for (int i = 1; i < 10; ++i) {
            if (!vis[i]) {
                if (u > 0 && p.charAt(u - 1) == 'I' && t.charAt(u - 1) - '0' >= i) {
                    continue;
                }
                if (u > 0 && p.charAt(u - 1) == 'D' && t.charAt(u - 1) - '0' <= i) {
                    continue;
                }
                vis[i] = true;
                t.append(i);
                dfs(u + 1);
                t.deleteCharAt(t.length() - 1);
                vis[i] = false;
            }
        }
    }
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
