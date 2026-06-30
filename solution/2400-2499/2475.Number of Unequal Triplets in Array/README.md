---
comments: true
difficulty: Easy
edit_url: https://github.com/doocs/leetcode/edit/main/solution/2400-2499/2475.Number%20of%20Unequal%20Triplets%20in%20Array/README_EN.md
rating: 1255
source: Weekly Contest 320 Q1
tags:
    - Array
    - Hash Table
    - Sorting
---

<!-- problem:start -->

# [2475. Number of Unequal Triplets in Array](https://leetcode.com/problems/number-of-unequal-triplets-in-array)

[Chinese Version](/solution/2400-2499/2475.Number%20of%20Unequal%20Triplets%20in%20Array/README.md)

## Description

<!-- description:start -->

<p>You are given a <strong>0-indexed</strong> array of positive integers <code>nums</code>. Find the number of triplets <code>(i, j, k)</code> that meet the following conditions:</p>

<ul>
	<li><code>0 &lt;= i &lt; j &lt; k &lt; nums.length</code></li>
	<li><code>nums[i]</code>, <code>nums[j]</code>, and <code>nums[k]</code> are <strong>pairwise distinct</strong>.
	<ul>
		<li>In other words, <code>nums[i] != nums[j]</code>, <code>nums[i] != nums[k]</code>, and <code>nums[j] != nums[k]</code>.</li>
	</ul>
	</li>
</ul>

<p>Return <em>the number of triplets that meet the conditions.</em></p>

<p>&nbsp;</p>
<p><strong class="example">Example 1:</strong></p>

<pre>
<strong>Input:</strong> nums = [4,4,2,4,3]
<strong>Output:</strong> 3
<strong>Explanation:</strong> The following triplets meet the conditions:
- (0, 2, 4) because 4 != 2 != 3
- (1, 2, 4) because 4 != 2 != 3
- (2, 3, 4) because 2 != 4 != 3
Since there are 3 triplets, we return 3.
Note that (2, 0, 4) is not a valid triplet because 2 &gt; 0.
</pre>

<p><strong class="example">Example 2:</strong></p>

<pre>
<strong>Input:</strong> nums = [1,1,1,1,1]
<strong>Output:</strong> 0
<strong>Explanation:</strong> No triplets meet the conditions so we return 0.
</pre>

<p>&nbsp;</p>
<p><strong>Constraints:</strong></p>

<ul>
	<li><code>3 &lt;= nums.length &lt;= 100</code></li>
	<li><code>1 &lt;= nums[i] &lt;= 1000</code></li>
</ul>

<!-- description:end -->

## Solutions

<!-- solution:start -->

### Solution 1: Brute Force Enumeration

We can directly enumerate all triples $(i, j, k)$ and count all the ones that meet the conditions.

The time complexity is $O(n^3)$, where $n$ is the length of the array $nums$. The space complexity is $O(1)$.

<!-- tabs:start -->

#### Java

```java
class Solution {
    public int unequalTriplets(int[] nums) {
        int n = nums.length;
        int ans = 0;
        for (int i = 0; i < n; ++i) {
            for (int j = i + 1; j < n; ++j) {
                for (int k = j + 1; k < n; ++k) {
                    if (nums[i] != nums[j] && nums[j] != nums[k] && nums[i] != nums[k]) {
                        ++ans;
                    }
                }
            }
        }
        return ans;
    }
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- solution:start -->

### Solution 2: Sorting + Enumeration of Middle Elements + Binary Search

We can also sort the array $nums$ first.

Then traverse $nums$, enumerate the middle element $nums[j]$, and use binary search to find the nearest index $i$ on the left side of $nums[j]$ such that $nums[i] < nums[j]$; find the nearest index $k$ on the right side of $nums[j]$ such that $nums[k] > nums[j]$. Then the number of triples with $nums[j]$ as the middle element and meeting the conditions is $(i + 1) \times (n - k)$, which is added to the answer.

The time complexity is $O(n \times \log n)$, and the space complexity is $O(\log n)$. Here, $n$ is the length of the array $nums$.

<!-- tabs:start -->

#### Java

```java
class Solution {
    public int unequalTriplets(int[] nums) {
        Arrays.sort(nums);
        int ans = 0, n = nums.length;
        for (int j = 1; j < n - 1; ++j) {
            int i = search(nums, nums[j], 0, j) - 1;
            int k = search(nums, nums[j] + 1, j + 1, n);
            if (i >= 0 && k < n) {
                ans += (i + 1) * (n - k);
            }
        }
        return ans;
    }

    private int search(int[] nums, int x, int left, int right) {
        while (left < right) {
            int mid = (left + right) >> 1;
            if (nums[mid] >= x) {
                right = mid;
            } else {
                left = mid + 1;
            }
        }
        return left;
    }
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- solution:start -->

### Solution 3: Hash Table

We can also use a hash table $cnt$ to count the number of each element in the array $nums$.

Then traverse the hash table $cnt$, enumerate the number of middle elements $b$, and denote the number of elements on the left as $a$. Then the number of elements on the right is $c = n - a - b$. At this time, the number of triples that meet the conditions is $a \times b \times c$, which is added to the answer. Then update $a = a + b$ and continue to enumerate the number of middle elements $b$.

The time complexity is $O(n)$, and the space complexity is $O(n)$. Here, $n$ is the length of the array $nums$.

<!-- tabs:start -->

#### Java

```java
class Solution {
    public int unequalTriplets(int[] nums) {
        Map<Integer, Integer> cnt = new HashMap<>();
        for (int v : nums) {
            cnt.merge(v, 1, Integer::sum);
        }
        int ans = 0, a = 0;
        int n = nums.length;
        for (int b : cnt.values()) {
            int c = n - a - b;
            ans += a * b * c;
            a += b;
        }
        return ans;
    }
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- solution:start -->

### Solution 4

<!-- solution:end -->

<!-- problem:end -->
