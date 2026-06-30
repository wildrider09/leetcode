---
comments: true
difficulty: Hard
edit_url: https://github.com/doocs/leetcode/edit/main/solution/1100-1199/1157.Online%20Majority%20Element%20In%20Subarray/README_EN.md
rating: 2205
source: Weekly Contest 149 Q4
tags:
    - Design
    - Binary Indexed Tree
    - Segment Tree
    - Array
    - Binary Search
---

<!-- problem:start -->

# [1157. Online Majority Element In Subarray](https://leetcode.com/problems/online-majority-element-in-subarray)

[Chinese Version](/solution/1100-1199/1157.Online%20Majority%20Element%20In%20Subarray/README.md)

## Description

<!-- description:start -->

<p>Design a data structure that efficiently finds the <strong>majority element</strong> of a given subarray.</p>

<p>The <strong>majority element</strong> of a subarray is an element that occurs <code>threshold</code> times or more in the subarray.</p>

<p>Implementing the <code>MajorityChecker</code> class:</p>

<ul>
	<li><code>MajorityChecker(int[] arr)</code> Initializes the instance of the class with the given array <code>arr</code>.</li>
	<li><code>int query(int left, int right, int threshold)</code> returns the element in the subarray <code>arr[left...right]</code> that occurs at least <code>threshold</code> times, or <code>-1</code> if no such element exists.</li>
</ul>

<p>&nbsp;</p>
<p><strong class="example">Example 1:</strong></p>

<pre>
<strong>Input</strong>
[&quot;MajorityChecker&quot;, &quot;query&quot;, &quot;query&quot;, &quot;query&quot;]
[[[1, 1, 2, 2, 1, 1]], [0, 5, 4], [0, 3, 3], [2, 3, 2]]
<strong>Output</strong>
[null, 1, -1, 2]

<strong>Explanation</strong>
MajorityChecker majorityChecker = new MajorityChecker([1, 1, 2, 2, 1, 1]);
majorityChecker.query(0, 5, 4); // return 1
majorityChecker.query(0, 3, 3); // return -1
majorityChecker.query(2, 3, 2); // return 2
</pre>

<p>&nbsp;</p>
<p><strong>Constraints:</strong></p>

<ul>
	<li><code>1 &lt;= arr.length &lt;= 2 * 10<sup>4</sup></code></li>
	<li><code>1 &lt;= arr[i] &lt;= 2 * 10<sup>4</sup></code></li>
	<li><code>0 &lt;= left &lt;= right &lt; arr.length</code></li>
	<li><code>threshold &lt;= right - left + 1</code></li>
	<li><code>2 * threshold &gt; right - left + 1</code></li>
	<li>At most <code>10<sup>4</sup></code> calls will be made to <code>query</code>.</li>
</ul>

<!-- description:end -->

## Solutions

<!-- solution:start -->

### Solution 1: Segment Tree + Boyer-Moore Voting Algorithm + Binary Search

We notice that the problem requires us to find the possible majority element in a specific interval, so we consider using a segment tree to maintain the candidate majority element and its occurrence in each interval.

We define each node of the segment tree as `Node`, each node contains the following attributes:

- `l`: The left endpoint of the node, the index starts from $1$.
- `r`: The right endpoint of the node, the index starts from $1$.
- `x`: The candidate majority element of the node.
- `cnt`: The occurrence of the candidate majority element of the node.

The segment tree mainly has the following operations:

- `build(u, l, r)`: Build the segment tree.
- `pushup(u)`: Use the information of the child nodes to update the information of the parent node.
- `query(u, l, r)`: Query the interval sum.

In the initialization method of the main function, we first create a segment tree, and use a hash table $d$ to record all indexes of each element in the array.

In the `query(left, right, threshold)` method, we directly call the `query` method of the segment tree to get the candidate majority element $x$. Then use binary search to find the first index $l$ in the array that is greater than or equal to $left$, and the first index $r$ that is greater than $right$. If $r - l \ge threshold$, return $x$, otherwise return $-1$.

In terms of time complexity, the time complexity of the initialization method is $O(n)$, and the time complexity of the query method is $O(\log n)$. The space complexity is $O(n)$. Here, $n$ is the length of the array.

<!-- tabs:start -->

#### Java

```java
class Node {
    int l, r;
    int x, cnt;
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
            tr[u].x = nums[l - 1];
            tr[u].cnt = 1;
            return;
        }
        int mid = (l + r) >> 1;
        build(u << 1, l, mid);
        build(u << 1 | 1, mid + 1, r);
        pushup(u);
    }

    public int[] query(int u, int l, int r) {
        if (tr[u].l >= l && tr[u].r <= r) {
            return new int[] {tr[u].x, tr[u].cnt};
        }
        int mid = (tr[u].l + tr[u].r) >> 1;
        if (r <= mid) {
            return query(u << 1, l, r);
        }
        if (l > mid) {
            return query(u << 1 | 1, l, r);
        }
        var left = query(u << 1, l, r);
        var right = query(u << 1 | 1, l, r);
        if (left[0] == right[0]) {
            left[1] += right[1];
        } else if (left[1] >= right[1]) {
            left[1] -= right[1];
        } else {
            right[1] -= left[1];
            left = right;
        }
        return left;
    }

    private void pushup(int u) {
        if (tr[u << 1].x == tr[u << 1 | 1].x) {
            tr[u].x = tr[u << 1].x;
            tr[u].cnt = tr[u << 1].cnt + tr[u << 1 | 1].cnt;
        } else if (tr[u << 1].cnt >= tr[u << 1 | 1].cnt) {
            tr[u].x = tr[u << 1].x;
            tr[u].cnt = tr[u << 1].cnt - tr[u << 1 | 1].cnt;
        } else {
            tr[u].x = tr[u << 1 | 1].x;
            tr[u].cnt = tr[u << 1 | 1].cnt - tr[u << 1].cnt;
        }
    }
}

class MajorityChecker {
    private SegmentTree tree;
    private Map<Integer, List<Integer>> d = new HashMap<>();

    public MajorityChecker(int[] arr) {
        tree = new SegmentTree(arr);
        for (int i = 0; i < arr.length; ++i) {
            d.computeIfAbsent(arr[i], k -> new ArrayList<>()).add(i);
        }
    }

    public int query(int left, int right, int threshold) {
        int x = tree.query(1, left + 1, right + 1)[0];
        int l = search(d.get(x), left);
        int r = search(d.get(x), right + 1);
        return r - l >= threshold ? x : -1;
    }

    private int search(List<Integer> arr, int x) {
        int left = 0, right = arr.size();
        while (left < right) {
            int mid = (left + right) >> 1;
            if (arr.get(mid) >= x) {
                right = mid;
            } else {
                left = mid + 1;
            }
        }
        return left;
    }
}

/**
 * Your MajorityChecker object will be instantiated and called as such:
 * MajorityChecker obj = new MajorityChecker(arr);
 * int param_1 = obj.query(left,right,threshold);
 */
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
