---
comments: true
difficulty: Medium
edit_url: https://github.com/doocs/leetcode/edit/main/lcci/08.14.Boolean%20Evaluation/README_EN.md
---

<!-- problem:start -->

# [08.14. Boolean Evaluation](https://leetcode.cn/problems/boolean-evaluation-lcci)

[Chinese Version](/lcci/08.14.Boolean%20Evaluation/README.md)

## Description

<!-- description:start -->

<p>Given a boolean expression consisting of the symbols <code>0</code> (false), <code>1</code> (true), <code>&amp;</code> (AND), <code>|</code> (OR), and <code>^</code>&nbsp;(XOR), and a desired boolean result value result, implement a function to count the number of ways of parenthesizing the expression such that it evaluates to result.</p>

<p><strong>Example 1:</strong></p>

<pre>

<strong>Input: </strong>s = &quot;1^0|0|1&quot;, result = 0

<strong>Output: </strong>2

<strong>Explanation:</strong>&nbsp;Two possible parenthesizing ways are:

1^(0|(0|1))

1^((0|0)|1)

</pre>

<p><strong>Example 2:</strong></p>

<pre>

<strong>Input: </strong>s = &quot;0&amp;0&amp;0&amp;1^1|0&quot;, result = 1

<strong>Output: </strong>10</pre>

<p><strong>Note: </strong></p>

<ul>
	<li>There are no more than&nbsp;19 operators in <code>s</code>.</li>
</ul>

<!-- description:end -->

## Solutions

<!-- solution:start -->

### Solution 1

<!-- tabs:start -->

#### Java

```java
class Solution {
    private Map<String, int[]> memo;

    public int countEval(String s, int result) {
        memo = new HashMap<>();
        int[] ans = dfs(s);
        return result == 0 || result == 1 ? ans[result] : 0;
    }

    private int[] dfs(String s) {
        if (memo.containsKey(s)) {
            return memo.get(s);
        }
        int[] res = new int[2];
        if (s.length() == 1) {
            res[Integer.parseInt(s)] = 1;
            return res;
        }
        for (int k = 0; k < s.length(); ++k) {
            char op = s.charAt(k);
            if (op == '&' || op == '|' || op == '^') {
                int[] left = dfs(s.substring(0, k));
                int[] right = dfs(s.substring(k + 1));
                for (int i = 0; i < 2; ++i) {
                    for (int j = 0; j < 2; ++j) {
                        int v = 0;
                        if (op == '&') {
                            v = i & j;
                        } else if (op == '|') {
                            v = i | j;
                        } else if (op == '^') {
                            v = i ^ j;
                        }
                        res[v] += left[i] * right[j];
                    }
                }
            }
        }
        memo.put(s, res);
        return res;
    }
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
