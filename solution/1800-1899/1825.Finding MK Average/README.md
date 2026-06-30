---
comments: true
difficulty: Hard
edit_url: https://github.com/doocs/leetcode/edit/main/solution/1800-1899/1825.Finding%20MK%20Average/README_EN.md
rating: 2395
source: Weekly Contest 236 Q4
tags:
    - Design
    - Queue
    - Data Stream
    - Ordered Set
    - Heap (Priority Queue)
---

<!-- problem:start -->

# [1825. Finding MK Average](https://leetcode.com/problems/finding-mk-average)

[Chinese Version](/solution/1800-1899/1825.Finding%20MK%20Average/README.md)

## Description

<!-- description:start -->

<p>You are given two integers, <code>m</code> and <code>k</code>, and a stream of integers. You are tasked to implement a data structure that calculates the <strong>MKAverage</strong> for the stream.</p>

<p>The <strong>MKAverage</strong> can be calculated using these steps:</p>

<ol>
	<li>If the number of the elements in the stream is less than <code>m</code> you should consider the <strong>MKAverage</strong> to be <code>-1</code>. Otherwise, copy the last <code>m</code> elements of the stream to a separate container.</li>
	<li>Remove the smallest <code>k</code> elements and the largest <code>k</code> elements from the container.</li>
	<li>Calculate the average value for the rest of the elements <strong>rounded down to the nearest integer</strong>.</li>
</ol>

<p>Implement the <code>MKAverage</code> class:</p>

<ul>
	<li><code>MKAverage(int m, int k)</code> Initializes the <strong>MKAverage</strong> object with an empty stream and the two integers <code>m</code> and <code>k</code>.</li>
	<li><code>void addElement(int num)</code> Inserts a new element <code>num</code> into the stream.</li>
	<li><code>int calculateMKAverage()</code> Calculates and returns the <strong>MKAverage</strong> for the current stream <strong>rounded down to the nearest integer</strong>.</li>
</ul>

<p>&nbsp;</p>
<p><strong class="example">Example 1:</strong></p>

<pre>
<strong>Input</strong>
[&quot;MKAverage&quot;, &quot;addElement&quot;, &quot;addElement&quot;, &quot;calculateMKAverage&quot;, &quot;addElement&quot;, &quot;calculateMKAverage&quot;, &quot;addElement&quot;, &quot;addElement&quot;, &quot;addElement&quot;, &quot;calculateMKAverage&quot;]
[[3, 1], [3], [1], [], [10], [], [5], [5], [5], []]
<strong>Output</strong>
[null, null, null, -1, null, 3, null, null, null, 5]

<strong>Explanation</strong>
<code>MKAverage obj = new MKAverage(3, 1); 
obj.addElement(3);        // current elements are [3]
obj.addElement(1);        // current elements are [3,1]
obj.calculateMKAverage(); // return -1, because m = 3 and only 2 elements exist.
obj.addElement(10);       // current elements are [3,1,10]
obj.calculateMKAverage(); // The last 3 elements are [3,1,10].
                          // After removing smallest and largest 1 element the container will be [3].
                          // The average of [3] equals 3/1 = 3, return 3
obj.addElement(5);        // current elements are [3,1,10,5]
obj.addElement(5);        // current elements are [3,1,10,5,5]
obj.addElement(5);        // current elements are [3,1,10,5,5,5]
obj.calculateMKAverage(); // The last 3 elements are [5,5,5].
                          // After removing smallest and largest 1 element the container will be [5].
                          // The average of [5] equals 5/1 = 5, return 5
</code></pre>

<p>&nbsp;</p>
<p><strong>Constraints:</strong></p>

<ul>
	<li><code>3 &lt;= m &lt;= 10<sup>5</sup></code></li>
	<li><code>1 &lt; k*2 &lt; m</code></li>
	<li><code>1 &lt;= num &lt;= 10<sup>5</sup></code></li>
	<li>At most <code>10<sup>5</sup></code> calls will be made to <code>addElement</code> and <code>calculateMKAverage</code>.</li>
</ul>

<!-- description:end -->

## Solutions

<!-- solution:start -->

### Solution 1: Ordered Set + Queue

We can maintain the following data structures or variables:

- A queue $q$ of length $m$, where the head of the queue is the earliest added element, and the tail of the queue is the most recently added element;
- Three ordered sets, namely $lo$, $mid$, $hi$, where $lo$ and $hi$ store the smallest $k$ elements and the largest $k$ elements respectively, and $mid$ stores the remaining elements;
- A variable $s$, maintaining the sum of all elements in $mid$;
- Some programming languages (such as Java, Go) additionally maintain two variables $size1$ and $size3$, representing the number of elements in $lo$ and $hi$ respectively.

When calling the $addElement(num)$ function, perform the following operations in order:

1. If $lo$ is empty, or $num \leq max(lo)$, then add $num$ to $lo$; otherwise if $hi$ is empty, or $num \geq min(hi)$, then add $num$ to $hi$; otherwise add $num$ to $mid$, and add the value of $num$ to $s$.
1. Next, add $num$ to the queue $q$. If the length of the queue $q$ is greater than $m$ at this time, remove the head element $x$ from the queue $q$, then choose one of $lo$, $mid$ or $hi$ that contains $x$, and remove $x$ from this set. If the set is $mid$, subtract the value of $x$ from $s$.
1. If the length of $lo$ is greater than $k$, then repeatedly remove the maximum value $max(lo)$ from $lo$, add $max(lo)$ to $mid$, and add the value of $max(lo)$ to $s$.
1. If the length of $hi$ is greater than $k$, then repeatedly remove the minimum value $min(hi)$ from $hi$, add $min(hi)$ to $mid$, and add the value of $min(hi)$ to $s$.
1. If the length of $lo$ is less than $k$ and $mid$ is not empty, then repeatedly remove the minimum value $min(mid)$ from $mid$, add $min(mid)$ to $lo$, and subtract the value of $min(mid)$ from $s$.
1. If the length of $hi$ is less than $k$ and $mid$ is not empty, then repeatedly remove the maximum value $max(mid)$ from $mid$, add $max(mid)$ to $hi$, and subtract the value of $max(mid)$ from $s$.

When calling the $calculateMKAverage()$ function, if the length of $q$ is less than $m$, return $-1$, otherwise return $\frac{s}{m - 2k}$.

In terms of time complexity, each call to the $addElement(num)$ function has a time complexity of $O(\log m)$, and each call to the $calculateMKAverage()$ function has a time complexity of $O(1)$. The space complexity is $O(m)$.

<!-- tabs:start -->

#### Java

```java
class MKAverage {

    private int m, k;
    private long s;
    private int size1, size3;
    private Deque<Integer> q = new ArrayDeque<>();
    private TreeMap<Integer, Integer> lo = new TreeMap<>();
    private TreeMap<Integer, Integer> mid = new TreeMap<>();
    private TreeMap<Integer, Integer> hi = new TreeMap<>();

    public MKAverage(int m, int k) {
        this.m = m;
        this.k = k;
    }

    public void addElement(int num) {
        if (lo.isEmpty() || num <= lo.lastKey()) {
            lo.merge(num, 1, Integer::sum);
            ++size1;
        } else if (hi.isEmpty() || num >= hi.firstKey()) {
            hi.merge(num, 1, Integer::sum);
            ++size3;
        } else {
            mid.merge(num, 1, Integer::sum);
            s += num;
        }
        q.offer(num);
        if (q.size() > m) {
            int x = q.poll();
            if (lo.containsKey(x)) {
                if (lo.merge(x, -1, Integer::sum) == 0) {
                    lo.remove(x);
                }
                --size1;
            } else if (hi.containsKey(x)) {
                if (hi.merge(x, -1, Integer::sum) == 0) {
                    hi.remove(x);
                }
                --size3;
            } else {
                if (mid.merge(x, -1, Integer::sum) == 0) {
                    mid.remove(x);
                }
                s -= x;
            }
        }
        for (; size1 > k; --size1) {
            int x = lo.lastKey();
            if (lo.merge(x, -1, Integer::sum) == 0) {
                lo.remove(x);
            }
            mid.merge(x, 1, Integer::sum);
            s += x;
        }
        for (; size3 > k; --size3) {
            int x = hi.firstKey();
            if (hi.merge(x, -1, Integer::sum) == 0) {
                hi.remove(x);
            }
            mid.merge(x, 1, Integer::sum);
            s += x;
        }
        for (; size1 < k && !mid.isEmpty(); ++size1) {
            int x = mid.firstKey();
            if (mid.merge(x, -1, Integer::sum) == 0) {
                mid.remove(x);
            }
            s -= x;
            lo.merge(x, 1, Integer::sum);
        }
        for (; size3 < k && !mid.isEmpty(); ++size3) {
            int x = mid.lastKey();
            if (mid.merge(x, -1, Integer::sum) == 0) {
                mid.remove(x);
            }
            s -= x;
            hi.merge(x, 1, Integer::sum);
        }
    }

    public int calculateMKAverage() {
        return q.size() < m ? -1 : (int) (s / (q.size() - k * 2));
    }
}

/**
 * Your MKAverage object will be instantiated and called as such:
 * MKAverage obj = new MKAverage(m, k);
 * obj.addElement(num);
 * int param_2 = obj.calculateMKAverage();
 */
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- solution:start -->

### Solution 2

<!-- solution:end -->

<!-- problem:end -->
