---
comments: true
difficulty: Medium
edit_url: https://github.com/doocs/leetcode/edit/main/solution/2000-2099/2061.Number%20of%20Spaces%20Cleaning%20Robot%20Cleaned/README_EN.md
tags:
    - Array
    - Matrix
    - Simulation
---

<!-- problem:start -->

# [2061. Number of Spaces Cleaning Robot Cleaned 🔒](https://leetcode.com/problems/number-of-spaces-cleaning-robot-cleaned)

[Chinese Version](/solution/2000-2099/2061.Number%20of%20Spaces%20Cleaning%20Robot%20Cleaned/README.md)

## Description

<!-- description:start -->

<p>A room is represented by a <strong>0-indexed</strong> 2D binary matrix <code>room</code> where a <code>0</code> represents an <strong>empty</strong> space and a <code>1</code> represents a space with an <strong>object</strong>. The top left corner of the room will be empty in all test cases.</p>

<p>A cleaning robot starts at the top left corner of the room and is facing right. The robot will continue heading straight until it reaches the edge of the room or it hits an object, after which it will turn 90 degrees <strong>clockwise</strong> and repeat this process. The starting space and all spaces that the robot visits are <strong>cleaned</strong> by it.</p>

<p>Return <em>the number of <strong>clean</strong> spaces in the room if the robot runs indefinitely.</em></p>

<p>&nbsp;</p>
<p><strong class="example">Example 1:</strong></p>
<img src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/2000-2099/2061.Number%20of%20Spaces%20Cleaning%20Robot%20Cleaned/images/image-20211101204703-1.png" style="width: 250px; height: 242px;" />
<p>&nbsp;</p>

<div class="example-block">
<p><strong>Input:</strong> <span class="example-io">room = [[0,0,0],[1,1,0],[0,0,0]]</span></p>

<p><strong>Output:</strong> <span class="example-io">7</span></p>

<p><strong>Explanation:</strong></p>

<ol>
	<li>​​​​​​​The robot cleans the spaces at (0, 0), (0, 1), and (0, 2).</li>
	<li>The robot is at the edge of the room, so it turns 90 degrees clockwise and now faces down.</li>
	<li>The robot cleans the spaces at (1, 2), and (2, 2).</li>
	<li>The robot is at the edge of the room, so it turns 90 degrees clockwise and now faces left.</li>
	<li>The robot cleans the spaces at (2, 1), and (2, 0).</li>
	<li>The robot has cleaned all 7 empty spaces, so return 7.</li>
</ol>
</div>

<p><strong class="example">Example 2:</strong></p>
<img src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/2000-2099/2061.Number%20of%20Spaces%20Cleaning%20Robot%20Cleaned/images/image-20211101204736-2.png" style="width: 250px; height: 245px;" />
<p>&nbsp;</p>

<div class="example-block">
<p><strong>Input:</strong> <span class="example-io">room = [[0,1,0],[1,0,0],[0,0,0]]</span></p>

<p><strong>Output:</strong> <span class="example-io">1</span></p>

<p><strong>Explanation:</strong></p>

<ol>
	<li>The robot cleans the space at (0, 0).</li>
	<li>The robot hits an object, so it turns 90 degrees clockwise and now faces down.</li>
	<li>The robot hits an object, so it turns 90 degrees clockwise and now faces left.</li>
	<li>The robot is at the edge of the room, so it turns 90 degrees clockwise and now faces up.</li>
	<li>The robot is at the edge of the room, so it turns 90 degrees clockwise and now faces right.</li>
	<li>The robot is back at its starting position.</li>
	<li>The robot has cleaned 1 space, so return 1.</li>
</ol>
</div>

<p><strong class="example">Example 3:</strong></p>

<div class="example-block">
<p><strong>Input:</strong> <span class="example-io">room = [[0,0,0],[0,0,0],[0,0,0]]</span></p>

<p><strong>Output:</strong> <span class="example-io">8</span>​​​​​​​</p>

<p>&nbsp;</p>
</div>

<p>&nbsp;</p>
<p><strong>Constraints:</strong></p>

<ul>
	<li><code>m == room.length</code></li>
	<li><code>n == room[r].length</code></li>
	<li><code>1 &lt;= m, n &lt;= 300</code></li>
	<li><code>room[r][c]</code> is either <code>0</code> or <code>1</code>.</li>
	<li><code>room[0][0] == 0</code></li>
</ul>

<!-- description:end -->

## Solutions

<!-- solution:start -->

### Solution 1

<!-- tabs:start -->

#### Java

```java
class Solution {
    private boolean[][][] vis;
    private int[][] room;
    private int ans;

    public int numberOfCleanRooms(int[][] room) {
        vis = new boolean[room.length][room[0].length][4];
        this.room = room;
        dfs(0, 0, 0);
        return ans;
    }

    private void dfs(int i, int j, int k) {
        if (vis[i][j][k]) {
            return;
        }
        int[] dirs = {0, 1, 0, -1, 0};
        ans += room[i][j] == 0 ? 1 : 0;
        room[i][j] = -1;
        vis[i][j][k] = true;
        int x = i + dirs[k], y = j + dirs[k + 1];
        if (x >= 0 && x < room.length && y >= 0 && y < room[0].length && room[x][y] != 1) {
            dfs(x, y, k);
        } else {
            dfs(i, j, (k + 1) % 4);
        }
    }
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- solution:start -->

### Solution 2

<!-- tabs:start -->

#### Java

```java
class Solution {
    public int numberOfCleanRooms(int[][] room) {
        int[] dirs = {0, 1, 0, -1, 0};
        int i = 0, j = 0, k = 0;
        int m = room.length, n = room[0].length;
        boolean[][][] vis = new boolean[m][n][4];
        int ans = 0;
        while (!vis[i][j][k]) {
            vis[i][j][k] = true;
            ans += room[i][j] == 0 ? 1 : 0;
            room[i][j] = -1;
            int x = i + dirs[k], y = j + dirs[k + 1];
            if (x >= 0 && x < m && y >= 0 && y < n && room[x][y] != 1) {
                i = x;
                j = y;
            } else {
                k = (k + 1) % 4;
            }
        }
        return ans;
    }
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
