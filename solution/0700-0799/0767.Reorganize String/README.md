---
comments: true
difficulty: Medium
edit_url: https://github.com/doocs/leetcode/edit/main/solution/0700-0799/0767.Reorganize%20String/README_EN.md
tags:
    - Greedy
    - Hash Table
    - String
    - Counting
    - Sorting
    - Heap (Priority Queue)
---

<!-- problem:start -->

# [767. Reorganize String](https://leetcode.com/problems/reorganize-string)

[Chinese Version](/solution/0700-0799/0767.Reorganize%20String/README.md)

## Description

<!-- description:start -->

<p>Given a string <code>s</code>, rearrange the characters of <code>s</code> so that any two adjacent characters are not the same.</p>

<p>Return <em>any possible rearrangement of</em> <code>s</code> <em>or return</em> <code>&quot;&quot;</code> <em>if not possible</em>.</p>

<p>&nbsp;</p>
<p><strong class="example">Example 1:</strong></p>
<pre><strong>Input:</strong> s = "aab"
<strong>Output:</strong> "aba"
</pre><p><strong class="example">Example 2:</strong></p>
<pre><strong>Input:</strong> s = "aaab"
<strong>Output:</strong> ""
</pre>
<p>&nbsp;</p>
<p><strong>Constraints:</strong></p>

<ul>
	<li><code>1 &lt;= s.length &lt;= 500</code></li>
	<li><code>s</code> consists of lowercase English letters.</li>
</ul>

<!-- description:end -->

## Solutions

<!-- solution:start -->

### Solution 1

<!-- tabs:start -->

#### Java

```java
class Solution {
    public String reorganizeString(String s) {
        int[] cnt = new int[26];
        int mx = 0;
        for (char c : s.toCharArray()) {
            int t = c - 'a';
            ++cnt[t];
            mx = Math.max(mx, cnt[t]);
        }
        int n = s.length();
        if (mx > (n + 1) / 2) {
            return "";
        }
        int k = 0;
        for (int v : cnt) {
            if (v > 0) {
                ++k;
            }
        }
        int[][] m = new int[k][2];
        k = 0;
        for (int i = 0; i < 26; ++i) {
            if (cnt[i] > 0) {
                m[k++] = new int[] {cnt[i], i};
            }
        }
        Arrays.sort(m, (a, b) -> b[0] - a[0]);
        k = 0;
        StringBuilder ans = new StringBuilder(s);
        for (int[] e : m) {
            int v = e[0], i = e[1];
            while (v-- > 0) {
                ans.setCharAt(k, (char) ('a' + i));
                k += 2;
                if (k >= n) {
                    k = 1;
                }
            }
        }
        return ans.toString();
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
    public String reorganizeString(String s) {
        return rearrangeString(s, 2);
    }

    public String rearrangeString(String s, int k) {
        int n = s.length();
        int[] cnt = new int[26];
        for (char c : s.toCharArray()) {
            ++cnt[c - 'a'];
        }
        PriorityQueue<int[]> pq = new PriorityQueue<>((a, b) -> b[0] - a[0]);
        for (int i = 0; i < 26; ++i) {
            if (cnt[i] > 0) {
                pq.offer(new int[] {cnt[i], i});
            }
        }
        Deque<int[]> q = new ArrayDeque<>();
        StringBuilder ans = new StringBuilder();
        while (!pq.isEmpty()) {
            var p = pq.poll();
            int v = p[0], c = p[1];
            ans.append((char) ('a' + c));
            q.offer(new int[] {v - 1, c});
            if (q.size() >= k) {
                p = q.pollFirst();
                if (p[0] > 0) {
                    pq.offer(p);
                }
            }
        }
        return ans.length() == n ? ans.toString() : "";
    }
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
