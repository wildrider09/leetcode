---
comments: true
difficulty: Easy
edit_url: https://github.com/doocs/leetcode/edit/main/lcci/08.10.Color%20Fill/README_EN.md
---

<!-- problem:start -->

# [08.10. Color Fill](https://leetcode.cn/problems/color-fill-lcci)

[Chinese Version](/lcci/08.10.Color%20Fill/README.md)

## Description

<!-- description:start -->

<p>Implement the &quot;paint fill&quot; function that one might see on many image editing programs. That is, given a screen (represented by a two-dimensional array of colors), a point, and a new color, fill in the surrounding area until the color changes from the original color.</p>

<p><strong>Example1:</strong></p>

<pre>

<strong>Input</strong>: 

image = [[1,1,1],[1,1,0],[1,0,1]] 

sr = 1, sc = 1, newColor = 2

<strong>Output</strong>: [[2,2,2],[2,2,0],[2,0,1]]

<strong>Explanation</strong>: 

From the center of the image (with position (sr, sc) = (1, 1)), all pixels connected 

by a path of the same color as the starting pixel are colored with the new color.

Note the bottom corner is not colored 2, because it is not 4-directionally connected

to the starting pixel.</pre>

<p><b>Note:</b></p>

<ul>
	<li>The length of&nbsp;<code>image</code>&nbsp;and&nbsp;<code>image[0]</code>&nbsp;will be in the range&nbsp;<code>[1, 50]</code>.</li>
	<li>The given starting pixel will satisfy&nbsp;<code>0 &lt;= sr &lt; image.length</code>&nbsp;and&nbsp;<code>0 &lt;= sc &lt; image[0].length</code>.</li>
	<li>The value of each color in&nbsp;<code>image[i][j]</code>&nbsp;and&nbsp;<code>newColor</code>&nbsp;will be an integer in&nbsp;<code>[0, 65535]</code>.</li>
</ul>

<!-- description:end -->

## Solutions

<!-- solution:start -->

### Solution 1: DFS

We design a function $dfs(i, j)$ to start filling color from $(i, j)$. If $(i, j)$ is not within the image range, or the color of $(i, j)$ is not the original color, or the color of $(i, j)$ has been filled with the new color, then return. Otherwise, fill the color of $(i, j)$ with the new color, and then recursively search the four directions: up, down, left, and right of $(i, j)$.

The time complexity is $O(m \times n)$, and the space complexity is $O(m \times n)$. Where $m$ and $n$ are the number of rows and columns in the image, respectively.

<!-- tabs:start -->

#### Java

```java
class Solution {
    private int[] dirs = {-1, 0, 1, 0, -1};
    private int[][] image;
    private int nc;
    private int oc;

    public int[][] floodFill(int[][] image, int sr, int sc, int newColor) {
        nc = newColor;
        oc = image[sr][sc];
        this.image = image;
        dfs(sr, sc);
        return image;
    }

    private void dfs(int i, int j) {
        if (i < 0 || i >= image.length || j < 0 || j >= image[0].length || image[i][j] != oc
            || image[i][j] == nc) {
            return;
        }
        image[i][j] = nc;
        for (int k = 0; k < 4; ++k) {
            dfs(i + dirs[k], j + dirs[k + 1]);
        }
    }
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- solution:start -->

### Solution 2: BFS

We can use the method of breadth-first search. Starting from the initial point, fill the color of the initial point with the new color, and then add the initial point to the queue. Each time a point is taken from the queue, the points in the four directions: up, down, left, and right are added to the queue, until the queue is empty.

The time complexity is $O(m \times n)$, and the space complexity is $O(m \times n)$. Where $m$ and $n$ are the number of rows and columns in the image, respectively.

<!-- tabs:start -->

#### Java

```java
class Solution {
    public int[][] floodFill(int[][] image, int sr, int sc, int newColor) {
        if (image[sr][sc] == newColor) {
            return image;
        }
        Deque<int[]> q = new ArrayDeque<>();
        q.offer(new int[] {sr, sc});
        int oc = image[sr][sc];
        image[sr][sc] = newColor;
        int[] dirs = {-1, 0, 1, 0, -1};
        while (!q.isEmpty()) {
            int[] p = q.poll();
            int i = p[0], j = p[1];
            for (int k = 0; k < 4; ++k) {
                int x = i + dirs[k], y = j + dirs[k + 1];
                if (x >= 0 && x < image.length && y >= 0 && y < image[0].length
                    && image[x][y] == oc) {
                    q.offer(new int[] {x, y});
                    image[x][y] = newColor;
                }
            }
        }
        return image;
    }
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
