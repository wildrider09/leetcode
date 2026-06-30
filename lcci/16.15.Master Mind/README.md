---
comments: true
difficulty: Easy
edit_url: https://github.com/doocs/leetcode/edit/main/lcci/16.15.Master%20Mind/README_EN.md
---

<!-- problem:start -->

# [16.15. Master Mind](https://leetcode.cn/problems/master-mind-lcci)

[Chinese Version](/lcci/16.15.Master%20Mind/README.md)

## Description

<!-- description:start -->

<p>The Game of Master Mind is played as follows:</p>
<p>The computer has four slots, and each slot will contain a ball that is red (R). yellow (Y). green (G) or blue (B). For example, the computer might have RGGB (Slot #1 is red, Slots #2 and #3 are green, Slot #4 is blue).</p>
<p>You, the user, are trying to guess the solution. You might, for example, guess YRGB.</p>
<p>When you guess the correct color for the correct slot, you get a &quot;hit:&#39; If you guess a color that exists but is in the wrong slot, you get a &quot;pseudo-hit:&#39; Note that a slot that is a hit can never count as a pseudo-hit.</p>
<p>For example, if the actual solution is RGBY and you guess GGRR, you have one hit and one pseudo-hit. Write a method that, given a guess and a solution, returns the number of hits and pseudo-hits.</p>
<p>Given a sequence of colors <code>solution</code>, and a <code>guess</code>, write a method that return the number of hits and pseudo-hit <code>answer</code>, where <code>answer[0]</code> is the number of hits and <code>answer[1]</code> is the number of pseudo-hit.</p>
<p><strong>Example: </strong></p>
<pre>

<strong>Input: </strong> solution=&quot;RGBY&quot;,guess=&quot;GGRR&quot;

<strong>Output: </strong> [1,1]

<strong>Explanation: </strong> hit once, pseudo-hit once.

</pre>
<p><strong>Note: </strong></p>
<ul>
	<li><code>len(solution) = len(guess) = 4</code></li>
	<li>There are only <code>&quot;R&quot;</code>,<code>&quot;G&quot;</code>,<code>&quot;B&quot;</code>,<code>&quot;Y&quot;</code> in <code>solution</code>&nbsp;and&nbsp;<code>guess</code>.</li>
</ul>

<!-- description:end -->

## Solutions

<!-- solution:start -->

### Solution 1: Hash Table

We simultaneously traverse both strings, count the number of corresponding characters that are the same, and accumulate them in $x$. Then we record the characters and their frequencies in both strings in hash tables $cnt1$ and $cnt2$, respectively.

Next, we traverse both hash tables, count the number of common characters, and accumulate them in $y$. The answer is then $[x, y - x]$.

The time complexity is $O(C)$, and the space complexity is $O(C)$. Here, $C=4$ for this problem.

<!-- tabs:start -->

#### Java

```java
class Solution {
    public int[] masterMind(String solution, String guess) {
        int x = 0, y = 0;
        Map<Character, Integer> cnt1 = new HashMap<>();
        Map<Character, Integer> cnt2 = new HashMap<>();
        for (int i = 0; i < 4; ++i) {
            char a = solution.charAt(i), b = guess.charAt(i);
            x += a == b ? 1 : 0;
            cnt1.merge(a, 1, Integer::sum);
            cnt2.merge(b, 1, Integer::sum);
        }
        for (char c : "RYGB".toCharArray()) {
            y += Math.min(cnt1.getOrDefault(c, 0), cnt2.getOrDefault(c, 0));
        }
        return new int[] {x, y - x};
    }
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
