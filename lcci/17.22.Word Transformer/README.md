---
comments: true
difficulty: Medium
edit_url: https://github.com/doocs/leetcode/edit/main/lcci/17.22.Word%20Transformer/README_EN.md
---

<!-- problem:start -->

# [17.22. Word Transformer](https://leetcode.cn/problems/word-transformer-lcci)

[Chinese Version](/lcci/17.22.Word%20Transformer/README.md)

## Description

<!-- description:start -->

<p>Given two words of equal length that are in a dictionary, write a method to transform one word into another word by changing only one letter at a time. The new word you get in each step must be in the dictionary.</p>

<p>Write code to return a possible transforming sequence. If there are more that one sequence, any one is ok.</p>

<p><strong>Example 1:</strong></p>

<pre>

<strong>Input:</strong>

beginWord = &quot;hit&quot;,

endWord = &quot;cog&quot;,

wordList = [&quot;hot&quot;,&quot;dot&quot;,&quot;dog&quot;,&quot;lot&quot;,&quot;log&quot;,&quot;cog&quot;]

<strong>Output:</strong>

[&quot;hit&quot;,&quot;hot&quot;,&quot;dot&quot;,&quot;lot&quot;,&quot;log&quot;,&quot;cog&quot;]

</pre>

<p><strong>Example 2:</strong></p>

<pre>

<strong>Input:</strong>

beginWord = &quot;hit&quot;

endWord = &quot;cog&quot;

wordList = [&quot;hot&quot;,&quot;dot&quot;,&quot;dog&quot;,&quot;lot&quot;,&quot;log&quot;]

<strong>Output: </strong>[]

<strong>Explanation:</strong>&nbsp;<em>endWord</em> &quot;cog&quot; is not in the dictionary, so there&#39;s no possible transforming sequence.</pre>

<!-- description:end -->

## Solutions

<!-- solution:start -->

### Solution 1: DFS

We define an answer array `ans`, initially containing only `beginWord`. Then we define an array `vis` to mark whether the words in `wordList` have been visited.

Next, we design a function `dfs(s)`, which represents whether we can successfully convert `s` to `endWord` starting from `s`. If successful, return `True`, otherwise return `False`.

The specific implementation of the function `dfs(s)` is as follows:

1. If `s` equals `endWord`, the conversion is successful, return `True`;
2. Otherwise, we traverse each word `t` in `wordList`. If `t` has not been visited and there is only one different character between `s` and `t`, then we mark `t` as visited, add `t` to `ans`, and then recursively call `dfs(t)`. If `True` is returned, the conversion is successful, we return `True`, otherwise we remove `t` from `ans` and continue to traverse the next word;
3. If all the words in `wordList` have been traversed and no convertible word is found, the conversion fails, we return `False`.

Finally, we call `dfs(beginWord)`. If `True` is returned, the conversion is successful, we return `ans`, otherwise return an empty array.

<!-- tabs:start -->

#### Java

```java
class Solution {
    private List<String> ans = new ArrayList<>();
    private List<String> wordList;
    private String endWord;
    private boolean[] vis;

    public List<String> findLadders(String beginWord, String endWord, List<String> wordList) {
        this.wordList = wordList;
        this.endWord = endWord;
        ans.add(beginWord);
        vis = new boolean[wordList.size()];
        return dfs(beginWord) ? ans : List.of();
    }

    private boolean dfs(String s) {
        if (s.equals(endWord)) {
            return true;
        }
        for (int i = 0; i < wordList.size(); ++i) {
            String t = wordList.get(i);
            if (vis[i] || !check(s, t)) {
                continue;
            }
            vis[i] = true;
            ans.add(t);
            if (dfs(t)) {
                return true;
            }
            ans.remove(ans.size() - 1);
        }
        return false;
    }

    private boolean check(String s, String t) {
        if (s.length() != t.length()) {
            return false;
        }
        int cnt = 0;
        for (int i = 0; i < s.length(); ++i) {
            if (s.charAt(i) != t.charAt(i)) {
                ++cnt;
            }
        }
        return cnt == 1;
    }
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
