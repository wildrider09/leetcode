---
comments: true
difficulty: Medium
edit_url: https://github.com/doocs/leetcode/edit/main/lcci/17.11.Find%20Closest/README_EN.md
---

<!-- problem:start -->

# [17.11. Find Closest](https://leetcode.cn/problems/find-closest-lcci)

[Chinese Version](/lcci/17.11.Find%20Closest/README.md)

## Description

<!-- description:start -->

<p>You have a large text file containing words. Given any two words, find the shortest distance (in terms of number of words) between them in the file. If the operation will be repeated many times for the same file (but different pairs of words), can you optimize your solution?</p>

<p><strong>Example: </strong></p>

<pre>

<strong>Input: </strong>words = [&quot;I&quot;,&quot;am&quot;,&quot;a&quot;,&quot;student&quot;,&quot;from&quot;,&quot;a&quot;,&quot;university&quot;,&quot;in&quot;,&quot;a&quot;,&quot;city&quot;], word1 = &quot;a&quot;, word2 = &quot;student&quot;

<strong>Output: </strong>1</pre>

<p>Note:</p>

<ul>
	<li><code>words.length &lt;= 100000</code></li>
</ul>

<!-- description:end -->

## Solutions

<!-- solution:start -->

### Solution 1: Single Pass

We use two pointers $i$ and $j$ to record the most recent occurrences of the two words $\textit{word1}$ and $\textit{word2}$, respectively. Initially, $i = \infty$ and $j = -\infty$.

Next, we traverse the entire text file. For each word $w$, if $w$ equals $\textit{word1}$, we update $i = k$, where $k$ is the index of the current word; if $w$ equals $\textit{word2}$, we update $j = k$. Then we update the answer $ans = \min(ans, |i - j|)$.

After the traversal, we return the answer $ans$.

The time complexity is $O(n)$, where $n$ is the number of words in the text file. The space complexity is $O(1)$.

<!-- tabs:start -->

#### Java

```java
class Solution {
    public int findClosest(String[] words, String word1, String word2) {
        final int inf = 1 << 29;
        int i = inf, j = -inf, ans = inf;
        for (int k = 0; k < words.length; ++k) {
            if (words[k].equals(word1)) {
                i = k;
            } else if (words[k].equals(word2)) {
                j = k;
            }
            ans = Math.min(ans, Math.abs(i - j));
        }
        return ans;
    }
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- solution:start -->

### Solution 2: Hash Table + Two Pointers

We can use a hash table $d$ to record the positions of each word. Then, for each pair of $\textit{word1}$ and $\textit{word2}$, we can find their shortest distance using the two-pointer method.

We traverse the entire text file. For each word $w$, we add the index of $w$ to $d[w]$.

Next, we find the positions where $\textit{word1}$ and $\textit{word2}$ appear in the text file, represented by $idx1$ and $idx2$ respectively. Then we use two pointers $i$ and $j$ to point to $idx1$ and $idx2$ respectively, with initial values $i = 0$, $j = 0$.

Next, we traverse $idx1$ and $idx2$. Each time we update the answer $ans = \min(ans, |idx1[i] - idx2[j]|)$, then we move the smaller pointer of $i$ and $j$ one step backward.

The time complexity is $O(n)$, and the space complexity is $O(n)$. Where $n$ is the number of words in the text file.

<!-- tabs:start -->

#### Java

```java
class Solution {
    public int findClosest(String[] words, String word1, String word2) {
        Map<String, List<Integer>> d = new HashMap<>();
        for (int i = 0; i < words.length; ++i) {
            d.computeIfAbsent(words[i], k -> new ArrayList<>()).add(i);
        }
        List<Integer> idx1 = d.get(word1), idx2 = d.get(word2);
        int i = 0, j = 0, m = idx1.size(), n = idx2.size();
        int ans = 1 << 29;
        while (i < m && j < n) {
            int t = Math.abs(idx1.get(i) - idx2.get(j));
            ans = Math.min(ans, t);
            if (idx1.get(i) < idx2.get(j)) {
                ++i;
            } else {
                ++j;
            }
        }
        return ans;
    }
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
