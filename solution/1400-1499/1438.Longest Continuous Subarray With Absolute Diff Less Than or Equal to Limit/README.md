---
comments: true
difficulty: Medium
edit_url: https://github.com/doocs/leetcode/edit/main/solution/1400-1499/1438.Longest%20Continuous%20Subarray%20With%20Absolute%20Diff%20Less%20Than%20or%20Equal%20to%20Limit/README_EN.md
rating: 1672
source: Weekly Contest 187 Q3
tags:
    - Queue
    - Array
    - Ordered Set
    - Sliding Window
    - Monotonic Queue
    - Heap (Priority Queue)
---

<!-- problem:start -->

# [1438. Longest Continuous Subarray With Absolute Diff Less Than or Equal to Limit](https://leetcode.com/problems/longest-continuous-subarray-with-absolute-diff-less-than-or-equal-to-limit)

[Chinese Version](/solution/1400-1499/1438.Longest%20Continuous%20Subarray%20With%20Absolute%20Diff%20Less%20Than%20or%20Equal%20to%20Limit/README.md)

## Description

<!-- description:start -->

<p>Given an array of integers <code>nums</code> and an integer <code>limit</code>, return the size of the longest <strong>non-empty</strong> subarray such that the absolute difference between any two elements of this subarray is less than or equal to <code>limit</code><em>.</em></p>

<p>&nbsp;</p>
<p><strong class="example">Example 1:</strong></p>

<pre>
<strong>Input:</strong> nums = [8,2,4,7], limit = 4
<strong>Output:</strong> 2 
<strong>Explanation:</strong> All subarrays are: 
[8] with maximum absolute diff |8-8| = 0 &lt;= 4.
[8,2] with maximum absolute diff |8-2| = 6 &gt; 4. 
[8,2,4] with maximum absolute diff |8-2| = 6 &gt; 4.
[8,2,4,7] with maximum absolute diff |8-2| = 6 &gt; 4.
[2] with maximum absolute diff |2-2| = 0 &lt;= 4.
[2,4] with maximum absolute diff |2-4| = 2 &lt;= 4.
[2,4,7] with maximum absolute diff |2-7| = 5 &gt; 4.
[4] with maximum absolute diff |4-4| = 0 &lt;= 4.
[4,7] with maximum absolute diff |4-7| = 3 &lt;= 4.
[7] with maximum absolute diff |7-7| = 0 &lt;= 4. 
Therefore, the size of the longest subarray is 2.
</pre>

<p><strong class="example">Example 2:</strong></p>

<pre>
<strong>Input:</strong> nums = [10,1,2,4,7,2], limit = 5
<strong>Output:</strong> 4 
<strong>Explanation:</strong> The subarray [2,4,7,2] is the longest since the maximum absolute diff is |2-7| = 5 &lt;= 5.
</pre>

<p><strong class="example">Example 3:</strong></p>

<pre>
<strong>Input:</strong> nums = [4,2,2,2,4,4,2,2], limit = 0
<strong>Output:</strong> 3
</pre>

<p>&nbsp;</p>
<p><strong>Constraints:</strong></p>

<ul>
	<li><code>1 &lt;= nums.length &lt;= 10<sup>5</sup></code></li>
	<li><code>1 &lt;= nums[i] &lt;= 10<sup>9</sup></code></li>
	<li><code>0 &lt;= limit &lt;= 10<sup>9</sup></code></li>
</ul>

<!-- description:end -->

## Solutions

<!-- solution:start -->

### Solution 1: Ordered Set + Sliding Window

We can enumerate each position as the right endpoint of the subarray, and find the leftmost left endpoint corresponding to it, such that the difference between the maximum and minimum values in the interval does not exceed $limit$. During the process, we use an ordered set to maintain the maximum and minimum values within the window.

The time complexity is $O(n \log n)$, and the space complexity is $O(n)$. Here, $n$ is the length of the array `nums`.

<!-- tabs:start -->

#### Java

```java
class Solution {
    public int longestSubarray(int[] nums, int limit) {
        TreeMap<Integer, Integer> tm = new TreeMap<>();
        int ans = 0;
        for (int i = 0, j = 0; i < nums.length; ++i) {
            tm.merge(nums[i], 1, Integer::sum);
            for (; tm.lastKey() - tm.firstKey() > limit; ++j) {
                if (tm.merge(nums[j], -1, Integer::sum) == 0) {
                    tm.remove(nums[j]);
                }
            }
            ans = Math.max(ans, i - j + 1);
        }
        return ans;
    }
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- solution:start -->

### Solution 2: Binary Search + Sliding Window

We notice that if a subarray of length $k$ satisfies the condition, then a subarray of length $k' < k$ also satisfies the condition. This shows a monotonicity, therefore, we can use binary search to find the longest subarray that satisfies the condition.

We define the left boundary of the binary search as $l = 0$, and the right boundary as $r = n$. For each $mid = \frac{l + r + 1}{2}$, we check whether there exists a subarray of length $mid$ that satisfies the condition. If it exists, we update $l = mid$, otherwise we update $r = mid - 1$. The problem is transformed into whether there exists a subarray of length $mid$ in the array that satisfies the condition, which is actually to find the difference between the maximum and minimum values in the sliding window does not exceed $limit$. We can use two monotonic queues to maintain the maximum and minimum values in the window respectively.

The time complexity is $O(n \times \log n)$, and the space complexity is $O(n)$. Where $n$ is the length of the array $nums$.

<!-- tabs:start -->

#### Java

```java
class Solution {
    private int[] nums;
    private int limit;

    public int longestSubarray(int[] nums, int limit) {
        this.nums = nums;
        this.limit = limit;
        int l = 1, r = nums.length;
        while (l < r) {
            int mid = (l + r + 1) >> 1;
            if (check(mid)) {
                l = mid;
            } else {
                r = mid - 1;
            }
        }
        return l;
    }

    private boolean check(int k) {
        Deque<Integer> minQ = new ArrayDeque<>();
        Deque<Integer> maxQ = new ArrayDeque<>();
        for (int i = 0; i < nums.length; ++i) {
            if (!minQ.isEmpty() && i - minQ.peekFirst() + 1 > k) {
                minQ.pollFirst();
            }
            if (!maxQ.isEmpty() && i - maxQ.peekFirst() + 1 > k) {
                maxQ.pollFirst();
            }
            while (!minQ.isEmpty() && nums[minQ.peekLast()] >= nums[i]) {
                minQ.pollLast();
            }
            while (!maxQ.isEmpty() && nums[maxQ.peekLast()] <= nums[i]) {
                maxQ.pollLast();
            }
            minQ.offer(i);
            maxQ.offer(i);
            if (i >= k - 1 && nums[maxQ.peekFirst()] - nums[minQ.peekFirst()] <= limit) {
                return true;
            }
        }
        return false;
    }
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- solution:start -->

### Solution 3: Sliding Window + Deque

We can use a deque to maintain the maximum and minimum values within the window. We maintain two deques, one for storing the indices of the maximum values and the other for the minimum values within the window. Define two pointers $l$ and $r$ to point to the left and right boundaries of the window, respectively.

Each time we move the right boundary $r$ to the right, we check if the element corresponding to the tail index of the maximum value deque is less than the current element. If it is, we dequeue the tail element until the element corresponding to the tail of the maximum value deque is not less than the current element. Similarly, we check if the element corresponding to the tail index of the minimum value deque is greater than the current element. If it is, we dequeue the tail element until the element corresponding to the tail of the minimum value deque is not greater than the current element. Then, we enqueue the current element's index.

If the difference between the elements at the front of the maximum value deque and the minimum value deque is greater than $limit$, then we move the left boundary $l$ to the right. If the element at the front of the maximum value deque is less than $l$, we dequeue the front element of the maximum value deque. Similarly, if the element at the front of the minimum value deque is less than $l$, we dequeue the front element of the minimum value deque.

The answer is $n - l$.

The time complexity is $O(n)$, and the space complexity is $O(n)$. Here, $n$ is the length of the array $nums$.

<!-- tabs:start -->

#### Java

```java
class Solution {
    public int longestSubarray(int[] nums, int limit) {
        Deque<Integer> maxQ = new ArrayDeque<>();
        Deque<Integer> minQ = new ArrayDeque<>();
        int n = nums.length;
        int l = 0;
        for (int r = 0; r < n; ++r) {
            while (!maxQ.isEmpty() && nums[maxQ.peekLast()] < nums[r]) {
                maxQ.pollLast();
            }
            while (!minQ.isEmpty() && nums[minQ.peekLast()] > nums[r]) {
                minQ.pollLast();
            }
            maxQ.offerLast(r);
            minQ.offerLast(r);
            if (nums[maxQ.peekFirst()] - nums[minQ.peekFirst()] > limit) {
                ++l;
                if (maxQ.peekFirst() < l) {
                    maxQ.pollFirst();
                }
                if (minQ.peekFirst() < l) {
                    minQ.pollFirst();
                }
            }
        }
        return n - l;
    }
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
