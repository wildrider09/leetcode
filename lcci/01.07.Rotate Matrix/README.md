---
comments: true
difficulty: Medium
edit_url: https://github.com/doocs/leetcode/edit/main/lcci/01.07.Rotate%20Matrix/README_EN.md
---

<!-- problem:start -->

# [01.07. Rotate Matrix](https://leetcode.cn/problems/rotate-matrix-lcci)

[Chinese Version](/lcci/01.07.Rotate%20Matrix/README.md)

## Description

<!-- description:start -->

<p>Given an image represented by an N x N matrix, where each pixel in the image is 4 bytes, write a method to rotate the image by 90 degrees. Can you do this in place?</p>

<p>&nbsp;</p>

<p><strong>Example 1:</strong></p>

<pre>

Given <strong>matrix</strong> =

[

  [1,2,3],

  [4,5,6],

  [7,8,9]

],

Rotate the matrix <strong>in place. </strong>It becomes:

[

  [7,4,1],

  [8,5,2],

  [9,6,3]

]

</pre>

<p><strong>Example 2:</strong></p>

<pre>

Given <strong>matrix</strong> =

[

  [ 5, 1, 9,11],

  [ 2, 4, 8,10],

  [13, 3, 6, 7],

  [15,14,12,16]

],

Rotate the matrix <strong>in place. </strong>It becomes:

[

  [15,13, 2, 5],

  [14, 3, 4, 1],

  [12, 6, 8, 9],

  [16, 7,10,11]

]

</pre>

<!-- description:end -->

## Solutions

<!-- solution:start -->

### Solution 1: In-Place Rotation

According to the problem requirements, we need to rotate $\text{matrix}[i][j]$ to $\text{matrix}[j][n - i - 1]$.

We can first flip the matrix upside down, i.e., swap $\text{matrix}[i][j]$ with $\text{matrix}[n - i - 1][j]$, and then flip the matrix along the main diagonal, i.e., swap $\text{matrix}[i][j]$ with $\text{matrix}[j][i]$. This will rotate $\text{matrix}[i][j]$ to $\text{matrix}[j][n - i - 1]$.

The time complexity is $O(n^2)$, where $n$ is the side length of the matrix. The space complexity is $O(1)$.

<!-- tabs:start -->

#### Java

```java
class Solution {
    public void rotate(int[][] matrix) {
        int n = matrix.length;
        for (int i = 0; i < n >> 1; ++i) {
            for (int j = 0; j < n; ++j) {
                int t = matrix[i][j];
                matrix[i][j] = matrix[n - i - 1][j];
                matrix[n - i - 1][j] = t;
            }
        }
        for (int i = 0; i < n; ++i) {
            for (int j = 0; j < i; ++j) {
                int t = matrix[i][j];
                matrix[i][j] = matrix[j][i];
                matrix[j][i] = t;
            }
        }
    }
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
