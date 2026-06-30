---
comments: true
difficulty: Medium
edit_url: https://github.com/doocs/leetcode/edit/main/solution/1600-1699/1604.Alert%20Using%20Same%20Key-Card%20Three%20or%20More%20Times%20in%20a%20One%20Hour%20Period/README_EN.md
rating: 1606
source: Biweekly Contest 36 Q2
tags:
    - Array
    - Hash Table
    - String
    - Sorting
---

<!-- problem:start -->

# [1604. Alert Using Same Key-Card Three or More Times in a One Hour Period](https://leetcode.com/problems/alert-using-same-key-card-three-or-more-times-in-a-one-hour-period)

[Chinese Version](/solution/1600-1699/1604.Alert%20Using%20Same%20Key-Card%20Three%20or%20More%20Times%20in%20a%20One%20Hour%20Period/README.md)

## Description

<!-- description:start -->

<p>LeetCode company workers use key-cards to unlock office doors. Each time a worker uses their key-card, the security system saves the worker&#39;s name and the time when it was used. The system emits an <strong>alert</strong> if any worker uses the key-card <strong>three or more times</strong> in a one-hour period.</p>

<p>You are given a list of strings <code>keyName</code> and <code>keyTime</code> where <code>[keyName[i], keyTime[i]]</code> corresponds to a person&#39;s name and the time when their key-card was used <strong>in a</strong> <strong>single day</strong>.</p>

<p>Access times are given in the <strong>24-hour time format &quot;HH:MM&quot;</strong>, such as <code>&quot;23:51&quot;</code> and <code>&quot;09:49&quot;</code>.</p>

<p>Return a <em>list of unique worker names who received an alert for frequent keycard use</em>. Sort the names in <strong>ascending order alphabetically</strong>.</p>

<p>Notice that <code>&quot;10:00&quot;</code> - <code>&quot;11:00&quot;</code> is considered to be within a one-hour period, while <code>&quot;22:51&quot;</code> - <code>&quot;23:52&quot;</code> is not considered to be within a one-hour period.</p>

<p>&nbsp;</p>
<p><strong class="example">Example 1:</strong></p>

<pre>
<strong>Input:</strong> keyName = [&quot;daniel&quot;,&quot;daniel&quot;,&quot;daniel&quot;,&quot;luis&quot;,&quot;luis&quot;,&quot;luis&quot;,&quot;luis&quot;], keyTime = [&quot;10:00&quot;,&quot;10:40&quot;,&quot;11:00&quot;,&quot;09:00&quot;,&quot;11:00&quot;,&quot;13:00&quot;,&quot;15:00&quot;]
<strong>Output:</strong> [&quot;daniel&quot;]
<strong>Explanation:</strong> &quot;daniel&quot; used the keycard 3 times in a one-hour period (&quot;10:00&quot;,&quot;10:40&quot;, &quot;11:00&quot;).
</pre>

<p><strong class="example">Example 2:</strong></p>

<pre>
<strong>Input:</strong> keyName = [&quot;alice&quot;,&quot;alice&quot;,&quot;alice&quot;,&quot;bob&quot;,&quot;bob&quot;,&quot;bob&quot;,&quot;bob&quot;], keyTime = [&quot;12:01&quot;,&quot;12:00&quot;,&quot;18:00&quot;,&quot;21:00&quot;,&quot;21:20&quot;,&quot;21:30&quot;,&quot;23:00&quot;]
<strong>Output:</strong> [&quot;bob&quot;]
<strong>Explanation:</strong> &quot;bob&quot; used the keycard 3 times in a one-hour period (&quot;21:00&quot;,&quot;21:20&quot;, &quot;21:30&quot;).
</pre>

<p>&nbsp;</p>
<p><strong>Constraints:</strong></p>

<ul>
	<li><code>1 &lt;= keyName.length, keyTime.length &lt;= 10<sup>5</sup></code></li>
	<li><code>keyName.length == keyTime.length</code></li>
	<li><code>keyTime[i]</code> is in the format <strong>&quot;HH:MM&quot;</strong>.</li>
	<li><code>[keyName[i], keyTime[i]]</code> is <strong>unique</strong>.</li>
	<li><code>1 &lt;= keyName[i].length &lt;= 10</code></li>
	<li><code>keyName[i] contains only lowercase English letters.</code></li>
</ul>

<!-- description:end -->

## Solutions

<!-- solution:start -->

### Solution 1: Hash Table + Sorting

First, we use a hash table $d$ to record all the clock-in times of each employee.

Then we traverse the hash table. For each employee, we first check whether the number of clock-in times is greater than or equal to 3. If not, we skip this employee. Otherwise, we sort all the clock-in times of this employee in chronological order, and then traverse the sorted clock-in times to check whether the two times at a distance of 2 indices are within the same hour. If so, we add this employee to the answer array.

Finally, we sort the answer array in lexicographical order to get the answer.

The time complexity is $O(n \times \log n)$, and the space complexity is $O(n)$. Where $n$ is the number of clock-in records.

<!-- tabs:start -->

#### Java

```java
class Solution {
    public List<String> alertNames(String[] keyName, String[] keyTime) {
        Map<String, List<Integer>> d = new HashMap<>();
        for (int i = 0; i < keyName.length; ++i) {
            String name = keyName[i];
            String time = keyTime[i];
            int t
                = Integer.parseInt(time.substring(0, 2)) * 60 + Integer.parseInt(time.substring(3));
            d.computeIfAbsent(name, k -> new ArrayList<>()).add(t);
        }
        List<String> ans = new ArrayList<>();
        for (var e : d.entrySet()) {
            var ts = e.getValue();
            int n = ts.size();
            if (n > 2) {
                Collections.sort(ts);
                for (int i = 0; i < n - 2; ++i) {
                    if (ts.get(i + 2) - ts.get(i) <= 60) {
                        ans.add(e.getKey());
                        break;
                    }
                }
            }
        }
        Collections.sort(ans);
        return ans;
    }
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
