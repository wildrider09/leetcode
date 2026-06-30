---
comments: true
difficulty: Medium
edit_url: https://github.com/doocs/leetcode/edit/main/solution/0300-0399/0311.Sparse%20Matrix%20Multiplication/README_EN.md
tags:
    - Array
    - Hash Table
    - Matrix
---

<!-- problem:start -->

# [311. Sparse Matrix Multiplication 🔒](https://leetcode.com/problems/sparse-matrix-multiplication)

[Chinese Version](/solution/0300-0399/0311.Sparse%20Matrix%20Multiplication/README.md)

## Description

<!-- description:start -->

<p>Given two <a href="https://en.wikipedia.org/wiki/Sparse_matrix" target="_blank">sparse matrices</a> <code>mat1</code> of size <code>m x k</code> and <code>mat2</code> of size <code>k x n</code>, return the result of <code>mat1 x mat2</code>. You may assume that multiplication is always possible.</p>

<p>&nbsp;</p>
<p><strong class="example">Example 1:</strong></p>
<img alt="" src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/0300-0399/0311.Sparse%20Matrix%20Multiplication/images/mult-grid.jpg" style="width: 500px; height: 142px;" />
<pre>
<strong>Input:</strong> mat1 = [[1,0,0],[-1,0,3]], mat2 = [[7,0,0],[0,0,0],[0,0,1]]
<strong>Output:</strong> [[7,0,0],[-7,0,3]]
</pre>

<p><strong class="example">Example 2:</strong></p>

<pre>
<strong>Input:</strong> mat1 = [[0]], mat2 = [[0]]
<strong>Output:</strong> [[0]]
</pre>

<p>&nbsp;</p>
<p><strong>Constraints:</strong></p>

<ul>
	<li><code>m == mat1.length</code></li>
	<li><code>k == mat1[i].length == mat2.length</code></li>
	<li><code>n == mat2[i].length</code></li>
	<li><code>1 &lt;= m, n, k &lt;= 100</code></li>
	<li><code>-100 &lt;= mat1[i][j], mat2[i][j] &lt;= 100</code></li>
</ul>

<!-- description:end -->

## Solutions

<!-- solution:start -->

### Solution 1: Direct Multiplication

We can directly calculate each element in the result matrix according to the definition of matrix multiplication.

The time complexity is $O(m \times n \times k)$, and the space complexity is $O(m \times n)$. Where $m$ and $n$ are the number of rows of matrix $mat1$ and the number of columns of matrix $mat2$ respectively, and $k$ is the number of columns of matrix $mat1$ or the number of rows of matrix $mat2$.

<!-- tabs:start -->

#### Java

```java
class Solution {
    public int[][] multiply(int[][] mat1, int[][] mat2) {
        int m = mat1.length, n = mat2[0].length;
        int[][] ans = new int[m][n];
        for (int i = 0; i < m; ++i) {
            for (int j = 0; j < n; ++j) {
                for (int k = 0; k < mat2.length; ++k) {
                    ans[i][j] += mat1[i][k] * mat2[k][j];
                }
            }
        }
        return ans;
    }
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- solution:start -->

### Solution 2: Preprocessing

We can preprocess the sparse representation of the two matrices, i.e., $g1[i]$ represents the column index and value of all non-zero elements in the $i$th row of matrix $mat1$, and $g2[i]$ represents the column index and value of all non-zero elements in the $i$th row of matrix $mat2$.

Next, we traverse each row $i$, traverse each element $(k, x)$ in $g1[i]$, traverse each element $(j, y)$ in $g2[k]$, then $mat1[i][k] \times mat2[k][j]$ will correspond to $ans[i][j]$ in the result matrix, and we can accumulate all the results.

The time complexity is $O(m \times n \times k)$, and the space complexity is $O(m \times n)$. Where $m$ and $n$ are the number of rows of matrix $mat1$ and the number of columns of matrix $mat2$ respectively, and $k$ is the number of columns of matrix $mat1$ or the number of rows of matrix $mat2$.

<!-- tabs:start -->

#### Java

```java
class Solution {
    public int[][] multiply(int[][] mat1, int[][] mat2) {
        int m = mat1.length, n = mat2[0].length;
        int[][] ans = new int[m][n];
        var g1 = f(mat1);
        var g2 = f(mat2);
        for (int i = 0; i < m; ++i) {
            for (int[] p : g1[i]) {
                int k = p[0], x = p[1];
                for (int[] q : g2[k]) {
                    int j = q[0], y = q[1];
                    ans[i][j] += x * y;
                }
            }
        }
        return ans;
    }

    private List<int[]>[] f(int[][] mat) {
        int m = mat.length, n = mat[0].length;
        List<int[]>[] g = new List[m];
        Arrays.setAll(g, i -> new ArrayList<>());
        for (int i = 0; i < m; ++i) {
            for (int j = 0; j < n; ++j) {
                if (mat[i][j] != 0) {
                    g[i].add(new int[] {j, mat[i][j]});
                }
            }
        }
        return g;
    }
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
