---
comments: true
difficulty: Medium
edit_url: https://github.com/doocs/leetcode/edit/main/solution/0500-0599/0547.Number%20of%20Provinces/README_EN.md
tags:
    - Depth-First Search
    - Breadth-First Search
    - Union Find
    - Graph
---

<!-- problem:start -->

# [547. Number of Provinces](https://leetcode.com/problems/number-of-provinces)

[Chinese Version](/solution/0500-0599/0547.Number%20of%20Provinces/README.md)

## Description

<!-- description:start -->

<p>There are <code>n</code> cities. Some of them are connected, while some are not. If city <code>a</code> is connected directly with city <code>b</code>, and city <code>b</code> is connected directly with city <code>c</code>, then city <code>a</code> is connected indirectly with city <code>c</code>.</p>

<p>A <strong>province</strong> is a group of directly or indirectly connected cities and no other cities outside of the group.</p>

<p>You are given an <code>n x n</code> matrix <code>isConnected</code> where <code>isConnected[i][j] = 1</code> if the <code>i<sup>th</sup></code> city and the <code>j<sup>th</sup></code> city are directly connected, and <code>isConnected[i][j] = 0</code> otherwise.</p>

<p>Return <em>the total number of <strong>provinces</strong></em>.</p>

<p>&nbsp;</p>
<p><strong class="example">Example 1:</strong></p>
<img alt="" src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/0500-0599/0547.Number%20of%20Provinces/images/graph1.jpg" style="width: 222px; height: 142px;" />
<pre>
<strong>Input:</strong> isConnected = [[1,1,0],[1,1,0],[0,0,1]]
<strong>Output:</strong> 2
</pre>

<p><strong class="example">Example 2:</strong></p>
<img alt="" src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/0500-0599/0547.Number%20of%20Provinces/images/graph2.jpg" style="width: 222px; height: 142px;" />
<pre>
<strong>Input:</strong> isConnected = [[1,0,0],[0,1,0],[0,0,1]]
<strong>Output:</strong> 3
</pre>

<p>&nbsp;</p>
<p><strong>Constraints:</strong></p>

<ul>
	<li><code>1 &lt;= n &lt;= 200</code></li>
	<li><code>n == isConnected.length</code></li>
	<li><code>n == isConnected[i].length</code></li>
	<li><code>isConnected[i][j]</code> is <code>1</code> or <code>0</code>.</li>
	<li><code>isConnected[i][i] == 1</code></li>
	<li><code>isConnected[i][j] == isConnected[j][i]</code></li>
</ul>

<!-- description:end -->

## Solutions

<!-- solution:start -->

### Solution 1: DFS

We create an array $\textit{vis}$ to record whether each city has been visited.

Next, we traverse each city $i$. If the city has not been visited, we start a depth-first search from that city. Using the matrix $\textit{isConnected}$, we find the cities directly connected to this city. These cities and the current city belong to the same province. We continue the depth-first search for these cities until all cities in the same province have been visited. This counts as one province, so we increment the answer $\textit{ans}$ by $1$. Then, we move to the next unvisited city and repeat the process until all cities have been traversed.

Finally, return the answer.

The time complexity is $O(n^2)$, and the space complexity is $O(n)$. Here, $n$ is the number of cities.

<!-- tabs:start -->

#### Java

```java
class Solution {
    private int[][] g;
    private boolean[] vis;

    public int findCircleNum(int[][] isConnected) {
        g = isConnected;
        int n = g.length;
        vis = new boolean[n];
        int ans = 0;
        for (int i = 0; i < n; ++i) {
            if (!vis[i]) {
                dfs(i);
                ++ans;
            }
        }
        return ans;
    }

    private void dfs(int i) {
        vis[i] = true;
        for (int j = 0; j < g.length; ++j) {
            if (!vis[j] && g[i][j] == 1) {
                dfs(j);
            }
        }
    }
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- solution:start -->

### Solution 2: Union-Find

We can also use the union-find data structure to maintain each connected component. Initially, each city belongs to a different connected component, so the number of provinces is $n$.

Next, we traverse the matrix $\textit{isConnected}$. If there is a connection between two cities $(i, j)$ and they belong to two different connected components, they will be merged into one connected component, and the number of provinces is decremented by $1$.

Finally, return the number of provinces.

The time complexity is $O(n^2 \times \log n)$, and the space complexity is $O(n)$. Here, $n$ is the number of cities, and $\log n$ is the time complexity of path compression in the union-find data structure.

<!-- tabs:start -->

#### Java

```java
class Solution {
    private int[] p;

    public int findCircleNum(int[][] isConnected) {
        int n = isConnected.length;
        p = new int[n];
        for (int i = 0; i < n; ++i) {
            p[i] = i;
        }
        int ans = n;
        for (int i = 0; i < n; ++i) {
            for (int j = i + 1; j < n; ++j) {
                if (isConnected[i][j] == 1) {
                    int pa = find(i), pb = find(j);
                    if (pa != pb) {
                        p[pa] = pb;
                        --ans;
                    }
                }
            }
        }
        return ans;
    }

    private int find(int x) {
        if (p[x] != x) {
            p[x] = find(p[x]);
        }
        return p[x];
    }
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
