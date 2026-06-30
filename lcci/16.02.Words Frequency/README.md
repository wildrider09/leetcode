---
comments: true
difficulty: Medium
edit_url: https://github.com/doocs/leetcode/edit/main/lcci/16.02.Words%20Frequency/README_EN.md
---

<!-- problem:start -->

# [16.02. Words Frequency](https://leetcode.cn/problems/words-frequency-lcci)

[Chinese Version](/lcci/16.02.Words%20Frequency/README.md)

## Description

<!-- description:start -->

<p>Design a method to find the frequency of occurrences of any given word in a book. What if we were running this algorithm multiple times?</p>

<p>You should implement following methods:</p>

<ul>
	<li><code>WordsFrequency(book)</code> constructor, parameter is a array of strings, representing the book.</li>
	<li><code>get(word)</code>&nbsp;get the frequency of <code>word</code> in the book.&nbsp;</li>
</ul>

<p><strong>Example: </strong></p>

<pre>

WordsFrequency wordsFrequency = new WordsFrequency({&quot;i&quot;, &quot;have&quot;, &quot;an&quot;, &quot;apple&quot;, &quot;he&quot;, &quot;have&quot;, &quot;a&quot;, &quot;pen&quot;});

wordsFrequency.get(&quot;you&quot;); //returns 0，&quot;you&quot; is not in the book

wordsFrequency.get(&quot;have&quot;); //returns 2，&quot;have&quot; occurs twice in the book

wordsFrequency.get(&quot;an&quot;); //returns 1

wordsFrequency.get(&quot;apple&quot;); //returns 1

wordsFrequency.get(&quot;pen&quot;); //returns 1

</pre>

<p><strong>Note: </strong></p>

<ul>
    <li><code>There are only lowercase letters in book[i].</code></li>
    <li><code>1 &lt;= book.length &lt;= 100000</code></li>
    <li><code>1 &lt;= book[i].length &lt;= 10</code></li>
    <li><code>get</code>&nbsp;function will not be called more than&nbsp;100000 times.</li>
</ul>

<!-- description:end -->

## Solutions

<!-- solution:start -->

### Solution 1: Hash Table

We use a hash table $cnt$ to count the number of occurrences of each word in $book$.

When calling the `get` function, we only need to return the number of occurrences of the corresponding word in $cnt$.

In terms of time complexity, the time complexity of initializing the hash table $cnt$ is $O(n)$, where $n$ is the length of $book$. The time complexity of the `get` function is $O(1)$. The space complexity is $O(n)$.

<!-- tabs:start -->

#### Java

```java
class WordsFrequency {
    private Map<String, Integer> cnt = new HashMap<>();

    public WordsFrequency(String[] book) {
        for (String x : book) {
            cnt.merge(x, 1, Integer::sum);
        }
    }

    public int get(String word) {
        return cnt.getOrDefault(word, 0);
    }
}

/**
 * Your WordsFrequency object will be instantiated and called as such:
 * WordsFrequency obj = new WordsFrequency(book);
 * int param_1 = obj.get(word);
 */
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
