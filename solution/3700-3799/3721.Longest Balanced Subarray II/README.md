---
comments: true
difficulty: Hard
edit_url: https://github.com/doocs/leetcode/edit/main/solution/3700-3799/3721.Longest%20Balanced%20Subarray%20II/README_EN.md
rating: 2723
source: Weekly Contest 472 Q4
tags:
    - Segment Tree
    - Array
    - Hash Table
    - Divide and Conquer
    - Prefix Sum
---

<!-- problem:start -->

# [3721. Longest Balanced Subarray II](https://leetcode.com/problems/longest-balanced-subarray-ii)

[Chinese Version](/solution/3700-3799/3721.Longest%20Balanced%20Subarray%20II/README.md)

## Description

<!-- description:start -->

<p>You are given an integer array <code>nums</code>.</p>

<p>A <strong><span data-keyword="subarray-nonempty">subarray</span></strong> is called <strong>balanced</strong> if the number of <strong>distinct even</strong> numbers in the subarray is equal to the number of <strong>distinct odd</strong> numbers.</p>

<p>Return the length of the <strong>longest</strong> balanced subarray.</p>

<p>&nbsp;</p>
<p><strong class="example">Example 1:</strong></p>

<div class="example-block">
<p><strong>Input:</strong> <span class="example-io">nums = [2,5,4,3]</span></p>

<p><strong>Output:</strong> <span class="example-io">4</span></p>

<p><strong>Explanation:</strong></p>

<ul>
	<li>The longest balanced subarray is <code>[2, 5, 4, 3]</code>.</li>
	<li>It has 2 distinct even numbers <code>[2, 4]</code> and 2 distinct odd numbers <code>[5, 3]</code>. Thus, the answer is 4.</li>
</ul>
</div>

<p><strong class="example">Example 2:</strong></p>

<div class="example-block">
<p><strong>Input:</strong> <span class="example-io">nums = [3,2,2,5,4]</span></p>

<p><strong>Output:</strong> <span class="example-io">5</span></p>

<p><strong>Explanation:</strong></p>

<ul>
	<li>The longest balanced subarray is <code>[3, 2, 2, 5, 4]</code>.</li>
	<li>It has 2 distinct even numbers <code>[2, 4]</code> and 2 distinct odd numbers <code>[3, 5]</code>. Thus, the answer is 5.</li>
</ul>
</div>

<p><strong class="example">Example 3:</strong></p>

<div class="example-block">
<p><strong>Input:</strong> <span class="example-io">nums = [1,2,3,2]</span></p>

<p><strong>Output:</strong> <span class="example-io">3</span></p>

<p><strong>Explanation:</strong></p>

<ul>
	<li>The longest balanced subarray is <code>[2, 3, 2]</code>.</li>
	<li>It has 1 distinct even number <code>[2]</code> and 1 distinct odd number <code>[3]</code>. Thus, the answer is 3.</li>
</ul>
</div>

<p>&nbsp;</p>
<p><strong>Constraints:</strong></p>

<ul>
	<li><code>1 &lt;= nums.length &lt;= 10<sup>5</sup></code></li>
	<li><code>1 &lt;= nums[i] &lt;= 10<sup>5</sup></code></li>
</ul>

<!-- description:end -->

## Solutions

<!-- solution:start -->

### Solution 1: Segment Tree + Prefix Sum + Hash Table

We can transform the problem into a prefix sum problem. Define a prefix sum variable $\textit{now}$, representing the difference between odd and even numbers in the current subarray:

$$
\textit{now} = \text{distinct odd numbers} - \text{distinct even numbers}
$$

For odd elements, record as $+1$, and for even elements, record as $-1$. Use a hash table $\textit{last}$ to record the last occurrence position of each number. If a number appears repeatedly, we need to revoke its previous contribution.

To efficiently calculate the subarray length each time a right endpoint element is added, we use a segment tree to maintain the minimum and maximum values of the interval prefix sum, while supporting interval addition operations and binary search queries on the segment tree. When iterating to right endpoint $i$, first update the contribution of the current element, then use the segment tree to query the earliest position $pos$ where the current prefix sum $\textit{now}$ appears. The current subarray length is $i - pos$, and we update the answer:

$$
\textit{ans} = \max(\textit{ans}, i - pos)
$$

The time complexity is $O(n \log n)$, where $n$ is the length of the array. Each segment tree modification and query operation takes $O(\log n)$, and enumerating the right endpoint $n$ times gives a total time complexity of $O(n \log n)$. The space complexity is $O(n)$, where segment tree nodes and the hash table each occupy $O(n)$ space.

<!-- tabs:start -->

#### Java

```java
/**
 *
 * Idea:
 * - Treat each distinct odd number as +1, and each distinct even number as -1
 * - Maintain prefix sums representing the balance
 * - When a number appears again, remove its previous contribution
 * - Use a segment tree to maintain min/max prefix sums with range add
 * - Binary search on the segment tree to find the earliest equal prefix sum
 */
class Solution {

    /**
     * Segment tree node
     */
    static class Node {
        int l, r; // segment range
        int mn, mx; // minimum / maximum prefix sum
        int lazy; // lazy propagation (range add)
    }

    /**
     * Segment tree supporting:
     * - range add
     * - find the smallest index with a given prefix sum
     */
    static class SegmentTree {
        Node[] tr;

        SegmentTree(int n) {
            tr = new Node[n << 2];
            for (int i = 0; i < tr.length; i++) {
                tr[i] = new Node();
            }
            build(1, 0, n);
        }

        // Build an empty tree with all prefix sums = 0
        void build(int u, int l, int r) {
            tr[u].l = l;
            tr[u].r = r;
            tr[u].mn = tr[u].mx = 0;
            tr[u].lazy = 0;
            if (l == r) return;
            int mid = (l + r) >> 1;
            build(u << 1, l, mid);
            build(u << 1 | 1, mid + 1, r);
        }

        // Add value v to all prefix sums in [l, r]
        void modify(int u, int l, int r, int v) {
            if (tr[u].l >= l && tr[u].r <= r) {
                apply(u, v);
                return;
            }
            pushdown(u);
            int mid = (tr[u].l + tr[u].r) >> 1;
            if (l <= mid) modify(u << 1, l, r, v);
            if (r > mid) modify(u << 1 | 1, l, r, v);
            pushup(u);
        }

        // Binary search on the segment tree
        // Find the smallest index where prefix sum == target
        int query(int u, int target) {
            if (tr[u].l == tr[u].r) {
                return tr[u].l;
            }
            pushdown(u);
            int left = u << 1;
            int right = u << 1 | 1;
            if (tr[left].mn <= target && target <= tr[left].mx) {
                return query(left, target);
            }
            return query(right, target);
        }

        // Apply range add
        void apply(int u, int v) {
            tr[u].mn += v;
            tr[u].mx += v;
            tr[u].lazy += v;
        }

        // Update from children
        void pushup(int u) {
            tr[u].mn = Math.min(tr[u << 1].mn, tr[u << 1 | 1].mn);
            tr[u].mx = Math.max(tr[u << 1].mx, tr[u << 1 | 1].mx);
        }

        // Push lazy tag down
        void pushdown(int u) {
            if (tr[u].lazy != 0) {
                apply(u << 1, tr[u].lazy);
                apply(u << 1 | 1, tr[u].lazy);
                tr[u].lazy = 0;
            }
        }
    }

    public int longestBalanced(int[] nums) {
        int n = nums.length;
        SegmentTree st = new SegmentTree(n);

        // last[x] = last position where value x appeared
        Map<Integer, Integer> last = new HashMap<>();

        int now = 0; // current prefix sum
        int ans = 0; // answer

        // Enumerate the right endpoint
        for (int i = 1; i <= n; i++) {
            int x = nums[i - 1];
            int det = (x & 1) == 1 ? 1 : -1;

            // Remove previous contribution if x appeared before
            if (last.containsKey(x)) {
                st.modify(1, last.get(x), n, -det);
                now -= det;
            }

            // Add current contribution
            last.put(x, i);
            st.modify(1, i, n, det);
            now += det;

            // Find earliest position with the same prefix sum
            int pos = st.query(1, now);
            ans = Math.max(ans, i - pos);
        }

        return ans;
    }
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
