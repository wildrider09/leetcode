---
comments: true
difficulty: Easy
edit_url: https://github.com/doocs/leetcode/edit/main/solution/3000-3099/3032.Count%20Numbers%20With%20Unique%20Digits%20II/README_EN.md
tags:
    - Hash Table
    - Math
    - Dynamic Programming
---

<!-- problem:start -->

# [3032. Count Numbers With Unique Digits II 🔒](https://leetcode.com/problems/count-numbers-with-unique-digits-ii)

[Chinese Version](/solution/3000-3099/3032.Count%20Numbers%20With%20Unique%20Digits%20II/README.md)

## Description

<!-- description:start -->

Given two <strong>positive</strong> integers <code>a</code> and <code>b</code>, return <em>the count of numbers having&nbsp;<strong>unique</strong> digits in the range</em> <code>[a, b]</code> <em>(<strong>inclusive</strong>).</em>

<p>&nbsp;</p>
<p><strong class="example">Example 1:</strong></p>

<pre>
<strong>Input:</strong> a = 1, b = 20
<strong>Output:</strong> 19
<strong>Explanation:</strong> All the numbers in the range [1, 20] have unique digits except 11. Hence, the answer is 19.
</pre>

<p><strong class="example">Example 2:</strong></p>

<pre>
<strong>Input:</strong> a = 9, b = 19
<strong>Output:</strong> 10
<strong>Explanation:</strong> All the numbers in the range [9, 19] have unique digits except 11. Hence, the answer is 10. 
</pre>

<p><strong class="example">Example 3:</strong></p>

<pre>
<strong>Input:</strong> a = 80, b = 120
<strong>Output:</strong> 27
<strong>Explanation:</strong> There are 41 numbers in the range [80, 120], 27 of which have unique digits.
</pre>

<p>&nbsp;</p>
<p><strong>Constraints:</strong></p>

<ul>
	<li><code>1 &lt;= a &lt;= b &lt;= 1000</code></li>
</ul>

<!-- description:end -->

## Solutions

<!-- solution:start -->

### Solution 1: State Compression + Digit DP

The problem asks to count how many numbers in the range $[a, b]$ have unique digits. We can solve this problem using state compression and digit DP.

We can use a function $f(n)$ to count how many numbers in the range $[1, n]$ have unique digits. Then the answer is $f(b) - f(a - 1)$.

In addition, we can use a binary number to record the digits that have appeared in the number. For example, if the digits $1, 3, 5$ have appeared in the number, we can use $10101$ to represent this state.

Next, we use memoization search to implement digit DP. We search from the starting point to the bottom layer to get the number of schemes, return the answer layer by layer and accumulate it, and finally get the final answer from the search starting point.

The basic steps are as follows:

1. We convert the number $n$ into a string $num$, where $num[0]$ is the highest digit and $num[len - 1]$ is the lowest digit.
2. Based on the problem information, we design a function $dfs(pos, mask, limit)$, where $pos$ represents the current processing position, $mask$ represents the digits that have appeared in the current number, and $limit$ represents whether there is a limit at the current position. If $limit$ is true, then the digit at the current position cannot exceed $num[pos]$.

The answer is $dfs(0, 0, true)$.

The time complexity is $O(m \times 2^{10} \times 10)$, and the space complexity is $O(m \times 2^{10})$. Where $m$ is the number of digits in $b$.

<!-- tabs:start -->

#### Java

```java
class Solution {
    private String num;
    private Integer[][] f;

    public int numberCount(int a, int b) {
        num = String.valueOf(a - 1);
        f = new Integer[num.length()][1 << 10];
        int x = dfs(0, 0, true);
        num = String.valueOf(b);
        f = new Integer[num.length()][1 << 10];
        int y = dfs(0, 0, true);
        return y - x;
    }

    private int dfs(int pos, int mask, boolean limit) {
        if (pos >= num.length()) {
            return mask > 0 ? 1 : 0;
        }
        if (!limit && f[pos][mask] != null) {
            return f[pos][mask];
        }
        int up = limit ? num.charAt(pos) - '0' : 9;
        int ans = 0;
        for (int i = 0; i <= up; ++i) {
            if ((mask >> i & 1) == 1) {
                continue;
            }
            int nxt = mask == 0 && i == 0 ? 0 : mask | 1 << i;
            ans += dfs(pos + 1, nxt, limit && i == up);
        }
        if (!limit) {
            f[pos][mask] = ans;
        }
        return ans;
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
    public int numberCount(int a, int b) {
        int res = 0;
        for (int i = a; i <= b; ++i) {
            if (isValid(i)) {
                ++res;
            }
        }
        return res;
    }
    private boolean isValid(int n) {
        boolean[] present = new boolean[10];
        Arrays.fill(present, false);
        while (n > 0) {
            int dig = n % 10;
            if (present[dig]) {
                return false;
            }
            present[dig] = true;
            n /= 10;
        }
        return true;
    }
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
