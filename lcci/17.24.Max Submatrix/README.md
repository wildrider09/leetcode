---
comments: true
difficulty: Hard
edit_url: https://github.com/doocs/leetcode/edit/main/lcci/17.24.Max%20Submatrix/README_EN.md
---

<!-- problem:start -->

# [17.24. Max Submatrix](https://leetcode.cn/problems/max-submatrix-lcci)

[Chinese Version](/lcci/17.24.Max%20Submatrix/README.md)

## Description

<!-- description:start -->

<p>Given an NxN matrix of positive and negative integers, write code to find the submatrix with the largest possible sum.</p>

<p>Return an array&nbsp;<code>[r1, c1, r2, c2]</code>, where&nbsp;<code>r1</code>, <code>c1</code> are the row number and the column number of the submatrix&#39;s upper left corner respectively, and&nbsp;<code>r2</code>, <code>c2</code> are the row number of and the column number of lower right corner. If there are more than one answers, return any one of them.</p>

<p><b>Note:&nbsp;</b>This problem is slightly different from the original one in the book.</p>

<p><strong>Example:</strong></p>

<pre>

<strong>Input:

</strong><code>[

&nbsp;  [-1,<strong>0</strong>],

&nbsp;  [0,-1]

]</code>

<strong>Output: </strong>[0,1,0,1]</pre>

<p><strong>Note: </strong></p>

<ul>
	<li><code>1 &lt;= matrix.length, matrix[0].length &lt;= 200</code></li>
</ul>

<!-- description:end -->

## Solutions

<!-- solution:start -->

### Solution 1

<!-- tabs:start -->

#### Java

```java
class Solution {
    public int[] getMaxMatrix(int[][] matrix) {
        int m = matrix.length, n = matrix[0].length;
        int[][] s = new int[m + 1][n];
        for (int i = 0; i < m; ++i) {
            for (int j = 0; j < n; ++j) {
                s[i + 1][j] = s[i][j] + matrix[i][j];
            }
        }
        int mx = matrix[0][0];
        int[] ans = new int[] {0, 0, 0, 0};
        for (int i1 = 0; i1 < m; ++i1) {
            for (int i2 = i1; i2 < m; ++i2) {
                int[] nums = new int[n];
                for (int j = 0; j < n; ++j) {
                    nums[j] = s[i2 + 1][j] - s[i1][j];
                }
                int start = 0;
                int f = nums[0];
                for (int j = 1; j < n; ++j) {
                    if (f > 0) {
                        f += nums[j];
                    } else {
                        f = nums[j];
                        start = j;
                    }
                    if (f > mx) {
                        mx = f;
                        ans = new int[] {i1, start, i2, j};
                    }
                }
            }
        }
        return ans;
    }
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
