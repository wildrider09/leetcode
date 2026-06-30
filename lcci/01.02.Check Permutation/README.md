---
comments: true
difficulty: Easy
edit_url: https://github.com/doocs/leetcode/edit/main/lcci/01.02.Check%20Permutation/README_EN.md
---

<!-- problem:start -->

# [01.02. Check Permutation](https://leetcode.cn/problems/check-permutation-lcci)

[Chinese Version](/lcci/01.02.Check%20Permutation/README.md)

## Description

<!-- description:start -->

<p>Given two strings,write a method to decide if one is a permutation of the other.</p>

<p><strong>Example 1:</strong></p>

<pre>

<strong>Input: </strong>s1 = &quot;abc&quot;, s2 = &quot;bca&quot;

<strong>Output: </strong>true

</pre>

<p><strong>Example 2:</strong></p>

<pre>

<strong>Input: </strong>s1 = &quot;abc&quot;, s2 = &quot;bad&quot;

<strong>Output: </strong>false

</pre>

<p><strong>Note:</strong></p>
<ol>
	<li><code>0 &lt;= len(s1) &lt;= 100 </code></li>
	<li><code>0 &lt;= len(s2) &lt;= 100</code></li>
</ol>

<!-- description:end -->

## Solutions

<!-- solution:start -->

### Solution 1: Array or Hash Table

First, we check whether the lengths of the two strings are equal. If they are not equal, we directly return `false`.

Then, we use an array or hash table to count the occurrence of each character in string $s1$.

Next, we traverse the other string $s2$. For each character we encounter, we decrement its corresponding count. If the count after decrementing is less than $0$, it means that the occurrence of characters in the two strings is different, so we directly return `false`.

Finally, after traversing string $s2$, we return `true`.

Note: In this problem, all test case strings only contain lowercase letters, so we can directly create an array of length $26$ for counting.

The time complexity is $O(n)$, and the space complexity is $O(C)$. Here, $n$ is the length of the string, and $C$ is the size of the character set. In this problem, $C=26$.

<!-- tabs:start -->

#### Java

```java
class Solution {
    public boolean CheckPermutation(String s1, String s2) {
        if (s1.length() != s2.length()) {
            return false;
        }
        int[] cnt = new int[26];
        for (char c : s1.toCharArray()) {
            ++cnt[c - 'a'];
        }
        for (char c : s2.toCharArray()) {
            if (--cnt[c - 'a'] < 0) {
                return false;
            }
        }
        return true;
    }
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- solution:start -->

### Solution 2: Sorting

We can also sort the two strings in lexicographical order, and then compare whether the two strings are equal.

The time complexity is $O(n \times \log n)$, and the space complexity is $O(n)$. Here, $n$ is the length of the string.

<!-- tabs:start -->

#### Java

```java
class Solution {
    public boolean CheckPermutation(String s1, String s2) {
        char[] cs1 = s1.toCharArray();
        char[] cs2 = s2.toCharArray();
        Arrays.sort(cs1);
        Arrays.sort(cs2);
        return Arrays.equals(cs1, cs2);
    }
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
