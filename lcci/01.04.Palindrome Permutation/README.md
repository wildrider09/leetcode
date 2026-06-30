---
comments: true
difficulty: Easy
edit_url: https://github.com/doocs/leetcode/edit/main/lcci/01.04.Palindrome%20Permutation/README_EN.md
---

<!-- problem:start -->

# [01.04. Palindrome Permutation](https://leetcode.cn/problems/palindrome-permutation-lcci)

[Chinese Version](/lcci/01.04.Palindrome%20Permutation/README.md)

## Description

<!-- description:start -->

<p>Given a string, write a function to check if it is a permutation of a palin&shy; drome. A palindrome is a word or phrase that is the same forwards and backwards. A permutation is a rearrangement of letters. The palindrome does not need to be limited to just dictionary words.</p>

<p>&nbsp;</p>

<p><strong>Example1: </strong></p>

<pre>

<strong>Input: &quot;</strong>tactcoa&quot;

<strong>Output: </strong>true（permutations: &quot;tacocat&quot;、&quot;atcocta&quot;, etc.）

</pre>

<!-- description:end -->

## Solutions

<!-- solution:start -->

### Solution 1: Hash Table

We use a hash table $cnt$ to store the occurrence count of each character. If more than $1$ character has an odd count, then it is not a palindrome permutation.

The time complexity is $O(n)$, and the space complexity is $O(n)$. Here, $n$ is the length of the string.

<!-- tabs:start -->

#### Java

```java
class Solution {
    public boolean canPermutePalindrome(String s) {
        Map<Character, Integer> cnt = new HashMap<>();
        for (int i = 0; i < s.length(); ++i) {
            cnt.merge(s.charAt(i), 1, Integer::sum);
        }
        int sum = 0;
        for (int v : cnt.values()) {
            sum += v & 1;
        }
        return sum < 2;
    }
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- solution:start -->

### Solution 2: Another Implementation of Hash Table

We use a hash table $vis$ to store whether each character has appeared. If it has appeared, we remove the character from the hash table; otherwise, we add the character to the hash table.

Finally, we check whether the number of characters in the hash table is less than $2$. If it is, then it is a palindrome permutation.

The time complexity is $O(n)$, and the space complexity is $O(n)$. Here, $n$ is the length of the string.

<!-- tabs:start -->

#### Java

```java
class Solution {
    public boolean canPermutePalindrome(String s) {
        Set<Character> vis = new HashSet<>();
        for (int i = 0; i < s.length(); ++i) {
            char c = s.charAt(i);
            if (!vis.add(c)) {
                vis.remove(c);
            }
        }
        return vis.size() < 2;
    }
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
