---
comments: true
difficulty: Medium
edit_url: https://github.com/doocs/leetcode/edit/main/lcci/10.02.Group%20Anagrams/README_EN.md
---

<!-- problem:start -->

# [10.02. Group Anagrams](https://leetcode.cn/problems/group-anagrams-lcci)

[Chinese Version](/lcci/10.02.Group%20Anagrams/README.md)

## Description

<!-- description:start -->

<p>Write a method to sort an array of strings so that all the anagrams are in the same group.</p>

<p><b>Note:&nbsp;</b>This problem is slightly different from the original one the book.</p>

<p><strong>Example:</strong></p>

<pre>

<strong>Input:</strong> <code>[&quot;eat&quot;, &quot;tea&quot;, &quot;tan&quot;, &quot;ate&quot;, &quot;nat&quot;, &quot;bat&quot;]</code>,

<strong>Output:</strong>

[

  [&quot;ate&quot;,&quot;eat&quot;,&quot;tea&quot;],

  [&quot;nat&quot;,&quot;tan&quot;],

  [&quot;bat&quot;]

]</pre>

<p><strong>Notes: </strong></p>

<ul>
	<li>All inputs will be in lowercase.</li>
	<li>The order of your output does not&nbsp;matter.</li>
</ul>

<!-- description:end -->

## Solutions

<!-- solution:start -->

### Solution 1: Hash Table

1. Traverse the string array, sort each string according to **character lexicographical order**, and get a new string.
2. Use the new string as `key` and `[str]` as `value`, and store them in the hash table (`HashMap<String, List<String>>`).
3. When the same `key` is encountered in subsequent traversals, add it to the corresponding `value`.

Take `strs = ["eat", "tea", "tan", "ate", "nat", "bat"]` as an example. At the end of the traversal, the state of the hash table is:

| key     | value                   |
| ------- | ----------------------- |
| `"aet"` | `["eat", "tea", "ate"]` |
| `"ant"` | `["tan", "nat"] `       |
| `"abt"` | `["bat"] `              |

Finally, return the `value` list of the hash table.

The time complexity is $O(n\times k\times \log k)$, where $n$ and $k$ are the length of the string array and the maximum length of the string, respectively.

<!-- tabs:start -->

#### Java

```java
class Solution {
    public List<List<String>> groupAnagrams(String[] strs) {
        Map<String, List<String>> d = new HashMap<>();
        for (String s : strs) {
            char[] t = s.toCharArray();
            Arrays.sort(t);
            String k = String.valueOf(t);
            d.computeIfAbsent(k, key -> new ArrayList<>()).add(s);
        }
        return new ArrayList<>(d.values());
    }
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- solution:start -->

### Solution 2: Counting

We can also change the sorting part in Solution 1 to counting, that is, use the characters in each string $s$ and their occurrence times as `key`, and the string $s$ as `value` to store in the hash table.

The time complexity is $O(n\times (k + C))$. Where $n$ and $k$ are the length of the string array and the maximum length of the string, respectively, and $C$ is the size of the character set. In this problem, $C = 26$.

<!-- tabs:start -->

#### Java

```java
class Solution {
    public List<List<String>> groupAnagrams(String[] strs) {
        Map<String, List<String>> d = new HashMap<>();
        for (String s : strs) {
            int[] cnt = new int[26];
            for (int i = 0; i < s.length(); ++i) {
                ++cnt[s.charAt(i) - 'a'];
            }
            StringBuilder sb = new StringBuilder();
            for (int i = 0; i < 26; ++i) {
                if (cnt[i] > 0) {
                    sb.append((char) ('a' + i)).append(cnt[i]);
                }
            }
            String k = sb.toString();
            d.computeIfAbsent(k, key -> new ArrayList<>()).add(s);
        }
        return new ArrayList<>(d.values());
    }
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
