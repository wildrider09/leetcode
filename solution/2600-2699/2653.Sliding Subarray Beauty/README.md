---
comments: true
difficulty: Medium
edit_url: https://github.com/doocs/leetcode/edit/main/solution/2600-2699/2653.Sliding%20Subarray%20Beauty/README_EN.md
rating: 1785
source: Weekly Contest 342 Q3
tags:
    - Array
    - Hash Table
    - Sliding Window
---

<!-- problem:start -->

# [2653. Sliding Subarray Beauty](https://leetcode.com/problems/sliding-subarray-beauty)

[Chinese Version](/solution/2600-2699/2653.Sliding%20Subarray%20Beauty/README.md)

## Description

<!-- description:start -->

<p>Given an integer array <code>nums</code> containing <code>n</code> integers, find the <strong>beauty</strong> of each subarray of size <code>k</code>.</p>

<p>The <strong>beauty</strong> of a subarray is the <code>x<sup>th</sup></code><strong> smallest integer </strong>in the subarray if it is <strong>negative</strong>, or <code>0</code> if there are fewer than <code>x</code> negative integers.</p>

<p>Return <em>an integer array containing </em><code>n - k + 1</code> <em>integers, which denote the </em><strong>beauty</strong><em> of the subarrays <strong>in order</strong> from the first index in the array.</em></p>

<ul>
	<li>
	<p>A subarray is a contiguous <strong>non-empty</strong> sequence of elements within an array.</p>
	</li>
</ul>

<p>&nbsp;</p>
<p><strong class="example">Example 1:</strong></p>

<pre>
<strong>Input:</strong> nums = [1,-1,-3,-2,3], k = 3, x = 2
<strong>Output:</strong> [-1,-2,-2]
<strong>Explanation:</strong> There are 3 subarrays with size k = 3. 
The first subarray is <code>[1, -1, -3]</code> and the 2<sup>nd</sup> smallest negative integer is -1.&nbsp;
The second subarray is <code>[-1, -3, -2]</code> and the 2<sup>nd</sup> smallest negative integer is -2.&nbsp;
The third subarray is <code>[-3, -2, 3]&nbsp;</code>and the 2<sup>nd</sup> smallest negative integer is -2.</pre>

<p><strong class="example">Example 2:</strong></p>

<pre>
<strong>Input:</strong> nums = [-1,-2,-3,-4,-5], k = 2, x = 2
<strong>Output:</strong> [-1,-2,-3,-4]
<strong>Explanation:</strong> There are 4 subarrays with size k = 2.
For <code>[-1, -2]</code>, the 2<sup>nd</sup> smallest negative integer is -1.
For <code>[-2, -3]</code>, the 2<sup>nd</sup> smallest negative integer is -2.
For <code>[-3, -4]</code>, the 2<sup>nd</sup> smallest negative integer is -3.
For <code>[-4, -5]</code>, the 2<sup>nd</sup> smallest negative integer is -4.&nbsp;</pre>

<p><strong class="example">Example 3:</strong></p>

<pre>
<strong>Input:</strong> nums = [-3,1,2,-3,0,-3], k = 2, x = 1
<strong>Output:</strong> [-3,0,-3,-3,-3]
<strong>Explanation:</strong> There are 5 subarrays with size k = 2<strong>.</strong>
For <code>[-3, 1]</code>, the 1<sup>st</sup> smallest negative integer is -3.
For <code>[1, 2]</code>, there is no negative integer so the beauty is 0.
For <code>[2, -3]</code>, the 1<sup>st</sup> smallest negative integer is -3.
For <code>[-3, 0]</code>, the 1<sup>st</sup> smallest negative integer is -3.
For <code>[0, -3]</code>, the 1<sup>st</sup> smallest negative integer is -3.</pre>

<p>&nbsp;</p>
<p><strong>Constraints:</strong></p>

<ul>
	<li><code>n == nums.length&nbsp;</code></li>
	<li><code>1 &lt;= n &lt;= 10<sup>5</sup></code></li>
	<li><code>1 &lt;= k &lt;= n</code></li>
	<li><code>1 &lt;= x &lt;= k&nbsp;</code></li>
	<li><code>-50&nbsp;&lt;= nums[i] &lt;= 50&nbsp;</code></li>
</ul>

<!-- description:end -->

## Solutions

<!-- solution:start -->

### Solution 1: Sliding Window

We notice that the range of elements in the array $nums$ is $[-50,50]$. Therefore, we can use an array of length $101$, denoted as $cnt$, to count the occurrences of each number in $[-50,50]$. Due to the presence of negative numbers, we can add $50$ to each number to make them all non-negative, so we can use the array $cnt$ to count the occurrences of each number.

Next, we traverse the array $nums$, maintaining a sliding window of length $k$. The occurrence times of all elements in the window are recorded in the array $cnt$. Then we traverse the array $cnt$ to find the $x$-th smallest number, which is the beauty value of the current sliding window. If there is no $x$-th smallest number, then the beauty value is $0$.

The time complexity is $O(n \times 50)$, and the space complexity is $O(100)$. Where $n$ is the length of the array $nums$.

<!-- tabs:start -->

#### Java

```java
class Solution {
    public int[] getSubarrayBeauty(int[] nums, int k, int x) {
        int n = nums.length;
        int[] cnt = new int[101];
        for (int i = 0; i < k; ++i) {
            ++cnt[nums[i] + 50];
        }
        int[] ans = new int[n - k + 1];
        ans[0] = f(cnt, x);
        for (int i = k, j = 1; i < n; ++i) {
            ++cnt[nums[i] + 50];
            --cnt[nums[i - k] + 50];
            ans[j++] = f(cnt, x);
        }
        return ans;
    }

    private int f(int[] cnt, int x) {
        int s = 0;
        for (int i = 0; i < 50; ++i) {
            s += cnt[i];
            if (s >= x) {
                return i - 50;
            }
        }
        return 0;
    }
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- solution:start -->

### Solution 2: Double Priority Queue (Min-Max Heap) + Delayed Deletion

We can use two priority queues (min-max heap) to maintain the elements in the current window, one priority queue stores the smaller $x$ elements in the current window, and the other priority queue stores the larger $k - x$ elements in the current window. We also need a delayed deletion dictionary `delayed` to record whether the elements in the current window need to be deleted.

We design a class `MedianFinder` to maintain the elements in the current window. This class includes the following methods:

- `add_num(num)`: Add `num` to the current window.
- `find()`: Return the beauty value of the current window.
- `remove_num(num)`: Remove `num` from the current window.
- `prune(pq)`: If the top element of the heap is in the delayed deletion dictionary `delayed`, pop it from the top of the heap and subtract one from its delayed deletion count. If the delayed deletion count of the element is zero, delete it from the delayed deletion dictionary.
- `rebalance()`: Balance the size of the two priority queues.

In the `add_num(num)` method, we first consider adding `num` to the smaller queue. If the count is greater than $x$ or `num` is less than or equal to the top element of the smaller queue, add `num` to the smaller queue; otherwise, add `num` to the larger queue. Then we call the `rebalance()` method to ensure that the number of elements in the smaller queue does not exceed $x$.

In the `remove_num(num)` method, we increase the delayed deletion count of `num` by one. Then we compare `num` with the top element of the smaller queue. If `num` is less than or equal to the top element of the smaller queue, update the size of the smaller queue and call the `prune()` method to ensure that the top element of the smaller queue is not in the delayed deletion dictionary. Otherwise, we update the size of the larger queue and call the `prune()` method to ensure that the top element of the larger queue is not in the delayed deletion dictionary.

In the `find()` method, if the size of the smaller queue is equal to $x$, return the top element of the smaller queue, otherwise return $0$.

In the `prune(pq)` method, if the top element of the heap is in the delayed deletion dictionary, pop it from the top of the heap and subtract one from its delayed deletion count. If the delayed deletion count of the element is zero, delete it from the delayed deletion dictionary.

In the `rebalance()` method, if the size of the smaller queue is greater than $x$, add the top element of the smaller queue to the larger queue and call the `prune()` method to ensure that the top element of the smaller queue is not in the delayed deletion dictionary. If the size of the smaller queue is less than $x$ and the size of the larger queue is greater than $0$, add the top element of the larger queue to the smaller queue and call the `prune()` method to ensure that the top element of the larger queue is not in the delayed deletion dictionary.

The time complexity is $O(n \times \log n)$, and the space complexity is $O(n)$. Where $n$ is the length of the array `nums`.

Similar problems:

- [480. Sliding Window Median](https://github.com/doocs/leetcode/blob/main/solution/0400-0499/0480.Sliding%20Window%20Median/README.md)

<!-- tabs:start -->

#### Java

```java
class MedianFinder {
    private PriorityQueue<Integer> small = new PriorityQueue<>(Comparator.reverseOrder());
    private PriorityQueue<Integer> large = new PriorityQueue<>();
    private Map<Integer, Integer> delayed = new HashMap<>();
    private int smallSize;
    private int largeSize;
    private int x;

    public MedianFinder(int x) {
        this.x = x;
    }

    public void addNum(int num) {
        if (smallSize < x || num <= small.peek()) {
            small.offer(num);
            ++smallSize;
        } else {
            large.offer(num);
            ++largeSize;
        }
        rebalance();
    }

    public int find() {
        return smallSize == x ? small.peek() : 0;
    }

    public void removeNum(int num) {
        delayed.merge(num, 1, Integer::sum);
        if (num <= small.peek()) {
            --smallSize;
            if (num == small.peek()) {
                prune(small);
            }
        } else {
            --largeSize;
            if (num == large.peek()) {
                prune(large);
            }
        }
        rebalance();
    }

    private void prune(PriorityQueue<Integer> pq) {
        while (!pq.isEmpty() && delayed.containsKey(pq.peek())) {
            if (delayed.merge(pq.peek(), -1, Integer::sum) == 0) {
                delayed.remove(pq.peek());
            }
            pq.poll();
        }
    }

    private void rebalance() {
        if (smallSize > x) {
            large.offer(small.poll());
            --smallSize;
            ++largeSize;
            prune(small);
        } else if (smallSize < x && largeSize > 0) {
            small.offer(large.poll());
            --largeSize;
            ++smallSize;
            prune(large);
        }
    }
}

class Solution {
    public int[] getSubarrayBeauty(int[] nums, int k, int x) {
        MedianFinder finder = new MedianFinder(x);
        for (int i = 0; i < k; ++i) {
            if (nums[i] < 0) {
                finder.addNum(nums[i]);
            }
        }
        int n = nums.length;
        int[] ans = new int[n - k + 1];
        ans[0] = finder.find();
        for (int i = k; i < n; ++i) {
            if (nums[i] < 0) {
                finder.addNum(nums[i]);
            }
            if (nums[i - k] < 0) {
                finder.removeNum(nums[i - k]);
            }
            ans[i - k + 1] = finder.find();
        }
        return ans;
    }
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
