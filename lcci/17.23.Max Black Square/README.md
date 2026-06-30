---
comments: true
difficulty: Medium
edit_url: https://github.com/doocs/leetcode/edit/main/lcci/17.23.Max%20Black%20Square/README_EN.md
---

<!-- problem:start -->

# [17.23. Max Black Square](https://leetcode.cn/problems/max-black-square-lcci)

[Chinese Version](/lcci/17.23.Max%20Black%20Square/README.md)

## Description

<!-- description:start -->

<p>Imagine you have a square matrix, where each cell (pixel) is either black or white Design an algorithm to find the maximum subsquare such that all four borders are filled with black pixels.</p>
<p>Return an array&nbsp;<code>[r, c, size]</code>, where&nbsp;<code>r</code>,&nbsp;<code>c</code>&nbsp;are the row number and the column number of the subsquare&#39;s upper left corner respectively, and <code>size</code>&nbsp;is the side length of the subsquare. If there are more than one answers, return the one that has smallest <code>r</code>. If there are more than one answers that have the same <code>r</code>, return the one that has smallest <code>c</code>. If there&#39;s no answer, return an empty array.</p>
<p><strong>Example 1:</strong></p>
<pre>

<strong>Input:

</strong>[

&nbsp; [1,0,1],

&nbsp; [<strong>0,0</strong>,1],

&nbsp; [<strong>0,0</strong>,1]

]

<strong>Output: </strong>[1,0,2]

<strong>Explanation:</strong> 0 represents black, and 1 represents white, bold elements in the input is the answer.

</pre>
<p><strong>Example 2:</strong></p>
<pre>

<strong>Input:

</strong>[

&nbsp; [<strong>0</strong>,1,1],

&nbsp; [1,0,1],

&nbsp; [1,1,0]

]

<strong>Output: </strong>[0,0,1]

</pre>
<p><strong>Note: </strong></p>
<ul>
	<li><code>matrix.length == matrix[0].length &lt;= 200</code></li>
</ul>

<!-- description:end -->

## Solutions

<!-- solution:start -->

### Solution 1

<!-- tabs:start -->

#### Java

```java
class Solution {
    public int[] findSquare(int[][] matrix) {
        int n = matrix.length;
        int[][] down = new int[n][n];
        int[][] right = new int[n][n];
        for (int i = n - 1; i >= 0; --i) {
            for (int j = n - 1; j >= 0; --j) {
                if (matrix[i][j] == 0) {
                    down[i][j] = i + 1 < n ? down[i + 1][j] + 1 : 1;
                    right[i][j] = j + 1 < n ? right[i][j + 1] + 1 : 1;
                }
            }
        }
        for (int k = n; k > 0; --k) {
            for (int i = 0; i <= n - k; ++i) {
                for (int j = 0; j <= n - k; ++j) {
                    if (down[i][j] >= k && right[i][j] >= k && right[i + k - 1][j] >= k
                        && down[i][j + k - 1] >= k) {
                        return new int[] {i, j, k};
                    }
                }
            }
        }
        return new int[0];
    }
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
