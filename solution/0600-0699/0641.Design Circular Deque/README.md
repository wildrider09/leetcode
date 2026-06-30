---
comments: true
difficulty: Medium
edit_url: https://github.com/doocs/leetcode/edit/main/solution/0600-0699/0641.Design%20Circular%20Deque/README_EN.md
tags:
    - Design
    - Queue
    - Array
    - Linked List
---

<!-- problem:start -->

# [641. Design Circular Deque](https://leetcode.com/problems/design-circular-deque)

[Chinese Version](/solution/0600-0699/0641.Design%20Circular%20Deque/README.md)

## Description

<!-- description:start -->

<p>Design your implementation of the circular double-ended queue (deque).</p>

<p>Implement the <code>MyCircularDeque</code> class:</p>

<ul>
	<li><code>MyCircularDeque(int k)</code> Initializes the deque with a maximum size of <code>k</code>.</li>
	<li><code>boolean insertFront()</code> Adds an item at the front of Deque. Returns <code>true</code> if the operation is successful, or <code>false</code> otherwise.</li>
	<li><code>boolean insertLast()</code> Adds an item at the rear of Deque. Returns <code>true</code> if the operation is successful, or <code>false</code> otherwise.</li>
	<li><code>boolean deleteFront()</code> Deletes an item from the front of Deque. Returns <code>true</code> if the operation is successful, or <code>false</code> otherwise.</li>
	<li><code>boolean deleteLast()</code> Deletes an item from the rear of Deque. Returns <code>true</code> if the operation is successful, or <code>false</code> otherwise.</li>
	<li><code>int getFront()</code> Returns the front item from the Deque. Returns <code>-1</code> if the deque is empty.</li>
	<li><code>int getRear()</code> Returns the last item from Deque. Returns <code>-1</code> if the deque is empty.</li>
	<li><code>boolean isEmpty()</code> Returns <code>true</code> if the deque is empty, or <code>false</code> otherwise.</li>
	<li><code>boolean isFull()</code> Returns <code>true</code> if the deque is full, or <code>false</code> otherwise.</li>
</ul>

<p>&nbsp;</p>
<p><strong class="example">Example 1:</strong></p>

<pre>
<strong>Input</strong>
[&quot;MyCircularDeque&quot;, &quot;insertLast&quot;, &quot;insertLast&quot;, &quot;insertFront&quot;, &quot;insertFront&quot;, &quot;getRear&quot;, &quot;isFull&quot;, &quot;deleteLast&quot;, &quot;insertFront&quot;, &quot;getFront&quot;]
[[3], [1], [2], [3], [4], [], [], [], [4], []]
<strong>Output</strong>
[null, true, true, true, false, 2, true, true, true, 4]

<strong>Explanation</strong>
MyCircularDeque myCircularDeque = new MyCircularDeque(3);
myCircularDeque.insertLast(1);  // return True
myCircularDeque.insertLast(2);  // return True
myCircularDeque.insertFront(3); // return True
myCircularDeque.insertFront(4); // return False, the queue is full.
myCircularDeque.getRear();      // return 2
myCircularDeque.isFull();       // return True
myCircularDeque.deleteLast();   // return True
myCircularDeque.insertFront(4); // return True
myCircularDeque.getFront();     // return 4
</pre>

<p>&nbsp;</p>
<p><strong>Constraints:</strong></p>

<ul>
	<li><code>1 &lt;= k &lt;= 1000</code></li>
	<li><code>0 &lt;= value &lt;= 1000</code></li>
	<li>At most <code>2000</code> calls will be made to <code>insertFront</code>, <code>insertLast</code>, <code>deleteFront</code>, <code>deleteLast</code>, <code>getFront</code>, <code>getRear</code>, <code>isEmpty</code>, <code>isFull</code>.</li>
</ul>

<!-- description:end -->

## Solutions

<!-- solution:start -->

### Solution 1: Array

We can use an array to implement the circular deque. We maintain a pointer $\textit{front}$ pointing to the front of the queue, a variable $\textit{size}$ representing the number of elements in the queue, and a variable $\textit{capacity}$ representing the queue's capacity. We use an array $\textit{q}$ to store the elements.

When $\textit{insertFront}$ is called, we first check if the queue is full; if so, return $\text{false}$. Otherwise, we move $\textit{front}$ one position forward (using modular arithmetic for circular wrapping), insert the new element at $\textit{front}$, and increment $\textit{size}$ by 1.

When $\textit{insertLast}$ is called, we first check if the queue is full; if so, return $\text{false}$. Otherwise, we compute the insertion position (using $\textit{front}$ and $\textit{size}$), insert the new element there, and increment $\textit{size}$ by 1.

When $\textit{deleteFront}$ is called, we first check if the queue is empty; if so, return $\text{false}$. Otherwise, we move $\textit{front}$ one position backward (using modular arithmetic for circular wrapping) and decrement $\textit{size}$ by 1.

When $\textit{deleteLast}$ is called, we first check if the queue is empty; if so, return $\text{false}$. Otherwise, we decrement $\textit{size}$ by 1.

When $\textit{getFront}$ is called, we first check if the queue is empty; if so, return $-1$. Otherwise, we return $\textit{q}[\textit{front}]$.

When $\textit{getRear}$ is called, we first check if the queue is empty; if so, return $-1$. Otherwise, we compute the position of the rear element (using $\textit{front}$ and $\textit{size}$) and return the element at that position.

When $\textit{isEmpty}$ is called, we check whether $\textit{size}$ equals $0$.

When $\textit{isFull}$ is called, we check whether $\textit{size}$ equals $\textit{capacity}$.

All operations above have a time complexity of $O(1)$ and a space complexity of $O(k)$, where $k$ is the capacity of the deque.

<!-- tabs:start -->

#### Java

```java
class MyCircularDeque {
    private int[] q;
    private int front;
    private int size;
    private int capacity;

    /** Initialize your data structure here. Set the size of the deque to be k. */
    public MyCircularDeque(int k) {
        q = new int[k];
        capacity = k;
    }

    /** Adds an item at the front of Deque. Return true if the operation is successful. */
    public boolean insertFront(int value) {
        if (isFull()) {
            return false;
        }
        if (!isEmpty()) {
            front = (front - 1 + capacity) % capacity;
        }
        q[front] = value;
        ++size;
        return true;
    }

    /** Adds an item at the rear of Deque. Return true if the operation is successful. */
    public boolean insertLast(int value) {
        if (isFull()) {
            return false;
        }
        int idx = (front + size) % capacity;
        q[idx] = value;
        ++size;
        return true;
    }

    /** Deletes an item from the front of Deque. Return true if the operation is successful. */
    public boolean deleteFront() {
        if (isEmpty()) {
            return false;
        }
        front = (front + 1) % capacity;
        --size;
        return true;
    }

    /** Deletes an item from the rear of Deque. Return true if the operation is successful. */
    public boolean deleteLast() {
        if (isEmpty()) {
            return false;
        }
        --size;
        return true;
    }

    /** Get the front item from the deque. */
    public int getFront() {
        if (isEmpty()) {
            return -1;
        }
        return q[front];
    }

    /** Get the last item from the deque. */
    public int getRear() {
        if (isEmpty()) {
            return -1;
        }
        int idx = (front + size - 1) % capacity;
        return q[idx];
    }

    /** Checks whether the circular deque is empty or not. */
    public boolean isEmpty() {
        return size == 0;
    }

    /** Checks whether the circular deque is full or not. */
    public boolean isFull() {
        return size == capacity;
    }
}

/**
 * Your MyCircularDeque object will be instantiated and called as such:
 * MyCircularDeque obj = new MyCircularDeque(k);
 * boolean param_1 = obj.insertFront(value);
 * boolean param_2 = obj.insertLast(value);
 * boolean param_3 = obj.deleteFront();
 * boolean param_4 = obj.deleteLast();
 * int param_5 = obj.getFront();
 * int param_6 = obj.getRear();
 * boolean param_7 = obj.isEmpty();
 * boolean param_8 = obj.isFull();
 */
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
