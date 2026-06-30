---
comments: true
difficulty: Hard
edit_url: https://github.com/doocs/leetcode/edit/main/solution/3200-3299/3283.Maximum%20Number%20of%20Moves%20to%20Kill%20All%20Pawns/README_EN.md
rating: 2473
source: Weekly Contest 414 Q4
tags:
    - Bit Manipulation
    - Breadth-First Search
    - Array
    - Math
    - Bitmask
    - Game Theory
---

<!-- problem:start -->

# [3283. Maximum Number of Moves to Kill All Pawns](https://leetcode.com/problems/maximum-number-of-moves-to-kill-all-pawns)

[Chinese Version](/solution/3200-3299/3283.Maximum%20Number%20of%20Moves%20to%20Kill%20All%20Pawns/README.md)

## Description

<!-- description:start -->

<p>There is a <code>50 x 50</code> chessboard with <strong>one</strong> knight and some pawns on it. You are given two integers <code>kx</code> and <code>ky</code> where <code>(kx, ky)</code> denotes the position of the knight, and a 2D array <code>positions</code> where <code>positions[i] = [x<sub>i</sub>, y<sub>i</sub>]</code> denotes the position of the pawns on the chessboard.</p>

<p>Alice and Bob play a <em>turn-based</em> game, where Alice goes first. In each player&#39;s turn:</p>

<ul>
	<li>The player <em>selects </em>a pawn that still exists on the board and captures it with the knight in the <strong>fewest</strong> possible <strong>moves</strong>. <strong>Note</strong> that the player can select <strong>any</strong> pawn, it <strong>might not</strong> be one that can be captured in the <strong>least</strong> number of moves.</li>
	<li><span>In the process of capturing the <em>selected</em> pawn, the knight <strong>may</strong> pass other pawns <strong>without</strong> capturing them</span>. <strong>Only</strong> the <em>selected</em> pawn can be captured in <em>this</em> turn.</li>
</ul>

<p>Alice is trying to <strong>maximize</strong> the <strong>sum</strong> of the number of moves made by <em>both</em> players until there are no more pawns on the board, whereas Bob tries to <strong>minimize</strong> them.</p>

<p>Return the <strong>maximum</strong> <em>total</em> number of moves made during the game that Alice can achieve, assuming both players play <strong>optimally</strong>.</p>

<p>Note that in one <strong>move, </strong>a chess knight has eight possible positions it can move to, as illustrated below. Each move is two cells in a cardinal direction, then one cell in an orthogonal direction.</p>

<p><img src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/3200-3299/3283.Maximum%20Number%20of%20Moves%20to%20Kill%20All%20Pawns/images/chess_knight.jpg" style="width: 275px; height: 273px;" /></p>

<p>&nbsp;</p>
<p><strong class="example">Example 1:</strong></p>

<div class="example-block">
<p><strong>Input:</strong> <span class="example-io">kx = 1, ky = 1, positions = [[0,0]]</span></p>

<p><strong>Output:</strong> <span class="example-io">4</span></p>

<p><strong>Explanation:</strong></p>

<p><img alt="" src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/3200-3299/3283.Maximum%20Number%20of%20Moves%20to%20Kill%20All%20Pawns/images/gif3.gif" style="width: 275px; height: 275px;" /></p>

<p>The knight takes 4 moves to reach the pawn at <code>(0, 0)</code>.</p>
</div>

<p><strong class="example">Example 2:</strong></p>

<div class="example-block">
<p><strong>Input:</strong> <span class="example-io">kx = 0, ky = 2, positions = [[1,1],[2,2],[3,3]]</span></p>

<p><strong>Output:</strong> <span class="example-io">8</span></p>

<p><strong>Explanation:</strong></p>

<p><strong><img alt="" src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/3200-3299/3283.Maximum%20Number%20of%20Moves%20to%20Kill%20All%20Pawns/images/gif4.gif" style="width: 320px; height: 320px;" /></strong></p>

<ul>
	<li>Alice picks the pawn at <code>(2, 2)</code> and captures it in two moves: <code>(0, 2) -&gt; (1, 4) -&gt; (2, 2)</code>.</li>
	<li>Bob picks the pawn at <code>(3, 3)</code> and captures it in two moves: <code>(2, 2) -&gt; (4, 1) -&gt; (3, 3)</code>.</li>
	<li>Alice picks the pawn at <code>(1, 1)</code> and captures it in four moves: <code>(3, 3) -&gt; (4, 1) -&gt; (2, 2) -&gt; (0, 3) -&gt; (1, 1)</code>.</li>
</ul>
</div>

<p><strong class="example">Example 3:</strong></p>

<div class="example-block">
<p><strong>Input:</strong> <span class="example-io">kx = 0, ky = 0, positions = [[1,2],[2,4]]</span></p>

<p><strong>Output:</strong> <span class="example-io">3</span></p>

<p><strong>Explanation:</strong></p>

<ul>
	<li>Alice picks the pawn at <code>(2, 4)</code> and captures it in two moves: <code>(0, 0) -&gt; (1, 2) -&gt; (2, 4)</code>. Note that the pawn at <code>(1, 2)</code> is not captured.</li>
	<li>Bob picks the pawn at <code>(1, 2)</code> and captures it in one move: <code>(2, 4) -&gt; (1, 2)</code>.</li>
</ul>
</div>

<p>&nbsp;</p>
<p><strong>Constraints:</strong></p>

<ul>
	<li><code>0 &lt;= kx, ky &lt;= 49</code></li>
	<li><code>1 &lt;= positions.length &lt;= 15</code></li>
	<li><code>positions[i].length == 2</code></li>
	<li><code>0 &lt;= positions[i][0], positions[i][1] &lt;= 49</code></li>
	<li>All <code>positions[i]</code> are unique.</li>
	<li>The input is generated such that <code>positions[i] != [kx, ky]</code> for all <code>0 &lt;= i &lt; positions.length</code>.</li>
</ul>

<!-- description:end -->

## Solutions

<!-- solution:start -->

### Solution 1: BFS + State Compression + Memoization

First, we preprocess the shortest distance for each pawn to any position on the chessboard and record it in the array $\textit{dist}$, where $\textit{dist}[i][x][y]$ represents the shortest distance for the $i$-th pawn to the position $(x, y)$.

Next, we design a function $\text{dfs}(\textit{last}, \textit{state}, \textit{k})$, where $\textit{last}$ represents the number of the last pawn eaten, $\textit{state}$ represents the current state of the remaining pawns, and $\textit{k}$ represents whether it is Alice's or Bob's turn. The function returns the maximum number of moves for the current turn. The answer is $\text{dfs}(n, 2^n-1, 1)$. Here, initially, the number of the last pawn eaten is $n$, which is also the position of the knight.

The specific implementation of the function $\text{dfs}$ is as follows:

- If $\textit{state} = 0$, it means there are no pawns left, return $0$;
- If $\textit{k} = 1$, it means it is Alice's turn. We need to find a pawn such that the number of moves after eating this pawn is maximized, i.e., $\text{dfs}(i, \textit{state} \oplus 2^i, \textit{k} \oplus 1) + \textit{dist}[\textit{last}][x][y]$;
- If $\textit{k} = 0$, it means it is Bob's turn. We need to find a pawn such that the number of moves after eating this pawn is minimized, i.e., $\text{dfs}(i, \textit{state} \oplus 2^i, \textit{k} \oplus 1) + \textit{dist}[\textit{last}][x][y]$.

To avoid repeated calculations, we use memoization, i.e., using a hash table to record the states that have already been calculated.

The time complexity is $O(n \times m^2 + 2^n \times n^2)$, and the space complexity is $O(n \times m^2 + 2^n \times n)$. Here, $n$ and $m$ represent the number of pawns and the size of the chessboard, respectively.

<!-- tabs:start -->

#### Java

```java
class Solution {
    private Integer[][][] f;
    private Integer[][][] dist;
    private int[][] positions;
    private final int[] dx = {1, 1, 2, 2, -1, -1, -2, -2};
    private final int[] dy = {2, -2, 1, -1, 2, -2, 1, -1};

    public int maxMoves(int kx, int ky, int[][] positions) {
        int n = positions.length;
        final int m = 50;
        dist = new Integer[n + 1][m][m];
        this.positions = positions;
        for (int i = 0; i <= n; ++i) {
            int x = i < n ? positions[i][0] : kx;
            int y = i < n ? positions[i][1] : ky;
            Deque<int[]> q = new ArrayDeque<>();
            q.offer(new int[] {x, y});
            for (int step = 1; !q.isEmpty(); ++step) {
                for (int k = q.size(); k > 0; --k) {
                    var p = q.poll();
                    int x1 = p[0], y1 = p[1];
                    for (int j = 0; j < 8; ++j) {
                        int x2 = x1 + dx[j], y2 = y1 + dy[j];
                        if (x2 >= 0 && x2 < m && y2 >= 0 && y2 < m && dist[i][x2][y2] == null) {
                            dist[i][x2][y2] = step;
                            q.offer(new int[] {x2, y2});
                        }
                    }
                }
            }
        }
        f = new Integer[n + 1][1 << n][2];
        return dfs(n, (1 << n) - 1, 1);
    }

    private int dfs(int last, int state, int k) {
        if (state == 0) {
            return 0;
        }
        if (f[last][state][k] != null) {
            return f[last][state][k];
        }
        int res = k == 1 ? 0 : Integer.MAX_VALUE;
        for (int i = 0; i < positions.length; ++i) {
            int x = positions[i][0], y = positions[i][1];
            if ((state >> i & 1) == 1) {
                int t = dfs(i, state ^ (1 << i), k ^ 1) + dist[last][x][y];
                res = k == 1 ? Math.max(res, t) : Math.min(res, t);
            }
        }
        return f[last][state][k] = res;
    }
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
