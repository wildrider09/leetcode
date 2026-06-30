---
comments: true
difficulty: Medium
edit_url: https://github.com/doocs/leetcode/edit/main/solution/2500-2599/2513.Minimize%20the%20Maximum%20of%20Two%20Arrays/README_EN.md
rating: 2302
source: Biweekly Contest 94 Q3
tags:
    - Math
    - Binary Search
    - Number Theory
---

<!-- problem:start -->

# [2513. Minimize the Maximum of Two Arrays](https://leetcode.com/problems/minimize-the-maximum-of-two-arrays)

[Chinese Version](/solution/2500-2599/2513.Minimize%20the%20Maximum%20of%20Two%20Arrays/README.md)

## Description

<!-- description:start -->

<p>We have two arrays <code>arr1</code> and <code>arr2</code> which are initially empty. You need to add positive integers to them such that they satisfy all the following conditions:</p>

<ul>
	<li><code>arr1</code> contains <code>uniqueCnt1</code> <strong>distinct</strong> positive integers, each of which is <strong>not divisible</strong> by <code>divisor1</code>.</li>
	<li><code>arr2</code> contains <code>uniqueCnt2</code> <strong>distinct</strong> positive integers, each of which is <strong>not divisible</strong> by <code>divisor2</code>.</li>
	<li><strong>No</strong> integer is present in both <code>arr1</code> and <code>arr2</code>.</li>
</ul>

<p>Given <code>divisor1</code>, <code>divisor2</code>, <code>uniqueCnt1</code>, and <code>uniqueCnt2</code>, return <em>the <strong>minimum possible maximum</strong> integer that can be present in either array</em>.</p>

<p>&nbsp;</p>
<p><strong class="example">Example 1:</strong></p>

<pre>
<strong>Input:</strong> divisor1 = 2, divisor2 = 7, uniqueCnt1 = 1, uniqueCnt2 = 3
<strong>Output:</strong> 4
<strong>Explanation:</strong> 
We can distribute the first 4 natural numbers into arr1 and arr2.
arr1 = [1] and arr2 = [2,3,4].
We can see that both arrays satisfy all the conditions.
Since the maximum value is 4, we return it.
</pre>

<p><strong class="example">Example 2:</strong></p>

<pre>
<strong>Input:</strong> divisor1 = 3, divisor2 = 5, uniqueCnt1 = 2, uniqueCnt2 = 1
<strong>Output:</strong> 3
<strong>Explanation:</strong> 
Here arr1 = [1,2], and arr2 = [3] satisfy all conditions.
Since the maximum value is 3, we return it.</pre>

<p><strong class="example">Example 3:</strong></p>

<pre>
<strong>Input:</strong> divisor1 = 2, divisor2 = 4, uniqueCnt1 = 8, uniqueCnt2 = 2
<strong>Output:</strong> 15
<strong>Explanation:</strong> 
Here, the final possible arrays can be arr1 = [1,3,5,7,9,11,13,15], and arr2 = [2,6].
It can be shown that it is not possible to obtain a lower maximum satisfying all conditions. 
</pre>

<p>&nbsp;</p>
<p><strong>Constraints:</strong></p>

<ul>
	<li><code>2 &lt;= divisor1, divisor2 &lt;= 10<sup>5</sup></code></li>
	<li><code>1 &lt;= uniqueCnt1, uniqueCnt2 &lt; 10<sup>9</sup></code></li>
	<li><code>2 &lt;= uniqueCnt1 + uniqueCnt2 &lt;= 10<sup>9</sup></code></li>
</ul>

<!-- description:end -->

## Solutions

<!-- solution:start -->

### Solution 1

<!-- tabs:start -->

#### Java

```java
class Solution {
    public int minimizeSet(int divisor1, int divisor2, int uniqueCnt1, int uniqueCnt2) {
        long divisor = lcm(divisor1, divisor2);
        long left = 1, right = 10000000000L;
        while (left < right) {
            long mid = (left + right) >> 1;
            long cnt1 = mid / divisor1 * (divisor1 - 1) + mid % divisor1;
            long cnt2 = mid / divisor2 * (divisor2 - 1) + mid % divisor2;
            long cnt = mid / divisor * (divisor - 1) + mid % divisor;
            if (cnt1 >= uniqueCnt1 && cnt2 >= uniqueCnt2 && cnt >= uniqueCnt1 + uniqueCnt2) {
                right = mid;
            } else {
                left = mid + 1;
            }
        }
        return (int) left;
    }

    private long lcm(int a, int b) {
        return (long) a * b / gcd(a, b);
    }

    private int gcd(int a, int b) {
        return b == 0 ? a : gcd(b, a % b);
    }
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
