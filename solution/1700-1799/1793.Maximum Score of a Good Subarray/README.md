---
comments: true
difficulty: Hard
edit_url: https://github.com/doocs/leetcode/edit/main/solution/1700-1799/1793.Maximum%20Score%20of%20a%20Good%20Subarray/README_EN.md
rating: 1945
source: Weekly Contest 232 Q4
tags:
    - Stack
    - Array
    - Two Pointers
    - Binary Search
    - Monotonic Stack
---

<!-- problem:start -->

# [1793. Maximum Score of a Good Subarray](https://leetcode.com/problems/maximum-score-of-a-good-subarray)

[Chinese Version](/solution/1700-1799/1793.Maximum%20Score%20of%20a%20Good%20Subarray/README.md)

## Description

<!-- description:start -->

<p>You are given an array of integers <code>nums</code> <strong>(0-indexed)</strong> and an integer <code>k</code>.</p>

<p>The <strong>score</strong> of a subarray <code>(i, j)</code> is defined as <code>min(nums[i], nums[i+1], ..., nums[j]) * (j - i + 1)</code>. A <strong>good</strong> subarray is a subarray where <code>i &lt;= k &lt;= j</code>.</p>

<p>Return <em>the maximum possible <strong>score</strong> of a <strong>good</strong> subarray.</em></p>

<p>&nbsp;</p>
<p><strong class="example">Example 1:</strong></p>

<pre>
<strong>Input:</strong> nums = [1,4,3,7,4,5], k = 3
<strong>Output:</strong> 15
<strong>Explanation:</strong> The optimal subarray is (1, 5) with a score of min(4,3,7,4,5) * (5-1+1) = 3 * 5 = 15. 
</pre>

<p><strong class="example">Example 2:</strong></p>

<pre>
<strong>Input:</strong> nums = [5,5,4,5,4,1,1,1], k = 0
<strong>Output:</strong> 20
<strong>Explanation:</strong> The optimal subarray is (0, 4) with a score of min(5,5,4,5,4) * (4-0+1) = 4 * 5 = 20.
</pre>

<p>&nbsp;</p>
<p><strong>Constraints:</strong></p>

<ul>
	<li><code>1 &lt;= nums.length &lt;= 10<sup>5</sup></code></li>
	<li><code>1 &lt;= nums[i] &lt;= 2 * 10<sup>4</sup></code></li>
	<li><code>0 &lt;= k &lt; nums.length</code></li>
</ul>

<!-- description:end -->

## Solutions

<!-- solution:start -->

### Solution 1: Monotonic Stack

We can enumerate each element $nums[i]$ in $nums$ as the minimum value of the subarray, and use a monotonic stack to find the first position $left[i]$ on the left that is less than $nums[i]$ and the first position $right[i]$ on the right that is less than or equal to $nums[i]$. Then, the score of the subarray with $nums[i]$ as the minimum value is $nums[i] \times (right[i] - left[i] - 1)$.

It should be noted that the answer can only be updated when the left and right boundaries $left[i]$ and $right[i]$ satisfy $left[i]+1 \leq k \leq right[i]-1$.

The time complexity is $O(n)$, and the space complexity is $O(n)$. Here, $n$ is the length of the array $nums$.

<!-- tabs:start -->

#### Java

```java
class Solution {
    public int maximumScore(int[] nums, int k) {
        int n = nums.length;
        int[] left = new int[n];
        int[] right = new int[n];
        Arrays.fill(left, -1);
        Arrays.fill(right, n);
        Deque<Integer> stk = new ArrayDeque<>();
        for (int i = 0; i < n; ++i) {
            int v = nums[i];
            while (!stk.isEmpty() && nums[stk.peek()] >= v) {
                stk.pop();
            }
            if (!stk.isEmpty()) {
                left[i] = stk.peek();
            }
            stk.push(i);
        }
        stk.clear();
        for (int i = n - 1; i >= 0; --i) {
            int v = nums[i];
            while (!stk.isEmpty() && nums[stk.peek()] > v) {
                stk.pop();
            }
            if (!stk.isEmpty()) {
                right[i] = stk.peek();
            }
            stk.push(i);
        }
        int ans = 0;
        for (int i = 0; i < n; ++i) {
            if (left[i] + 1 <= k && k <= right[i] - 1) {
                ans = Math.max(ans, nums[i] * (right[i] - left[i] - 1));
            }
        }
        return ans;
    }
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- solution:start -->

### Solution 2: Two Pointers

We can initialize two pointers at the core index `k` and expand outward to the left and right.
By maintaining the minimum value within current window, we can find maximum score in strict linear time.

**Algorithm Steps:**

1. Initialize left pointer `i = k`, right pointer `j = k`, and window minimum value `min_num = nums[k]`. Set initial maximum score `max_score = nums[k]`.

2. Expand pointers while `i > 0` or `j < len(nums) - 1`:
    - **Direction**: If left boundary can't expand (`i == 0`), move right pointer `j++`. If right boundary can't expand (`j == len(nums) - 1`), move left pointer `i--`.
    - If both sides are expandable, compare `nums[i - 1]` and `nums[j + 1]`, and expand towards the side with a larger value (i.e., if `nums[i - 1] >= nums[j + 1]`, decrement `i`; otherwise, increment `j`).

3. **Update State**: after each pointer movement, update current window minimum value: `min_num = min(min_num, nums[i] or nums[j])`.

4. **Calculate Score**: length of the current good subarray is `j + 1 - i`, and its score is `score = min_num * (j + 1 - i)`. Update global maximum score: `max_score = max(max_score, score)`.

5. Return `max_score` once the for loop terminates.

**Complexity Analysis:**

- **Time Complexity**: $O(n)$, where $n$ is length of array `nums`. Each element is scanned at most once.
- **Space Complexity**: $O(1)$, as it only requires a constant amount of extra space for two pointers, window minimum value and global maximum score.

<!-- solution:end -->

<!-- problem:end -->
