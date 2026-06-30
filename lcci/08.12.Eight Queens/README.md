---
comments: true
difficulty: Hard
edit_url: https://github.com/doocs/leetcode/edit/main/lcci/08.12.Eight%20Queens/README_EN.md
---

<!-- problem:start -->

# [08.12. Eight Queens](https://leetcode.cn/problems/eight-queens-lcci)

[Chinese Version](/lcci/08.12.Eight%20Queens/README.md)

## Description

<!-- description:start -->

<p>Write an algorithm to print all ways of arranging n queens on an n x n&nbsp;chess board so that none of them share the same row, column, or diagonal. In this case, &quot;diagonal&quot; means all diagonals, not just the two that bisect the board.</p>

<p><strong>Notes: </strong>This&nbsp;problem is a generalization of the original one in the book.</p>

<p><strong>Example:</strong></p>

<pre>

<strong> Input</strong>: 4

<strong> Output</strong>: [[&quot;.Q..&quot;,&quot;...Q&quot;,&quot;Q...&quot;,&quot;..Q.&quot;],[&quot;..Q.&quot;,&quot;Q...&quot;,&quot;...Q&quot;,&quot;.Q..&quot;]]

<strong> Explanation</strong>: 4 queens has following two solutions

[

&nbsp;[&quot;.Q..&quot;, &nbsp;// solution 1

&nbsp; &quot;...Q&quot;,

&nbsp; &quot;Q...&quot;,

&nbsp; &quot;..Q.&quot;],

&nbsp;[&quot;..Q.&quot;, &nbsp;// solution 2

&nbsp; &quot;Q...&quot;,

&nbsp; &quot;...Q&quot;,

&nbsp; &quot;.Q..&quot;]

]

</pre>

<!-- description:end -->

## Solutions

<!-- solution:start -->

### Solution 1: DFS (Backtracking)

We define three arrays $col$, $dg$, and $udg$ to represent whether there is a queen in the column, the main diagonal, and the anti-diagonal, respectively. If there is a queen at position $(i, j)$, then $col[j]$, $dg[i + j]$, and $udg[n - i + j]$ are all $1$. In addition, we use an array $g$ to record the current state of the chessboard, where all elements in $g$ are initially `'.'`.

Next, we define a function $dfs(i)$, which represents placing queens starting from the $i$th row.

In $dfs(i)$, if $i = n$, it means that we have completed the placement of all queens. We put the current $g$ into the answer array and end the recursion.

Otherwise, we enumerate each column $j$ of the current row. If there is no queen at position $(i, j)$, that is, $col[j]$, $dg[i + j]$, and $udg[n - i + j]$ are all $0$, then we can place a queen, that is, change $g[i][j]$ to `'Q'`, and set $col[j]$, $dg[i + j]$, and $udg[n - i + j]$ to $1$. Then we continue to search the next row, that is, call $dfs(i + 1)$. After the recursion ends, we need to change $g[i][j]$ back to `'.'` and set $col[j]$, $dg[i + j]$, and $udg[n - i + j]$ to $0$.

In the main function, we call $dfs(0)$ to start recursion, and finally return the answer array.

The time complexity is $O(n^2 \times n!)$, and the space complexity is $O(n)$. Here, $n$ is the integer given in the problem.

<!-- tabs:start -->

#### Java

```java
class Solution {
    private List<List<String>> ans = new ArrayList<>();
    private int[] col;
    private int[] dg;
    private int[] udg;
    private String[][] g;
    private int n;

    public List<List<String>> solveNQueens(int n) {
        this.n = n;
        col = new int[n];
        dg = new int[n << 1];
        udg = new int[n << 1];
        g = new String[n][n];
        for (int i = 0; i < n; ++i) {
            Arrays.fill(g[i], ".");
        }
        dfs(0);
        return ans;
    }

    private void dfs(int i) {
        if (i == n) {
            List<String> t = new ArrayList<>();
            for (int j = 0; j < n; ++j) {
                t.add(String.join("", g[j]));
            }
            ans.add(t);
            return;
        }
        for (int j = 0; j < n; ++j) {
            if (col[j] + dg[i + j] + udg[n - i + j] == 0) {
                g[i][j] = "Q";
                col[j] = dg[i + j] = udg[n - i + j] = 1;
                dfs(i + 1);
                col[j] = dg[i + j] = udg[n - i + j] = 0;
                g[i][j] = ".";
            }
        }
    }
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
