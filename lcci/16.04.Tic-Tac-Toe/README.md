---
comments: true
difficulty: Medium
edit_url: https://github.com/doocs/leetcode/edit/main/lcci/16.04.Tic-Tac-Toe/README_EN.md
---

<!-- problem:start -->

# [16.04. Tic-Tac-Toe](https://leetcode.cn/problems/tic-tac-toe-lcci)

[Chinese Version](/lcci/16.04.Tic-Tac-Toe/README.md)

## Description

<!-- description:start -->

<p>Design an algorithm to figure out if someone has won a game of tic-tac-toe.&nbsp;Input is a string array&nbsp;of size N x N, including characters &quot; &quot;, &quot;X&quot; and &quot;O&quot;, where &quot; &quot; represents a empty grid.</p>
<p>The rules of tic-tac-toe are as follows:</p>
<ul>
	<li>Players place characters into an empty grid(&quot; &quot;) in turn.</li>
	<li>The first player always place character &quot;O&quot;, and the second one place &quot;X&quot;.</li>
	<li>Players are only allowed to place characters in empty grid. Replacing a character is not allowed.</li>
	<li>If there is any row, column or diagonal filled with N&nbsp;same characters, the game ends. The player who place the last charater wins.</li>
	<li>When there is no empty grid, the game ends.</li>
	<li>If the game ends, players cannot place any character further.</li>
</ul>
<p>If there is any winner, return the character that the winner used. If there&#39;s a draw, return &quot;Draw&quot;. If the game doesn&#39;t end and there is no winner, return &quot;Pending&quot;.</p>
<p><strong>Example 1: </strong></p>
<pre>

<strong>Input: </strong> board = [&quot;O X&quot;,&quot; XO&quot;,&quot;X O&quot;]

<strong>Output: </strong> &quot;X&quot;

</pre>
<p><strong>Example 2: </strong></p>
<pre>

<strong>Input: </strong> board = [&quot;OOX&quot;,&quot;XXO&quot;,&quot;OXO&quot;]

<strong>Output: </strong> &quot;Draw&quot;

<strong>Explanation: </strong> no player wins and no empty grid left

</pre>
<p><strong>Example 3: </strong></p>
<pre>

<strong>Input: </strong> board = [&quot;OOX&quot;,&quot;XXO&quot;,&quot;OX &quot;]

<strong>Output: </strong> &quot;Pending&quot;

<strong>Explanation: </strong> no player wins but there is still a empty grid

</pre>
<p><strong>Note: </strong></p>
<ul>
	<li><code>1 &lt;= board.length == board[i].length &lt;= 100</code></li>
	<li>Input follows the rules.</li>
</ul>

<!-- description:end -->

## Solutions

<!-- solution:start -->

### Solution 1: Counting

For each cell, if it is `X`, we can add $1$ to the count; if it is `O`, we can subtract $1$ from the count. When the absolute value of the count of a row, column, or diagonal equals $n$, it means that the current player has placed $n$ identical characters in that row, column, or diagonal, and the game is over. We can return the corresponding character.

Specifically, we use a one-dimensional array $rows$ and $cols$ of length $n$ to represent the count of each row and column, and use $dg$ and $udg$ to represent the count of the two diagonals. When a player places a character at $(i, j)$, we update the corresponding elements in the arrays $rows$, $cols$, $dg$, and $udg$ based on whether the character is `X` or `O`. After each update, we check whether the absolute value of the corresponding element equals $n$. If it does, it means that the current player has placed $n$ identical characters in that row, column, or diagonal, and the game is over. We can return the corresponding character.

Finally, we traverse the entire board. If there is a character ` `, it means that the game is not over yet, and we return `Pending`. Otherwise, we return `Draw`.

The time complexity is $O(n^2)$, and the space complexity is $O(n)$, where $n$ is the side length of the board.

<!-- tabs:start -->

#### Java

```java
class Solution {
    public String tictactoe(String[] board) {
        int n = board.length;
        int[] rows = new int[n];
        int[] cols = new int[n];
        int dg = 0, udg = 0;
        boolean hasEmptyGrid = false;
        for (int i = 0; i < n; ++i) {
            for (int j = 0; j < n; ++j) {
                char c = board[i].charAt(j);
                if (c == ' ') {
                    hasEmptyGrid = true;
                    continue;
                }
                int v = c == 'X' ? 1 : -1;
                rows[i] += v;
                cols[j] += v;
                if (i == j) {
                    dg += v;
                }
                if (i + j + 1 == n) {
                    udg += v;
                }
                if (Math.abs(rows[i]) == n || Math.abs(cols[j]) == n || Math.abs(dg) == n
                    || Math.abs(udg) == n) {
                    return String.valueOf(c);
                }
            }
        }
        return hasEmptyGrid ? "Pending" : "Draw";
    }
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
