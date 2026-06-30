---
comments: true
difficulty: Hard
edit_url: https://github.com/doocs/leetcode/edit/main/solution/2800-2899/2851.String%20Transformation/README_EN.md
rating: 2857
source: Weekly Contest 362 Q4
tags:
    - Math
    - String
    - Dynamic Programming
    - String Matching
---

<!-- problem:start -->

# [2851. String Transformation](https://leetcode.com/problems/string-transformation)

[Chinese Version](/solution/2800-2899/2851.String%20Transformation/README.md)

## Description

<!-- description:start -->

<p>You are given two strings <code>s</code> and <code>t</code> of equal length <code>n</code>. You can perform the following operation on the string <code>s</code>:</p>

<ul>
	<li>Remove a <strong>suffix</strong> of <code>s</code> of length <code>l</code> where <code>0 &lt; l &lt; n</code> and append it at the start of <code>s</code>.<br />
	For example, let <code>s = &#39;abcd&#39;</code> then in one operation you can remove the suffix <code>&#39;cd&#39;</code> and append it in front of <code>s</code> making <code>s = &#39;cdab&#39;</code>.</li>
</ul>

<p>You are also given an integer <code>k</code>. Return <em>the number of ways in which </em><code>s</code> <em>can be transformed into </em><code>t</code><em> in <strong>exactly</strong> </em><code>k</code><em> operations.</em></p>

<p>Since the answer can be large, return it <strong>modulo</strong> <code>10<sup>9</sup> + 7</code>.</p>

<p>&nbsp;</p>
<p><strong class="example">Example 1:</strong></p>

<pre>
<strong>Input:</strong> s = &quot;abcd&quot;, t = &quot;cdab&quot;, k = 2
<strong>Output:</strong> 2
<strong>Explanation:</strong> 
First way:
In first operation, choose suffix from index = 3, so resulting s = &quot;dabc&quot;.
In second operation, choose suffix from index = 3, so resulting s = &quot;cdab&quot;.

Second way:
In first operation, choose suffix from index = 1, so resulting s = &quot;bcda&quot;.
In second operation, choose suffix from index = 1, so resulting s = &quot;cdab&quot;.
</pre>

<p><strong class="example">Example 2:</strong></p>

<pre>
<strong>Input:</strong> s = &quot;ababab&quot;, t = &quot;ababab&quot;, k = 1
<strong>Output:</strong> 2
<strong>Explanation:</strong> 
First way:
Choose suffix from index = 2, so resulting s = &quot;ababab&quot;.

Second way:
Choose suffix from index = 4, so resulting s = &quot;ababab&quot;.
</pre>

<p>&nbsp;</p>
<p><strong>Constraints:</strong></p>

<ul>
	<li><code>2 &lt;= s.length &lt;= 5 * 10<sup>5</sup></code></li>
	<li><code>1 &lt;= k &lt;= 10<sup>15</sup></code></li>
	<li><code>s.length == t.length</code></li>
	<li><code>s</code> and <code>t</code> consist of only lowercase English alphabets.</li>
</ul>

<!-- description:end -->

## Solutions

<!-- solution:start -->

### Solution 1

<!-- tabs:start -->

#### Java

```java
class Solution {
    private static final int M = 1000000007;

    private int add(int x, int y) {
        if ((x += y) >= M) {
            x -= M;
        }
        return x;
    }

    private int mul(long x, long y) {
        return (int) (x * y % M);
    }

    private int[] getZ(String s) {
        int n = s.length();
        int[] z = new int[n];
        for (int i = 1, left = 0, right = 0; i < n; ++i) {
            if (i <= right && z[i - left] <= right - i) {
                z[i] = z[i - left];
            } else {
                int z_i = Math.max(0, right - i + 1);
                while (i + z_i < n && s.charAt(i + z_i) == s.charAt(z_i)) {
                    z_i++;
                }
                z[i] = z_i;
            }
            if (i + z[i] - 1 > right) {
                left = i;
                right = i + z[i] - 1;
            }
        }
        return z;
    }

    private int[][] matrixMultiply(int[][] a, int[][] b) {
        int m = a.length, n = a[0].length, p = b[0].length;
        int[][] r = new int[m][p];
        for (int i = 0; i < m; ++i) {
            for (int j = 0; j < p; ++j) {
                for (int k = 0; k < n; ++k) {
                    r[i][j] = add(r[i][j], mul(a[i][k], b[k][j]));
                }
            }
        }
        return r;
    }

    private int[][] matrixPower(int[][] a, long y) {
        int n = a.length;
        int[][] r = new int[n][n];
        for (int i = 0; i < n; ++i) {
            r[i][i] = 1;
        }
        int[][] x = new int[n][n];
        for (int i = 0; i < n; ++i) {
            System.arraycopy(a[i], 0, x[i], 0, n);
        }
        while (y > 0) {
            if ((y & 1) == 1) {
                r = matrixMultiply(r, x);
            }
            x = matrixMultiply(x, x);
            y >>= 1;
        }
        return r;
    }

    public int numberOfWays(String s, String t, long k) {
        int n = s.length();
        int[] dp = matrixPower(new int[][] {{0, 1}, {n - 1, n - 2}}, k)[0];
        s += t + t;
        int[] z = getZ(s);
        int m = n + n;
        int result = 0;
        for (int i = n; i < m; ++i) {
            if (z[i] >= n) {
                result = add(result, dp[i - n == 0 ? 0 : 1]);
            }
        }
        return result;
    }
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
