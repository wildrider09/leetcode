---
comments: true
difficulty: Medium
edit_url: https://github.com/doocs/leetcode/edit/main/lcci/01.08.Zero%20Matrix/README_EN.md
---

<!-- problem:start -->

# [01.08. Zero Matrix](https://leetcode.cn/problems/zero-matrix-lcci)

[Chinese Version](/lcci/01.08.Zero%20Matrix/README.md)

## Description

<!-- description:start -->

<p>Write an algorithm such that if an element in an MxN matrix is 0, its entire row and column are set to 0.</p>

<p>&nbsp;</p>

<p><strong>Example 1: </strong></p>

<pre>

<strong>Input: </strong>

[

  [1,1,1],

  [1,0,1],

  [1,1,1]

]

<strong>Output: </strong>

[

  [1,0,1],

  [0,0,0],

  [1,0,1]

]

</pre>

<p><strong>Example 2: </strong></p>

<pre>

<strong>Input: </strong>

[

  [0,1,2,0],

  [3,4,5,2],

  [1,3,1,5]

]

<strong>Output: </strong>

[

  [0,0,0,0],

  [0,4,5,0],

  [0,3,1,0]

]

</pre>

<!-- description:end -->

## Solutions

<!-- solution:start -->

### Solution 1: Array Marking

We use arrays `rows` and `cols` to mark the rows and columns to be zeroed.

Then we traverse the matrix again, zeroing the elements corresponding to the rows and columns marked in `rows` and `cols`.

The time complexity is $O(m \times n)$, and the space complexity is $O(m + n)$. Here, $m$ and $n$ are the number of rows and columns of the matrix, respectively.

<!-- tabs:start -->

#### Java

```java
class Solution {
    public void setZeroes(int[][] matrix) {
        int m = matrix.length, n = matrix[0].length;
        boolean[] rows = new boolean[m];
        boolean[] cols = new boolean[n];
        for (int i = 0; i < m; ++i) {
            for (int j = 0; j < n; ++j) {
                if (matrix[i][j] == 0) {
                    rows[i] = true;
                    cols[j] = true;
                }
            }
        }
        for (int i = 0; i < m; ++i) {
            for (int j = 0; j < n; ++j) {
                if (rows[i] || cols[j]) {
                    matrix[i][j] = 0;
                }
            }
        }
    }
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- solution:start -->

### Solution 2: In-place Marking

In Solution 1, we used additional arrays to mark the rows and columns to be zeroed. In fact, we can directly use the first row and first column of the matrix for marking, without needing to allocate additional array space.

Since the first row and first column are used for marking, their values may change due to the marking. Therefore, we need additional variables $i0$ and $j0$ to mark whether the first row and first column need to be zeroed.

The time complexity is $O(m \times n)$, where $m$ and $n$ are the number of rows and columns of the matrix, respectively. The space complexity is $O(1)$.

<!-- tabs:start -->

#### Java

```java
class Solution {
    public void setZeroes(int[][] matrix) {
        int m = matrix.length, n = matrix[0].length;
        boolean i0 = false, j0 = false;
        for (int j = 0; j < n; ++j) {
            if (matrix[0][j] == 0) {
                i0 = true;
                break;
            }
        }
        for (int i = 0; i < m; ++i) {
            if (matrix[i][0] == 0) {
                j0 = true;
                break;
            }
        }
        for (int i = 1; i < m; ++i) {
            for (int j = 1; j < n; ++j) {
                if (matrix[i][j] == 0) {
                    matrix[i][0] = 0;
                    matrix[0][j] = 0;
                }
            }
        }
        for (int i = 1; i < m; ++i) {
            for (int j = 1; j < n; ++j) {
                if (matrix[i][0] == 0 || matrix[0][j] == 0) {
                    matrix[i][j] = 0;
                }
            }
        }
        if (i0) {
            for (int j = 0; j < n; ++j) {
                matrix[0][j] = 0;
            }
        }
        if (j0) {
            for (int i = 0; i < m; ++i) {
                matrix[i][0] = 0;
            }
        }
    }
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
