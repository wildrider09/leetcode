---
comments: true
difficulty: Hard
edit_url: https://github.com/doocs/leetcode/edit/main/solution/1700-1799/1782.Count%20Pairs%20Of%20Nodes/README_EN.md
rating: 2457
source: Biweekly Contest 47 Q4
tags:
    - Graph
    - Array
    - Hash Table
    - Two Pointers
    - Binary Search
    - Counting
    - Sorting
---

<!-- problem:start -->

# [1782. Count Pairs Of Nodes](https://leetcode.com/problems/count-pairs-of-nodes)

[Chinese Version](/solution/1700-1799/1782.Count%20Pairs%20Of%20Nodes/README.md)

## Description

<!-- description:start -->

<p>You are given an undirected graph defined by an integer <code>n</code>, the number of nodes, and a 2D integer array <code>edges</code>, the edges in the graph, where <code>edges[i] = [u<sub>i</sub>, v<sub>i</sub>]</code> indicates that there is an <strong>undirected</strong> edge between <code>u<sub>i</sub></code> and <code>v<sub>i</sub></code>. You are also given an integer array <code>queries</code>.</p>

<p>Let <code>incident(a, b)</code> be defined as the <strong>number of edges</strong> that are connected to <strong>either</strong> node <code>a</code> or <code>b</code>.</p>

<p>The answer to the <code>j<sup>th</sup></code> query is the <strong>number of pairs</strong> of nodes <code>(a, b)</code> that satisfy <strong>both</strong> of the following conditions:</p>

<ul>
	<li><code>a &lt; b</code></li>
	<li><code>incident(a, b) &gt; queries[j]</code></li>
</ul>

<p>Return <em>an array </em><code>answers</code><em> such that </em><code>answers.length == queries.length</code><em> and </em><code>answers[j]</code><em> is the answer of the </em><code>j<sup>th</sup></code><em> query</em>.</p>

<p>Note that there can be <strong>multiple edges</strong> between the same two nodes.</p>

<p>&nbsp;</p>
<p><strong class="example">Example 1:</strong></p>
<img alt="" src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/1700-1799/1782.Count%20Pairs%20Of%20Nodes/images/winword_2021-06-08_00-58-39.png" style="width: 529px; height: 305px;" />
<pre>
<strong>Input:</strong> n = 4, edges = [[1,2],[2,4],[1,3],[2,3],[2,1]], queries = [2,3]
<strong>Output:</strong> [6,5]
<strong>Explanation:</strong> The calculations for incident(a, b) are shown in the table above.
The answers for each of the queries are as follows:
- answers[0] = 6. All the pairs have an incident(a, b) value greater than 2.
- answers[1] = 5. All the pairs except (3, 4) have an incident(a, b) value greater than 3.
</pre>

<p><strong class="example">Example 2:</strong></p>

<pre>
<strong>Input:</strong> n = 5, edges = [[1,5],[1,5],[3,4],[2,5],[1,3],[5,1],[2,3],[2,5]], queries = [1,2,3,4,5]
<strong>Output:</strong> [10,10,9,8,6]
</pre>

<p>&nbsp;</p>
<p><strong>Constraints:</strong></p>

<ul>
	<li><code>2 &lt;= n &lt;= 2 * 10<sup>4</sup></code></li>
	<li><code>1 &lt;= edges.length &lt;= 10<sup>5</sup></code></li>
	<li><code>1 &lt;= u<sub>i</sub>, v<sub>i</sub> &lt;= n</code></li>
	<li><code>u<sub>i </sub>!= v<sub>i</sub></code></li>
	<li><code>1 &lt;= queries.length &lt;= 20</code></li>
	<li><code>0 &lt;= queries[j] &lt; edges.length</code></li>
</ul>

<!-- description:end -->

## Solutions

<!-- solution:start -->

### Solution 1: Hash Table + Sorting + Binary Search

From the problem, we know that the number of edges connected to the point pair $(a, b)$ is equal to the "number of edges connected to $a$" plus the "number of edges connected to $b$", minus the number of edges connected to both $a$ and $b$.

Therefore, we can first use the array $cnt$ to count the number of edges connected to each point, and use the hash table $g$ to count the number of each point pair.

Then, for each query $q$, we can enumerate $a$. For each $a$, we can find the first $b$ that satisfies $cnt[a] + cnt[b] > q$ through binary search, add the number to the current query answer, and then subtract some duplicate edges.

The time complexity is $O(q \times (n \times \log n + m))$, and the space complexity is $O(n + m)$. Where $n$ and $m$ are the number of points and edges respectively, and $q$ is the number of queries.

<!-- tabs:start -->

#### Java

```java
class Solution {
    public int[] countPairs(int n, int[][] edges, int[] queries) {
        int[] cnt = new int[n];
        Map<Integer, Integer> g = new HashMap<>();
        for (var e : edges) {
            int a = e[0] - 1, b = e[1] - 1;
            ++cnt[a];
            ++cnt[b];
            int k = Math.min(a, b) * n + Math.max(a, b);
            g.merge(k, 1, Integer::sum);
        }
        int[] s = cnt.clone();
        Arrays.sort(s);
        int[] ans = new int[queries.length];
        for (int i = 0; i < queries.length; ++i) {
            int t = queries[i];
            for (int j = 0; j < n; ++j) {
                int x = s[j];
                int k = search(s, t - x, j + 1);
                ans[i] += n - k;
            }
            for (var e : g.entrySet()) {
                int a = e.getKey() / n, b = e.getKey() % n;
                int v = e.getValue();
                if (cnt[a] + cnt[b] > t && cnt[a] + cnt[b] - v <= t) {
                    --ans[i];
                }
            }
        }
        return ans;
    }

    private int search(int[] arr, int x, int i) {
        int left = i, right = arr.length;
        while (left < right) {
            int mid = (left + right) >> 1;
            if (arr[mid] > x) {
                right = mid;
            } else {
                left = mid + 1;
            }
        }
        return left;
    }
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
