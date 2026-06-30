---
comments: true
difficulty: Medium
edit_url: https://github.com/doocs/leetcode/edit/main/solution/1100-1199/1109.Corporate%20Flight%20Bookings/README_EN.md
rating: 1569
source: Weekly Contest 144 Q2
tags:
    - Array
    - Prefix Sum
---

<!-- problem:start -->

# [1109. Corporate Flight Bookings](https://leetcode.com/problems/corporate-flight-bookings)

[Chinese Version](/solution/1100-1199/1109.Corporate%20Flight%20Bookings/README.md)

## Description

<!-- description:start -->

<p>There are <code>n</code> flights that are labeled from <code>1</code> to <code>n</code>.</p>

<p>You are given an array of flight bookings <code>bookings</code>, where <code>bookings[i] = [first<sub>i</sub>, last<sub>i</sub>, seats<sub>i</sub>]</code> represents a booking for flights <code>first<sub>i</sub></code> through <code>last<sub>i</sub></code> (<strong>inclusive</strong>) with <code>seats<sub>i</sub></code> seats reserved for <strong>each flight</strong> in the range.</p>

<p>Return <em>an array </em><code>answer</code><em> of length </em><code>n</code><em>, where </em><code>answer[i]</code><em> is the total number of seats reserved for flight </em><code>i</code>.</p>

<p>&nbsp;</p>
<p><strong class="example">Example 1:</strong></p>

<pre>
<strong>Input:</strong> bookings = [[1,2,10],[2,3,20],[2,5,25]], n = 5
<strong>Output:</strong> [10,55,45,25,25]
<strong>Explanation:</strong>
Flight labels:        1   2   3   4   5
Booking 1 reserved:  10  10
Booking 2 reserved:      20  20
Booking 3 reserved:      25  25  25  25
Total seats:         10  55  45  25  25
Hence, answer = [10,55,45,25,25]
</pre>

<p><strong class="example">Example 2:</strong></p>

<pre>
<strong>Input:</strong> bookings = [[1,2,10],[2,2,15]], n = 2
<strong>Output:</strong> [10,25]
<strong>Explanation:</strong>
Flight labels:        1   2
Booking 1 reserved:  10  10
Booking 2 reserved:      15
Total seats:         10  25
Hence, answer = [10,25]

</pre>

<p>&nbsp;</p>
<p><strong>Constraints:</strong></p>

<ul>
	<li><code>1 &lt;= n &lt;= 2 * 10<sup>4</sup></code></li>
	<li><code>1 &lt;= bookings.length &lt;= 2 * 10<sup>4</sup></code></li>
	<li><code>bookings[i].length == 3</code></li>
	<li><code>1 &lt;= first<sub>i</sub> &lt;= last<sub>i</sub> &lt;= n</code></li>
	<li><code>1 &lt;= seats<sub>i</sub> &lt;= 10<sup>4</sup></code></li>
</ul>

<!-- description:end -->

## Solutions

<!-- solution:start -->

### Solution 1: Difference Array

We notice that each booking is for `seats` seats on all flights within a certain interval `[first, last]`. Therefore, we can use the idea of a difference array. For each booking, we add `seats` to the number at the `first` position and subtract `seats` from the number at the `last + 1` position. Finally, we calculate the prefix sum of the difference array to get the total number of seats booked for each flight.

The time complexity is $O(n)$, where $n$ is the number of flights. Ignoring the space consumption of the answer, the space complexity is $O(1)$.

<!-- tabs:start -->

#### Java

```java
class Solution {
    public int[] corpFlightBookings(int[][] bookings, int n) {
        int[] ans = new int[n];
        for (var e : bookings) {
            int first = e[0], last = e[1], seats = e[2];
            ans[first - 1] += seats;
            if (last < n) {
                ans[last] -= seats;
            }
        }
        for (int i = 1; i < n; ++i) {
            ans[i] += ans[i - 1];
        }
        return ans;
    }
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- solution:start -->

### Solution 2: Binary Indexed Tree + Difference Idea

We can also use a binary indexed tree, combined with the idea of difference, to implement the above operations. We can consider each booking as booking `seats` seats on all flights within a certain interval `[first, last]`. Therefore, for each booking, we add `seats` to the `first` position of the binary indexed tree and subtract `seats` from the `last + 1` position of the binary indexed tree. Finally, we calculate the prefix sum for each position in the binary indexed tree to get the total number of seats booked for each flight.

The time complexity is $O(n \times \log n)$, and the space complexity is $O(n)$. Here, $n$ is the number of flights.

Here is a basic introduction to the binary indexed tree:

A binary indexed tree, also known as a "Binary Indexed Tree" or Fenwick tree. It can efficiently implement the following two operations:

1. **Single Point Update** `update(x, delta)`: Add a value delta to the number at position x in the sequence;
1. **Prefix Sum Query** `query(x)`: Query the interval sum of the sequence `[1,...x]`, that is, the prefix sum of position x.

The time complexity of these two operations is $O(\log n)$.

<!-- tabs:start -->

#### Java

```java
class Solution {
    public int[] corpFlightBookings(int[][] bookings, int n) {
        BinaryIndexedTree tree = new BinaryIndexedTree(n);
        for (var e : bookings) {
            int first = e[0], last = e[1], seats = e[2];
            tree.update(first, seats);
            tree.update(last + 1, -seats);
        }
        int[] ans = new int[n];
        for (int i = 0; i < n; ++i) {
            ans[i] = tree.query(i + 1);
        }
        return ans;
    }
}

class BinaryIndexedTree {
    private int n;
    private int[] c;

    public BinaryIndexedTree(int n) {
        this.n = n;
        c = new int[n + 1];
    }

    public void update(int x, int delta) {
        while (x <= n) {
            c[x] += delta;
            x += x & -x;
        }
    }

    public int query(int x) {
        int s = 0;
        while (x > 0) {
            s += c[x];
            x -= x & -x;
        }
        return s;
    }
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
