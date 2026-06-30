---
comments: true
difficulty: Medium
edit_url: https://github.com/doocs/leetcode/edit/main/lcci/16.18.Pattern%20Matching/README_EN.md
---

<!-- problem:start -->

# [16.18. Pattern Matching](https://leetcode.cn/problems/pattern-matching-lcci)

[Chinese Version](/lcci/16.18.Pattern%20Matching/README.md)

## Description

<!-- description:start -->

<p>You are given two strings, pattern and value. The pattern string consists of just the letters a and b, describing a pattern within a string. For example, the string catcatgocatgo matches the pattern aabab (where cat is a and go is b). It also matches patterns like a, ab, and b. Write a method to determine if value matches pattern. a and b cannot be the same string.</p>
<p><strong>Example 1: </strong></p>
<pre>

<strong>Input: </strong> pattern = &quot;abba&quot;, value = &quot;dogcatcatdog&quot;

<strong>Output: </strong> true

</pre>
<p><strong>Example 2: </strong></p>
<pre>

<strong>Input: </strong> pattern = &quot;abba&quot;, value = &quot;dogcatcatfish&quot;

<strong>Output: </strong> false

</pre>
<p><strong>Example 3: </strong></p>
<pre>

<strong>Input: </strong> pattern = &quot;aaaa&quot;, value = &quot;dogcatcatdog&quot;

<strong>Output: </strong> false

</pre>
<p><strong>Example 4: </strong></p>
<pre>

<strong>Input: </strong> pattern = &quot;abba&quot;, value = &quot;dogdogdogdog&quot;

<strong>Output: </strong> true

<strong>Explanation: </strong> &quot;a&quot;=&quot;dogdog&quot;,b=&quot;&quot;，vice versa.

</pre>
<p><strong>Note: </strong></p>
<ul>
	<li><code>0 &lt;= len(pattern) &lt;= 1000</code></li>
	<li><code>0 &lt;= len(value) &lt;= 1000</code></li>
	<li><code>pattern</code>&nbsp;only contains&nbsp;<code>&quot;a&quot;</code>&nbsp;and&nbsp;<code>&quot;b&quot;</code>,&nbsp;<code>value</code> only contains lowercase letters.</li>
</ul>

<!-- description:end -->

## Solutions

<!-- solution:start -->

### Solution 1: Enumeration

We first count the number of characters `'a'` and `'b'` in the pattern string $pattern$, denoted as $cnt[0]$ and $cnt[1]$, respectively. Let the length of the string $value$ be $n$.

If $cnt[0]=0$, it means that the pattern string only contains the character `'b'`. We need to check whether $n$ is a multiple of $cnt[1]$, and whether $value$ can be divided into $cnt[1]$ substrings of length $n/cnt[1]$, and all these substrings are the same. If not, return $false$ directly.

If $cnt[1]=0$, it means that the pattern string only contains the character `'a'`. We need to check whether $n$ is a multiple of $cnt[0]$, and whether $value$ can be divided into $cnt[0]$ substrings of length $n/cnt[0]$, and all these substrings are the same. If not, return $false$ directly.

Next, we denote the length of the string matched by the character `'a'` as $la$, and the length of the string matched by the character `'b'` as $lb$. Then we have $la \times cnt[0] + lb \times cnt[1] = n$. If we enumerate $la$, we can determine the value of $lb$. Therefore, we can enumerate $la$ and check whether there exists an integer $lb$ that satisfies the above equation.

The time complexity is $O(n^2)$, and the space complexity is $O(n)$. Here, $n$ is the length of the string $value$.

<!-- tabs:start -->

#### Java

```java
class Solution {
    private String pattern;
    private String value;

    public boolean patternMatching(String pattern, String value) {
        this.pattern = pattern;
        this.value = value;
        int[] cnt = new int[2];
        for (char c : pattern.toCharArray()) {
            ++cnt[c - 'a'];
        }
        int n = value.length();
        if (cnt[0] == 0) {
            return n % cnt[1] == 0 && value.substring(0, n / cnt[1]).repeat(cnt[1]).equals(value);
        }
        if (cnt[1] == 0) {
            return n % cnt[0] == 0 && value.substring(0, n / cnt[0]).repeat(cnt[0]).equals(value);
        }
        for (int la = 0; la <= n; ++la) {
            if (la * cnt[0] > n) {
                break;
            }
            if ((n - la * cnt[0]) % cnt[1] == 0) {
                int lb = (n - la * cnt[0]) / cnt[1];
                if (check(la, lb)) {
                    return true;
                }
            }
        }
        return false;
    }

    private boolean check(int la, int lb) {
        int i = 0;
        String a = null, b = null;
        for (char c : pattern.toCharArray()) {
            if (c == 'a') {
                if (a != null && !a.equals(value.substring(i, i + la))) {
                    return false;
                }
                a = value.substring(i, i + la);
                i += la;
            } else {
                if (b != null && !b.equals(value.substring(i, i + lb))) {
                    return false;
                }
                b = value.substring(i, i + lb);
                i += lb;
            }
        }
        return !a.equals(b);
    }
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
