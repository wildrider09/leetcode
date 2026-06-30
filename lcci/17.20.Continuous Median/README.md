---
comments: true
difficulty: Hard
edit_url: https://github.com/doocs/leetcode/edit/main/lcci/17.20.Continuous%20Median/README_EN.md
---

<!-- problem:start -->

# [17.20. Continuous Median](https://leetcode.cn/problems/continuous-median-lcci)

[Chinese Version](/lcci/17.20.Continuous%20Median/README.md)

## Description

<!-- description:start -->

<p>Numbers are randomly generated and passed to a method. Write a program to find and maintain the median value as new values are generated.</p>

<p>Median is the middle value in an ordered integer list. If the size of the list is even, there is no middle value. So the median is the mean of the two middle value.</p>

<p>For example,</p>

<p>[2,3,4], the median is&nbsp;3</p>

<p>[2,3], the median is (2 + 3) / 2 = 2.5</p>

<p>Design a data structure that supports the following two operations:</p>

<ul>
	<li>void addNum(int num) - Add a integer number from the data stream to the data structure.</li>
	<li>double findMedian() - Return the median of all elements so far.</li>
</ul>

<p><strong>Example: </strong></p>

<pre>

addNum(1)

addNum(2)

findMedian() -&gt; 1.5

addNum(3)

findMedian() -&gt; 2

</pre>

<!-- description:end -->

## Solutions

<!-- solution:start -->

### Solution 1: Min Heap and Max Heap (Priority Queue)

We can use two heaps to maintain all the elements, a min heap $\textit{minQ}$ and a max heap $\textit{maxQ}$, where the min heap $\textit{minQ}$ stores the larger half, and the max heap $\textit{maxQ}$ stores the smaller half.

When calling the `addNum` method, we first add the element to the max heap $\textit{maxQ}$, then pop the top element of $\textit{maxQ}$ and add it to the min heap $\textit{minQ}$. If at this time the size difference between $\textit{minQ}$ and $\textit{maxQ}$ is greater than $1$, we pop the top element of $\textit{minQ}$ and add it to $\textit{maxQ}$. The time complexity is $O(\log n)$.

When calling the `findMedian` method, if the size of $\textit{minQ}$ is equal to the size of $\textit{maxQ}$, it means the total number of elements is even, and we can return the average value of the top elements of $\textit{minQ}$ and $\textit{maxQ}$; otherwise, we return the top element of $\textit{minQ}$. The time complexity is $O(1)$.

The space complexity is $O(n)$, where $n$ is the number of elements.

<!-- tabs:start -->

#### Java

```java
class MedianFinder {
    private PriorityQueue<Integer> minQ = new PriorityQueue<>();
    private PriorityQueue<Integer> maxQ = new PriorityQueue<>(Collections.reverseOrder());

    public MedianFinder() {
    }

    public void addNum(int num) {
        maxQ.offer(num);
        minQ.offer(maxQ.poll());
        if (minQ.size() - maxQ.size() > 1) {
            maxQ.offer(minQ.poll());
        }
    }

    public double findMedian() {
        return minQ.size() == maxQ.size() ? (minQ.peek() + maxQ.peek()) / 2.0 : minQ.peek();
    }
}

/**
 * Your MedianFinder object will be instantiated and called as such:
 * MedianFinder obj = new MedianFinder();
 * obj.addNum(num);
 * double param_2 = obj.findMedian();
 */
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
