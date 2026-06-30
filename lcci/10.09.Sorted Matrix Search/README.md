---
comments: true
difficulty: Medium
edit_url: https://github.com/doocs/leetcode/edit/main/lcci/10.09.Sorted%20Matrix%20Search/README_EN.md
---

<!-- problem:start -->

# [10.09. Sorted Matrix Search](https://leetcode.cn/problems/sorted-matrix-search-lcci)

[Chinese Version](/lcci/10.09.Sorted%20Matrix%20Search/README.md)

## Description

<!-- description:start -->

<p>Given an M x N matrix in which each row and each column is sorted in ascending order, write a method to find an element.</p>

<p><strong>Example:</strong></p>

<p>Given matrix:</p>

<pre>

[

  [1,   4,  7, 11, 15],

  [2,   5,  8, 12, 19],

  [3,   6,  9, 16, 22],

  [10, 13, 14, 17, 24],

  [18, 21, 23, 26, 30]

]

</pre>

<p>Given target&nbsp;=&nbsp;5,&nbsp;return&nbsp;<code>true.</code></p>

<p>Given target&nbsp;=&nbsp;20, return&nbsp;<code>false.</code></p>

<!-- description:end -->

## Solutions

<!-- solution:start -->

### Solution 1: Binary Search

Since all elements in each row are sorted in ascending order, we can use binary search to find the first element that is greater than or equal to `target` for each row, and then check if this element is equal to `target`. If it equals `target`, it means the target value has been found, and we directly return `true`. If it does not equal `target`, it means all elements in this row are less than `target`, and we should continue to search the next row.

If all rows have been searched and the target value has not been found, it means the target value does not exist, so we return `false`.

The time complexity is $O(m \times \log n)$, where $m$ and $n$ are the number of rows and columns in the matrix, respectively. The space complexity is $O(1)$.

<!-- tabs:start -->

#### Java

```java
class Solution {
    public boolean searchMatrix(int[][] matrix, int target) {
        for (var row : matrix) {
            int j = Arrays.binarySearch(row, target);
            if (j >= 0) {
                return true;
            }
        }
        return false;
    }
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- solution:start -->

### Solution 2: Search from the Bottom Left or Top Right

Here, we start searching from the bottom left corner and move towards the top right direction, comparing the current element `matrix[i][j]` with `target`:

- If $\textit{matrix}[i][j] = \textit{target}$, it means the target value has been found, and we directly return `true`.
- If $\textit{matrix}[i][j] > \textit{target}$, it means all elements in this column from the current position upwards are greater than `target`, so we should move the $i$ pointer upwards, i.e., $i \leftarrow i - 1$.
- If $\textit{matrix}[i][j] < \textit{target}$, it means all elements in this row from the current position to the right are less than `target`, so we should move the $j$ pointer to the right, i.e., $j \leftarrow j + 1$.

If the search ends and the `target` is still not found, return `false`.

The time complexity is $O(m + n)$, where $m$ and $n$ are the number of rows and columns in the matrix, respectively. The space complexity is $O(1)$.

<!-- tabs:start -->

#### Java

```java
class Solution {
    public boolean searchMatrix(int[][] matrix, int target) {
        if (matrix == null || matrix.length == 0) {
            return false;
        }
        int m = matrix.length, n = matrix[0].length;
        int i = m - 1, j = 0;
        while (i >= 0 && j < n) {
            if (matrix[i][j] == target) {
                return true;
            }
            if (matrix[i][j] > target) {
                --i;
            } else {
                ++j;
            }
        }
        return false;
    }
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
