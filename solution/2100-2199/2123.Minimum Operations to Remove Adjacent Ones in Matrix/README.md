---
comments: true
difficulty: Hard
edit_url: https://github.com/doocs/leetcode/edit/main/solution/2100-2199/2123.Minimum%20Operations%20to%20Remove%20Adjacent%20Ones%20in%20Matrix/README_EN.md
tags:
    - Graph
    - Array
    - Matrix
---

<!-- problem:start -->

# [2123. Minimum Operations to Remove Adjacent Ones in Matrix 🔒](https://leetcode.com/problems/minimum-operations-to-remove-adjacent-ones-in-matrix)

[Chinese Version](/solution/2100-2199/2123.Minimum%20Operations%20to%20Remove%20Adjacent%20Ones%20in%20Matrix/README.md)

## Description

<!-- description:start -->

<p>You are given a <strong>0-indexed</strong> binary matrix <code>grid</code>. In one operation, you can flip any <code>1</code> in <code>grid</code> to be <code>0</code>.</p>

<p>A binary matrix is <strong>well-isolated</strong> if there is no <code>1</code> in the matrix that is <strong>4-directionally connected</strong> (i.e., horizontal and vertical) to another <code>1</code>.</p>

<p>Return <em>the minimum number of operations to make </em><code>grid</code><em> <strong>well-isolated</strong></em>.</p>

<p>&nbsp;</p>
<p><strong class="example">Example 1:</strong></p>
<img src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/2100-2199/2123.Minimum%20Operations%20to%20Remove%20Adjacent%20Ones%20in%20Matrix/images/image-20211223181501-1.png" style="width: 644px; height: 250px;" />
<pre>
<strong>Input:</strong> grid = [[1,1,0],[0,1,1],[1,1,1]]
<strong>Output:</strong> 3
<strong>Explanation:</strong> Use 3 operations to change grid[0][1], grid[1][2], and grid[2][1] to 0.
After, no more 1&#39;s are 4-directionally connected and grid is well-isolated.
</pre>

<p><strong class="example">Example 2:</strong></p>
<img src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/2100-2199/2123.Minimum%20Operations%20to%20Remove%20Adjacent%20Ones%20in%20Matrix/images/image-20211223181518-2.png" style="height: 250px; width: 255px;" />
<pre>
<strong>Input:</strong> grid = [[0,0,0],[0,0,0],[0,0,0]]
<strong>Output:</strong> 0
<strong>Explanation:</strong> There are no 1&#39;s in grid and it is well-isolated.
No operations were done so return 0.
</pre>

<p><strong class="example">Example 3:</strong></p>
<img src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/2100-2199/2123.Minimum%20Operations%20to%20Remove%20Adjacent%20Ones%20in%20Matrix/images/image-20211223181817-3.png" style="width: 165px; height: 167px;" />
<pre>
<strong>Input:</strong> grid = [[0,1],[1,0]]
<strong>Output:</strong> 0
<strong>Explanation:</strong> None of the 1&#39;s are 4-directionally connected and grid is well-isolated.
No operations were done so return 0.
</pre>

<p>&nbsp;</p>
<p><strong>Constraints:</strong></p>

<ul>
	<li><code>m == grid.length</code></li>
	<li><code>n == grid[i].length</code></li>
	<li><code>1 &lt;= m, n &lt;= 300</code></li>
	<li><code>grid[i][j]</code> is either <code>0</code> or <code>1</code>.</li>
</ul>

<!-- description:end -->

## Solutions

<!-- solution:start -->

### Solution 1: Hungarian Algorithm

We observe that if two $1$s in the matrix are adjacent, they must belong to different groups. Therefore, we can treat all $1$s in the matrix as vertices, and connect an edge between two adjacent $1$s to construct a bipartite graph.

Then, the problem can be transformed into finding the minimum vertex cover of a bipartite graph, which means selecting the minimum number of vertices to cover all edges. Since the minimum vertex cover of a bipartite graph equals the maximum matching, we can use the Hungarian algorithm to find the maximum matching of the bipartite graph.

The core idea of the Hungarian algorithm is to continuously search for augmenting paths starting from unmatched vertices until no augmenting path exists, which yields the maximum matching.

The time complexity is $O(m \times n)$, where $n$ and $m$ are the number of $1$s in the matrix and the number of edges, respectively.

<!-- tabs:start -->

#### Java

```java
class Solution {
    private Map<Integer, List<Integer>> g = new HashMap<>();
    private Set<Integer> vis = new HashSet<>();
    private int[] match;

    public int minimumOperations(int[][] grid) {
        int m = grid.length, n = grid[0].length;
        for (int i = 0; i < m; ++i) {
            for (int j = 0; j < n; ++j) {
                if ((i + j) % 2 == 1 && grid[i][j] == 1) {
                    int x = i * n + j;
                    if (i < m - 1 && grid[i + 1][j] == 1) {
                        g.computeIfAbsent(x, z -> new ArrayList<>()).add(x + n);
                    }
                    if (i > 0 && grid[i - 1][j] == 1) {
                        g.computeIfAbsent(x, z -> new ArrayList<>()).add(x - n);
                    }
                    if (j < n - 1 && grid[i][j + 1] == 1) {
                        g.computeIfAbsent(x, z -> new ArrayList<>()).add(x + 1);
                    }
                    if (j > 0 && grid[i][j - 1] == 1) {
                        g.computeIfAbsent(x, z -> new ArrayList<>()).add(x - 1);
                    }
                }
            }
        }
        match = new int[m * n];
        Arrays.fill(match, -1);
        int ans = 0;
        for (int i : g.keySet()) {
            ans += find(i);
            vis.clear();
        }
        return ans;
    }

    private int find(int i) {
        for (int j : g.get(i)) {
            if (vis.add(j)) {
                if (match[j] == -1 || find(match[j]) == 1) {
                    match[j] = i;
                    return 1;
                }
            }
        }
        return 0;
    }
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
