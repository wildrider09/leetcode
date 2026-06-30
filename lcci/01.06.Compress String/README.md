---
comments: true
difficulty: Easy
edit_url: https://github.com/doocs/leetcode/edit/main/lcci/01.06.Compress%20String/README_EN.md
---

<!-- problem:start -->

# [01.06. Compress String](https://leetcode.cn/problems/compress-string-lcci)

[Chinese Version](/lcci/01.06.Compress%20String/README.md)

## Description

<!-- description:start -->

<p>Implement a method to perform basic string compression using the counts of repeated characters. For example, the string aabcccccaaa would become a2blc5a3. If the &quot;compressed&quot; string would not become smaller than the original string, your method should return the original string. You can assume the string has only uppercase and lowercase letters (a - z).</p>

<p><strong>Example 1:</strong></p>

<pre>

<strong>Input: </strong>&quot;aabcccccaaa&quot;

<strong>Output: </strong>&quot;a2b1c5a3&quot;

</pre>

<p><strong>Example 2:</strong></p>

<pre>

<strong>Input: </strong>&quot;abbccd&quot;

<strong>Output: </strong>&quot;abbccd&quot;

<strong>Explanation: </strong>

The compressed string is &quot;a1b2c2d1&quot;, which is longer than the original string.

</pre>

<p><strong>Note:</strong></p>

- `0 <= S.length <= 50000`

<!-- description:end -->

## Solutions

<!-- solution:start -->

### Solution 1: Two Pointers

We can use two pointers to find the start and end positions of each consecutive character, calculate the length of the consecutive characters, and then append the character and length to the string $t$.

Finally, we compare the lengths of $t$ and $S$. If the length of $t$ is less than $S$, we return $t$, otherwise we return $S$.

The time complexity is $O(n)$, and the space complexity is $O(n)$. Here, $n$ is the length of the string.

<!-- tabs:start -->

#### Java

```java
class Solution {
    public String compressString(String S) {
        int n = S.length();
        StringBuilder sb = new StringBuilder();
        for (int i = 0; i < n;) {
            int j = i + 1;
            while (j < n && S.charAt(j) == S.charAt(i)) {
                ++j;
            }
            sb.append(S.charAt(i)).append(j - i);
            i = j;
        }
        String t = sb.toString();
        return t.length() < n ? t : S;
    }
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
