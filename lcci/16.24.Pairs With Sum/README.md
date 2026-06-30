---
comments: true
difficulty: Medium
edit_url: https://github.com/doocs/leetcode/edit/main/lcci/16.24.Pairs%20With%20Sum/README_EN.md
---

<!-- problem:start -->

# [16.24. Pairs With Sum](https://leetcode.cn/problems/pairs-with-sum-lcci)

## Description

<!-- description:start -->

<p>Design an algorithm to find all pairs of integers within an array which sum to a specified value.</p>
<p><strong>Example 1:</strong></p>
<pre>

<strong>Input:</strong> nums = [5,6,5], target = 11

<strong>Output: </strong>[[5,6]]</pre>

<p><strong>Example 2:</strong></p>
<pre>

<strong>Input:</strong> nums = [5,6,5,6], target = 11

<strong>Output: </strong>[[5,6],[5,6]]</pre>

<p><strong>Note: </strong></p>
<ul>
	<li><code>nums.length &lt;= 100000</code></li>
</ul>

<!-- description:end -->

## Solutions

<!-- solution:start -->

### Solution 1: Hash Table

We can use a hash table to store the elements in the array, with the keys being the elements in the array and the values being the number of times the element appears.

We traverse the array, and for each element $x$, we calculate $y = target - x$. If $y$ exists in the hash table, it means that there is a pair of numbers $(x, y)$ that add up to the target, and we add it to the answer and reduce the count of $y$ by $1$. If $y$ does not exist in the hash table, it means that there is no such pair of numbers, and we increase the count of $x$ by $1$.

After the traversal, we can obtain the answer.

The time complexity is $O(n)$, and the space complexity is $O(n)$. Here, $n$ is the length of the array.

<!-- tabs:start -->

#### Java

```java
class Solution {
    public List<List<Integer>> pairSums(int[] nums, int target) {
        Map<Integer, Integer> cnt = new HashMap<>();
        List<List<Integer>> ans = new ArrayList<>();
        for (int x : nums) {
            int y = target - x;
            if (cnt.containsKey(y)) {
                ans.add(List.of(x, y));
                if (cnt.merge(y, -1, Integer::sum) == 0) {
                    cnt.remove(y);
                }
            } else {
                cnt.merge(x, 1, Integer::sum);
            }
        }
        return ans;
    }
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
