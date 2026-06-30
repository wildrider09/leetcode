---
comments: true
difficulty: Hard
edit_url: https://github.com/doocs/leetcode/edit/main/solution/3600-3699/3661.Maximum%20Walls%20Destroyed%20by%20Robots/README_EN.md
rating: 2525
source: Weekly Contest 464 Q4
tags:
    - Array
    - Binary Search
    - Dynamic Programming
    - Sorting
---

<!-- problem:start -->

# [3661. Maximum Walls Destroyed by Robots](https://leetcode.com/problems/maximum-walls-destroyed-by-robots)

[Chinese Version](/solution/3600-3699/3661.Maximum%20Walls%20Destroyed%20by%20Robots/README.md)

## Description

<!-- description:start -->

<div data-docx-has-block-data="false" data-lark-html-role="root" data-page-id="Rax8d6clvoFeVtx7bzXcvkVynwf">
<div class="old-record-id-Y5dGdSKIMoNTttxGhHLccrpEnaf">There is an endless straight line populated with some robots and walls. You are given integer arrays <code>robots</code>, <code>distance</code>, and <code>walls</code>:</div>
</div>

<ul>
	<li><code>robots[i]</code> is the position of the <code>i<sup>th</sup></code> robot.</li>
	<li><code>distance[i]</code> is the <strong>maximum</strong> distance the <code>i<sup>th</sup></code> robot&#39;s bullet can travel.</li>
	<li><code>walls[j]</code> is the position of the <code>j<sup>th</sup></code> wall.</li>
</ul>

<p>Every robot has <strong>one</strong> bullet that can either fire to the left or the right <strong>at most </strong><code>distance[i]</code> meters.</p>

<p>A bullet destroys every wall in its path that lies within its range. Robots are fixed obstacles: if a bullet hits another robot before reaching a wall, it <strong>immediately stops</strong> at that robot and cannot continue.</p>

<p>Return the <strong>maximum</strong> number of <strong>unique</strong> walls that can be destroyed by the robots.</p>

<p>Notes:</p>

<ul>
	<li>A wall and a robot may share the same position; the wall can be destroyed by the robot at that position.</li>
	<li>Robots are not destroyed by bullets.</li>
</ul>

<p>&nbsp;</p>
<p><strong class="example">Example 1:</strong></p>

<div class="example-block">
<p><strong>Input:</strong> <span class="example-io">robots = [4], distance = [3], walls = [1,10]</span></p>

<p><strong>Output:</strong> <span class="example-io">1</span></p>

<p><strong>Explanation:</strong></p>

<ul>
	<li><code>robots[0] = 4</code> fires <strong>left</strong> with <code>distance[0] = 3</code>, covering <code>[1, 4]</code> and destroys <code>walls[0] = 1</code>.</li>
	<li>Thus, the answer is 1.</li>
</ul>
</div>

<p><strong class="example">Example 2:</strong></p>

<div class="example-block">
<p><strong>Input:</strong> <span class="example-io">robots = [10,2], distance = [5,1], walls = [5,2,7]</span></p>

<p><strong>Output:</strong> <span class="example-io">3</span></p>

<p><strong>Explanation:</strong></p>

<ul>
	<li><code>robots[0] = 10</code> fires <strong>left</strong> with <code>distance[0] = 5</code>, covering <code>[5, 10]</code> and destroys <code>walls[0] = 5</code> and <code>walls[2] = 7</code>.</li>
	<li><code>robots[1] = 2</code> fires <strong>left</strong> with <code>distance[1] = 1</code>, covering <code>[1, 2]</code> and destroys <code>walls[1] = 2</code>.</li>
	<li>Thus, the answer is 3.</li>
</ul>
</div>
<strong class="example">Example 3:</strong>

<div class="example-block">
<p><strong>Input:</strong> <span class="example-io">robots = [1,2], distance = [100,1], walls = [10]</span></p>

<p><strong>Output:</strong> <span class="example-io">0</span></p>

<p><strong>Explanation:</strong></p>

<p>In this example, only <code>robots[0]</code> can reach the wall, but its shot to the <strong>right</strong> is blocked by <code>robots[1]</code>; thus the answer is 0.</p>
</div>

<p>&nbsp;</p>
<p><strong>Constraints:</strong></p>

<ul>
	<li><code>1 &lt;= robots.length == distance.length &lt;= 10<sup>5</sup></code></li>
	<li><code>1 &lt;= walls.length &lt;= 10<sup>5</sup></code></li>
	<li><code>1 &lt;= robots[i], walls[j] &lt;= 10<sup>9</sup></code></li>
	<li><code>1 &lt;= distance[i] &lt;= 10<sup>5</sup></code></li>
	<li>All values in <code>robots</code> are <strong>unique</strong></li>
	<li>All values in <code>walls</code> are <strong>unique</strong></li>
</ul>

<!-- description:end -->

## Solutions

<!-- solution:start -->

### Solution 1: Memoized Search

We first store each robot with its range in an array and sort them by robot position. We also sort the wall positions. Next, we use depth-first search (DFS) to calculate the number of walls each robot can destroy, and use memoized search to avoid redundant calculations.

We design a function $\text{dfs}(i, j)$, where $i$ represents the index of the current robot being considered, and $j$ represents the firing direction of the next robot (0 for left, 1 for right), and returns the number of walls that can be destroyed. The answer is $\text{dfs}(n - 1, 1)$, where $j$ can be 0 or 1 in the boundary state.

The execution logic of function $\text{dfs}(i, j)$ is as follows:

If $i \lt 0$, it means all robots have been considered, return 0.

Otherwise, for the current robot, there are two firing directions to choose from.

If choosing to fire **left**, we need to calculate the left range $[\text{left}, \text{robot}[i][0]]$, and use binary search to calculate the number of walls that can be destroyed in this range. In this case, a total of $\text{dfs}(i - 1, 0) + \text{count}$ walls can be destroyed, where $\text{count}$ is the number of walls destroyed when the current robot fires left.

If choosing to fire **right**, we need to calculate the right range $[\text{robot}[i][0], \text{right}]$, and use binary search to calculate the number of walls that can be destroyed in this range. In this case, a total of $\text{dfs}(i - 1, 1) + \text{count}$ walls can be destroyed, where $\text{count}$ is the number of walls destroyed when the current robot fires right.

The return value of the function is the maximum number of walls that can be destroyed by the two firing directions.

Time complexity $O(n \times \log n + m \times \log m)$, space complexity $O(n)$. Where $n$ and $m$ are the numbers of robots and walls respectively.

<!-- tabs:start -->

#### Java

```java
class Solution {
    private Integer[][] f;
    private int[][] arr;
    private int[] walls;
    private int n;

    public int maxWalls(int[] robots, int[] distance, int[] walls) {
        n = robots.length;
        arr = new int[n][2];
        for (int i = 0; i < n; i++) {
            arr[i][0] = robots[i];
            arr[i][1] = distance[i];
        }
        Arrays.sort(arr, Comparator.comparingInt(a -> a[0]));
        Arrays.sort(walls);
        this.walls = walls;
        f = new Integer[n][2];
        return dfs(n - 1, 1);
    }

    private int dfs(int i, int j) {
        if (i < 0) {
            return 0;
        }
        if (f[i][j] != null) {
            return f[i][j];
        }

        int left = arr[i][0] - arr[i][1];
        if (i > 0) {
            left = Math.max(left, arr[i - 1][0] + 1);
        }
        int l = lowerBound(walls, left);
        int r = lowerBound(walls, arr[i][0] + 1);
        int ans = dfs(i - 1, 0) + (r - l);

        int right = arr[i][0] + arr[i][1];
        if (i + 1 < n) {
            if (j == 0) {
                right = Math.min(right, arr[i + 1][0] - arr[i + 1][1] - 1);
            } else {
                right = Math.min(right, arr[i + 1][0] - 1);
            }
        }
        l = lowerBound(walls, arr[i][0]);
        r = lowerBound(walls, right + 1);
        ans = Math.max(ans, dfs(i - 1, 1) + (r - l));
        return f[i][j] = ans;
    }

    private int lowerBound(int[] arr, int target) {
        int idx = Arrays.binarySearch(arr, target);
        if (idx < 0) {
            return -idx - 1;
        }
        return idx;
    }
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
