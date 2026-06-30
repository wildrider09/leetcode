---
comments: true
difficulty: Medium
edit_url: https://github.com/doocs/leetcode/edit/main/solution/1200-1299/1297.Maximum%20Number%20of%20Occurrences%20of%20a%20Substring/README_EN.md
rating: 1748
source: Weekly Contest 168 Q3
tags:
    - Hash Table
    - String
    - Sliding Window
---

<!-- problem:start -->

# [1297. Maximum Number of Occurrences of a Substring](https://leetcode.com/problems/maximum-number-of-occurrences-of-a-substring)

[Chinese Version](/solution/1200-1299/1297.Maximum%20Number%20of%20Occurrences%20of%20a%20Substring/README.md)

## Description

<!-- description:start -->

<p>Given a string <code>s</code>, return the maximum number of occurrences of <strong>any</strong> substring under the following rules:</p>

<ul>
	<li>The number of unique characters in the substring must be less than or equal to <code>maxLetters</code>.</li>
	<li>The substring size must be between <code>minSize</code> and <code>maxSize</code> inclusive.</li>
</ul>

<p>&nbsp;</p>
<p><strong class="example">Example 1:</strong></p>

<pre>
<strong>Input:</strong> s = &quot;aababcaab&quot;, maxLetters = 2, minSize = 3, maxSize = 4
<strong>Output:</strong> 2
<strong>Explanation:</strong> Substring &quot;aab&quot; has 2 occurrences in the original string.
It satisfies the conditions, 2 unique letters and size 3 (between minSize and maxSize).
</pre>

<p><strong class="example">Example 2:</strong></p>

<pre>
<strong>Input:</strong> s = &quot;aaaa&quot;, maxLetters = 1, minSize = 3, maxSize = 3
<strong>Output:</strong> 2
<strong>Explanation:</strong> Substring &quot;aaa&quot; occur 2 times in the string. It can overlap.
</pre>

<p>&nbsp;</p>
<p><strong>Constraints:</strong></p>

<ul>
	<li><code>1 &lt;= s.length &lt;= 10<sup>5</sup></code></li>
	<li><code>1 &lt;= maxLetters &lt;= 26</code></li>
	<li><code>1 &lt;= minSize &lt;= maxSize &lt;= min(26, s.length)</code></li>
	<li><code>s</code> consists of only lowercase English letters.</li>
</ul>

<!-- description:end -->

## Solutions

<!-- solution:start -->

### Solution 1: Hash Table + Enumeration

According to the problem description, if a long string meets the condition, then its substring of length $\textit{minSize}$ must also meet the condition. Therefore, we only need to enumerate all substrings of length $\textit{minSize}$ in $s$, then use a hash table to record the occurrence frequency of all substrings, and find the maximum frequency as the answer.

The time complexity is $O(n \times m)$, and the space complexity is $O(n \times m)$. Here, $n$ and $m$ are the lengths of the string $s$ and $\textit{minSize}$, respectively. In this problem, $m$ does not exceed $26$.

<!-- tabs:start -->

#### Java

```java
class Solution {
    public int maxFreq(String s, int maxLetters, int minSize, int maxSize) {
        int ans = 0;
        Map<String, Integer> cnt = new HashMap<>();
        for (int i = 0; i < s.length() - minSize + 1; ++i) {
            String t = s.substring(i, i + minSize);
            Set<Character> ss = new HashSet<>();
            for (int j = 0; j < minSize; ++j) {
                ss.add(t.charAt(j));
            }
            if (ss.size() <= maxLetters) {
                cnt.merge(t, 1, Integer::sum);
                ans = Math.max(ans, cnt.get(t));
            }
        }
        return ans;
    }
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- solution:start -->

### Solution 2: Sliding Window + String Hashing

We can use a sliding window to maintain the number of distinct letters in the current substring, while using string hashing to efficiently calculate the hash value of substrings, thereby avoiding using strings as hash table keys and improving performance.

Specifically, we define a $\textit{Hashing}$ class to preprocess the prefix hash values and power values of the string $s$, so that we can calculate the hash value of any substring in $O(1)$ time.

Then, we use a sliding window to traverse the string $s$, maintaining the number of distinct letters in the current window. When the window size reaches $\textit{minSize}$, we calculate the hash value of the current substring and update its occurrence count. Next, we slide the window one position to the right and update the letter frequencies and the count of distinct letters.

The time complexity is $O(n)$ and the space complexity is $O(n)$, where $n$ is the length of the string $s$.

<!-- tabs:start -->

#### Java

```java
class Hashing {
    private final long[] p;
    private final long[] h;
    private final long mod;

    public Hashing(String word) {
        this(word, 13331, 998244353);
    }

    public Hashing(String word, long base, int mod) {
        int n = word.length();
        p = new long[n + 1];
        h = new long[n + 1];
        p[0] = 1;
        this.mod = mod;
        for (int i = 1; i <= n; i++) {
            p[i] = p[i - 1] * base % mod;
            h[i] = (h[i - 1] * base + word.charAt(i - 1)) % mod;
        }
    }

    public long query(int l, int r) {
        return (h[r] - h[l - 1] * p[r - l + 1] % mod + mod) % mod;
    }
}

class Solution {
    public int maxFreq(String s, int maxLetters, int minSize, int maxSize) {
        int n = s.length();
        Hashing hashing = new Hashing(s);
        int[] freq = new int[256];
        int k = 0;
        int ans = 0;
        Map<Long, Integer> cnt = new HashMap<>();

        for (int i = 1; i <= n; i++) {
            if (++freq[s.charAt(i - 1)] == 1) {
                k++;
            }

            if (i >= minSize) {
                if (k <= maxLetters) {
                    long x = hashing.query(i - minSize + 1, i);
                    ans = Math.max(ans, cnt.merge(x, 1, Integer::sum));
                }
                int j = i - minSize;
                if (--freq[s.charAt(j)] == 0) {
                    k--;
                }
            }
        }

        return ans;
    }
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
