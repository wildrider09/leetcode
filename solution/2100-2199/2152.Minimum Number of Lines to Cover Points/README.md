---
comments: true
difficulty: Medium
edit_url: https://github.com/doocs/leetcode/edit/main/solution/2100-2199/2152.Minimum%20Number%20of%20Lines%20to%20Cover%20Points/README_EN.md
tags:
    - Bit Manipulation
    - Geometry
    - Array
    - Hash Table
    - Math
    - Dynamic Programming
    - Backtracking
    - Bitmask
---

<!-- problem:start -->

# [2152. Minimum Number of Lines to Cover Points 🔒](https://leetcode.com/problems/minimum-number-of-lines-to-cover-points)

[Chinese Version](/solution/2100-2199/2152.Minimum%20Number%20of%20Lines%20to%20Cover%20Points/README.md)

## Description

<!-- description:start -->

<p>You are given an array <code>points</code> where <code>points[i] = [x<sub>i</sub>, y<sub>i</sub>]</code> represents a point on an <strong>X-Y </strong>plane.</p>

<p><strong>Straight lines</strong> are going to be added to the <strong>X-Y</strong> plane, such that every point is covered by at <strong>least </strong>one line.</p>

<p>Return <em>the <strong>minimum </strong>number of <strong>straight lines</strong> needed to cover all the points</em>.</p>

<p>&nbsp;</p>
<p><strong class="example">Example 1:</strong></p>
<img src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/2100-2199/2152.Minimum%20Number%20of%20Lines%20to%20Cover%20Points/images/image-20220123200023-1.png" style="width: 350px; height: 402px;" />
<pre>
<strong>Input:</strong> points = [[0,1],[2,3],[4,5],[4,3]]
<strong>Output:</strong> 2
<strong>Explanation:</strong> The minimum number of straight lines needed is two. One possible solution is to add:
- One line connecting the point at (0, 1) to the point at (4, 5).
- Another line connecting the point at (2, 3) to the point at (4, 3).
</pre>

<p><strong class="example">Example 2:</strong></p>
<img src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/2100-2199/2152.Minimum%20Number%20of%20Lines%20to%20Cover%20Points/images/image-20220123200057-3.png" style="width: 350px; height: 480px;" />
<pre>
<strong>Input:</strong> points = [[0,2],[-2,-2],[1,4]]
<strong>Output:</strong> 1
<strong>Explanation:</strong> The minimum number of straight lines needed is one. The only solution is to add:
- One line connecting the point at (-2, -2) to the point at (1, 4).
</pre>

<p>&nbsp;</p>
<p><strong>Constraints:</strong></p>

<ul>
	<li><code>1 &lt;= points.length &lt;= 10</code></li>
	<li><code>points[i].length == 2</code></li>
	<li><code>-100 &lt;= x<sub>i</sub>, y<sub>i</sub> &lt;= 100</code></li>
	<li>All the <code>points</code> are <strong>unique</strong>.</li>
</ul>

<!-- description:end -->

## Solutions

<!-- solution:start -->

### Solution 1

<!-- tabs:start -->

#### Java

```java
class Solution {
    private int[] f;
    private int[][] points;
    private int n;

    public int minimumLines(int[][] points) {
        n = points.length;
        this.points = points;
        f = new int[1 << n];
        return dfs(0);
    }

    private int dfs(int state) {
        if (state == (1 << n) - 1) {
            return 0;
        }
        if (f[state] != 0) {
            return f[state];
        }
        int ans = 1 << 30;
        for (int i = 0; i < n; ++i) {
            if (((state >> i) & 1) == 0) {
                for (int j = i + 1; j < n; ++j) {
                    int nxt = state | 1 << i | 1 << j;
                    for (int k = j + 1; k < n; ++k) {
                        if (((state >> k) & 1) == 0 && check(i, j, k)) {
                            nxt |= 1 << k;
                        }
                    }
                    ans = Math.min(ans, dfs(nxt) + 1);
                }
                if (i == n - 1) {
                    ans = Math.min(ans, dfs(state | 1 << i) + 1);
                }
            }
        }
        return f[state] = ans;
    }

    private boolean check(int i, int j, int k) {
        int x1 = points[i][0], y1 = points[i][1];
        int x2 = points[j][0], y2 = points[j][1];
        int x3 = points[k][0], y3 = points[k][1];
        return (x2 - x1) * (y3 - y1) == (x3 - x1) * (y2 - y1);
    }
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
