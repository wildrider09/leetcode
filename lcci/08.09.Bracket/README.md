---
comments: true
difficulty: Medium
edit_url: https://github.com/doocs/leetcode/edit/main/lcci/08.09.Bracket/README_EN.md
---

<!-- problem:start -->

# [08.09. Bracket](https://leetcode.cn/problems/bracket-lcci)

[Chinese Version](/lcci/08.09.Bracket/README.md)

## Description

<!-- description:start -->

<p>Implement an algorithm to print all valid (e.g., properly opened and closed) combinations of n pairs of parentheses.</p>

<p>Note: The result set should not contain duplicated subsets.</p>

<p>For example, given&nbsp;n = 3, the result should be:</p>

<pre>

[

  &quot;((()))&quot;,

  &quot;(()())&quot;,

  &quot;(())()&quot;,

  &quot;()(())&quot;,

  &quot;()()()&quot;

]

</pre>

<!-- description:end -->

## Solutions

<!-- solution:start -->

### Solution 1: DFS + Pruning

The range of $n$ in the problem is $[1, 8]$, so we can directly solve this problem quickly through "brute force search + pruning".

We design a function `dfs(l, r, t)`, where $l$ and $r$ represent the number of left and right parentheses respectively, and $t$ represents the current parentheses sequence. Then we can get the following recursive structure:

- If $l > n$ or $r > n$ or $l < r$, then the current parentheses combination $t$ is illegal, return directly;
- If $l = n$ and $r = n$, then the current parentheses combination $t$ is legal, add it to the answer array `ans`, and return directly;
- We can choose to add a left parenthesis, and recursively execute `dfs(l + 1, r, t + "(")`;
- We can also choose to add a right parenthesis, and recursively execute `dfs(l, r + 1, t + ")")`.

The time complexity is $O(2^{n\times 2} \times n)$, and the space complexity is $O(n)$.

<!-- tabs:start -->

#### Java

```java
class Solution {
    private List<String> ans = new ArrayList<>();
    private int n;

    public List<String> generateParenthesis(int n) {
        this.n = n;
        dfs(0, 0, "");
        return ans;
    }

    private void dfs(int l, int r, String t) {
        if (l > n || r > n || l < r) {
            return;
        }
        if (l == n && r == n) {
            ans.add(t);
            return;
        }
        dfs(l + 1, r, t + "(");
        dfs(l, r + 1, t + ")");
    }
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
