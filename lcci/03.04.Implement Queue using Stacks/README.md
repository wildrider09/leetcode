---
comments: true
difficulty: Easy
edit_url: https://github.com/doocs/leetcode/edit/main/lcci/03.04.Implement%20Queue%20using%20Stacks/README_EN.md
---

<!-- problem:start -->

# [03.04. Implement Queue using Stacks](https://leetcode.cn/problems/implement-queue-using-stacks-lcci)

[Chinese Version](/lcci/03.04.Implement%20Queue%20using%20Stacks/README.md)

## Description

<!-- description:start -->

<p>Implement a MyQueue class which implements a queue using two stacks.</p>

&nbsp;

<p><strong>Example: </strong></p>

<pre>

MyQueue queue = new MyQueue();

queue.push(1);

queue.push(2);

queue.peek();  // return 1

queue.pop();   // return 1

queue.empty(); // return false</pre>

<p>&nbsp;</p>

<p><b>Notes:</b></p>

<ul>
	<li>You must use&nbsp;<i>only</i>&nbsp;standard operations of a stack -- which means only&nbsp;<code>push to top</code>,&nbsp;<code>peek/pop from top</code>,&nbsp;<code>size</code>, and&nbsp;<code>is empty</code>&nbsp;operations are valid.</li>
	<li>Depending on your language, stack may not be supported natively. You may simulate a stack by using a list or deque (double-ended queue), as long as you use only standard operations of a stack.</li>
	<li>You may assume that all operations are valid (for example, no pop or peek operations will be called on an empty queue).</li>
</ul>

<p>&nbsp;</p>

<!-- description:end -->

## Solutions

<!-- solution:start -->

### Solution 1: Double Stack

We use two stacks, where `stk1` is used for enqueue, and another stack `stk2` is used for dequeue.

When enqueueing, we directly push the element into `stk1`. The time complexity is $O(1)$.

When dequeueing, we first check whether `stk2` is empty. If it is empty, we pop all elements from `stk1` and push them into `stk2`, and then pop an element from `stk2`. If `stk2` is not empty, we directly pop an element from `stk2`. The amortized time complexity is $O(1)$.

When getting the front element, we first check whether `stk2` is empty. If it is empty, we pop all elements from `stk1` and push them into `stk2`, and then get the top element from `stk2`. If `stk2` is not empty, we directly get the top element from `stk2`. The amortized time complexity is $O(1)$.

When checking whether the queue is empty, we only need to check whether both stacks are empty. The time complexity is $O(1)$.

<!-- tabs:start -->

#### Java

```java
class MyQueue {
    private Deque<Integer> stk1 = new ArrayDeque<>();
    private Deque<Integer> stk2 = new ArrayDeque<>();

    public MyQueue() {
    }

    public void push(int x) {
        stk1.push(x);
    }

    public int pop() {
        move();
        return stk2.pop();
    }

    public int peek() {
        move();
        return stk2.peek();
    }

    public boolean empty() {
        return stk1.isEmpty() && stk2.isEmpty();
    }

    private void move() {
        while (stk2.isEmpty()) {
            while (!stk1.isEmpty()) {
                stk2.push(stk1.pop());
            }
        }
    }
}

/**
 * Your MyQueue object will be instantiated and called as such:
 * MyQueue obj = new MyQueue();
 * obj.push(x);
 * int param_2 = obj.pop();
 * int param_3 = obj.peek();
 * boolean param_4 = obj.empty();
 */
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
