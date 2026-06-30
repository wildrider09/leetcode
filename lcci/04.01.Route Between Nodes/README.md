---
comments: true
difficulty: Medium
edit_url: https://github.com/doocs/leetcode/edit/main/lcci/04.01.Route%20Between%20Nodes/README_EN.md
---

<!-- problem:start -->

# [04.01. Route Between Nodes](https://leetcode.cn/problems/route-between-nodes-lcci)

[Chinese Version](/lcci/04.01.Route%20Between%20Nodes/README.md)

## Description

<!-- description:start -->

<p>Given a directed graph, design an algorithm to find out whether there is a route between two nodes.</p>

<p><strong>Example1:</strong></p>

<pre>

<strong> Input</strong>: n = 3, graph = [[0, 1], [0, 2], [1, 2], [1, 2]], start = 0, target = 2

<strong> Output</strong>: true

</pre>

<p><strong>Example2:</strong></p>

<pre>

<strong> Input</strong>: n = 5, graph = [[0, 1], [0, 2], [0, 4], [0, 4], [0, 1], [1, 3], [1, 4], [1, 3], [2, 3], [3, 4]], start = 0, target = 4

<strong> Output</strong> true

</pre>

<p><strong>Note: </strong></p>

<ol>
	<li><code>0 &lt;= n &lt;= 100000</code></li>
	<li>All node numbers are within the range [0, n].</li>
	<li>There might be self cycles and duplicated edges.</li>
</ol>

<!-- description:end -->

## Solutions

<!-- solution:start -->

### Solution 1: DFS

First, we construct an adjacency list $g$ based on the given graph, where $g[i]$ represents all the neighboring nodes of node $i$. We use a hash table or array $vis$ to record the visited nodes, and then start a depth-first search from node $start$. If we search to node $target$, we return `true`, otherwise we return `false`.

The process of depth-first search is as follows:

1. If the current node $i$ equals the target node $target$, return `true`.
2. If the current node $i$ has been visited, return `false`.
3. Otherwise, mark the current node $i$ as visited, and then traverse all the neighboring nodes $j$ of node $i$, and recursively search node $j$.

The time complexity is $O(n + m)$, and the space complexity is $O(n + m)$, where $n$ and $m$ are the number of nodes and edges respectively.

<!-- tabs:start -->

#### Java

```java
class Solution {
    private List<Integer>[] g;
    private boolean[] vis;
    private int target;

    public boolean findWhetherExistsPath(int n, int[][] graph, int start, int target) {
        vis = new boolean[n];
        g = new List[n];
        this.target = target;
        Arrays.setAll(g, k -> new ArrayList<>());
        for (int[] e : graph) {
            g[e[0]].add(e[1]);
        }
        return dfs(start);
    }

    private boolean dfs(int i) {
        if (i == target) {
            return true;
        }
        if (vis[i]) {
            return false;
        }
        vis[i] = true;
        for (int j : g[i]) {
            if (dfs(j)) {
                return true;
            }
        }
        return false;
    }
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- solution:start -->

### Solution 2: BFS

Similar to Solution 1, we first construct an adjacency list $g$ based on the given graph, where $g[i]$ represents all the neighboring nodes of node $i$. We use a hash table or array $vis$ to record the visited nodes, and then start a breadth-first search from node $start$. If we search to node $target$, we return `true`, otherwise we return `false`.

The time complexity is $O(n + m)$, and the space complexity is $O(n + m)$, where $n$ and $m$ are the number of nodes and edges respectively.

<!-- tabs:start -->

#### Java

```java
class Solution {
    public boolean findWhetherExistsPath(int n, int[][] graph, int start, int target) {
        List<Integer>[] g = new List[n];
        Arrays.setAll(g, k -> new ArrayList<>());
        boolean[] vis = new boolean[n];
        for (int[] e : graph) {
            g[e[0]].add(e[1]);
        }
        Deque<Integer> q = new ArrayDeque<>();
        q.offer(start);
        vis[start] = true;
        while (!q.isEmpty()) {
            int i = q.poll();
            if (i == target) {
                return true;
            }
            for (int j : g[i]) {
                if (!vis[j]) {
                    q.offer(j);
                    vis[j] = true;
                }
            }
        }
        return false;
    }
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
