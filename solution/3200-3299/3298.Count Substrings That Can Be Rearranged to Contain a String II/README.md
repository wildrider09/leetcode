---
comments: true
difficulty: Hard
edit_url: https://github.com/doocs/leetcode/edit/main/solution/3200-3299/3298.Count%20Substrings%20That%20Can%20Be%20Rearranged%20to%20Contain%20a%20String%20II/README_EN.md
rating: 1909
source: Weekly Contest 416 Q4
tags:
    - Hash Table
    - String
    - Sliding Window
---

<!-- problem:start -->

# [3298. Count Substrings That Can Be Rearranged to Contain a String II](https://leetcode.com/problems/count-substrings-that-can-be-rearranged-to-contain-a-string-ii)

[Chinese Version](/solution/3200-3299/3298.Count%20Substrings%20That%20Can%20Be%20Rearranged%20to%20Contain%20a%20String%20II/README.md)

## Description

<!-- description:start -->

<p>You are given two strings <code>word1</code> and <code>word2</code>.</p>

<p>A string <code>x</code> is called <strong>valid</strong> if <code>x</code> can be rearranged to have <code>word2</code> as a <span data-keyword="string-prefix">prefix</span>.</p>

<p>Return the total number of <strong>valid</strong> <span data-keyword="substring-nonempty">substrings</span> of <code>word1</code>.</p>

<p><strong>Note</strong> that the memory limits in this problem are <strong>smaller</strong> than usual, so you <strong>must</strong> implement a solution with a <em>linear</em> runtime complexity.</p>

<p>&nbsp;</p>
<p><strong class="example">Example 1:</strong></p>

<div class="example-block">
<p><strong>Input:</strong> <span class="example-io">word1 = &quot;bcca&quot;, word2 = &quot;abc&quot;</span></p>

<p><strong>Output:</strong> <span class="example-io">1</span></p>

<p><strong>Explanation:</strong></p>

<p>The only valid substring is <code>&quot;bcca&quot;</code> which can be rearranged to <code>&quot;abcc&quot;</code> having <code>&quot;abc&quot;</code> as a prefix.</p>
</div>

<p><strong class="example">Example 2:</strong></p>

<div class="example-block">
<p><strong>Input:</strong> <span class="example-io">word1 = &quot;abcabc&quot;, word2 = &quot;abc&quot;</span></p>

<p><strong>Output:</strong> <span class="example-io">10</span></p>

<p><strong>Explanation:</strong></p>

<p>All the substrings except substrings of size 1 and size 2 are valid.</p>
</div>

<p><strong class="example">Example 3:</strong></p>

<div class="example-block">
<p><strong>Input:</strong> <span class="example-io">word1 = &quot;abcabc&quot;, word2 = &quot;aaabc&quot;</span></p>

<p><strong>Output:</strong> <span class="example-io">0</span></p>
</div>

<p>&nbsp;</p>
<p><strong>Constraints:</strong></p>

<ul>
	<li><code>1 &lt;= word1.length &lt;= 10<sup>6</sup></code></li>
	<li><code>1 &lt;= word2.length &lt;= 10<sup>4</sup></code></li>
	<li><code>word1</code> and <code>word2</code> consist only of lowercase English letters.</li>
</ul>

<!-- description:end -->

## Solutions

<!-- solution:start -->

### Solution 1: Sliding Window

The problem is essentially to find how many substrings in $\textit{word1}$ contain all the characters in $\textit{word2}$. We can use a sliding window to handle this.

First, if the length of $\textit{word1}$ is less than the length of $\textit{word2}$, then it is impossible for $\textit{word1}$ to contain all the characters of $\textit{word2}$, so we directly return $0$.

Next, we use a hash table or an array of length $26$ called $\textit{cnt}$ to count the occurrences of characters in $\textit{word2}$. Then, we use $\textit{need}$ to record how many more characters are needed to meet the condition, initialized to the length of $\textit{cnt}$.

We then use a sliding window $\textit{win}$ to record the occurrences of characters in the current window. We use $\textit{ans}$ to record the number of substrings that meet the condition, and $\textit{l}$ to record the left boundary of the window.

We traverse each character in $\textit{word1}$. For the current character $c$, we add it to $\textit{win}$. If the value of $\textit{win}[c]$ equals $\textit{cnt}[c]$, it means the current window already contains one of the characters in $\textit{word2}$, so we decrement $\textit{need}$ by one. If $\textit{need}$ equals $0$, it means the current window contains all the characters in $\textit{word2}$. We need to shrink the left boundary of the window until $\textit{need}$ is greater than $0$. Specifically, if $\textit{win}[\textit{word1}[l]]$ equals $\textit{cnt}[\textit{word1}[l]]$, it means the current window contains one of the characters in $\textit{word2}$. After shrinking the left boundary, it no longer meets the condition, so we increment $\textit{need}$ by one and decrement $\textit{win}[\textit{word1}[l]]$ by one. Then, we increment $\textit{l}$ by one. At this point, the window is $[l, r]$. For any $0 \leq l' < l$, $[l', r]$ are substrings that meet the condition, and there are $l$ such substrings, which we add to the answer.

After traversing all characters in $\textit{word1}$, we get the answer.

The time complexity is $O(n + m)$, where $n$ and $m$ are the lengths of $\textit{word1}$ and $\textit{word2}$, respectively. The space complexity is $O(|\Sigma|)$, where $\Sigma$ is the character set. Here, it is the set of lowercase letters, so the space complexity is constant.

<!-- tabs:start -->

#### Java

```java
class Solution {
    public long validSubstringCount(String word1, String word2) {
        if (word1.length() < word2.length()) {
            return 0;
        }
        int[] cnt = new int[26];
        int need = 0;
        for (int i = 0; i < word2.length(); ++i) {
            if (++cnt[word2.charAt(i) - 'a'] == 1) {
                ++need;
            }
        }
        long ans = 0;
        int[] win = new int[26];
        for (int l = 0, r = 0; r < word1.length(); ++r) {
            int c = word1.charAt(r) - 'a';
            if (++win[c] == cnt[c]) {
                --need;
            }
            while (need == 0) {
                c = word1.charAt(l) - 'a';
                if (win[c] == cnt[c]) {
                    ++need;
                }
                --win[c];
                ++l;
            }
            ans += l;
        }
        return ans;
    }
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
