---
comments: true
difficulty: Medium
edit_url: https://github.com/doocs/leetcode/edit/main/solution/2400-2499/2473.Minimum%20Cost%20to%20Buy%20Apples/README_EN.md
tags:
    - Graph
    - Array
    - Shortest Path
    - Heap (Priority Queue)
---

<!-- problem:start -->

# [2473. Minimum Cost to Buy Apples 🔒](https://leetcode.com/problems/minimum-cost-to-buy-apples)

[Chinese Version](/solution/2400-2499/2473.Minimum%20Cost%20to%20Buy%20Apples/README.md)

## Description

<!-- description:start -->

<p>You are given a positive integer <code>n</code> representing <code>n</code> cities numbered from <code>1</code> to <code>n</code>. You are also given a <strong>2D</strong> array <code>roads</code>, where <code>roads[i] = [a<sub>i</sub>, b<sub>i</sub>, cost<sub>i</sub>]</code> indicates that there is a <strong>bidirectional </strong>road between cities <code>a<sub>i</sub></code> and <code>b<sub>i</sub></code> with a cost of traveling equal to <code>cost<sub>i</sub></code>.</p>

<p>You can buy apples in <strong>any</strong> city you want, but some cities have different costs to buy apples. You are given the 1-based array <code>appleCost</code> where <code>appleCost[i]</code> is the cost of buying one apple from city <code>i</code>.</p>

<p>You start at some city, traverse through various roads, and eventually buy <strong>exactly</strong> one apple from <strong>any</strong> city. After you buy that apple, you have to return back to the city you <strong>started</strong> at, but now the cost of all the roads will be <strong>multiplied</strong> by a given factor <code>k</code>.</p>

<p>Given the integer <code>k</code>, return <em>a 1-based array </em><code>answer</code><em> of size </em><code>n</code><em> where </em><code>answer[i]</code><em> is the <strong>minimum</strong> total cost to buy an apple if you start at city </em><code>i</code>.</p>

<p>&nbsp;</p>
<p><strong class="example">Example 1:</strong></p>
<img alt="" src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/2400-2499/2473.Minimum%20Cost%20to%20Buy%20Apples/images/graph55.png" style="width: 241px; height: 309px;" />
<pre>
<strong>Input:</strong> n = 4, roads = [[1,2,4],[2,3,2],[2,4,5],[3,4,1],[1,3,4]], appleCost = [56,42,102,301], k = 2
<strong>Output:</strong> [54,42,48,51]
<strong>Explanation:</strong> The minimum cost for each starting city is the following:
- Starting at city 1: You take the path 1 -&gt; 2, buy an apple at city 2, and finally take the path 2 -&gt; 1. The total cost is 4 + 42 + 4 * 2 = 54.
- Starting at city 2: You directly buy an apple at city 2. The total cost is 42.
- Starting at city 3: You take the path 3 -&gt; 2, buy an apple at city 2, and finally take the path 2 -&gt; 3. The total cost is 2 + 42 + 2 * 2 = 48.
- Starting at city 4: You take the path 4 -&gt; 3 -&gt; 2 then you buy at city 2, and finally take the path 2 -&gt; 3 -&gt; 4. The total cost is 1 + 2 + 42 + 1 * 2 + 2 * 2 = 51.
</pre>

<p><strong class="example">Example 2:</strong></p>
<img alt="" src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/2400-2499/2473.Minimum%20Cost%20to%20Buy%20Apples/images/graph4.png" style="width: 167px; height: 309px;" />
<pre>
<strong>Input:</strong> n = 3, roads = [[1,2,5],[2,3,1],[3,1,2]], appleCost = [2,3,1], k = 3
<strong>Output:</strong> [2,3,1]
<strong>Explanation:</strong> It is always optimal to buy the apple in the starting city.
</pre>

<p>&nbsp;</p>
<p><strong>Constraints:</strong></p>

<ul>
	<li><code>2 &lt;= n &lt;= 1000</code></li>
	<li><code>1 &lt;= roads.length &lt;= 2000</code></li>
	<li><code>1 &lt;= a<sub>i</sub>, b<sub>i</sub> &lt;= n</code></li>
	<li><code>a<sub>i</sub> != b<sub>i</sub></code></li>
	<li><code>1 &lt;= cost<sub>i</sub> &lt;= 10<sup>5</sup></code></li>
	<li><code>appleCost.length == n</code></li>
	<li><code>1 &lt;= appleCost[i] &lt;= 10<sup>5</sup></code></li>
	<li><code>1 &lt;= k &lt;= 100</code></li>
	<li>There are no repeated edges.</li>
</ul>

<!-- description:end -->

## Solutions

<!-- solution:start -->

### Solution 1: Heap-optimized Dijkstra's Algorithm

We enumerate the starting point, and for each starting point, we use Dijkstra's algorithm to find the shortest distance to all other points, and update the minimum value accordingly.

The time complexity is $O(n \times m \times \log m)$, where $n$ and $m$ are the number of cities and roads, respectively.

<!-- tabs:start -->

#### Java

```java
class Solution {
    private int k;
    private int[] cost;
    private int[] dist;
    private List<int[]>[] g;
    private static final int INF = 0x3f3f3f3f;

    public long[] minCost(int n, int[][] roads, int[] appleCost, int k) {
        cost = appleCost;
        g = new List[n];
        dist = new int[n];
        this.k = k;
        for (int i = 0; i < n; ++i) {
            g[i] = new ArrayList<>();
        }
        for (var e : roads) {
            int a = e[0] - 1, b = e[1] - 1, c = e[2];
            g[a].add(new int[] {b, c});
            g[b].add(new int[] {a, c});
        }
        long[] ans = new long[n];
        for (int i = 0; i < n; ++i) {
            ans[i] = dijkstra(i);
        }
        return ans;
    }

    private long dijkstra(int u) {
        PriorityQueue<int[]> q = new PriorityQueue<>((a, b) -> a[0] - b[0]);
        q.offer(new int[] {0, u});
        Arrays.fill(dist, INF);
        dist[u] = 0;
        long ans = Long.MAX_VALUE;
        while (!q.isEmpty()) {
            var p = q.poll();
            int d = p[0];
            u = p[1];
            ans = Math.min(ans, cost[u] + (long) (k + 1) * d);
            for (var ne : g[u]) {
                int v = ne[0], w = ne[1];
                if (dist[v] > dist[u] + w) {
                    dist[v] = dist[u] + w;
                    q.offer(new int[] {dist[v], v});
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
