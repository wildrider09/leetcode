---
comments: true
difficulty: Medium
edit_url: https://github.com/doocs/leetcode/edit/main/solution/1100-1199/1197.Minimum%20Knight%20Moves/README_EN.md
rating: 1722
source: Biweekly Contest 9 Q2
tags:
    - Breadth-First Search
---

<!-- problem:start -->

# [1197. Minimum Knight Moves 🔒](https://leetcode.com/problems/minimum-knight-moves)

[Chinese Version](/solution/1100-1199/1197.Minimum%20Knight%20Moves/README.md)

## Description

<!-- description:start -->

<p>In an <strong>infinite</strong> chess board with coordinates from <code>-infinity</code> to <code>+infinity</code>, you have a <strong>knight</strong> at square <code>[0, 0]</code>.</p>

<p>A knight has 8 possible moves it can make, as illustrated below. Each move is two squares in a cardinal direction, then one square in an orthogonal direction.</p>
<img src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/1100-1199/1197.Minimum%20Knight%20Moves/images/knight.png" style="height: 250px; width: 250px;" />
<p>Return <em>the minimum number of steps needed to move the knight to the square</em> <code>[x, y]</code>. It is guaranteed the answer exists.</p>

<p>&nbsp;</p>
<p><strong class="example">Example 1:</strong></p>

<pre>
<strong>Input:</strong> x = 2, y = 1
<strong>Output:</strong> 1
<strong>Explanation: </strong>[0, 0] &rarr; [2, 1]
</pre>

<p><strong class="example">Example 2:</strong></p>

<pre>
<strong>Input:</strong> x = 5, y = 5
<strong>Output:</strong> 4
<strong>Explanation: </strong>[0, 0] &rarr; [2, 1] &rarr; [4, 2] &rarr; [3, 4] &rarr; [5, 5]
</pre>

<p>&nbsp;</p>
<p><strong>Constraints:</strong></p>

<ul>
	<li><code>-300 &lt;= x, y &lt;= 300</code></li>
	<li><code>0 &lt;= |x| + |y| &lt;= 300</code></li>
</ul>

<!-- description:end -->

## Solutions

<!-- solution:start -->

### Solution 1: BFS

This problem can be solved using the BFS shortest path model. The search space for this problem is not large, so we can directly use the naive BFS. The solution below also provides the code for bidirectional BFS for reference.

Bidirectional BFS is a common optimization method for BFS. The main implementation ideas are as follows:

1. Create two queues, q1 and q2, for "start -> end" and "end -> start" search directions, respectively.
2. Create two hash maps, m1 and m2, to record the visited nodes and their corresponding expansion times (steps).
3. During each search, prioritize the queue with fewer elements for search expansion. If a node visited from the other direction is found during the expansion, it means the shortest path has been found.
4. If one of the queues is empty, it means that the search in the current direction cannot continue, indicating that the start and end points are not connected, and there is no need to continue the search.

<!-- tabs:start -->

#### Java

```java
class Solution {
    public int minKnightMoves(int x, int y) {
        x += 310;
        y += 310;
        int ans = 0;
        Queue<int[]> q = new ArrayDeque<>();
        q.offer(new int[] {310, 310});
        boolean[][] vis = new boolean[700][700];
        vis[310][310] = true;
        int[][] dirs = {{-2, 1}, {-1, 2}, {1, 2}, {2, 1}, {2, -1}, {1, -2}, {-1, -2}, {-2, -1}};
        while (!q.isEmpty()) {
            for (int k = q.size(); k > 0; --k) {
                int[] p = q.poll();
                if (p[0] == x && p[1] == y) {
                    return ans;
                }
                for (int[] dir : dirs) {
                    int c = p[0] + dir[0];
                    int d = p[1] + dir[1];
                    if (!vis[c][d]) {
                        vis[c][d] = true;
                        q.offer(new int[] {c, d});
                    }
                }
            }
            ++ans;
        }
        return -1;
    }
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- solution:start -->

### Solution 2

<!-- tabs:start -->

#### Java

```java
class Solution {
    private int n = 700;

    public int minKnightMoves(int x, int y) {
        if (x == 0 && y == 0) {
            return 0;
        }
        x += 310;
        y += 310;
        Map<Integer, Integer> m1 = new HashMap<>();
        Map<Integer, Integer> m2 = new HashMap<>();
        m1.put(310 * n + 310, 0);
        m2.put(x * n + y, 0);
        Queue<int[]> q1 = new ArrayDeque<>();
        Queue<int[]> q2 = new ArrayDeque<>();
        q1.offer(new int[] {310, 310});
        q2.offer(new int[] {x, y});
        while (!q1.isEmpty() && !q2.isEmpty()) {
            int t = q1.size() <= q2.size() ? extend(m1, m2, q1) : extend(m2, m1, q2);
            if (t != -1) {
                return t;
            }
        }
        return -1;
    }

    private int extend(Map<Integer, Integer> m1, Map<Integer, Integer> m2, Queue<int[]> q) {
        int[][] dirs = {{-2, 1}, {-1, 2}, {1, 2}, {2, 1}, {2, -1}, {1, -2}, {-1, -2}, {-2, -1}};
        for (int k = q.size(); k > 0; --k) {
            int[] p = q.poll();
            int step = m1.get(p[0] * n + p[1]);
            for (int[] dir : dirs) {
                int x = p[0] + dir[0];
                int y = p[1] + dir[1];
                if (m1.containsKey(x * n + y)) {
                    continue;
                }
                if (m2.containsKey(x * n + y)) {
                    return step + 1 + m2.get(x * n + y);
                }
                m1.put(x * n + y, step + 1);
                q.offer(new int[] {x, y});
            }
        }
        return -1;
    }
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
