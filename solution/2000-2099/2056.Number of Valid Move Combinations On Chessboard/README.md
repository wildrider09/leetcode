---
comments: true
difficulty: Hard
edit_url: https://github.com/doocs/leetcode/edit/main/solution/2000-2099/2056.Number%20of%20Valid%20Move%20Combinations%20On%20Chessboard/README_EN.md
rating: 2610
source: Biweekly Contest 64 Q4
tags:
    - Array
    - String
    - Backtracking
    - Simulation
---

<!-- problem:start -->

# [2056. Number of Valid Move Combinations On Chessboard](https://leetcode.com/problems/number-of-valid-move-combinations-on-chessboard)

[Chinese Version](/solution/2000-2099/2056.Number%20of%20Valid%20Move%20Combinations%20On%20Chessboard/README.md)

## Description

<!-- description:start -->

<p>There is an <code>8 x 8</code> chessboard containing <code>n</code> pieces (rooks, queens, or bishops). You are given a string array <code>pieces</code> of length <code>n</code>, where <code>pieces[i]</code> describes the type (rook, queen, or bishop) of the <code>i<sup>th</sup></code> piece. In addition, you are given a 2D integer array <code>positions</code> also of length <code>n</code>, where <code>positions[i] = [r<sub>i</sub>, c<sub>i</sub>]</code> indicates that the <code>i<sup>th</sup></code> piece is currently at the <strong>1-based</strong> coordinate <code>(r<sub>i</sub>, c<sub>i</sub>)</code> on the chessboard.</p>

<p>When making a <strong>move</strong> for a piece, you choose a <strong>destination</strong> square that the piece will travel toward and stop on.</p>

<ul>
	<li>A rook can only travel <strong>horizontally or vertically</strong> from <code>(r, c)</code> to the direction of <code>(r+1, c)</code>, <code>(r-1, c)</code>, <code>(r, c+1)</code>, or <code>(r, c-1)</code>.</li>
	<li>A queen can only travel <strong>horizontally, vertically, or diagonally</strong> from <code>(r, c)</code> to the direction of <code>(r+1, c)</code>, <code>(r-1, c)</code>, <code>(r, c+1)</code>, <code>(r, c-1)</code>, <code>(r+1, c+1)</code>, <code>(r+1, c-1)</code>, <code>(r-1, c+1)</code>, <code>(r-1, c-1)</code>.</li>
	<li>A bishop can only travel <strong>diagonally</strong> from <code>(r, c)</code> to the direction of <code>(r+1, c+1)</code>, <code>(r+1, c-1)</code>, <code>(r-1, c+1)</code>, <code>(r-1, c-1)</code>.</li>
</ul>

<p>You must make a <strong>move</strong> for every piece on the board simultaneously. A <strong>move combination</strong> consists of all the <strong>moves</strong> performed on all the given pieces. Every second, each piece will instantaneously travel <strong>one square</strong> towards their destination if they are not already at it. All pieces start traveling at the <code>0<sup>th</sup></code> second. A move combination is <strong>invalid</strong> if, at a given time, <strong>two or more</strong> pieces occupy the same square.</p>

<p>Return <em>the number of <strong>valid</strong> move combinations</em>​​​​​.</p>

<p><strong>Notes:</strong></p>

<ul>
	<li><strong>No two pieces</strong> will start in the<strong> same</strong> square.</li>
	<li>You may choose the square a piece is already on as its <strong>destination</strong>.</li>
	<li>If two pieces are <strong>directly adjacent</strong> to each other, it is valid for them to <strong>move past each other</strong> and swap positions in one second.</li>
</ul>

<p>&nbsp;</p>
<p><strong class="example">Example 1:</strong></p>
<img alt="" src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/2000-2099/2056.Number%20of%20Valid%20Move%20Combinations%20On%20Chessboard/images/a1.png" style="width: 215px; height: 215px;" />
<pre>
<strong>Input:</strong> pieces = [&quot;rook&quot;], positions = [[1,1]]
<strong>Output:</strong> 15
<strong>Explanation:</strong> The image above shows the possible squares the piece can move to.
</pre>

<p><strong class="example">Example 2:</strong></p>
<img alt="" src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/2000-2099/2056.Number%20of%20Valid%20Move%20Combinations%20On%20Chessboard/images/a2.png" style="width: 215px; height: 215px;" />
<pre>
<strong>Input:</strong> pieces = [&quot;queen&quot;], positions = [[1,1]]
<strong>Output:</strong> 22
<strong>Explanation:</strong> The image above shows the possible squares the piece can move to.
</pre>

<p><strong class="example">Example 3:</strong></p>
<img alt="" src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/2000-2099/2056.Number%20of%20Valid%20Move%20Combinations%20On%20Chessboard/images/a3.png" style="width: 214px; height: 215px;" />
<pre>
<strong>Input:</strong> pieces = [&quot;bishop&quot;], positions = [[4,3]]
<strong>Output:</strong> 12
<strong>Explanation:</strong> The image above shows the possible squares the piece can move to.
</pre>

<p>&nbsp;</p>
<p><strong>Constraints:</strong></p>

<ul>
	<li><code>n == pieces.length </code></li>
	<li><code>n == positions.length</code></li>
	<li><code>1 &lt;= n &lt;= 4</code></li>
	<li><code>pieces</code> only contains the strings <code>&quot;rook&quot;</code>, <code>&quot;queen&quot;</code>, and <code>&quot;bishop&quot;</code>.</li>
	<li>There will be at most one queen on the chessboard.</li>
	<li><code>1 &lt;= r<sub>i</sub>, c<sub>i</sub> &lt;= 8</code></li>
	<li>Each <code>positions[i]</code> is distinct.</li>
</ul>

<!-- description:end -->

## Solutions

<!-- solution:start -->

### Solution 1: DFS

The problem has at most $4$ pieces, and each piece can move in up to $8$ directions. We can consider using DFS to search all possible move combinations.

We enumerate each piece in order. For each piece, we can choose not to move or move according to the rules. We use an array $\textit{dist}[i]$ to record the movement of the $i$-th piece, where $\textit{dist}[i][x][y]$ represents the time when the $i$-th piece passes through the coordinate $(x, y)$. We use an array $\textit{end}[i]$ to record the endpoint coordinates and time of the $i$-th piece. During the search, we need to determine whether the current piece can stop moving and whether it can continue moving in the current direction.

We define a method $\text{checkStop}(i, x, y, t)$ to determine whether the $i$-th piece can stop at coordinate $(x, y)$ at time $t$. If for all previous pieces $j$, $\textit{dist}[j][x][y] < t$, then the $i$-th piece can stop moving.

Additionally, we define a method $\text{checkPass}(i, x, y, t)$ to determine whether the $i$-th piece can pass through coordinate $(x, y)$ at time $t$. If any other piece $j$ also passes through coordinate $(x, y)$ at time $t$, or if any other piece $j$ stops at $(x, y)$ and the time does not exceed $t$, then the $i$-th piece cannot pass through coordinate $(x, y)$ at time $t$.

The time complexity is $O((n \times M)^n)$, and the space complexity is $O(n \times M)$. Here, $n$ is the number of pieces, and $M$ is the movement range of each piece.

<!-- tabs:start -->

#### Java

```java
class Solution {
    int n, m = 9, ans;
    int[][][] dist;
    int[][] end;
    String[] pieces;
    int[][] positions;
    int[][] rookDirs = {{1, 0}, {-1, 0}, {0, 1}, {0, -1}};
    int[][] bishopDirs = {{1, 1}, {1, -1}, {-1, 1}, {-1, -1}};
    int[][] queenDirs = {{1, 0}, {-1, 0}, {0, 1}, {0, -1}, {1, 1}, {1, -1}, {-1, 1}, {-1, -1}};

    public int countCombinations(String[] pieces, int[][] positions) {
        n = pieces.length;
        dist = new int[n][m][m];
        end = new int[n][3];
        ans = 0;
        this.pieces = pieces;
        this.positions = positions;

        dfs(0);
        return ans;
    }

    private void dfs(int i) {
        if (i >= n) {
            ans++;
            return;
        }

        int x = positions[i][0], y = positions[i][1];
        resetDist(i);
        dist[i][x][y] = 0;
        end[i] = new int[] {x, y, 0};

        if (checkStop(i, x, y, 0)) {
            dfs(i + 1);
        }

        int[][] dirs = getDirs(pieces[i]);
        for (int[] dir : dirs) {
            resetDist(i);
            dist[i][x][y] = 0;
            int nx = x + dir[0], ny = y + dir[1], nt = 1;

            while (isValid(nx, ny) && checkPass(i, nx, ny, nt)) {
                dist[i][nx][ny] = nt;
                end[i] = new int[] {nx, ny, nt};
                if (checkStop(i, nx, ny, nt)) {
                    dfs(i + 1);
                }
                nx += dir[0];
                ny += dir[1];
                nt++;
            }
        }
    }

    private void resetDist(int i) {
        for (int j = 0; j < m; j++) {
            for (int k = 0; k < m; k++) {
                dist[i][j][k] = -1;
            }
        }
    }

    private boolean checkStop(int i, int x, int y, int t) {
        for (int j = 0; j < i; j++) {
            if (dist[j][x][y] >= t) {
                return false;
            }
        }
        return true;
    }

    private boolean checkPass(int i, int x, int y, int t) {
        for (int j = 0; j < i; j++) {
            if (dist[j][x][y] == t) {
                return false;
            }
            if (end[j][0] == x && end[j][1] == y && end[j][2] <= t) {
                return false;
            }
        }
        return true;
    }

    private boolean isValid(int x, int y) {
        return x >= 1 && x < m && y >= 1 && y < m;
    }

    private int[][] getDirs(String piece) {
        char c = piece.charAt(0);
        return switch (c) {
            case 'r' -> rookDirs;
            case 'b' -> bishopDirs;
            default -> queenDirs;
        };
    }
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
