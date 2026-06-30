---
comments: true
difficulty: Medium
edit_url: https://github.com/doocs/leetcode/edit/main/solution/1200-1299/1208.Get%20Equal%20Substrings%20Within%20Budget/README_EN.md
rating: 1496
source: Weekly Contest 156 Q2
tags:
    - String
    - Binary Search
    - Prefix Sum
    - Sliding Window
---

<!-- problem:start -->

# [1208. Get Equal Substrings Within Budget](https://leetcode.com/problems/get-equal-substrings-within-budget)

[Chinese Version](/solution/1200-1299/1208.Get%20Equal%20Substrings%20Within%20Budget/README.md)

## Description

<!-- description:start -->

<p>You are given two strings <code>s</code> and <code>t</code> of the same length and an integer <code>maxCost</code>.</p>

<p>You want to change <code>s</code> to <code>t</code>. Changing the <code>i<sup>th</sup></code> character of <code>s</code> to <code>i<sup>th</sup></code> character of <code>t</code> costs <code>|s[i] - t[i]|</code> (i.e., the absolute difference between the ASCII values of the characters).</p>

<p>Return <em>the maximum length of a substring of </em><code>s</code><em> that can be changed to be the same as the corresponding substring of </em><code>t</code><em> with a cost less than or equal to </em><code>maxCost</code>. If there is no substring from <code>s</code> that can be changed to its corresponding substring from <code>t</code>, return <code>0</code>.</p>

<p>&nbsp;</p>
<p><strong class="example">Example 1:</strong></p>

<pre>
<strong>Input:</strong> s = &quot;abcd&quot;, t = &quot;bcdf&quot;, maxCost = 3
<strong>Output:</strong> 3
<strong>Explanation:</strong> &quot;abc&quot; of s can change to &quot;bcd&quot;.
That costs 3, so the maximum length is 3.
</pre>

<p><strong class="example">Example 2:</strong></p>

<pre>
<strong>Input:</strong> s = &quot;abcd&quot;, t = &quot;cdef&quot;, maxCost = 3
<strong>Output:</strong> 1
<strong>Explanation:</strong> Each character in s costs 2 to change to character in t,  so the maximum length is 1.
</pre>

<p><strong class="example">Example 3:</strong></p>

<pre>
<strong>Input:</strong> s = &quot;abcd&quot;, t = &quot;acde&quot;, maxCost = 0
<strong>Output:</strong> 1
<strong>Explanation:</strong> You cannot make any change, so the maximum length is 1.
</pre>

<p>&nbsp;</p>
<p><strong>Constraints:</strong></p>

<ul>
	<li><code>1 &lt;= s.length &lt;= 10<sup>5</sup></code></li>
	<li><code>t.length == s.length</code></li>
	<li><code>0 &lt;= maxCost &lt;= 10<sup>6</sup></code></li>
	<li><code>s</code> and <code>t</code> consist of only lowercase English letters.</li>
</ul>

<!-- description:end -->

## Solutions

<!-- solution:start -->

### Solution 1: Prefix Sum + Binary Search

We can create an array $f$ of length $n + 1$, where $f[i]$ represents the sum of the absolute differences of ASCII values between the first $i$ characters of string $s$ and the first $i$ characters of string $t$. Thus, we can calculate the sum of the absolute differences of ASCII values from the $i$-th character to the $j$-th character of string $s$ by $f[j + 1] - f[i]$, where $0 \leq i \leq j < n$.

Note that the length has monotonicity, i.e., if there exists a substring of length $x$ that satisfies the condition, then a substring of length $x - 1$ must also satisfy the condition. Therefore, we can use binary search to find the maximum length.

We define a function $check(x)$, which indicates whether there exists a substring of length $x$ that satisfies the condition. In this function, we only need to enumerate all substrings of length $x$ and check whether they satisfy the condition. If there exists a substring that satisfies the condition, the function returns `true`, otherwise it returns `false`.

Next, we define the left boundary $l$ of binary search as $0$ and the right boundary $r$ as $n$. In each step, we let $mid = \lfloor \frac{l + r + 1}{2} \rfloor$. If the return value of $check(mid)$ is `true`, we update the left boundary to $mid$, otherwise we update the right boundary to $mid - 1$. After the binary search, the left boundary we get is the answer.

The time complexity is $O(n \times \log n)$, and the space complexity is $O(n)$. Here, $n$ is the length of string $s$.

<!-- tabs:start -->

#### Java

```java
class Solution {
    private int maxCost;
    private int[] f;
    private int n;

    public int equalSubstring(String s, String t, int maxCost) {
        n = s.length();
        f = new int[n + 1];
        this.maxCost = maxCost;
        for (int i = 0; i < n; ++i) {
            int x = Math.abs(s.charAt(i) - t.charAt(i));
            f[i + 1] = f[i] + x;
        }
        int l = 0, r = n;
        while (l < r) {
            int mid = (l + r + 1) >>> 1;
            if (check(mid)) {
                l = mid;
            } else {
                r = mid - 1;
            }
        }
        return l;
    }

    private boolean check(int x) {
        for (int i = 0; i + x - 1 < n; ++i) {
            int j = i + x - 1;
            if (f[j + 1] - f[i] <= maxCost) {
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

### Solution 2: Two Pointers

We can maintain two pointers $l$ and $r$, initially $l = r = 0$; maintain a variable $\textit{cost}$, which represents the sum of the absolute values of the ASCII code differences in the index interval $[l,..r]$. In each step, we move $r$ to the right by one position, then update $\textit{cost} = \textit{cost} + |s[r] - t[r]|$. If $\textit{cost} \gt \textit{maxCost}$, then we loop to move $l$ to the right by one position, and decrease the value of $\textit{cost}$, until $\textit{cost} \leq \textit{maxCost}$. Then we update the answer, that is, $\textit{ans} = \max(\textit{ans}, r - l + 1)$.

Finally, return the answer.

The time complexity is $O(n)$, where $n$ is the length of the string $s$. The space complexity is $O(1)$.

<!-- tabs:start -->

#### Java

```java
class Solution {
    public int equalSubstring(String s, String t, int maxCost) {
        int n = s.length();
        int ans = 0, cost = 0;
        for (int l = 0, r = 0; r < n; ++r) {
            cost += Math.abs(s.charAt(r) - t.charAt(r));
            while (cost > maxCost) {
                cost -= Math.abs(s.charAt(l) - t.charAt(l));
                ++l;
            }
            ans = Math.max(ans, r - l + 1);
        }
        return ans;
    }
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- solution:start -->

### Solution 3: Another Way of Using Two Pointers

In Solution 2, the interval maintained by the two pointers may become shorter or longer. Since the problem only requires the maximum length, we can maintain a monotonically increasing interval.

Specifically, we use two pointers $l$ and $r$ to point to the left and right endpoints of the interval, initially $l = r = 0$. In each step, we move $r$ to the right by one position, then update $\textit{cost} = \textit{cost} + |s[r] - t[r]|$. If $\textit{cost} \gt \textit{maxCost}$, then we move $l$ to the right by one position, and decrease the value of $\textit{cost}$.

Finally, return $n - l$.

The time complexity is $O(n)$, and the space complexity is $O(1)$. Where $n$ is the length of the string $s$.

<!-- tabs:start -->

#### Java

```java
class Solution {
    public int equalSubstring(String s, String t, int maxCost) {
        int n = s.length();
        int cost = 0, l = 0;
        for (int r = 0; r < n; ++r) {
            cost += Math.abs(s.charAt(r) - t.charAt(r));
            if (cost > maxCost) {
                cost -= Math.abs(s.charAt(l) - t.charAt(l));
                ++l;
            }
        }
        return n - l;
    }
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
