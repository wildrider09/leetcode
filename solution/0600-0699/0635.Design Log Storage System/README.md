---
comments: true
difficulty: Medium
edit_url: https://github.com/doocs/leetcode/edit/main/solution/0600-0699/0635.Design%20Log%20Storage%20System/README_EN.md
tags:
    - Design
    - Hash Table
    - String
    - Ordered Set
---

<!-- problem:start -->

# [635. Design Log Storage System 🔒](https://leetcode.com/problems/design-log-storage-system)

[Chinese Version](/solution/0600-0699/0635.Design%20Log%20Storage%20System/README.md)

## Description

<!-- description:start -->

<p>You are given several logs, where each log contains a unique ID and timestamp. Timestamp is a string that has the following format: <code>Year:Month:Day:Hour:Minute:Second</code>, for example, <code>2017:01:01:23:59:59</code>. All domains are zero-padded decimal numbers.</p>

<p>Implement the <code>LogSystem</code> class:</p>

<ul>
	<li><code>LogSystem()</code> Initializes the <code>LogSystem</code><b> </b>object.</li>
	<li><code>void put(int id, string timestamp)</code> Stores the given log <code>(id, timestamp)</code> in your storage system.</li>
	<li><code>int[] retrieve(string start, string end, string granularity)</code> Returns the IDs of the logs whose timestamps are within the range from <code>start</code> to <code>end</code> inclusive. <code>start</code> and <code>end</code> all have the same format as <code>timestamp</code>, and <code>granularity</code> means how precise the range should be (i.e. to the exact <code>Day</code>, <code>Minute</code>, etc.). For example, <code>start = &quot;2017:01:01:23:59:59&quot;</code>, <code>end = &quot;2017:01:02:23:59:59&quot;</code>, and <code>granularity = &quot;Day&quot;</code> means that we need to find the logs within the inclusive range from <strong>Jan. 1st 2017</strong> to <strong>Jan. 2nd 2017</strong>, and the <code>Hour</code>, <code>Minute</code>, and <code>Second</code> for each log entry can be ignored.</li>
</ul>

<p>&nbsp;</p>
<p><strong class="example">Example 1:</strong></p>

<pre>
<strong>Input</strong>
[&quot;LogSystem&quot;, &quot;put&quot;, &quot;put&quot;, &quot;put&quot;, &quot;retrieve&quot;, &quot;retrieve&quot;]
[[], [1, &quot;2017:01:01:23:59:59&quot;], [2, &quot;2017:01:01:22:59:59&quot;], [3, &quot;2016:01:01:00:00:00&quot;], [&quot;2016:01:01:01:01:01&quot;, &quot;2017:01:01:23:00:00&quot;, &quot;Year&quot;], [&quot;2016:01:01:01:01:01&quot;, &quot;2017:01:01:23:00:00&quot;, &quot;Hour&quot;]]
<strong>Output</strong>
[null, null, null, null, [3, 2, 1], [2, 1]]

<strong>Explanation</strong>
LogSystem logSystem = new LogSystem();
logSystem.put(1, &quot;2017:01:01:23:59:59&quot;);
logSystem.put(2, &quot;2017:01:01:22:59:59&quot;);
logSystem.put(3, &quot;2016:01:01:00:00:00&quot;);

// return [3,2,1], because you need to return all logs between 2016 and 2017.
logSystem.retrieve(&quot;2016:01:01:01:01:01&quot;, &quot;2017:01:01:23:00:00&quot;, &quot;Year&quot;);

// return [2,1], because you need to return all logs between Jan. 1, 2016 01:XX:XX and Jan. 1, 2017 23:XX:XX.
// Log 3 is not returned because Jan. 1, 2016 00:00:00 comes before the start of the range.
logSystem.retrieve(&quot;2016:01:01:01:01:01&quot;, &quot;2017:01:01:23:00:00&quot;, &quot;Hour&quot;);
</pre>

<p>&nbsp;</p>
<p><strong>Constraints:</strong></p>

<ul>
	<li><code>1 &lt;= id &lt;= 500</code></li>
	<li><code>2000 &lt;= Year &lt;= 2017</code></li>
	<li><code>1 &lt;= Month &lt;= 12</code></li>
	<li><code>1 &lt;= Day &lt;= 31</code></li>
	<li><code>0 &lt;= Hour &lt;= 23</code></li>
	<li><code>0 &lt;= Minute, Second &lt;= 59</code></li>
	<li><code>granularity</code> is one of the values <code>[&quot;Year&quot;, &quot;Month&quot;, &quot;Day&quot;, &quot;Hour&quot;, &quot;Minute&quot;, &quot;Second&quot;]</code>.</li>
	<li>At most <code>500</code> calls will be made to <code>put</code> and <code>retrieve</code>.</li>
</ul>

<!-- description:end -->

## Solutions

<!-- solution:start -->

### Solution 1: String Comparison

Store the `id` and `timestamp` of the logs as tuples in an array. Then in the `retrieve()` method, truncate the corresponding parts of `start` and `end` based on `granularity`, and traverse the array, adding the `id` that meets the conditions to the result array.

In terms of time complexity, the time complexity of the `put()` method is $O(1)$, and the time complexity of the `retrieve()` method is $O(n)$, where $n$ is the length of the array.

<!-- tabs:start -->

#### Java

```java
class LogSystem {
    private List<Log> logs = new ArrayList<>();
    private Map<String, Integer> d = new HashMap<>();

    public LogSystem() {
        d.put("Year", 4);
        d.put("Month", 7);
        d.put("Day", 10);
        d.put("Hour", 13);
        d.put("Minute", 16);
        d.put("Second", 19);
    }

    public void put(int id, String timestamp) {
        logs.add(new Log(id, timestamp));
    }

    public List<Integer> retrieve(String start, String end, String granularity) {
        List<Integer> ans = new ArrayList<>();
        int i = d.get(granularity);
        String s = start.substring(0, i);
        String e = end.substring(0, i);
        for (var log : logs) {
            String t = log.ts.substring(0, i);
            if (s.compareTo(t) <= 0 && t.compareTo(e) <= 0) {
                ans.add(log.id);
            }
        }
        return ans;
    }
}

class Log {
    int id;
    String ts;

    Log(int id, String ts) {
        this.id = id;
        this.ts = ts;
    }
}

/**
 * Your LogSystem object will be instantiated and called as such:
 * LogSystem obj = new LogSystem();
 * obj.put(id,timestamp);
 * List<Integer> param_2 = obj.retrieve(start,end,granularity);
 */
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
