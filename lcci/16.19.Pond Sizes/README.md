---
comments: true
difficulty: Medium
edit_url: https://github.com/doocs/leetcode/edit/main/lcci/16.19.Pond%20Sizes/README_EN.md
---

<!-- problem:start -->

# [16.19. Pond Sizes](https://leetcode.cn/problems/pond-sizes-lcci)

[Chinese Version](/lcci/16.19.Pond%20Sizes/README.md)

## Description

<!-- description:start -->

<p>You have an integer matrix representing a plot of land, where the value at that loca&shy;tion represents the height above sea level. A value of zero indicates water. A pond is a region of water connected vertically, horizontally, or diagonally. The size of the pond is the total number of connected water cells. Write a method to compute the sizes of all ponds in the matrix.</p>

<p><strong>Example: </strong></p>

<pre>

<strong>Input: </strong>

[

  [0,2,1,0],

  [0,1,0,1],

  [1,1,0,1],

  [0,1,0,1]

]

<strong>Output: </strong> [1,2,4]

</pre>

<p><strong>Note: </strong></p>

<ul>
	<li><code>0 &lt; len(land) &lt;= 1000</code></li>
	<li><code>0 &lt; len(land[i]) &lt;= 1000</code></li>
</ul>

<!-- description:end -->

## Solutions

<!-- solution:start -->

### Solution 1: DFS

We can traverse each point $(i, j)$ in the integer matrix $land$. If the value of the point is $0$, we start a depth-first search from this point until we reach a point with a non-zero value. The number of points searched during this process is the size of the pond, which is added to the answer array.

> Note: To avoid duplicate searches, we set the value of the searched points to $1$.

Finally, we sort the answer array to obtain the final answer.

The time complexity is $O(m \times n \times \log (m \times n))$, and the space complexity is $O(m \times n)$. Here, $m$ and $n$ are the number of rows and columns in the matrix $land$, respectively.

<!-- tabs:start -->

#### Java

```java
class Solution {
    private int m;
    private int n;
    private int[][] land;

    public int[] pondSizes(int[][] land) {
        m = land.length;
        n = land[0].length;
        this.land = land;
        List<Integer> ans = new ArrayList<>();
        for (int i = 0; i < m; ++i) {
            for (int j = 0; j < n; ++j) {
                if (land[i][j] == 0) {
                    ans.add(dfs(i, j));
                }
            }
        }
        return ans.stream().sorted().mapToInt(Integer::valueOf).toArray();
    }

    private int dfs(int i, int j) {
        int res = 1;
        land[i][j] = 1;
        for (int x = i - 1; x <= i + 1; ++x) {
            for (int y = j - 1; y <= j + 1; ++y) {
                if (x >= 0 && x < m && y >= 0 && y < n && land[x][y] == 0) {
                    res += dfs(x, y);
                }
            }
        }
        return res;
    }
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
