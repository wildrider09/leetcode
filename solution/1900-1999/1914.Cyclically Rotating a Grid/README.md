---
comments: true
difficulty: Medium
edit_url: https://github.com/doocs/leetcode/edit/main/solution/1900-1999/1914.Cyclically%20Rotating%20a%20Grid/README_EN.md
rating: 1766
source: Weekly Contest 247 Q2
tags:
    - Array
    - Matrix
    - Simulation
---

<!-- problem:start -->

# [1914. Cyclically Rotating a Grid](https://leetcode.com/problems/cyclically-rotating-a-grid)

[Chinese Version](/solution/1900-1999/1914.Cyclically%20Rotating%20a%20Grid/README.md)

## Description

<!-- description:start -->

<p>You are given an <code>m x n</code> integer matrix <code>grid</code>​​​, where <code>m</code> and <code>n</code> are both <strong>even</strong> integers, and an integer <code>k</code>.</p>

<p>The matrix is composed of several layers, which is shown in the below image, where each color is its own layer:</p>

<p><img alt="" src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/1900-1999/1914.Cyclically%20Rotating%20a%20Grid/images/ringofgrid.png" style="width: 231px; height: 258px;" /></p>

<p>A cyclic rotation of the matrix is done by cyclically rotating <strong>each layer</strong> in the matrix. To cyclically rotate a layer once, each element in the layer will take the place of the adjacent element in the <strong>counter-clockwise</strong> direction. An example rotation is shown below:</p>

<img alt="" src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/1900-1999/1914.Cyclically%20Rotating%20a%20Grid/images/explanation_grid.jpg" style="width: 500px; height: 268px;" />

<p>Return <em>the matrix after applying </em><code>k</code> <em>cyclic rotations to it</em>.</p>

<p>&nbsp;</p>

<p><strong class="example">Example 1:</strong></p>

<img alt="" src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/1900-1999/1914.Cyclically%20Rotating%20a%20Grid/images/rod2.png" style="width: 421px; height: 191px;" />

<pre>

<strong>Input:</strong> grid = [[40,10],[30,20]], k = 1

<strong>Output:</strong> [[10,20],[40,30]]

<strong>Explanation:</strong> The figures above represent the grid at every state.

</pre>

<p><strong class="example">Example 2:</strong></p>

<strong><img alt="" src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/1900-1999/1914.Cyclically%20Rotating%20a%20Grid/images/ringofgrid5.png" style="width: 231px; height: 262px;" /></strong> <strong><img alt="" src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/1900-1999/1914.Cyclically%20Rotating%20a%20Grid/images/ringofgrid6.png" style="width: 231px; height: 262px;" /></strong> <strong><img alt="" src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/1900-1999/1914.Cyclically%20Rotating%20a%20Grid/images/ringofgrid7.png" style="width: 231px; height: 262px;" /></strong>

<pre>

<strong>Input:</strong> grid = [[1,2,3,4],[5,6,7,8],[9,10,11,12],[13,14,15,16]], k = 2

<strong>Output:</strong> [[3,4,8,12],[2,11,10,16],[1,7,6,15],[5,9,13,14]]

<strong>Explanation:</strong> The figures above represent the grid at every state.

</pre>

<p>&nbsp;</p>

<p><strong>Constraints:</strong></p>

<ul>

    <li><code>m == grid.length</code></li>

    <li><code>n == grid[i].length</code></li>

    <li><code>2 &lt;= m, n &lt;= 50</code></li>

    <li>Both <code>m</code> and <code>n</code> are <strong>even</strong> integers.</li>

    <li><code>1 &lt;= grid[i][j] &lt;=<sup> </sup>5000</code></li>

    <li><code>1 &lt;= k &lt;= 10<sup>9</sup></code></li>

</ul>

<!-- description:end -->

## Solutions

<!-- solution:start -->

### Solution 1: Layer-by-Layer Simulation

First, we compute the number of layers in the matrix, denoted by $p$, and then simulate the cyclic rotation layer by layer from the outside to the inside.

For each layer, we traverse clockwise and append the elements on the top, right, bottom, and left edges to an array $nums$ in order. Let the length of $nums$ be $l$. Next, we take $k \bmod l$. Then, starting from index $k$ in the array, we write the elements back to the matrix along the top, right, bottom, and left edges in order.

The time complexity is $O(m \times n)$, and the space complexity is $O(m + n)$, where $m$ and $n$ are the number of rows and columns of the matrix, respectively.

<!-- tabs:start -->

#### Java

```java
class Solution {
    private int m;
    private int n;
    private int[][] grid;

    public int[][] rotateGrid(int[][] grid, int k) {
        m = grid.length;
        n = grid[0].length;
        this.grid = grid;
        for (int p = 0; p < Math.min(m, n) / 2; ++p) {
            rotate(p, k);
        }
        return grid;
    }

    private void rotate(int p, int k) {
        List<Integer> nums = new ArrayList<>();
        for (int j = p; j < n - p - 1; ++j) {
            nums.add(grid[p][j]);
        }
        for (int i = p; i < m - p - 1; ++i) {
            nums.add(grid[i][n - p - 1]);
        }
        for (int j = n - p - 1; j > p; --j) {
            nums.add(grid[m - p - 1][j]);
        }
        for (int i = m - p - 1; i > p; --i) {
            nums.add(grid[i][p]);
        }
        int l = nums.size();
        k %= l;
        if (k == 0) {
            return;
        }
        for (int j = p; j < n - p - 1; ++j) {
            grid[p][j] = nums.get(k++ % l);
        }
        for (int i = p; i < m - p - 1; ++i) {
            grid[i][n - p - 1] = nums.get(k++ % l);
        }
        for (int j = n - p - 1; j > p; --j) {
            grid[m - p - 1][j] = nums.get(k++ % l);
        }
        for (int i = m - p - 1; i > p; --i) {
            grid[i][p] = nums.get(k++ % l);
        }
    }
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
