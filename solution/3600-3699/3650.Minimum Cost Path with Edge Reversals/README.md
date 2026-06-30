---
comments: true
difficulty: Medium
edit_url: https://github.com/doocs/leetcode/edit/main/solution/3600-3699/3650.Minimum%20Cost%20Path%20with%20Edge%20Reversals/README_EN.md
rating: 1853
source: Biweekly Contest 163 Q3
tags:
    - Graph
    - Shortest Path
    - Heap (Priority Queue)
---

<!-- problem:start -->

# [3650. Minimum Cost Path with Edge Reversals](https://leetcode.com/problems/minimum-cost-path-with-edge-reversals)

[Chinese Version](/solution/3600-3699/3650.Minimum%20Cost%20Path%20with%20Edge%20Reversals/README.md)

## Description

<!-- description:start -->

<p>You are given a directed, weighted graph with <code>n</code> nodes labeled from 0 to <code>n - 1</code>, and an array <code>edges</code> where <code>edges[i] = [u<sub>i</sub>, v<sub>i</sub>, w<sub>i</sub>]</code> represents a directed edge from node <code>u<sub>i</sub></code> to node <code>v<sub>i</sub></code> with cost <code>w<sub>i</sub></code>.</p>

<p>Each node <code>u<sub>i</sub></code> has a switch that can be used <strong>at most once</strong>: when you arrive at <code>u<sub>i</sub></code> and have not yet used its switch, you may activate it on one of its incoming edges <code>v<sub>i</sub> &rarr; u<sub>i</sub></code> reverse that edge to <code>u<sub>i</sub> &rarr; v<sub>i</sub></code> and <strong>immediately</strong> traverse it.</p>

<p>The reversal is only valid for that single move, and using a reversed edge costs <code>2 * w<sub>i</sub></code>.</p>

<p>Return the <strong>minimum</strong> total cost to travel from node 0 to node <code>n - 1</code>. If it is not possible, return -1.</p>

<p>&nbsp;</p>
<p><strong class="example">Example 1:</strong></p>

<div class="example-block">
<p><strong>Input:</strong> <span class="example-io">n = 4, edges = [[0,1,3],[3,1,1],[2,3,4],[0,2,2]]</span></p>

<p><strong>Output:</strong> <span class="example-io">5</span></p>

<p><strong>Explanation: </strong></p>

<p><strong><img alt="" src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/3600-3699/3650.Minimum%20Cost%20Path%20with%20Edge%20Reversals/images/e1drawio.png" style="width: 171px; height: 111px;" /></strong></p>

<ul>
	<li>Use the path <code>0 &rarr; 1</code> (cost 3).</li>
	<li>At node 1 reverse the original edge <code>3 &rarr; 1</code> into <code>1 &rarr; 3</code> and traverse it at cost <code>2 * 1 = 2</code>.</li>
	<li>Total cost is <code>3 + 2 = 5</code>.</li>
</ul>
</div>

<p><strong class="example">Example 2:</strong></p>

<div class="example-block">
<p><strong>Input:</strong> <span class="example-io">n = 4, edges = [[0,2,1],[2,1,1],[1,3,1],[2,3,3]]</span></p>

<p><strong>Output:</strong> <span class="example-io">3</span></p>

<p><strong>Explanation:</strong></p>

<ul>
	<li>No reversal is needed. Take the path <code>0 &rarr; 2</code> (cost 1), then <code>2 &rarr; 1</code> (cost 1), then <code>1 &rarr; 3</code> (cost 1).</li>
	<li>Total cost is <code>1 + 1 + 1 = 3</code>.</li>
</ul>
</div>

<p>&nbsp;</p>
<p><strong>Constraints:</strong></p>

<ul>
	<li><code>2 &lt;= n &lt;= 5 * 10<sup>4</sup></code></li>
	<li><code>1 &lt;= edges.length &lt;= 10<sup>5</sup></code></li>
	<li><code>edges[i] = [u<sub>i</sub>, v<sub>i</sub>, w<sub>i</sub>]</code></li>
	<li><code>0 &lt;= u<sub>i</sub>, v<sub>i</sub> &lt;= n - 1</code></li>
	<li><code>1 &lt;= w<sub>i</sub> &lt;= 1000</code></li>
</ul>

<!-- description:end -->

## Solutions

<!-- solution:start -->

### Solution 1: Dijkstra's Algorithm

According to the problem description, we can construct a directed graph $g$ where each edge $(u, v)$ allows for two types of traversal:

- Direct traversal with cost $w$, corresponding to edge $(u, v)$.
- Reverse traversal with cost $2w$, corresponding to edge $(v, u)$.

Then, we can use Dijkstra's algorithm on graph $g$ to find the shortest path from node $0$ to node $n-1$, which corresponds to the minimum total cost required.

Specifically, we define a priority queue $pq$, where each element is a tuple $(d, u)$, indicating that the current minimum cost to reach node $u$ is $d$. We also define an array $\textit{dist}$, where $\textit{dist}[u]$ represents the minimum cost from node $0$ to node $u$. Initially, we set $\textit{dist}[0] = 0$, and the costs for all other nodes to infinity, then push $(0, 0)$ into the queue.

In each iteration, we extract the node $(d, u)$ with the minimum cost from the priority queue. If $d$ is greater than $\textit{dist}[u]$, we skip this node. Otherwise, we traverse all neighbors $v$ of node $u$, calculating the new cost $nd = d + w$ to reach node $v$ via node $u$. If $nd$ is less than $\textit{dist}[v]$, we update $\textit{dist}[v] = nd$ and push $(nd, v)$ into the queue.

When we extract node $n-1$, the current $d$ is the minimum total cost from node $0$ to node $n-1$. If the priority queue becomes empty and node $n-1$ has not been extracted, it implies that node $n-1$ is unreachable, so we return -1.

The time complexity is $O(n + m \times \log m)$, and the space complexity is $O(n + m)$. Here, $n$ and $m$ refer to the number of nodes and edges, respectively.

<!-- tabs:start -->

#### Java

```java
class Solution {
    public int minCost(int n, int[][] edges) {
        List<int[]>[] g = new ArrayList[n];
        Arrays.setAll(g, k -> new ArrayList<>());
        for (int[] e : edges) {
            int u = e[0], v = e[1], w = e[2];
            g[u].add(new int[] {v, w});
            g[v].add(new int[] {u, w * 2});
        }

        final int inf = Integer.MAX_VALUE / 2;
        int[] dist = new int[n];
        Arrays.fill(dist, inf);
        dist[0] = 0;

        PriorityQueue<int[]> pq = new PriorityQueue<>(Comparator.comparingInt(a -> a[0]));
        pq.offer(new int[] {0, 0});

        while (!pq.isEmpty()) {
            int[] cur = pq.poll();
            int d = cur[0], u = cur[1];
            if (d > dist[u]) {
                continue;
            }
            if (u == n - 1) {
                return d;
            }
            for (int[] nei : g[u]) {
                int v = nei[0], w = nei[1];
                int nd = d + w;
                if (nd < dist[v]) {
                    dist[v] = nd;
                    pq.offer(new int[] {nd, v});
                }
            }
        }
        return -1;
    }
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
