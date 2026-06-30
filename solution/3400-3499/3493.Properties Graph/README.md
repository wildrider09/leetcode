---
comments: true
difficulty: Medium
edit_url: https://github.com/doocs/leetcode/edit/main/solution/3400-3499/3493.Properties%20Graph/README_EN.md
rating: 1565
source: Weekly Contest 442 Q2
tags:
    - Depth-First Search
    - Breadth-First Search
    - Union Find
    - Graph
    - Array
    - Hash Table
---

<!-- problem:start -->

# [3493. Properties Graph](https://leetcode.com/problems/properties-graph)

[Chinese Version](/solution/3400-3499/3493.Properties%20Graph/README.md)

## Description

<!-- description:start -->

<p>You are given a 2D integer array <code>properties</code> having dimensions <code>n x m</code> and an integer <code>k</code>.</p>

<p>Define a function <code>intersect(a, b)</code> that returns the <strong>number of distinct integers</strong> common to both arrays <code>a</code> and <code>b</code>.</p>

<p>Construct an <strong>undirected</strong> graph where each index <code>i</code> corresponds to <code>properties[i]</code>. There is an edge between node <code>i</code> and node <code>j</code> if and only if <code>intersect(properties[i], properties[j]) &gt;= k</code>, where <code>i</code> and <code>j</code> are in the range <code>[0, n - 1]</code> and <code>i != j</code>.</p>

<p>Return the number of <strong>connected components</strong> in the resulting graph.</p>

<p>&nbsp;</p>
<p><strong class="example">Example 1:</strong></p>

<div class="example-block">
<p><strong>Input:</strong> <span class="example-io">properties = [[1,2],[1,1],[3,4],[4,5],[5,6],[7,7]], k = 1</span></p>

<p><strong>Output:</strong> <span class="example-io">3</span></p>

<p><strong>Explanation:</strong></p>

<p>The graph formed has 3 connected components:</p>

<p><img height="171" src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/3400-3499/3493.Properties%20Graph/images/image.png" width="279" /></p>
</div>

<p><strong class="example">Example 2:</strong></p>

<div class="example-block">
<p><strong>Input:</strong> <span class="example-io">properties = [[1,2,3],[2,3,4],[4,3,5]], k = 2</span></p>

<p><strong>Output:</strong> <span class="example-io">1</span></p>

<p><strong>Explanation:</strong></p>

<p>The graph formed has 1 connected component:</p>

<p><img alt="" src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/3400-3499/3493.Properties%20Graph/images/screenshot-from-2025-02-27-23-58-34.png" style="width: 219px; height: 171px;" /></p>
</div>

<p><strong class="example">Example 3:</strong></p>

<div class="example-block">
<p><strong>Input:</strong> <span class="example-io">properties = [[1,1],[1,1]], k = 2</span></p>

<p><strong>Output:</strong> <span class="example-io">2</span></p>

<p><strong>Explanation:</strong></p>

<p><code>intersect(properties[0], properties[1]) = 1</code>, which is less than <code>k</code>. This means there is no edge between <code>properties[0]</code> and <code>properties[1]</code> in the graph.</p>
</div>

<p>&nbsp;</p>
<p><strong>Constraints:</strong></p>

<ul>
	<li><code>1 &lt;= n == properties.length &lt;= 100</code></li>
	<li><code>1 &lt;= m == properties[i].length &lt;= 100</code></li>
	<li><code>1 &lt;= properties[i][j] &lt;= 100</code></li>
	<li><code>1 &lt;= k &lt;= m</code></li>
</ul>

<!-- description:end -->

## Solutions

<!-- solution:start -->

### Solution 1: Hash Table + DFS

We first convert each attribute array into a hash table and store them in a hash table array $\textit{ss}$. We define a graph $\textit{g}$, where $\textit{g}[i]$ stores the indices of attribute arrays that are connected to $\textit{properties}[i]$.

Then, we iterate through all attribute hash tables. For each pair of attribute hash tables $(i, j)$ where $j < i$, we check whether the number of common elements between them is at least $k$. If so, we add an edge from $i$ to $j$ in the graph $\textit{g}$, as well as an edge from $j$ to $i$.

Finally, we use Depth-First Search (DFS) to compute the number of connected components in the graph $\textit{g}$.

The time complexity is $O(n^2 \times m)$, and the space complexity is $O(n \times m)$, where $n$ is the length of the attribute arrays and $m$ is the number of elements in an attribute array.

<!-- tabs:start -->

#### Java

```java
class Solution {
    private List<Integer>[] g;
    private boolean[] vis;

    public int numberOfComponents(int[][] properties, int k) {
        int n = properties.length;
        g = new List[n];
        Set<Integer>[] ss = new Set[n];
        Arrays.setAll(g, i -> new ArrayList<>());
        Arrays.setAll(ss, i -> new HashSet<>());
        for (int i = 0; i < n; ++i) {
            for (int x : properties[i]) {
                ss[i].add(x);
            }
        }
        for (int i = 0; i < n; ++i) {
            for (int j = 0; j < i; ++j) {
                int cnt = 0;
                for (int x : ss[i]) {
                    if (ss[j].contains(x)) {
                        ++cnt;
                    }
                }
                if (cnt >= k) {
                    g[i].add(j);
                    g[j].add(i);
                }
            }
        }

        int ans = 0;
        vis = new boolean[n];
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
        for (int j : g[i]) {
            if (!vis[j]) {
                dfs(j);
            }
        }
    }
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
