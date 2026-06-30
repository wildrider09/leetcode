---
comments: true
difficulty: Medium
edit_url: https://github.com/doocs/leetcode/edit/main/lcci/16.21.Sum%20Swap/README_EN.md
---

<!-- problem:start -->

# [16.21. Sum Swap](https://leetcode.cn/problems/sum-swap-lcci)

[Chinese Version](/lcci/16.21.Sum%20Swap/README.md)

## Description

<!-- description:start -->

<p>Given two arrays of integers, find a pair of values (one value from each array) that you can swap to give the two arrays the same sum.</p>

<p>Return an array, where the first element is the element in the first array that will be swapped, and the second element is another one in the second array. If there are more than one answers, return any one of them. If there is no answer, return an empty array.</p>

<p><strong>Example:</strong></p>

<pre>

<strong>Input:</strong> array1 = [4, 1, 2, 1, 1, 2], array2 = [3, 6, 3, 3]

<strong>Output:</strong> [1, 3]

</pre>

<p><strong>Example:</strong></p>

<pre>

<strong>Input:</strong> array1 = <code>[1, 2, 3], array2 = [4, 5, 6]</code>

<strong>Output: </strong>[]</pre>

<p><strong>Note: </strong></p>

<ul>
	<li><code>1 &lt;= array1.length, array2.length &lt;= 100000</code></li>
</ul>

<!-- description:end -->

## Solutions

<!-- solution:start -->

### Solution 1: Hash Table

We first calculate the sum of the two arrays, and then calculate the difference $diff$ between the sums. If $diff$ is odd, it means that the sums of the two arrays cannot be equal, so we directly return an empty array.

If $diff$ is even, we can traverse one of the arrays. Suppose the current element being traversed is $a$, then we need to find an element $b$ in the other array such that $a - b = diff / 2$, i.e., $b = a - diff / 2$. We can use a hash table to quickly check whether $b$ exists. If it exists, it means that we have found a pair of elements that meet the conditions, and we can return them directly.

The time complexity is $O(m + n)$, and the space complexity is $O(n)$. Here, $m$ and $n$ are the lengths of the two arrays.

<!-- tabs:start -->

#### Java

```java
class Solution {
    public int[] findSwapValues(int[] array1, int[] array2) {
        long s1 = 0, s2 = 0;
        Set<Integer> s = new HashSet<>();
        for (int x : array1) {
            s1 += x;
        }
        for (int x : array2) {
            s2 += x;
            s.add(x);
        }
        long diff = s1 - s2;
        if (diff % 2 != 0) {
            return new int[0];
        }
        diff /= 2;
        for (int a : array1) {
            int b = (int) (a - diff);
            if (s.contains(b)) {
                return new int[] {a, b};
            }
        }
        return new int[0];
    }
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
