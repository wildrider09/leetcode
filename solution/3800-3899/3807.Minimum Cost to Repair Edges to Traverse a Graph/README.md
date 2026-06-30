---
comments: true
difficulty: Medium
edit_url: https://github.com/doocs/leetcode/edit/main/solution/3800-3899/3807.Minimum%20Cost%20to%20Repair%20Edges%20to%20Traverse%20a%20Graph/README_EN.md
tags:
    - Breadth-First Search
    - Graph
    - Binary Search
---

<!-- problem:start -->

# [3807. Minimum Cost to Repair Edges to Traverse a Graph 🔒](https://leetcode.com/problems/minimum-cost-to-repair-edges-to-traverse-a-graph)

[Chinese Version](/solution/3800-3899/3807.Minimum%20Cost%20to%20Repair%20Edges%20to%20Traverse%20a%20Graph/README.md)

## Description

<!-- description:start -->

<p>You are given an <strong>undirected graph</strong> with <code>n</code> nodes labeled from 0 to <code>n - 1</code>. The graph consists of <code>m</code> edges represented by a 2D integer array <code>edges</code>, where <code>edges[i] = [u<sub>i</sub>, v<sub>i</sub>, w<sub>i</sub>]</code> indicates that there is an edge between nodes <code>u<sub>i</sub></code> and <code>v<sub>i</sub></code> with a repair cost of <code>w<sub>i</sub></code>.</p>

<p>You are also given an integer <code>k</code>. Initially, <strong>all</strong> edges are damaged.</p>

<p>You may choose a non-negative integer <code>money</code> and repair <strong>all</strong> edges whose repair cost is <strong>less than or equal</strong> to <code>money</code>. All other edges remain damaged and cannot be used.</p>

<p>You want to travel from node 0 to node <code>n - 1</code> using at most <code>k</code> edges.</p>

<p>Return an integer denoting the <strong>minimum</strong> amount of money required to make this possible, or return -1 if it is impossible.</p>

<p>&nbsp;</p>
<p><strong class="example">Example 1:</strong></p>

<p><strong class="example"><img alt="" src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/3800-3899/3807.Minimum%20Cost%20to%20Repair%20Edges%20to%20Traverse%20a%20Graph/images/ex1drawio.png" style="width: 211px; height: 171px;" /></strong></p>

<div class="example-block">
<p><strong>Input:</strong> <span class="example-io">n = 3, edges = [[0,1,10],[1,2,10],[0,2,100]], k = 1</span></p>

<p><strong>Output:</strong> <span class="example-io">100</span></p>

<p><strong>Explanation:</strong></p>

<p>The only valid path using at most <code>k = 1</code> edge is <code>0 -&gt; 2</code>, which requires repairing the edge with cost 100. Therefore, the minimum required amount of money is 100.</p>
</div>

<p><strong class="example">Example 2:</strong></p>

<p><strong class="example"><img alt="" src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/3800-3899/3807.Minimum%20Cost%20to%20Repair%20Edges%20to%20Traverse%20a%20Graph/images/ex2drawio.png" style="width: 361px; height: 251px;" /></strong></p>

<div class="example-block">
<p><strong>Input:</strong> <span class="example-io">n = 6, edges = [[0,2,5],[2,3,6],[3,4,7],[4,5,5],[0,1,10],[1,5,12],[0,3,9],[1,2,8],[2,4,11]], k = 2</span></p>

<p><strong>Output:</strong> <span class="example-io">12</span></p>

<p><strong>Explanation:</strong></p>

<ul>
	<li>With <code>money = 12</code>, all edges with repair cost at most 12 become usable.</li>
	<li>This allows the path <code>0 -&gt; 1 -&gt; 5</code>, which uses exactly 2 edges and reaches node 5.</li>
	<li>If <code>money &lt; 12</code>, there is no available path of length at most <code>k = 2</code> from node 0 to node 5.</li>
	<li>Therefore, the minimum required money is 12.</li>
</ul>
</div>

<p><strong class="example">Example 3:</strong></p>

<p><strong class="example"><img alt="" src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/3800-3899/3807.Minimum%20Cost%20to%20Repair%20Edges%20to%20Traverse%20a%20Graph/images/ex3drawio.png" style="width: 312px; height: 52px;" />​​​​​​​</strong></p>

<div class="example-block">
<p><strong>Input:</strong> <span class="example-io">n = 3, edges = [[0,1,1]], k = 1</span></p>

<p><strong>Output:</strong> <span class="example-io">-1</span></p>

<p><strong>Explanation:</strong></p>

<p>It is impossible to reach node 2 from node 0 using any amount of money. Therefore, the answer is -1.</p>
</div>

<p>&nbsp;</p>
<p><strong>Constraints:</strong></p>

<ul>
	<li><code>2 &lt;= n &lt;= 5 * 10<sup>4</sup></code></li>
	<li><code>1 &lt;= edges.length == m &lt;= 10<sup>5</sup></code></li>
	<li><code>edges[i] = [u<sub>i</sub>, v<sub>i</sub>, w<sub>i</sub>]</code></li>
	<li><code>0 &lt;= u<sub>i</sub>, v<sub>i</sub> &lt; n</code></li>
	<li><code>1 &lt;= w<sub>i</sub> &lt;= 10<sup>9</sup></code></li>
	<li><code>1 &lt;= k &lt;= n</code></li>
	<li>There are no self-loops or duplicate edges in the graph.</li>
</ul>

<!-- description:end -->

## Solutions

<!-- solution:start -->

### Solution 1: Binary Search + BFS

We observe that the higher the repair cost, the more edges become available, making it easier to satisfy the requirement of reaching node $n - 1$ from node $0$ using at most $k$ edges. Moreover, the minimum repair cost must be among the costs in $\textit{edges}$. Therefore, we first sort $\textit{edges}$ by repair cost, then use binary search to find the minimum repair cost that satisfies the requirement.

We perform binary search on the index of the repair cost, defining the left boundary as $l = 0$ and the right boundary as $r = |\textit{edges}| - 1$. For the middle position $mid = \lfloor (l + r) / 2 \rfloor$, we add all edges with repair cost less than or equal to $\textit{edges}[mid][2]$ to the graph, then use BFS to determine whether we can reach node $n - 1$ from node $0$ using at most $k$ edges. If possible, we update the right boundary to $r = mid$; otherwise, we update the left boundary to $l = mid + 1$. After the binary search completes, we need to perform one more BFS to check if $\textit{edges}[l][2]$ satisfies the requirement. If it does, we return $\textit{edges}[l][2]$; otherwise, we return $-1$.

The time complexity is $O((m + n) \times \log m)$ and the space complexity is $O(n)$, where $n$ and $m$ are the number of nodes and edges, respectively.

<!-- tabs:start -->

#### Java

```java
class Solution {
    private int n;
    private int[][] edges;
    private int k;

    public int minCost(int n, int[][] edges, int k) {
        this.n = n;
        this.edges = edges;
        this.k = k;
        Arrays.sort(edges, (a, b) -> a[2] - b[2]);
        int l = 0, r = edges.length - 1;
        while (l < r) {
            int mid = (l + r) >> 1;
            if (check(mid)) {
                r = mid;
            } else {
                l = mid + 1;
            }
        }
        return check(l) ? edges[l][2] : -1;
    }

    private boolean check(int idx) {
        List<Integer>[] g = new List[n];
        Arrays.setAll(g, i -> new ArrayList<>());
        for (int i = 0; i <= idx; ++i) {
            int u = edges[i][0], v = edges[i][1];
            g[u].add(v);
            g[v].add(u);
        }
        List<Integer> q = new ArrayList<>();
        q.add(0);
        int dist = 0;
        boolean[] vis = new boolean[n];
        vis[0] = true;
        while (!q.isEmpty()) {
            List<Integer> nq = new ArrayList<>();
            for (int u : q) {
                if (u == n - 1) {
                    return dist <= k;
                }
                for (int v : g[u]) {
                    if (!vis[v]) {
                        vis[v] = true;
                        nq.add(v);
                    }
                }
            }
            q = nq;
            ++dist;
        }
        return false;
    }
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
