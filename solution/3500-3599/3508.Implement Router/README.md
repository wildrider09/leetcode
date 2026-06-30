---
comments: true
difficulty: Medium
edit_url: https://github.com/doocs/leetcode/edit/main/solution/3500-3599/3508.Implement%20Router/README_EN.md
rating: 1851
source: Weekly Contest 444 Q2
tags:
    - Design
    - Queue
    - Array
    - Hash Table
    - Binary Search
    - Ordered Set
---

<!-- problem:start -->

# [3508. Implement Router](https://leetcode.com/problems/implement-router)

[Chinese Version](/solution/3500-3599/3508.Implement%20Router/README.md)

## Description

<!-- description:start -->

<p>Design a data structure that can efficiently manage data packets in a network router. Each data packet consists of the following attributes:</p>

<ul>
	<li><code>source</code>: A unique identifier for the machine that generated the packet.</li>
	<li><code>destination</code>: A unique identifier for the target machine.</li>
	<li><code>timestamp</code>: The time at which the packet arrived at the router.</li>
</ul>

<p>Implement the <code>Router</code> class:</p>

<p><code>Router(int memoryLimit)</code>: Initializes the Router object with a fixed memory limit.</p>

<ul>
	<li><code>memoryLimit</code> is the <strong>maximum</strong> number of packets the router can store at any given time.</li>
	<li>If adding a new packet would exceed this limit, the <strong>oldest</strong> packet must be removed to free up space.</li>
</ul>

<p><code>bool addPacket(int source, int destination, int timestamp)</code>: Adds a packet with the given attributes to the router.</p>

<ul>
	<li>A packet is considered a duplicate if another packet with the same <code>source</code>, <code>destination</code>, and <code>timestamp</code> already exists in the router.</li>
	<li>Return <code>true</code> if the packet is successfully added (i.e., it is not a duplicate); otherwise return <code>false</code>.</li>
</ul>

<p><code>int[] forwardPacket()</code>: Forwards the next packet in FIFO (First In First Out) order.</p>

<ul>
	<li>Remove the packet from storage.</li>
	<li>Return the packet as an array <code>[source, destination, timestamp]</code>.</li>
	<li>If there are no packets to forward, return an empty array.</li>
</ul>

<p><code>int getCount(int destination, int startTime, int endTime)</code>:</p>

<ul>
	<li>Returns the number of packets currently stored in the router (i.e., not yet forwarded) that have the specified destination and have timestamps in the inclusive range <code>[startTime, endTime]</code>.</li>
</ul>

<p><strong>Note</strong> that queries for <code>addPacket</code> will be made in non-decreasing order of <code>timestamp</code>.</p>

<p>&nbsp;</p>
<p><strong class="example">Example 1:</strong></p>

<div class="example-block">
<p><strong>Input:</strong><br />
<span class="example-io">[&quot;Router&quot;, &quot;addPacket&quot;, &quot;addPacket&quot;, &quot;addPacket&quot;, &quot;addPacket&quot;, &quot;addPacket&quot;, &quot;forwardPacket&quot;, &quot;addPacket&quot;, &quot;getCount&quot;]<br />
[[3], [1, 4, 90], [2, 5, 90], [1, 4, 90], [3, 5, 95], [4, 5, 105], [], [5, 2, 110], [5, 100, 110]]</span></p>

<p><strong>Output:</strong><br />
<span class="example-io">[null, true, true, false, true, true, [2, 5, 90], true, 1] </span></p>

<p><strong>Explanation</strong></p>
Router router = new Router(3); // Initialize Router with memoryLimit of 3.<br />
router.addPacket(1, 4, 90); // Packet is added. Return True.<br />
router.addPacket(2, 5, 90); // Packet is added. Return True.<br />
router.addPacket(1, 4, 90); // This is a duplicate packet. Return False.<br />
router.addPacket(3, 5, 95); // Packet is added. Return True<br />
router.addPacket(4, 5, 105); // Packet is added, <code>[1, 4, 90]</code> is removed as number of packets exceeds memoryLimit. Return True.<br />
router.forwardPacket(); // Return <code>[2, 5, 90]</code> and remove it from router.<br />
router.addPacket(5, 2, 110); // Packet is added. Return True.<br />
router.getCount(5, 100, 110); // The only packet with destination 5 and timestamp in the inclusive range <code>[100, 110]</code> is <code>[4, 5, 105]</code>. Return 1.</div>

<p><strong class="example">Example 2:</strong></p>

<div class="example-block">
<p><strong>Input:</strong><br />
<span class="example-io">[&quot;Router&quot;, &quot;addPacket&quot;, &quot;forwardPacket&quot;, &quot;forwardPacket&quot;]<br />
[[2], [7, 4, 90], [], []]</span></p>

<p><strong>Output:</strong><br />
<span class="example-io">[null, true, [7, 4, 90], []] </span></p>

<p><strong>Explanation</strong></p>
Router router = new Router(2); // Initialize <code>Router</code> with <code>memoryLimit</code> of 2.<br />
router.addPacket(7, 4, 90); // Return True.<br />
router.forwardPacket(); // Return <code>[7, 4, 90]</code>.<br />
router.forwardPacket(); // There are no packets left, return <code>[]</code>.</div>

<p>&nbsp;</p>
<p><strong>Constraints:</strong></p>

<ul>
	<li><code>2 &lt;= memoryLimit &lt;= 10<sup>5</sup></code></li>
	<li><code>1 &lt;= source, destination &lt;= 2 * 10<sup>5</sup></code></li>
	<li><code>1 &lt;= timestamp &lt;= 10<sup>9</sup></code></li>
	<li><code>1 &lt;= startTime &lt;= endTime &lt;= 10<sup>9</sup></code></li>
	<li>At most <code>10<sup>5</sup></code> calls will be made to <code>addPacket</code>, <code>forwardPacket</code>, and <code>getCount</code> methods altogether.</li>
	<li>queries for <code>addPacket</code> will be made in non-decreasing order of <code>timestamp</code>.</li>
</ul>

<!-- description:end -->

## Solutions

<!-- solution:start -->

### Solution 1: Hash Map + Queue + Binary Search

We use a hash map $\textit{vis}$ to store the hash values of packets that have already been added, a queue $\textit{q}$ to store the packets currently in the router, a hash map $\textit{idx}$ to record the number of packets already forwarded for each destination, and a hash map $\textit{d}$ to store the list of timestamps for each destination.

For the $\textit{addPacket}$ method, we compute the hash value of the packet. If it already exists in $\textit{vis}$, we return $\text{false}$; otherwise, we add it to $\textit{vis}$, check if the current queue size exceeds the memory limit, and if so, call the $\textit{forwardPacket}$ method to remove the oldest packet. Then, we add the new packet to the queue and append its timestamp to the corresponding destination's timestamp list, finally returning $\text{true}$. The time complexity is $O(1)$.

For the $\textit{forwardPacket}$ method, if the queue is empty, we return an empty array; otherwise, we remove the packet at the head of the queue, delete its hash value from $\textit{vis}$, update the number of forwarded packets for the corresponding destination, and return the packet. The time complexity is $O(1)$.

For the $\textit{getCount}$ method, we get the timestamp list and the number of forwarded packets for the given destination, then use binary search to find the number of timestamps within the specified range, and return that count. The time complexity is $O(\log n)$, where $n$ is the length of the timestamp list.

The space complexity is $O(n)$.

<!-- tabs:start -->

#### Java

```java
class Router {
    private int lim;
    private Set<Long> vis = new HashSet<>();
    private Deque<int[]> q = new ArrayDeque<>();
    private Map<Integer, Integer> idx = new HashMap<>();
    private Map<Integer, List<Integer>> d = new HashMap<>();

    public Router(int memoryLimit) {
        this.lim = memoryLimit;
    }

    public boolean addPacket(int source, int destination, int timestamp) {
        long x = f(source, destination, timestamp);
        if (vis.contains(x)) {
            return false;
        }
        vis.add(x);
        if (q.size() >= lim) {
            forwardPacket();
        }
        q.offer(new int[] {source, destination, timestamp});
        d.computeIfAbsent(destination, k -> new ArrayList<>()).add(timestamp);
        return true;
    }

    public int[] forwardPacket() {
        if (q.isEmpty()) {
            return new int[] {};
        }
        int[] packet = q.poll();
        int s = packet[0], d_ = packet[1], t = packet[2];
        vis.remove(f(s, d_, t));
        idx.merge(d_, 1, Integer::sum);
        return new int[] {s, d_, t};
    }

    private long f(int a, int b, int c) {
        return ((long) a << 46) | ((long) b << 29) | (long) c;
    }

    public int getCount(int destination, int startTime, int endTime) {
        List<Integer> ls = d.getOrDefault(destination, List.of());
        int k = idx.getOrDefault(destination, 0);
        int i = lowerBound(ls, startTime, k);
        int j = lowerBound(ls, endTime + 1, k);
        return j - i;
    }

    private int lowerBound(List<Integer> list, int target, int fromIndex) {
        int l = fromIndex, r = list.size();
        while (l < r) {
            int m = (l + r) >>> 1;
            if (list.get(m) < target) {
                l = m + 1;
            } else {
                r = m;
            }
        }
        return l;
    }
}

/**
 * Your Router object will be instantiated and called as such:
 * Router obj = new Router(memoryLimit);
 * boolean param_1 = obj.addPacket(source,destination,timestamp);
 * int[] param_2 = obj.forwardPacket();
 * int param_3 = obj.getCount(destination,startTime,endTime);
 */
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
