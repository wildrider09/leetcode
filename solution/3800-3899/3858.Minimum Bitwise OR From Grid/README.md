---
comments: true
difficulty: Medium
edit_url: https://github.com/doocs/leetcode/edit/main/solution/3800-3899/3858.Minimum%20Bitwise%20OR%20From%20Grid/README_EN.md
rating: 1947
source: Weekly Contest 491 Q3
---

<!-- problem:start -->

# [3858. Minimum Bitwise OR From Grid](https://leetcode.com/problems/minimum-bitwise-or-from-grid)

[Chinese Version](/solution/3800-3899/3858.Minimum%20Bitwise%20OR%20From%20Grid/README.md)

## Description

<!-- description:start -->

<p>You are given a 2D integer array <code>grid</code> of size <code>m x n</code>.</p>

<p>You must select <strong>exactly one</strong> integer from each row of the grid.</p>

<p>Return an integer denoting the <strong>minimum possible bitwise OR</strong> of the selected integers from each row.</p>

<p>&nbsp;</p>
<p><strong class="example">Example 1:</strong></p>

<div class="example-block">
<p><strong>Input:</strong> <span class="example-io">grid = [[1,5],[2,4]]</span></p>

<p><strong>Output:</strong> <span class="example-io">3</span></p>

<p><strong>Explanation:</strong></p>

<ul>
	<li>Choose 1 from the first row and 2 from the second row.</li>
	<li>The bitwise OR of <code>1 | 2 = 3</code>​​​​​​​, which is the minimum possible.</li>
</ul>
</div>

<p><strong class="example">Example 2:</strong></p>

<div class="example-block">
<p><strong>Input:</strong> <span class="example-io">grid = [[3,5],[6,4]]</span></p>

<p><strong>Output:</strong> <span class="example-io">5</span></p>

<p><strong>Explanation:</strong></p>

<ul>
	<li>Choose 5 from the first row and 4 from the second row.</li>
	<li>The bitwise OR of <code>5 | 4 = 5</code>​​​​​​​, which is the minimum possible.</li>
</ul>
</div>

<p><strong class="example">Example 3:</strong></p>

<div class="example-block">
<p><strong>Input:</strong> <span class="example-io">grid = [[7,9,8]]</span></p>

<p><strong>Output:</strong> <span class="example-io">7</span></p>

<p><strong>Explanation:</strong></p>

<ul>
	<li>Choosing 7 gives the minimum bitwise OR.</li>
</ul>
</div>

<p>&nbsp;</p>
<p><strong>Constraints:</strong></p>

<ul>
	<li><code>1 &lt;= m == grid.length &lt;= 10<sup>5</sup></code></li>
	<li><code>1 &lt;= n == grid[i].length &lt;= 10<sup>5</sup></code></li>
	<li><code>m * n &lt;= 10<sup>5</sup></code></li>
	<li><code>1 &lt;= grid[i][j] &lt;= 10<sup>5</sup>​​​​​​​</code></li>
</ul>

<!-- description:end -->

## Solutions

<!-- solution:start -->

### Solution 1

<!-- tabs:start -->

#### Java

```java
class Solution {
    public int minimumOR(int[][] grid) {
        int mx = 0;
        for (int[] row : grid) {
            for (int x : row) {
                mx = Math.max(mx, x);
            }
        }

        int m = 32 - Integer.numberOfLeadingZeros(mx);
        int ans = 0;

        for (int i = m - 1; i >= 0; i--) {
            int mask = ans | ((1 << i) - 1);
            for (int[] row : grid) {
                boolean found = false;
                for (int x : row) {
                    if ((x | mask) == mask) {
                        found = true;
                        break;
                    }
                }
                if (!found) {
                    ans |= 1 << i;
                    break;
                }
            }
        }

        return ans;
    }
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
