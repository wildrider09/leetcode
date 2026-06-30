---
comments: true
difficulty: Medium
edit_url: https://github.com/doocs/leetcode/edit/main/lcci/10.10.Rank%20from%20Stream/README_EN.md
---

<!-- problem:start -->

# [10.10. Rank from Stream](https://leetcode.cn/problems/rank-from-stream-lcci)

[Chinese Version](/lcci/10.10.Rank%20from%20Stream/README.md)

## Description

<!-- description:start -->

<p>Imagine you are reading in a stream of integers. Periodically, you wish to be able to look up the rank of a number <code>x</code> (the number of values less than or equal to <code>x</code>). lmplement the data structures and algorithms to support these operations. That is, implement the method <code>track (int x)</code>, which is called when each number is generated, and the method <code>getRankOfNumber(int x)</code>, which returns the number of values less than or equal to <code>x</code>.</p>

<p><b>Note:&nbsp;</b>This problem is slightly different from the original one in the book.</p>

<p><strong>Example:</strong></p>

<pre>

<strong>Input:</strong>

[&quot;StreamRank&quot;, &quot;getRankOfNumber&quot;, &quot;track&quot;, &quot;getRankOfNumber&quot;]

[[], [1], [0], [0]]

<strong>Output:

</strong>[null,0,null,1]

</pre>

<p><strong>Note: </strong></p>

<ul>
	<li><code>x &lt;= 50000</code></li>
	<li>The number of calls of both&nbsp;<code>track</code>&nbsp;and&nbsp;<code>getRankOfNumber</code>&nbsp;methods are less than or equal to 2000.</li>
</ul>

<!-- description:end -->

## Solutions

<!-- solution:start -->

### Solution 1: Binary Indexed Tree

We can use a Binary Indexed Tree (also known as a Fenwick Tree) to maintain the count of numbers that are less than or equal to the current number among the added numbers.

We create a Binary Indexed Tree with a length of $50010$. For the `track` method, we increment the current number and add it to the Binary Indexed Tree. For the `getRankOfNumber` method, we directly query the count of numbers that are less than or equal to $x + 1$ in the Binary Indexed Tree.

In terms of time complexity, both the update and query operations of the Binary Indexed Tree have a time complexity of $O(\log n)$, where $n$ is the length of the Binary Indexed Tree.

<!-- tabs:start -->

#### Java

```java
class BinaryIndexedTree {
    private int n;
    private int[] c;

    public BinaryIndexedTree(int n) {
        this.n = n;
        this.c = new int[n + 1];
    }

    public void update(int x, int delta) {
        for (; x <= n; x += x & -x) {
            c[x] += delta;
        }
    }

    public int query(int x) {
        int s = 0;
        for (; x > 0; x -= x & -x) {
            s += c[x];
        }
        return s;
    }
}

class StreamRank {
    private BinaryIndexedTree tree = new BinaryIndexedTree(50010);

    public StreamRank() {

    }

    public void track(int x) {
        tree.update(x + 1, 1);
    }

    public int getRankOfNumber(int x) {
        return tree.query(x + 1);
    }
}

/**
 * Your StreamRank object will be instantiated and called as such:
 * StreamRank obj = new StreamRank();
 * obj.track(x);
 * int param_2 = obj.getRankOfNumber(x);
 */
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
