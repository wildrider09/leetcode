---
comments: true
difficulty: Hard
edit_url: https://github.com/doocs/leetcode/edit/main/solution/2500-2599/2569.Handling%20Sum%20Queries%20After%20Update/README_EN.md
rating: 2397
source: Biweekly Contest 98 Q4
tags:
    - Segment Tree
    - Array
---

<!-- problem:start -->

# [2569. Handling Sum Queries After Update](https://leetcode.com/problems/handling-sum-queries-after-update)

[Chinese Version](/solution/2500-2599/2569.Handling%20Sum%20Queries%20After%20Update/README.md)

## Description

<!-- description:start -->

<p>You are given two <strong>0-indexed</strong> arrays <code>nums1</code> and <code>nums2</code> and a 2D array <code>queries</code> of queries. There are three types of queries:</p>

<ol>
	<li>For a query of type 1, <code>queries[i]&nbsp;= [1, l, r]</code>. Flip the values from <code>0</code> to <code>1</code> and from <code>1</code> to <code>0</code> in <code>nums1</code>&nbsp;from index <code>l</code> to index <code>r</code>. Both <code>l</code> and <code>r</code> are <strong>0-indexed</strong>.</li>
	<li>For a query of type 2, <code>queries[i]&nbsp;= [2, p, 0]</code>. For every index <code>0 &lt;= i &lt; n</code>, set&nbsp;<code>nums2[i] =&nbsp;nums2[i]&nbsp;+ nums1[i]&nbsp;* p</code>.</li>
	<li>For a query of type 3, <code>queries[i]&nbsp;= [3, 0, 0]</code>. Find the sum of the elements in <code>nums2</code>.</li>
</ol>

<p>Return <em>an array containing all the answers to the third type&nbsp;queries.</em></p>

<p>&nbsp;</p>
<p><strong class="example">Example 1:</strong></p>

<pre>
<strong>Input:</strong> nums1 = [1,0,1], nums2 = [0,0,0], queries = [[1,1,1],[2,1,0],[3,0,0]]
<strong>Output:</strong> [3]
<strong>Explanation:</strong> After the first query nums1 becomes [1,1,1]. After the second query, nums2 becomes [1,1,1], so the answer to the third query is 3. Thus, [3] is returned.
</pre>

<p><strong class="example">Example 2:</strong></p>

<pre>
<strong>Input:</strong> nums1 = [1], nums2 = [5], queries = [[2,0,0],[3,0,0]]
<strong>Output:</strong> [5]
<strong>Explanation:</strong> After the first query, nums2 remains [5], so the answer to the second query is 5. Thus, [5] is returned.
</pre>

<p>&nbsp;</p>
<p><strong>Constraints:</strong></p>

<ul>
	<li><code>1 &lt;= nums1.length,nums2.length &lt;= 10<sup>5</sup></code></li>
	<li><code>nums1.length = nums2.length</code></li>
	<li><code>1 &lt;= queries.length &lt;= 10<sup>5</sup></code></li>
	<li><code><font face="monospace">queries[i].length = 3</font></code></li>
	<li><code><font face="monospace">0 &lt;= l &lt;= r &lt;= nums1.length - 1</font></code></li>
	<li><code><font face="monospace">0 &lt;= p &lt;= 10<sup>6</sup></font></code></li>
	<li><code>0 &lt;= nums1[i] &lt;= 1</code></li>
	<li><code>0 &lt;= nums2[i] &lt;= 10<sup>9</sup></code></li>
</ul>

<!-- description:end -->

## Solutions

<!-- solution:start -->

### Solution 1: Segment Tree

According to the problem description:

- Operation $1$ is to reverse all numbers in the index range $[l,..r]$ of array `nums1`, that is, change $0$ to $1$ and $1$ to $0$.
- Operation $3$ is to sum all numbers in array `nums2`.
- Operation $2$ is to add the sum of all numbers in array `nums2` with $p$ times the sum of all numbers in array `nums1`, that is, $sum(nums2) = sum(nums2) + p * sum(nums1)$.

Therefore, we actually only need to maintain the segment sum of array `nums1`, which can be implemented through a segment tree.

We define each node of the segment tree as `Node`, each node contains the following attributes:

- `l`: The left endpoint of the node, the index starts from $1$.
- `r`: The right endpoint of the node, the index starts from $1$.
- `s`: The segment sum of the node.
- `lazy`: The lazy tag of the node.

The segment tree mainly has the following operations:

- `build(u, l, r)`: Build the segment tree.
- `pushdown(u)`: Propagate the lazy tag.
- `pushup(u)`: Update the information of the parent node with the information of the child nodes.
- `modify(u, l, r)`: Modify the segment sum. In this problem, it is to reverse each number in the segment, so the segment sum $s = r - l + 1 - s$.
- `query(u, l, r)`: Query the segment sum.

First, calculate the sum of all numbers in array `nums2`, denoted as $s$.

When executing operation $1$, we only need to call `modify(1, l + 1, r + 1)`.

When executing operation $2$, we update $s = s + p \times query(1, 1, n)$.

When executing operation $3$, we just need to add $s$ to the answer array.

The time complexity is $O(n + m \times \log n)$, and the space complexity is $O(n)$. Where $n$ and $m$ are the lengths of arrays `nums1` and `queries` respectively.

<!-- tabs:start -->

#### Java

```java
class Node {
    int l, r;
    int s, lazy;
}

class SegmentTree {
    private Node[] tr;
    private int[] nums;

    public SegmentTree(int[] nums) {
        int n = nums.length;
        this.nums = nums;
        tr = new Node[n << 2];
        for (int i = 0; i < tr.length; ++i) {
            tr[i] = new Node();
        }
        build(1, 1, n);
    }

    private void build(int u, int l, int r) {
        tr[u].l = l;
        tr[u].r = r;
        if (l == r) {
            tr[u].s = nums[l - 1];
            return;
        }
        int mid = (l + r) >> 1;
        build(u << 1, l, mid);
        build(u << 1 | 1, mid + 1, r);
        pushup(u);
    }

    public void modify(int u, int l, int r) {
        if (tr[u].l >= l && tr[u].r <= r) {
            tr[u].lazy ^= 1;
            tr[u].s = tr[u].r - tr[u].l + 1 - tr[u].s;
            return;
        }
        pushdown(u);
        int mid = (tr[u].l + tr[u].r) >> 1;
        if (l <= mid) {
            modify(u << 1, l, r);
        }
        if (r > mid) {
            modify(u << 1 | 1, l, r);
        }
        pushup(u);
    }

    public int query(int u, int l, int r) {
        if (tr[u].l >= l && tr[u].r <= r) {
            return tr[u].s;
        }
        pushdown(u);
        int mid = (tr[u].l + tr[u].r) >> 1;
        int res = 0;
        if (l <= mid) {
            res += query(u << 1, l, r);
        }
        if (r > mid) {
            res += query(u << 1 | 1, l, r);
        }
        return res;
    }

    private void pushup(int u) {
        tr[u].s = tr[u << 1].s + tr[u << 1 | 1].s;
    }

    private void pushdown(int u) {
        if (tr[u].lazy == 1) {
            int mid = (tr[u].l + tr[u].r) >> 1;
            tr[u << 1].s = mid - tr[u].l + 1 - tr[u << 1].s;
            tr[u << 1].lazy ^= 1;
            tr[u << 1 | 1].s = tr[u].r - mid - tr[u << 1 | 1].s;
            tr[u << 1 | 1].lazy ^= 1;
            tr[u].lazy ^= 1;
        }
    }
}

class Solution {
    public long[] handleQuery(int[] nums1, int[] nums2, int[][] queries) {
        SegmentTree tree = new SegmentTree(nums1);
        long s = 0;
        for (int x : nums2) {
            s += x;
        }
        int m = 0;
        for (var q : queries) {
            if (q[0] == 3) {
                ++m;
            }
        }
        long[] ans = new long[m];
        int i = 0;
        for (var q : queries) {
            if (q[0] == 1) {
                tree.modify(1, q[1] + 1, q[2] + 1);
            } else if (q[0] == 2) {
                s += 1L * q[1] * tree.query(1, 1, nums2.length);
            } else {
                ans[i++] = s;
            }
        }
        return ans;
    }
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
